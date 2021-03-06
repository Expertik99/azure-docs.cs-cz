---
title: Azure SQL Database Managed Instance konfigurace virtuální sítě | Dokumentace Microsoftu
description: Toto téma popisuje možnosti konfigurace pro virtuální síť (VNet) s Azure SQL Database Managed Instance.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 748489785241c0eab6022e3585164974f330d6f9
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44049669"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Konfigurace virtuální sítě pro spravovanou instanci Azure SQL Database

Azure SQL Database Managed Instance (preview) musí být nasazen v rámci Azure [virtuální síť (VNet)](../virtual-network/virtual-networks-overview.md). Toto nasazení umožňuje následující scénáře: 
- Připojení k Managed Instance přímo z místní sítě 
- Připojení Managed Instance pro odkazovaný server nebo další místní úložiště dat 
- Připojování k prostředkům Azure Managed Instance  

## <a name="plan"></a>Plánování

Naplánujte, jak nasadit spravované Instance ve virtuální síti pomocí odpovědi na následující otázky: 
- Máte v úmyslu nasadit jeden nebo více spravovaných instancí? 

  Počet spravovaných instancí Určuje minimální velikost podsítě k přidělení pro Managed instance. Další informace najdete v tématu [určit velikost podsítě pro Managed Instance](#determine-the-size-of-subnet-for-managed-instances). 
- Je potřeba k nasazení Managed Instance do existující virtuální síť nebo vytvoříte novou síť? 

   Pokud plánujete použití existující virtuální sítě, budete muset upravit tuto konfiguraci sítě tak, aby vyhovovaly Managed Instance. Další informace najdete v tématu [upravit existující virtuální sítě pro Managed Instance](#modify-an-existing-virtual-network-for-managed-instances). 

   Pokud budete chtít vytvořit novou virtuální síť, přečtěte si téma [vytvořit novou virtuální síť pro Managed Instance](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Požadavky

Pro vytvoření Managed Instance je třeba vyhradit podsítě uvnitř virtuální sítě, který splňuje následující požadavky:
- **Vyhrazené podsíti**: podsítě nesmí obsahovat jiné cloudové služby přidružené k němu a nesmí se jednat o podsíť brány. Nebudete mít k vytvoření Managed Instance v podsíti, která obsahuje prostředky než spravovaná instance nebo přidat další prostředky v podsíti později.
- **Žádnou skupinu zabezpečení sítě**: podsítě nesmí obsahovat skupinu zabezpečení sítě spojenou s ním. 
- **Máte konkrétní směrovací tabulka**: podsíť musí mít uživatelem směrovací tabulky (UDR) 0.0.0.0/0 dalšího segmentu směrování Internetu jako pouze trasy přiřazenou. Další informace najdete v tématu [vytvoříte požadovaný směrovací tabulku a přidružíte ho](#create-the-required-route-table-and-associate-it)
3. **Volitelné vlastní DNS**: Pokud je vlastní DNS zadaná ve virtuální síti, Azure rekurzivní překladače IP adresu (například adresy 168.63.129.16) musí být přidaný do seznamu. Další informace najdete v tématu [konfigurace vlastního DNS](sql-database-managed-instance-custom-dns.md).
4. **Žádné koncové body služby**: podsítě nesmí být přiřazen koncový bod služby. Ujistěte se, že při vytváření virtuální sítě je zakázána možnost koncových bodů služby.
5. **Dostatek IP adres**: podsíť musí mít minimální 16 IP adres (Doporučená minimální je 32 IP adresy). Další informace najdete v tématu [určit velikost podsítě pro Managed instance](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Nebude moct nasadit nové Managed Instance, pokud není kompatibilní se všemi výše uvedených požadavků cílové podsíti. Cílový virtuální síť a podsíť musí uchovávat v souladu s požadavky na tyto spravované Instance (před a po nasazení), jako jakékoliv porušení může způsobit, že instance do chybného stavu a přestanou být dostupné. Obnovení ze stavu vyžaduje k vytvoření nové instance ve virtuální síti s předpisy sítě, vytvořte znovu dat na úrovni instance a obnovit své databáze. To přináší významné výpadky vašich aplikací.

Se zavedením _zásady sítě záměr_, v podsíti Managed Instance můžete přidat skupinu zabezpečení sítě (NSG), po vytvoření Managed Instance.

Teď můžete skupinu NSG můžete zúžit rozsahy IP adres, ze kterých aplikací a uživatelů může dotazování a správu dat pomocí filtrování síťového provozu na portu 1433. 

> [!IMPORTANT]
> Při konfiguraci pravidla NSG, které se omezí přístup k portu 1433, budete potřebovat k vložení nejvyšší prioritou příchozích pravidel zobrazí v následující tabulce. Jinak zásady sítě záměr blokuje změny jako kompatibilní.

| NÁZEV       |PORT                        |PROTOKOL|ZDROJ           |CÍL|AKCE|
|------------|----------------------------|--------|-----------------|-----------|------|
|Správa  |9000, 9003, 1438, 1440, 1452|Všechny     |Všechny              |Všechny        |Povolit |
|mi_subnet   |Všechny                         |Všechny     |MI PODSÍTĚ        |Všechny        |Povolit |
|health_probe|Všechny                         |Všechny     |AzureLoadBalancer|Všechny        |Povolit |

Směrování prostředí jsme vylepšili také tak, aby kromě 0.0.0.0/0 typ dalšího segmentu směrování Internetu, můžete teď přidat UDR tak, aby směrovat provoz směrem k vaší místní soukromé rozsahy IP adres prostřednictvím brány virtuální sítě nebo virtuální síťové zařízení (NVA).

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Určit velikost podsítě pro Managed instance

Když vytvoříte Managed Instance, Platforma Azure přiřadí počet virtuálních počítačů v závislosti na úroveň, na kterou jste zvolili během zřizování. Protože tyto virtuální počítače jsou spojeny s vaší podsítě, vyžadují IP adresy. K zajištění vysoké dostupnosti během standardních operací a údržby služby, může Azure přidělte další virtuální počítače. V důsledku toho počet požadované IP adresy v podsíti je větší než počet spravovaných instancí v této podsíti. 

Návrh Managed Instance vyžaduje minimálně 16 IP adresy v podsíti a může použít až 256 IP adresy. V důsledku toho můžete použít masky podsítě velikosti/28 do /24 při definování rozsahy IP adres vaší podsítě. 

> [!IMPORTANT]
> Velikost podsítě s IP adresami 16 je minimum, s omezenou potenciál pro další Managed Instance horizontální navýšení kapacity. Když zvolíte podsítě s předponou velikost/27 nebo pod důrazně doporučujeme. 

Pokud budete chtít nasadit víc instancí v podsíti spravované a třeba optimalizovat na velikost podsítě, použijte k výpočtu tyto parametry: 

- Azure používá 5 IP adres v podsíti pro vlastní potřeby 
- Každá instance pro obecné účely potřebuje dvě adresy 
- Každá instance pro důležité obchodní informace potřebuje čtyři adresy

**Příklad**: bude mít tři pro obecné účely a dvě obchodní kritické Managed instance. Že znamená, že potřebujete 5 + 3 * 2 + 2 * 4 = 19 IP adresy. Když rozsahy IP adres jsou definovány v Power BI 2, budete potřebovat IP rozsahu 32 (2 ^ 5) IP adresy. Proto musíte rezervovat podsíť s maskou podsítě/27. 

> [!IMPORTANT]
> Výpočet zobrazený nad bude stát zastaralým další vylepšení. 

## <a name="create-a-new-virtual-network-for-managed-instance-using-azure-resource-manager-deployment"></a>Vytvoření nové virtuální sítě pro Managed Instance pomocí modelu nasazení Azure Resource Manageru

Nejjednodušší způsob, jak vytvořit a konfigurovat virtuální sítě je použití šablony nasazení Azure Resource Manageru.

1. Přihlaste se k portálu Azure.

2. Použití **nasadit do Azure** tlačítko k nasazení virtuální sítě v cloudu Azure:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Toto tlačítko otevře formulář, který můžete použít ke konfiguraci síťového prostředí kde můžete nasadit Managed Instance.

  > [!Note]
  > Tuto šablonu Azure Resource Manageru nasadí virtuální síť se dvěma podsítěmi. Jedna podsíť s názvem **ManagedInstances** je vyhrazen pro Managed instance a má předem nakonfigurované směrovací tabulku, zatímco jiné podsíti nazývají **výchozí** slouží k jiným prostředkům, které by měl přístup ke spravované Instance (například virtuální počítače Azure). Můžete odebrat **výchozí** podsítě, pokud už nepotřebujete.

3. Konfigurace prostředí sítě. V následujícím formátu můžete nakonfigurovat parametry vaše síťové prostředí:

![Konfigurace sítě azure](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

Můžete změnit názvy virtuální sítě a podsítě a upravit rozsahy IP adres přidružené k síťové prostředky. Po stisknutí tlačítka "Koupit", bude tento formulář vytvoření a konfiguraci prostředí. Pokud už nebudete potřebovat dvě podsítě můžete odstranit výchozí hodnotu. 

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Upravit existující virtuální sítě pro Managed instance 

Otázky a odpovědi v této části ukazují, jak přidat do existující virtuální síť Managed Instance. 

**Používáte pro existující virtuální síť modelu nasazení Classic nebo Resource Manager?** 

Managed Instance můžete vytvořit pouze ve virtuálním sítím Resource Manageru. 

**Chcete vytvořit novou podsíť pro spravované SQL instance nebo použijte existující?**

Pokud chcete vytvořit nový certifikát: 

- Vypočítá velikost podsítě podle pokynů v [určit velikost podsítě pro Managed instance](#determine-the-size-of-subnet-for-managed-instances) oddílu.
- Postupujte podle kroků v [přidání, změna nebo odstranění podsítě virtuální sítě](../virtual-network/virtual-network-manage-subnet.md). 
- Vytvoření směrovací tabulky, která obsahuje jednu položku **0.0.0.0/0**, jako další směrování Internetu a přidružte ji k podsíti pro Managed Instance.  

Pokud chcete vytvořit uvnitř existující podsíť Managed Instance, doporučujeme následující skript prostředí PowerShell pro přípravu na podsíť.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Příprava podsítě se provádí ve třech jednoduchých krocích:

- Ověření – vybrané virtuální netwok a podsítě se ověřují pro Managed Instance, požadavky na síť
- Potvrďte – uživatel je zobrazen sady změn, které je potřeba provedené při přípravě podsíť Managed Instance nasazení a zobrazí výzva ho o souhlas
- Příprava – správně nakonfigurována virtuální síť a podsíť

**Máte vlastní server DNS nakonfigurovaný?** 

Pokud ano, přečtěte si téma [konfigurace vlastního DNS](sql-database-managed-instance-custom-dns.md). 

- Vytvořit požadované směrovací tabulku a přidružíte ho: viz [vytvoříte požadovaný směrovací tabulku a přidružíte ho](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Další postup

- Přehled najdete v tématu [co je Managed Instance](sql-database-managed-instance.md)
- Kurz ukazuje, jak vytvořit virtuální síť, vytvoříte Managed Instance a obnovit databázi ze zálohy databáze, najdete v tématu [vytvoření Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Problémy s DNS, najdete v článku [konfigurace vlastního DNS](sql-database-managed-instance-custom-dns.md)
