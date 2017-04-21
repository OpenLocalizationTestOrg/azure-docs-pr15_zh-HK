<properties
   pageTitle="逻辑应用程序方案︰ 创建 Azure 服务总线功能触发器 |Microsoft Azure"
   description="使用 Azure 函数创建逻辑应用程序的服务总线触发器"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>逻辑应用程序方案︰ 通过使用 Azure 函数创建 Azure 服务总线触发器

Azure 函数可用于为创建一个触发器逻辑应用程序时您需要部署一个长时间运行侦听程序或任务。 例如，可以创建一个函数，将侦听队列，然后立即激发的推触发器逻辑的应用程序。

## <a name="build-the-logic-app"></a>构建的逻辑应用程序

在此示例中，必须为每个需要触发的逻辑应用程序运行的函数。 首先，创建一个逻辑应用程序具有 HTTP 请求触发。 接收队列消息时，该函数将调用该终结点。  

1. 创建新的逻辑应用程序;选择**手册 — — 当 HTTP 请求接收到**触发器。  
   （可选） 您可以指定要使用像[jsonschema.net](http://jsonschema.net)工具使用队列消息的 JSON 架构。 在触发器中粘贴该架构。 这有助于理解数据的图形设计器，更轻松地流动在工作流的属性。
1. 添加任何您想要接收队列消息后出现的附加措施。 例如，发送一封电子邮件通过 Office 365。  
1. 保存逻辑应用程序生成的触发器绑定到此逻辑应用程序的回调 URL。 该 URL 显示在触发器卡上。

![回调 URL 显示在触发器卡][1]

## <a name="build-the-function"></a>生成函数

接下来，您需要创建一个将充当触发器并侦听队列函数。

1. 在[Azure 函数门户](https://functions.azure.com/signin)中，选择**新的函数**，然后选择**ServiceBusQueueTrigger-C#**模板。

    ![Azure 函数入口][2]

2. 配置连接到服务总线队列 (其中将使用 Azure 服务总线 SDK`OnMessageReceive()`侦听器)。
3. 编写一个简单的函数调用 （前面部分创建） 的逻辑应用程序终结点为触发器使用队列的消息。 下面是一个函数的完整示例。 该示例使用`application/json`消息的内容类型，但您可以更改此设置需要时。

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

若要测试，添加队列消息通过[服务总线资源管理器](https://github.com/paolosalvatori/ServiceBusExplorer)等工具。 请参阅函数接收的消息后，立即引发逻辑应用程序。

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
