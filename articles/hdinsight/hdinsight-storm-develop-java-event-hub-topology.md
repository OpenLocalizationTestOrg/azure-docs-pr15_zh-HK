<properties
   pageTitle="处理事件从事件集线器使用 Java 的 HDInsight 上的风暴与 |Azure"
   description="了解如何通过使用 Maven 创建 Java 风暴拓扑处理事件集线器数据。"
   services="hdinsight,notification hubs"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>从 Azure 事件集线器上 HDInsight (Java) 的冲击与流程事件

Azure 事件集线器可以处理大量的数据从网站、 应用程序和设备。 事件集线器管口使其易于使用 Apache 风暴在 HDInsight 分析实时数据。 您还可以编写数据到事件集线器从风暴通过事件集线器螺栓。

在本教程中，您将学习如何使用事件集线器管口和螺栓来读取和写入数据的基于 Java 的风暴拓扑结构中。

## <a name="prerequisites"></a>系统必备组件

* 在 HDInsight 群集上的 Apache 风暴。 使用以下获取的开始来创建群集的文章︰

    - [在 HDInsight 群集上的基于 Linux 的风暴](hdinsight-apache-storm-tutorial-get-started-linux.md)︰ 选择此选项，如果您想要使用 SSH 来使用 Linux、 Unix、 OS X 或 Windows 客户端从群集

    - [在 HDInsight 群集上的基于 Windows 的风暴](hdinsight-apache-storm-tutorial-get-started.md)︰ 选择此选项，如果您想要使用 PowerShell 处理从 Windows 客户机的群集

    > [AZURE.NOTE] 本文档中的步骤操作基于使用 3.3 或 3.4 HDInsight 群集上的冲击。 这些群集提供 0.10.0，风暴和 Hadoop 2.7，减少使此示例正确运行所需的步骤数。
    >
    > 配合上 HDInsight 3.2 风暴 0.9.3 本示例的版本，请参阅示例存储库中的[风暴 v0.9.3](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub/tree/Storm_v0.9.3)分支。

