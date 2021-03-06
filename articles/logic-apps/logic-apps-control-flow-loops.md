---
title: Přidání smyčky, které opakování akce nebo zpracovat pole - Azure Logic Apps | Microsoft Docs
description: Vytvoření smyčky, které opakování akce pracovního postupu nebo zpracovat polí v Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 87595eeb0330a2d8210258c097c29b205b628cf4
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298181"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Vytvoření smyčky, které opakování akce pracovního postupu nebo zpracovat polí v Azure Logic Apps

K iteraci v rámci polí v aplikaci logiky, můžete použít [smyčka "Typu Foreach"](#foreach-loop) nebo [sekvenční smyčka "Foreach"](#sequential-foreach-loop). Iterace pro standardní smyčka "Foreach" spuštěna paralelně, zatímco iterace pro sekvenční smyčka "Foreach" spustit po jednom. Maximální počet položek pole, které může zpracovat "Foreach" smyčky v jednom logiku aplikaci spustit, najdete v části [omezení a konfigurace](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Pokud máte aktivační událost, která přijímá pole a chcete spustit pracovní postup pro každou položku pole, můžete *debatch* tohoto pole s [ **SplitOn** aktivovat vlastnost](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Chcete-li akce opakujte, dokud je splněna podmínka nebo došlo ke změně některých stavu, použijte ["Do" smyčka](#until-loop). Aplikace logiky nejprve provádí všechny akce uvnitř smyčky a poté zkontroluje podmínky jako poslední krok. Pokud je splněna podmínka, cyklus se ukončí. Jinak se opakuje smyčky. Maximální počet slova "Do" smyčky v jednom logiku aplikaci spustit, najdete v části [omezení a konfigurace](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Pokud předplatné nemáte, [zaregistrujte si bezplatný účet Azure](https://azure.microsoft.com/free/). 

* Základní znalosti o [postup vytvoření aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>Smyčka "Typu Foreach"

Opakování akce pro každou položku v pole, použijte smyčku "Foreach" v pracovním postupu logiku aplikace. Může obsahovat více akcí ve smyčce "Foreach" a "Foreach" smyčky v sobě navzájem lze vnořit. Ve výchozím nastavení se cykly ve smyčce standardní "Foreach" spustí paralelně. Maximální počet paralelní cyklů této "Foreach" můžete spustit smyčky, najdete v části [omezení a konfigurace](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Smyčka "Foreach" pracuje pouze s pole a použijte akce v smyčky `@item()` odkaz na zpracování každé položky v poli. Pokud zadáte data, která není v matici, pracovní postup aplikace logiky se nezdaří. 

Například tato aplikace logiky vám pošle denní souhrn z informačního kanálu RSS na web. Aplikace používá smyčku "Foreach", který odešle e-mailu každý nový položka nalezena.

1. [Vytvoření aplikace logiky Tato ukázka](../logic-apps/quickstart-create-first-logic-app-workflow.md) pomocí účtu, Outlook.com nebo Office 365 Outlook.

2. Mezi RSS aktivovat a odeslat e-mailu akce, přidejte smyčku "Foreach". 

   Přidání smyčky mezi kroky, přesuňte ukazatel přes šipku, ve které chcete přidat smyčky. 
   Vyberte **znaménko plus** (**+**), který se zobrazuje, zvolte **přidat pro každou**.

   ![Přidání smyčky "Foreach" mezi kroky](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Nyní sestavení smyčky. V části **vyberte výstup z předchozích kroků** po **přidávat dynamický obsah** se zobrazí seznam, vyberte **kanálu odkazy** pole, která je výstup z RSS aktivační událost. 

   ![Vyberte ze seznamu dynamických obsahu](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Můžete vybrat *pouze* pole výstupy z předchozího kroku.

   Vybrané pole se teď zobrazí tady:

   ![Vyberte pole](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. K provedení akce na každou položku pole, přetáhněte **e-mailovou zprávu** akce do **pro každou** smyčky. 

   Aplikace logiky může vypadat podobně jako tento příklad:

   ![Přidání kroků do smyčka "Typu Foreach"](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Uložte svou aplikaci logiky. Můžete ručně vyzkoušet aplikace logiky na panelu nástrojů návrháře zvolte **spustit**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definice smyčka "Foreach" (JSON)

Pokud pracujete v zobrazení kódu pro svou aplikaci logiky, můžete definovat `Foreach` smyčky v definici aplikace logiky JSON místo, například:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {
                "type": "ApiConnection",
                "inputs": {
                    "body": {
                        "Body": "@{item()}",
                        "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                        "To": "me@contoso.com"
                    },
                    "host": {
                        "api": {
                            "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                        },
                        "connection": {
                            "name": "@parameters('$connections')['office365']['connectionId']"
                        }
                    },
                    "method": "post",
                    "path": "/Mail"
                },
                "runAfter": {}
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
    }
},
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>Smyčka "Typu Foreach": sekvenční

Ve výchozím nastavení spouští každý cyklus ve smyčce "Foreach" paralelně pro každou položku pole. Chcete-li spustit každý cyklus postupně, nastavte **sekvenční** možnost "Foreach" smyčce.

1. V horním pravém rohu smyčky, zvolte **výpustky** (**...** ) > **Nastavení**.

   ![Smyčka "Foreach" Vyberte "..." > "Nastavení"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Zapnout **sekvenční** nastavení, zvolte **provádí**.

   ![Zapnout nastavení sekvenční smyčka "Typu Foreach"](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Můžete také nastavit **operationOptions** parametru `Sequential` v definici aplikace logiky JSON. Příklad:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {               
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
        "operationOptions": "Sequential"
    }
},
```

<a name="until-loop"></a>

## <a name="until-loop"></a>"Do" smyčky
  
Opakování akce, dokud je splněna podmínka nebo došlo ke změně některých stavu, použijte smyčky "Dokud" v pracovním postupu logiku aplikace. Zde jsou některé běžné případy použití, kde můžete použít smyčky "Dokud":

* Volání koncový bod, dokud nezískáte odpověď, který chcete.
* Vytvořit záznam v databázi, počkejte na určitém poli v tom, že záznam schválení a pokračovat ve zpracování. 

Například v 8:00 AM každý den, tato aplikace logiky zvýší proměnné dokud hodnota proměnné se rovná 10. Aplikace logiky pak odešle e-mail, který potvrdí, že je aktuální hodnota. I když tento příklad používá Office 365 Outlook, které mohou využívat kteréhokoli zprostředkovatele e-mailu nepodporuje Logic Apps ([kontrolní seznam konektory zde](https://docs.microsoft.com/connectors/)). Pokud použijete jiný e-mailový účet, celkový postup bude stejný, ale vaše uživatelské rozhraní se může mírně lišit. 

1. Vytvoření prázdné aplikace logiky V návrháři aplikace logiky, vyhledejte "recurrence" a vyberte této aktivační události: **plán - opakování** 

   ![Přidat aktivační události "Plán opakování –"](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Zadejte, kdy aktivační událost aktivuje nastavením interval, četnost a hodinu dne. Chcete-li nastavit hodiny, zvolte **zobrazit rozšířené možnosti**.

   ![Přidat aktivační události "Plán opakování –"](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Vlastnost | Hodnota |
   | -------- | ----- |
   | **Interval** | 1 | 
   | **Frekvence** | Den |
   | **V těchto hodinách** | 8 |
   ||| 

3. V části aktivační událost, zvolte **nový krok** > **přidat akci**. Vyhledejte "proměnné" a pak vyberte tuto akci: **proměnné - inicializovat proměnné**

   ![Přidejte "Proměnné - inicializovat proměnná" akce](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Nastavte vaše proměnná s těmito hodnotami:

   ![Umožňuje nastavit vlastnosti proměnné](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Vlastnost | Hodnota | Popis |
   | -------- | ----- | ----------- |
   | **Název** | Omezení | Název vaše proměnná | 
   | **Typ** | Integer | Vaše proměnná datový typ | 
   | **Hodnota** | 0 | Vaše proměnná je počáteční hodnota | 
   |||| 

5. V části **inicializovat proměnná** akce, zvolte **nový krok** > **Další**. Vyberte tento smyčka: **přidat proveďte až**

   ![Přidání "provést až" smyčky](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Sestavení ukončení smyčku výběrem **Limit** proměnné a **rovná** operátor. Zadejte **10** jako hodnotu porovnání.

   ![Sestavení ukončovací podmínky pro ukončení smyčky](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Uvnitř smyčky, zvolte **přidat akci**. Vyhledejte "proměnné" a pak přidejte tuto akci: **proměnné - přírůstek proměnné**

   ![Přidat akci zvyšování hodnoty proměnné](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Pro **název**, vyberte **Limit** proměnné. Pro **hodnotu**, zadejte "1". 

   ![Přírůstek "Limit" o 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. V části, ale mimo smyčky přidejte akci, která odešle e-mail. Pokud budete vyzváni, přihlaste se k e-mailovému účtu.

   ![Přidat akci, která odešle e-mailu](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Nastavte vlastnosti e-mailu. Přidat **Limit** proměnných subjektu. Tímto způsobem můžete potvrdit, že aktuální hodnotu pro proměnnou splňuje zadanou podmínku, například:

    ![Nastavit vlastnosti e-mailu](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Vlastnost | Hodnota | Popis |
    | -------- | ----- | ----------- | 
    | **Komu** | *<email-address@domain>* | příjemce e-mailovou adresu. Pro testování, použijte vlastní e-mailovou adresu. | 
    | **Předmět** | Aktuální hodnota pro "Limit" je **Limit** | Zadejte předmět e-mailu. V tomto příkladu zkontrolujte, jestli jste zahrnuli **Limit** proměnné. | 
    | **Text** | <*obsah e-mailu*> | Zadejte e-mailové zprávy obsah, který chcete odeslat. V tomto příkladu zadejte jakýkoli text, který chcete. | 
    |||| 

11. Uložte svou aplikaci logiky. Můžete ručně vyzkoušet aplikace logiky na panelu nástrojů návrháře zvolte **spustit**.

    Až po logika spuštění, dostanete e-mail s obsahem, který jste zadali:

    ![Přijatých e-mailů](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Zabránit nekonečná smyčka

Smyčky "Dokud" má výchozí omezení, které zastavit provádění, pokud některá z těchto podmínek:

| Vlastnost | Výchozí hodnota | Popis | 
| -------- | ------------- | ----------- | 
| **Počet** | 60 | Maximální počet cykly, které spustí před ukončení smyčky. Výchozí hodnota je 60 cyklů. | 
| **Časový limit** | PT1H | Maximální množství času spuštění smyčku před smyčky ukončí. Výchozí hodnota je jedna hodina a je zadána ve formátu ISO 8601. <p>Hodnota časového limitu se vyhodnotí pro každý cyklus smyčky. Pokud žádnou akci ve smyčce trvá déle, než časový limit, není zastavit aktuální cyklus, ale na další cyklus není spustit, protože není splněna podmínka limit. | 
|||| 

Chcete-li změnit tato výchozí omezení, zvolte **zobrazit rozšířené možnosti** ve tvaru akce smyčky.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Do" definice (JSON)

Pokud pracujete v zobrazení kódu pro svou aplikaci logiky, můžete definovat `Until` smyčky v definici aplikace logiky JSON místo, například:

``` json
"actions": {
    "Initialize_variable": {
        // Definition for initialize variable action
    },
    "Send_an_email": {
        // Definition for send email action
    },
    "Until": {
        "type": "Until",
        "actions": {
            "Increment_variable": {
                "type": "IncrementVariable",
                "inputs": {
                    "name": "Limit",
                    "value": 1
                },
                "runAfter": {}
            }
        },
        "expression": "@equals(variables('Limit'), 10)",
        // To prevent endless loops, an "Until" loop 
        // includes these default limits that stop the loop. 
        "limit": { 
            "count": 60,
            "timeout": "PT1H"
        },
        "runAfter": {
            "Initialize_variable": [
                "Succeeded"
            ]
        },
    }
},
```

Například Tato smyčka "Dokud" zavolá koncový bod protokolu HTTP, který vytvoří prostředek a zastaví, když text odpovědi HTTP, vrátí se stavem "Dokončeno". Aby nekonečná smyčka smyčky také zastaví, pokud některá z těchto podmínek:

* Smyčky byla spuštěna 10krát zadané `count` atribut. Výchozí hodnota je 60 x. 
* Smyčky se pokusila spustit pro dvě hodiny podle specifikace `timeout` atribut ve formátu ISO 8601. Výchozí hodnota je jedna hodina.
  
``` json
"actions": {
    "myUntilLoopName": {
        "type": "Until",
        "actions": {
            "Create_new_resource": {
                "type": "Http",
                "inputs": {
                    "body": {
                        "resourceId": "@triggerBody()"
                    },
                    "url": "https://domain.com/provisionResource/create-resource",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                },
                "runAfter": {},
                "type": "ApiConnection"
            }
        },
        "expression": "@equals(triggerBody(), 'Completed')",
        "limit": {
            "count": 10,
            "timeout": "PT2H"
        },
        "runAfter": {}
    }
},
```

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy, navštivte [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Pro odeslání nebo hlasovat o funkcích a návrhy, [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další postup

* [Spustit kroky na základě podmínky (podmíněné příkazy)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Spustit kroky na základě různých hodnot (příkazech switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Spuštění nebo sloučení paralelní kroky (větve)](../logic-apps/logic-apps-control-flow-branches.md)
* [Spustit kroky na základě stavu seskupené akce (oborům)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
