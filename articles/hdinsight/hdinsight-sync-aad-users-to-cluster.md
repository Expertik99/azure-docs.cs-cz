---
title: Synchronizace uživatelů Azure Active Directory do clusteru – Azure HDInsight
description: Synchronizujte ověřené uživatele z Azure Active Directory do clusteru.
services: hdinsight
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 08/19/2018
ms.openlocfilehash: 7e002a43c774bd1a6df9cfe46207ddebd02284b3
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43104219"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Synchronizace uživatelů Azure Active Directory pro HDInsight cluster

[Clustery HDInsight připojené k doméně](hdinsight-domain-joined-introduction.md) můžete používat silné ověřování s uživateli Azure Active Directory (Azure AD), stejně jako použít *řízení přístupu na základě rolí* zásady (RBAC). Jak budete přidávat uživatele a skupiny do služby Azure AD, můžete synchronizovat uživatele, kteří potřebují přístup k vašemu clusteru.

## <a name="prerequisites"></a>Požadavky

Pokud jste tak již neučinili, [vytvořit cluster HDInsight připojený k doméně](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Přidat nové Azure AD uživatelům

Chcete-li zobrazit hostitele, otevřete webové uživatelské rozhraní Ambari. Každý uzel se aktualizují pomocí nového nastavení bezobslužného upgradu.

1. V [webu Azure portal](https://portal.azure.com), přejděte do adresáře Azure AD přidružený cluster připojený k doméně.

2. Vyberte **všichni uživatelé** z nabídky vlevo vyberte **nového uživatele**.

    ![Všichni uživatelé podokno](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Vyplňte formulář nové uživatele. Vyberte skupiny, které jste vytvořili pro přiřazení oprávnění na základě clusteru. V tomto příkladu vytvořte skupinu s názvem "HiveUsers", do které můžete přiřadit nové uživatele. [Příklad pokyny](hdinsight-domain-joined-configure.md) pro vytvoření clusteru připojeného k doméně zahrnují přidání dvou skupin a `HiveUsers` a `AAD DC Administrators`.

    ![Nové podokno uživatele](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Vyberte **Vytvořit**.

## <a name="use-the-ambari-rest-api-to-synchronize-users"></a>Použití rozhraní Ambari REST API k synchronizaci uživatelů

V tu chvíli se synchronizují skupiny uživatelů zadaný během procesu vytváření clusteru. Synchronizace uživatelů proběhne automaticky jednou za hodinu. Okamžitě synchronizovat uživatele nebo synchronizovat skupiny než skupiny zadané při vytváření clusteru pomocí rozhraní Ambari REST API.

Pomocí rozhraní Ambari REST API používá následující Metoda POST. Další informace najdete v tématu [HDInsight Správa clusterů pomocí rozhraní Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Připojení ke clusteru pomocí SSH](hdinsight-hadoop-linux-use-ssh-unix.md). V podokně přehled pro váš cluster na webu Azure Portal, vyberte **Secure Shell (SSH)** tlačítko.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Zkopírujte zobrazený `ssh` příkaz a vložte ho do vašeho klienta SSH. Zadejte ssh heslo uživatele po zobrazení výzvy.

3. Po ověření, zadejte následující příkaz:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Odpověď by měla vypadat nějak takto:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Pokud chcete zobrazit stav synchronizace, spusťte novou `curl` příkaz:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    Odpověď by měla vypadat nějak takto:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. Tento výsledek zobrazí, že stav je **COMPLETE**jednoho nového uživatele vytvořil a uživateli se přiřadí členství. V tomto příkladu uživateli přiřazena "HiveUsers" synchronizovat skupiny protokolu LDAP, protože uživatel přidal do stejné skupiny ve službě Azure AD.

> [!NOTE]
> Předchozí metoda synchronizuje pouze podle skupiny Azure AD **přístupová skupina uživatelů** vlastnost nastavení domény při vytváření clusteru. Další informace najdete v tématu [vytvořit HDInsight cluster](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Ověřte nově přidané uživatele Azure AD

Otevřít [webovému uživatelskému rozhraní Ambari](hdinsight-hadoop-manage-ambari.md) pro ověření, že nové uživatele Azure AD byl přidán. Přístup k webovému uživatelskému rozhraní Ambari, tak, že přejdete do **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Zadejte uživatelské jméno správce clusteru a heslo.

1. Z řídicího panelu Ambari vyberte **spravovat Ambari** pod **správce** nabídky.

    ![Správa Ambari](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Vyberte **uživatelé** pod **uživatele a skupiny Správa** skupina nabídky na levé straně stránky.

    ![Položka nabídky uživatele](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. Nový uživatel by měla být uvedená v tabulce uživatelů. Typ je nastavený na `LDAP` spíše než `Local`.

    ![Stránka uživatele](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Přihlaste se k Ambari jako nový uživatel

Když se nový uživatel (nebo jiný uživatel domény) přihlásí k Ambari, používají jejich úplné uživatelské jméno a doménu přihlašovacích údajů Azure AD.  Ambari zobrazí alias uživatele, který je zobrazované jméno uživatele ve službě Azure AD. Uživatelské jméno má nový uživatel příklad `hiveuser3@contoso.com`. V Ambari, tento nový uživatel zobrazí jako `hiveuser3` , ale uživatel přihlásí do Ambari jako `hiveuser3@contoso.com`.

## <a name="see-also"></a>Další informace najdete v tématech

* [Konfigurace zásad Hivu ve HDInsight připojených k doméně](hdinsight-domain-joined-run-hive.md)
* [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md)
* [Povolit uživatelům Ambari](hdinsight-authorize-users-to-ambari.md)
