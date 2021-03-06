---
title: Použití Caffe pro distribuované obsáhlého learningu v Azure HDInsight Spark
description: Použití Caffe pro distribuované obsáhlého learningu v Azure HDInsight Spark
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/17/2017
ms.openlocfilehash: a7873996d83dbc79b4d44c58bd964c274f9c7709
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622911"
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Použití Caffe pro distribuované obsáhlého learningu v Azure HDInsight Spark


## <a name="introduction"></a>Úvod

Obsáhlý learning se dopadu na všechno od zdravotní péče na transportation do výroby a další. Podniky přecházejí ke hloubkového učení řeší těžké problémy, jako je třeba [klasifikace obrázků](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [rozpoznávání řeči](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objektu rozpoznávání a počítač překladu. 

Existují [mnoho oblíbených architektur](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), včetně [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, atd. Caffe je jedno z rozhraní nejslavnějšího-symbolické (imperativní) neuronové sítě a běžně používá v mnoha oblastech, včetně pro počítačové zpracování obrazu. Kromě toho [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinuje Caffe s Apache Sparkem v takovém případě obsáhlý learning můžete snadno použít v existujícím clusteru Hadoop. Obsáhlý learning spolu s kanály Spark ETL, snížení složitosti systému a latence můžete použít pro učení tak získají kompletní řešení.

[HDInsight](https://azure.microsoft.com/services/hdinsight/) je nabídka Hadoopu v cloudu, který poskytuje optimalizované opensourcové analytické clustery pro Spark, Hive, Hadoop, HBase, Storm, Kafka a služby ML. HDInsight je zajištěná smlouvou SLA 99,9 %. Každá z těchto technologií pro velké objemy dat a aplikace nezávislých výrobců softwaru je jednoduše nasadit jako spravovaný cluster se zabezpečením a monitorováním pro podniky.

Tento článek ukazuje, jak nainstalovat [Caffe ve Sparku](https://github.com/yahoo/CaffeOnSpark) pro HDInsight cluster. Tento článek také používá integrované ukázka mnist ručně k věnovaných ukázce používání distribuovaných hloubkového učení pomocí Sparku v HDInsight v procesorech.

Existují čtyři kroky ke splnění úkolu:

1. Nainstalujte požadované závislosti na všechny uzly
2. Vytvoření hlavního uzlu Caffe ve Sparku pro HDInsight
3. Distribuovat požadované knihovny do všech pracovních uzlů
4. Vytvoření modelu Caffe a spustíte ji v distribuovanému.

HDInsight je řešení PaaS, nabízí skvělou platformou funkce – tak, aby byl snadno provádět některé úlohy. Jedna funkce použité v tomto blogovém příspěvku se nazývá [akce skriptu](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), pomocí kterého můžete spustit příkazy prostředí přizpůsobit uzly clusteru (hlavního uzlu, pracovním uzlu nebo hraničnímu uzlu).

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>Krok 1: Instalace požadované závislosti pro všechny uzly v

Abyste mohli začít, musíte nainstalovat závislosti. Lokality Caffe a [CaffeOnSpark lokality](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) nabízí některé užitečné wiki pro instalaci závislosti na režimu YARN pro Spark. HDInsight také využívá Spark v režimu YARN. Musíte však přidat několik Další závislosti na platformě HDInsight. Uděláte to tak, použijte akci skriptu a spusťte ho na všechny hlavní uzly a uzly pracovního procesu. Tuto akci se skripty trvá přibližně 20 minut, jak tyto závislosti také závisí na jiné balíčky. Ji by měl umístit do umístění, které je dostupné pro váš cluster HDInsight, jako je například umístění Githubu nebo výchozí účet úložiště objektů BLOB.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose you install them on all the nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


Existují dva kroky v akci skriptu. Prvním krokem je instalace potřebných knihoven. Tyto knihovny zahrnují potřebné knihovny pro kompilaci Caffe (například gflags, glog) i s Caffe (například numpy). Používáte libatlas optimalizace využití procesoru, ale vždy provedením CaffeOnSpark wikiwebu o instalaci jiných optimalizace knihoven, jako je například MKL nebo CUDA (pro GPU).

Druhým krokem je ke stažení, kompilaci a instalace protobuf 2.5.0 Caffe za běhu. Protobuf 2.5.0 [vyžádáním](https://github.com/yahoo/CaffeOnSpark/issues/87), ale tato verze není k dispozici jako balíček na Ubuntu 16, takže je musíte je zkompilovat ze zdrojového kódu. Existují také několik prostředků na Internetu o tom, jak jej zkompilovat. Další informace najdete v tématu [tady](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html).

Abyste mohli začít, lze pouze spustit tuto akci se skripty u vašeho clusteru na všechny uzly pracovního procesu a hlavní uzly (pro HDInsight 3.5). Můžete spustit skript akce v existujícím clusteru, nebo při vytváření clusteru pomocí skriptových akcí. Další informace o akcí skriptů najdete v dokumentaci [tady](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions).

![Skript akce instalace závislostí](./media/apache-spark-deep-learning-caffe/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a>Krok 2: Vytvoření Caffe ve Sparku pro HDInsight hlavního uzlu

Druhým krokem je vytvoření Caffe v hlavního uzlu a pak distribuovat kompilovaných knihoven do všech pracovních uzlů. V tomto kroku je nutné [ssh do vašeho hlavního uzlu](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix). Poté je třeba dodržovat [proces sestavení CaffeOnSpark](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn). Níže je skript, který můžete použít k sestavení CaffeOnSpark v několika dalších krocích. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Budete muset provést více než co dokumentace CaffeOnSpark říká. Změny jsou tyto:
- Změnit pouze na CPU a použít libatlas pro tento konkrétní účel.
- Umístěte datové sady do úložiště objektů BLOB, který je sdílené umístění, která je přístupná pro všechny uzly pracovního procesu pro pozdější použití.
- Vložit kompilovaných knihoven Caffe do úložiště objektů BLOB a později zkopírujete těchto knihoven na všech uzlech, pomocí akce skriptu, aby čas další kompilace.


### <a name="troubleshooting-an-ant-buildexception-has-occurred-exec-returned-2"></a>Řešení potíží: Ant BuildException došlo k chybě: exec vrátí: 2

Při prvním pokusu o sestavení CaffeOnSpark, někdy stavu

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Vyčistit úložiště kódu pomocí "provést čisté" a spuštění "Zkontrolujte sestavení" Tento problém odstranit, dokud máte správnými závislostmi.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Řešení potíží: Maven úložiště časový limit připojení

Někdy maven poskytuje k připojení vypršení časového limitu, podobně jako následující fragment kódu:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Za pár minut, zkuste to znovu.


### <a name="troubleshooting-test-failure-for-caffe"></a>Řešení potíží: Test selhání pro Caffe

Pravděpodobně uvidíte při poslední kontrole CaffeOnSpark selhání testu. To je pravděpodobně týkající se kódování UTF-8, ale by neměla mít vliv na použití Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>Krok 3: Distribuujte požadované knihovny do všech pracovních uzlů

Dalším krokem je pro distribuci knihoven (v podstatě knihovny v CaffeOnSpark/caffe – veřejné/distribuovat/lib/a CaffeOnSpark/caffe distribuční/rozdělit/lib /) na všech uzlech. V kroku 2 vložte tyto knihovny v úložišti objektů BLOB a v tomto kroku pomocí akcí skriptů a zkopírujte ho do hlavní uzly a uzly pracovního procesu.

K tomu, spustíte akci skriptu, jak je znázorněno v následujícím fragmentu kódu:

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Ujistěte se, že potřebujete, přejděte na správném místě konkrétní cluster)

Vzhledem k tomu, že v kroku 2, vložíte ho v úložišti objektů BLOB, která je přístupná pro všechny uzly, v tomto kroku stačí ho zkopírovat do všech uzlů.

## <a name="step-4-compose-a-caffe-model-and-run-it-in-a-distributed-manner"></a>Krok 4: Vytvoření modelu Caffe a spustíte ji v distribuovanému

Caffe je instalovat až po dokončení spuštění v předchozích krocích. Dalším krokem je napsat modelu Caffe. 

Caffe používá "výrazové architekturu", kde pro vytváření modelu, stačí definovat konfiguračního souboru, a bez psaní kódu vůbec (ve většině případů). Pojďme se podívat existuje. 

Model, který tréninku je ukázkový model pro trénování mnist ručně. Databázi mnist ručně zapsaných číslic má sadu 60 000 příklady trénovací a testovací sadu 10 000 příklady. Je podmnožinou s větším NIST k dispozici. Číslice se velikost normalizovaných a uprostřed image s pevnou velikostí. CaffeOnSpark má některé skripty stáhnout datovou sadu a převést na správný formát.

CaffeOnSpark uvádí některé ukázkové topologie sítě pro trénování mnist ručně. Má dobrý návrh rozdělení síťovou architekturu (topologie sítě) a optimalizace. V takovém případě se vyžaduje dva soubory: 

soubor "Solver" (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) se používá pro dohled optimalizace a generování parametrů aktualizace. Například definuje, jestli CPU nebo GPU se používá, co je podpora, kolik iterací jsou, atd. Definuje také topologie sítě které neuron program používejte (což je druhý soubor, který potřebujete). Další informace o řešení, najdete v části [Caffe dokumentaci](http://caffe.berkeleyvision.org/tutorial/solver.html).

V tomto příkladu vzhledem k tomu, že používáte procesoru spíše než GPU, by měl změnit na poslední řádek:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Konfigurace Caffe](./media/apache-spark-deep-learning-caffe/Caffe-1.png)

Podle potřeby můžete změnit další řádky.

Druhý soubor (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definuje, jak síť neuron vypadá a relevantní vstupní a výstupní soubor. také musíte aktualizovat soubor tak, aby odrážely umístění dat školení. Změňte následující součásti na lenet_memory_train_test.prototxt (potřebujete tak, aby odkazovala na správné umístění, které jsou specifické pro váš cluster):

- Změňte "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" na "wasb: / / / projektů/machine_learning/image_dataset/mnist_train_lmdb"
- Změňte "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" na "wasb: / / / projektů/machine_learning/image_dataset/mnist_test_lmdb"

![Konfigurace Caffe](./media/apache-spark-deep-learning-caffe/Caffe-2.png)

Další informace o tom, jak definovat v síti, zkontrolujte, [Caffe dokumentaci na datové sadě mnist ručně](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Pro účely tohoto článku můžete použít tento příklad mnist ručně. Spuštěním následujících příkazů z hlavního uzlu:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

Ve výstupu předchozího příkazu distribuuje požadované soubory (lenet_memory_solver.prototxt a lenet_memory_train_test.prototxt) pro každý kontejner YARN. Příkaz také nastaví příslušné cesty každý ovladač Spark/vykonavatele LD_LIBRARY_PATH. LD_LIBRARY_PATH je definována v předchozím fragmentu kódu a odkazuje na umístění, které obsahuje CaffeOnSpark knihovny. 

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží

Vzhledem k tomu, že používáte režim clusteru YARN, v takovém případě ovladač Spark přehrávání bude naplánován libovolného kontejneru (a libovolný pracovní uzel) byste měli vidět jenom v konzole něco jako výstup:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Pokud chcete vědět, co se stalo, obvykle potřebujete získat ovladače protokolu, který obsahuje další informace o Sparku. V takovém případě budete muset přejít do uživatelského rozhraní YARN najít relevantní protokoly YARN. Můžete využít uživatelské rozhraní YARN pomocí této adresy URL: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![UŽIVATELSKÉ ROZHRANÍ YARN](./media/apache-spark-deep-learning-caffe/YARN-UI-1.png)

Podívejte se na tom, kolik prostředky se přidělují pro tuto konkrétní aplikaci můžete využít. Můžete kliknout na odkaz "Plánovač" a uvidíte, že pro tuto aplikaci neexistují devět kterých kontejnerech je spuštěná. Požádejte YARN poskytnout osm prováděcí moduly a jiný kontejner pro proces ovladačů. 

![Plánovač YARN](./media/apache-spark-deep-learning-caffe/YARN-Scheduler.png)

Můžete chtít zkontrolujte ovladač protokolů nebo protokoly kontejneru, zda nedochází k chybám. Pro protokoly ovladačů můžete kliknutím na ID aplikace v uživatelském rozhraní YARN a pak klikněte na tlačítko "Protokoly". Protokoly ovladačů se zapisují do stderr.

![UŽIVATELSKÉ ROZHRANÍ YARN 2](./media/apache-spark-deep-learning-caffe/YARN-UI-2.png)

Například může se zobrazit některé chyby pod z protokolů ovladač oznamující, že přidělíte příliš mnoho prováděcí moduly.

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

V některých případech problému může dojít ve prováděcí moduly spíše než ovladače. V takovém případě musíte zkontrolovat protokoly kontejneru. Můžete vždy získat protokoly kontejneru a pak získat kontejneru se nezdařilo. Například může splňovat toto selhání při spouštění Caffe.

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

V takovém případě budete muset získat ID neúspěšné kontejner (v případě výše je container_1485916338528_0008_05_000005). Pak budete muset spustit 

    yarn logs -containerId container_1485916338528_0008_03_000005

z hlavního uzlu. Po zkontrolování chyby kontejneru, je způsobena režimu GPU (kde používejte režimu procesoru místo) v lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Získání výsledků

Protože jsou přidělování 8 prováděcí moduly a topologie sítě je jednoduché, má pouze trvat přibližně 30 minut výsledky spuštění. Z příkazového řádku můžete zobrazit nechte modelu wasb:///mnist.model a vložení výsledků do složky s názvem wasb: / / / / / mnist_features_result.

Výsledky můžete získat spuštěním

    hadoop fs -cat hdfs:///mnist_features_result/*

a výsledek bude vypadat takto:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

SampleID představuje ID v datové sadě mnist ručně a popisek je číslo identifikující modelu.


## <a name="conclusion"></a>Závěr

V této dokumentaci jste se pokusili nainstalovat CaffeOnSpark se spouštěním jednoduchý příklad. HDInsight je plně spravované cloudovou distribuované výpočetní platforma a je nejlepším místem pro spouštění úlohy pokročilých analýz a strojového učení na velkých sadách dat a pro distribuované obsáhlého learningu, může použití Caffe pro HDInsight Spark k provádění hloubkové učení úlohy.


## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](apache-spark-resource-manager.md)

