<properties
    pageTitle="分析与 Hadoop 在 HDInsight 航班延迟数据 |Microsoft Azure"
    description="了解如何使用一个 Windows PowerShell 脚本来创建一个 HDInsight 群集、 运行配置单元作业、 运行 Sqoop 作业，和删除群集。"
    services="hdinsight"
    documentationCenter=""
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>使用 HDInsight 中的配置单元分析航班延迟数据

配置单元提供了一种通过调用类似 SQL 的脚本语言运行 Hadoop MapReduce 作业*[HiveQL][hadoop-hiveql]*，可向汇总、 查询和分析大量数据的应用。

> [AZURE.NOTE] 本文档中的步骤要求基于 Windows HDInsight 群集。 使用基于 Linux 的群集的步骤，请参阅[通过使用 HDInsight (Linux) 中的配置单元的分析航班延迟数据](hdinsight-analyze-flight-delay-data-linux.md)。

Azure HDInsight 的主要好处之一是数据存储和计算的分离。 HDInsight 使用 Azure Blob 存储的数据存储。 一个典型的作业包括三个部分︰

1. **在 Azure Blob 存储中存储的数据。**  例如，天气数据、 传感器数据、 web 日志，并在这种情况下，航班延迟数据保存到 Azure Blob 存储。
2. **运行作业。** 在处理数据时，您运行 Windows PowerShell 脚本 （或客户端应用程序） 来创建一个 HDInsight 群集、 运行作业，以及删除群集。 这些作业将输出数据保存到 Azure Blob 存储。 删除群集的情况下，即使保留的输出数据。 这样一来，您支付只什么您已用完。
3. **检索从 Azure Blob 存储输出**，或在本教程中，将数据导出到 SQL Azure 数据库。

下图说明了方案和本指南的结构︰

![HDI。FlightDelays.flow][img-hdi-flightdelays-flow]

**注**︰ 图中的数字对应于分区标题。 **M**代表主进程。 **A**代表附录中的内容。

本教程的主要部分显示了如何使用一个 Windows PowerShell 脚本来执行以下任务︰

- 创建一个 HDInsight 的群集。
- 计算平均延迟机场在群集上运行配置单元作业。 航班延迟数据存储在 Azure Blob 存储帐户。
- 运行导出到 SQL Azure 数据库配置单元作业输出 Sqoop 作业。
- 删除 HDInsight 群集。

在附录中，您可以找到航班延迟数据上载、 创建/上载配置单元查询字符串和 SQL Azure 数据库准备 Sqoop 作业说明。

