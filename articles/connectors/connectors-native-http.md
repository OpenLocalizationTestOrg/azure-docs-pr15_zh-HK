<properties
    pageTitle="在应用程序逻辑中添加 HTTP 操作 |Microsoft Azure"
    description="具有属性的 HTTP 操作的概述"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>开始使用 HTTP 操作

使用 HTTP 操作，可以扩展为您的组织的工作流，并通过 HTTP 进行通信的任何终结点。

您可以：

- 在您管理的网站出现故障时激活 （触发器） 的应用程序工作流创建的逻辑。
- 通过 HTTP 来将您的工作流扩展到其他服务与任何终结点进行通信。

若要开始使用 HTTP 操作逻辑应用程序中，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="use-the-http-trigger"></a>使用 HTTP 触发器

触发器是可以用于启动的工作流逻辑应用程序中定义的事件。 [了解更多有关触发器](connectors-overview.md)的信息。

这里是如何设置逻辑应用程序设计器中的 HTTP 触发器示例序列。

1. 逻辑应用程序中添加 HTTP 触发器。
2. 填写您要轮询的 HTTP 端点的参数。
3. 修改重复出现的间隔上它应该轮询的频率。
4. 逻辑应用程序现在触发与每个检查期间返回任何内容。

![HTTP 触发器](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>HTTP 触发器的工作原理

HTTP 触发器调用了 HTTP 端点上的重复时间间隔。 默认情况下，任何 HTTP 响应代码逻辑应用程序运行在少于 300 个结果。 在将 HTTP 调用，以确定是否应触发逻辑应用程序后评估的代码视图中，可以添加条件。 下面是一个示例的 HTTP 返回的状态代码是大于或等于时将触发的触发器`400`。

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

有关 HTTP 触发器参数的完整详细信息可在[MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger)上。

## <a name="use-the-http-action"></a>使用 HTTP 操作

对策是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](connectors-overview.md)信息。

1. 选择**新步骤**按钮。
2. 选择**添加操作**。
3. 操作的搜索框中键入**http**列出 HTTP 操作。

    ![选择 HTTP 操作](./media/connectors-native-http/using-action-1.png)

4. 添加任何所需的 HTTP 调用的参数。

    ![完成 HTTP 操作](./media/connectors-native-http/using-action-2.png)

5. 单击保存工具栏左上的角。 逻辑应用程序会进行保存和发布 （激活）。

## <a name="http-trigger"></a>HTTP 触发器

以下是此连接器支持的触发器的详细信息。 HTTP 连接器有一个触发器。

|触发器|说明|
|---|---|
|HTTP|使 HTTP 调用，并返回响应内容。|

## <a name="http-action"></a>HTTP 操作

以下是该连接器所支持的操作的详细信息。 HTTP 连接器有可能的操作。

|操作|说明|
|---|---|
|HTTP|使 HTTP 调用，并返回响应内容。|

## <a name="http-details"></a>HTTP 的详细信息

下表描述了操作和相应的输出详细信息与使用操作相关联的必需和可选的输入的字段。


#### <a name="http-request"></a>HTTP 请求
以下是该动作，发出出站 HTTP 请求的输入的字段。
A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|方法 *|方法|要使用的 HTTP 谓词|
|URI *|uri|HTTP 请求的 URI|
|邮件头|邮件头|HTTP 标头包含一个 JSON 对象|
|正文|正文|HTTP 请求正文|
|身份验证|身份验证|在[身份验证](#authentication)部分的详细信息|
<br>

#### <a name="output-details"></a>输出详细信息

下面是输出的 HTTP 响应的详细信息。

|属性名称|数据类型|说明|
|---|---|---|
|邮件头|对象|响应标头|
|正文|对象|响应对象|
|状态代码|int|HTTP 状态代码|

## <a name="authentication"></a>身份验证

Azure 应用程序服务的应用程序逻辑功能允许您使用不同类型的身份验证 HTTP 端点。 可以使用**HTTP**、 **[HTTP + Swagger](./connectors-native-http-swagger.md)**，和**[HTTP Webhook](./connectors-native-webhook.md)**连接器使用此身份验证。 以下类型的身份验证进行配置︰

* [基本身份验证](#basic-authentication)
* [客户端证书身份验证](#client-certificate-authentication)
* [Azure 的活动目录 (AD Azure) OAuth 身份验证](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>基本身份验证

对于基本身份验证需要以下身份验证对象。
A * 意味着它是必填的字段。

|属性名称|数据类型|说明|
|---|---|---|
|类型 *|类型|身份验证类型 (必须是`Basic`用于基本身份验证)|
|用户名 *|用户名|进行身份验证的用户名|
|密码 *|密码|密码身份验证|

>[AZURE.TIP] 如果您想要使用的密码，无法从定义，使用`securestring`参数与`@parameters()`[工作流定义函数](http://aka.ms/logicappdocs)。

因此您将身份验证域中创建此类的对象︰

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>客户端证书身份验证

客户端证书身份验证时需要下列身份验证对象。 A * 意味着它是必填的字段。

|属性名称|数据类型|说明|
|---|---|---|
|类型 *|类型|身份验证类型 (必须为`ClientCertificate`的 SSL 客户端证书)|
|PFX *|pfx|个人信息交换 (PFX) 文件的 Base64 编码的内容|
|密码 *|密码|若要访问该 PFX 文件密码|

>[AZURE.TIP] 您可以使用`securestring`参数与`@parameters()`[工作流定义函数](http://aka.ms/logicappdocs)使用不能阅读后保存逻辑应用程序定义中的参数。

例如︰

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth 身份验证

Azure AD OAuth 身份验证需要下列身份验证对象。 A * 意味着它是必填的字段。

|属性名称|数据类型|说明|
|---|---|---|
|类型 *|类型|身份验证类型 (必须为`ActiveDirectoryOAuth`的 Azure AD OAuth)|
|租户 *|租户|Azure AD 租户租户标识符|
|观众 *|访问群体|设置为`https://management.core.windows.net/`|
|客户机 ID *|客户机 Id|Azure AD 应用程序的客户端标识符|
|秘密 *|秘密|客户端请求令牌将机密|

>[AZURE.TIP] 您可以使用`securestring`参数与`@parameters()`[工作流定义函数](http://aka.ms/logicappdocs)使用的参数，将不能保存后定义中阅读。

例如︰

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>下一步行动

现在，试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 可以通过看一看我们的[Api 列表](apis-list.md)浏览其他可用的连接器应用程序逻辑中。
