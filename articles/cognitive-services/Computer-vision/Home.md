---
title: Počítačové zpracování obrazu pro služby Azure Cognitive Services | Microsoft Docs
description: Využijte pokročilé algoritmy v Počítačovém zpracování obrazu, které vám pomůžou zpracovávat obrázky a vracet informace ve službách Azure Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: v-deken
ms.openlocfilehash: ec3ffa8599192ecd9a4092b026a8d249f50c64c9
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248618"
---
# <a name="what-is-computer-vision"></a>Co je počítačové zpracování obrazu?

Cloudová služba Počítačové zpracování obrazu poskytuje vývojářům přístup k pokročilým algoritmům pro zpracování obrázků a vrácení informací. Počítačové zpracování obrazu pracuje s oblíbenými formáty obrázků, jako je JPEG a PNG. Když chcete analyzovat obrázek, můžete ho nahrát nebo zadat jeho adresu URL. Algoritmy Počítačového zpracování obrazu dokáží analyzovat obsah obrázků různými způsoby v závislosti na vizuálních vlastnostech, které vás zajímají. Počítačové zpracování obrazu například dokáže určit, jestli obrázek obsahuje nevhodný obsah nebo obsah pro dospělé, nebo vyhledat všechny tváře na obrázku.

Pokud chcete využít Počítačové zpracování obrazu ve své aplikaci, můžete službu vyvolat pomocí našich [klientských knihoven](quickstarts-sdk/csharp-analyze-sdk.md) nebo volat přímo rozhraní [REST API](vision-api-how-to-topics/howtocallvisionapi.md). Pak můžete provádět následující úlohy:

