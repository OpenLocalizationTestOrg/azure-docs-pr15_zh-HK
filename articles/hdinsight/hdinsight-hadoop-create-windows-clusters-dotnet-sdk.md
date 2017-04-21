<properties
   pageTitle="在 HDInsight 使用.NET SDK 中创建基于 Windows 的 Hadoop 群集 |Microsoft Azure"
    description="了解如何创建使用.NET SDK 的 Azure HDInsight HDInsight 群集。"
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-net-sdk"></a>在 HDInsight 使用.NET SDK 中创建基于 Windows 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]


了解如何创建使用.NET SDK HDInsight 群集。 其他群集创建工具和功能单击此页面顶部选项卡中的选择或者查看[群集创建方法](hdinsight-provision-clusters.md#cluster-creation-methods)。


##<a name="prerequisites"></a>系统必备组件︰

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

在开始这篇文章中的说明操作之前，必须具有下列︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- Visual Studio 2013年或 2015年。

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>创建群集

HDInsight.NET SDK 提供了更方便地使用.NET Framework 应用程序中的 HDInsight 的.NET 客户端库。 按照下面创建一个 Visual Studio 的控制台应用程序和粘贴的代码创建群集的说明。

应用程序需要 Azure 的资源组和默认的存储帐户。  [附录 A](#appx-a-create-dependent-components)提供了 PowerShell 脚本来创建依赖的组件。

**若要创建一个 Visual Studio 的控制台应用程序**

1. 在 Visual Studio 中创建一个新 C# 控制台应用程序。
2. Nuget 程序包管理控制台中运行下面的 Nuget 命令。

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

6. 从解决方案资源管理器中，双击以打开它，粘贴下面的代码中，为变量提供值**Program.cs** :

        using System;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Net.Http;
        
        namespace CreateHDInsightCluster
        {
            class Program
            {
                // The client for managing HDInsight
                private static HDInsightManagementClient _hdiManagementClient;
                // Replace with your AAD tenant ID if necessary
                private const string TenantId = UserTokenProvider.CommonTenantId; 
                private const string SubscriptionId = "<Your Azure Subscription ID>";
                // This is the GUID for the PowerShell client. Used for interactive logins in this example.
                private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
                private const string ExistingResourceGroupName = "<Azure Resource Group Name>";
                private const string ExistingStorageName = "<Default Storage Account Name>.blob.core.windows.net";
                private const string ExistingStorageKey = "<Default Storage Account Key>";
                private const string ExistingBlobContainer = "<Default Blob Container Name>";
                private const string NewClusterName = "<HDInsight Cluster Name>";
                private const int NewClusterNumWorkerNodes = 2;
                private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
                private const OSType NewClusterOsType = OSType.Windows;
                private const string NewClusterType = "Hadoop";
                private const string NewClusterVersion = "3.2";
                private const string NewClusterUsername = "admin";
                private const string NewClusterPassword = "<HTTP User password>";
                

        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");
        
                    // Authenticate and get a token
                    var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                    // Flag subscription for HDInsight, if it isn't already.
                    EnableHDInsight(authToken);
                    // Get an HDInsight management client
                    _hdiManagementClient = new HDInsightManagementClient(authToken);

                    // Set parameters for the new cluster
                    var parameters = new ClusterCreateParameters
                    {
                        ClusterSizeInNodes = NewClusterNumWorkerNodes,
                        UserName = NewClusterUsername,
                        Password = NewClusterPassword,
                        Location = NewClusterLocation,
                        DefaultStorageAccountName = ExistingStorageName,
                        DefaultStorageAccountKey = ExistingStorageKey,
                        DefaultStorageContainer = ExistingBlobContainer,
                        ClusterType = NewClusterType,
                        OSType = NewClusterOsType
                    };
                    // Create the cluster
                    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

                    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                /// <summary>
                /// Authenticate to an Azure subscription and retrieve an authentication token
                /// </summary>
                /// <param name="TenantId">The AAD tenant ID</param>
                /// <param name="ClientId">The AAD client ID</param>
                /// <param name="SubscriptionId">The Azure subscription ID</param>
                /// <returns></returns>
                static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
                {
                    var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                    var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                        ClientId, 
                        new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                        PromptBehavior.Always, 
                        UserIdentifier.AnyUser);
                    return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
                }
                /// <summary>
                /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
                /// </summary>
                /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
                /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
                /// <param name="authToken">An authentication token for your Azure subscription</param>
                static void EnableHDInsight(TokenCloudCredentials authToken)
                {
                    // Create a client for the Resource manager and set the subscription ID
                    var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                    resourceManagementClient.SubscriptionId = SubscriptionId;
                    // Register the HDInsight provider
                    var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
                }
            }
        }

7. 按**f5 键**以运行该应用程序。 控制台窗口应打开并显示该应用程序的状态。 您将还会提示您输入您的 Azure 帐户的凭据。 它可能需要几分钟来创建一个 HDInsight 的群集。



##<a name="next-steps"></a>下一步行动
在本文中，您学习了几种方法来创建一个 HDInsight 的群集。 若要了解详细信息，请参阅下列文章︰

- [开始使用 Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) -了解如何开始使用 HDInsight 群集
- [HDInsight 使用.NET SDK 中的运行配置单元作业](hdinsight-hadoop-use-hive-dotnet-sdk.md)
- [HDInsight 使用.NET SDK 中运行猪的作业](hdinsight-hadoop-use-pig-dotnet-sdk.md)
- [HDInsight 使用.NET SDK 中运行 Sqoop 作业](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
- [在 HDInsight 中运行 Oozie 作业](hdinsight-use-oozie.md)
- [Azure HDInsight SDK 文档] [ hdinsight-sdk-documentation] -发现 HDInsight SDK

[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx


##<a name="appx-a-create-dependent-components"></a>Appx A 创建依赖组件

脚本可以是以下 Azure PowerShell 创建从属组件所需的.NET 应用程序在本教程中使用。

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    ####################################
    # Service names and varialbes
    ####################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect to Azure
    ####################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use the cluster name as the container name

    Write-Host "Use the following names in your .NET application" -ForegroundColor Green
    Write-host "Resource Group Name: $resourceGroupName"
    Write-host "Default Storage Account Name: $defaultStorageAccountName"
    Write-host "Default Storage Account Key: $defaultStorageAccountKey"
    Write-host "Default Blob Container Name: $defaultBlobContainerName"
