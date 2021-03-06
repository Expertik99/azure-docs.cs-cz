---
title: Uspořádání prostředků se skupinami pro správu Azure | Dokumentace Microsoftu
description: Další informace o skupin pro správu a jejich použití.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2018
ms.author: rithorn
ms.openlocfilehash: b95dba65a7ab89306844e48641aba584e3d6b175
ms.sourcegitcommit: e45b2aa85063d33853560ec4bc867f230c1c18ce
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43371512"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Uspořádání prostředků se skupinami pro správu Azure

Pokud má vaše organizace mnoho předplatných, možná budete potřebovat způsob, jak efektivně spravovat přístup, zásady a dodržování předpisů pro tato předplatná. Skupiny pro správu Azure zajišťují určitou úroveň oboru nad předplatných. Uspořádání předplatných do kontejnerů s názvem "skupiny pro správu" a použít vaše podmínky zásad správného řízení do skupin pro správu. Všechna předplatná v rámci skupiny pro správu automaticky dědí podmínky pro skupinu pro správu. Skupiny pro správu poskytují správy na podnikové úrovni ve velkém měřítku bez ohledu na to, jaký typ předplatného můžete mít.

Můžete například použít zásady ke skupině pro správu, který omezuje oblasti k dispozici pro vytváření virtuálních počítačů (VM). Tyto zásady se použijí pro úhradu skupin pro správu, předplatná a prostředky v rámci této skupiny pro správu tím, že pouze virtuální počítače vytvořené v této oblasti.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarchie skupin pro správu a předplatná

Můžete vytvářet flexibilní strukturu skupin pro správu a předplatných k uspořádání prostředků do hierarchie pro jednotné zásady a správu přístupu.
Následující diagram ukazuje příklad vytvoření hierarchie pro použití skupin pro správu zásad správného řízení.

![strom](media/management-groups/MG_overview.png)

Vytvořením hierarchie jako v tomto příkladu můžete použít zásadu, třeba umístění virtuálního počítače omezená na oblasti USA – západ skupiny "adaptérů infrastruktury skupiny pro správu" Povolit interních předpisů a zásad zabezpečení. Tyto zásady budou dědit do obou EA předplatná v rámci této skupiny pro správu a budou platit pro všechny virtuální počítače v rámci těchto předplatných. Jak tyto zásady se dědí ze skupiny pro správu pro předplatná, tuto zásadu zabezpečení nelze změnit vlastníkem prostředku nebo předplatnému umožňující lepší zásad správného řízení.

Jiný scénář, kde by použití skupin pro správu je poskytnout uživatelský přístup k více předplatným.  Díky přesunu více předplatných v rámci této skupiny pro správu, máte možnost vytvořit jeden RBAC přiřazení skupiny pro správu, který zdědí tento přístup pro všechna předplatná.  Bez nutnosti skript RBAC přiřazení několika předplatným můžete povolit jedno přiřazení skupiny pro správu uživatelé měli přístup ke všemu, co potřebují.

### <a name="important-facts-about-management-groups"></a>Důležité skutečnosti o skupin pro správu

- 10 000 skupin pro správu může být podporovaný v jednom adresáři (tenanta Azure Active Directory).
- Strom skupin správy může podporovat až šest úrovní hloubky.
  - Tento limit neobsahuje úrovni kořenového adresáře nebo na úrovni předplatného.
