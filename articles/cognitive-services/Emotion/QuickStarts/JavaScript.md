---
title: Rychlý start rozpoznávání emocí úrovně rozhraní API JavaScript | Microsoft Docs
description: Get informace a ukázky kódu můžete rychle začít používat rozhraní API pro rozpoznávání emocí úrovně v jazyce JavaScript v kognitivní služby.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 2578b0212f9b4a6483402074d7c9eff73e51ca6b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/23/2018
ms.locfileid: "35342704"
---
# <a name="emotion-api-javascript-quick-start"></a>Rychlý Start rozpoznávání emocí úrovně rozhraní API jazyka JavaScript

> [!IMPORTANT]
> 30. října 2017 ukončen video Preview rozhraní API. Vyzkoušet nový [Preview rozhraní API Indexer Video](https://azure.microsoft.com/services/cognitive-services/video-indexer/) k snadno rozbalte statistiky z videa a vylepšení možnosti zjišťování obsahu, jako je například výsledky hledání, pomocí zjišťování mluvené slovo, řezy, znaků a emoce. [Další informace](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Tento článek obsahuje informace a ukázky kódu, které vám pomohou rychle začít používat [rozpoznávání emocí úrovně rozhraní API rozpoznat metoda](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) v jazyce JavaScript rozpoznat emoce vyjádřená jeden nebo více osob v obraze.

## <a name="prerequisite"></a>Požadavek
* Získat klíč bezplatné předplatné [zde](https://azure.microsoft.com/try/cognitive-services/), nebo pokud máte předplatné Azure, vytvořte prostředek rozhraní API pro rozpoznávání emocí úrovně a získat klíč předplatného a koncového bodu existuje.

![Vytvořte prostředek rozpoznávání emocí úrovně rozhraní API](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Rozpoznat požadavek emoce příklad v jazyce JavaScript

Zkopírujte následující a uložte ho do souboru, jako `test.html`. Žádost o změnu `url` použít tak umístění, kde získat klíče pro předplatné a nahraďte hodnotu "Ocp-Apim-Subscription-Key" klíč platné předplatné. Ty lze najít na portálu Azure v části Přehled a klíče rozhraní API pro rozpoznávání emocí úrovně prostředku, v uvedeném pořadí. 

![Koncový bod rozhraní API](../Images/api-url.png)

![Klíč rozhraní API předplatného](../Images/keys.png)

a změňte textu požadavku na umístění bitové kopie, kterou chcete použít. Ke spuštění souboru ukázce přetažení myší do prohlížeče.

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };
      
        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }
            
            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
```

## <a name="recognize-emotions-sample-response"></a>Rozpoznat emoce ukázková odpověď
Úspěšné volání vrátí pole položek vzhled a jejich přidružené rozpoznávání emocí úrovně skóre podle velikosti obdélníku řez v sestupném pořadí. Prázdnou odpověď označuje, že nebyly zjištěny žádné řezy. Položka rozpoznávání emocí úrovně obsahuje následující pole:
* faceRectangle - obdélníku umístění řez v bitové kopii.
* skóre - rozpoznávání emocí úrovně skóre pro každý řez v bitové kopii. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
