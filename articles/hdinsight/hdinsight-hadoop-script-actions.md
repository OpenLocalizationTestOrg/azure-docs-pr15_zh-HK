<properties
    pageTitle="脚本操作开发与 HDInsight |Microsoft Azure"
    description="了解如何自定义脚本操作的 Hadoop 群集。 要安装在 Hadoop 群集上运行的其他软件或更改安装在群集上的应用程序的配置，可以使用脚本操作。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>开发基于 HDInsight Windows 群集脚本操作脚本

了解如何为 HDInsight 编写脚本操作脚本。 有关使用脚本操作脚本的信息，请参阅[使用脚本操作的自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster.md)。 为基于 Linux 的 HDInsight 群集编写的同一篇文章，请参阅[HDInsight 的开发脚本操作脚本](hdinsight-hadoop-script-actions-linux.md)。

> [AZURE.NOTE] 此文档中的信息是特定于基于 Windows HDInsight 群集。 在使用基于 Windows 群集脚本操作的信息，请参见[脚本操作开发与 HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md)。


要安装在 Hadoop 群集上运行的其他软件或更改安装在群集上的应用程序的配置，可以使用脚本操作。 脚本操作时部署 HDInsight 群集，在群集节点运行的脚本，执行它们在群集中的节点完成 HDInsight 配置。 将操作脚本保存在系统管理员帐户权限下执行并提供到群集节点的完全访问权限。 每个群集可以提供脚本指定它们的顺序执行的操作的列表。

> [AZURE.NOTE] 如果您遇到以下错误消息︰
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> 这是因为没有包括的帮助器方法。  请参阅[自定义脚本的帮助器方法](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts)。

## <a name="sample-scripts"></a>示例脚本

有关如何在 Windows 操作系统上创建 HDInsight 群集，脚本操作是 Azure PowerShell 脚本。下面是一个示例脚本配置站点配置文件︰

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

此脚本将四个参数、 配置文件名称、 要修改，若要设置，所需的值的属性和说明。 例如︰

    hive-site.xml hive.metastore.client.socket.timeout 90

这些参数将在配置单元 site.xml 文件中将 hive.metastore.client.socket.timeout 值设置为 90。  默认值为 60 秒。

也可以在[https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1)找到此示例脚本。

HDInsight 提供几个 HDInsight 群集上安装其他组件的脚本︰

名称 | 脚本
----- | -----
**安装触发** | https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1。 请参阅[安装和使用 HDInsight 群集上激励][hdinsight-install-spark]。
**R 安装** | https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1。 请参阅[安装和使用 HDInsight 群集上的 R][hdinsight-r-scripts]。
**安装 Solr** | https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1。 [Solr 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-solr-install.md)，请参阅。
- **安装 Giraph** | https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1。 请参阅[Giraph 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-giraph-install.md)。

从 Azure 的门户中，Azure PowerShell 或通过 HDInsight.NET SDK，可以部署脚本操作。  有关详细信息，请参阅[自定义 HDInsight 群集使用脚本操作][hdinsight-cluster-customize]。

> [AZURE.NOTE] 示例脚本工作仅使用 HDInsight 群集版本 3.1 或更高。 HDInsight 群集版本的详细信息，请参阅[HDInsight 群集版本](hdinsight-component-versioning.md)。





## <a name="helper-methods-for-custom-scripts"></a>自定义脚本的帮助器方法

脚本操作的帮助器方法是编写自定义脚本时，您可以使用的实用程序。 这些在[https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)，并可以包含在使用以下脚本︰

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

以下是该脚本提供的帮助器方法︰

帮助器方法 | 说明
-------------- | -----------
**保存-HDIFile** | 下载文件到分配给该群集的 Azure VM 节点的本地磁盘上的位置从指定的统一资源标识符 (URI)。
**展开 HDIZippedFile** | 解压压缩的文件。
**调用 HDICmdScript** | 从 cmd.exe 运行脚本。
**写 HDILog** | 从脚本操作使用的自定义脚本编写输出。
**获取服务** | 获取在其中执行该脚本的计算机上运行的服务的列表。
**获取服务** | 具有特定的服务名称作为输入，获得特定服务的详细的信息 （服务名称、 进程 ID、 状态、 等） 在其中执行该脚本的计算机上。
**获得 HDIServices** | 获取在其中执行该脚本的计算机上运行的 HDInsight 服务的列表。
**获得 HDIService** | 使用特定 HDInsight 的服务名称作为输入，获得特定服务的详细的信息 （服务名称、 进程 ID、 状态、 等） 在其中执行该脚本的计算机上。
**获取服务运行** | 获取正在运行的服务的计算机上执行该脚本的位置的列表。
**获得 ServiceRunning** | 检查特定服务 （按名称） 运行在计算机上执行该脚本的位置。
**获得 HDIServicesRunning** | 获取在其中执行该脚本的计算机上运行的 HDInsight 服务的列表。
**获得 HDIServiceRunning** | 检查是否特定 HDInsight 服务 （按名称） 运行在计算机上执行该脚本的位置。
**获得 HDIHadoopVersion** | 获取在其中执行该脚本的计算机上安装的 Hadoop 的版本。
**测试 IsHDIHeadNode** | 在其中执行该脚本的计算机是头节点检查。
**测试 IsActiveHDIHeadNode** | 在其中执行该脚本的计算机是一个主动的头节点检查。
**测试 IsHDIDataNode** | 检查是否在执行该脚本的计算机的某个数据节点。
**编辑 HDIConfigFile** | 编辑配置文件配置单元 site.xml、 酷睿 site.xml，hdfs site.xml，mapred-site.xml 或 yarn site.xml。