- Každá skupina pro správu a předplatné podporuje pouze jeden nadřazený prvek.
- Každou skupinu pro správu může mít více podřízených položek.
- Všechna předplatná a skupiny pro správu jsou obsaženy v jedné hierarchii v každém adresáři. Zobrazit [důležitých faktů o skupině pro správu kořenového](#important-facts-about-the-root-management-group) pro výjimky ve verzi Preview.

## <a name="root-management-group-for-each-directory"></a>Skupina serveru root management pro každý adresář

Každý adresář je přiřazena nejvyšší úrovně správy skupinu s názvem "Root" skupiny pro správu. Tato kořenová skupina pro správu je integrovaná do hierarchie tak, aby pod ní spadaly všechny skupiny pro správu a všechna předplatná. Tato skupina serveru Root management umožňuje globální zásady a přiřazení RBAC uplatňovat na úrovni adresáře. [Musí Directory správce sami zvýšit](../role-based-access-control/elevate-access-global-admin.md) zpočátku vlastník této kořenové skupiny. Jakmile správce je vlastníkem skupiny, může přiřadit libovolnou roli RBAC pro jiné adresáře uživatele nebo skupiny pro správu v hierarchii.  

### <a name="important-facts-about-the-root-management-group"></a>Důležité skutečnosti o skupinu Root management

- Název a ID skupiny pro správu kořenového jsou uvedeny ve výchozím nastavení. Zobrazovaný název je aktualizovat v každém okamžiku k zobrazení různých na webu Azure portal.
  - Název bude "Tenanta kořenovou skupinu".
  - ID bude ID Azure Active Directory.
- Skupinu root management se nedá přesunout ani odstranit, na rozdíl od dalších skupin pro správu.  
- Všechna předplatná a skupiny pro správu Složte jednu kořenové skupině pro správu v adresáři.
  - Všechny prostředky v adresáři Složte kořenové skupině pro správu pro globální správu.
  - Nová předplatná se automaticky nastavena na výchozí skupinu root management při vytvoření.
- Všechny zákazníky Azure můžete zobrazit skupinu root management, ale ne všichni zákazníci mají přístup ke správě této kořenové skupiny pro správu.
  - Každý, kdo má přístup k předplatnému můžete zobrazit kontextu se toto předplatné v hierarchii.  
  - Přístup k výchozím nikdo je vzhledem ke kořenové skupině pro správu. Globální správce adresáře jsou pouze uživatelé, které může zvýšit nároky samy o sobě získat přístup.  Jakmile budou mít přístup, správci adresáře můžete přiřadit libovolnou roli RBAC ostatním uživatelům ke správě.  

>[!NOTE]
>Pokud adresáři začali používat službu skupiny pro správu před 6/25/2018, nemusí být nastavit adresáře všech předplatných v hierarchii. Tým správy skupiny zpětně aktualizuje každý adresář, který pomůže začít používat skupiny pro správu ve verzi Public Preview do tohoto data v rámci dne /. srpna 2018. Všechna předplatná v adresářích bude proveden podřízené složky do skupiny pro správu kořenového předchozí.  
>
>Pokud máte nějaké otázky k tomuto zpětnou procesu, obraťte se na: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Počáteční nastavení skupin pro správu

Každý uživatel spustí při použití skupin pro správu, došlo při počátečním nastavení proces, který se stane. Prvním krokem je že vytvoření skupiny pro správu kořenového v adresáři. Po vytvoření této skupiny jsou provedeny všechny existující odběry, které existují v adresáři podřízené skupiny pro správu kořenového.  Důvod pro tento proces se zajišťuje, že existuje pouze jedna hierarchie správy skupiny v adresáři.  Jedné hierarchii, v adresáři umožňuje zákazníkům správu použití globálního přístupu a zásady, které se nedá obejít ostatních zákazníků v adresáři. NIC přiřazené v kořenovém adresáři se uplatní pro všechny skupiny pro správu, předplatná, skupiny prostředků a prostředky v rámci adresáře tak, že jedna hierarchie v adresáři.  

> [!IMPORTANT]
> Jakémukoliv svalování přiřazení přístupu nebo zásad uživatelů na skupinu root management **se vztahuje na všechny prostředky v rámci adresáře**. Z tohoto důvodu by se měl vyhodnotit všichni zákazníci měli po položky definované v tomto oboru.  Přiřazení přístupu a zásad uživatele by měl být "Musí mít" pouze v tomto oboru.  
  
## <a name="management-group-access"></a>Přístup ke správě skupiny

Podpora skupinami pro správu Azure [Azure Role-Based řízení přístupu (RBAC)](../role-based-access-control/overview.md) pro všechny přístupy do prostředků a definice rolí. Tato oprávnění se dědí do podřízené prostředky, které existují v hierarchii. Žádné předdefinované role RBAC je přiřadit ke skupině pro správu, který zdědí hierarchií k prostředkům.  Přispěvatel virtuálních počítačů role RBAC můžete například přiřadit ke skupině pro správu. Tato role nemá žádnou akci na skupině pro správu, ale budou dědit do všech virtuálních počítačů v rámci této skupiny pro správu.  

Následující graf zobrazuje seznam rolí a podporované akce na skupiny pro správu.

| Název Role RBAC             | Vytvořit | Přejmenovat | Přesunout | Odstranění | Přiřazení přístupu | Přiřadit zásady | Čtení  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Vlastník                       | X      | X      | X    | X      | X             | X             | X     |
|Přispěvatel                 | X      | X      | X    | X      |               |               | X     |
|Přispěvatel MG *             | X      | X      | X    | X      |               |               | X     |
|Čtenář                      |        |        |      |        |               |               | X     |
|MG čtečky *                  |        |        |      |        |               |               | X     |
|Přispěvatel zásad prostředků |        |        |      |        |               | X             |       |
|Správce přístupu uživatelů   |        |        |      |        | X             |               |       |

*: MG Přispěvatel a čtenář MG povolit pouze uživatele k provedení těchto akcí v oboru skupiny pro správu.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Definice Role RBAC vlastní a přiřazení

Na skupiny pro správu nejsou v tuto chvíli nepodporuje vlastní role RBAC.  Najdete v článku [fóru pro zpětnou vazbu skupiny správy](https://aka.ms/mgfeedback) chcete podívat na stav této položky.

## <a name="next-steps"></a>Další postup

Další informace o skupinách správy, naleznete v tématu:

- [Vytvoření skupin pro správu k uspořádání prostředků Azure](management-groups-create.md)
- [Jak změnit, odstranit nebo Správa skupin pro správu](management-groups-manage.md)
- [Instalace modulu Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Projděte si specifikace rozhraní API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Nainstalujte rozšíření Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
