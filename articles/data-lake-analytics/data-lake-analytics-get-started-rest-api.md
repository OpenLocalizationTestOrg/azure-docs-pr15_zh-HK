<properties 
   pageTitle="开始使用 REST API 数据湖分析 |Microsoft Azure" 
   description="使用 WebHDFS REST Api 来执行数据湖分析上的操作" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>有关使用 REST Api Azure 数据湖分析入门

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

了解如何使用 WebHDFS REST Api 和数据湖分析 REST Api 来管理数据湖分析帐户、 作业和目录。 

## <a name="prerequisites"></a>系统必备组件

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **创建 Azure 的活动目录的应用程序**。 Azure AD 应用程序用于验证数据湖分析应用程序使用 Azure 的广告。 有不同的方法来验证使用 Azure 的广告，是**最终用户身份验证**或**身份验证服务的服务**。 有关说明和如何进行身份验证的详细信息，请参阅[使用 Azure Active Directory 数据湖分析与验证](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)。

- [卷曲](http://curl.haxx.se/)。 本文使用 cURL 演示如何进行数据湖分析帐户针对 REST API 调用。

## <a name="authenticate-with-azure-active-directory"></a>使用 Azure Active Directory 进行身份验证

有两种方法可以使用 Azure Active Directory 进行身份验证。

### <a name="end-user-authentication-interactive"></a>最终用户身份验证 （交互）

使用这种方法，应用程序会提示用户登录，用户的上下文中执行所有操作。 

请按照下列步骤进行交互式身份验证︰

1. 您的应用程序，通过将用户重定向到下面的 URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<重定向 URI > 需要在 URL 中使用的编码。 因此，对于 https://localhost，使用`https%3A%2F%2Flocalhost`)

    教程学习，请可以替换上面的 URL 中的占位符值并将其粘贴到 web 浏览器的地址栏中。 您将被重定向为使用 Azure 您的登录进行身份验证。 一旦您成功地登录，浏览器的地址栏中显示响应。 响应将以下面的格式︰
        
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

使用此方法时，应用程序可以提供自己的凭据来执行操作。 为此，必须发出 POST 请求类似如下所示︰ 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

此请求的输出将包含授权标记 (由`access-token`在下面的输出)，随后将使用 REST API 调用。 将此身份验证标记保存在一个文本文件;您将在本文后面需要它。

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

本文使用**非交互式**方法。 非交互式 （服务到服务调用） 的详细信息，请参阅[使用凭据的服务调用的服务](https://msdn.microsoft.com/library/azure/dn645543.aspx)。
## <a name="create-a-data-lake-analytics-account"></a>创建一个数据湖分析帐户

您可以创建一个数据湖分析帐户前，您必须创建 Azure 资源组中，并且数据湖存储帐户。  请参阅[创建数据湖存储帐户](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account)。

以下 Curl 命令显示如何创建一个帐户︰

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

更换\< `REDACTED` \>与授权标记， \< `AzureSubscriptionID` \>与您的订阅 ID \< `AzureResourceGroupName` \>与现有 Azure 资源组名称，和\< `NewAzureDataLakeAnalyticsAccountName` \>使用新数据湖分析帐户名称。 为提供**CreateDatalakeAnalyticsAccountRequest.json**文件中包含该命令的请求有效负载`-d`上面的参数。 Input.json 文件的内容如下所示︰

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>在订阅的列表数据湖分析帐户

以下 Curl 命令演示如何列出在订阅的帐户︰

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

更换\< `REDACTED` \>与授权标记， \< `AzureSubscriptionID` \>与您的订阅 id。 输出内容类似于︰

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>获取有关数据湖分析帐户信息

以下 Curl 命令演示如何获取帐户信息︰

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

更换\< `REDACTED` \>与授权标记， \< `AzureSubscriptionID` \>与您的订阅 ID \< `AzureResourceGroupName` \>与现有 Azure 资源组名称，和\< `DataLakeAnalyticsAccountName` \>与现有数据湖分析帐户的名称。 输出内容类似于︰

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>列表数据湖储存的数据湖分析帐户

以下 Curl 命令演示如何列出数据湖存储的帐户︰

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

更换\< `REDACTED` \>与授权标记， \< `AzureSubscriptionID` \>与您的订阅 ID \< `AzureResourceGroupName` \>与现有 Azure 资源组名称，和\< `DataLakeAnalyticsAccountName` \>与现有数据湖分析帐户的名称。 输出内容类似于︰

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>将 U SQL 作业提交

以下 Curl 命令显示如何提交 U SQL 作业︰

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

更换\< `REDACTED` \>与授权标记， \< `DataLakeAnalyticsAccountName` \>与现有数据湖分析帐户的名称。 为提供**SubmitADLAJob.json**文件中包含该命令的请求有效负载`-d`上面的参数。 Input.json 文件的内容如下所示︰

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

输出内容类似于︰

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>U SQL 作业列表

以下 Curl 命令演示如何列出 U SQL 作业︰

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

更换\< `REDACTED` \>使用授权令牌和\< `DataLakeAnalyticsAccountName` \>与现有数据湖分析帐户的名称。 


输出内容类似于︰

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>获取目录项

以下 Curl 命令显示如何从目录获取数据库︰

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

输出内容类似于︰

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>请参见

- 更复杂的查询，请参阅[分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)。
- 首先开发 U SQL 应用程序，请参见[使用 Visual Studio 的数据湖工具开发 U-SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)。
- U SQL，请参阅[开始使用 Azure 数据湖分析 U SQL 语言](data-lake-analytics-u-sql-get-started.md)。
- 管理任务，请参阅[管理 Azure 数据湖分析使用 Azure 的门户](data-lake-analytics-manage-use-portal.md)。
- 若要获取数据分析湖泊功能的概述，请参阅[Azure 数据湖分析概述](data-lake-analytics-overview.md)。
- 若要查看同一教程使用其他工具，请单击页面顶部的选项卡上选择器。
- 若要记录的诊断信息，请参阅[访问诊断日志中的 Azure 数据湖分析](data-lake-analytics-diagnostic-logs.md)
