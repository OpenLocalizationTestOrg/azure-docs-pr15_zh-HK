<properties
pageTitle="限制使用共享访问签名数据对 HDInsight 的访问"
description="了解如何使用共享访问签名来限制对存储在 Azure 存储 blob 数据的 HDInsight 访问。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>使用 Azure 存储共享访问签名限制访问的数据与 HDInsight

HDInsight 使用 Azure 存储 Blob 数据存储。 HDInsight 必须有到用作群集默认存储 blob 的完全访问权限，而您可以限制对存储在群集使用其他 blob 数据的权限。 例如，可能需要将某些数据只读。 你可以使用共享访问签名。

共享访问签名 (SA) 是 Azure 存储帐户的功能，允许您限制对数据的访问。 例如，提供对数据的只读访问权限。 在本文中，您将学习如何使用 SAS 从 HDInsight 到 blob 容器启用读取和仅限列表的访问权限。

##<a name="requirements"></a>要求

* Azure 的订阅

* C# 或者 Python。 为 Visual Studio 解决方案提供了 C# 的示例代码。

    * Visual Studio 必须是版本 2013年或 2015
    
    * Python 必须是 2.7 或更高版本

* 基于 linux * 的 HDInsight 群集或[Azure PowerShell] [ powershell] -如果您有一个现有的基于 Linux 的群集，您可以使用 Ambari 将共享访问签名添加到群集。 如果没有，您可以使用 Azure PowerShell 创建新的群集，在群集创建过程中添加共享访问签名。

* [Https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)中的示例文件。 此存储库中具有以下︰

    * 可以使用 HDInsight 创建存储容器，存储的策略和 SAS Visual Studio 项目
    
    * 可以创建存储容器，存储的策略和 SAS 与 HDInsight 一起使用的 Python 脚本
    
    * 可以创建一个新的 HDInsight 群集并将其配置为使用 SAS PowerShell 脚本。

##<a name="shared-access-signatures"></a>共享的访问签名

有两种形式的共享访问签名︰

* 点对点︰ 开始时间、 到期时间和 SAS 的权限是所有在 SAS URI 指定 （或暗示的保证，在未开始时间的情况下）。

* 存储访问策略︰ 存储访问权限策略的资源容器的 blob 容器、 表、 队列或文件共享的定义，并且可以用于管理一个或多个共享的访问签名的约束。 当您将 SAS 存储的访问策略相关联时，SAS 继承约束的开始时间、 过期时间和存储的访问策略中定义的权限的。

这两种形式之间的区别是很重要的一个关键方案︰ 吊销。 SAS 是一个 URL，因此获得了 SA 人均可以使用它，而不管谁首先请求。 如果 SA 已经公开发布了，可以在世界上的任何人使用它。 SAS 是分布是有效的直到发生以下四件事之一︰

1. 达到指定的 SAS 的到期时间。

2. （如果被引用的存储的访问策略，以及其指定的过期时间） 到达 SA 所引用的存储的访问策略中指定的到期时间。 这既是因为间隔已过期，或您已修改了存储的访问策略在过去，是一种方法撤消 SAS 的过期时间。

3. 所引用的 SAS 存储的访问策略被删除，这是另一种方式，废除了 SA。 请注意，是否您重新创建具有相同的名称存储的访问策略，所有现有的 SAS 标记再次将符合与该存储的访问策略 （假设，尚未通过 SAS 的到期时间） 相关联的权限。 如果打算废除了 SA，请务必使用一个不同的名称，如果不重新创建访问策略与将来的过期时间。

4. 用于创建 SA 帐户密钥重新生成。 请注意，这样将导致使用该帐户密钥失败之前不进行更新以使用另一个有效的帐户密钥或最近重新生成的帐户密码进行身份验证的所有应用程序组件。

> [AZURE.IMPORTANT] 共享的访问签名 URI 相关联使用帐户密钥用于创建该签名，并且关联存储访问策略 （如果有的话）。 如果未不指定任何存储的访问策略，撤消共享的访问签名的唯一办法是更改帐户密码。 

建议您始终使用存储的访问策略，以便您可以撤消签名，也可以根据需要扩展的到期日期。 此文档，请使用中的步骤操作存储访问策略，以生成 SAS。

共享访问签名的详细信息，请参阅[了解 SAS 模型](../storage/storage-dotnet-shared-access-signature-part-1.md)。

##<a name="create-a-stored-policy-and-generate-a-sas"></a>创建一个存储的策略，并生成 SAS

当前必须以编程方式创建存储的策略。 您可以找到 C# 和在[https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)中创建一个存储的策略和 SAS 的 Python 示例。

