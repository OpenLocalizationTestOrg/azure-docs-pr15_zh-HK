<properties
    pageTitle="在 HDInsight 中使用 Hadoop Sqoop |Microsoft Azure"
    description="了解如何从工作站使用 Azure PowerShell 运行 Sqoop 的导入和导出 Hadoop 群集 SQL Azure 数据库之间。"
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>使用 Hadoop 在 HDInsight Sqoop

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何在 HDInsight 中使用 Sqoop 来导入和导出 HDInsight 群集和 SQL Azure 数据库或 SQL Server 数据库之间。

尽管 Hadoop 是一个自然的选择，用于处理非结构化和 semistructured 数据，例如日志文件和文件，也可能需要处理结构化的数据存储在关系数据库中。

[Sqoop] [sqoop-user-guide-1.4.4]是旨在 Hadoop 群集和关系数据库之间传输数据的工具。 您可以使用它从关系数据库管理系统 (RDBMS) 导入数据，如 SQL Server、 MySQL 或到 Hadoop 分布式文件系统 (HDFS) 的 Oracle 转变 Hadoop 使用 MapReduce 或配置单元时中的数据并将数据导出到 RDBMS。 在本教程中，您为您的关系数据库使用 SQL Server 数据库。

Sqoop 在 HDInsight 群集受支持的版本，请参阅[由 HDInsight 提供的群集版本中的新增功能？][hdinsight-versions].

##<a name="understand-the-scenario"></a>了解该方案

HDInsight 群集提供了一些示例数据。 您将使用下列两个示例︰

- Log4j 日志文件，位于*/example/data/sample.log*。 以下日志文件中提取︰

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- 一个名为*hivesampletable*，其中引用了位于*/hive/warehouse/hivesampletable*的数据文件配置单元表。 表中包含某些移动设备数据。 

  	| 字段 | 数据类型 |
  	| ----- | --------- |
  	| 客户机 id | 字符串 |
  	| querytime | 字符串 |
  	| 市场 | 字符串 |
  	| deviceplatform | 字符串 |
  	| devicemake | 字符串 |
  	| devicemodel | 字符串 |
  	| 状态 | 字符串 |
  	| 国家/地区 | 字符串 |
  	| querydwelltime | 双精度 |
  	| 会话标识符 | bigint |
  	| sessionpagevieworder | bigint |

将到 SQL Azure 数据库或 SQL Server 首先导出*sample.log*和*hivesampletable* ，并使用以下路径包含移动设备数据回发到 HDInsight 的表，然后将导入︰

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>创建群集和 SQL 数据库

