<properties
   pageTitle="为可调用的终结点的逻辑应用程序"
   description="如何创建和配置终结点触发器并在 Azure 应用程序服务中的一个逻辑应用程序中使用它们"
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


# <a name="logic-apps-as-callable-endpoints"></a>为可调用的终结点的逻辑应用程序

逻辑应用程序本身可以表现同步的 HTTP 端点为触发器。  可调用端点的模式还可用于为嵌套工作流逻辑的应用程序中的"工作流"操作通过调用应用程序逻辑。

有 3 种类型的触发器，可以接收请求︰

* 请求
* ApiConnectionWebhook
* HttpWebhook

文章的剩余部分，我们将为此示例中，使用**请求**，但所有的原则完全适用于其他两种类型的触发器。

## <a name="adding-a-trigger-to-your-definition"></a>添加到您定义的触发器
第一步是添加到您可以接收传入的请求的逻辑应用程序定义的触发器。  您可以在设计器中为"HTTP 请求"来添加触发器卡中进行搜索。 您可以定义请求正文 JSON 架构和设计器将生成标记可帮助您分析和从手动触发器通过工作流传递数据。  我建议使用像[jsonschema.net](http://jsonschema.net)一样的工具来从示例正文负载生成 JSON 架构。

![请求触发器卡][2]

保存您的应用程序逻辑的定义后，会生成回调 URL 类似于此︰
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

该 URL 包含 SAS 键中用于身份验证的查询参数。

也可以在 Azure 门户中获取此终结点︰

![][1]

或者，通过调用︰

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>触发器 URL 的安全

安全地使用共享访问签名生成逻辑应用程序回调 Url。  签名作为查询参数传递，将会触发逻辑应用程序之前，必须进行验证。  它是通过每个应用程序逻辑、 触发器和正在执行的操作的秘密密钥的唯一组合生成的。  除非有人秘密逻辑应用程序注册表项的访问，他们将不能生成有效的签名。

## <a name="calling-the-logic-app-triggers-endpoint"></a>调用触发器逻辑应用程序终结点

一旦您已经创建的触发器的终结点，可以触发它通过`POST`的完整 url。 您可以在正文中包括其他头文件，以及任何内容。

如果内容类型为`application/json`，然后您将能够从请求中的引用属性。 否则，它将作为一个可以传递给其他 Api，但不能在工作流引用而无需将内容转换二进制单元处理。  例如，如果您通过`application/xml`内容可以使用`@xpath()`执行 xpath 抽出，或`@json()`将从 XML 转换为 JSON。  使用内容的详细信息类型[可以是以下位置找到](app-service-logic-content-type.md)

此外，您可以在定义中指定一个 JSON 架构。 这会导致设计器，以生成标记，然后将传递到的步骤。  例如将使以下`title`，`name`在设计器中可用的标记︰

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>引用的传入请求的内容

`@triggerOutputs()`函数将输出传入请求的内容。 例如，它将如下所示︰

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

您可以使用`@triggerBody()`快捷方式来访问`body`属性明确。 

## <a name="responding-to-the-request"></a>对该请求作出响应

对于某些启动逻辑应用程序的请求，可能需要对调用方具有一些内容作出响应。 没有新的操作类型称为**响应**可用于构造状态代码、 正文和标题您的响应。 请注意，如果没有**响应**形状存在，逻辑应用程序终结点将*立即*以**202 接受**响应。

![HTTP 响应操作][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

响应具有下列项目︰

| 属性 | 说明 |
| -------- | ----------- |
| 状态代码 | 要对传入请求进行响应的 HTTP 状态代码。 它可以是任何有效状态代码 2xx、 4xx 或 5xx 开头。 不允许 3xx 状态代码。 | 
| 正文 | 一个主体对象，它可以是一个字符串或 JSON 对象，甚至是二进制前一步骤中所引用的内容。 | 
| 邮件头 | 您可以定义任意数量的要包含在响应标头 | 

所有所需的响应在应用程序中的逻辑步骤必须在接收响应**除非作为嵌套的逻辑应用程序被调用工作流**的原始请求*60 秒*内完成。 如果没有响应操作已达到内 60 秒，然后传入请求将超时并收到**408 客户端超时**HTTP 响应。  为嵌套逻辑应用程序的父应用程序逻辑将继续等待响应，直到结束，而不考虑的时间越长。

## <a name="advanced-endpoint-configuration"></a>高级的终结点配置

逻辑应用程序提供了内置支持直接访问终结点，并总是使用`POST`方法来启动逻辑的应用程序的运行。 以前**HTTP 侦听程序**API 的应用程序还支持更改 URL 分段和 HTTP 方法。 您可以通过将其添加到 API 应用程序主机 （Web 应用程序承载 API 的应用程序） 的甚至设置额外的安全或自定义的域。 

此功能可通过**API 管理**︰
* [更改请求的方法](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [更改请求的 URL 片段](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* 设置您的 API 管理域经典 Azure 门户中的**配置**选项卡
* 设置要用于基本身份验证 （**需要链接**） 检查策略

## <a name="summary-of-migration-from-2014-12-01-preview"></a>从 2014年-12-01-预览迁移的摘要

|  2014-12-01-预览 | 2016-06-01 |
|---------------------|--------------------|
| 在**HTTP 侦听程序**API 的应用程序上单击 | 单击**手动触发器**（没有 API 的应用程序所需） |
| 设置"*自动发送响应*"HTTP 侦听程序 | 或者包括**响应**操作或不在工作流定义 |
| 配置基本或 OAuth 身份验证 | 通过 API 管理 |
| 配置 HTTP 方法 | 通过 API 管理 |
| 配置相对路径 | 通过 API 管理 |
| 引用通过传入的主体`@triggerOutputs().body.Content` | 通过引用`@triggerOutputs().body` |
| 在 HTTP 侦听程序**发送的 HTTP 响应**操作 | 单击在**响应的 HTTP 请求**（没有 API 的应用程序所需）


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
