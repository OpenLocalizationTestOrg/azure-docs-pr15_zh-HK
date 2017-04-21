
<properties
    pageTitle="添加 HTTP + Swagger 逻辑应用程序中的操作 |Microsoft Azure"
    description="概述了 HTTP + Swagger 操作和操作"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http--swagger-action"></a>开始使用 HTTP + Swagger 操作

采用 HTTP + Swagger 操作，您可以创建任何 REST 端点通过[Swagger 文档](https://swagger.io)的第一类连接器。 您还可以扩展逻辑的应用程序调用任何一类的逻辑应用程序设计器体验的 REST 端点。

若要开始使用 HTTP + Swagger 操作中的逻辑的应用程序，请参阅[创建新的逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>使用 HTTP + Swagger 作为触发器或操作

HTTP + Swagger 触发器和操作功能与[HTTP 操作](connectors-native-http.md)相同，但通过[Swagger 元数据](https://swagger.io)API 和输出的形状显示在设计器中提供更好的设计体验。 此外，还可以使用 HTTP + Swagger 为触发器。 如果想要实现轮询触发器，它应按照所述[创建一个自定义的 API 与逻辑应用程序使用](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers)的轮询模式。

[了解更多关于逻辑的应用程序的触发器和操作。](connectors-overview.md)

下面是使用 HTTP + Swagger 操作作为一个逻辑应用程序中的工作流中的操作方法的示例。

1. 选择**新步骤**按钮。
2. 选择**添加操作**。
3. 在操作搜索框中，键入与列表 HTTP + Swagger 操作**swagger** 。

    ![选择 HTTP + Swagger 操作](./media/connectors-native-http-swagger/using-action-1.png)

4. 键入 Swagger 文档的 URL:
    - 要从逻辑应用程序设计器，该 URL 必须是 HTTPS 终结点并启用了 CORS。
    - 如果 Swagger 文档不能满足这一要求，可以使用[与启用了 CORS Azure 存储](#hosting-swagger-from-storage)来存储文档。
5. 单击**下一个**要读取和呈现从 Swagger 文档。
6. 添加任何所需的 HTTP 调用的参数。

    ![完整的 HTTP 动作](./media/connectors-native-http-swagger/using-action-2.png)

1. 在工具栏上，并且您的逻辑应用程序将同时保存的左上角单击**保存**并发布 （激活）。

### <a name="host-swagger-from-azure-storage"></a>从 Azure 存储主机 Swagger

您可能想要引用 Swagger 文档不承载，或者，不能满足安全和跨源设计器的要求。 若要解决此问题，可以在 Azure 存储中存储 Swagger 文档并启用 CORS 引用的文档。  

下面是创建、 配置和 Azure 存储中存储 Swagger 文档的步骤︰

1. [创建使用 Azure Blob 存储 Azure 存储帐户](../storage/storage-create-storage-account.md)。 （若要执行此操作，设置权限为**公共访问**。
2. 启用该 blob CORS。 可以使用[此 PowerShell 脚本](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1)来配置该设置，将自动。
3. 将 Swagger 文件上载到 blob。 从[Azure 门户](https://portal.azure.com)或类似[Azure 存储浏览器](http://storageexplorer.com/)工具，您可以执行此操作。
1. 引用的 HTTPS 链接到 Azure Blob 存储中的文档。 (链接如下格式`https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`。)



## <a name="technical-details"></a>技术详细信息

以下是触发器和操作的详细信息，此 HTTP + Swagger 连接器支持。

## <a name="http--swagger-triggers"></a>HTTP + Swagger 触发器

触发器是可以用于启动的工作流逻辑应用程序中定义的事件。 [了解有关触发器的更多信息。](connectors-overview.md) HTTP + Swagger 连接器有一个触发器。

|触发器|说明|
|---|---|
|HTTP + Swagger|执行 HTTP 调用，并返回响应内容|

## <a name="http--swagger-actions"></a>HTTP + Swagger 操作

对策是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细信息。](connectors-overview.md) HTTP + Swagger 连接器有可能的操作。

|操作|说明|
|---|---|
|HTTP + Swagger|执行 HTTP 调用，并返回响应内容|

### <a name="action-details"></a>操作详细信息

HTTP + Swagger 连接器提供了可能的操作。 下面是有关每个操作、 他们必选和可选的输入的字段和相应的输出详细信息与它们的用法信息。

#### <a name="http--swagger"></a>HTTP + Swagger

协助 Swagger 元数据进行出站 HTTP 请求。
星号 （*） 表示必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|方法 *|方法|若要使用的 HTTP 谓词。|
|URI *|uri|HTTP 请求的 URI。|
|邮件头|邮件头|HTTP 标头包含一个 JSON 对象。|
|正文|正文|HTTP 请求的正文。|
|身份验证|身份验证|要用于请求的身份验证。 [有关详细信息，请参阅 HTTP](./connectors-native-http.md#authentication)。|

**输出详细信息**

HTTP 响应

|属性名称|数据类型|说明|
|---|---|---|
|邮件头|对象|响应标头|
|正文|对象|响应对象|
|状态代码|int|HTTP 状态代码|

### <a name="http-responses"></a>HTTP 响应

在对各种操作的调用时，可能会得到某些响应。 下面是一个表，其中列出了相应的答复和说明。

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误。|

---

## <a name="next-steps"></a>下一步行动

试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)现在。 通过观察我们[的 Api 的列表](apis-list.md)，可以浏览逻辑应用程序中的其他可用连接器。
