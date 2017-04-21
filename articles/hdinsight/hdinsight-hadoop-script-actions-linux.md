<properties
    pageTitle="脚本操作开发的基于 Linux 的 HDInsight |Microsoft Azure"
    description="如何自定义脚本操作的基于 Linux 的 HDInsight 群集。 脚本操作是通过指定群集的配置设置或在群集上安装附加的服务、 工具或其他软件自定义 Azure HDInsight 群集的方法。 "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>使用 HDInsight 脚本操作开发

脚本操作是通过指定群集的配置设置或在群集上安装附加的服务、 工具或其他软件自定义 Azure HDInsight 群集的方法。 在群集创建过程中或在群集上运行，可以使用脚本的操作。

> [AZURE.NOTE] 此文档中的信息是特定于基于 Linux 的 HDInsight 群集。 在使用基于 Windows 群集脚本操作的信息，请参见[脚本操作开发与 HDInsight (Windows)](hdinsight-hadoop-script-actions.md)。

## <a name="what-are-script-actions"></a>脚本操作有哪些？

脚本操作是 Azure 运行在群集节点安装软件或进行配置更改的 Bash 脚本。 将操作脚本保存为根，执行并提供到群集节点的完全访问权限。

可以通过以下方法应用脚本操作︰

| 用于应用脚本... | 在群集创建... | 在群集上运行。. |
| ----- |:-----:|:-----:|
| Azure 门户 | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| HDInsight.NET SDK | ✓ | ✓ |
| Azure 的资源管理器模板 | ✓ | &nbsp; |

