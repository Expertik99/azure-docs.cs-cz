---
title: zahrnout soubor
description: zahrnout soubor
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 62bb91a2e51c39caf31719f72d68a6edab9205bc
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38941347"
---
> [!NOTE]
> Při vytváření účtu Batch můžete zvolit mezi dvěma režimy *přidělování fondů*: **Předplatné uživatele** a **Služba Batch**. Pro většinu případů byste měli použít výchozí režim Služba Batch, při kterém se fondy přidělují na pozadí v předplatných spravovaných Azure. V alternativním režimu Předplatné uživatele se virtuální počítače a další prostředky služby Batch vytvářejí přímo ve vašem předplatném při vytvoření fondu. Režim Předplatné uživatele se vyžaduje, pokud chcete vytvořit fondy služby Batch pomocí [Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/). Pokud chcete vytvořit účet Batch v režimu předplatného uživatele, musíte také zaregistrovat předplatné ve službě Azure Batch a k účtu přidružit službu Azure Key Vault.