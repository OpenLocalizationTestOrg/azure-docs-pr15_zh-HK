<properties
   pageTitle="分析传感器数据与 Apache 风暴和 HBase |Microsoft Azure"
   description="了解如何使用虚拟网络连接到 Apache 风暴。 风暴与一起使用 HBase 过程的传感器数据从事件中心和使其可视化与 D3.js。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>分析与 Apache 风暴、 事件中心和 HBase 中 HDInsight (Hadoop) 传感器数据 

了解如何使用 Apache 风暴上 HDInsight 过程的传感器数据从 Azure 事件中心、 将其存储到 Apache HBase 上 HDInsight，和通过使用 D3.js 作为 Azure Web 应用程序运行进行可视化。

本文档中使用 Azure 资源管理器模板演示如何创建多个 Azure 资源的资源组中。 特别是，它将创建 Azure 虚拟网络、 两个 HDInsight 群集 （风暴和 HBase，） 和 Azure Web 应用程序。 实时 web 仪表盘的 node.js 实现自动部署到 web 应用程序中。

> [AZURE.NOTE] 在本文档和示例提供，信息经过使用基于 Linux 的 HDInsight 3.3 和 3.4 群集版本。

## <a name="prerequisites"></a>系统必备组件

* Azure 的订阅。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

    > [AZURE.IMPORTANT] 您不需要现有 HDInsight 群集;此文档中的步骤将创建以下资源︰
    >
    > * Azure 的虚拟网络
    > * 在 HDInsight 群集上的风暴 (基于 Linux 的 2 个辅助节点)
    > * 在 HDInsight 群集 HBase (基于 Linux 的 2 个辅助节点)
    > * Azure Web 应用程序承载 web 仪表板

