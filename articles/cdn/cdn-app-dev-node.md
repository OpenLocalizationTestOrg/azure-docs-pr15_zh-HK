<properties
    pageTitle="开始使用 Azure CDN SDK 的 Node.js |Microsoft Azure"
    description="了解如何编写 Node.js 应用程序来管理 Azure CDN。"
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure CDN 开发入门

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

您可以使用[Azure CDN SDK 的 Node.js](https://www.npmjs.com/package/azure-arm-cdn)自动化的 CDN 的配置文件和终结点创建和管理。  本教程将带领通过创建一个简单的 Node.js 控制台应用程序演示几个可用的操作。  本教程不是详细描述的 Node.js Azure CDN SDK 的所有方面。

若要完成本教程，您应该已经[Node.js](http://www.nodejs.org) **4.x.x**或更高版本安装和配置。  您可以使用任何文本编辑器，您想要创建 Node.js 应用程序。  若要编写本教程中，我使用[Visual Studio 代码](https://code.visualstudio.com)。  

> [AZURE.TIP] [完成本教程中的项目，](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)则可以在 MSDN 上的下载。

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>创建项目和添加 NPM 相关性

现在，我们已经为我们 CDN 配置文件创建的资源组，并给我们 Azure AD 的应用程序权限管理 CDN 配置文件和组中的终结点，我们可以开始创建我们的应用程序。

创建一个文件夹以存储您的应用程序。  从当前路径中的 Node.js 工具控制台，将您当前的位置设置为该新文件夹并通过执行初始化您的项目︰
    
    npm init
    
然后可以看到一系列的问题来初始化您的项目。  对于**入门级产品**，本教程将使用*app.js*。  您可以看到在下面的示例中我其他的选择。

![NPM 初始化输出](./media/cdn-app-dev-node/cdn-npm-init.png)

现在，我们的项目已初始化为*packages.json*文件。  我们的项目要使用 NPM 包中包含某些 Azure 库。  我们将使用 Node.js (ms 其余 azure) Azure 客户端运行时和 Azure CDN 客户端库作为 Node.js (azure 臂 cd)。  让我们为依赖项添加到项目中的那些。
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

软件包在安装时， *package.json*文件应该看起来类似于以下示例 （版本的数字可能会有所不同）︰

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

最后，使用文本编辑器，创建一个空文本文件并将其保存为*app.js*我们项目文件夹的根目录中。  现在我们就可以开始编写代码。

## <a name="requires-constants-authentication-and-structure"></a>要求常量、 验证和结构

有了*app.js*在我们的编辑器中打开，让我们得到我们编写程序的基本结构。

1. 添加"要求"我们 NPM 包的顶部使用以下︰

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. 我们需要定义一些常量，将使用我们的方法。  添加以下内容。  一定要替换占位符，包括**&lt;尖括号&gt;**，使用您自己的值根据需要。

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. 接下来，我们将实例化 CDN 管理客户端并为其提供我们的凭据。

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    如果您使用的单个用户验证，这两行看略有不同。

    >[AZURE.IMPORTANT] 如果您选择了单个用户身份验证，而不是服务主体，仅使用此代码示例。  要小心保护个人用户凭据并将它们保存机密。

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    一定要替换的项目**&lt;尖括号&gt;**用正确的信息。  为`<redirect URI>`，使用重定向 URI Azure AD 中注册应用程序时输入的。
    

4.  我们 Node.js 控制台应用程序将执行一些命令行参数。  让我们验证至少一个参数传递。

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. 接下来我们计划，在其中我们分支出来对基于何种参数传递其他函数的主要部分。

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  在我们的计划中的多个位置，我们需要确保适当数量的参数中传递并显示一些帮助，如果它们看起来不正确。  让我们创建函数来执行该操作。

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. 最后，我们将使用 CDN 管理客户端的函数是异步的因此他们需要回当它们完成时要调用的方法。  让我们先创建一个可以显示 CDN 管理客户端 （如果有的话） 的输出并退出该程序。

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

既然我们程序的基本结构编写的我们应该创建基于我们的参数调用的函数。

## <a name="list-cdn-profiles-and-endpoints"></a>列表 CDN 配置文件和终结点

首先，列出我们现有的配置文件和终结点的代码。  我的代码注释提供预期的语法，使我们了解每个参数的位置。

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN 的配置文件和终结点创建

接下来，我们将编写的函数来创建配置文件和终结点。

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>清除某个终结点

假定已创建终结点，我们可能想要执行我们的计划中的一个常见任务清除我们终结点中的内容。

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>删除 CDN 配置文件和终结点

我们将包括的最后一个函数中删除终结点和配置文件。

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>运行程序

现在，我们可以执行我们使用我们最喜爱的调试器的 Node.js 计划或在控制台上。

> [AZURE.TIP] 如果您使用 Visual Studio 代码作为您的调试器，您需要设置您的环境中的命令行参数传递。  Visual Studio 代码在**lanuch.json**文件中执行此操作。  命名**参数**的属性查找并添加您的参数的字符串值的数组，以便它类似于下图︰ `"args": ["list", "profiles"]`。

让我们先列出我们的配置文件。

![配置文件列表](./media/cdn-app-dev-node/cdn-list-profiles.png)

我们返回了一个空数组。  我们没有任何配置我们的资源组中，因为，预期。  现在让我们创建一个配置文件。

![创建配置文件](./media/cdn-app-dev-node/cdn-create-profile.png)

现在，让我们添加终结点。

![创建终结点](./media/cdn-app-dev-node/cdn-create-endpoint.png)

最后，让我们删除我们的配置文件。

![删除配置文件](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>下一步行动

若要查看此演练中，[下载示例](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)从已完成的项目。

要了解该引用的 Azure CDN sdk Node.js，查看[引用](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)。

要查找 Node.js Azure SDK 上附加的文档，查看[完整引用](http://azure.github.io/azure-sdk-for-node/)。

管理与[PowerShell](./cdn-manage-powershell.md)CDN 资源。

