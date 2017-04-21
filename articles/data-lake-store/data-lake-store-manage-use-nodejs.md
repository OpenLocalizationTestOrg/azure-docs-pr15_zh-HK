<properties 
   pageTitle="学习如何使用 Azure 数据湖商店的 Node.js 使用 Azure SDK |Microsoft Azure"
   description="了解如何使用 Node.js 处理数据湖存储帐户和文件系统。" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>学习如何使用 Azure 数据湖商店的 Node.js 使用 Azure SDK

> [AZURE.SELECTOR]
- [门户网站](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API，](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


了解如何使用 Azure 的 Node.js SDK 创建 Azure 数据湖存储帐户并执行基本的操作，如创建文件夹，上载和下载的数据文件，删除您的帐户，等等。有关数据湖商店的详细信息，请参阅[数据湖商店的概述](data-lake-store-overview.md)。 目前，该 SDK 支持

  *  **Node.js 版本︰ 0.10.0 或更高版本**
  *  **帐户的 REST API 版本︰ 2015年-10-01-预览**
  *  **文件系统的 REST API 版本︰ 2015年-10-01-预览**

## <a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **创建 Azure 的活动目录的应用程序**。 Azure AD 应用程序用于验证数据湖存储应用程序使用 Azure 的广告。 有不同的方法来验证使用 Azure 的广告，是**最终用户身份验证**或**身份验证服务的服务**。 有关说明和如何进行身份验证的详细信息，请参阅[身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

## <a name="how-to-install"></a>如何安装

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>使用 Azure Active Directory 进行身份验证

下面的代码段显示了使用 Azure 广告两种不同的方式与数据湖存储区中进行身份验证。 要用于身份验证，与数据湖存储的各种方法的详细讨论，请参阅[身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

下面的代码段还要求输入类似 Azure AD 域名称，客户端 ID 的 Azure 广告应用程序，等等。从 Azure 广告应用程序，您必须创建的详细信息，其中也包括在上面的链接，可以检索所有这些详细信息。

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>创建数据湖商店的客户端

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>创建一个数据湖存储帐户

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>创建一个文件，内容
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>获取文件和文件夹的列表

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>请参见

- [对于 Node.js Microsoft Azure SDK](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK 的 Node.js-数据分析湖泊管理](https://www.npmjs.com/package/azure-arm-datalake-analytics)
