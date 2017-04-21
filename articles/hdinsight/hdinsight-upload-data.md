<properties
    pageTitle="上载数据的 Hadoop 作业中 HDInsight |Microsoft Azure"
    description="了解如何上载并访问数据，Hadoop 作业中使用 Azure CLI、 Azure 存储浏览器，Azure PowerShell，Hadoop 命令行或 Sqoop 的 HDInsight。"
    services="hdinsight,storage"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>上载数据中 HDInsight 的 Hadoop 作业

Azure HDInsight 通过 Azure Blob 存储提供齐全 Hadoop 分布式文件系统 (HDFS)。 它旨在作为 HDFS 扩展为客户提供无缝体验。 它使直接在它所管理的数据上运行 Hadoop 生态系统中的组件的完整集。 Azure 的 Blob 存储和 HDFS 是经过优化的存储数据和计算对该数据的不同的文件系统。 使用 Azure Blob 存储的好处的信息，请参阅[使用 Azure Blob 存储与 HDInsight][hdinsight-storage]。

**系统必备组件**

在开始之前，请注意以下要求︰

* Azure HDInsight 群集。 有关说明，请参阅[开始使用 Azure HDInsight] [hdinsight-get-started]或[群集调配 HDInsight][hdinsight-provision]。

##<a name="why-blob-storage"></a>为什么 blob 存储？

Azure HDInsight 群集通常部署运行 MapReduce 作业，这些作业完成后丢弃群集。 在 HDFS 中保持数据簇计算完成之后会存储该数据的代价。 是 azure Blob 存储的高可用性、 高可扩展性、 高容量、 低成本、 可共享的存储选项是使用 HDInsight 进行处理的数据。 将数据存储在一个 blob 启用 HDInsight 群集用于计算可以被安全地释放而不会丢失数据。

###<a name="directories"></a>目录

Azure Blob 存储容器的键/值对的形式存储数据，并且没有任何目录层次结构。 但是"/"字符可以用于中的键名，使它看起来就像文件存储在目录结构中。 HDInsight 看到这些就像他们是实际的目录。

例如，某个 blob 密钥可能*input/log1.txt*。 没有实际的"输入"目录已存在，但由于"/"字符的键名存在，有文件路径的外观。

因此，如果您使用 Azure 资源管理器工具可能会注意到一些 0 字节的文件。 这些文件有两个用途︰

- 如果空文件夹，它们标记的文件夹存在。 Azure Blob 存储是足够聪明，知道如果存在名为 foo/条斑点，还有名为**foo**的文件夹。 但表示名为**foo**的空文件夹的唯一办法是通过在地方让此特殊 0 字节的文件。

- 它们保存特定元数据所需的 Hadoop 文件系统，值得注意的是权限和文件夹的所有者。

##<a name="command-line-utilities"></a>命令行实用程序

Microsoft 提供了使用 Azure Blob 存储以下实用程序︰

| 工具 | Linux | OS X | 窗口 |
| ---- |:-----:|:----:|:-------:|
| [Azure 的命令行界面][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop 命令](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] 虽然所有可以在 Azure CLI、 Azure PowerShell 和 AzCopy 可用于从外部 Azure，Hadoop 命令才可用 HDInsight 群集上，并只允许将数据从本地文件系统加载到 Azure Blob 存储。

###<a id="xplatcli"></a>Azure CLI

Azure CLI 可以允许您管理 Azure 服务的跨平台工具。 使用以下步骤将数据上载到 Azure Blob 存储︰

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [安装和配置 Mac、 Linux 和 Windows Azure CLI](../xplat-cli-install.md)。

2. 打开命令提示符，bash 或其他 shell 中，并使用以下方法来验证到 Azure 订购。

        azure login

    出现提示时，输入的用户名称和密码为您的订阅。

3. 输入以下命令以列出您的订阅的存储帐户︰

        azure storage account list

4. 选择包含您想要使用的 blob 存储帐户，然后使用下面的命令以检索此帐户密钥︰

        azure storage account keys list <storage-account-name>

    这应返回**主**、**辅**密钥。 因为在接下来的步骤中将使用它，将复制的**主**键值。

5. 使用下面的命令来检索 blob 容器中存储帐户的列表︰

        azure storage container list -a <storage-account-name> -k <primary-key>

