---
title: Kopírování dat z Oraclu služby v cloudu pomocí Azure Data Factory (Preview) | Dokumentace Microsoftu
description: Informace o kopírování dat z cloudové služby Oracle do úložišť dat podporovaných jímky pomocí aktivity kopírování v kanálu Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: jingwang
ms.openlocfilehash: a576b94881e114a97e58bf93515e372221da3346
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44096190"
---
# <a name="copy-data-from-oracle-service-cloud-using-azure-data-factory-preview"></a>Kopírování dat z Oraclu služby v cloudu pomocí Azure Data Factory (Preview)

Tento článek popisuje, jak pomocí aktivity kopírování ve službě Azure Data Factory ke zkopírování dat z cloudové služby Oracle. Je nástavbou [přehled aktivit kopírování](copy-activity-overview.md) článek, který nabízí obecný přehled o aktivitě kopírování.

> [!IMPORTANT]
> Tento konektor je aktuálně ve verzi preview. Můžete vyzkoušet a poskytnout zpětnou vazbu. Pokud do svého řešení chcete zavést závislost na konektorech ve verzi Preview, kontaktujte [podporu Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Podporované funkce

Kopírování dat z cloudové služby Oracle do jakékoli podporovaného úložiště dat jímky. Seznam úložišť dat podporovaných aktivitou kopírování jako zdroje a jímky, najdete v článku [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Poskytuje integrované ovladače chcete umožnit připojení k Azure Data Factory, proto není nutné ručně nainstalovat všechny ovladače používání tohoto konektoru.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Následující části obsahují podrobnosti o vlastnostech, které se používají k definování entit služby Data Factory konkrétní konektor Oracle cloudové služby.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro cloudové služby Oracle propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost type musí být nastavená na: **OracleServiceCloud** | Ano |
| hostitel | Adresa URL instance cloudové služby Oracle.  | Ano |
| uživatelské jméno | Uživatelské jméno, který používáte pro přístup k serveru Oracle cloudové služby.  | Ano |
| heslo | Heslo odpovídající uživatelské jméno, které jste zadali v klíči uživatelské jméno. Můžete označit pole jako SecureString bezpečně uložit ve službě ADF nebo ukládání hesel ve službě Azure Key Vault a umožnit ADF kopírování acitivty o přijetí změn z něj při kopírování dat – Další informace z [Store přihlašovacích údajů ve službě Key Vault](store-credentials-in-key-vault.md). | Ano |
| useEncryptedEndpoints | Určuje, zda jsou koncové body zdroje dat šifrovat pomocí protokolu HTTPS. Výchozí hodnota je true.  | Ne |
| useHostVerification | Určuje, jestli se vyžaduje název hostitele v certifikátu serveru tak, aby odpovídaly názvu hostitele serveru při připojení přes protokol SSL. Výchozí hodnota je true.  | Ne |
| usePeerVerification | Určuje, jestli se má ověřit identitu serveru při připojení přes protokol SSL. Výchozí hodnota je true.  | Ne |

**Příklad:**

```json
{
    "name": "OracleServiceCloudLinkedService",
    "properties": {
        "type": "OracleServiceCloud",
        "typeProperties": {
            "host" : "<host>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datových sad](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností, které podporuje Cloudová služba Oracle datové sady.

Ke zkopírování dat z cloudové služby Oracle, nastavte vlastnost typ datové sady na **OracleServiceCloudObject**. Neexistuje žádné další vlastnosti specifické pro typ. v tomto typu datové sady.

**Příklad**

```json
{
    "name": "OracleServiceCloudDataset",
    "properties": {
        "type": "OracleServiceCloudObject",
        "linkedServiceName": {
            "referenceName": "<OracleServiceCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností podporovaných zdrojem Oracle cloudové služby.

### <a name="oracle-service-cloud-as-source"></a>Oracle cloudové služby jako zdroj

Ke zkopírování dat z cloudové služby Oracle, nastavte typ zdroje v aktivitě kopírování do **OracleServiceCloudSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu zdroje aktivity kopírování musí být nastavena na: **OracleServiceCloudSource** | Ano |
| query | Použijte vlastní dotaz SQL číst data. Například: `"SELECT * FROM MyTable"`. | Ano |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromOracleServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OracleServiceCloud input dataset name>",
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
                "type": "OracleServiceCloudSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Další postup
Seznam úložišť dat podporovaných jako zdroje a jímky v aktivitě kopírování ve službě Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).