###<a name="create-a-stored-policy-and-sas-using-c"></a>创建存储的策略和使用 C SA\#

1. 在 Visual Studio 中打开该解决方案。

2. 在解决方案资源管理器中，右击__SASToken__项目并选择__属性__。

3. 选择__设置__，然后添加以下项的值︰

    * StorageConnectionString︰ 您想要创建一个存储的策略和 SAS 的存储帐户连接字符串。 格式应为`DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey`，`myaccount`为您的存储帐户的名称和`mykey`是存储帐户的关键。
    
    * 容器名称︰ 中要限制访问权限的存储帐户的容器。
    
    * SASPolicyName︰ 使用的名称将创建存储策略。
    
    * FileToUpload︰ 将上载到容器的文件的路径。
    
4. 运行该项目。 将显示一个控制台窗口，并生成 SAS 后，将显示类似于以下的信息︰

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    保存的 SAS 策略标记，如与 HDInsight 群集关联的存储帐户时，将会需要它。 您还需要存储帐户名称和容器名称。
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>创建存储的策略和使用 Python 的 SAS

1. 打开 SASToken.py 文件，并更改以下值︰

    * 策略\_名称︰ 用于存储将创建的策略的名称。
    
    * 存储\_帐户\_名称︰ 您的存储帐户的名称。
    
    * 存储\_帐户\_键︰ 存储帐户的键。
    
    * 存储\_容器\_名称︰ 中您想要限制访问权限的存储帐户的容器。
    
    * 示例\_文件\_路径︰ 将上载到的容器的文件的路径

2. 运行该脚本。 脚本完成时，它将显示类似于以下的 SAS 标记︰

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    保存的 SAS 策略标记，如与 HDInsight 群集关联的存储帐户时，将会需要它。 您还需要存储帐户名称和容器名称。
    
##<a name="use-the-sas-with-hdinsight"></a>使用 HDInsight 的 SA

时创建的 HDInsight 群集，您必须指定一个主存储帐户，您可以选择指定附加的存储帐户。 这两种方法添加存储的需要完全访问权限的存储帐户和所使用的容器。

为了使用共享访问签名限制到容器的访问权限，必须对__核心站点__配置为群集添加了自定义项。

* 对于__基于 Windows__或__基于 Linux 的__HDInsight 群集，你可以在使用 PowerShell 创建群集期间。

* 对于__基于 Linux 的__HDInsight 群集，您可以更改后使用 Ambari 创建的群集的配置。

###<a name="create-a-new-cluster-that-uses-the-sas"></a>创建新的群集使用 SAS

创建使用了 SA HDInsight 群集的示例包括在`CreateCluster`存储库的目录。 若要使用它，请使用以下步骤︰

1. 打开`CreateCluster\HDInsightSAS.ps1`文件的文本编辑器中，并修改以下值在文档的开头。

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    例如，将`'mycluster'`为您要创建的群集的名称。 创建存储帐户和 SAS 令牌时，SAS 值应匹配的值从前面的步骤。
    
    一旦您已经更改值，保存该文件。

1. 打开一个新的 Azure PowerShell 提示符。 如果您不熟悉 Azure PowerShell 或未安装，请参阅[安装和配置 Azure PowerShell][powershell]。

2. 在提示符下，使用以下方法来验证到 Azure 订购︰

        Login-AzureRmAccount
    
    当系统提示您使用 Azure 订阅的帐户登录。
    
    如果您的登录信息与多个 Azure 的订阅，您可能需要使用`Select-AzureRmSubscription`来选择您想要使用的订阅。

2. 在提示符下，将目录改为`CreateCluster`的 HDInsightSAS.ps1 文件所在的目录。 然后使用以下方法来运行脚本
        
        .\HDInsightSAS.ps1
    
    在脚本运行时，它将输出记录到 PowerShell 提示作为创建资源组和存储的帐户。 然后将提示您输入 HTTP 用户 HDInsight 群集。 这是用来安全 HTTP/s 访问群集的用户帐户。
    
    如果您要创建一个基于 Linux 的群集，将还提示输入 SSH 用户帐户名和密码。 这使用远程登录到该群集。
    
    > [AZURE.IMPORTANT] 当系统提示您输入 HTTP/s 或 SSH 用户名称和密码，您必须提供密码符合下列条件︰
    >
    > - 必须至少为 10 个字符长
    > - 必须包含至少一个数字
    > - 必须包含至少一个非字母数字字符
    > - 必须包含至少一大写或小写字母


它需要一段时间为此脚本来完成，通常大约 15 分钟。 当脚本完成后没有任何错误时，已创建群集。