* [Azure 事件中心](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

* [Oracle Java 开发人员工具箱 (JDK) 版本 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)或等效物，如[OpenJDK](http://openjdk.java.net/)

* [Maven](https://maven.apache.org/download.cgi): Maven 是 Java 项目的项目生成系统

* 文本编辑器或 Java 集成的开发环境 (IDE)

    > [AZURE.NOTE] 您的编辑器或 IDE 可能必须使用 Maven 不解决本文档中的特定功能。 编辑环境中的功能的信息，请参阅您正在使用的产品的文档。

 * SSH 客户端。 在 HDInsight 中使用 SSH 请参阅以下文章获取详细信息之一︰

    - [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

* SCP 的客户端。 这是所有 Linux 和 Unix 中，OS X 系统都提供。 对于 Windows 客户端，建议 PSCP，即可以从[PuTTY 下载页面](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

##<a name="understanding-the-example"></a>了解此示例

该[hdinsight java 风暴 eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)示例包含两个拓扑︰

__com.microsoft.example.EventHubWriter__在 Azure 事件集线器中写入随机数据。 数据生成的管口，并且是一个随机的设备 ID 和设备值。 因此它模拟某些硬件发出字符串 ID 和数值。

__com.microsoft.example.EventHubReader__从事件中心 （由 EventHubWriter，写入数据） 读取数据并将其存储到 HDFS (在本例中，因为这是编写和测试使用 Azure HDInsight WASB) 在 /devicedata 目录中。

数据格式化为 JSON 文档之前写入到事件中心，并通过读取器读取时进行分析，从 JSON 取出并放到元组。 JSON 格式如下所示︰

    { "deviceId": "unique identifier", "deviceValue": some value }

使用 JSON 文档以将数据存储到事件中心的原因是，以便我们知道格式是什么，而不是依赖事件中心 Spout 和螺栓的内部格式结构。

###<a name="project-configuration"></a>项目配置

**POM.xml**文件中包含此 Maven 项目的配置信息。 有趣的部分是︰

####<a name="the-eventhubs-storm-spout-dependency"></a>EventHubs 冲击 Spout 依赖项

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-eventhubs</artifactId>
      <version>0.10.0</version>
    </dependency>

这将添加风暴 eventhubs 包，其中包含用于读取事件集线器，管口和向其写入的螺栓的依存关系。

> [AZURE.NOTE] 此包只是适用于风暴版本 0.10.0 或更高。 当使用风暴 0.9.3 时，您必须手动安装由 Microsoft 提供的管口包。 冲击 0.9.3 所使用的示例，请参阅示例存储库中的[风暴 v0.9.3](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub/tree/Storm_v0.9.3)分支。

####<a name="the-hdfsbolt-and-wasb-components"></a>HdfsBolt 和 WASB 组件

HdfsBolt 通常用于将数据存储到 Hadoop 分布式文件系统 HDFS。 但是 HDInsight 群集使用 Azure 存储 (WASB) 作为默认数据存储区，所以，我们有几个组件，它们允许 HdfsBolt 了解 WASB 文件系统加载。

      <!--HdfsBolt stuff -->
        <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-hdfs</artifactId>
        <exclusions>
            <exclusion>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-hdfs</artifactId>
            </exclusion>
        </exclusions>
        <version>0.10.0</version>
        </dependency>
    <!--So HdfsBolt knows how to talk to WASB -->
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
        <version>2.7.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-hdfs</artifactId>
        <version>2.7.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-azure</artifactId>
        <version>2.7.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.1</version>
        <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
        </exclusions>
    </dependency>

> [AZURE.NOTE] 在使用早期版本的 HDInsight，如版本 3.2，您必须手动注册这些组件。 举例说明，以及对于较旧的 HDInsight 群集所需的自定义位请参阅请参阅[风暴 v0.9.3](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub/tree/Storm_v0.9.3)资料库分支的示例。

####<a name="the-maven-compiler-plugin"></a>Maven 编译器插件

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>2.3.2</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

这说明，该项目应编译与兼容的 Java 7，这是由 HDInsight 群集 Maven。

####<a name="the-maven-shade-plugin"></a>Maven 底纹插件

      <!-- build an uber jar -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <transformers>
            <!-- Keep us from getting a can't overwrite file error -->
            <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
            <!-- Keep us from getting errors when trying to use WASB from the storm-hdfs bolt -->
            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
          </transformers>
          <!-- Keep us from getting a bad signature error -->
          <filters>
            <filter>
                <artifact>*:*</artifact>
                <excludes>
                    <exclude>META-INF/*.SF</exclude>
                    <exclude>META-INF/*.DSA</exclude>
                    <exclude>META-INF/*.RSA</exclude>
                </excludes>
            </filter>
          </filters>
        </configuration>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

这用来打包到 uber jar，它包含的项目代码和所需的依赖项的解决方案。 它也用于︰

* 重命名许可证文件的依赖项︰ 如果不这样做它会导致基于 Windows HDInsight 群集上的运行时错误。

* 排除安全/签名︰ 如果不这样做可能导致在 HDInsight 群集上运行时产生错误。

* 确保同一接口的多个实现将合并成一项。 如果不这样做，您将收到错误的风暴 HDFS 螺栓不理解如何与 WASB 文件系统进行通信。

####<a name="the-exec-maven-plugin"></a>该执行 maven 插件

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.2.1</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

这使您可以在您使用以下命令的开发环境中本地运行拓扑︰

    mvn compile exec:java -Dstorm.topology=<CLASSNAME>

例如， `mvn compile exec:java -Dstorm.topology=com.microsoft.example.EventHubWriter`。

####<a name="the-resources-section"></a>资源部分

    <resources>
      <resource>
        <directory>${basedir}/conf</directory>
        <filtering>false</filtering>
        <includes>
          <include>EventHubs.properties</include>
          <include>core-site.xml</include>
        </includes>
      </resource>
    </resources>

这定义了项目所需的资源︰

- **EventHubs.properties**︰ 包含用于连接到 Azure 事件中心的信息
- **核心-site.xml**︰ 包含 HDInsight 群集使用 Azure 存储有关信息。

有关事件中心和 HDInsight 群集，必须填充这两个信息。

##<a name="configure-environment-variables"></a>配置环境变量

在开发工作站上安装 Java 和 JDK 时，可以设置以下环境变量。 但是，您应该检查它们存在，它们包含您系统的正确值。

* **JAVA_HOME** -应指向 Java 运行时环境 (JRE) 的安装目录。 例如，在 Unix 或 Linux 分发，它应具有值类似于`/usr/lib/jvm/java-7-oracle`。 在 Windows 中，它将具有类似于一个值`c:\Program Files (x86)\Java\jre1.7`

* **路径**-应包含以下路径︰

    * **JAVA_HOME**（或等效的路径）

    * **JAVA_HOME\bin**（或等效的路径）

    * Maven 时的安装目录

## <a name="configure-event-hub"></a>配置事件中心

事件集线器是此示例的数据源。 使用以下步骤创建新的事件中心。

1. 从[Azure 传统门户网站](https://manage.windowsazure.com)，选择**新建** > **服务总线** > **事件中心** > **自定义创建**。

2. 在**添加新的事件中心**屏幕上，输入**事件中心的名称**，选择要创建中，并创建新的命名空间或选择一个现有的**地区**。 单击**箭头**以继续。

    ![向导第 1 页](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

    > [AZURE.NOTE] 为您在 HDInsight 服务器上的冲击，减少反应时间和成本，应该选择相同的**位置**。

2. 在**配置事件中心**屏幕上，输入**分区计数**和**消息保留**值。 对于此示例，使用 10 分区计数和 1 的消息保留。 请注意分区计数，因为您以后将需要此值。

    ![向导的第 2 页](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

3. 在创建事件中心后，选择的命名空间、 选择**事件集线器**，然后选择您在前面创建的事件集线器。

4. 选择**配置**，然后使用以下信息创建两个新的访问策略。

    <table>
    <tr><th>名称</th><th>权限</th></tr>
    <tr><td>编写器</td><td>发送</td></tr>
    <tr><td>读取器</td><td>侦听</td></tr>
    </table>

    您创建的权限后，选择页面底部的**保存**图标。 这将创建将用于发送 （编写器） 和侦听此事件中心 （读者） 的共享的访问策略。

    ![策略](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

5. 在保存策略后，使用**共享访问密钥生成器**在页面的底部可检索的**编写**和**读取**策略的键。 保存这些，因为他们将稍后使用。

## <a name="download-and-build-the-project"></a>下载并生成项目

1. 从 GitHub 下载项目︰ [hdinsight java 风暴 eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)。 可以下载 zip 存档，作为软件包，也可以使用[git](https://git-scm.com/)克隆本地项目。

2. 使用以下方法来构建和打包项目︰

        mvn package

    这将下载必需的依存关系、 构建和包项目。 输出将__/target__目录中存储为__EventHubExample-1.0-SNAPSHOT.jar__。

## <a name="deploy-the-topologies"></a>部署拓扑结构

创建此项目的 jar 包含两种拓扑结构;__com.microsoft.example.EventHubWriter__和__com.microsoft.example.EventHubReader__。 中的事件写入事件集线器，然后读取 EventHubReader 应首先，启动 EventHubWriter 拓扑。

###<a name="if-using-a-linux-based-cluster"></a>如果使用的基于 Linux 的群集

1. 使用 SCP jar 包复制到 HDInsight 群集。 替换为群集的 SSH 用户用户名。 群集名称替换 HDInsight 群集的名称︰

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用`-i`参数指定的密钥文件的路径。 例如， `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`。

    > [AZURE.NOTE] 如果您的客户端是一个 Windows 工作站，则您可能没有安装 SCP 命令。 我们建议 PSCP，可以从[PuTTY 下载页](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)中下载。

    此命令将该文件复制到您在群集上的 SSH 用户的主目录。

1. 一旦文件完成上载后，使用 SSH 连接到 HDInsight 群集。 更换**用户名**的 SSH 登录名称。 **群集名称**替换您的 HDInsight 群集名称︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] 如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用`-i`参数指定的密钥文件的路径。 下面的示例将加载私钥从`~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

    如果您使用的 PuTTY，输入`CLUSTERNAME-ssh.azurehdinsight.net`__主机名 （或 IP 地址）__的字段，然后单击__打开__连接。 系统将提示您输入您的 SSH 帐户名。

    > [AZURE.NOTE] 如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用以下步骤选择的注册表项︰
    >
    > 1. 在**类别**中，扩展**连接**，展开**SSH**，选择**身份验证**。
    > 2. 单击**浏览**，然后选择.ppk 文件，其中包含您的私钥。
    > 3. 单击__打开__连接。

2. 要启动拓扑，请使用下面的命令︰

        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubWriter writer
        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubReader reader

    这将启动拓扑，并赋予它们"读取"和"写入"的好记的名称。

3. 等待一分钟或两个允许写入和读取事件从事件中心的拓扑，然后使用下面的命令以验证 EventHubReader 存储数据到 HDInsight 存储︰

        hadoop fs -ls /devicedata

    这应返回类似于以下的文件的列表︰

        -rw-r--r--   1 storm supergroup      10283 2015-08-11 19:35 /devicedata/wasbbolt-14-0-1439321744110.txt
        -rw-r--r--   1 storm supergroup      10277 2015-08-11 19:35 /devicedata/wasbbolt-14-1-1439321748237.txt
        -rw-r--r--   1 storm supergroup      10280 2015-08-11 19:36 /devicedata/wasbbolt-14-10-1439321760398.txt
        -rw-r--r--   1 storm supergroup      10267 2015-08-11 19:36 /devicedata/wasbbolt-14-11-1439321761090.txt
        -rw-r--r--   1 storm supergroup      10259 2015-08-11 19:36 /devicedata/wasbbolt-14-12-1439321762679.txt

    > [AZURE.NOTE] 某些文件可能显示的大小为 0，因为它们是由 EventHubReader，但数据未存储到这些尚未。

    您可以通过使用下面的命令来查看这些文件的内容︰

        hadoop fs -text /devicedata/*.txt

    这将返回类似于以下内容的数据︰

        3409e622-c85d-4d64-8622-af45e30bf774,848981614
        c3305f7e-6948-4cce-89b0-d9fbc2330c36,-1638780537
        788b9796-e2ab-49c4-91e3-bc5b6af1f07e,-1662107246
        6403df8a-6495-402f-bca0-3244be67f225,275738503
        d7c7f96c-581a-45b1-b66c-e32de6d47fce,543829859
        9a692795-e6aa-4946-98c1-2de381b37593,1857409996
        3c8d199b-0003-4a79-8d03-24e13bde7086,-1271260574

    第一列包含的设备 ID 值和第二列是设备的值。

4. 使用以下命令来停止拓扑︰

        storm kill reader
        storm kill writer

###<a name="if-using-a-windows-based-cluster"></a>如果使用基于 Windows 群集

1. 打开浏览器到 https://CLUSTERNAME.azurehdinsight.net。 出现提示时，输入 HDInsight 群集管理员凭据。 您将到达风暴仪表板。

2. 使用__Jar 文件__下拉列表来浏览并选择 EventHubExample-1.0-SNAPSHOT.jar 文件从您的生成环境。

3. 对于__类名称__，输入`com.mirosoft.example.EventHubWriter`。

4. 对于__其他参数__，输入`writer`。 最后，单击__提交__上载罐和启动 EventHubWriter 拓扑。

5. 拓扑结构启动后，使用窗体启动 EventHubReader:

    * __Jar 文件__︰ 选择 EventHubExample-1.0-SNAPSHOT.jar 以前上载
    * __类名称__︰ 输入`com.microsoft.example.EventHubReader`
    * __附加参数__︰ 输入`reader`

    单击提交以开始 EventHubReader 拓扑。

6. 请等待几分钟以使拓扑生成事件，然后存储到 Azure 存储中，然后选择__Hadoop 查询控制台__选项卡顶部的__风暴仪表板__页。

7. 在__查询控制台__中，选择__配置单元编辑器__和替换默认的`select * from hivesampletable`如下︰

        create external table devicedata (deviceid string, devicevalue int) row format delimited fields terminated by ',' stored as textfile location 'wasbs:///devicedata/';
        select * from devicedata limit 10;

    单击__选择__以运行查询。 由 EventHubReader 写到 Azure 存储 (WASB) 的数据，这将返回 10 行。 查询完成后，您应该看到类似于下面的数据︰

        3409e622-c85d-4d64-8622-af45e30bf774,848981614
        c3305f7e-6948-4cce-89b0-d9fbc2330c36,-1638780537
        788b9796-e2ab-49c4-91e3-bc5b6af1f07e,-1662107246
        6403df8a-6495-402f-bca0-3244be67f225,275738503
        d7c7f96c-581a-45b1-b66c-e32de6d47fce,543829859
        9a692795-e6aa-4946-98c1-2de381b37593,1857409996
        3c8d199b-0003-4a79-8d03-24e13bde7086,-1271260574

8. 选择__风暴仪表板__顶部的页面，然后选择__风暴 UI__。 __冲击用户界面__中，从选择__读卡器__拓扑的链接，然后使用__取消__按钮停止拓扑。 对于__编写器__拓扑中重复此过程。



### <a name="checkpointing"></a>执行检查点操作

EventHubSpout 定期检查其状态到 Zookeeper 节点，从而节省了消息的当前偏移量从队列中读取。 这样的组件以开始接收以下方案中已保存的偏移量处的消息︰

* 组件实例失败，并且将重新启动。

* 扩大或缩小通过添加或移除节点的群集。

* 拓扑被杀死，并且**具有相同的名称**重新启动。

####<a name="on-windows-based-hdinsight-clusters"></a>在基于 Windows 的 HDInsight 群集上

您可以导出和导入 WASB （HDInsight 群集使用 Azure 存储。） 的持久的检查点若要执行此操作的脚本位于 HDInsight 群集，在**c:\apps\dist\storm-0.9.3.2.2.1.0-2340\zkdatatool-1.0\bin**上风暴。

>[AZURE.NOTE] 路径中的版本号可能不同，因为风暴在群集上安装的版本可能会在将来更改。

此目录中的脚本如下︰

* **stormmeta_import.cmd**︰ 从群集默认存储容器的所有风暴元数据导都入到 Zookeeper。

* **stormmeta_export.cmd**︰ 从 Zookeeper 的风暴的所有元数据导出到群集默认存储容器。

* **stormmeta_delete.cmd**︰ 删除 Zookeeper 风暴的所有元数据。

导入允许您时您需要删除该群集，但想要恢复处理从该集线器中的当前偏移量，使新群集返回到联机状态时保持检查点数据的导出。

> [AZURE.NOTE] 由于数据会保存到默认存储容器，新群集**必须**使用相同的存储帐户和容器作为以前的群集。

## <a name="delete-your-cluster"></a>删除群集

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="troubleshooting"></a>故障排除

如果您看不到存储的文件的 /devicedata 位置 (使用`hadoop fs -ls /devicedata`命令或在查询控制台上，配置单元 commandd) 风暴 UI 用于寻找拓扑由返回的任何错误。

使用冲击用户界面的详细信息，请参阅下列主题︰

* 如果使用 HDInsight 群集上的__基于 Linux 的__风暴，请参阅[部署和管理基于 Linux 的 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)

* 如果使用 HDInsight 群集上的__基于 Windows__的风暴，请参阅[部署和管理基于 Windows 的 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="next-steps"></a>下一步行动

* [在 HDInsight 上的风暴的示例拓扑](hdinsight-storm-example-topology.md)