- [Analýza obrázků za účelem získání přehledu](#analyzing-images-for-insight)
- [Extrakce textu z obrázků](#extracting-text-from-images)
- [Moderování obsahu obrázků](#moderating-content-in-images)

## <a name="analyzing-images-for-insight"></a>Analýza obrázků za účelem získání přehledu

Prostřednictvím analýzy obrázků pomocí Počítačového zpracování obrazu můžete detekovat vizuální vlastnosti a charakteristiky obrázků a poskytovat jejich přehled. Pokud chcete analyzovat místní obrázky, můžete nahrát obsah obrázku. Nebo můžete zadat adresu URL obrázku, pokud chcete analyzovat vzdálené obrázky.

Počítačové zpracování obrazu při analýze obrázku může provádět následující akce:

| Akce | Popis |
| ------ | ----------- |
|**[Označování vizuálních vlastností](quickstarts/csharp-analyze.md)**|Identifikuje a označuje vizuální vlastnosti obrázku na základě více než 2 000 rozpoznatelných objektů, živých bytostí, krajin a akcí. V případě, kdy jsou značky nejednoznačné nebo nejsou všeobecně známé, poskytne odpověď „nápovědu“ pro objasnění významu značky v kontextu známého prostředí. Označování se neomezuje na hlavní předmět, například postavu v popředí, ale zahrnuje také prostředí (interiér nebo exteriér), nábytek, nástroje, rostliny, zvířata, příslušenství, pomůcky atd.|
|**[Kategorizace obrázku](quickstarts/csharp-analyze.md)**|Identifikuje a kategorizuje celý obrázek s využitím [taxonomie kategorií](Category-Taxonomy.md) s dědičnými hierarchiemi nadřízený/podřízený objekt. Kategorie je možné používat samostatně nebo s našimi novými modely označování.<br/>V současné době je jediným podporovaným jazykem pro označování a kategorizaci obrázků angličtina.|
|**[Popis obrázku](quickstarts/csharp-analyze.md)**|Vygeneruje popis celého obrázku v celých větách v čitelném jazyce. Algoritmy Počítačového zpracování obrazu generují různé popisy v závislosti na objektech identifikovaných na obrázku. Jednotlivé popisy se vyhodnotí a vygeneruje se pro ně skóre spolehlivosti. Pak se vrátí seznam seřazený od nejvyššího skóre spolehlivosti po nejnižší.<br/>Příklad robota, který tuto technologii využívá ke generování popisků, najdete na [GitHubu](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).|
|**[Rozpoznávání tváří](quickstarts/csharp-analyze.md)** |Rozpoznává tváře na obrázku a poskytuje informace o jednotlivých rozpoznaných tvářích. Počítačové zpracování obrazu pro každou rozpoznanou tvář vrátí souřadnice, obdélník, pohlaví a věk.<br/>Počítačové zpracování obrazu poskytuje podmnožinu funkcí, které najdete ve službě [Rozpoznávání tváře](/azure/cognitive-services/face/), kterou můžete využít k podrobnější analýze, například identifikaci tváří a rozpoznávání pózy.|
|**[Rozpoznávání typů obrázků](quickstarts/csharp-analyze.md)**|Rozpoznává charakteristiky obrázku, například jestli jde o perokresbu nebo s jakou pravděpodobností je obrázek klipart.|
|**[Rozpoznávání obsahu specifického doménu](quickstarts/python-domain.md)**|S využitím doménových modelů rozpoznává a identifikuje obsah obrázku specifický pro doménu, například celebrity a památky. Pokud jsou na obrázku například lidé, může Počítačové zpracování obrazu využít doménový model pro celebrity, který je součástí služby, a určit, jestli se lidé rozpoznaní na obrázku shodují se známými celebritami.|
|**[Rozpoznávání barevného schématu](quickstarts/csharp-analyze.md)**|Analyzuje použité barvy na obrázku. Počítačové zpracování obrazu dokáže určit, jestli je obrázek černobílý nebo barevný, a u barevných obrázků identifikovat dominantní a doplňkové barvy.|
|**[Vytvoření miniatury](quickstarts/csharp-thumb.md)**|Analyzuje obsah obrázku a vytvoří pro obrázek odpovídající miniaturu. Počítačové zpracování obrazu nejprve vygeneruje miniaturu ve vysokém rozlišení a pak analýzou objektů na obrázku určí *oblasti zájmu* (ROI). Počítačové zpracování obrazu pak obrázek ořízne, aby odpovídal požadavkům oblasti zájmu. Vytvořená miniatura může mít podle vašich potřeb jiný poměr stran než původní obrázek.|

## <a name="extracting-text-from-images"></a>Extrakce textu z obrázků

Pomocí Počítačového zpracování obrazu můžete z obrázku [extrahovat text s využitím technologie OCR](quickstarts/csharp-print-text.md) do strojově čitelného proudu znaků. V případě potřeby technologie OCR opraví otočení rozpoznaného textu ve stupních okolo vodorovné osy obrázku a poskytne souřadnice jednotlivých slov. Technologie OCR podporuje 25 jazyků a automaticky rozpoznává jazyk extrahovaného textu.

Na obrázku můžete také [rozpoznávat tištěný a rukou psaný text](quickstarts/csharp-hand-text.md). Počítačové zpracování obrazu dokáže rozpoznat a extrahovat tištěný i rukou psaný text z obrázků různých objektů s různými povrchy a pozadími, jako jsou například účtenky, plakáty, vizitky, dopisy a tabule. Rozpoznávání tištěného a rukou psaného textu je ve verzi Preview a jediným podporovaným jazykem je angličtina.  

## <a name="moderating-content-in-images"></a>Moderování obsahu obrázků

Pomocí Počítačového zpracování obrazu můžete na obrázku [detekovat nevhodný obsah a obsah pro dospělé](quickstarts/csharp-analyze.md) díky tomu, že hodnotí, s jakou pravděpodobností daný obrázek obsahuje nevhodný obsah nebo obsah pro dospělé a pro obojí generuje skóre spolehlivosti. Filtr detekce nevhodného obsahu a obsahu pro dospělé je možné podle potřeby nastavit na posuvné stupnici.

## <a name="image-requirements"></a>Požadavky image

Počítačové zpracování obrazu dokáže analyzovat obrázky, které splňují následující požadavky:

- Obrázek musí být ve formátu JPEG, PNG, GIF nebo BMP.
- Velikost souboru obrázku musí být menší než 4 megabajty (MB).
- Rozměry obrázku musí být větší než 50 × 50 pixelů.  
  Pro technologii OCR musí být rozměry obrázku v rozmezí 40 × 40 a 3 200 × 3 200 pixelů a obrázek nesmí být větší než 10 megapixelů.

## <a name="next-steps"></a>Další kroky

Začněte s Počítačovým zpracováním obrazu s využitím některého z našich rychlých startů:

- [Analýza obrázku](/quickstarts-sdk/csharp-analyze-sdk.md)
- [Extrakce rukou psaného textu](/quickstarts-sdk/csharp-hand-text-sdk.md)
- [Vytvoření miniatury](/quickstarts-sdk/csharp-thumb-sdk.md)
