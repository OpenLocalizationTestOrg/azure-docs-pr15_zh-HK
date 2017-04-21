<properties 
    pageTitle="Azure 应用程序服务的工作原理" 
    description="了解应用程序服务的工作方式" 
    keywords="应用程序服务，azure 应用程序服务，比例，可伸缩的应用程序服务计划应用程序服务成本"
    services="app-service" 
    documentationCenter="" 
    authors="yochay" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="02/10/2016" 
    ms.author="yochay"/>

# <a name="how-app-service-works"></a>应用程序服务的工作方式

Azure 应用程序服务是为解决目前面临的工程师的实际问题而设计的云服务。 应用程序服务致力于提供优秀的开发人员工作效率而不会影响交付应用程序在云中规模较大的必要性。 功能和同时支撑与最受欢迎的开发语言 （.NET、 Java、 PHP、 Node.JS 和 Python） 的开发人员撰写的企业业务线应用程序所需的框架，还提供了应用程序服务。
使用服务的应用程序开发人员可以︰

* 构建高度可扩展的 Web 应用程序
* 快速构建移动应用程序后端使用一套简单易用的移动功能，如数据后端，与移动应用程序的用户身份验证和推送通知。 
* 实现、 部署和发布 API 应用程序使用的 Api。
* 将业务应用程序联系在一起的工作流和逻辑应用程序用来转换数据。

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

所有的应用程序类型依赖于可扩展而且灵活的 Web 应用程序平台使开发人员能够从应用程序设计到应用程序维护完整的生命周期优化的体验。 生命周期功能允许︰

* 快速的应用程序创建-从头开始，或者从 Azure 市场挑选 OS 程序包。 
* 连续部署-自动部署新代码从流行源控制解决方案，例如从在线存储服务，如 OneDrive 和收存箱 TFS，GitHub BitBucket 并同步内容。
* 在生产环境中测试-平稳创建预生产环境，并管理对它们的通信部分。 中云时需要和部署后，如果发现问题时进行调试。
* 正在运行的异步任务和批处理作业的后台进程中执行代码，或激活代码基于事件 （例如，登录在 Azure 存储队列中的邮件），并计划时间 (CRON)。
* 扩展应用程序 — 使用多个选项之一来扩展您的服务水平和垂直方向自动基于通信和资源利用率。 配置专用于您的应用程序的专用环境   
* 维护应用程序的利用有许多调试和诊断功能以领先问题并有效地之一 （与功能，如自动修复和实时调试） 进行实时解决或事后通过分析日志和内存转储
 
应用程序服务功能合在一起使开发人员可以专注于他们的代码和快速达到稳定、 高度可扩展的生产状态。 使用 API 和应用程序的逻辑功能，开发人员可以生成桥接障碍之间业务解决方案还可以在部署到云集成真实的企业应用程序。  

[AZURE.INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]
