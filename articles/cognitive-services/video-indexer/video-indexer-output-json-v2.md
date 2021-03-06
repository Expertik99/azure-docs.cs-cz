---
title: Prozkoumejte Azure Video Indexer výstup vytvořený pomocí rozhraní API v2 | Dokumentace Microsoftu
description: Toto téma popisuje funkce Video Indexer výstup vytvořený pomocí rozhraní API v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 43cc02417fad8a2fa46bd309235951393cd55b8a
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41987652"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Prozkoumání výstupu funkce Video Indexer vytvořené metodou rozhraní API v2

> [!Note]
> 1. srpna 2018 se přestala nabízet Video Indexer V1 API. Byste teď měli použít rozhraní Video Indexer API v2. <br/>Vývoj pomocí rozhraní API pro Video Indexer v2, najdete pokyny, nalezeno [tady](https://api-portal.videoindexer.ai/). 

Při volání **získat Index Video** rozhraní API a stav odpovědi je v pořádku, získejte podrobný výstup JSON jako obsah odpovědi. Obsah JSON obsahuje podrobné informace o zadané nové poznatky z videí. Informace zahrnují dimenzí, jako jsou: záznamy o studiu, ocrs, tváří, témat, bloky, atd. Dimenze mít instancí časových rozsahů, které ukazují jednotlivých rozměrů zobrazené ve videu.  

Souhrnný přehled videa můžete také vizuálně zkoumat stisknutím kombinace kláves **Přehrát** tlačítko na video na portálu pro Video Indexer. Další informace najdete v tématu [prohlížení a úpravy nové poznatky z videí](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Tento článek zkoumá vrácený obsah JSON **získat Index Video** rozhraní API. 

> [!NOTE]
> Vypršení platnosti všech přístupových tokenů ve Video indexeru je jedna hodina.


## <a name="root-elements"></a>Kořenové prvky

|Název|Popis|
|---|---|
|ID účtu|ID účtu VI seznamu stop|
|id|ID seznamu stop.|
|jméno|Název seznamu stop.|
|description|Popis seznamu stop.|
|uživatelské jméno|Jméno uživatele, který vytvořil seznam stop.|
|vytvořené|Čas vytvoření seznamu stop.|
|privacyMode|Režim ochrany osobních údajů seznamu stop (Private/Public).|
|state|Seznamu testů (nahrané, zpracování, zpracování, selhalo, umístěné do karantény).|
|isOwned|Označuje, zda byl vytvořen seznam stop aktuálním uživatelem.|
|iseditable –|Určuje, zda je aktuální uživatel oprávnění k úpravě seznamu stop.|
|isBase|Označuje, zda seznam testů je základní stop (video) nebo seznam skladeb tvořeny další videa (derived).|
|durationInSeconds|Celková doba trvání seznamu stop.|
|summarizedInsights|Obsahuje nejméně jednu [summarizedInsights](#summarizedinsights).
|videa|Seznam [videa](#videos) vytváření seznamu stop.<br/>Pokud tento seznam testů z vytvořen z časových rozsahů jiných videí (derived) na videa v tomto seznamu bude obsahovat pouze data ze součástí časových rozsahů.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

Tato část uvádí přehled informací.

|Atribut | Popis|
|---|---|
|jméno|Název videa. Například Azure Monitor.|
|shortId|ID videa. Například 63c6d532ff.|
|privacyMode|Vaše rozpis může mít jednu z těchto režimů: **privátní**, **veřejné**. **Veřejné** – video je viditelné všem uživatelům v účtu a každý uživatel, který obsahuje odkaz na video. **Privátní** – video je viditelné všem uživatelům ve vašem účtu.|
|doba trvání|Obsahuje jeden dobu, po kterou popisuje čas, kdy došlo k chybě přehledů. Doba trvání je během několika sekund.|
|thumbnailVideoId|ID videa, ze kterého bylo převedeno na miniaturu.
|thumbnailId|ID miniatury videa. Chcete-li získat skutečný miniatur volání Get-Miniatura (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) a předejte jí thumbnailVideoId a thumbnailId.|
|tváří|Může obsahovat nula nebo více ploch. Další informace najdete v tématu [tváří](#faces).|
|klíčová slova|Může obsahovat nula nebo více klíčových slov. Další informace najdete v tématu [klíčová slova](#keywords).|
|zabarvení|Může obsahovat nula nebo více mínění. Další informace najdete v tématu [zabarvení](#sentiments).|
|audioEffects| Může obsahovat nula nebo více audioEffects. Další informace najdete v tématu [audioEffects](#audioeffects).|
|popisky| Může obsahovat nula nebo více štítků. Další informace najdete v tématu [popisky](#labels).|
|značky| Může obsahovat nula nebo více značek. Další informace najdete v tématu [značky](#brands).|
|statistiky | Další informace najdete v tématu [statistiky](#statistics).|

## <a name="videos"></a>videa

|Název|Popis|
|---|---|
|ID účtu|ID účtu VI videa|
|id|ID videa.|
|jméno|Název videa.
|state|Stav videa (nahrané, zpracování, zpracování, selhalo, umístěné do karantény).|
|processingProgress|Průběh zpracování během zpracování (například 20 %).|
|Kód chyby|Kód chyby, pokud se proces (například "UnsupportedFileType").|
|failureMessage|Zpráva selhání, pokud se nepodařilo zpracovat.|
|externalId|Videa externí ID (Pokud je zadaný uživatelem).|
|adresy externalUrl|Videa externí adresa url (Pokud je zadaný uživatelem).|
|zprostředkovatele identity|Externí metadat videa (Pokud je zadaný uživatelem).|
|isAdult|Určuje, jestli se video ručně zkontrolovat a identifikována jako dospělého videa.|
|přehledy|Objekt, který insights. Další informace najdete v tématu [insights](#insights).|
|thumbnailId|ID miniatury videa. Chcete-li získat skutečný miniatur volání Get-Miniatura (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) a předejte jí videa ID a thumbnailId.|
|publishedUrl|Adresa url pro streamování videa.|
|publishedUrlProxy|Adresa url pro streamování videa z (pro zařízení Apple).|
|viewToken|Krátkodobý zobrazení token pro streamování videa.|
|sourceLanguage|Zdrojový jazyk videa.|
|Jazyk|Aktuální jazyk videa (překlad).|
|indexingPreset|Přednastavení, použít k indexování videa.|
|streamingPreset|Přednastavení k publikování videa.|
|linguisticModelId|Model CRIS používaný k přepisy videa.|
|statistiky | Další informace najdete v tématu [statistiky](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>přehledy

Přehledy jsou sadu dimenzí (například přepisu řádky, tváří, značky, atd.), kde každé dimenze je seznam jedinečných prvků (například face1 ze předu 2, ze předu 3), a každý prvek má svůj vlastní metadata a seznam její instance (které jsou s časových rozsahů Další volitelná metadata).

Řez může mít ID, název, miniaturu, další metadata a seznam její dočasná instance (Příklad: 00:00:05 – 00:00:10: 00:01:00 - 00:02:30 a 00:41:21 – 00:41:49.) Každá dočasná instance může mít další metadata. Například pro rozpoznávání tváře obdélník koordinuje (20,230,60,60).

|Verze|Kód verze|
|---|---|
|sourceLanguage|Zdrojový jazyk videa (za předpokladu, že jeden hlavní jazyk). Ve formuláři [BCP-47](https://tools.ietf.org/html/bcp47) řetězec.|
|Jazyk|Jazyk insights (přeloženého ze zdrojového jazyka). Ve formuláři [BCP-47](https://tools.ietf.org/html/bcp47) řetězec.|
|přepis|[Přepisu](#transcript) dimenze.|
|optické rozpoznávání znaků|[Ocr](#ocr) dimenze.|
|klíčová slova|[Klíčová slova](#keywords) dimenze.|
|bloky|Může obsahovat jednu nebo více [bloky](#blocks)|
|tváří|[Tváří](#faces) dimenze.|
|popisky|[Popisky](#labels) dimenze.|
|snímky|[Snímky](#shots) dimenze.|
|značky|[Značky](#brands) dimenze.|
|audioEffects|[AudioEffects](#audioEffects) dimenze.|
|zabarvení|[Zabarvení](#sentiments) dimenze.|
|visualContentModeration|[VisualContentModeration](#visualcontentmoderation) dimenze.|
|textualConentModeration|[TextualConentModeration](#textualconentmoderation) dimenze.|

Příklad:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="blocks"></a>bloky

Atribut | Popis
---|---
id|ID bloku.|
instance|Seznam časových rozsahů tento blok.|

#### <a name="transcript"></a>přepis

|Název|Popis|
|---|---|
|id|ID řádku.|
|text|Přepis samotný.|
|Jazyk|Jazyk přepisu. Určené pro podporu přepisu, kde každý řádek může mít jiný jazyk.|
|instance|Seznam časových rozsahů, ve kterém se tento řádek. Pokud je instance přepisu, bude mít jenom 1 instance.|

Příklad:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>optické rozpoznávání znaků

|Název|Popis|
|---|---|
|id|ID OCR řádku.|
|text|OCR textu.|
|spolehlivosti|Rozpoznávání spolehlivosti.|
|Jazyk|OCR jazyk.|
|instance|Seznam časových rozsahů, ve kterém se objevil tento OCR (stejné OCR může objevit více než jednou).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>klíčová slova

|Název|Popis|
|---|---|
|id|ID – klíčové slovo.|
|text|Text – klíčové slovo.|
|spolehlivosti|Klíčové slovo rozpoznávání spolehlivosti.|
|Jazyk|Jazyk – klíčové slovo (při překladu).|
|instance|Seznam časových rozsahů, ve kterém se nacházela toto klíčové slovo (klíčové slovo může objevit více než jednou).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>tváří

|Název|Popis|
|---|---|
|id|ID tváře.|
|jméno|Název vzhledu. Může být "Neznámý #0", identifikovaný celebrit nebo trénovaného osoba zákazníka.|
|spolehlivosti|Identifikace spolehlivosti pro rozpoznávání tváře.|
|description|Popis celebrity. |
|thumbnalId|ID miniatury této pro rozpoznávání tváře.|
|knownPersonId|Pokud se jedná o známé osoba, jeho interní ID.|
|referenceId|Pokud je celebrit Bing, jeho ID Bingu.|
|Hodnota referenceType|Aktuálně jenom Bingu.|
|název|Pokud se jedná celebrit, jeho název (například "CEO společnosti Microsoft").|
|imageUrl|Pokud se jedná celebrit jeho adresa url obrázku.|
|instance|Toto jsou instance z kde zobrazovaly plochu v daném časovém rozsahu. Každá instance má také thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>popisky

|Název|Popis|
|---|---|
|id|ID popisku.|
|jméno|Název popisku (například "Computer", "TV").|
|Jazyk|Popisek názvu jazyka (při překladu). BCP-47|
|instance|Seznam časových rozsahů, ve kterém se tento popisek (popisek se může objevit více než jednou). Každá instance má pole jistotou. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>snímky

|Název|Popis|
|---|---|
|id|Snímek ID.|
|klíčové snímky|Seznam klíčových snímků v rámci snímku (každý má ID a seznam instancí časových rozsahů). Instance klíčové snímky mají thumbnailId pole s Miniatura na klíčový snímek ID.|
|instance|Seznam časových rozsahů tento snímek (snímky mají jenom 1 instance).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>značky

Firmy a produktů názvy v převodu řeči na text přepisu a/nebo Video optické rozpoznávání znaků. To nezahrnuje visual rozpoznávání značky nebo detekce logo.

|Název|Popis|
|---|---|
|id|ID značky.|
|jméno|Název značky.|
|referenceId | Přípona adresy url wikipedie značky. Například "Target_Corporation" je přípona [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Značka uživatele adresu url Wikipedie, pokud existuje. Příklad: [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|Popis značky.|
|tags|Seznam předdefinovaných značky, které byly přidruženy k této značky.|
|spolehlivosti|Hodnota spolehlivosti detektoru Video Indexer značky (0-1).|
|instance|Seznam časových rozsahů tuto značku. Každá instance má brandType, který označuje, zda tato značka se objevil přepisu nebo optické rozpoznávání znaků.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistiky

|Název|Popis|
|---|---|
|CorrespondenceCount|Počet srovnávací ve videu.|
|WordCount|Počet slov za mluvčího.|
|SpeakerNumberOfFragments|Množství fragmenty mluvčího se ve videu.|
|SpeakerLongestMonolog|Nejdelší monolog tohoto mluvčího. Pokud má mluvčího silences uvnitř protokolu monolog je zahrnuté. Na začátku a konci protokolu monolog nečinnosti se odstraní.| 
|SpeakerTalkToListenRatio|Výpočet vychází čas strávený na mluvčího protokolu monolog (bez nečinnosti mezi) rozdělené podle celkové doby trvání videa. Čas se zaokrouhlí na třetí desetinné čárky.|

#### <a name="audioeffects"></a>audioEffects

|Název|Popis|
|---|---|
|id|ID zvukové efekty.|
|type|Typ zvukové efekty (například nečinnosti Clapping, řeči,).|
|instance|Seznam časových rozsahů, ve kterém se nacházela této zvukové efekty.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>zabarvení

Zabarvení se agregují podle jejich sentimentType pole (neutrální/kladné nebo záporné). Například 0 0.1, 0.2 0,1.

|Název|Popis|
|---|---|
|id|ID mínění.|
|Průměr |Průměr všech skóre všech instancí tohoto typu mínění – pozitivní nebo neutrální nebo negativní|
|instance|Seznam časových rozsahů, ve kterém se objevil tento mínění.|
|sentimentType |Typ může být "Pozitivní", 'Neutrální' nebo "Záporné".|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

Blok visualContentModeration obsahuje časových rozsahů, které můžou mít obsah pro dospělé Video Indexer. Pokud visualContentModeration je prázdný, není žádný obsah pro dospělé, který byl identifikován.

Videa, které se nacházejí na obsah pro dospělé nebo pikantního mohou být k dispozici pouze privátní zobrazení. Uživatelé mají možnost odeslat žádost pro lidskou kontrolu obsahu, ve kterém bude obsahovat případ atribut IsAdult výsledek kontrolu člověkem.

|Název|Popis|
|---|---|
|id|ID visual moderování obsahu.|
|adultScore|Hodnocení obsahu pro dospělé (z content moderatoru).|
|racyScore|Hodnocení obsahu pro dospělé (z moderování obsahu).|
|instance|Seznam časových rozsahů, ve kterém se objevil tento visual moderování obsahu.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|Název|Popis|
|---|---|
|id|ID textové moderování obsahu.|
|bannedWordsCount |Počet zakázaných slov v příspěvcích.|
|bannedWordsRatio |Poměr z celkového počtu slov.|


## <a name="next-steps"></a>Další postup

[Video Indexer API](https://api-portal.videoindexer.ai)

Informace o tom, jak vložení widgetů do aplikace najdete v tématu [Video Indexer vložení widgetů do svých aplikací](video-indexer-embed-widgets.md). 

