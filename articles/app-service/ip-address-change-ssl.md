---
title: Postup přípravy na změnu adresy SSL IP – Azure
description: Pokud je vaše adresa SSL IP změnit, zjistěte, co můžete udělat tak, aby vaše aplikace i nadále fungovat po provedení změny.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.openlocfilehash: e8558b4c3c7dafca8d4fff7e2aae0597a66c031d
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576538"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>Postup přípravy na změnu adresy SSL IP

Pokud jste dostali oznámení, že se mění SSL IP adresu vaší aplikace Azure App Service, postupujte podle pokynů v tomto článku na verze existující adresu SSL IP a přiřadit nový.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>Uvolnit adresy SSL IP a přiřaďte nové značky

1.  Otevřete web [Azure Portal](https://portal.azure.com).

2.  V levé navigační nabídce vyberte **App Services**.

3.  Vyberte ze seznamu vaši aplikaci služby App Service.

4.  V části **nastavení** záhlaví, klikněte na tlačítko **nastavení SSL** v levém navigačním panelu.

5. V části vazby SSL vyberte záznam název hostitele. V editoru, které se otevře, zvolte **SNI SSL** na **typ SSL** rozevírací nabídky a klikněte na tlačítko **přidat vazbu**. Když se zobrazí zpráva o úspěchu operací, stávající IP adresa byla uvolněna.

6.  V **vazby SSL** znovu vyberte záznam stejného názvu hostitele s certifikátem. V editoru, které se otevře, zvolte tuto chvíli **IP SSL na základě** na **typ SSL** rozevírací nabídky a klikněte na tlačítko **přidat vazbu**. Když se zobrazí zpráva o úspěchu operací, které jste získali novou IP adresu.

7.  Pokud je záznam (záznam DNS odkazuje přímo na IP adresu) nakonfigurované v portálu pro registraci vaší domény (třetích stran poskytovatele DNS nebo Azure DNS), nahraďte existující IP adresu nově vygenerovaný. Podle pokynů uvedených v následující části najdete novou IP adresu.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Na webu Azure Portal najdete novou adresu SSL IP

1.  Počkejte pár minut a pak otevřete [webu Azure portal](https://portal.azure.com).

2.  V levé navigační nabídce vyberte **App Services**.

3.  Vyberte ze seznamu vaši aplikaci služby App Service.

4.  V části **nastavení** záhlaví, klikněte na tlačítko **vlastnosti** v levém navigačním panelu a najít v části s názvem **virtuální IP adresa**.

5. Zkopírujte IP adresu a překonfigurujte záznamu domény nebo IP mechanismus.

## <a name="next-steps"></a>Další postup

Tento článek vysvětlil, jak připravit pro změnu IP adresy, který byl inicializován nástrojem Azure. Další informace o IP adresách v Azure App Service najdete v tématu [SSL a SSL IP adres ve službě Azure App Service](app-service-ip-addresses.md).
