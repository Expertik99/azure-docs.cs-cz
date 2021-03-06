---
title: Vytváření aplikací .NET jako neinteraktivní ověřování v Azure HDInsight
description: Zjistěte, jak vytvořit neinteraktivní ověřování aplikace Microsoft .NET v Azure HDInsight.
ms.reviewer: jasonh
services: hdinsight
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 4537c0308ee587d921dc795054966f6a3dbb69c4
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43093216"
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Vytvoření aplikace .NET HDInsight pro neinteraktivní ověřování
Můžete spustit aplikaci Microsoft .NET Azure HDInsight s identitou aplikace vlastní (neinteraktivní) nebo v části identita přihlášeného uživatele (interaktivní) aplikace. V tomto článku se dozvíte, jak vytvořit aplikaci .NET pro připojení k Azure a spravovat HDInsight pro neinteraktivní ověřování. Ukázku interaktivní aplikace najdete v tématu [připojení k Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). 

Jako neinteraktivní aplikace .NET budete potřebovat:

* Vaše ID tenanta předplatného Azure (tzv. *ID adresáře*). Zobrazit [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* ID klienta aplikace Azure Active Directory (Azure AD). Zobrazit [vytvoření aplikace Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) a [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
* Azure AD tajný klíč aplikace. Zobrazit [Get aplikace ověřovací klíč](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

## <a name="prerequisites"></a>Požadavky
* HDInsight cluster. Zobrazit [úvodní kurz](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Přiřazení role aplikace Azure AD
Přiřaďte aplikaci Azure AD [role](../role-based-access-control/built-in-roles.md), aby aplikaci udělil oprávnění k provedení akce. Nastavit obor na úrovni předplatného, skupinu prostředků nebo prostředek. Oprávnění se dědí do oboru na nižších úrovních. (Například přidáním aplikace k roli Čtenář pro skupinu prostředků znamená, že aplikace může číst skupinu prostředků a všechny prostředky v ní.) V tomto kurzu nastavit obor na úrovni skupiny prostředků. Další informace najdete v tématu [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../role-based-access-control/role-assignments-portal.md).

**Přidání role vlastníka do aplikace Azure AD**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo vyberte **skupiny prostředků**.
3. Vyberte skupinu prostředků, které má cluster HDInsight, na kterém budete spouštět dotaz Hive později v tomto kurzu. Pokud máte velký počet skupin prostředků, můžete použít filtr a vyberte ten, který chcete.
4. V nabídce skupiny prostředků, vyberte **řízení přístupu (IAM)**.
5. V části **uživatelé**vyberte **přidat**.
6. Postupujte podle pokynů k přidání role vlastníka do aplikace Azure AD. Po úspěšném přidání role aplikace uvedený v části **uživatelé**, v roli vlastník. 

## <a name="develop-an-hdinsight-client-application"></a>Vývoj klientské aplikace HDInsight

1. Vytvořte konzolovou aplikaci C#.
2. Přidejte následující balíčky NuGet:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Spusťte následující kód:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```


## <a name="next-steps"></a>Další postup
* [Vytvoření aplikace Azure Active Directory a instančního objektu na webu Azure Portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).
* Zjistěte, jak [ověřování instančního objektu pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md).
* Další informace o [řízení přístupu Azure na základě rolí (RBAC)](../role-based-access-control/role-assignments-portal.md).
