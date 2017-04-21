<properties
   pageTitle="逻辑应用程序示例和应用场景 |Microsoft Azure"
   description="常见的逻辑应用程序示例，请参阅，了解如何实现的常见方案"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>逻辑应用程序示例和常见方案

此文档的详细信息的常见方案和示例，以帮助您了解的几种方法，您可以使用逻辑应用程序自动化的业务流程。 

## <a name="custom-triggers-and-actions"></a>自定义的触发器和操作

有几种方法可以触发从另一个应用程序逻辑的应用程序。 以下是一些常见示例︰

- [创建自定义的触发器或操作](app-service-logic-create-api-app.md)
- [长时间运行的操作](app-service-logic-create-api-app.md)
- [HTTP 请求触发 （开机自检）](app-service-logic-http-endpoint.md)
- [Webhook 触发器和操作](app-service-logic-create-api-app.md)
- [轮询触发器](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>方案

- [同步请求的响应](app-service-logic-http-endpoint.md)
- [与 SMS 的请求响应](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>错误处理和日志记录

- [异常和错误处理](app-service-logic-exception-handling.md)
- [配置 Azure 警报和诊断](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>方案

- [使用案例︰ 错误和异常处理](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>部署和管理

- [创建自动的部署](app-service-logic-create-deploy-template.md)
- [生成和部署从 Visual Studio 的逻辑应用程序](app-service-logic-deploy-from-vs.md)
- [监视器逻辑应用程序](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>内容类型、 转换和转换

逻辑应用程序[工作流定义语言](http://aka.ms/logicappsdocs)包含许多功能，以便您可以转换并使用不同的内容类型。  另外该引擎将执行所有它可以尽可能地保留内容类型工作流通过的数据流。

- 如应用程序/json、 应用程序/xml 和纯/文本[内容类型的处理](app-service-logic-content-type.md)
- [创作的工作流定义](app-service-logic-author-definitions.md)
- [工作流定义语言参考](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>批次和循环

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [直到](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Azure 的功能与集成

- [Azure 功能集成](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>方案

- [Azure 服务总线触发器的功能](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP、 存放和 SOAP

 - [SOAP 调用](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


我们将不断添加示例和应用场景到此文档。 使用下面的注释部分，让我们知道哪些示例或方案想要在这里看到。