> [AZURE.NOTE] 此文档中的步骤适用于基于 Windows HDInsight 群集。 使用基于 Linux 的群集的步骤，请参阅[使用 HDInsight (Linux) 中的配置单元分析航班延迟数据](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下各项︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**在本教程中使用的文件**

本教程使用航空飞行数据的[研究和创新的技术管理、 运输统计信息局或莉塔]上时性能 [rita-website]。 一份数据已上载到 Azure Blob 存储容器使用公钥 Blob 的访问权限。 PowerShell 脚本的一部分将数据复制到群集中的默认 blob 容器公钥 blob 容器。 HiveQL 脚本也被复制到同一个 Blob 容器中。 如果您想要了解如何获取/上载的数据到存储您自己的帐户，以及如何创建/上载 HiveQL 脚本文件，请参阅[附录 A](#appendix-a)和[附录 B](#appendix-b)。

下表列出了在本教程中使用的文件︰

<table border="1">
<tr><th>文件</th><th>说明</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>配置单元作业使用 HiveQL 脚本文件。 此脚本已上载到 Azure Blob 存储客户，与公共访问权限。 <a href="#appendix-b">附录 B</a>有准备，并将该文件上载到 Azure Blob 存储帐户的说明。</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>该配置单元作业的的输入的数据。 将数据上载到 Azure Blob 存储客户，与公共访问权限。 <a href="#appendix-a">附录 A</a>具有获取数据并将数据上载到 Azure Blob 存储帐户的说明。</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>配置单元作业的输出路径。 默认容器用于存储输出数据。</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>配置单元作业状态的默认容器上的文件夹。</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>创建群集和运行配置单元/Sqoop 作业

Hadoop MapReduce 是批处理。 运行配置单元作业最经济高效的方法是创建群集的作业，并在工作完成后删除该作业。 下面的脚本介绍了整个过程。 创建一个 HDInsight 群集和正在运行的配置单元作业的详细信息，请参阅[创建 Hadoop 群集中 HDInsight] [hdinsight-provision]并[使用配置单元与 HDInsight][hdinsight-use-hive]。

**通过 Azure PowerShell 运行配置单元查询**

1. [附录 C](#appendix-c)中使用的说明创建一个 SQL Azure 数据库和 Sqoop 作业输出表。
3. 打开 Windows PowerShell ISE，并运行下面的脚本︰

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. 连接到 SQL 数据库，请参阅按 AvgDelays 表中的市/县平均航班延迟︰

    ![HDI。FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>附录 A-上载航班延迟到 Azure Blob 存储的数据
上载数据文件和 HiveQL 脚本文件 （见[附录 B](#appendix-b)），则需要进行一些规划。 其目的是用于存储数据文件和 HiveQL 文件之前创建一个 HDInsight 群集和运行配置单元作业。 有两种选择︰

- **使用相同的 Azure 存储帐户将用作默认的文件系统由 HDInsight 群集。** 由于 HDInsight 群集将存储帐户访问键，您不需要进行任何其他更改。
- **使用 HDInsight 群集默认文件系统中的其他 Azure 存储帐户。** 如果出现这种情况，您必须修改 Windows PowerShell 脚本中[创建的 HDInsight 群集和运行配置单元/Sqoop 作业](#runjob)链接作为一个附加的存储帐户的存储帐户的创建部件。 有关说明，请参阅[创建 Hadoop 群集中 HDInsight][hdinsight-provision]。 HDInsight 群集然后知道存储帐户的访问键。

>[AZURE.NOTE] 数据文件的 Blob 存储路径进行硬编码 HiveQL 脚本文件中。 您必须相应地更新它。

**若要下载飞行数据**

1. 浏览到[研究和创新的技术管理，局运输统计的][rita-website]。
2. 在页上，选择以下值︰

    <table border="1">
    <tr><th>名称</th><th>值</th></tr>
    <tr><td>筛选器年</td><td>2013 </td></tr>
    <tr><td>过滤周期</td><td>1 月</td></tr>
    <tr><td>字段</td><td>*年*， *FlightDate*、 *UniqueCarrier*、*承运人*、 *FlightNum*、 *OriginAirportID*、*来源*、 *OriginCityName*、 *OriginState*、 *DestAirportID*、*目标*、 *DestCityName*、 *DestState*、 *DepDelayMinutes*、 *ArrDelay*、 *ArrDelayMinutes*、 *CarrierDelay*、 *WeatherDelay*、 *NASDelay*、 *SecurityDelay*、 *LateAircraftDelay* （清除所有其他字段）</td></tr>
    </table>

3. 单击**下载**。
4. 请将它解压缩到**C:\Tutorials\FlightDelay\2013Data**文件夹。 每个文件是一个 CSV 文件，大约为 60 GB 的大小。
5.  文件重命名为它包含的数据的月份名称。 例如，包含 1 月数据的文件将被命名为*January.csv*。
6. 重复步骤 2 和 5 在 2013年在 12 个月的每个下载文件。 您将需要至少一个文件以运行该教程。  

**若要将航班延迟数据上载到 Azure Blob 存储**

1. 准备参数︰

    <table border="1">
    <tr><th>变量名称</th><th>备注</th></tr>
    <tr><td>$storageAccountName</td><td>想要上载的数据到 Azure 存储帐户。</td></tr>
    <tr><td>$blobContainerName</td><td>想要上载的数据 Blob 容器。</td></tr>
    </table>
2. 打开 Azure PowerShell ISE。
3. 将下面的脚本粘贴到脚本窗格中︰

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. 按**f5 键**以运行该脚本。

如果您选择使用不同的方法，用于将文件上载，请确保文件路径是教程/flightdelay/数据。 访问这些文件的语法如下︰

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

路径教程/flightdelay/数据是您上载的文件时所创建的虚拟文件夹。 验证存在 12 文件，每个月一个。

>[AZURE.NOTE] 您必须更新该配置单元查询以从新位置中读取。

> 或者，您必须配置容器的访问权限是公共的或绑定到 HDInsight 群集的存储帐户。 否则，配置单元查询字符串将不能访问数据文件。

---
##<a id="appendix-b"></a>附录 B-创建并上载一个 HiveQL 脚本

使用 Azure PowerShell，可以一次运行多个 HiveQL 语句一或打包到一个脚本文件的 HiveQL 语句。 本节演示如何创建一个 HiveQL 脚本，将脚本上传到 Azure Blob 存储通过使用 Azure PowerShell。 配置单元需要的 HiveQL 脚本存储在 Azure Blob 存储。

HiveQL 脚本将执行以下︰

1. **删除 delays_raw 表**，以防万一的表已存在。
2. **创建 delays_raw 外部配置单元表**与航班延迟文件指向 Blob 存储位置。 此查询指定字段分隔的"，"和"\n"，终止线。 这产生了一个问题，当字段值包含逗号，因为配置单元无法区分字段分隔符是逗号和一个字段值的一部分 (这是这种情况在字段值来源的\_城市\_名称和目标\_城市\_名称)。 若要解决此问题，该查询创建 TEMP 列以容纳错误地拆分为列的数据。  
3. **删除延迟表**，以防万一的表已存在。
4. **创建延迟表**。 最好先清除数据，然后再进一步处理。 该查询创建新表，*延迟*，从 delays_raw 表。 请注意，温度 （如上文所述） 不会被复制，并且**子字符串**函数用来从数据中删除引号。
5. **按市/县名称计算平均天气延迟并对结果进行分组。** 它还将输出到 Blob 存储结果。 请注意，该查询将从数据中删除撇号将排除的行**weather_delay**的值为 null 的情况。 因为 Sqoop，以后在本教程中，使用不适当处理这些值，默认情况下，这是必要的。

HiveQL 命令的完整列表，请参阅[配置单元数据定义语言][hadoop-hiveql]。 每个 HiveQL 命令必须用分号终止。

**若要创建一个 HiveQL 脚本文件**

1. 准备参数︰

    <table border="1">
    <tr><th>变量名称</th><th>备注</th></tr>
    <tr><td>$storageAccountName</td><td>想要传到的 HiveQL 脚本 Azure 存储帐户。</td></tr>
    <tr><td>$blobContainerName</td><td>想要传到的 HiveQL 脚本 Blob 容器。</td></tr>
    </table>
2. 打开 Azure PowerShell ISE。

3. 复制并粘贴到脚本窗格下面的脚本︰

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    以下是该脚本中使用的变量︰

    - **$hqlLocalFileName** -该脚本将保存 HiveQL 脚本文件本地之前将其上载到 Blob 存储。 这是文件的名称。 默认值是<u>C:\tutorials\flightdelay\flightdelays.hql</u>。
    - **$hqlBlobName** -这是 Azure Blob 存储中使用的 HiveQL 脚本文件 blob 名称。 默认值是 tutorials/flightdelay/flightdelays.hql。 该文件将直接写入 Azure Blob 存储，因为没有"/"开头的 blob 名称。 如果您想要访问 Blob 存储中的该文件，您需要添加"/"开头的文件名。
    - **$srcDataFolder**和**$dstDataFolder** -="flightdelay/教程/数据"="flightdelay/教程/输出"


---
##<a id="appendix-c"></a>附录 C-Sqoop 作业输出为准备 SQL Azure 数据库
**若要准备 SQL 数据库 （这与合并 Sqoop 脚本）**

1. 准备参数︰

    <table border="1">
    <tr><th>变量名称</th><th>备注</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>SQL Azure 数据库服务器的名称。 输入执行任何操作以创建新的服务器。</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>SQL Azure 数据库服务器登录名。 如果 $sqlDatabaseServerName 是现有服务器，登录名和登录密码用于与服务器进行身份验证。 否则它们用于创建新的服务器。</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>SQL Azure 数据库服务器登录密码。</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>仅当您要创建新的 Azure 数据库服务器，则使用此值。</td></tr>
    <tr><td>$sqlDatabaseName</td><td>用于创建 Sqoop 作业的 AvgDelays 表的 SQL 数据库。 保留为空将创建名为 HDISqoop 的数据库。 Sqoop 作业输出表名称是 AvgDelays。 </td></tr>
    </table>
2. 打开 Azure PowerShell ISE。
3. 复制并粘贴到脚本窗格下面的脚本︰

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] 该脚本使用一种具象状态传输 (REST) 服务，http://bot.whatismyipaddress.com，以检索您的外部 IP 地址。 IP 地址用于创建您的 SQL 数据库服务器的防火墙规则。  

    下面是一些脚本中使用的变量︰

    - **$ipAddressRestService** -默认值是 http://bot.whatismyipaddress.com。 它是一个公用 IP 地址用于获取您的外部 IP 地址的其余部分服务。 如果需要，您可以使用其他服务。 通过此服务检索到的外部 IP 地址将用于创建您的 SQL Azure 数据库服务器，防火墙规则，以便可以从工作站中访问数据库 （通过使用 Windows PowerShell 脚本）。
    - **$fireWallRuleName** -这是 SQL Azure 数据库服务器防火墙规则的名称。 默认名称是<u>FlightDelay</u>。 如果需要，可以重它。
    - 仅当您要创建新的 SQL Azure 数据库服务器使用**$sqlDatabaseMaxSizeGB** -此值。 默认值为 10 GB。 10 GB 足以满足本教程。
    - 仅当您要创建一个新的 SQL Azure 数据库时使用**$sqlDatabaseName** -此值。 默认值是 HDISqoop。 如果您重命名它，则必须相应更新 Sqoop Windows PowerShell 脚本。

4. 按**f5 键**以运行该脚本。
5. 验证脚本输出。 确保该脚本已成功运行。

##<a id="nextsteps"></a>下一步行动
现在您了解了如何将文件上载到 Azure Blob 存储、 如何使用 Azure Blob 存储中的数据填充配置单元表、 如何运行配置单元查询和如何使用 Sqoop 来 HDFS 数据导出到 SQL Azure 数据库。 若要了解详细信息，请参阅下列文章︰

* [要开始使用 HDInsight][hdinsight-get-started]
* [使用 HDInsight 配置单元][hdinsight-use-hive]
* [HDInsight 使用 Oozie][hdinsight-use-oozie]
* [使用 HDInsight Sqoop][hdinsight-use-sqoop]
* [使用 HDInsight 的小猪][hdinsight-use-pig]
* [HDInsight 为开发 Java MapReduce 程序][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