###<a name="update-an-existing-cluster-to-use-the-sas"></a>更新现有群集使用 SAS

如果您有一个现有的基于 Linux 的群集时，可以通过使用以下步骤将 SAS 添加到__核心站点__配置︰

1. 打开群集 Ambari web 用户界面。 此页的地址是 https://YOURCLUSTERNAME.azurehdinsight.net。 出现提示时，通过身份验证群集使用的管理名称 （管理员） 和在创建群集时所用的密码。

2. 从 Ambari web 用户界面的左侧， __HDFS__中选择，然后选择__配置__选项卡页的中间。

3. 选择__高级__选项卡，然后滚动直到找到__自定义网站用户核心__部分。

4. 展开__自定义核心网站__部分，然后滚动到末尾并选择__添加属性...__链接。 __键__和__值__字段中使用以下值︰

    * __键__︰ fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __值__︰ SAS 先前运行 C# 或 Python 应用程序返回
    
    __容器名称__替换与 C# 或 SAS 应用程序一起使用的容器名称。 您使用的存储帐户名称替换__STORAGEACCOUNTNAME__ 。

5. 单击__添加__按钮以保存此键和值，然后单击__保存__按钮以保存配置更改。 出现提示时，添加 （"添加 SAS 存储访问"等） 的更改的说明，然后单击__保存__。

    完成更改后，请单击__确定__。

    > [AZURE.IMPORTANT] 这将保存配置更改，但更改生效之前，您必须重新启动多个服务。

6. 在 Ambari web 用户界面中，从左侧列表中选择__HDFS__ ，然后从__服务操作__下拉列表右侧的列表中选择__重新启动所有__。 出现提示时，选择__开启维护模式__，然后选择 __Conform 重新启动所有"。

    在该页的左侧列表中的 MapReduce2 和 YARN 项重复此过程。

7. 一旦这些已经重新启动，请选择每个并禁用的__服务操作__维护模式下拉列表中。

##<a name="test-restricted-access"></a>测试受限制的访问

若要验证限制访问，请使用以下方法︰

* 对于__基于 Windows 的__HDInsight 群集，使用远程桌面连接到群集。 有关更多信息，请参见[使用 RDP 的 HDInsight 到 Connecto](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) 。

    一旦连接， __Hadoop 命令行__图标在桌面上使用打开命令提示符窗口。

* 对于__基于 Linux 的__HDInsight 群集，使用 SSH 连接到群集。 使用基于 Linux 的群集使用 SSH 上看到以下信息之一︰

    * [HDInsight 从 Linux、 OS X 和 Unix 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)
    
一旦连接到群集，使用以下步骤验证您可以在 SAS 存储帐户上的只有读和列表项︰

1. 在提示符下，键入以下命令。 替换为 SAS 存储帐户创建的容器的名称为__SASCONTAINER__ 。 __SASACCOUNTNAME__替换用于 SAS 存储帐户的名称︰

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    这将列出容器，其中应包括已上载的文件的内容创建的容器和 SAS 时。
    
2. 使用以下方法来验证您可以读取该文件的内容。 按照以上步骤替换的__SASCONTAINER__和__SASACCOUNTNAME__ 。 在上述命令中显示的文件的名称替换__文件名__︰

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    这将列出该文件的内容。
    
3. 使用以下方法来将文件下载到本地文件系统︰

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    这会将文件下载到本地文件__就绪__。

4. 使用以下方法将本地文件上载到名为__testupload.txt__的 SAS 存储新文件︰

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    您将收到类似于以下的消息︰
    
        put: java.io.IOException
        
    由于存储位置是读 + 仅列表，将发生此错误。 使用以下方法来将数据放在群集中，它是可写的默认存储︰
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    这一次，应成功完成该操作。
    
##<a name="troubleshooting"></a>故障排除

###<a name="a-task-was-canceled"></a>任务已被取消

__症状__︰ 在创建群集使用 PowerShell 的脚本时，您可能会收到以下错误消息︰

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__原因__︰ 如果群集，或 （对于基于 Linux 的群集） 的管理/HTTP 用户使用密码，则会出现此错误的 SSH 用户。

__解决方法__︰ 使用密码符合下列条件︰

- 必须至少为 10 个字符长
- 必须包含至少一个数字
- 必须包含至少一个非字母数字字符
- 必须包含至少一大写或小写字母

##<a name="next-steps"></a>下一步行动

既然您已了解如何将限制访问存储添加到 HDInsight 群集，请在群集上使用数据的其他方法︰

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)

* [使用 HDInsight 的小猪](hdinsight-use-pig.md)

* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