本节演示如何创建一个群集和 SQL 数据库架构，以便运行该教程使用 Azure 的门户和 Azure 资源管理器模板。  如果您喜欢使用 Azure PowerShell，请参阅[附录 A](#appendix-a---a-powershell-sample)。

1. 请单击以下图像在 Azure 门户打开资源管理器模板。         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    资源管理器模板位于*https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*的公钥 blob 容器中。 
    
    资源管理器模板调用 bacpac 软件包部署到 SQL 数据库的表架构。  Bacpac 包同样位于 https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac 的公钥 blob 容器。 如果您想要使用专用容器的 bacpac 文件，请在模板中使用以下值︰
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. 从参数刀片式服务器，输入以下命令︰

    - **群集名称**︰ 输入，您将创建的 Hadoop 群集的名称。
    - **群集登录名和密码**︰ 默认登录名是管理员。
    - **SSH 用户名称和密码**。
    - **SQL 数据库服务器登录名和密码**。

    下列值是硬编码的变量中︰
    
  	|默认的存储帐户名|<CluterName>存储区|
  	|----------------------------|-----------------|
  	|Azure SQL 数据库服务器名称|<ClusterName>dbserver|
  	|Azure 的 SQL 数据库名称|<ClusterName>db|
    
    请记下这些值。  在本教程后面部分，将需要它们。
    
3.单击**确定**以保存参数。

4.从**自定义部署**刀片式服务器，**资源组**下拉框中，单击，然后单击**新建**以创建新的资源组。 资源组是群集、 从属存储帐户和其他链接的资源进行分组的容器。

5.单击**法律条款**，，然后单击**创建**。

6.单击**创建**。 您将看到标题为提交部署模板部署新的拼贴。 大约需要大约 20 分钟的时间来创建群集和 SQL 数据库。

如果您选择使用现有的 SQL Azure 数据库或 Microsoft SQL Server

- **SQL azure 数据库**︰ 您必须配置防火墙规则以允许从您的工作站访问 SQL Azure 数据库服务器。 说明如何创建 Azure SQL 数据库并配置防火墙，请参阅[开始使用 SQL Azure 数据库][sqldatabase-get-started]。 

    > [AZURE.NOTE] 默认情况下 SQL Azure 数据库允许从 Azure 服务，例如 Azure HDInsight 的连接。 如果禁用该防火墙设置，则必须启用它从 Azure 的门户。 有关创建 SQL Azure 数据库和配置防火墙规则的说明，请参阅[创建和配置 SQL 数据库][sqldatabase-create-configue]。

- **SQL Server**︰ 如果 HDInsight 群集所在的虚拟网络在 Azure 中的 SQL Server 上，您可以使用步骤本文中导入和导出到 SQL Server 数据库的数据。

    > [AZURE.NOTE] HDInsight 支持仅基于位置的虚拟网络，并且它目前不能使用基于相似性组的虚拟网络。

    * 若要创建和配置虚拟网络，请参阅[创建虚拟网络使用 Azure 的门户](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)。

        * 当您在您的数据中心中使用 SQL Server 时，则必须为*站点到站点*或*点到站点*配置虚拟网络。

            > [AZURE.NOTE] 为虚拟网络中**点到网站**，SQL Server 必须在 VPN 客户端配置的应用程序，可从 Azure 的虚拟网络配置**仪表板**。

        * 当您使用 SQL Server Azure 的虚拟机上时，任何虚拟网络配置可承载 SQL Server 虚拟机是否与 HDInsight 相同的虚拟网络的成员。

    * 若要创建虚拟网络 HDInsight 群集，请参阅[创建 Hadoop 群集中使用的自定义选项的 HDInsight](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server 还允许进行身份验证。 必须使用 SQL Server 登录来完成本文中的步骤。


## <a name="run-sqoop-jobs"></a>运行 Sqoop 作业

HDInsight 可以通过多种方法运行 Sqoop 作业。 使用下表来确定哪种方法最适合您，再按照演练的链接。

| 如果需要，请**使用此选项**...                                   | ...an**交互式**外壳程序 | ...**批处理**操作 | ..厎此**群集操作系统** | ...from 此**客户端操作系统** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux 和 Unix、 Mac OS X 和 Windows        |
| [Hadoop 的.NET SDK](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux 或窗口                          | （现在） 窗口                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux 或窗口                          | 窗口                                  |

##<a name="limitations"></a>限制

* 批量导出-基于与 Linux 的 HDInsight、 Sqoop 连接器用于将数据导出到 Microsoft SQL Server 或 SQL Azure 数据库当前不支持大容量插入。

* 属性的基于 Linux 的 HDInsight，当使用`-batch`开关执行插入时，Sqoop 将执行多个而不是批处理插入操作的插入。

##<a name="next-steps"></a>下一步行动

现在您已了解如何使用 Sqoop。 若要了解详细信息，请参阅︰

- [使用 HDInsight 配置单元](hdinsight-use-hive.md)
- [使用 HDInsight 的小猪](hdinsight-use-pig.md)
- [Oozie 使用 HDInsight][hdinsight-use-oozie]︰ 在 Oozie 工作流中使用 Sqoop 操作。
- [航班延迟使用分析数据 HDInsight][hdinsight-analyze-flight-data]︰ 使用配置单元来分析飞行延迟数据，然后使用 Sqoop 将数据导出到 SQL Azure 数据库。
- [将数据上传到 HDInsight][hdinsight-upload-data]︰ 找到其他方法，用于将数据上载到 HDInsight/Azure Blob 存储。


## <a name="appendix-a---a-powershell-sample"></a>附录 A-PowerShell 示例

PowerShell 示例中执行以下步骤︰

1. 连接到 Azure。
2. 创建一个 Azure 的资源组。 有关详细信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)
3. 创建 SQL Azure 数据库服务器，SQL Azure 数据库和两个表。 

    如果您使用的是 SQL Server，请使用下面的语句创建表︰
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    要检查的数据库和表的最简单方法是使用 Visual Studio。 数据库服务器和数据库可以进行检查，使用 Azure 的门户。

4. 创建一个 HDInsight 的群集。

    要检查群集时，可以使用 Azure PowerShell 的 Azure 的门户。

5. 预先处理源数据文件。

    在本教程中，您将到 SQL Azure 数据库导出 log4j 日志文件 （带分隔符的文件） 和配置单元表。 带分隔符的文件被称为*/example/data/sample.log*。 前面的教程中，您看到了少数 log4j 日志的示例。 在日志文件中，有一些空行和部分行类似如下︰
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    这是有关其他示例，请使用此数据，但我们必须在我们可以导入 SQL Azure 数据库或 SQL Server 之前删除这些异常。 如果有一个空的字符串或带到数量较少的行，Sqoop 导出将失败元素数目比定义 SQL Azure 数据库表中的字段数。 Log4jlogs 表中的字符串类型字段 7。

    此过程创建新文件的群集上︰ tutorials/usesqoop/data/sample.log。 要检查已修改的数据文件，可以使用 Azure 存储资源管理器工具或 Azure PowerShell 的 Azure 的入口。 [开始使用 HDInsight] [hdinsight-get-started]已经使用 Azure PowerShell 下载一个文件并显示文件内容的一个代码示例。

6. 导出到 SQL Azure 数据库的数据文件。

    源文件是 tutorials/usesqoop/data/sample.log。 数据导出到其中的表称为 log4jlogs。
    
    > [AZURE.NOTE] 不同连接字符串信息，此部分中的步骤应处理 SQL Azure 数据库或 SQL Server。 使用下列配置经过以下步骤︰
    >
    > * **Azure 的虚拟网络点到站点配置**︰ 虚拟网络连接到 SQL Server 专用数据中心中的 HDInsight 群集。 有关详细信息，请参阅[配置点到站点 VPN 管理门户中](../vpn-gateway/vpn-gateway-point-to-site-create.md)。
    > * **Azure HDInsight 3.1**︰ 关于在虚拟网络上创建群集的信息，请参阅[创建 Hadoop 群集中使用的自定义选项的 HDInsight](hdinsight-provision-clusters.md) 。
    > * **SQL Server 2014年**︰ 配置为允许身份验证和运行配置软件包要安全地连接到虚拟网络的 VPN 客户端。

7. 将配置单元表导出到 SQL Azure 数据库。

8. 对 HDInsight 群集导入 mobiledata 表。

    要检查已修改的数据文件，可以使用 Azure 存储资源管理器工具或 Azure PowerShell 的 Azure 的入口。  [开始使用 HDInsight] [hdinsight-get-started]有关于使用 Azure PowerShell 下载一个文件并显示文件内容的代码示例。


### <a name="the-powershell-sample"></a>PowerShell 示例

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
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
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
