---
title: Jak hledat adresu pomocí služby Azure Maps Search | Dokumentace Microsoftu
description: Zjistěte, jak vyhledat adresu pomocí služby Azure Search mapy
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: fe3bb3a778a42696cd15f9e4265448479bf043a1
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/11/2018
ms.locfileid: "42056794"
---
# <a name="how-to-find-an-address-using-the-azure-maps-search-service"></a>Jak najít adresu pomocí služby vyhledávání Azure Maps

Search service map je RESTful sadu rozhraní API vývojářům vyhledávat adresy, místa, body zájmu, výpisy firem a jiných zeměpisných údajů. Služba přiřadí zeměpisné šířky a délky na konkrétní adresu, křížové ulici, geografické funkci nebo bodu zájmu (POI). Hodnoty zeměpisné šířky a délky vrácenou při hledání může používat jako parametry v jiných mapy služeb, třeba toku provozu a trasách.

## <a name="prerequisites"></a>Požadavky

Aby všechna volání do rozhraní API služby Maps, potřebujete účet Maps a klíč. Informace o vytváření účtu a klíč načítání, najdete v části [jak spravovat účet Azure Maps a klíče](how-to-manage-account-keys.md).

Tento článek používá [aplikaci Postman](https://www.getpostman.com/apps) vytvářet volání REST. Můžete použít všechny vývojové prostředí rozhraní API, které dáváte přednost. 


## <a name="using-fuzzy-search"></a>Použití vyhledávání přibližných shod

Výchozí rozhraní API pro službu search je [vyhledávání přibližných shod](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), která zpracovává vstupy libovolnou kombinaci adres nebo tokenů POI. Toto rozhraní API pro vyhledávání je canonical "jednořádkový hledání" a je užitečné, pokud si nejste jisti co vaše uživatelské vstupy jako vyhledávací dotaz. Rozhraní API pro vyhledávání přibližných shod je kombinací POI vyhledávání a geokódování. Rozhraní API může také být zváženo v porovnání s kontextové pozice (tabulky lat./fyzický pevný disk. Pair –), plně omezeny souřadnic a poloměru nebo mohou být provedeny obecněji bez jakékoli geograficky usměrnění údajů bodu ukotvení.

Většinu vyhledávacích dotazů ve výchozím nastavení "maxFuzzyLevel = 1' Chcete-li získat výkon a snížit neobvyklé výsledky. Toto výchozí nastavení můžete přepsat, podle potřeby každý požadavek předáním parametru dotazu "maxFuzzyLevel = 2' nebo '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Hledání adresy použití vyhledávání přibližných shod

1. Otevřete aplikaci Postman, klikněte na nový | Vytvořte nový a vyberte **požadavek GET**. Zadejte název žádosti o **vyhledávání přibližných shod**, vyberte příslušnou kolekci nebo složce a uložit ho. tím, klikněte na tlačítko **Uložit**.

2. Na kartě tvůrce, vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.

    ![Vyhledávání přibližných shod ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/fuzzy/json? |
    | Autorizace | Bez ověřování |

    **Json** Určuje atribut v cestě adresy URL, formát odpovědi. Json v celém tomto článku využívají pro snadné použití a čitelnost. Můžete najít formátů dostupné odpovědi v **získat vyhledávání přibližných shod** definice [odkaz funkční rozhraní API map] (https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Klikněte na tlačítko **Params**a zadejte následující klíč / hodnota dvojice, které budou používat jako parametry dotazu nebo cesta v adrese URL požadavku:

    ![Vyhledávání přibližných shod ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze API-version | 1.0 |
    | klíč předplatného | \<klíč Azure Maps\> |
    | query | pizza |

4. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 

    Řetězec dotazu nejednoznačný "pizza" vrátí 10 bodů zájmu (POI) výsledky s kategorií, které spadají do "pizza" a "restaurace". Každý výsledek vrátí adresu, zeměpisné šířky / délky hodnoty view portu a vstupní bod pro umístění.
    
    Výsledky jsou nastaveny pro tento dotaz nejsou vázané na libovolný odkaz na konkrétní umístění. Můžete použít **countrySet** parametr zadat jenom v zemích, u kterých vaše aplikace potřebuje pokrytí, jako je výchozí chování pro celý svět, vrací potenciálně zbytečné výsledky hledání.

5. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |------------------|-------------------------|
    | countrySet | USA |
    
    Výsledky jsou nyní ohraničené směrové číslo země a dotaz vrátí pizza restaurace ve Spojených státech.
    
    K poskytování výsledků orientovaný na konkrétní umístění, můžete dotazovat bodu zájmu a použít vrácené zeměpisná šířka a zeměpisná délka hodnoty ve volání do služby vyhledávání přibližných shod. V tomto případě používá službu Search nevrátilo umístění ručička místo Seattle a použít tabulky lat. / fyzický pevný disk. hodnoty k orientaci hledání.
    
4. Parametry, zadejte následující klíč / páry hodnot a klikněte na tlačítko **odeslat**:

    ![Vyhledávání přibližných shod ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Klíč | Hodnota |
    |-----|------------|
    | LAT | 47.62039 |
    | fyzický pevný disk | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Vyhledat vlastnosti adresy a souřadnice 

Můžete předat úplné nebo částečné adresu na adresu hledání rozhraní API a přijetí odpovědi, který obsahuje adresu podrobné vlastnosti, jako je magistrát nebo dělení, jakož i poziční hodnoty v zeměpisné šířky a délky.

1. V nástroji Postman, klikněte na tlačítko **novou žádost o** | **požadavek GET** a pojmenujte ho **hledání adresy**.
2. Na kartě tvůrce, vyberte **získat** metodu HTTP, zadejte adresu URL požadavku pro koncový bod rozhraní API a vyberte autorizační protokol, pokud existuje.

    ![Hledání adresy ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/json? |
    | Autorizace | Bez ověřování |

2. Klikněte na tlačítko **Params**a zadejte následující klíč / hodnota dvojice, které budou používat jako parametry dotazu nebo cesta v adrese URL požadavku:
    
    ![Hledání adresy ](./media/how-to-search-for-address/address_search_params.png)
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze API-version | 1.0 |
    | klíč předplatného | \<klíč Azure Maps\> |
    | query | 400 širokým SV, Seattle, WA 98109 |
    
3. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 
    
    V tomto případě zadaný dotaz úplnou adresu a zobrazit jeden výsledek v textu odpovědi. 
    
4. V parametrech upravte řetězec dotazu, který má následující hodnotu:
    ```
        400 Broad, Seattle
    ```

5. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | typeahead | true (pravda) |

    **Typeahead** příznak říká rozhraní API pro vyhledávání adresu považovat za částečné vstup dotazu a vrátí pole hodnot, prediktivní.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Vyhledejte adresu pomocí reverzního hledání adresy
1. V nástroji Postman, klikněte na tlačítko **novou žádost o** | **požadavek GET** a pojmenujte ho **reverzního vyhledávání adresu**.

2. Na kartě tvůrce, vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.
    
    ![Zpětná adresa URL vyhledávání ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/reverse/json? |
    | Autorizace | Bez ověřování |
    
2. Klikněte na tlačítko **Params**a zadejte následující klíč / hodnota dvojice, které budou používat jako parametry dotazu nebo cesta v adrese URL požadavku:
    
    ![Zpětná adresa parametry hledání ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze API-version | 1.0 |
    | klíč předplatného | \<klíč Azure Maps\> |
    | query | 47.59093,-122.33263 |
    
3. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 
    
    Odpověď obsahuje položku POI pro Safeco Field se poi kategorii "stadionem". 
    
4. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | číslo | true (pravda) |

    Pokud [číslo](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu se odešle s požadavkem, odpověď může obsahovat straně ulice (doleva nebo doprava) a také posunutí pozice za daný počet.
    
5. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | returnSpeedLimit | true (pravda) |
    
    Když [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je nastavena, odpověď vrácení odeslaných omezení rychlosti.

6. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | returnRoadUse | true (pravda) |

    Když [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je nastavena, odpověď se vrátí pole použití silniční reversegeocodes ulice na úrovni.

7. Přidat následující klíč / hodnota do **Params** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | roadUse | true (pravda) |

    Dotaz zpětné geokód můžete omezit na určitý typ pomocí použití silniční [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Hledání napříč ulice pomocí reverzního ulice hledání různé adresy

1. V nástroji Postman, klikněte na tlačítko **novou žádost o** | **požadavek GET** a pojmenujte ho **zpětná adresa pro různé ulice hledání**.

2. Na kartě tvůrce, vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.
    
    ![Reverse hledání napříč ulici adresy ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Autorizace | Bez ověřování |
    
3. Klikněte na tlačítko **Params**a zadejte následující klíč / hodnota dvojice, které budou používat jako parametry dotazu nebo cesta v adrese URL požadavku:
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze API-version | 1.0 |
    | klíč předplatného | \<klíč Azure Maps\> |
    | query | 47.59093,-122.33263 |
    
4. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 

## <a name="next-steps"></a>Další postup
- Prozkoumejte [vyhledávací služby Azure Maps](https://docs.microsoft.com/rest/api/maps/search) dokumentace k rozhraní API 
