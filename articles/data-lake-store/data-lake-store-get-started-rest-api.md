<properties 
   pageTitle="与使用 REST API 数据湖商店开始 |Microsoft Azure" 
   description="使用 WebHDFS REST Api 对数据湖存储区执行操作" 
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

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>学习如何使用 Azure 存储使用 REST Api 数据湖泊区

> [AZURE.SELECTOR]
- [门户网站](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API，](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

在本文中，您将学习如何使用 WebHDFS REST Api 和数据湖商店 REST Api 来执行帐户管理，以及 Azure 数据湖存储上的文件系统操作。 Azure 数据湖商店公开自己 REST Api，可用于帐户管理操作。 但是，由于数据湖商店是兼容 HDFS 和 Hadoop 的生态系统，它支持使用 WebHDFS REST Api 执行文件系统操作。

>[AZURE.NOTE] REST API 的详细信息数据湖商店的支持，请参阅[Azure 数据湖商店 REST API 参考](https://msdn.microsoft.com/library/mt693424.aspx)。

## <a name="prerequisites"></a>系统必备组件

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **创建 Azure 的活动目录的应用程序**。 Azure AD 应用程序用于验证数据湖存储应用程序使用 Azure 的广告。 有不同的方法来验证使用 Azure 的广告，是**最终用户身份验证**或**身份验证服务的服务**。 有关说明和如何进行身份验证的详细信息，请参阅[身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

- [卷曲](http://curl.haxx.se/)。 本文使用 cURL 演示如何进行数据湖存储帐户针对 REST API 调用。

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>如何验证使用 Azure Active Directory？

您可以使用两种方法使用 Azure Active Directory 进行身份验证。

### <a name="end-user-authentication-interactive"></a>最终用户身份验证 （交互）

在这种情况下，应用程序会提示用户登录，用户的上下文中执行所有操作。 交互式身份验证，请执行以下步骤。

1. 您的应用程序，通过将用户重定向到下面的 URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<重定向 URI > 需要在 URL 中使用的编码。 因此，对于 https://localhost，使用`https%3A%2F%2Flocalhost`)

    教程学习，请可以替换上面的 URL 中的占位符值并将其粘贴到 web 浏览器的地址栏中。 您将被重定向为使用 Azure 您的登录进行身份验证。 一旦成功登录，浏览器的地址栏中显示响应。 响应将以下面的格式︰
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. 捕获响应中的授权代码。 对于本教程，您可以从 web 浏览器的地址栏复制授权代码并将其传递在 POST 请求中到令牌的终结点，如下所示︰

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] 在这种情况下，\<重定向 URI > 不需要编码。

3. 响应是一个 JSON 对象，该对象包含一个访问令牌 (例如， `"access_token": "<ACCESS_TOKEN>"`) 和刷新令牌 (例如， `"refresh_token": "<REFRESH_TOKEN>"`)。 您的应用程序使用的访问令牌访问 Azure 数据湖商店和刷新令牌时获取另一个访问令牌访问令牌过期时。

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  当访问令牌到期时，您可以请求新的访问令牌使用刷新标记，如下所示︰

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
交互式用户身份验证的详细信息，请参阅[授权代码授予流](https://msdn.microsoft.com/library/azure/dn645542.aspx)。

### <a name="service-to-service-authentication-non-interactive"></a>服务的服务身份验证 （非交互式）

在这种情况下，该应用程序提供自己的凭据来执行操作。 为此，必须发出类似如下所示的一个 POST 请求。 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

此请求的输出将包含授权标记 (由`access-token`在下面的输出)，随后将使用 REST API 调用。 将此身份验证标记保存在一个文本文件;您将在本文后面需要它。

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

本文使用**非交互式**方法。 非交互式 （服务到服务调用） 的详细信息，请参阅[使用凭据的服务调用的服务](https://msdn.microsoft.com/library/azure/dn645543.aspx)。

## <a name="create-a-data-lake-store-account"></a>创建一个数据湖存储帐户

此操作基于 REST API 调用定义[在此处](https://msdn.microsoft.com/library/mt694078.aspx)。

使用以下 cURL 命令。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

在上面的命令中，替换\< `REDACTED` \>授权令牌与更早版本检索。 为提供**input.json**文件中包含该命令的请求有效负载`-d`上面的参数。 Input.json 文件的内容如下所示︰

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>在数据湖存储帐户中创建文件夹

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory)。

使用以下 cURL 命令。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

在上面的命令中，替换\< `REDACTED` \>授权令牌与更早版本检索。 此命令创建一个名为**mytempdir**的数据湖存储帐户的根文件夹下目录。

如果此操作成功完成后，您应看到类似下面的响应︰

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>列表文件夹中的数据湖存储帐户

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory)。

使用以下 cURL 命令。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

在上面的命令中，替换\< `REDACTED` \>授权令牌与更早版本检索。

如果此操作成功完成后，您应看到类似下面的响应︰

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>将数据上载到数据湖存储帐户

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File)。

上载数据使用 REST API，WebHDFS 是一个两步过程，如下文所述。

1. 不发送上载的文件数据提交将 HTTP 请求。 在下面的命令，将**\<yourstorename >**数据湖存储区名称。

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    此命令的输出将被包含临时重定向 URL 中，像一个如下所示。

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. 现在，您必须提交另一个将 HTTP 请求根据**位置**属性在响应中列出的 URL。 更换**\<yourstorename >**数据湖存储区名称。

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    输出将类似如下︰

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>从数据湖存储帐户的读取数据

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File)。

从数据湖存储区读取数据帐户是一个两步过程。

* 第一次提交 GET 请求该终结点对`https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`。 这将返回要提交到的下一个 GET 请求的位置。
* 然后提交 GET 请求与该终结点`https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`。 这将显示该文件的内容。

但是，由于没有在输入参数中的第一个和第二步之间没有区别，因此您可以使用`-L`提交的第一个请求的参数。 `-L`选项实质上是将两个请求合并为一个，并将使卷曲恢复上的新位置的请求。 最后，所有请求调用的输出显示，类似下面所示。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

您应该看到类似于下面的输出︰

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>重命名数据湖存储帐户中的文件

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory)。

使用以下 cURL 命令重命名文件。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

您应该看到类似于下面的输出︰

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>从数据湖存储帐户中删除文件

此操作基于 WebHDFS REST API 调用定义[在此处](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory)。

使用以下 cURL 命令可删除文件。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

您应该看到类似于下面的输出︰

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>删除数据湖存储帐户

此操作基于 REST API 调用定义[在此处](https://msdn.microsoft.com/library/mt694075.aspx)。

使用以下 cURL 命令来删除数据湖存储帐户。 更换**\<yourstorename >**数据湖存储区名称。

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

您应该看到类似于下面的输出︰

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>请参见

- [打开源大数据的应用程序符合 Azure 数据湖存储区](data-lake-store-compatible-oss-other-applications.md)
 
