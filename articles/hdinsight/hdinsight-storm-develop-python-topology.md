<properties
   pageTitle="在 HDinsight 上的风暴拓扑中使用 Python 组件 |Microsoft Azure"
   description="了解如何使用 Python 组件与 Apache Azure HDInsight 上的冲击。 您将学习如何使用 Python 组件中的基础，这两种 Java，Clojure 基于风暴拓扑。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>开发在 HDInsight 上使用 Python 的 Apache 风暴拓扑

Apache 风暴支持多种语言，甚至使您可以组合在一个拓扑中的多个语言版本的组件。 在本文中，您将学习如何在您的 Java 和基于 Clojure 的风暴拓扑中在 HDInsight 上使用 Python 组件。

##<a name="prerequisites"></a>系统必备组件

* Python 2.7 或更高版本

* Java JDK 1.7 或更高版本

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>冲击多语言支持

风暴被设计用于编写使用任何编程语言，但是这需要组件了解如何处理与[风暴的储蓄定义](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)的组件。 对于 Python，作为使您可以轻松地与风暴 Apache 风暴项目的一部分提供了一个模块。 您可以在[https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py)中找到此模块。

因为 Apache 风暴是运行在 Java 虚拟机 (JVM) 的 Java 进程，用其他语言编写的组件是作为子流程执行。 在 JVM 中运行的风暴位与使用 JSON 消息通过标准输入/标准输出发送这些子进程进行通信。 [多语言协议](https://storm.apache.org/documentation/Multilang-protocol.html)文档中找不到更多详细信息组件之间的通信。

###<a name="the-storm-module"></a>风暴模块

冲击模块 (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py，) 提供了创建工作与风暴的 Python 组件所需的位。

这提供了一些操作，例如`storm.emit`发出元，和`storm.logInfo`向日志中写入。 我建议您来覆盖此文件阅读和理解它的提供。

##<a name="challenges"></a>面临的挑战

使用__storm.py__模块，您可以创建使用数据和处理数据的螺栓，但仍使用 Java 或 Clojure 编写组件之间的通信连线的整体冲击拓扑定义的 Python spouts。 此外，如果您使用 Java，您还必须创建充当 Python 组件的接口的 Java 组件。

此外，由于风暴群集运行以分散的方式，必须确保 Python 组件需要的任何模块是群集中的所有辅助节点上可用。 风暴没有提供任何简便的方法来实现此目的的多语言资源-您必须包括所有依赖关系的 jar 文件的拓扑，或群集中的每个工作节点上手动安装依赖项。

###<a name="java-vs-clojure-topology-definition"></a>Java 与 Clojure 拓扑定义

定义一个拓扑结构的两种方法，Clojure 是迄今为止最简单/彻底可以直接引用拓扑定义中的 python 组件。 对于基于 Java 的拓扑定义，您还必须定义处理某些操作，如声明中元组的字段返回从 Python 组件的 Java 组件。

在此文档中，示例项目并介绍了这两种方法。

##<a name="python-components-with-a-java-topology"></a>Python 组件具有 Java 拓扑

> [AZURE.NOTE] 本示例位于__JavaTopology__目录中的[https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) 。 这是基于 Maven 项目。 如果您熟悉 Maven，请参阅创建用于风暴拓扑的 Maven 项目的详细信息[Apache HDInsight 上的风暴与基于 Java 开发的拓扑](hdinsight-storm-develop-java-topology.md)。

基于 Java 的拓扑结构，使用 Python （或其他 JVM 语言组件） 最初显示使用 Java 组件;但是，如果您查看每个 Java spouts/螺栓中，您将看到类似于下列的代码︰

    public SplitBolt() {
        super("python", "countbolt.py");
    }

这是在 Java 调用 Python 和运行脚本包含实际的螺栓逻辑。 （例如，） Java spouts/螺栓只声明中将发射基本的 Python 组件的元组的字段。

实际的 Python 文件存储在`/multilang/resources`在此示例中的目录。 `/multilang`在__pom.xml__中引用目录︰

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

这包括在所有文件`/multilang`文件夹将从该项目中生成 jar 中的。

> [AZURE.IMPORTANT] 请注意，这仅指定`/multilang`目录并不`/multilang/resources`。 风暴预计在非 JVM 资源`resources`目录下，因此它已经内部寻找。 将组件放在此文件夹中允许只是参考，通过在 Java 代码中的名称。 例如， `super("python", "countbolt.py");`。 另一种思考它的方法就是风暴所见`resources`为根 （/） 时访问多语言资源目录。
>
> 此示例项目，`storm.py`模块包含在`/multilang/resources`目录。

###<a name="build-and-run-the-project"></a>生成和运行项目

以在本地运行此项目，只需使用下面的 Maven 命令生成并在本地模式下运行︰

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

使用 ctrl + c 来终止进程。

若要将项目部署到运行 Apache 风暴 HDInsight 群集，使用以下步骤︰

1. 建立 uber jar:

        mvn package

    这将创建一个同名文档中的__字数统计-1.0-SNAPSHOT.jar__ `/target`为此项目目录。

2. 将 jar 文件上载到 Hadoop 群集使用下列方法之一︰

    * 对于__基于 Linux 的__HDInsight 群集︰ 使用`scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar`将 jar 文件复制到该群集，使用 SSH 用户名称和群集名称用户名替换 HDInsight 群集名称。

        一旦文件完成上载后，连接到群集使用 SSH 和开始使用拓扑`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * 对于__基于 Windows 的__HDInsight 群集︰ 通过在浏览器转到 HTTPS://CLUSTERNAME.azurehdinsight.net/ 连接到风暴的仪表板。 群集名称替换您的 HDInsight 群集名称和提供的管理员用户名和密码提示时。

        使用窗体，请执行以下操作︰

        * __Jar 文件__︰ 选择__浏览__，然后选择__字数统计-1.0-SNAPSHOT.jar__文件
        * __类名称__︰ 输入`com.microsoft.example.WordCount`
        * __附加参数__︰ 输入的友好名称，例如`wordcount`来识别拓扑

        最后，选择__提交__开始拓扑结构。

> [AZURE.NOTE] 启动后，风暴拓扑运行直到停止 （删除）。若要停止拓扑结构，使用两种`storm kill TOPOLOGYNAME`命令从命令行 （SSH 会话到 Linux 群集等） 或使用风暴用户界面，选择拓扑，，然后选择__取消__按钮。

##<a name="python-components-with-a-clojure-topology"></a>Clojure 拓扑的 Python 组件

> [AZURE.NOTE] 本示例位于__ClojureTopology__目录中的[https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) 。

这种拓扑是通过使用[Leiningen](http://leiningen.org) [创建一个新的 Clojure 项目](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)创建的。 在此之后，已进行到基架项目以下修改︰

* __project.clj__︰ 为冲击和可能导致问题时部署到 HDInsight 服务器的项目排除条件添加依赖项。
* __资源/资源__︰ Leiningen 中创建一个默认`resources`目录中，但此处存储的文件会被添加到此项目中，从创建的 jar 文件的根和风暴需要命名的子目录中的文件`resources`。 这样一个子目录已添加，并且 Python 文件存储在`resources/resources`。 在运行时，这将被视为根 （/） 来访问 Python 组件。
* __src/wordcount/core.clj__︰ 此文件包含的拓扑定义，并从__project.clj__文件引用。 使用 Clojure 定义风暴拓扑的详细信息，请参阅[Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html)。

###<a name="build-and-run-the-project"></a>生成和运行项目

__若要生成并运行本地项目__，请使用以下命令︰

    lein clean, run

若要停止拓扑结构，使用__Ctrl + C__。

__若要生成 uberjar，并将部署到 HDInsight__，请执行以下步骤︰

1. 创建包含的拓扑结构和所需的相关性 uberjar:

        lein uberjar

    这将创建一个新文件，名为`wordcount-1.0-SNAPSHOT.jar`在`target\uberjar+uberjar`目录。
    
2. 使用下列方法之一来部署和运行 HDInsight 群集拓扑结构︰

    * __基于 linux * 的 HDInsight__
    
        1. 将文件复制到 HDInsight 群集的头节点使用`scp`。 例如︰
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            替换为您的群集和群集名称为 HDInsight 群集名称 SSH 用户用户名。
            
        2. 一旦该文件已复制到群集，使用 SSH 连接到群集并提交作业。 在 HDInsight 中使用 SSH 的信息，请参阅下列项之一︰
        
            * [SSH 使用 Linux、 Unix 或 OS X 中的基于 Linux 的 HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [SSH 使用 Windows 中的基于 Linux 的 HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. 建立连接后，使用以下方法来启动拓扑︰
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __基于 Windows 的 HDInsight__
    
        1. 通过在您的浏览器转到 HTTPS://CLUSTERNAME.azurehdinsight.net/ 连接到风暴的仪表板。 群集名称替换您的 HDInsight 群集名称和提供的管理员用户名和密码提示时。

        2. 使用窗体，请执行以下操作︰

            * __Jar 文件__︰ 选择__浏览__，然后选择__字数统计-1.0-SNAPSHOT.jar__文件
            * __类名称__︰ 输入`wordcount.core`
            * __附加参数__︰ 输入的友好名称，例如`wordcount`来识别拓扑

            最后，选择__提交__开始拓扑结构。

> [AZURE.NOTE] 启动后，风暴拓扑运行直到停止 （删除）。若要停止拓扑结构，使用两种`storm kill TOPOLOGYNAME`命令从命令行 （SSH 会话到 Linux 群集） 或使用风暴用户界面，选择拓扑，，然后选择__取消__按钮。

##<a name="next-steps"></a>下一步行动

在本文中，您学习了如何从风暴拓扑结构中使用 Python 组件。 请参阅以下文档以 HDInsight 中使用 Python 的其他方法︰

* [如何使用 Python 的 MapReduce 作业流](hdinsight-hadoop-streaming-python.md)
* [如何使用 Python 用户定义函数 (UDF) 在小猪和配置单元](hdinsight-python.md)
