---
title: Kurz použití vzoru rolí k vylepšení predikce služby LUIS – Azure | Dokumentace Microsoftu
titleSuffix: Cognitive Services
description: V tomto kurzu používá k vylepšení predikce služby LUIS vzor role pro kontextově související entity.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: 6f3e7c9db7bbdb6bc24d123208355fc7a1d8e7e8
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161930"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Kurz: Vylepšit aplikaci pomocí vzoru role

V tomto kurzu použijte jednoduchou entitu s rolemi v kombinaci s modely ke zvýšení predikcí záměr a entity.  Při použití vzorů, jsou potřeba méně projevy příklad záměr.

> [!div class="checklist"]
* Principy role modelu
* Použití jednoduché entity s rolemi 
* Vytvoření vzor pro projevy pomocí jednoduchých entit s rolemi
* Postup ověření vylepšení predikce modelu

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Než začnete
Pokud nemáte aplikaci lidských zdrojů [vzor](luis-tutorial-pattern.md) kurzu [importovat](luis-how-to-start-new-app.md#import-new-app) JSON do nové aplikace ve [LUIS](luis-reference-regions.md#luis-website) webu. App k importu se nachází v [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json) úložiště GitHub.

Pokud chcete zachovat původní aplikaci pro lidské zdroje, naklonujte verzi na stránce [Settings](luis-how-to-manage-versions.md#clone-a-version) (Nastavení) a pojmenujte ji `roles`. Klonování představuje skvělý způsob, jak si můžete vyzkoušet různé funkce služby LUIS, aniž by to mělo vliv na původní verzi. 

## <a name="the-purpose-of-roles"></a>Účel role
Účelem role je extrahovat kontextově související entity v utterance. V utterance `Move new employee Robert Williams from Sacramento and San Francisco`, Město původu a Město cílové hodnoty se vztahují k sobě navzájem a použijte společný jazyk pro každé umístění. 

Při použití vzorů, musí být rozpoznány všechny entity ve vzoru _před_ vzor odpovídá utterance. 

Když vytvoříte vzor, prvním krokem je vybrat záměr pro vzor. Tak, že vyberete záměr, pokud vzor odpovídá, správné záměr je vždy vrátí s vysokým skóre (obvykle 99 100 %). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Porovnání hierarchické entita, která má jednoduchou entitu s rolemi

V [hierarchické kurzu](luis-quickstart-intent-and-hier-entity.md), **MoveEmployee** záměr zjistila, kdy se má přesunout existující zaměstnance z jednoho sestavení a office do jiného. Projevy příklad měl zdroj a cíl umístění, ale nepoužívá role. Místo toho zdroj a cíl byly podřízené prvky hierarchické entity. 

V tomto kurzu zjistí aplikaci lidských zdrojů projevy informace o přesunu do jiného Noví zaměstnanci z jedno z měst. Tyto dva druhy projevy jsou podobné, ale s různými schopnostmi LUIS vyřešit.

|Kurz|Příklad utterance|Zdroj a cíl umístění|
|--|--|--|
|[Hierarchické (žádné role)](luis-quickstart-intent-and-hier-entity.md)|MV Jill Jones z **a-2349** k **b-1298**|a-2349 b-1298|
|Tento kurz (s rolemi)|Přesunout Billy Patterson z **Yuma** k **Denver**.|Yuma, Denver|

Hierarchické entity ve vzoru nelze použít, protože pouze hierarchické nadřazených objektů se používají ve vzorcích. Aby bylo možné vrátit pojmenovaná umístění, původu a cíl, muse použití vzoru.

### <a name="simple-entity-for-new-employee-name"></a>Jednoduchou entitu pro nové jméno zaměstnance
Název nového zaměstnance Billy Patterson, není součástí seznamu entity **zaměstnance** ještě. Název nového zaměstnance je extrahován nejprve, aby bylo možné odeslat název do externího systému k vytvoření přihlašovacích údajů společnosti. Po vytvoření přihlašovacích údajů společnosti pověření zaměstnanec jsou přidány do seznamu entit **zaměstnance**.

**Zaměstnance** seznam byl vytvořen v [seznamu kurzu](luis-quickstart-intent-and-list-entity.md).

**Novýzaměstnanec** entita je jednoduché entity se žádné role. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Jednoduché entity s rolemi pro přemístění města
Nové zaměstnance a řady musí přesunout z aktuální Město ve městě, kde se nachází fiktivní společnosti. Protože nového zaměstnance můžou pocházet z libovolné Město, umístění musí být zjištěny. Nastavit seznam jako je například seznam entit nebude fungovat, protože by být extrahována pouze města v seznamu.

Názvy rolí přidružený zdroj a cíl měst musí být jedinečný mezi všechny entity. Snadný způsob, jak zajistit, aby že byly jedinečné role je a jejich spárování obsahující entity prostřednictvím strategie vytváření názvů. **NewEmployeeRelocation** entita je jednoduchou entitu s dvě role: **NewEmployeeReloOrigin** a **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Jednoduché entity potřebovat dostatek příklady detekovanou
Protože příklad utterance `Move new employee Robert Williams from Sacramento and San Francisco` má pouze entity se naučili počítače je potřeba zajistit dostatek příklad projevy k příslušnému záměru, takže entity, které jsou zjištěny.  

**Zatímco vzorky umožňují poskytovat méně příklad projevy, pokud nejsou zjištěny entity, neodpovídá vzoru.**

Pokud máte potíže s jednoduchou entitu zjišťování, protože je název, například města, zvažte přidání frázi seznam hodnoty jsou podobné. To pomáhá zjistit název města tím, že služba LUIS další signál o tomto typu slova nebo fráze. Fráze seznamy jenom pomohou vzor tím, že pomáhá s detekcí entity, která je nezbytná pro odpovídající vzor. 

## <a name="create-new-entities"></a>Vytvoří nové entity
1. Vyberte **sestavení** v horní nabídce.

2. Vyberte **entity** z levé navigace. 

3. Vyberte **Create new entity** (Vytvořit novou entitu).

4. V místním okně zadejte `NewEmployee` jako **jednoduché** entity.

5. Vyberte **Create new entity** (Vytvořit novou entitu).

6. V místním okně zadejte `NewEmployeeRelocation` jako **jednoduché** entity.

7. Vyberte **NewEmployeeRelocation** ze seznamu entit. 

8. Zadejte první role jako `NewEmployeeReloOrigin` a vyberte možnost enter.

9. Zadejte druhá role jako `NewEmployeeReloDestination` a vyberte možnost enter.

## <a name="create-new-intent"></a>Vytvořit nový záměr
Označování entit v následujícím postupu může být jednodušší, pokud před zahájením pak přidá zpátky, až budete hotovi s kroky v této části se odebere keyPhrase předem připravených entit. 

1. Vyberte **záměry** z levé navigace.

2. Vyberte **Create new intent** (Vytvořit nový záměr). 

3. Zadejte `NewEmployeeRelocationProcess` jako název v místním dialogovém záměru.

4. Zadejte následující příklad projevy, označování popisky nové entity. Hodnoty entity a role jsou tučným písmem. Nezapomeňte přepnout na **tokeny zobrazení** Pokud pro vás snadnější k označení textu. 

    Při používání popisků ve službě záměr nezadáte roli entity. Postup, který později při vytváření vzoru. 

    |Promluva|Novýzaměstnanec|NewEmployeeRelocation|
    |--|--|--|
    |Přesunout **Bob Jones** z **Seattle** k **Los Colinas**|Bob Jones|Praha, Los Colinas|
    |Přesunout **Dave C. Cooper** z **Redmond** k **města New York**|Dave C. Cooper|Redmond, New York City|
    |Přesunout **Jan Paul Smith** z **Toronto** k **Vancouver – západ**|Jan Paul Smith|Toronto, Vancouver – západ|
    |Přesunout **J. Benson** z **Bostonu** k **Staines po Thames**|J. Benson|Boston, Staines po Thames|
    |Přesunout **Travis "Trav" Hinton** z **Castelo Branco** k **Orlando**|Hinton Travis "Trav"|Castelo Branco Orlando|
    |Přesunout **Trevor Nottington III** z **Aranda de Duero** k **Boise**|Trevor Nottington III|Aranda de Duero Boise|
    |Přesunout **zotavení po havárii. Greg Williams** z **Orlando** k **Ellicott Město**|Dr. Greg Williams|Orlando, Ellicott Město|
    |Přesunout **Robert "Jana" Gregson** z **Kansas Město** k **Capistrana Juan po síti San**|Robert "Jana" Gregson|Kansas City, San Juan Capistrana|
    |Přesunout **Patti Owens** z **Bellevue** k **Rockford**|Patti Owens|Bellevue Rockford|
    |Přesunout **Janet Bartlet** z **Tuscan** k **Santa Fe**|Janet Bartlet|Tuscan Santa Fe|

    Jméno zaměstnance obsahuje řadu předponu, počet slov, syntaxi a přípona. To je důležité pro LUIS pochopit variace nové jméno zaměstnance. Názvy měst také mít celou řadu syntaxi a počet slov. Tato různých je důležité LUIS naučit, jak se tyto entity může vyskytovat utterance uživatele. 
    
    Pokud stejný počet slov a žádné další varianty konfigurací byla buď entity, by naučíte LUIS, že tato entita má jenom tento počet slov a žádné další varianty konfigurací. Služba LUIS nebude moci správně odhadnout širší sadě změn, protože se nebyl žádný zobrazovat. 

    Pokud jste odebrali keyPhrase entity, přidejte ji do aplikace nyní.

## <a name="train-the-luis-app"></a>Trénování aplikace LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publikování aplikace a získání adresy URL koncového bodu

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-without-pattern"></a>Koncový bod bez vzor dotazu

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Na konec adresy URL zadejte `Move Wayne Berry from Miami to Mount Vernon`. Poslední parametr řetězce dotazu je `q`, což je **dotaz** promluvy. 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

Skóre záměru předpovědi je pouze o 50 %. Pokud vaše klientská aplikace vyžaduje větší číslo, to je potřeba opravit. Entity, které nebyly buď předpovědět.

Vzory vám pomohou skóre predikcí, však entity musí být správně předpovědět před vzor odpovídá utterance. 

## <a name="add-a-pattern-that-uses-roles"></a>Přidejte vzor, který používá role
1. Vyberte **sestavení** v horním navigačním panelu.

2. Vyberte **vzory** v levém navigačním panelu.

3. Vyberte **NewEmployeeRelocationProcess** z **vyberte záměru** rozevíracího seznamu. 

4. Zadejte následujícímu vzoru: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Pokud trénování, publikovat a dotazování na koncový bod, může být uspokojivých výsledků zobrazíte, že entity, které nebyly nalezeny, takže vzorek neodpovídal, proto neměli zlepšit do predikce. Toto je důsledkem nedostatek projevy příklad s entitami s popiskem. Nepřidávat Další příklady přidáte frázi seznamu pro odstranění tohoto problému.

## <a name="create-a-phrase-list-for-cities"></a>Vytvoření seznamu frázi pro města
Měst, jako jsou jména osob jsou velmi obtížné, mohou být libovolné kombinaci slova a interpunkční znaménka. Ale měst oblasti a svět jsou známé, takže LUIS musí frázi seznam měst začít učit. 

1. Vyberte **seznam frází** z **zvýšit výkon aplikace** část v levé nabídce. 

2. Název seznamu `Cities` a přidejte následující `values` seznamu:

    |Hodnoty seznamu frází|
    |--|
    |Seattle|
    |Síť SAN Diegu|
    |New York City|
    |Los Angeles|
    |Portland|
    |Philadephia|
    |Miami|
    |Dallas|

    Nepřidávejte každé město v celém světě nebo dokonce každé město v oblasti. Služba LUIS musí být schopen generalize městě je v seznamu. 

    Ujistěte se, že aby **tyto hodnoty jsou zaměnitelné** vybrané. Toto nastavení znamená, že slova v seznamu na považován za synonym. To je přesně jak by zpracovávat ve vzoru.

    Mějte na paměti [posledního](luis-quickstart-primary-and-secondary-data.md) vytvořili seznam frázi série kurzů jste také a zvýšit tak detekci entity jednoduché entity.  

3. Trénování a publikování aplikace.

## <a name="query-endpoint-for-pattern"></a>Koncový bod dotazu pro model
1. V dolní části stránky **Publish** (Publikovat) vyberte odkaz na **koncový bod**. Tato akce otevře další okno prohlížeče s adresou URL koncového bodu v adresním řádku. 

2. Na konec adresy URL zadejte `Move wayne berry from miami to mount vernon`. Poslední parametr řetězce dotazu je `q`, což je **dotaz** promluvy. 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

Záměru skóre je teď mnohem vyšší a názvy rolí jsou součástí odpovědi entity.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Přečtěte si osvědčené postupy pro aplikace LUIS](luis-concept-best-practices.md)
