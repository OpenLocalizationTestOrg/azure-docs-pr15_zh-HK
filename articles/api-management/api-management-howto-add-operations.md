<properties 
    pageTitle="如何将操作添加到 API 在 Azure API 管理 |Microsoft Azure" 
    description="了解如何将操作添加到在 Azure API 管理 API。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>如何将操作添加到在 Azure API 管理 API

可以使用 API 管理 API 之前，必须添加操作。 本指南介绍了如何添加和配置 API 管理中的不同类型的 api 操作。

## <a name="add-operation"></a>添加操作

操作是添加和配置为发布者门户中的 API。 若要访问发布者门户，单击**管理**API 管理服务的 Azure 传统门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

出版商门户中选择所需的 API，然后选择**操作**选项卡。 

![操作][api-management-operations]

单击**添加操作**若要添加新的操作。 将显示**新的操作**，将默认选中**签名**选项卡。

![添加操作][api-management-add-operation]

通过从下拉列表中选择指定**的 HTTP 谓词**。

![HTTP 方法][api-management-http-method]

<a name="url-template"></a>

通过键入 URL 片段组成的一个或多个 URL 路径段和零个或多个查询字符串参数中定义的 URL 模板。 追加到 API 的基 URL 的 URL 模板标识单个 HTTP 操作。 它可能包含一个或多个命名由大括号的可变部分。 这些可变的部分称为模板参数，而动态分配的值由 API 管理平台正在处理的请求时从请求的 URL 中提取。

![URL 模板][api-management-url-template]

<a name="rewrite-url-template"></a>

如果需要，指定**重写 URL 模板**。 这使您可以使用标准的 URL 模板用于处理传入的请求，请在前端，通过转换 URL 重写模板根据后端调用时。 应重写模板中使用 URL 模板中的模板参数。 下面的示例演示如何内容编码为 web 服务从前面的示例中的路径段可作为发布通过使用 URL 模板的 API 管理平台的 API 中的查询参数提供的类型。

![URL 模板重写][api-management-url-template-rewrite]

该操作的调用方将使用格式`/customers?customerid=ALFKI`，这将被映射到`/Customers('ALFKI')`当调用后端服务。


**显示**名称和**说明**提供的操作说明，用于在开发人员门户中使用此 API 的开发人员提供的文档。

![说明][api-management-description]

可以为纯文本或 HTML 的**描述**文本框中指定的操作说明。

## <a name="operation-caching"></a>高速缓存操作

响应缓存可以减少认定的 API 的使用者、 降低带宽消耗和减少 HTTP web 上的负载服务实现 API 的等待时间。 

若要轻松快速地启用缓存的操作，选择**缓存**选项卡，选中**启用**复选框。

![缓存][api-management-caching-tab]

**持续时间**指定在此期间操作响应在缓存中保留多长时间。 默认值为 3600 秒或 1 小时。

缓存键用于区分响应，这样，对应每个不同的缓存键的响应将获得自己单独的缓存的值。 （可选），输入特定的查询字符串参数和/或用于分别计算缓存关键字值的**查询字符串参数变化**和**Vary 标头由**文本框中的 HTTP 标头。 当没有指定的完整请求 URL 和缓存密钥生成过程中使用以下 HTTP 标头值︰**接受**和**接受字符集**。

>缓存和缓存策略的详细信息，请参阅[如何在 Azure API 管理缓存操作结果][]。


## <a name="request-parameters"></a>请求参数

参数选项卡上管理操作参数。 在**签名**选项卡上的**URL 模板**中指定的参数将被自动添加，可以只通过编辑 URL 模板更改。 可以手动输入额外的参数。

若要添加新的查询参数，请单击**添加查询参数**并输入以下信息︰

-   **名称**的参数名称。
-   **说明**-（可选） 该参数的简短说明。
-   **类型**的参数类型，在下拉菜单中选择向下。
-   **值**的可分配给该参数的值。 一个值可以标记为默认值 （可选）。
-   **必需**-使该参数成为必需通过选中复选框。 

![请求的参数][api-management-request-parameters]

## <a name="request-body"></a>请求正文

如果该操作允许例如 PUT (开机自检） 并要求主体可能提供一个实例，它在所有受支持的表示形式的格式 (例如 json，XML)。 

>请求正文使用仅用于文档和未通过验证。

要输入请求正文，请切换到**正文**选项卡。

单击**添加表示**，开始键入所需内容类型的名称 (例如应用程序/json)、 在下拉列表中选择它并需的请求正文示例以选定格式粘贴文本框。 

![请求正文][api-management-request-body]

在其他表示形式，您还可以指定可选文字说明的**描述**文本框中。

## <a name="responses"></a>响应

它是最好的操作可能产生的所有状态代码提供的响应示例。 每个状态代码可能有多个响应正文的示例中，另一个用于每个受支持的内容类型。 

要添加响应，请单击**添加**，并开始键入所需的状态代码。 在此示例中的状态代码是**200 确定**。 在下拉列表中显示的代码，一旦选中它，并响应代码是创建并添加到您的操作。

![响应代码][api-management-response-code]

单击**添加表示**，开始键入所需内容类型名称 (如应用程序/json)，然后将它向下选择在下拉菜单中。

![正文内容类型][api-management-response-body-content-type]

将以所选格式的响应正文示例粘贴到文本框。 

![响应正文][api-management-response-body]

如果需要，添加到**说明**文本框中的可选说明。

配置此操作后，单击**保存**。


## <a name="next-steps"></a>下一步行动

一旦 api 添加操作下, 一步是将 API 与产品相关联并将其发布，以便开发人员可以调用的操作。

-   [如何创建和发布产品][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[如何创建和发布产品]: api-management-howto-add-products.md
[如何在 Azure API 管理缓存操作结果]: api-management-howto-cache.md