## <a name="best-practices-for-script-development"></a>脚本开发的最佳做法

在开发自定义脚本 HDInsight 群集时，有几种最佳做法，请记住︰

- Hadoop 版本检查

    只有 HDInsight 版本 3.1 (Hadoop 2.4) 及以上使用脚本操作群集上安装自定义组件的支持。 在自定义脚本，必须使用**Get HDIHadoopVersion**帮助器方法以检查与在脚本中执行其他任务之前的 Hadoop 版本。


- 提供稳定的脚本资源的链接

    用户应该确保所有的脚本和其他用于群集中的自定义项的项目保留在该群集的整个生存期内提供和持续时间不变，这些文件的版本。 如果群集中的节点重新映像是必需的这些资源是必需的。 最佳做法是下载和归档中存储帐户，该用户控件的所有内容。 这可以是默认的存储帐户或任何其他存储帐户在一个自定义群集部署时指定。
    在添姿加 R 自定义群集示例在文档中，例如，我们做了资源的本地副本中此存储帐户提供︰ https://hdiconfigactions.blob.core.windows.net/。


- 确保群集的自定义脚本是幂等

    您就必须预料，HDInsight 群集的节点将重新映像的群集生存期内。 只要群集重新映像运行群集自定义脚本。 此脚本必须能够进行幂等意义上说，在重新映像，脚本应确保群集则返回到同一自定义是在最初创建群集的第一次运行该脚本后的状态。 例如，如果自定义脚本安装应用程序在 D:\AppLocation 在其第一次运行，则每个后续运行时，在重新映像，该脚本应检查是否存在应用程序的 D:\AppLocation 位置继续与其他脚本中的步骤之前。


- 在最佳位置中安装自定义组件

    当群集节点重新映像后，可以重新格式化，从而导致丢失的数据和应用程序必须已安装在这些驱动器 C:\ 资源驱动器和 D:\ 系统驱动器。 如果是群集的一部分的 Azure 的虚拟机 (VM) 节点出现故障时，替换为一个新的节点，也可能发生这种情况。 在 D:\ 驱动器或群集上的 C:\apps 位置，可以安装组件。 C:\ 驱动器上的所有其他场所都被保留。 指定应用程序或库在哪里安装在群集的自定义脚本的位置。


- 确保高可用性的群集体系结构

    HDInsight 具有高可用性，一个头节点处于活动模式 （其上运行的 HDInsight 服务） 和其他的头节点处于待机模式 （在哪个 HDInsight 服务未在运行） 是一个主动-被动体系结构。 如果 HDInsight 服务将被中断，节点切换主动和被动模式。 如果脚本操作用于高可用性的两个头节点上安装服务，注意 HDInsight 故障切换机制将无法自动故障转移这些用户安装服务。 因此用户安装需要具有高可用性的 HDInsight head 节点上的服务必须以主动-被动模式如果自己故障转移机制要么处于活动模式。

    头节点角色指定*ClusterRoleCollection*参数中的值为时，两个头节点上运行 HDInsight 脚本操作命令。 所以设计一个自定义脚本时，确保您的脚本已意识到此安装程序。 您不应遇到相同的服务已安装并在两者的头节点上启动，它们最终会与相互竞争的问题。 另外，请注意，数据将会丢失在重新映像，过程，以便通过脚本操作安装软件必须要有适应这类事件。 应用程序应可处理分布在多个节点的高可用性数据。 请注意多达 1/5 的群集中的节点可以同时重新映像。


- 配置要使用 Azure Blob 存储的自定义组件

    在群集节点安装的自定义组件可能具有的默认配置，使用 Hadoop 分布式文件系统 (HDFS) 存储。 您应该更改要改为使用 Azure Blob 存储的配置。 在群集重新映像，HDFS 文件系统获取格式化，就会丢失所存储的任何数据。 改为使用 Azure Blob 存储保证您的数据将被保留。

## <a name="common-usage-patterns"></a>常见的使用模式

此部分提供了指南在编写您自己的自定义脚本时可能会遇到的常见使用模式的一些实现。

### <a name="configure-environment-variables"></a>配置环境变量

通常在开发脚本操作中，会感到需要设置的环境变量。 例如，最可能的情况是当从外部站点下载二进制文件、 将其安装在群集中，并添加到 PATH 环境变量的安装位置的位置。 下面的代码段演示如何在自定义脚本中设置环境变量。

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

