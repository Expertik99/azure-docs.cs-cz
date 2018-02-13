---
title: "Kopírování dat z databáze Oracle pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z podporované zdrojové úložiště k databázi Oracle nebo Oracle na podporované podřízený úložiště pomocí služby Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: dfbc6e1d8bdf20cc7a0a4b1571882ba84487dddc
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Kopírování dat z a do databáze Oracle pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 - všeobecně dostupná](v1/data-factory-onprem-oracle-connector.md)
> * [Verze 2 – Preview](connector-oracle.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do databáze Oracle. Vychází [aktivity kopírování přehled](copy-activity-overview.md) článek, který představuje obecný přehled o aktivitě kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 objektu pro vytváření dat, která je obecně k dispozici, najdete v části [Oracle konektoru verze 1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data z databáze Oracle do úložiště dat žádné podporované jímky. Také můžete zkopírovat data z úložiště dat žádné podporované zdrojové k databázi Oracle. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Oracle podporuje následující verze databáze Oracle. Také podporuje ověřování Basic nebo ID objektu:

- R1 Oracle 12c (12.1)
- R1 Oracle 11g nebo R2 (11.1, 11.2)
- R1 Oracle 10g, R2 (10,1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Požadavky

Ke zkopírování dat z a do databáze Oracle, který není veřejně přístupný, musíte nastavit Self-hosted integrace Runtime. Další informace o běhu integrace najdete v tématu [Self-hosted integrace Runtime](create-self-hosted-integration-runtime.md). Modul runtime integrace poskytuje integrované ovladače Oracle. Proto nemusíte ručně nainstalovat ovladač při kopírování dat z a do databáze Oracle.

## <a name="get-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru Oracle.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Následující vlastnosti jsou podporovány pro Oracle propojené služby.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavená na **Oracle**. | Ano |
| připojovací řetězec | Určuje informace potřebné pro připojení k instanci databáze Oracle. Toto pole označit jako SecureString bezpečně uložit v datové továrně nebo [odkazovat tajného klíče uložené v Azure Key Vault](store-credentials-in-key-vault.md).<br><br>**Typ připojení podporovaný**: můžete použít **Oracle SID** nebo **název služby Oracle** k identifikaci vaší databáze:<br>– Pokud používáte SID:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>– Pokud používáte název služby:`Host=<host>;Port=<port>;ServiceName=<sid>;User Id=<username>;Password=<password>;` | Ano |
| connectVia | [Integrace runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje datovou sadu Oracle.

Ke zkopírování dat z a do databáze Oracle, nastavte vlastnost typu datové sady, která **OracleTable**. Jsou podporovány následující vlastnosti.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na **OracleTable**. | Ano |
| tableName |Název tabulky v databázi Oracle, který propojená služba odkazuje na. | Ano |

**Příklad:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Oracle zdroj a jímka.

### <a name="oracle-as-a-source-type"></a>Oracle jako typ zdroje

Ke zkopírování dat z databáze Oracle, nastavte typ zdroje v aktivitě kopírování do **OracleSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na **OracleSource**. | Ano |
| oracleReaderQuery | Čtení dat pomocí vlastního dotazu SQL. Příklad: `"SELECT * FROM MyTable"`. | Ne |

Pokud nezadáte "oracleReaderQuery", sloupce definované v části "struktura" datové sady se používají k vytvoření dotazu (`select column1, column2 from mytable`) ke spouštění databáze Oracle. Pokud definice datové sady nemá "struktura", jsou vybrány všechny sloupce z tabulky.

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Oracle jako typ jímky

Pokud chcete zkopírovat data do databáze Oracle, nastavte typ jímky v aktivitě kopírování do **OracleSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky aktivity kopírování musíte nastavit **OracleSink**. | Ano |
| writeBatchSize | Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.<br/>Povolené hodnoty jsou celé číslo (počet řádků). |Ne (výchozí hodnota je 10 000) |
| writeBatchTimeout | Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.<br/>Povolené hodnoty jsou časový interval. Příkladem je 00:30:00 (30 minut). | Ne |
| preCopyScript | Zadejte dotaz SQL pro aktivitu kopírování ke spuštění před zápis dat do Oracle při každém spuštění. Tato vlastnost slouží vyčistit předem načtené data. | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Datový typ mapování pro Oracle

Při kopírování dat z a do databáze Oracle, se používají následující mapování Oracle datových typů k objektu pro vytváření dat dočasné datové typy. Další informace o aktivitě kopírování jak mapuje typ zdroje schéma a data na jímky najdete v tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md).

| Oracle datový typ | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |
| BFILE |Byte[] |
| OBJEKT BLOB |Byte[]<br/>(podporováno pouze Oracle 10g a vyšší) |
| CHAR – |Řetězec |
| DATOVÝ TYP CLOB |Řetězec |
| DATE (Datum) |Datum a čas |
| PLOVOUCÍ DESETINNÁ ČÁRKA |Decimal, řetězec (Pokud přesnost > 28) |
| CELÉ ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| DLOUHÁ |Řetězec |
| DLOUHO NEZPRACOVANÁ |Byte[] |
| NCHAR |Řetězec |
| NCLOB |Řetězec |
| ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| NVARCHAR2 |Řetězec |
| NEZPRACOVANÁ |Byte[] |
| ID ŘÁDKU |Řetězec |
| ČASOVÉ RAZÍTKO |Datum a čas |
| ČASOVÉ RAZÍTKO S MÍSTNÍM ČASOVÉM PÁSMU |Řetězec |
| ČASOVÉ RAZÍTKO S ČASOVÝM PÁSMEM |Řetězec |
| CELÉ ČÍSLO BEZ ZNAMÉNKA |Číslo |
| VARCHAR2 |Řetězec |
| XML |Řetězec |

> [!NOTE]
> Datové typy INTERVAL roku na měsíc a den na INTERVAL druhý nejsou podporovány.


## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v objektu pro vytváření dat najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).