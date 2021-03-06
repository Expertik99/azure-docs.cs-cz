---
title: Vyhledejte prostředky Azure ke správě v PIM | Dokumentace Microsoftu
description: Zjistěte, jak zjistit prostředky Azure pro správu v Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/30/2018
ms.author: rolyon
ms.openlocfilehash: d9a6ab49d619e487eee6fb13abe128cfc167b560
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306685"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Vyhledejte prostředky Azure ke správě v PIM

Pomocí Azure AD Privileged Identity Management (PIM), můžete zlepšit ochranu vašich prostředků Azure. To je užitečné pro organizace, které už používají k ochraně role adresáře Azure AD PIM a vlastníkům správu skupin a předplatné, kteří chtějí zabezpečit prostředky v produkčním prostředí.

Když poprvé nastavujete PIM pro prostředky Azure, budete muset zjistit a vyberte prostředky, které chcete chránit pomocí PIM. Neexistuje žádné omezení počtu prostředků, které můžete spravovat pomocí služby PIM. Však doporučujeme začít s prostředky těch nejdůležitějších (produkce).

## <a name="discover-resources"></a>Zjistit prostředky

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

1. Otevřít **Azure AD Privileged Identity Management**.

1. Klikněte na tlačítko **prostředky Azure**.

    Pokud je toto vaše první přihlášení pomocí PIM pro prostředky Azure, uvidíte podokno zjišťování prostředků.

    ![Vyhledejte prostředky – první](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Pokud jiný prostředek nebo adresáře správce ve vaší organizaci je už správu prostředků Azure v PIM, zobrazí se vám seznam prostředků, které jsou aktuálně spravován.

    ![Zjistit prostředky podokno](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Klikněte na tlačítko **zjistit prostředky** ke spuštění prostředí pro zjišťování.

    ![Podokno zjišťování](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. V podokně zjišťování pomocí **filtr stavu prostředků** a **vyberte typ prostředku** filtrovat správu skupiny nebo předplatného máte oprávnění k zápisu do. To je pravděpodobně nejjednodušším začít **všechny** původně.

    Je možné pouze vyhledejte a vyberte správu skupiny nebo předplatného prostředky spravovat pomocí PIM. Když spravujete skupinu pro správu nebo předplatného v PIM, můžete také spravovat její podřízené prostředky.

1. Přidáte značku zaškrtnutí vedle všech nespravovaných prostředků, které chcete spravovat.

    > [!NOTE]
    > Po nastavení skupiny pro správu nebo předplatného je spravovat, nemůže nespravované. Odebírá se nastavení PIM tomu jiný správce prostředků.

    ![Zjišťování – Správa prostředků](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Klikněte na tlačítko **prostředky spravovat** k zahájení správy vybrané prostředky.

## <a name="next-steps"></a>Další postup

- [Konfigurace nastavení role prostředků Azure v PIM](pim-resource-roles-configure-role-settings.md)
- [Přiřazení role prostředků Azure v PIM](pim-resource-roles-assign-roles.md)
