<properties
    pageTitle="Azure 功能开发人员参考 |Microsoft Azure"
    description="了解 Azure 函数概念和常见的各种语言和绑定的组件。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure 功能开发人员参考

Azure 功能共享的一些核心技术概念和组件，而不考虑语言或使用的绑定。 阅读本文档中学习特定于给定的语言或绑定的详细信息之前，请务必通读此概述适用于所有这些。

本文假定您已经读过[Azure 功能概述](functions-overview.md)，并且熟悉[WebJobs SDK 的概念，如触发器、 绑定和 JobHost 运行时](../app-service-web/websites-dotnet-webjobs-sdk.md)。 WebJobs SDK 基于 azure 的函数。 


## <a name="function-code"></a>函数代码

*函数*是 Azure 函数中的主要概念。 您选择的语言编写的函数代码，将代码文件和配置文件保存在同一文件夹中。 配置 json 格式，并且该文件命名为`function.json`。 支持多种语言，并且每个具有优化为最适合该语言略有不同的体验。 

`function.json`文件中包含的函数，包括其绑定到特定的配置。 运行时读取此文件以确定要关闭的触发的事件时调用该函数，并将数据发送到何处要包括哪些数据传递函数本身。 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

您可以防止运行时运行该函数，通过设置`disabled`属性为`true`。

`bindings`属性，可以配置触发器和绑定。 每个绑定共享几个常见的设置和一些特定于绑定的特定类型的设置。 每个绑定需要下列设置︰

|属性|值/类型|注释|
|---|-----|------|
|`type`|字符串|绑定类型。 例如， `queueTrigger`。
|`direction`|in，出| 指示绑定是否接收数据到函数或从函数发送数据。
| `name` | 字符串 | 将绑定数据的函数中使用的名称。 对于 C# 这是一个参数名称;对于 JavaScript 中，它将是一个键/值列表中的项。

## <a name="function-app"></a>函数的应用程序

函数应用程序由一个或多个 Azure 应用程序服务一起管理的各个函数。 在应用程序中函数的函数的所有共享相同的定价计划、 连续部署和运行时版本。 以多种语言编写的函数可以共享相同的函数应用程序。 函数应用程序看作是一种方法来组织和共同管理您的函数。 

## <a name="runtime-script-host-and-web-host"></a>（脚本宿主和 web 主机） 运行时

运行时或脚本宿主为基础 WebJobs SDK 主机侦听事件、 收集和发送的数据，并最终运行您的代码。 

为了便于 HTTP 触发器，还会有专为坐前面的脚本宿主在生产方案中的 web 主机。 这可以帮助隔离 web 主机托管在前端通信中的脚本宿主。

## <a name="folder-structure"></a>文件夹结构

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

当设置用于部署到 Azure 应用程序服务中函数的应用程序功能的项目，可以将此文件夹结构视为站点代码。 您可以使用现有工具如持续集成和部署中，或自定义部署脚本做部署时间包安装或代码 transpilation。

>[AZURE.NOTE] `wwwroot`文件夹这里是获取文件部署到的位置。 但是，您不能包含该文件夹在您部署时，其结果会导致文件`wwwroot\wwwroot`。 相反，您`host.json`文件和函数文件夹应直接在您的部署的根。

## <a id="fileupdate"></a>如何更新函数的应用程序文件

Azure 门户中内置函数编辑器允许您更新*function.json*文件和一个函数的代码文件。 若要上载或更新其他文件，例如*package.json*或*project.json*或依赖项，您必须使用其他部署方法。

应用程序服务，内置函数的应用程序，因此所有[使用标准 web 应用程序的部署选项](../app-service-web/web-sites-deploy.md)都可用于函数应用程序。 以下是一些可用于上载或更新函数的应用程序文件的方法。 

#### <a name="to-use-app-service-editor"></a>若要使用应用程序服务编辑器

1. 在 Azure 函数门户中，请单击**函数的应用程序设置**。

2. 在**高级设置**部分中，单击**转到应用程序和服务设置**。

3. 在**开发工具**下，单击应用程序菜单导航中的**应用程序服务编辑器**。

4.  单击**转到**。

    应用程序服务编辑器加载后，您将看到在*wwwroot*下的*host.json*文件，函数文件夹。 

5. 打开文件以进行编辑，或拖放从开发计算机将文件上传。

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>若要使用函数的应用程序的 SCM (Kudu) 终结点

1. 导航到︰ `https://<function_app_name>.scm.azurewebsites.net`。

2. 单击**调试控制台 > CMD**。

3. 定位到`D:\home\site\wwwroot\`更新*host.json*或`D:\home\site\wwwroot\<function_name>`更新函数的文件。

4. 拖放文件您要上载到相应的文件夹中的文件网格。 您可以拖放文件的文件网格中有两个方面。 *.Zip*文件中，出现一个框，其标签为"拖动此处上载并解压缩。" 对于其它文件类型，删除文件网格中，但"解压缩"框外。

#### <a name="to-use-ftp"></a>若要使用 FTP

1. 按照[此处](../app-service-web/web-sites-deploy.md#ftp)以获取 FTP 配置。

2. 当您连接到该函数的应用程序网站时，更新的*host.json*将文件复制到`/site/wwwroot`或函数将文件复制到`/site/wwwroot/<function_name>`。

#### <a name="to-use-continuous-deployment"></a>使用连续的部署

按照[Azure 函数的连续部署](functions-continuous-deployment.md)该主题中的说明进行操作。

## <a name="parallel-execution"></a>并行执行

多个触发事件发生时比单线程函数运行时处理速度更快，运行时可以调用多次中并行的函数。  如果函数应用程序正在使用[动态服务计划](functions-scale.md#dynamic-service-plan)，该函数应用程序无法自动放大。  函数的应用程序，每个实例应用程序运行的动态服务计划或常规[应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)，是否可能处理并发的函数调用中使用多个线程并行。  并发的函数调用每个函数的应用程序实例中的最大数量根据函数的应用程序的内存大小而定。 

## <a name="azure-functions-pulse"></a>Azure 函数脉冲  

脉冲是一个实况事件流，它显示了您的函数的运行频率，以及成功和失败。 您还可以监视您的平均执行时间。 我们将添加更多特性和自定义它随着时间的推移。 可以从**监视**选项卡来访问**脉冲**页。

## <a name="repositories"></a>存储库

Azure 函数的代码是开源并存储在 GitHub 存储库︰

* [Azure 函数运行时](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure 函数入口](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure 函数模板](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK 扩展](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>绑定

这里是所有受支持的绑定的表。

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>报告问题

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅以下资源︰

* [Azure 函数 C# 开发人员参考](functions-reference-csharp.md)
* [Azure 函数 F# 开发人员参考](functions-reference-fsharp.md)
* [Azure 函数 NodeJS 开发人员参考](functions-reference-node.md)
* [Azure 功能触发器和绑定](functions-triggers-bindings.md)
* [Azure 函数︰ 旅](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/)Azure 应用程序服务团队博客上。 Azure 函数如何开发的历史记录。