* [Node.js](http://nodejs.org/)︰ 这用来预览您的开发环境在本地 web 的仪表板。

* [Java 和 JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)︰ 用于开发风暴拓扑。

* [Maven](http://maven.apache.org/what-is-maven.html)︰ 用于生成和编译该项目。

* [Git](http://git-scm.com/)︰ 用于从 GitHub 下载的项目。

* __SSH__客户端︰ 连接到基于 Linux 的 HDInsight 群集使用。 在 HDInsight 中使用 SSH 的详细信息，请参阅以下文档。

    * [使用 SSH HDInsight 从 Windows 客户端](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [使用 SSH HDInsight 从 Linux、 Unix 或 Mac 客户端](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] 您还必须具有访问`scp`命令，用于本地开发环境和使用 SSH 的 HDInsight 群集之间复制文件。

## <a name="architecture"></a>体系结构

![体系结构关系图](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

此示例由以下组件组成︰

* **Azure 事件集线器**︰ 包含来自传感器收集的数据。 在本例中，应用程序是提供程序生成数据。

* **在 HDInsight 上的风暴**︰ 从事件中心提供的数据的实时处理。

* **在 HDInsight 上的 HBase**︰ 风暴处理后的数据提供持久 NoSQL 数据存储区。

* **Azure 虚拟网络服务**︰ 使 HDInsight 在风暴和 HBase HDInsight 群集之间的安全通信。

    > [AZURE.NOTE] 虚拟网络是需要使用 Java HBase 客户端 API，不会公开对 HBase 群集公用网关。 安装到相同的虚拟网络的 HBase 和风暴群集允许风暴群集 （或虚拟网络上的任何其他系统） 来直接访问 HBase 使用客户端 API。

* **仪表板网站**︰ 图表中实时数据的示例仪表板。

    * 该网站采用 Node.js，因此它可以运行在任何客户端的操作系统进行测试，或者可以将它部署到 Azure 网站。

    * [Socket.io](http://socket.io/)用于风暴拓扑和网站之间的实时通信。

        > [AZURE.NOTE] 这是实现详细信息。 您可以使用任何通讯框架，如原始 Websocket 或那么 SignalR。

    * [D3.js](http://d3js.org/)用于图表的数据发送到该网站。

> [AZURE.IMPORTANT] 两个群集，则需要不受支持的方法来创建一个 HDInsight 群集风暴和 HBase 原样。

拓扑数据读取事件中心通过使用[org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html)类，并将数据写入到使用[org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html)类 HBase。 与网站进行通信可以通过使用[socket.io client.java](https://github.com/nkzawa/socket.io-client.java)。

以下是该拓扑图。

![拓扑图](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] 这是非常简化的拓扑视图。 在运行时，每个组件的实例创建为每个分区为正在读取事件中心。 这些实例都分布在群集中的节点和数据路由它们之间，如下所示︰
>
> * 将数据从管口到分析器进行负载平衡。
> * 从解析器的仪表板和 HBase 数据分为设备 ID，以便邮件从同一台设备总是流向相同的组件。

### <a name="topology-components"></a>拓扑结构组件

* **EventHub 管口**︰ 作为管口是提供一部分 Apache 风暴版本 0.10.0 或更高。

    > [AZURE.NOTE] 在此示例中使用事件集线器管口要求 HDInsight 群集版本 3.3 或 3.4 风暴。 如何使用旧版本的 HDInsight 在风暴中使用事件集线器的信息，请参阅[处理事件从 HDInsight 上的风暴与 Azure 事件集线器](hdinsight-storm-develop-java-event-hub-topology.md)。

* **ParserBolt.java**︰ 发射管口的数据是原始 JSON，而有时多个事件发送一次。 该螺栓演示如何读取数据发射的管口，并发出它新流作为包含多个字段组成的元组。

* **DashboardBolt.java**︰ 此示例演示如何使用 Java 的 Socket.io 客户端库在发送数据实时访问 web 的仪表板。

此示例使用了[通量](https://storm.apache.org/releases/0.10.0/flux.html)框架，所以 YAML 文件中包含的拓扑定义。 有两个︰

* __否-hbase.yaml__ -使用此文件在您的开发环境中测试拓扑时。 它不使用 HBase 组件，因为您不能访问来自网络外部的虚拟群集居住在 HBase Java API。

* __与 hbase.yaml__ -使用此文件时对冲击群集部署拓扑。 它使用 HBase 组件，因为它在 HBase 群集所在的虚拟网络中运行。

## <a name="prepare-your-environment"></a>准备您的环境

在您使用本示例之前，必须创建风暴拓扑读取从 Azure 事件集线器。

### <a name="configure-event-hub"></a>配置事件中心

事件的中心是此示例的数据源。 使用以下步骤创建新的事件中心。

1. 从[Azure 的门户网站](https://portal.azure.com)，选择**新建 +** -> __互联网的东西__ -> __事件集线器__。

2. 在__创建 Namespace__刀片式服务器，请执行以下任务︰

    1. 输入命名空间__名称__。
    2. 选择定价层。 __基本__足以满足本示例。
    3. 选择 Azure__订阅__以使用。
    4. 请选择一个现有的资源组或创建一个新。
    5. 选择事件中心__的位置__。
    6. 选择__附到仪表板__，然后再单击__创建__。

3. 创建过程完成后，将显示您的名称空间的事件集线器刀片。 在此处选择__+ 添加事件中心__。 在__创建事件中心__刀片式服务器，输入__sensordata__的名称，然后选择__创建__。 将其他字段的默认值。

4. 从事件集线器刀片式服务器为您的命名空间，请选择__事件集线器__。 选择__sensordata__条目。

5. 从 sensordata 事件中心的刀片式服务器，选择__共享访问策略__。 使用__+ 添加__链接以添加以下策略︰


  	| 策略名称 | 索赔 |
  	| ----- | ----- |
  	| 设备 | 发送 |
  	| 风暴 | 侦听 |

5. 选择这两种策略和__主关键字__值的记录。 在以后的步骤这两个策略，您将需要值。

## <a name="download-and-configure-the-project"></a>下载并配置项目

使用以下方法来从 GitHub 下载的项目。

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

在命令完成后，您将具有以下目录结构︰

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] 本文档不会不转到详细的代码包含在本示例中;但是，完全被注释代码。

打开**hdinsight-eventhub-example/TemperatureMonitor/dev.properties**文件，并添加下面几行事件中心信息︰

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] 本示例假定有__倾听__的索赔，该策略的名称，使用__风暴__和事件中心被命名为__sensordata__。

 添加此信息后，请保存该文件。

## <a name="compile-and-test-locally"></a>编译和测试本地

在测试之前必须启动仪表板查看输出的拓扑结构并生成事件中心中存储的数据。

> [AZURE.IMPORTANT] 在本地，测试作为 Java API HBase 群集不能从外部进行访问包含群集的 Azure 虚拟网络时，此拓扑的 HBase 组件处于非活动。

### <a name="start-the-web-application"></a>启动 web 应用程序

1. 打开新的命令提示符或终端，并将目录更改为**hdinsight-eventhub-示例/仪表板**，然后使用下面的命令来安装所需的 web 应用程序的依赖项︰

        npm install

2. 使用下面的命令以启动 web 应用程序︰

        node server.js

    您应该看到类似于以下的消息︰

        Server listening at port 3000

2. 打开 web 浏览器并输入**http://localhost:3000 /**的地址。 您应该看到类似以下的页︰

    ![web 的仪表板](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    保持此命令提示符或终端打开。 测试完成后，使用 CTRL-C 停止 web 服务器。

### <a name="start-generating-data"></a>开始生成数据

> [AZURE.NOTE] 本节中的步骤使用 Node.js，以便可以在任何平台上使用它们。 其他语言的示例，请参见**SendEvents**目录。

1. 打开新的命令提示符、 外壳或终端，并将目录更改为**hdinsight-eventhub-示例/SendEvents/nodejs**，然后使用下面的命令来安装所需的应用程序的依赖项︰

        npm install

2. 在文本编辑器中打开**app.js**文件，并添加之前获得事件中心信息︰

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] 本示例假定，您已经使用__sensordata__作为事件中心和__设备__名称为已__发送__索赔的策略的名称。

2. 使用以下命令在事件集线器插入新项︰

        node app.js

    您应该看到几个输出行包含发送到事件中心的数据。 这些将类似于以下内容︰

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>开始了拓扑

2. 打开新的命令提示符、 外壳或为__hdinsight-eventhub-示例/TemperatureMonitor__，终端和更改目录，然后使用下面的命令启动拓扑︰

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    如果您正在使用 PowerShell，改用下列︰

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] 如果您是在 Linux/Unix/OS X 系统上，并已[安装在您的开发环境中的冲击](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)，可以改用下面的命令︰
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    这将启动在本地模式下的__无 hbase.yaml__文件中定义的拓扑。 在__dev.properties__文件中包含的值提供事件集线器的连接信息。 一旦启动，拓扑读取事件中心的条目并将它们发送到您的本地计算机上运行的仪表板。 您应该看到类似于以下 web 面板中显示的行︰

    ![仪表板的数据](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. 运行仪表板时，使用`node app.js`命令从前面的步骤将新数据发送到事件的集线器。 由于温度值随机生成的应更新关系图以显示温度存在较大差异。

    > [AZURE.NOTE] 您必须在__hdinsight-eventhub-示例/SendEvents/Nodejs__目录中时使用`node app.js`命令。

3. 在验证这工作后，停止使用 Ctrl + C 的拓扑。 可以使用 Ctrl + C 也停止本地 web 服务器。

## <a name="create-a-storm-and-hbase-cluster"></a>创建风暴和 HBase 群集

为了运行 HDInsight，拓扑结构并启用 HBase 螺栓，必须创建新的风暴簇和 HBase 群集。 本节中的步骤使用[Azure 资源管理器模板](../resource-group-template-deploy.md)来创建新 Azure 虚拟网络的冲击和 HBase 群集上的虚拟网络。 该模板还创建 Azure Web 应用程序，并部署到它的复制仪表板。

> [AZURE.NOTE] 虚拟网络使用了风暴群集上运行的拓扑可以直接与 HBase 群集使用 HBase Java API 进行通信。

本文档中使用的资源管理器模板位于位于__https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__的公钥 blob 容器中。

1. 请单击下面的按钮，登录到 Azure 和 Azure 门户中打开资源管理器的模板。

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. 从**参数**刀片式服务器，输入以下命令︰

    ![HDInsight 参数](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**︰ 此值将用作风暴的基名称和 HBase 群集。 例如，输入__hdi__将创建风暴群集命名__风暴 hdi__和命名__hbase hdi__HBase 群集。
    * __CLUSTERLOGINUSERNAME__︰ 风暴和 HBase 群集的管理员用户名。
    * __CLUSTERLOGINPASSWORD__︰ 风暴和 HBase 群集管理员用户密码。
    * __SSHUSERNAME__: SSH 用户创建的风暴和 HBase 群集。
    * __SSHPASSWORD__︰ 风暴和 HBase 群集的 SSH 用户的密码。
    * __位置__︰ 群集将在中创建的地区。
    
    单击__确定__以保存参数。
    
3. 使用__资源组__部分中创建新的资源组，或选择一个现有。

4. 在__资源组位置__下拉菜单中，为所选的__位置__参数中选择相同的位置。

5. 选择__法律条款__，，然后选择__创建__。

6. 最后，检查__附到仪表板__，然后选择__创建__。 它大约需要 20 分钟来创建群集。

一旦资源都已经创建，您将重定向到包含群集和仪表板 web 资源组刀片。

![Vnet 和群集资源组刀片](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] 请注意，HDInsight 群集的名称是__风暴 BASENAME__和__hbase 基名称__，其中基名称为您提供对该模板的名称。 连接到群集时，您将在后续步骤中使用这些名称。 此外请注意仪表板站点的名称，是__basename 仪表板__。 查看仪表板时，您将从更高版本使用此。

## <a name="configure-the-dashboard-bolt"></a>配置仪表板螺栓

为了将数据发送到仪表板部署为 web 应用程序，您必须修改__dev.properties__文件中的以下行︰

    dashboard.uri: http://localhost:3000

更改`http://localhost:3000`到`http://BASENAME-dashboard.azurewebsites.net`，然后保存文件。 __基名称__替换您在上一步中提供的基名称。 您还可以使用以前创建选择仪表板并查看 URL 的资源组。

## <a name="create-the-hbase-table"></a>创建 HBase 表

为了在 HBase 中存储的数据，我们必须首先创建一个表。 通常需要预先创建风暴需要写入的资源，为想要创建从风暴拓扑内的资源可能会导致代码尝试创建同一资源的多个分布式副本。 创建拓扑结构以外的资源，只是用于读取/写入和分析的风暴。

1. 使用 SSH 连接到 HBase 群集使用 SSH 用户和密码在群集创建过程中提供的模板。 例如，如果使用连接`ssh`命令，您可以使用下面的语法︰

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    在此命令中，替换您提供与您提供的基名称创建的群集中和__基名称__时的 SSH 用户名称__用户名__。 出现提示时，输入 SSH 用户的密码。

2. 从 SSH 会话，开始 HBase 外壳。

        hbase shell
    
    外壳程序已加载后，您将看到`hbase(main):001:0>`提示。

3. 从 HBase 外壳程序，输入以下命令以创建一个表以将传感器数据存储︰

        create 'SensorData', 'cf'

4. 验证已通过使用下面的命令创建了表︰

        scan 'SensorData'
        
    这返回类似于以下示例中，指示在表中存在 0 行的信息。
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. 输入以下退出 HBase shell 命令︰

        exit

## <a name="configure-the-hbase-bolt"></a>配置 HBase 螺栓

要写入 HBase 风暴群集，您必须提供 HBase 螺栓使用 HBase 群集的配置详细信息。 若要执行此操作的最简单方法是下载__hbase site.xml__从群集并将其纳入您的项目。 您还必须取消注释负载冲击 hbase 组件和所需的相关性依赖在__pom.xml__文件中，若干项。

> [AZURE.IMPORTANT] 您还必须下载风暴 hbase.jar 文件提供了有关在 HDInsight 群集 3.3 或 3.4 群集; 您风暴本版编译使用 HBase 1.1.x，用于 HBase HDInsight 3.3 和 3.4 群集上。 如果从其他地方使用风暴 hbase 组件，它可能被编译 HBase 的较旧版本。

### <a name="download-the-hbase-sitexml"></a>下载 hbase site.xml

从命令提示符下使用 SCP __hbase site.xml__文件下载从群集。 在下面的示例中，替换为您提供与您在上面提供的基名称创建的群集中和__基名称__时的 SSH 用户__用户名__。 出现提示时，输入 SSH 用户的密码。 更换`/path/to/TemperatureMonitor/resources/hbase-site.xml`TemperatureMonitor 项目中的此文件的路径。

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

这将下载__hbase site.xml__指定的路径。

### <a name="download-and-install-the-storm-hbase-component"></a>下载并安装该风暴 hbase 组件

1. 从命令提示符下使用 SCP__风暴 hbase.jar__文件下载来自风暴群集。 在下面的示例中，替换为您提供与您在上面提供的基名称创建的群集中和__基名称__时的 SSH 用户__用户名__。 出现提示时，输入 SSH 用户的密码。

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    这将下载文件命名为`storm-hbase-####.jar`，其中 # # # 是该群集的版本号的风暴。 记下此数字，因为以后用它。

2. 使用下面的命令将此组件安装到本地 Maven 存储库在您的开发环境。 这样，Maven 查找包时编译该项目。 更换__####__文件的文件名中包含版本号。

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    如果您正在使用 PowerShell，使用下面的命令︰

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>启用项目中的暴风雨 hbase 组件

1. 打开__TemperatureMonitor/pom.xml__文件，并删除以下行︰

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] 只能删除这两行;不要删除任何它们之间的界限。
    
    这样，几个与 HBase 使用 hbase 螺栓进行通信时所需的组件。

2. 找到下列代码行，然后将替换__####__与前面下载的冲击 hbase 文件的版本号。

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] 版本号必须与您使用时将组件安装到本地的 Maven 存储库中，Maven 使用此信息来生成项目时加载组件的版本相匹配。

2. 保存__pom.xml__文件。

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>生成包，并将解决方案部署到 HDInsight

在开发环境中，使用以下步骤对风暴群集部署风暴拓扑。

1. 从__TemperatureMonitor__目录中，使用下面的命令来执行新的生成并从您的项目创建的 JAR 包︰

        mvn clean compile package

    这将创建名为**TemperatureMonitor-1.0-SNAPSHOT.jar** ，项目的**目标**目录中的文件。

2. 使用 scp 的__TemperatureMonitor-1.0-SNAPSHOT.jar__文件上载到风暴群集。 在下面的示例中，替换为您提供与您在上面提供的基名称创建的群集中和__基名称__时的 SSH 用户__用户名__。 出现提示时，输入 SSH 用户的密码。

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] 可能需要几分钟时间来上载文件，因为它将几兆字节大小。

    使用 scp 要上载的__dev.properties__文件，其中包含用来连接到事件集线器和仪表板的信息。

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. 一旦已上载文件，连接到群集使用 SSH。

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. 从 SSH 会话，请使用下面的命令启动拓扑结构。

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    这将启动在__dev.properties__文件中使用__的 hbase.yaml__文件中的拓扑定义和配置值的拓扑。

3. 启动了拓扑后，打开一个浏览器到 Azure 发布的网站，然后使用`node app.js`命令将数据发送到事件中心。 您会看到 web 仪表板更新显示的信息。

    ![仪表板](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>查看 HBase 数据

在提交数据到拓扑使用之后`node app.js`，使用以下步骤连接到 HBase 并验证数据已写入您在前面创建的表。

1. 使用 SSH 连接到 HBase 群集。

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. 从 SSH 会话，开始 HBase 外壳。

        hbase shell
    
    外壳程序已加载后，您将看到`hbase(main):001:0>`提示。

2. 查看表中的行︰

        scan 'SensorData'
        
    这将返回类似于以下内容，信息，指示在表中是否存在 0 行。
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] 该扫描操作从表中将只返回最大为 10 行。

## <a name="delete-your-clusters"></a>删除群集

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

若要一次删除群集、 存储和 web 应用程序，删除包含它们的资源组。

## <a name="next-steps"></a>下一步行动

现在，您已了解如何使用风暴事件集线器从读取数据、 将其存储到 HBase，和通过使用 Socket.io 和 D3.js 可视化外部的仪表板上的信息。

* 与 HDInsight 风暴拓扑结构的更多示例，请参阅︰

    * [在 HDInsight 上的风暴的示例拓扑](hdinsight-storm-example-topology.md)

* Apache 风暴有关的详细信息，请参阅[Apache 风暴](https://storm.incubator.apache.org/)网站。

* HDInsight 在 HBase 的详细信息，请参阅[HDInsight 概述与 HBase](hdinsight-hbase-overview.md)。

* 有关 Socket.io 的详细信息，请参阅[socket.io](http://socket.io/)站点。

* 有关 D3.js 的详细信息，请参阅[D3.js-数据驱动的文档](http://d3js.org/)。

* 在 Java 中创建拓扑信息，请参见[开发 Java 在 HDInsight 上的 Apache 风暴的拓扑](hdinsight-storm-develop-java-topology.md)。

* 在.NET 中创建拓扑信息，请参见[开发 C# HDInsight 使用 Visual Studio 在 Apache 风暴的拓扑](hdinsight-storm-develop-csharp-visual-studio-topology.md)。

[azure-portal]: https://portal.azure.com
