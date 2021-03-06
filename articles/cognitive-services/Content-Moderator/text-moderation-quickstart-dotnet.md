---
title: Azure Content Moderator – moderování textu pomocí .NET | Dokumentace Microsoftu
description: Tom, jak pomocí Azure Content Moderator SDK pro .NET moderování textu
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 87ed816077d2c742223a0350851cdf2f0c5653f6
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023034"
---
# <a name="moderate-text-using-net"></a>Moderování textu s použitím .NET

Tento článek obsahuje informace a ukázky kódu, které vám pomůžou začít používat [Content Moderator SDK pro .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) na:
- Detekovat vulgární výrazy v textu pomocí filtrování podle termínu
- Použití modelů na základě machine learningu k [klasifikovat text](text-moderation-api.md#classification) do tří kategorií.
- Například USA a UK telefonní čísla, e-mailové adresy a USA poštovní adresy rozpoznat identifikovatelné osobní údaje (PII).
- Normalizovat překlepy text a automatické opravy

Tento článek předpokládá, že jste již obeznámeni s Visual Studio a C#.

## <a name="sign-up-for-content-moderator-services"></a>Zaregistrovat do služby Content Moderator

Než budete moct použít služby Content Moderator přes rozhraní REST API nebo sady SDK, je nutné klíč předplatného.
Odkazovat [rychlý Start](quick-start.md) se dozvíte, jak můžete získat klíč.

## <a name="create-your-visual-studio-project"></a>Vytvoření projektu sady Visual Studio

1. Přidat nový **Konzolová aplikace (.NET Framework)** do svého řešení projekt.

   Ve vzorovém kódu, pojmenujte projekt **TextModeration**.

1. Vyberte tento projekt jako jeden spouštěný projekt pro řešení.

1. Přidejte odkaz na **ModeratorHelper** sestavení, který jste vytvořili v projektu [rychlý start pomocné rutiny klienta Content Moderator](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalace požadovaných balíčků

Nainstalujte následující balíčky NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualizace programu v nástrojích příkazy

Upravit program v nástrojích příkazy.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;


### <a name="initialize-application-specific-settings"></a>Inicializace nastavení specifické pro aplikaci

Přidejte následující statické pole na **Program** třída v souboru Program.cs.

    /// <summary>
    /// The name of the file that contains the text to evaluate.
    /// </summary>
    /// <remarks>You will need to create an input file and update this path
    /// accordingly. Relative paths are ralative the execution directory.</remarks>
    private static string TextFile = "TextFile.txt";

    /// <summary>
    /// The name of the file to contain the output from the evaluation.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private static string OutputFile = "TextModerationOutput.txt";

Generovat ve výstupu v tomto rychlém startu jsme použili následující text:

> [!NOTE]
> Neplatné číslo sociálního pojištění v následující ukázkový text je úmyslné. Slouží k předání ukázkové vstupní a výstupní formát.

    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
    0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Přidejte kód pro načítání a vyhodnocení vstupního textu

Přidejte následující kód, který **hlavní** metody.

    // Load the input text.
    string text = File.ReadAllText(TextFile);
    text = text.Replace(System.Environment.NewLine, " ");

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        // Create a Content Moderator client and evaluate the text.
        using (var client = Clients.NewClient())
        {
            // Screen the input text: check for profanity, classify the text into three categories
                // do autocorrect text, and check for personally identifying 
                // information (PII)
                outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
                var screenResult =
                    client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
                outputWriter.WriteLine(
                        JsonConvert.SerializeObject(screenResult, Formatting.Indented));
        }
        outputWriter.Flush();
        outputWriter.Close();
    }

> [!NOTE]
> Klíč služby Content Moderator má požadavků za druhé omezení četnosti (předávajících stran) a při překročení limitu, vyvolá výjimku s kódem chyby 429, sady SDK.
>
> Při použití klíče na úrovni free, rychlost požadavků je omezený na jeden požadavek za sekundu.

## <a name="run-the-program-and-review-the-output"></a>Spusťte program a prohlédněte si výstup

Ukázkový výstup programu, jak jsou zapsány do souboru protokolu je:

    Autocorrect typos, check for matching terms, PII, and classify.
    {
    "OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
    "NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
    "AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
    "Misrepresentation": null,
    
    "Classification": {
            "Category1": {
            "Score": 1.5113095059859916E-06
            },
            "Category2": {
            "Score": 0.12747249007225037
            },
            "Category3": {
            "Score": 0.98799997568130493
            },
            "ReviewRecommended": true
    },
    "Status": {
        "Code": 3000,
        "Description": "OK",
        "Exception": null
    },
    "PII": {
            "Email": [
                {
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 33
                }
                ],
            "IPA": [
                {
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 73
                }
                ],
            "Phone": [
                {
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 57
                },
                {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                },
                {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                },
                {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                },
                {
            "   CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
                }
                ],
             "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                 "Index": 89
                    }]
        },
    "Language": "eng",
    "Terms": [
        {
            "Index": 22,
            "OriginalIndex": 22,
            "ListId": 0,
            "Term": "crap"
        }
    ],
    "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
    }

## <a name="next-steps"></a>Další postup

Získejte [Content Moderator sady .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) a [řešení sady Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) pro tuto a další rychlé starty Content Moderator pro platformu .NET a začít používat svoji integraci.
