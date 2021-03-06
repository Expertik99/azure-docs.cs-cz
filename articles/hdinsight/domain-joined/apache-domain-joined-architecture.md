---
title: Architektura Azure HDInsight připojené k doméně
description: Naučte se plánovat službu HDInsight připojenou k doméně.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: d5132cf2414045ca1343354215b2a4564f696190
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044766"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Standardní cluster Azure HDInsight je Jednouživatelský cluster. Je vhodný pro většinu společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Každý uživatel můžete vytvořit vyhrazenou clusteru na vyžádání a zničit ho, když ho už nepotřebují. 

Řada podniků přešli k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Tyto větší podniky potřebují více uživateli přístup na každý cluster v Azure HDInsight.

HDInsight spoléhá na zprostředkovatele oblíbených identity – Active Directory – spravované způsobem. Díky integraci služby HDInsight s [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), dostanete clustery pomocí vašich přihlašovacích údajů domény. 

Virtuální počítače (VM) v HDInsight jsou domény připojené k zadané doméně. Proto všechny služby spuštěné v HDInsight (Ambari, Hive, Ranger, server Spark thrift server a další) pro ověřeného uživatele bezproblémově fungují. Správci pak mohou vytvářet zásady silné ověřování s použitím Apache Ranger k poskytování řízení přístupu na základě rolí pro prostředky v clusteru.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrace služby HDInsight s Active Directory

Open source Hadoop závisí na protokolu Kerberos pro ověřování a zabezpečení. Proto jsou uzly clusteru HDInsight připojené k doméně, který je spravovaný službou Azure AD DS. Zabezpečení protokolu Kerberos je nakonfigurován pro součásti platformy Hadoop v clusteru. 

Pro jednotlivé komponenty systému Hadoop instanční objekt služby vytvořit automaticky. Pro každý počítač, který je připojený k doméně se také vytvoří odpovídající počítač instančního objektu. K ukládání těchto služeb a počítačů objekty zabezpečení, je nutné zadat organizační jednotku (OU) v řadiči domény (Azure AD DS), kde jsou tyto objekty umístěny. 

Souhrnně řečeno, musíte nastavit prostředí pomocí:

- Domény služby Active Directory (spravované službou Azure AD DS).
- Zabezpečený LDAP (LDAPS) povolena ve službě Azure AD DS.
- Správné síťové připojení z virtuální sítě HDInsight do Azure AD DS virtuální sítě, pokud se rozhodnete pro ně samostatné virtuální sítě. Virtuální počítač ve virtuální síti HDInsight by měl mít dohled do služby Azure AD DS prostřednictvím partnerského vztahu virtuální sítě. Pokud HDInsight a Azure AD DS jsou nasazené ve stejné virtuální síti, připojení se automaticky k dispozici a není potřeba žádná další akce.
- Organizační jednotka [vytvořené v Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).
- Účet služby, který má oprávnění:
    - Vytvoření instančních objektů v organizační jednotce.
    - Připojení počítače k doméně a vytvářet objekty počítače v organizační jednotce.

Následující snímek obrazovky ukazuje organizační jednotku vytvořené v doméně contoso.com. Profil také ukazuje některé instančních objektů a objektů počítačů.

![Organizační jednotka pro clustery HDInsight připojené k doméně](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>Nastavit různými řadiči domény
HDInsight aktuálně podporuje pouze Azure AD DS jako řadič domény hlavním cluster používá pro komunikaci pomocí protokolu Kerberos. Ale jiné komplexní nastavení služby Active Directory je to možné, tak dlouho, dokud taková konfigurace vede k povolení služby Azure AD DS pro přístup k HDInsight.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) poskytuje spravovanou doménu, která je plně kompatibilní s Windows Server Active Directory. Microsoft se postará o správu, použití dílčích oprav a sledování domény v s vysokou dostupností (HA) nastavení. Cluster můžete nasadit bez starostí o údržbu řadičů domény. 

Uživatelé, skupiny a hesla jsou synchronizovaná z Azure Active Directory (Azure AD). Jednosměrné synchronizaci z vaší instance služby Azure AD do služby Azure AD DS umožňuje uživatelům přihlášení ke clusteru pomocí stejné podnikové přihlašovací údaje. 

Další informace najdete v tématu [clusterů HDInsight připojených k doméně konfigurovat pomocí služby Azure AD DS](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>V místní službě Active Directory nebo Active Directory na virtuálních počítačích IaaS

Pokud máte složitější nastavení služby Active Directory nebo instance místní služby Active Directory pro vaši doménu, můžete tyto identity do služby Azure AD synchronizovat s použitím služby Azure AD Connect. Pak můžete povolit Azure AD DS na tohoto tenanta Active Directory. 

Protože protokol Kerberos závisí na hodnot hash hesel, budete muset [povolení synchronizace hodnot hash hesel ve službě Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Pokud používáte federace s Active Directory Federation Services (AD FS), můžete volitelně nastavit synchronizace hodnot hash hesel jako záložní v případě selhání infrastruktury služby AD FS. Další informace najdete v tématu [povolení synchronizace hodnot hash hesel pomocí synchronizace Azure AD Connect](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). 

Pomocí místní služby Active Directory nebo Active Directory na virtuálních počítačích IaaS samostatně, bez Azure AD a Azure AD DS, není podporovaná konfigurace pro clustery HDInsight připojené k doméně.

## <a name="next-steps"></a>Další postup
* [Konfigurace clusterů HDInsight připojených k doméně](apache-domain-joined-configure-using-azure-adds.md)
* [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md)
* [Správa clusterů HDInsight připojených k doméně](apache-domain-joined-manage.md) 
