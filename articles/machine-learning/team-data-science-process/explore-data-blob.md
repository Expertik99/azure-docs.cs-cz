---
title: Zkoumání dat ve službě Azure blob storage pomocí knihovny Pandas | Dokumentace Microsoftu
description: Popisuje, jak zkoumat data, která je uložená v kontejneru objektů blob v Azure pomocí Pandas.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 61943da154483d8e815fa135fba30b136724b011
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/21/2018
ms.locfileid: "42055692"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Zkoumání dat ve službě Azure Blob Storage pomocí knihovny Pandas
Tento dokument popisuje, jak zkoumat data, která je uložená v kontejneru objektů blob v Azure pomocí [Pandas](http://pandas.pydata.org/) balíček Pythonu.

Následující **nabídky** odkazy na témata, které popisují způsob použití nástroje pro zkoumání dat z různých prostředí úložiště. Tato úloha je nějaký krok [vědecké zpracování dat](overview.md).

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* Vytvoření účtu služby Azure storage. Pokud potřebujete pokyny, přečtěte si [vytvoření účtu služby Azure Storage](../../storage/common/storage-quickstart-create-account.md)
* Vaše data uložená v účtu úložiště objektů blob v Azure. Pokud potřebujete pokyny, přečtěte si [přesun dat do a z Azure Storage](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Načtení dat do struktury Pandas DataFrame
K prozkoumání a manipulaci s datovou sadu, ho musíte nejprve stáhnout na zdroj objektu blob do místního souboru, který lze načíst v Pandas DataFrame. Tady jsou kroky pro tento postup:

1. Stahování dat z Azure blob s následující vzorový kód Pythonu pomocí služby blob service. Nahraďte konkrétní hodnoty proměnné v následujícím kódu: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. Načtení dat do dat – rámec Pandas ze staženého souboru.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nyní jste připraveni na zkoumání dat a generovat funkce pro tuto datovou sadu.

## <a name="blob-dataexploration"></a>Příklady použití Pandasu zkoumání dat
Tady je pár příkladů, jak zkoumat data pomocí Pandas:

1. Zkontrolujte **počet řádků a sloupců** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Kontrola** několik prvním nebo posledním **řádky** v datové sadě následující:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Zkontrolujte, **datový typ** každý sloupec byl importován jako následující ukázkový kód, pomocí
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Zkontrolujte, **základní statistiky** pro sloupce v datech nastavte následujícím způsobem
   
        dataframe_blobdata.describe()
5. Podívejte se na počet položek pro každou hodnotu sloupce, které následujícím způsobem
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Počet chybějících hodnot** a skutečný počet položek v jednotlivých sloupcích pomocí následujícího ukázkového kódu
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Pokud máte **chybějící hodnoty** pro konkrétní sloupec v datech, můžete zrušit je následujícím způsobem:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape
   
   Dalším způsobem, jak nahradit chybějících hodnot je ve funkci režimu:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< Název_sloupce >: .mode()[0]}) dataframe_blobdata [. < Název_sloupce >']        
8. Vytvoření **histogram** vykreslení pomocí proměnný počet intervalů k vykreslení distribuce proměnné    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Podívejte se na **korelace** mezi proměnné pomocí diagnostického nebo funkcí vestavěné korelace
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