此语句将环境变量**MDS_RUNNER_CUSTOM_CLUSTER**设置为值 true，并且还将设置此变量为计算机范围的范围。 有时很重要，在适当的范围内--计算机或用户设置环境变量。 请[在此处][1]上设置环境变量的详细信息。

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>访问的位置存储的自定义脚本

用于自定义群集脚本需要或者是群集的默认存储帐户中或公共只读容器上存储的任何其他帐户中。 如果您的脚本访问位于其他位置的资源这些需要在公开访问 (至少公共只读的)。 例如您可能想要访问的文件并将其使用的 SaveFile HDI 命令保存。

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

在此示例中，必须确保容器中存储帐户 somestorageaccount somecontainer 是可公开访问。 否则为脚本将引发没有找到异常而失败。

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>将参数传递到添加 AzureRmHDInsightScriptAction cmdlet

若要将多个参数传递给添加 AzureRmHDInsightScriptAction cmdlet，您需要设置格式的字符串值，包含所有参数的脚本。 例如︰

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

或

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>引发异常的故障的群集部署

如果您想要获取准确的通知一个事实，即自定义群集不成功如预期的那样，是重要群集创建会失败并引发异常。 例如，您可能希望处理的文件，如果存在处理文件中不存在的错误情况。 这样可确保脚本能够正常退出，并正确地知道群集的状态。 下面的代码段举例说明如何实现这一点的︰

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

在本段中，如果文件不存在，它会导致到其中脚本实际上能够正常退出后打印错误消息，并且群集达到假定其"成功"完成群集自定义过程的运行状态的状态。 如果您想要一个事实，即实质上是群集自定义准确地通知未成功按照预期的方式丢失的文件，引发异常并使群集自定义步骤失败更为合适。 为此必须使用下面的示例代码段。

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>部署脚本操作的核对清单
我们采取了在准备部署这些脚本步骤如下︰

1. 将这些文件包含在部署过程中群集节点可以访问的位置中的自定义脚本。 这可以是任何默认或指定的群集部署或任何其他公共可访问的存储容器时的额外存储帐户。
2. 添加脚本以确保它们执行 idempotently，以便脚本可以执行多次的同一个节点上检查。
3. 使用打印到标准输出，以及 STDERR**输出写入**Azure PowerShell cmdlet。 不要使用**写入主机**。
4. 使用临时文件的文件夹，例如 $env︰ 温度，若要将下载的文件使用的脚本，然后清理它们之后执行脚本。
5. 安装自定义软件只能在 D:\ 或 C:\apps。 因为他们被保留，不应使用 c︰ 驱动器上的其他位置。 请注意，安装 C:\apps 文件夹外部 c︰ 驱动器上的文件可能会导致安装程序失败的节点重新映像期间。
6. OS 级别的设置或 Hadoop 服务配置文件已更改，您可能需要重新启动 HDInsight 服务，以便他们可以拿起任何 OS 级别设置，如脚本中设置这些环境变量。

## <a name="debug-custom-scripts"></a>调试自定义脚本

存储脚本错误日志，以及其他的输出，您指定为群集创建的默认存储帐户中。 日志都存储在一个表的名称*u < \cluster-name-fragment >< \time-stamp > setuplog*。 这些是所有的脚本运行在群集中的节点 （head 节点和辅助节点） 中的记录的聚合的日志。
检查日志中的简便方法是使用 HDInsight 工具 Visual Studio。 有关安装的工具，请参阅[开始使用 Visual Studio 的 Hadoop HDInsight 工具](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**若要检查使用 Visual Studio 的日志**

1. 打开 Visual Studio。
2. 单击**视图**，然后单击**服务器资源管理器**。
3. 用鼠标右键单击"Azure"，单击连接到**Microsoft Azure 订阅**，然后输入您的凭据。
4. 展开**存储**展开 Azure 存储帐户用作默认的文件系统、 展开**表**，然后双击表名称。


您可以同时远程群集节点，若要同时查看到 STDOUT 和自定义脚本的 STDERR。 每个节点上的日志专用于该节点，且已登录到**C:\HDInsightLogs\DeploymentAgent.log**。 这些日志文件记录的自定义脚本的所有输出。 用于触发脚本操作的示例日志代码段如下所示︰

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


在此日志中，很显然，在名为 HEADNODE0 的 VM 上执行触发脚本操作和无例外，在执行过程中引发。

执行故障发生时，在此日志文件中也可包含描述它的输出。 这些日志中提供的信息应在调试脚本中可能出现的问题很有帮助。


## <a name="see-also"></a>请参见

- [自定义 HDInsight 群集使用脚本操作][hdinsight-cluster-customize]
- [安装和使用 HDInsight 群集上触发][hdinsight-install-spark]
- [上安装和使用 R HDInsight 群集][hdinsight-r-scripts]
- [Solr 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-solr-install.md)。
- [Giraph 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-giraph-install.md)。

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
