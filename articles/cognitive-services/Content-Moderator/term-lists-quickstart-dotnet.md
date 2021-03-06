---
title: Střední se seznamy vlastní termín v Azure Content Moderator | Dokumentace Microsoftu
description: Střední vlastní pojmem seznamů pomocí Azure Content Moderator SDK pro .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 2ae080518a9ad78552a8ec173e7f4d70085c7a6b
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022636"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>Střední se seznamy vlastní termín v .NET

Výchozí globální seznam termínů v Azure Content Moderator je dostačující pro potřeby z hlediska většina obsahu moderování. Ale můžete potřebovat obrazovky pro podmínky, které jsou specifické pro vaši organizaci. Například můžete chtít konkurence názvy značek pro další kontrolu. 

Můžete použít [Content Moderator SDK pro .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) vytvořit vlastní seznamy termínů pro použití s rozhraním API pro moderování textu.

> [!NOTE]
> Je maximální limit **5 termín uvádí** s každou seznamu **není delší než 10 000 podmínky**.
>

Tento článek obsahuje informace a ukázky kódu, které vám pomůžou začít používat Content Moderator SDK pro .NET po:
- Vytvoření seznamu.
- Přidání podmínky do seznamu.
- Obrazovka podmínky s výrazy v seznamu.
- Odstraňte podmínky ze seznamu.
- Odstranění seznamu.
- Upravte informace o seznamu.
- Aktualizujte index tak, aby se změny provedené v seznamu jsou součástí nové skenování.

Tento článek předpokládá, že jste již obeznámeni s Visual Studio a C#.

## <a name="sign-up-for-content-moderator-services"></a>Zaregistrovat do služby Content Moderator

Než budete moct použít služby Content Moderator přes rozhraní REST API nebo sady SDK, je nutné klíč předplatného.

Na řídicím panelu Content Moderator, můžete najít váš klíč předplatného v **nastavení** > **pověření** > **API**  >  **Zkušební Ocp-Apim-Subscription-Key**. Další informace najdete v tématu [přehled](overview.md).

## <a name="create-your-visual-studio-project"></a>Vytvoření projektu sady Visual Studio

1. Přidat nový **Konzolová aplikace (.NET Framework)** do svého řešení projekt.

1. Pojmenujte projekt **TermLists**. Vyberte tento projekt jako jeden spouštěný projekt pro řešení.

1. Přidejte odkaz na **ModeratorHelper** sestavení, který jste vytvořili v projektu [rychlý start pomocné rutiny klienta Content Moderator](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalace požadovaných balíčků

Nainstalujte následující balíčky NuGet pro projekt TermLists:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualizace programu v nástrojích příkazy

Upravit program v nástrojích příkazy.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Přidáním vlastních vlastností

Přidejte následující soukromé vlastnosti do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Vytvoření seznamu termínů

Vytvořit seznam termínů s **ContentModeratorClient.ListManagementTermLists.Create**. První parametr **vytvořit** je řetězec, který obsahuje typ MIME, které by se měly "application/json". Další informace najdete v tématu [reference k rozhraní API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). Je druhý parametr **tělo** objekt, který obsahuje název a popis seznamu nový termín.

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

> [!NOTE]
> Klíč služby Content Moderator má požadavků za druhé omezení četnosti (předávajících stran) a při překročení limitu, vyvolá výjimku s kódem chyby 429, sady SDK. 
>
> Klíč úroveň free má omezení četnosti jeden RPS.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Aktualizovat název termínu seznam a popis

Aktualizovat informace o seznamu termín s **ContentModeratorClient.ListManagementTermLists.Update**. První parametr **aktualizace** je termín seznamu. Druhý parametr je typ MIME, které by se měly "application/json". Další informace najdete v tématu [reference k rozhraní API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). Třetí parametr je **tělo** objektu, který obsahuje nový název a popis.

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Přidat podmínku do seznamu termín

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Získat všechny podmínky v seznamu termínů

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Přidejte kód k aktualizaci indexu vyhledávání

Po provedení změn na seznam termínů, obnovíte jeho index vyhledávání pro změny, které mají být zahrnuty při příštím použití seznamu termín na obrazovce text. To se podobá jak vyhledávacího webu na ploše (je-li povoleno) nebo webového vyhledávacího webu průběžně aktualizuje jeho index nových souborů nebo stránky.

Aktualizovat seznam termínů vyhledávací index s **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Text obrazovky pomocí seznamu termínů

Obrazovky textu s použitím seznam termínů **ContentModeratorClient.TextModeration.ScreenText**, které mají následující parametry.

- Jazyk podmínky v seznamu termín.
- Typ MIME, což může být "text/html", "text/xml", "text/markdown" nebo "text/plain".
- Text na obrazovku.
- Logická hodnota. Nastavte pole na **true** do textu před jeho blokování automatické opravy.
- Logická hodnota. Nastavte pole na **true** zjistit osobní údaje (PII) v textu.
- ID seznamu termín

Další informace najdete v tématu [reference k rozhraní API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** vrátí **obrazovky** objektu, který má **podmínky** vlastnost, která obsahuje všechny podmínky této Content Moderator v prověřování zjištěna. Všimněte si, že pokud Content Moderator nezjistili žádné podmínky během blokování, **podmínky** vlastnost má hodnotu **null**.

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Odstranit podmínky a seznamy

Odstraňuje se termín nebo seznamu je jednoduché. Sady SDK můžete provádět následující úlohy:

- Odstraňte termín. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Odstraňte všechny podmínky v seznamu bez odstranění seznamu. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Odstraňte seznam a veškerý jeho obsah. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Odstranit podmínku

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Odstranit všechny podmínky v seznamu termínů

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Odstranění seznamu termín

Přidejte následující definici metody do oboru názvů TermLists, třídu programu.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Vložení všechno dohromady

Přidat **hlavní** definici metody do oboru názvů TermLists, třídu programu. A konečně zavřete třídu Program a TermLists oboru názvů.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Spusťte aplikaci chcete zobrazit výstup

Výstup bude na následující řádky, ale data se můžou lišit.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Další postup

Získejte [Content Moderator sady .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) a [řešení sady Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) pro tuto a další rychlé starty Content Moderator pro platformu .NET a začít používat svoji integraci.
