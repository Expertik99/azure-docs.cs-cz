---
title: Ovladač systému souborů objektů Blob v Azure pro verzi Preview služby Azure Data Lake Storage Gen2
description: Ovladač systému souborů Hadoop ABFS
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: dedf398064dd0a49e5691e952ea7c9b6d16e34fd
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054613"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Ovladač systému souborů Azure Blob (ABFS): vyhrazené ovladač Azure Storage pro Hadoop

Jednou z metod primární přístup k datům ve verzi Preview služby Azure Data Lake Storage Gen2 je prostřednictvím [systému souborů Hadoop](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Azure Data Lake Storage Gen2 funkce přidružené ovladače, ovladač systému souborů Azure Blob nebo `ABFS`. ABFS je součástí systému Apache Hadoop a je součástí mnoha komerčních distribuce hadoopu. Pomocí tohoto ovladače, mnoho aplikací a architektur můžou k datům v Data Lake Storage Gen2 bez jakéhokoli kódu výslovně odkazuje na službu Data Lake Storage Gen2.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Předchozí funkce: ovladače Windows Azure Storage Blob

Ovladače Windows Azure Storage Blob nebo [WASB ovladač](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) původní podporu pro Azure Storage BLOB k dispozici. Tento ovladač provádět složité úlohy systému souborů mapování sémantiku (podle potřeby pomocí rozhraní Hadoop systému souborů) na tento objekt uložení rozhraní se stylem podobným vystavený službou Azure Blob Storage. Tento ovladač i nadále podporuje tento model poskytuje vysoký výkon přístupu k datům uloženým v objektech BLOB, ale obsahuje značné množství kódu provádí toto mapování, kvůli tomu obtížné udržovat. Kromě toho některé operace, jako [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) a [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) při použití do adresáře vyžadovat ovladač obrovské množství operací (vzhledem k absenci objektu úložiště Podpora pro adresáře) která často vede k snížení výkonu. Nová služba Azure Data Lake Storage je navržená k překonání inherentní nedostatky WASB.

## <a name="the-azure-blob-file-system-driver"></a>Ovladač systému souborů Azure Blob

[Rozhraní Azure Data Lake Storage REST](https://docs.microsoft.com/en-us/rest/api/storageservices/data-lake-storage-gen2) je navržen pro podporu sémantiku systému souborů v Azure Blob Storage. Vzhledem k tomu, že systém souborů Hadoop je navržený tak, aby podporují stejnou sémantiku neexistuje žádný požadavek pro komplexní mapování v ovladači. Ovladač systému souborů Azure Blob (nebo ABFS) tedy překrytí pouhé klienta pro rozhraní REST API.

Existují však některé funkce, které ovladače musí i nadále provádět:

### <a name="uri-scheme-to-reference-data"></a>Schéma identifikátoru URI pro referenční data

Konzistentní s jinými implementacemi systému souborů v systému Hadoop, ovladač ABFS definuje vlastní schéma identifikátoru URI tak, aby (adresářů a souborů) může být výrazně materiály. Schéma identifikátoru URI je popsána v [použít identifikátor URI služby Azure Data Lake Storage Gen2](./introduction-abfs-uri.md). Struktura identifikátoru URI je: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Pomocí výše uvedených formát identifikátoru URI, standardní nástroje Hadoop a architektury je možné odkazovat na tyto prostředky:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Interně ovladač ABFS přeloží zadaný v identifikátoru URI k souborům a adresářům nebo prostředky a provede volání REST API služby Azure Data Lake Storage s odkazy.

### <a name="authentication"></a>Authentication

Ovladač ABFS podporuje dva typy ověřování tak, aby aplikaci Hadoop může zabezpečený přístup k prostředků obsažených v rámci účtu Data Lake Storage Gen2 podporuje. Úplné podrobnosti o dostupných ověřovací schémata jsou k dispozici v [Průvodci zabezpečením Azure Storage](../common/storage-security-guide.md). Jsou to tyto:

- **Sdílený klíč:** to uživatelům umožňuje přístup ke všem prostředkům v účtu. Klíč je zašifrované a uložené v konfiguraci systému Hadoop.

- **Azure Active Directory OAuth nosný Token:** jsou nosné tokeny služby Azure AD získaných a aktualizovat ovladač pomocí buď identitě koncového uživatele nebo instančního objektu nakonfigurované. Pomocí tohoto modelu ověřování, autorizaci veškerý přístup na základě za volání pomocí identity přidružené k zadaného tokenu a vyhodnotit proti přiřazené POSIX přístupu ovládacího prvku seznam (ACL).

### <a name="configuration"></a>Konfigurace

Všechny konfigurace pro ovladač ABFS je uložená v <code>core-site.xml</code> konfigurační soubor. V distribucích systému Hadoop s [Ambari](http://ambari.apache.org/), konfigurace může spravovat také pomocí webového portálu nebo rozhraní Ambari REST API.

Podrobnosti o všech položek podporované konfigurace, které jsou určené v [Hadoop oficiální dokumentaci](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Dokumentace ke službě Hadoop

Ovladač ABFS je plně dokumentovány v článku [Hadoop oficiální dokumentaci](http://hadoop.apache.org/docs/current/hadoop-azure/index.html)

## <a name="next-steps"></a>Další postup

- [Nastavení clusterů HDInsight](./quickstart-create-connect-hdi-cluster.md)
- [Vytvoření clusteru služby Azure Databricks](./quickstart-create-databricks-account.md)
- [Použití Azure Data Lake Storage Gen2 identifikátoru URI](./introduction-abfs-uri.md)
