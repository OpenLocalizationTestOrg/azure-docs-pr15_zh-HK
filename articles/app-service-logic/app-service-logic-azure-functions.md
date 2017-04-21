<properties
   pageTitle="与逻辑应用程序使用 Azure 函数 |Microsoft Azure"
   description="了解如何使用应用程序逻辑的 Azure 函数"
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
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>与逻辑应用程序使用 Azure 的函数

您可以通过逻辑应用程序内使用 Azure 函数的运行 C# 或 node.js 的自定义代码段。  [Azure 函数](../azure-functions/functions-overview.md)提供无服务器的 Microsoft Azure 中计算。 这可用于执行以下任务︰

* 高级格式设置或计算的逻辑应用程序内的字段
* 执行工作流中的计算
* 扩展功能的逻辑应用程序与 C# 或 node.js 中支持的功能

## <a name="create-a-function-for-logic-apps"></a>对于逻辑的应用程序创建的函数

我们建议使用**泛型 Webhook-节点**或**一般的 Webhook-C#**模板在 Azure 函数门户中创建新函数。 这自动填入接受一个模板`application/json`从逻辑应用程序。  如果您选择的**集成**选项卡在 Azure 函数应有**模式**设置为**Webhook**和**Webhook 类型**的**泛型 JSON**。  自动发现并列出下逻辑应用程序设计器中使用这些模板函数**在我地区的 Azure 函数。**

Webhook 函数接受请求并将其传递到该方法通过`data`变量。 可通过使用点表示法，如您的负载的属性`data.foo`。  例如，一个简单的 JavaScript 函数，将日期/时间值转换为日期字符串查找如下例所示︰

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>从逻辑应用程序调用 Azure 函数

在设计器中，如果您单击**操作**菜单中，您可以选择**在我地区的 Azure 函数**。  这列出了您的订阅中的容器，并允许您选择要调用的函数。  

选择函数后，会提示您指定一个输入有效负载。 这是逻辑的应用程序将发送到该函数，该消息，它必须是一个 JSON 对象。 例如，如果您想要传递的**最后一次修改**日期从队伍触发器，函数有效负载可能如下所示︰

![最后的 modfied 日期][1]

## <a name="trigger-logic-apps-from-a-function"></a>从函数中返回触发器逻辑应用程序

也很可能会触发逻辑应用程序从一个函数中。  若要执行此操作，只需用手动触发器创建逻辑应用程序。 有关详细信息，请参阅[为可调用的终结点的逻辑应用程序](app-service-logic-http-endpoint.md)。  然后，在您的函数生成 HTTP 发送带有您想要发送到的逻辑应用程序负载的手动触发 url。

### <a name="create-a-function-from-the-designer"></a>在设计器中创建函数

您还可以创建从设计器中的 node.js webhook 函数。 首先，选择**地区，在 Azure 函数**，然后选择您的函数的容器。  如果您还没有一个容器，您需要创建一个从[Azure 函数入口](https://functions.azure.com/signin)。 然后选择**新建**。  

若要生成的数据，可计算出所基于的模板，指定要传递到一个函数的上下文对象。 这必须是一个 JSON 对象。 例如，如果您传递一个 FTP 操作从文件内容中，上下文有效载荷将如下所示︰

![上下文的有效负载][2]

>[AZURE.NOTE] 此对象未被强制转换为字符串，因为该内容将直接添加到 JSON 负载。 但是，如果它不是一个 JSON 标记 （即字符串或 JSON 对象/阵列） 会出错误。 将其强制转换为字符串，只是在这篇文章中第一张图中所示添加引号。

然后，该设计器生成一个函数模板，您可以创建内联。 预创建基于计划要传递给该函数的上下文变量。




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
