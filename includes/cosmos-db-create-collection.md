---
title: zahrnout soubor
description: zahrnout soubor
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: cf77eaa07d45222cecf0450fb33fe62e556bcd9e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38728991"
---
Teď můžete pomocí nástroje Průzkumník dat na webu Azure Portal vytvořit databázi a kolekci. 

1. Klikněte na **Průzkumník dat** > **Nová kolekce**. 
    
    Úplně vpravo se zobrazí oblast **Přidat kolekci**. Pokud ji nevidíte, možná se budete muset posunout doprava.

    ![Průzkumník dat na webu Azure Portal – okno Přidat kolekci](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. Na stránce **Přidat kolekci** zadejte nastavení pro novou kolekci.

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|Úlohy|Jako název nové databáze zadejte *Tasks*. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky /, \\, #, ? ani koncové mezery.
    ID kolekce|Items|Jako název nové kolekce zadejte *Items*. ID kolekcí mají stejné požadavky na znaky jako názvy databází.
    Kapacita úložiště| Pevná (10 GB)|Použijte výchozí hodnotu **Pevná (10 GB)**. Tato hodnota je kapacita úložiště databáze.
    Propustnost|400 RU|Změňte propustnost na 400 jednotek žádostí za sekundu (RU/s). Abyste mohli propustnost nastavit na 400 RU/s, kapacita úložiště musí být nastavená na **Pevná (10 GB)**. Pokud budete chtít snížit latenci, můžete propustnost později navýšit. 
    
    Kromě předchozích nastavení můžete pro kolekci volitelně přidat **Jedinečné klíče**. V tomto příkladu ponecháme toto pole prázdné. Jedinečné klíče umožňují vývojářům přidat do databáze vrstvu integrity dat. Vytvořením zásady jedinečného klíče při vytváření kolekce zajistíte jedinečnost jedné nebo více hodnot pro každý klíč oddílu. Další informace najdete v článku [Jedinečné klíče ve službě Azure Cosmos DB](../articles/cosmos-db/unique-keys.md).
    
    Klikněte na **OK**.

    Průzkumník dat zobrazí novou databázi a kolekci.

    ![Průzkumník dat na webu Azure Portal zobrazující novou databázi a kolekci](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)