6. 使用以下命令上载和下载文件至 blob:

    * 若要上载的文件︰

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * 若要下载文件︰

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 如果您始终会使用相同的存储帐户，可以设置以下环境变量而不是指定的帐户和每个命令键︰
>
> * **AZURE\_存储\_帐户**︰ 存储帐户名称
>
> * **AZURE\_存储\_访问\_键**︰ 存储帐户密码

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell 是一种可用于控制和自动化的部署和管理您的工作负载在 Azure 中的脚本环境。 有关将工作站配置为运行 Azure PowerShell 的信息，请参阅[安装和配置 Azure PowerShell](../powershell-install-configure.md)。

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**要将本地文件上载到 Azure Blob 存储**

1. 中说明的那样打开 Azure PowerShell 控制台[安装和配置 Azure PowerShell](../powershell-install-configure.md)。
2. 在以下脚本中设置前五个变量的值︰

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. 将脚本粘贴到 Azure PowerShell 控制台可以运行它来复制该文件。

例如，PowerShell 脚本创建使用 HDInsight，请参阅[HDInsight 工具](https://github.com/blackmist/hdinsight-tools)。

###<a id="azcopy"></a>AzCopy

AzCopy 是一个命令行工具，它可以简化这一任务将数据传入与传出 Azure 存储帐户。 您可以使用它作为一个独立的工具或并入现有的应用程序使用此工具。 [下载 AzCopy][azure-azcopy-download]。

AzCopy 的语法是︰

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

有关详细信息，请参阅[AzCopy-上传/下载文件的 Azure Blob][azure-azcopy]。


###<a id="commandline"></a>Hadoop 命令行

Hadoop 命令行是只适用于已存在群集的头节点上数据时将数据存储到 blob 存储。

为了使用 Hadoop 命令，您必须首先连接到 headnode，使用下列方法之一︰

* **基于 Windows 的 HDInsight**︰[使用远程桌面连接](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **基于 linux * 的 HDInsight**︰ 使用 SSH （[SSH 命令](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)或[PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)） 连接

建立连接后，可以使用下面的语法将文件上载到的存储。

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

例如，`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

由于 HDInsight 的默认文件系统是 Azure Blob 存储中，则 /example/data.txt 实际是在 Azure Blob 存储。 您可以参考该文件为︰

    wasbs:///example/data/data.txt

或

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

其他 Hadoop 命令的处理文件的列表，请参见[http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] 在 HBase 群集默认阻止写入数据为 256 KB 时使用的大小。 虽然这可以正常使用 HBase Api 或 REST Api 时，使用`hadoop`或`hdfs dfs`命令写入大于 ~ 12 GB 的数据会导致错误。 请参阅下面的详细信息[写入 blob 存储异常](#storageexception)一节。

##<a name="graphical-clients"></a>图形化客户端

也有几个使用 Azure 存储提供一个图形界面的应用程序。 以下是几个这些应用程序的列表︰

| 客户端 | Linux | OS X | 窗口 |
| ------ |:-----:|:----:|:-------:|
| [HDInsight 的 Microsoft Visual Studio 工具](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure 存储资源管理器](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [云存储 Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure 的资源管理器](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>HDInsight 的 visual Studio 工具

有关详细信息，请参阅[导航链接的资源](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources)。

###<a id="storageexplorer"></a>Azure 存储资源管理器

*Azure 存储浏览器*是一个有用的工具，用于检查和修改 blob 中的数据。 它是一种免费的开源工具，可以从[http://storageexplorer.com/](http://storageexplorer.com/)下载。 此链接也提供了源代码。

之前使用的工具，您必须知道 Azure 存储帐户用户名和帐户密码。 有关获取此信息的说明，请参见"如何︰ 查看、 复制和重新生成存储访问密钥"部分中[创建、 管理或删除存储帐户]的[azure-create-storage-account]。  

1. 运行 Azure 存储资源管理器。 如果这是您第一次运行存储资源管理器中，您将会提示输入 ___存储帐户名称__和__存储帐户密钥__。 如果您有运行之前，使用__添加__按钮以添加新的存储帐户名称和密钥。

    输入的名称和密钥存储帐户使用 HDinsight 群集，然后选择__保存并打开__。

    ![HDI。AzureStorageExplorer][image-azure-storage-explorer]

5. 在容器的界面左侧的列表中，单击与 HDInsight 群集关联的容器的名称。 默认情况下，这是 HDInsight 群集的名称，但如果您输入一个特定的名称在创建群集时可能会不同。

6. 从工具栏上，选择上载图标。

    ![与上载图标突出显示工具条](./media/hdinsight-upload-data/toolbar.png)

7. 指定要上载的文件，然后单击**打开**。 出现提示时，选择__上载__将文件上载到的存储容器根。 如果您想要将文件上载到指定路径，在__目标__字段中输入路径，然后选择__上载__。

    ![文件上载对话框](./media/hdinsight-upload-data/fileupload.png)
    
    一旦文件完成上载后，您可以使用它从 HDInsight 群集上的作业。

##<a name="mount-azure-blob-storage-as-local-drive"></a>装载 Azure Blob 存储为本地驱动器

[装载 Azure Blob 存储为本地驱动器](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)，请参阅。

##<a name="services"></a>服务

###<a name="azure-data-factory"></a>Azure 数据工厂

Azure 数据工厂服务是完全托管的服务的简单、 可扩展且可靠的数据生产管线到撰写数据存储、 数据处理和数据移动服务。

可以使用 azure 数据工厂，将数据移到 Azure Blob 存储或创建数据管道直接使用 HDInsight 功能，如配置单元和小猪。

有关详细信息，请参阅[Azure 数据工厂文档](https://azure.microsoft.com/documentation/services/data-factory/)。

###<a id="sqoop"></a>Apache Sqoop

Sqoop 是旨在 Hadoop 和关系数据库之间传输数据的工具。 可用于将数据导入关系数据库管理系统 (RDBMS)，如 SQL Server、 MySQL 或到 Hadoop 分布式文件系统 (HDFS) 的 Oracle 转变 Hadoop 使用 MapReduce 或配置单元时中的数据并将数据导出到 RDBMS。

有关详细信息，请参阅[使用 Sqoop 与 HDInsight][hdinsight-use-sqoop]。

##<a name="development-sdks"></a>开发 Sdk

也可以使用以下编程语言从 Azure SDK 访问 azure Blob 存储︰

* .NET
* Java
* Node.js
* PHP
* Python
* 拼音

Azure Sdk 安装的详细信息，请参阅[Azure 下载](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>故障排除

### <a id="storageexception"></a>对 blob 写入的存储异常

__症状__︰ 使用时`hadoop`或`hdfs dfs`命令写入 ~ 12 gb 的文件或大在 HBase 群集上，则可能会遇到以下错误︰ 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__原因__︰ HDInsight 在 HBase 在 Azure 存储中写入时的块大小为 256 KB 到群集默认。 尽管 HBase 的 Api 或 REST Api，此方法有效，它将导致错误时使用`hadoop`或`hdfs dfs`命令行实用程序。

__解决方法__︰ 使用`fs.azure.write.request.size`指定较大的块大小。 你可以在每次使用的基础上通过使用`-D`参数。 下面是一个示例使用参数`hadoop`命令︰

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

您还可以增加的值`fs.azure.write.request.size`使用 Ambari 的全局。 可以使用下列步骤来更改 Ambari Web 用户界面中的值︰

1. 在浏览器中，转到您的群集 Ambari Web 用户界面。 这是 https://CLUSTERNAME.azurehdinsight.net，其中__群集名称__是该群集的名称。

    出现提示时，输入对群集的管理员名称和密码。

2. 从屏幕的左侧，选择__HDFS__，然后选择__配置__选项卡。

3. 在__筛选器__字段中，输入`fs.azure.write.request.size`。 这将显示字段，在页上的当前值。

4. 值从 262144 (256 KB) 更改为新值。 例如，4194304 (4 MB)。

![更改的值通过 Ambari Web 用户界面的图像](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

使用 Ambari 的详细信息，请参阅[管理 HDInsight 群集使用 Ambari Web 用户界面](hdinsight-hadoop-manage-ambari.md)。

## <a name="next-steps"></a>下一步行动

现在，您已了解如何将数据导入 HDInsight，请阅读以下文章以了解如何进行分析︰

* [开始使用 Azure HDInsight][hdinsight-get-started]
* [以编程方式提交 Hadoop 作业][hdinsight-submit-jobs]
* [使用 HDInsight 配置单元][hdinsight-use-hive]
* [使用 HDInsight 的小猪][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
