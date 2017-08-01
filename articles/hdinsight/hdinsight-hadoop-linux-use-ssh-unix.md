---
title: "Použití SSH se systémem Hadoop – Azure HDInsight | Dokumentace Microsoftu"
description: "Pro přístup k systému HDInsight můžete použít Secure Shell (SSH). V tomto dokumentu najdete informace o připojení ke službě HDInsight pomocí příkazů SSH a SCP z klientů se systémem Windows, Linux, Unix nebo macOS."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "příkazy hadoop v linuxu, příkazy hadoop linux, hadoop macos, ssh hadoop, ssh hadoop cluster"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 3eb1d4df7ab87ec692716339eb0ecb9df4c58732
ms.contentlocale: cs-cz
ms.lasthandoff: 05/16/2017


---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>Připojení k HDInsightu (Hadoop) pomocí protokolu SSH

Zjistěte, jak používat [SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell) pro zabezpečené připojení ke clusteru Hadoop v Azure HDInsight. 

HDInsight může pro uzly v clusteru Hadoop jako operační systém využívat Linux (Ubuntu). Následující tabulka obsahuje informace o adrese a portu potřebné pro připojení ke službě HDInsight v Linuxu pomocí klienta SSH:

| Adresa | Port | Připojení... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Hraniční uzel (R Server v HDInsightu) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Hraniční uzel (všechny ostatní typy clusteru, pokud hraniční uzel existuje) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Primární hlavní uzel |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Sekundární hlavní uzel |

