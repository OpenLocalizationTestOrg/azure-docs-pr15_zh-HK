<properties
   pageTitle="使用数据湖存储.NET SDK 来开发应用程序 |Microsoft Azure"
   description="使用 Azure 数据湖存储.NET SDK 来开发应用程序"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>学习如何使用 Azure 数据湖存储区使用.NET SDK

> [AZURE.SELECTOR]
- [门户网站](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API，](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

了解如何使用[Azure 数据湖存储.NET SDK](https://msdn.microsoft.com/library/mt581387.aspx)来执行基本的操作，如创建文件夹，上载和下载的数据文件，等等。有关数据湖的详细信息，请参阅[Azure 数据湖存储区](data-lake-store-overview.md)。

## <a name="prerequisites"></a>系统必备组件

* **Visual Studio 2013年或 2015年**。 下面的说明使用 Visual Studio 2015年。

* **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

* **Azure 数据湖存储帐户**。 有关如何创建帐户的说明，请参阅[入门 Azure 数据湖存储区](data-lake-store-get-started-portal.md)

* **创建 Azure 的活动目录的应用程序**。 Azure AD 应用程序用于验证数据湖存储应用程序使用 Azure 的广告。 有不同的方法来验证使用 Azure 的广告，是**最终用户身份验证**或**身份验证服务的服务**。 有关说明和如何进行身份验证的详细信息，请参阅[身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

## <a name="create-a-net-application"></a>创建.NET 应用程序

1. 打开 Visual Studio 并创建一个控制台应用程序。

2. 从**文件**菜单上，单击**新建**，然后单击**项目**。

3. 从**新建项目**，键入或选择以下值︰

  	| 属性 | 值                       |
  	|----------|-----------------------------|
  	| 类别 | 模板/视频 C# / 窗口 |
  	| 模板 | 控制台应用程序         |
  	| 名称     | CreateADLApplication        |

4. 单击**确定**以创建项目。

5. Nuget 程序包添加到项目中。

    1. 用鼠标右键单击解决方案资源管理器中的项目名称，然后单击**管理 NuGet 程序包**。
    2. **Nuget 程序包管理器**选项卡上，在确保**软件包源**设置为**nuget.org** ，**包括预发布版**复选框已选中。
    3. 搜索并安装下面的 NuGet 程序包︰

        * `Microsoft.Azure.Management.DataLake.Store`-本教程使用 v0.12.5 预览。
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-本教程使用 v0.10.6 预览。
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-本教程使用 v2.2.8 预览。

        ![Nuget 源添加](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "创建一个新的 Azure 数据湖帐户")

    4. 关闭**Nuget 程序包管理器**。

6. 打开**Program.cs**，删除现有的代码，然后将以下语句添加到命名空间的引用。

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. 声明的变量，如下所示，并提供数据湖存储区名称和资源组名称已存在的值。 另外，请确保您在此处提供的本地路径和文件名称的计算机上必须存在。 命名空间声明的后面添加下面的代码段。

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

在文章的剩余部分中，您可以看到如何使用可用的.NET 方法来执行操作，如身份验证、 文件上传等。

## <a name="authentication"></a>身份验证

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>如果您使用的最终用户身份验证 （在本教程推荐）

这使用现有 Azure 广告"本机客户端"应用程序;一是为您提供下面。 为了帮助您更快地完成本教程中，我们建议使用这种方法。

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

几个有关上述此段须知。

* 为了帮助您更快地完成本教程，此代码段使用 Azure 广告一个默认情况下，所有的 Azure 订阅可用的域和客户机 ID。 因此，您可以**使用作为此段-在您应用程序**。
* 但是，如果您想要使用自己的 Azure AD 域和应用程序客户机 ID，您必须创建 Azure AD 本机应用程序然后使用 Azure AD 域中，客户机 ID 和您创建的应用程序的重定向 URI。 有关说明，请参阅[创建活动目录应用程序](../resource-group-create-service-principal-portal.md#create-an-active-directory-application)。

>[AZURE.NOTE] 在上面的链接中的说明进行操作是 Azure 广告 web 应用程序。 但是，这些步骤是完全相同即使选择改为创建本机客户端应用程序。 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>如果您使用的提供服务的身份验证与客户端的机密 

下面的代码段可用于非交互方式验证您的应用程序使用客户端密钥 / 应用程序 / 服务主体的关键。 与现有[Azure 广告"Web 应用程序"应用程序](../resource-group-create-service-principal-portal.md)中使用它。

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>如果您使用的服务的服务身份验证使用证书

第三个选项，如下面的代码段可以非交互方式的应用程序身份验证使用证书对应用程序 / 服务的主体。 与现有[Azure 广告"Web 应用程序"应用程序](../resource-group-create-service-principal-portal.md)中使用它。

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>创建对象的客户端

下面的代码段创建数据湖存储帐户和文件系统客户端用来与服务发出请求的对象。

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>列出在订阅中的所有数据湖存储帐户

下面的代码段列出给定的 Azure 订阅中的所有数据湖存储帐户。

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>创建目录

下面的代码段演示`CreateDirectory`可用于创建数据湖存储帐户内的目录的方法。

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>上载的文件

下面的代码段演示`UploadFile`方法可用于将文件上载到数据湖存储帐户。

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`支持递归上载和下载本地文件的路径和数据湖存储文件路径之间。    

## <a name="get-file-or-directory-info"></a>获取文件或目录信息

下面的代码段演示`GetItemInfo`方法可用于检索有关的文件或目录可用数据湖存储区中的信息。 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>列表文件或目录

下面的代码段演示`ListItem`可以使用列出的文件及目录数据湖存储帐户中的方法。

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>合并文件

下面的代码段演示`ConcatenateFiles`方法，用于将文件连接起来。 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>追加到文件

下面的代码段演示`AppendToFile`所使用方法将数据追加到已存储在数据湖存储帐户的文件。

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>下载文件

下面的代码段演示`DownloadFile`方法，用于从数据湖存储帐户下载文件。

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>下一步行动

- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [数据湖存储.NET SDK 参考](https://msdn.microsoft.com/library/mt581387.aspx)
- [数据湖商店其余部分引用](https://msdn.microsoft.com/library/mt693424.aspx)
