<properties
   pageTitle="Azure 功能概述 |Microsoft Azure"
   description="了解如何使用 Azure 函数优化异步工作负载，以分钟为单位。"
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Azure 功能概述

Azure 函数是云中的轻松地运行小段代码或"函数"的解决方案。 您可以编写只是所需的问题，而不必担心整个应用程序或基础结构运行的代码。 这可以使开发更具效率，并可以使用您的开发语言的选择，如 C#、 F#、 Node.js，Python 或 PHP。 只为您的代码运行的时间和信任 Azure 支付所需的规模。

本主题提供了 Azure 函数的高级概述。 如果要在开始使用 Azure 的函数，从开始[创建您的第一个 Azure 函数](functions-create-first-azure-function.md)。 如果您正在寻找有关函数的详细技术信息，请参阅[开发人员参考](functions-reference.md)。

## <a name="features"></a>功能

以下是 Azure 函数的一些关键功能︰
    
* **选择的语言**-使用 C#、 F#、 Node.js，Python、 PHP、 批次、 大扫除、 Java 或任何可执行文件的写操作。
* **付薪每使用定价模型**-仅用于运行您的代码所花费的时间的付薪。 请参阅[定价节](#pricing)下方的动态应用程序服务计划选项。  
* **将您自己的依赖项**的功能支持 NuGet 和 NPM，，因此您可以使用您最喜爱的库。  
* **集成的安全性**-使用 OAuth 的提供商，例如 Azure Active Directory、 Facebook、 Google、 Twitter，以及 Microsoft 客户保护 HTTP 触发的函数。  
* **简化的集成**-方便地利用 Azure 服务和软件为-服务 (SaaS) 产品。 请参阅[集成部分](#integrations)下面的一些示例。  
* **灵活的开发**，但在门户功能权限的代码或设置持续集成和部署代码通过 GitHub、 Visual Studio 的团队服务，和其他[受支持的开发工具](../app-service-web/web-sites-deploy.md#deploy-using-an-ide)。  
* **开放源代码**的函数运行时是开放源代码，并[在 GitHub 上可用](https://github.com/azure/azure-webjobs-sdk-script)。  

## <a name="what-can-i-do-with-functions"></a>使用函数可以做什么？

Azure 函数是好的解决方案，用于处理数据集成系统中，使用互联网-的-事情 (IoT) 和构建简单的 Api 和 microservices。 考虑功能，如图像或订单处理、 文件维护、 长时间运行任务，您想要在后台线程中运行的任务或任何您想要按计划运行的任务。 

功能提供模板，以便您开始关键的方案，其中包括︰

* **BlobTrigger** -在它们被添加到容器时进程 Azure 存储 blob。 您可以用此用于调整图像的大小。
* **EventHubTrigger** -响应的事件传送到 Azure 事件中心。 在应用程序规范、 用户体验或工作流处理和互联网内容 (IoT) 方案中尤其有用。
* **一般的 webhook** -过程 webhook HTTP 请求支持 webhooks 的任何服务。
* **GitHub webhook** -响应在 GitHub 资料库中发生的事件。 有关示例，请参阅[创建 webhook 或 API 函数](functions-create-a-web-hook-or-api-function.md)。
* **HTTPTrigger** -触发器使用 HTTP 请求代码的执行。
* **QueueTrigger** -响应消息到达 Azure 存储队列中。 有关示例，请参阅[创建该绑定到 Azure 服务 Azure 函数](functions-create-an-azure-connected-function.md)。
* **ServiceBusQueueTrigger** -将您的代码连接到其他 Azure 服务或后端服务通过侦听消息队列。 
* **ServiceBusTopicTrigger** -将您的代码连接到其他 Azure 服务或后端服务订阅的主题。 
* **TimerTrigger** -在预定的时间表上执行清理或其他批次的任务。 有关示例，请参阅[创建事件处理函数](functions-create-an-event-processing-function.md)。

Azure 函数支持*触发器*，即开始执行代码的方法和*绑定*，简化用于输入和输出数据的编码方法。 触发器和 Azure 函数提供的绑定的详细说明，请参阅[Azure 功能触发器和绑定开发人员参考](functions-triggers-bindings.md)。


## <a name="integrations"></a>集成

Azure 功能集成与各种 Azure 和第三方服务。 您可以使用这些来触发您的函数并开始执行，或以作为输入和输出的代码。 Azure 函数支持以下服务集成。 

* Azure DocumentDB
* Azure 事件集线器 
* Azure 的移动应用程序 （表格）
* Azure 通知集线器
* Azure 服务总线 （队列和主题）
* Azure 存储 （blob、 队列和表） 
* GitHub (webhooks)
* 内部 （使用服务总线）

## <a name="pricing"></a>多少工作成本？

Azure 函数有两种类型的价格计划，选择一个最适合您的需要︰ 

* **动态宿主计划**-您的函数运行时，Azure 提供所有必要的计算资源。 您不必担心资源管理，并只为您的代码运行的时间付款。 定价详细信息都可以在[函数定价页](/pricing/details/functions)。 

* **应用程序服务计划**的运行您的函数就像您的 web、 移动设备、 和 API 的应用程序。 当您已经为其他应用程序使用应用程序服务时，您可以运行相同的计划，在不增加成本函数。 [应用程序服务定价页](/pricing/details/app-service/)上可以找到完整的详细信息。

扩展您的函数的详细信息，请参阅[如何缩放 Azure 的函数](functions-scale.md)。

##<a name="next-steps"></a>下一步行动

+ [创建您的第一个 Azure 函数](functions-create-first-azure-function.md)  
直接并创建您的第一个函数使用 Azure 函数快速入门。 
+ [Azure 功能开发人员参考](functions-reference.md)  
用于编码函数和触发器和绑定定义 Azure 函数运行库和一个引用有关的更多技术信息。
+ [测试 Azure 的函数](functions-test-a-function.md)  
介绍了各种工具和技术来测试您的函数。
+ [如何缩放 Azure 的函数](functions-scale.md)  
讨论了 Azure 功能，包括动态服务计划，以及如何选择正确的规划提供的服务计划。 
+ [了解更多有关 Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)  
Azure 函数利用 Azure 应用程序服务平台的核心功能部署、 环境变量和诊断等。 