使用这些方法来应用脚本操作的详细信息，请参阅[自定义 HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

## <a name="bestPracticeScripting"></a>脚本开发的最佳做法

在开发自定义脚本 HDInsight 群集时，有几种最佳做法，请记住︰

- [目标的 Hadoop 版本](#bPS1)
- [目标操作系统版本](#bps10)
- [提供稳定的脚本资源的链接](#bPS2)
- [使用预编译的资源](#bPS4)
- [确保群集的自定义脚本是幂等](#bPS3)
- [确保高可用性的群集体系结构](#bPS5)
- [配置要使用 Azure Blob 存储的自定义组件](#bPS6)
- [将信息写入到标准输出和 STDERR](#bPS7)
- [将文件保存为 ASCII，与 LF 行尾](#bps8)
- [使用重试逻辑从瞬态错误恢复](#bps9)

> [AZURE.IMPORTANT] 脚本操作必须在 60 分钟内，完成或他们将会超时。 过程节点资源调配，是与安装和配置的其他进程同时运行脚本。 对 CPU 时间或网络带宽等资源的争夺可能会导致脚本需要较长时间才能完成比它在您的开发环境。

### <a name="bPS1"></a>目标的 Hadoop 版本

不同版本的 HDInsight 有 Hadoop 服务和安装的组件的不同版本。 如果您的脚本需要的服务或组件的特定版本，您只应与包含所需的组件的 HDInsight 版本使用脚本。 可以使用[HDInsight 组件版本控制](hdinsight-component-versioning.md)文档找到 HDInsight 中包含的组件版本的信息。

###<a name="bps10"></a>目标操作系统版本

基于 linux * 的 HDInsight 基于 Ubuntu Linux 分发。 不同版本的 HDInsight 依赖不同版本的 Ubuntu，可能会影响您的脚本的处理方式。 例如，HDInsight 3.4 及更早版本基于 Ubuntu 版本，并且使用 Upstart。 3.5 版基于 Ubuntu 16.04，使用 Systemd。 Systemd 和 Upstart 依赖不同的命令，使您的脚本编写时应该同时使用。

HDInsight 3.4 和 3.5 的另一个重要区别在于`JAVA_HOME`现在指向 Java 8。

您可以通过使用检查操作系统版本`lsb_release`。 下面从色相安装脚本的代码段演示如何确定是否 Ubuntu 14 或 16 上运行该脚本︰

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

您可以找到包含这些代码段在 https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh 的完整脚本。

Ubuntu 是由 HDInsight 的版本，请参阅[HDInsight 组件版本](hdinsight-component-versioning.md)文档。

要了解 Systemd 和 Upstart 之间的差异，请参阅[Systemd Upstart 用户](https://wiki.ubuntu.com/SystemdForUpstartUsers)。

### <a name="bPS2"></a>提供稳定的脚本资源的链接

请确保脚本和由脚本的资源保持可用的整个生存期内群集，并且，这些文件的版本不会更改持续时间。 如果将新节点添加到群集缩放操作时，这些资源是必需的。

最佳做法是下载并在您的订购 Azure 存储帐户中的所有内容进行存档。

> [AZURE.IMPORTANT] 使用存储帐户必须是群集或公共的只读容器上的任何其他存储帐户默认存储帐户。

例如，Microsoft 提供的示例存储在[https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/)存储帐户，它是由 HDInsight 团队维护公共的只读的容器中。

### <a name="bPS4"></a>使用预编译的资源

若要减少运行该脚本所需的时间，避免编译源代码中的代码资源的操作。 相反，预编译的资源，以便它可以快速下载到群集脚本从 Azure Blob 存储中存储的二进制版本。

### <a name="bPS3"></a>确保群集的自定义脚本是幂等

脚本必须能够是在这个意义上，如果脚本运行多次的幂等的它应确保每次它运行群集则返回到相同的状态。

例如，如果自定义脚本安装应用程序在 /usr/local/bin 上其第一次运行，然后在每次后续运行该脚本应检查是否已存在应用程序的 /usr/local/bin 位置继续与其他脚本中的步骤之前。

### <a name="bPS5"></a>确保高可用性的群集体系结构

基于 linux * 的 HDInsight 群集提供了两个头节点是群集中的活动，为两个节点运行操作的脚本。 如果您安装的组件应该只有一个头节点，您必须设计一个脚本，它只将一个头两个群集节点上安装组件。

> [AZURE.IMPORTANT] 作为 HDInsight 的一部分安装的默认服务旨在头的两个节点之间切换，根据需要但是此功能不扩展到通过脚本 ctions 安装的自定义组件。 如果您需要通过脚本动作高度可安装的组件，您必须实现自己使用的两种可用的头节点的故障转移机制。

### <a name="bPS6"></a>配置要使用 Azure Blob 存储的自定义组件

在群集安装的组件可能会使用 Hadoop 分布式文件系统 (HDFS) 存储的默认配置。 HDInsight 使用 Azure 存储 Blob (WASB) 作为默认存储。 这提供了仍然存在的数据，即使删除群集 HDFS 兼容的文件系统。 您应该配置组件安装而不是 HDFS 中使用 WASB。

例如，以下将复制 giraph examples.jar 文件从本地文件系统到 WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>将信息写入到标准输出和 STDERR

在脚本执行过程中写入到标准输出和 STDERR 登录此计算机，并且可以使用 Ambari web 用户界面来查看。

> [AZURE.NOTE] Ambari 将只能成功创建群集时可用。 如果您使用脚本操作期间创建的群集，并且创建操作失败，请参见[自定义 HDInsight 群集使用脚本操作](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)的故障排除部分访问记录的信息的其他方式

大多数工具和安装程序包将已经信息写入标准输出和 STDERR，但是要添加额外的日志记录。 若要将文本发送到标准输出使用`echo`。 例如︰

        echo "Getting ready to install Foo"

默认情况下，`echo`会将字符串发送到 STDOUT。 若要将其应用到 STDERR，添加`>&2`前`echo`。 例如︰

        >&2 echo "An error occurred installing Foo"

这将重定向到 STDERR (2) 的信息发送到标准输出 (1，即因此未在此处列出的默认值)。 在 IO 重定向的详细信息，请参阅[http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html)。

查看脚本操作记录信息的详细信息，请参阅[使用脚本操作的自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>将文件保存为 ASCII，与 LF 行尾

Bash 脚本应存储为 ASCII 格式，行以换行符结尾。 如果文件存储为 utf-8，其中可能包括字节顺序标记开头的文件，或行尾的 CRLF，是很常见的 Windows 编辑器，脚本将失败有类似于下面的错误︰

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>使用重试逻辑从瞬态错误恢复

下载安装包使用 apt get 或其他操作的网络传输的数据的文件时该操作可能由于暂时的网络错误而导致失败。 例如，与您通讯的远程资源可能在过程中故障转移到备份节点。

若要使您的脚本弹性瞬态错误，您可以实现重试逻辑。 以下是将运行程序给它传递任何命令 （如果该命令失败，） 重试最多三次函数的一个示例。 它将一直等待两秒钟，每次重试之间。

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

以下是使用此函数的示例。

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>自定义脚本的帮助器方法

脚本操作的帮助器方法是编写自定义脚本时，您可以使用的实用程序。 这些在[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)，并可以包含在使用以下脚本︰

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

这使得以下帮助程序可用于在脚本中︰

| 帮助程序使用情况 | 说明 |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | 为指定的文件路径的源 url 下载文件。 默认情况下，它不会覆盖现有的文件。 |
| `untar_file TARFILE DESTDIR` | 提取一个 tar 文件 (使用`-xf`，) 到目标目录。 |
| `test_is_headnode` | 如果运行在群集头节点，返回 1;否则为返回 0。 |
| `test_is_datanode` | 如果当前节点是一个数据 （工作人员） 节点，则返回 1;否则为返回 0。 |
| `test_is_first_datanode` | 如果当前节点 （称为 workernode0） 的第一个数据 （工作人员） 节点，则返回 1;否则为返回 0。 |
| `get_headnodes` | 在群集中返回 headnodes 的完全限定的域名称。 名称是用逗号分隔。 在出现错误则返回一个空字符串。 |
| `get_primary_headnode` | 获取完全限定的域名称主 headnode。 在出现错误则返回一个空字符串。 |
| `get_secondary_headnode` | 获取辅助 headnode 的完全限定的域名。 在出现错误则返回一个空字符串。 |
| `get_primary_headnode_number` | 获取主 headnode 数字后缀。 在出现错误则返回一个空字符串。 |
| `get_secondary_headnode_number` | 获取辅助 headnode 数字后缀。 在出现错误则返回一个空字符串。 |

## <a name="commonusage"></a>常见的使用模式

此部分提供了指南在编写您自己的自定义脚本时可能会遇到的常见使用模式的一些实现。

### <a name="passing-parameters-to-a-script"></a>将参数传递到脚本

在某些情况下，您的脚本可能需要参数。 例如，可能需为群集管理员密码 Ambari REST API，从中检索信息。

传递给脚本的参数被称为_定位参数_，并且被指派给`$1`为第一个参数，`$2`的第二个，和这样的上的。 `$0`包含该脚本本身的名称。

这样传递的值被视为文本，并进行特殊处理不包含字符的值作为参数传递给该脚本应包含用单引号 （'） ' ！。

### <a name="setting-environment-variables"></a>设置环境变量

通过以下方式执行设置环境变量︰

    VARIABLENAME=value

其中，变量名是变量的名称。 若要完成此操作后访问该变量，请使用`$VARIABLENAME`。 例如，若要指定环境变量名为密码作为位置参数提供的值，将使用以下︰

    PASSWORD=$1

然后，后续访问的信息可以使用`$PASSWORD`。

在脚本中设置的环境变量只存在脚本的范围内。 在某些情况下，您可能需要添加后脚本已完成，将持续的系统范围的环境变量。 这通常是这样群集通过 SSH 连接的用户可以使用由您的脚本安装的组件。 您可以做到这一点将添加到该环境变量`/etc/environment`。 例如，下面的示例__HADOOP\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>访问的位置存储的自定义脚本

脚本用于自定义群集需要到或者是默认的存储帐户对于群集或，如果在另一个存储帐户，在一个公共的只读容器。 如果您的脚本访问位于其他位置的资源这些还需要在公开访问 (至少公共只读的)。 例如要将文件下载到群集使用`download_file`。

在 Azure 存储帐户可访问的文件存储群集 （例如，默认的存储帐户），将提供快速访问，因为此存储是 Azure 的网络内。

### <a name="checking-the-operating-system-version"></a>检查操作系统版本

不同版本的 HDInsight 依赖特定版本的 Ubuntu。 可能有您必须检查您的脚本中的 OS 版本之间的差异。 例如，您可能需要安装与 Ubuntu 的版本的二进制文件。

若要检查操作系统版本，请使用`lsb_release`。 例如，下面的示例演示如何引用不同的 tar 文件根据操作系统版本︰

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>部署脚本操作的核对清单

我们采取了在准备部署这些脚本步骤如下︰

- 将这些文件包含在部署过程中群集节点可以访问的位置中的自定义脚本。 这可以是任何默认或指定的群集部署或任何其他公共可访问的存储容器时的额外存储帐户。

- 添加脚本以确保它们执行 impotently，以便脚本可以执行多次的同一个节点上检查。

- 使用临时文件目录 /tmp 保留下载的文件使用的脚本，然后清理它们之后执行脚本了。

- OS 级别的设置或 Hadoop 服务配置文件已更改，您可能需要重新启动 HDInsight 服务，以便他们可以拿起任何 OS 级别设置，如脚本中设置这些环境变量。

## <a name="runScriptAction"></a>如何运行脚本操作

脚本操作可用于通过使用 Azure 门户，Azure PowerShell，Azure 资源管理器 (ARM) 的模板或 HDInsight.NET SDK 的自定义 HDInsight 群集。 有关说明，请参阅[如何使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

## <a name="sampleScripts"></a>自定义脚本示例

Microsoft 提供的示例脚本在 HDInsight 群集上安装组件。 示例脚本，说明如何使用它们，请访问下面的链接︰

- [安装和使用 HDInsight 群集上的色调](hdinsight-hadoop-hue-linux.md)
- [上安装和使用 R HDInsight Hadoop 群集](hdinsight-hadoop-r-scripts-linux.md)
- [上安装和使用 Solr HDInsight 群集](hdinsight-hadoop-solr-install-linux.md)
- [安装和使用 HDInsight 群集上的 Giraph](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] 上面的链接的文档是基于 Linux 的 HDInsight 群集所特有。 为使用基于 Windows 的 HDInsight 脚本请参见[脚本操作开发与 HDInsight (Windows)](hdinsight-hadoop-script-actions.md)或使用顶部的每篇文章的链接。

##<a name="troubleshooting"></a>故障排除

使用您已经开发的脚本时可能遇到的错误如下︰

__Error__: `$'\r': command not found`. 有时跟`syntax error: unexpected end of file`。

_原因_︰ 当以 CRLF 结束脚本中的行，则导致此错误。 Unix 系统预计仅 LF 作为行尾。

此问题通常会出现在 Windows 环境中，编写脚本时 CRLF 是常见的行尾的许多文本编辑器窗口。

_解决方法_︰ 如果它是在文本编辑器中的选项，选择 Unix 格式或 LF 的行尾。 您可能在 Unix 系统上使用以下命令更改为 LF 的 CRLF:

> [AZURE.NOTE] 在于他们应该改 LF CRLF 行尾，以下命令是大致相当。 根据您的系统上可用的实用程序中选择一个。

| 命令 | 备注 |
| ------- | ----- |
| `unix2dos -b INFILE` | 将使用备份原始文件。BAK 扩展 |
| `tr -d '\r' < INFILE > OUTFILE` | OUTFILE 将包含只 LF 尾的版本 |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | 这将创建一个新文件无直接修改文件 |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | OUTFILE 将包含只 LF 尾的版本。

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_原因_︰ 脚本保存为 UTF-8 的字节顺序标记 (BOM) 时，会发生此错误。

_解决方法_︰ 将文件保存为 ascii 码或 utf-8 没有物料清单。 您可能在 Linux 或 Unix 系统上使用以下命令来创建一个新文件，而该物料清单︰

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

上面的命令中，将__使用 INFILE__替换包含清单的文件。 __OUTFILE__应该是一个新的文件名，将包含该脚本而无需物料清单。

## <a name="seeAlso"></a>下一步行动

* 了解如何[使用脚本操作的自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md)

* 使用[HDInsight.NET SDK 参考中](https://msdn.microsoft.com/library/mt271028.aspx)了解更多有关创建管理 HDInsight 的.NET 应用程序

* 使用[HDInsight REST API，](https://msdn.microsoft.com/library/azure/mt622197.aspx)若要了解如何使用其余 HDInsight 群集上执行管理操作。
