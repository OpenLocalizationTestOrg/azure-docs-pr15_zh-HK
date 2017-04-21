<properties
    pageTitle="添加缓存以提高性能在 Azure API 管理 |Microsoft Azure"
    description="了解如何提高延迟、 带宽占用量和 API 管理服务调用的 web 服务加载。"
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>添加缓存以提高性能在 Azure API 管理

可以为响应缓存配置 API 管理中的操作。 响应缓存可以显著减少 API 延迟、 带宽占用量和 web 服务负载并不经常更改的数据。

本指南介绍了如何添加响应缓存，您的 API 和配置示例回响 API 操作的策略。 然后可以从开发人员门户来验证缓存操作中调用操作。

>[AZURE.NOTE] 通过使用策略表达式的键的缓存项的信息，请参阅[自定义缓存在 Azure API 管理](api-management-sample-cache-by-key.md)。

## <a name="prerequisites"></a>系统必备组件

本指南中的步骤之前，您必须具有一个 API 的 API 管理服务实例和产品配置。 如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

## <a name="configure-caching"></a>配置为缓存的操作

在此步骤中，您可以回顾回响 API 的示例**获得资源 （缓存的）**操作的缓存设置。

>[AZURE.NOTE] 每个 API 管理服务实例来自预配置了一个回声 API，可用于试验和了解 API 管理。 有关详细信息，请参阅[开始使用 Azure API 管理][]。

若要开始，请单击 Azure 经典门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

单击在左侧， **API 管理**菜单中的**Api** ，然后单击**回响 API**。

![回音式 API][api-management-echo-api]

单击**操作**选项卡，然后单击**操作**列表中**获取资源，（缓存的）**操作。

![回音式 API 操作][api-management-echo-api-operations]

单击**缓存**选项卡以查看此操作的缓存设置。

![高速缓存选项卡][api-management-caching-tab]

若要启用缓存的操作，选择**启用**复选框。 在此示例中，启用缓存。

每个操作响应被键控，基于**查询字符串参数的变化**和**变化的标头**字段中的值。 如果您希望缓存多个基于查询字符串参数或头的响应，可以在这两个字段中进行配置。

**持续时间**指定缓存的响应的过期间隔。 在此示例中，间隔为**3600**秒，等效于一个小时。

在此示例中，使用缓存的配置，**获得资源 （缓存的）**操作的第一个请求从后端服务返回的响应。 将缓存此响应，关键字指定的标头和查询字符串参数。 随后调用该操作，具有匹配的参数，将缓存的响应的缓存持续时间间隔已过期之前返回。

## <a name="caching-policies"></a>查看缓存策略

在此步骤中，您可以查看示例回响 API 的**获得资源 （缓存的）**操作的缓存设置。

缓存设置配置为**缓存**选项卡上的操作，操作会添加缓存策略。 这些策略都可以查看和编辑策略编辑器中。

从**API 管理**菜单的左侧，单击**策略**，然后选择**回声 API / 获得资源 （缓存的）**从**操作**下拉列表。

![策略的作用域操作][api-management-operation-dropdown]

这在策略编辑器中显示此操作的策略。

![API 管理策略编辑器][api-management-policy-editor]

此操作的策略定义包括的策略定义的缓存配置，使用上一步中的**缓存**选项卡进行了考察。

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] 缓存策略在策略编辑器中所做的更改将反映在相反的操作，以及**缓存**选项卡上。

## <a name="test-operation"></a>调用的操作和测试缓存

若要查看缓存的作用，我们可以从开发人员门户中调用操作。 顶部右菜单中单击**开发人员门户**。

![开发人员门户][api-management-developer-portal-menu]

在顶部的菜单中，单击**Api** ，然后选择**回响 API**。

![回音式 API][api-management-apis-echo-api]

>如果您有一个 API 配置或可见到您的帐户，然后单击 Api 使您直接进入该 API 的操作。

选择**获取资源 （缓存的）**操作，然后单击**打开控制台**。

![打开的控制台][api-management-open-console]

控制台允许您调用操作直接从开发人员门户。

![控制台][api-management-console]

请为**参数 1** ，**参数 2**的默认值。

从**订阅项**下拉列表中选择所需的密钥。 如果您的帐户有只有一个订阅，它已被选中。

**请求标头**的文本框中输入**sampleheader:value1** 。

单击**HTTP Get** ，记下的响应标头。

**请求标头**的文本框中输入**sampleheader:value2** ，然后单击**HTTP Get**。

请注意， **sampleheader**的值仍然为**value1**响应中。 尝试一些不同的值，请注意，返回第一个调用的缓存的响应。

在**参数 2**字段中，输入**25** ，然后单击**HTTP Get**。

请注意， **sampleheader**在响应中的值现在**值 2**。 操作结果的关键字查询字符串，因为未返回以前缓存的响应。

## <a name="next-steps"></a>下一步行动

-   有关缓存策略的详细信息，请参阅[API 管理策略引用][]中的[缓存策略][]。
-   通过使用策略表达式的键的缓存项的信息，请参阅[自定义缓存在 Azure API 管理](api-management-sample-cache-by-key.md)。

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md

[API 管理策略参考]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[缓存策略]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