> [!NOTE]
> Parametr `<edgenodename>` nahraďte názvem hraničního uzlu.
>
> Nahraďte `<clustername>` názvem svého clusteru.
>
> Pokud cluster obsahuje hraniční uzel, doporučujeme vám __připojovat se k hraničnímu uzlu vždy__ pomocí příkazu SSH. Hlavní uzly hostují služby, které jsou pro stav clusteru Hadoop klíčové. Na hraničním uzlu se spustí jenom to, co na něj umístíte.
>
> Další informace o použití hraničních uzlů najdete v tématu věnovaném [použití hraničních uzlů v HDInsightu](hdinsight-apps-use-edge-node.md#access-an-edge-node).

## <a name="ssh-clients"></a>Klienti SSH

Systémy Linux, Unix a macOS nabízí příkazy `ssh` a `scp`. Klient `ssh` se v systému založeném na Linuxu nebo Unixu běžně používá k vytvoření vzdálené relace příkazového řádku. Klient `scp` slouží k bezpečnému kopírování souborů mezi vaším klientem a vzdáleným systémem.

Microsoft Windows ve výchozím nastavení neposkytuje žádné klienty SSH. Klienti `ssh` a `scp` jsou ve Windows k dispozici prostřednictvím následujících balíčků:

* [Azure Cloud Shell](../cloud-shell/quickstart.md): Balíček Cloud Shell přináší prostředí Bashe v prohlížeči a nabízí `ssh`, `scp` a další běžné linuxové příkazy.

* [Bash na Ubuntu ve Windows 10:](https://msdn.microsoft.com/commandline/wsl/about) Příkazy `ssh` a `scp` jsou dostupné prostřednictvím Bashe na příkazovém řádku Windows.

* [Git (https://git-scm.com/)](https://git-scm.com/): Příkazy `ssh` a `scp` jsou k dispozici prostřednictvím příkazového řádku GitBash.

* [GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) Příkazy `ssh` a `scp` jsou k dispozici prostřednictvím příkazového řádku GitHub Shell. GitHub Desktop se dá nakonfigurovat tak, aby jako příkazový řádek pro Git Shell používal Bash, příkazový řádek Windows nebo PowerShell.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH):](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH) Tým PowerShell portuje OpenSSH do Windows a poskytuje testovací verze.

    > [!WARNING]
    > Balíček OpenSSH zahrnuje serverovou komponentu SSH, `sshd`. Tato komponenta spustí ve vašem systému server SSH a umožní ostatním, aby se k němu připojili. Pokud nechcete ve vašem systému hostovat server SSH, nekonfigurujte tuto komponentu a neotevírejte port 22. Ke komunikaci s HDInsightem není potřeba.

Existuje také několik grafických klientů SSH, jako je [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) a [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). I když je možné tyto klienty použít pro připojení k HDInsightu, proces připojení se od použití nástroje `ssh` liší. Další informace najdete v dokumentaci ke grafickému klientu, který používáte.

## <a id="sshkey"></a>Ověřování: Klíče SSH

Klíče SSH používají k ověřování relací SSH [šifrování s veřejným klíčem](https://en.wikipedia.org/wiki/Public-key_cryptography). Klíče SSH jsou bezpečnější než hesla a nabízí snadný způsob, jak zabezpečit přístup ke clusteru Hadoop.

Pokud je váš účet SSH zabezpečený pomocí klíče, klient musí při připojení poskytnout odpovídající privátní klíč:

* Většinu klientů jde nakonfigurovat k použití __výchozího klíče__. Například klient `ssh` v prostředích Linux a Unix hledá privátní klíč v `~/.ssh/id_rsa`.

* Můžete zadat __cestu k privátnímu klíči__. U klienta `ssh` se k zadání cesty k privátnímu klíči využívá parametr `-i`. Například, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Pokud máte __více privátních klíčů__, které používáte na různých serverech, zvažte používání nástroje, jako je [SSH agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent). Nástroj `ssh-agent` může sloužit k automatickému výběru správného klíče při navazování relace SSH.

> [!IMPORTANT]
>
> Pokud privátní klíč zabezpečíte pomocí přístupového hesla, musíte při použití klíče zadat toto heslo. Nástroje, jako je `ssh-agent`, mohou pro usnadnění práce ukládat hesla do mezipaměti.

### <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH

K vytvoření souborů veřejného a privátního klíče použijte příkaz `ssh-keygen`. Následující příkaz generuje 2048bitový pár klíčů RSA, který je možné použít s HDInsightem:

    ssh-keygen -t rsa -b 2048

Během procesu vytváření klíčů se zobrazí výzva k zadání informací, třeba kde jsou klíče uložené nebo jestli se má použít přístupové heslo. Po dokončení tohoto procesu se vytvoří dva soubory: veřejný klíč a privátní klíč.

* __Veřejný klíč__ se používá k vytvoření clusteru HDInsight. Veřejný klíč má příponu `.pub`.

* __Privátní klíč__ se používá k ověření vašeho klienta pro cluster HDInsight.

> [!IMPORTANT]
> Klíče můžete zabezpečit pomocí přístupového hesla. Přístupové heslo je ve skutečnosti heslo k privátnímu klíči. I kdyby někdo získal váš privátní klíč, může ho použít jenom v případě, že má přístupové heslo.

### <a name="create-hdinsight-using-the-public-key"></a>Vytvoření HDInsightu s využitím veřejného klíče

| Metoda vytvoření | Jak použít veřejný klíč |
| ------- | ------- |
| **Azure Portal** | Zrušte zaškrtnutí políčka __Použít stejné heslo jako pro přihlášení ke clusteru__ a potom jako typ ověřování SSH vyberte __Veřejný klíč__. Nakonec vyberte soubor veřejného klíče nebo vložte testový obsah tohoto souboru do pole __Veřejný klíč SSH__.</br>![Dialogové okno Veřejný klíč SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Použijte parametr `-SshPublicKey` rutiny `New-AzureRmHdinsightCluster` a obsah veřejného klíče předejte jako řetězec.|
| **Azure CLI 1.0** | Použijte parametr `--sshPublicKey` příkazu `azure hdinsight cluster create` a obsah veřejného klíče předejte jako řetězec. |
| **Šablona Resource Manageru** | Příklad použití klíčů SSH s využití šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí klíče SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/). Element `publicKeys` v souboru [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) slouží při vytváření clusteru k předání klíčů do Azure. |

## <a id="sshpassword"></a>Ověřování: Heslo

Účty SSH je možné zabezpečit pomocí hesla. Když se připojíte k HDInsightu pomocí SSH, zobrazí se výzva k zadání hesla.

> [!WARNING]
> Ověřování heslem pro SSH nedoporučujeme. Hesla se dají uhádnout a můžou se stát terčem útoku hrubou silou. Místo toho doporučujeme využívat [klíče SSH k ověřování](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>Vytvoření HDInsight s využitím hesla

| Metoda vytvoření | Jak zadat heslo |
| --------------- | ---------------- |
| **Azure Portal** | Ve výchozím nastavení má uživatelský účet SSH stejné heslo jako účet pro přihlášení ke clusteru. Pokud chcete použít jiné heslo, zrušte zaškrtnutí políčka __Použít stejné heslo jako pro přihlášení ke clusteru__ a potom do pole __Heslo SSH__ zadejte požadované heslo.</br>![Dialogové okno Heslo SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Použijte parametr `--SshCredential` rutiny `New-AzureRmHdinsightCluster` a předejte objekt `PSCredential`, který obsahuje heslo a jméno uživatelského účtu SSH. |
| **Azure CLI 1.0** | Použijte parametr `--sshPassword` příkazu `azure hdinsight cluster create` a zadejte hodnotu hesla. |
| **Šablona Resource Manageru** | Příklad použití hesla s využitím šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí hesla SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). Element `linuxOperatingSystemProfile` v souboru [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) slouží při vytváření clusteru k předání hesla a názvu učtu SSH do Azure.|

### <a name="change-the-ssh-password"></a>Změna hesla SSH

Informace o změně hesla uživatelského účtu SSH najdete v dokumentu věnovaném [správě HDInsightu](hdinsight-administer-use-portal-linux.md#change-passwords) v části __Změna hesel__.

## <a id="domainjoined"></a>Ověřování: HDInsight připojený k doméně

Pokud používáte __cluster HDInsight připojený k doméně__, musíte po připojení pomocí protokolu SSH použít příkaz `kinit`. Tento příkaz vás vyzve k zadání hesla a uživatele domény a ověří platnost vaší relace s využitím domény služby Azure Active Directory přidružené ke clusteru.

Další informace najdete v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).

## <a name="connect-to-worker-and-zookeeper-nodes"></a>Připojení k pracovním uzlům a uzlům Zookeeper

Pracovní uzly a uzly Zookeeper nejsou přístupné přímo z internetu. Lze se k nim dostat z hlavních nebo hraničních uzlů clusteru. Toto jsou obecné kroky pro připojení k ostatním uzlům:

1. Pomocí SSH se připojte k hlavnímu nebo hraničnímu uzlu:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. V rámci připojení SSH k hlavnímu nebo hraničnímu uzlu použijte příkaz `ssh` pro připojení k pracovnímu uzlu v clusteru:

        ssh sshuser@wn0-myhdi

    Pokud chcete načíst seznam doménových názvů uzlů v clusteru, přečtěte si dokument [Správa HDInsight pomocí rozhraní Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).

Pokud je účet SSH zabezpečený __heslem__, zadejte toto heslo při připojování.

Pokud je účet SSH zabezpečený __klíči SSH__, zkontrolujte, jestli je na klientovi zapnuté přesměrování SSH.

> [!NOTE]
> Dalším možným způsobem přímého přístupu ke všem uzlům clusteru je instalace HDInsightu do služby Azure Virtual Network. Potom můžete váš vzdálený počítač připojit ke stejné virtuální síti a získat přímý přístup ke všem uzlům v clusteru.
>
> Další informace najdete v tématu [Použití virtuální sítě s HDInsightem](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>Konfigurace přesměrování agenta SSH

> [!IMPORTANT]
> V následujících krocích se předpokládá využití systému založeného na Linuxu nebo UNIXu a funkce Bash on Windows 10. Pokud tyto kroky pro váš systém nefungují, možná budete nahlédnout do dokumentace k příslušnému klientovi SSH.

1. V textovém editoru otevřete `~/.ssh/config`. Pokud tento soubor neexistuje, můžete ho vytvořit tak, že na příkazovém řádku zadáte `touch ~/.ssh/config`.

2. Do souboru `config` přidejte následující text.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Informace o __hostiteli__ nahraďte adresou uzlu, ke kterému se připojujete pomocí SSH. Předchozí příklad používá hraniční uzel. Tato položka nakonfiguruje přesměrování agenta SSH pro zadaný uzel.

3. Proveďte test přesměrování agenta SSH pomocí následujícího příkazu z terminálu:

        echo "$SSH_AUTH_SOCK"

    Tento příkaz by měl vrátit informace podobné následujícímu textu:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Když se nic nevrátí, znamená to, že nástroj `ssh-agent` není spuštěný. Další informace o spouštěcích skriptech agenta najdete v tématu [Použití nástroje SSH agent s SSH (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) nebo v dokumentaci ke klientovi SSH.

4. Jakmile si ověříte, že  se **ssh-agent** spustil, podle následujícího postupu přidejte svůj privátní klíč SSH k agentovi:

        ssh-add ~/.ssh/id_rsa

    Pokud je privátní klíč uložen v jiném souboru, nahraďte `~/.ssh/id_rsa` cestou k souboru.

5. Připojte se k hlavním uzlům nebo hraničnímu uzlu clusteru pomocí protokolu SSH. Potom se pomocí příkazu SSH k připojte k pracovnímu uzlu nebo k uzlu Zookeeper. Připojení se naváže s využitím přesměrovaného klíče.

## <a name="next-steps"></a>Další kroky

* [Použití tunelování SSH s HDInsightem](hdinsight-linux-ambari-ssh-tunnel.md)
* [Použití virtuální sítě s HDInsightem](hdinsight-extend-hadoop-virtual-network.md)
* [Použití hraničních uzlů v HDInsightu](hdinsight-apps-use-edge-node.md#access-an-edge-node)
