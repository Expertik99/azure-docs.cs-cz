---
title: Nástroj QnA Maker omezení – Azure Cognitive Services | Dokumentace Microsoftu
description: Omezení nástroje QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93471faab9aac94616c770cbee21fb0364f73639
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501856"
---
# <a name="qna-maker-limits"></a>Omezení nástroje QnA Maker
Úplný seznam omezení napříč QnA Maker.

## <a name="knowledge-bases"></a>Znalostních bází

* Maximální počet znalostních bází na základě [omezení vrstvy Azure Search](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Vrstva Azure Search** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD, HIGH DENSITY**|
|---|---|---|---|---|---|----|
|(Max. počet indexů – 1 (vyhrazené pro test) povolený maximální počet publikovaných znalostních bází|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Extrakce omezení
* Maximální počet souborů, které může být extrahována a maximální velikost souboru: viz [ceny QnA maker](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Maximální počet hloubkové odkazy, které je možné procházet ze stránek HTML – nejčastější dotazy pro extrakci maximálně: 20

## <a name="metadata-limits"></a>Omezení metadat
* Maximální počet polí metadat za znalostní báze podle [omezení vrstvy Azure Search](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Vrstva Azure Search** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD, HIGH DENSITY**|
|---|---|---|---|---|---|----|
|Pole maximální metadat na službu QnA Maker (v rámci všech znalostní báze)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Omezení obsahu znalostní báze
Celkové limitů na obsah znalostní báze knowledge base:
* Délka textu odpovědi: částku 250 000 jedné
* Délka textu otázku: 1000
* Délka textu klíč/hodnota metadat: 100
* Podporované znaky pro název metadat: písmena, číslice a _  
* Podporované znaky pro hodnota metadat: všechny s výjimkou: a | 
* Délka názvu souboru: 200
* Podporované formáty souborů: "TSV", ".pdf", ".txt", ".docx", ".xlsx".
* Maximální počet alternativní otázek: 100
* Maximální počet dvojic otázka – odpověď: závisí [vrstva Azure Search](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) zvolené 

## <a name="create-knowledge-base-call-limits"></a>Vytvoření znalostní báze volání omezení:
Tyto představují vytvořit omezení pro každou akci znalostní báze; To znamená, že kliknete na *vytvořit KB* nebo volání rozhraní API CreateKnowledgeBase.
* Maximální počet alternativní otázky na odpovědi: 100
* Maximální počet adres URL: 10
* Maximální počet souborů: 10

## <a name="update-knowledge-base-call-limits"></a>Aktualizovat omezení volání znalostní báze
Představují limity pro každou akci aktualizace; To znamená, že kliknete na *uložit a jejich trénování* nebo volání rozhraní API UpdateKnowledgeBase.
* Délka názvu každého zdroje: 300
* Maximální počet alternativní otázek přidána nebo odstraněna: 100
* Maximální počet polí metadat přidána nebo odstraněna: 10
* Maximální počet adres URL, které lze aktualizovat: 5
