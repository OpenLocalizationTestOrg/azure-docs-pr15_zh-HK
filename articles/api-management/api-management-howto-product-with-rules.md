<properties
    pageTitle="保护您的 API 使用 Azure API 管理 |Microsoft Azure"
    description="了解如何保护您的 API 使用配额和限制 （速率限制） 的策略。"
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>保护您的 API 使用 Azure API 管理的速率限制

本指南介绍通过使用 Azure API 管理配置速率限制和配额策略添加保护您的后端 API 是多么容易。

在本教程中，您将创建使开发人员能够对您的 API 进行每分钟，每周 200 电话最多达 10 次调用使用的[每个订阅限制调用速率](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate)和[每个订阅设置使用配额](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)策略"免费试用"API 产品。 然后，您将发布 API 和测试速率限制策略。

使用[通过密钥速率限制](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey)和[配额由密钥](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey)策略更高级的调节方案，请参阅[高级的申请使用 Azure API 管理限制](api-management-sample-flexible-throttling.md)。

## <a name="create-product"></a>以创建产品

在此步骤中，您将创建一个免费试用版产品，不需要订阅批准。

>[AZURE.NOTE] 如果已经具有配置的产品并且想要将它用于本教程，可以跳转到[配置调用速率限制和配额策略][]，并从那里使用您的产品的免费试用版产品代替使用本教程。

若要开始，请单击在 Azure 经典 API 管理服务的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[在 Azure API 管理您第一个 API 管理][]教程中参阅[创建 API 管理服务实例][]。

单击在左侧显示**产品**页**API 管理**菜单中的**产品**。

![添加产品][api-management-add-product]

单击以显示**添加新产品**对话框中**添加产品**。

![添加新产品][api-management-new-product-window]

在**标题**框中，键入**免费试用**。

在**说明**框中，键入以下文本︰ **订阅服务器将能够运行 10 个电话/分钟最多 200 电话/周后访问被拒绝。**

在 API 管理产品可以保护或打开。 受保护的产品必须订阅了，才可以使用。 不订阅的情况下，可以使用打开产品。 确保**需要订阅**选择，以创建一个受保护的产品，要求订阅。 这是默认设置。

如果您希望管理员审阅并接受或拒绝订阅到该产品的尝试，选择**需要订阅审批**。 如果未选中复选框，则订阅尝试将自动批准。 在此示例中，订阅是自动批准，因此没有选中此框。

若要允许开发人员帐户，若要订阅多个新产品，选择**允许多个同时订阅**复选框。 本教程不使用多个同步订阅，因此保留未选中状态。

输入的所有值后，单击**保存**可以创建产品。

![添加产品][api-management-product-added]

默认情况下，新产品是在**管理员组**的用户可见。 我们将添加的**开发人员**组。 **免费试用版**，请单击，然后单击**可见性**选项卡。

>在 API 管理组用于管理产品给开发人员的可见性。 产品授予组的可见性，开发人员可以查看和订阅的产品，都可以看到在它们所属的组。 有关详细信息，请参阅[如何创建并使用在 Azure API 管理组][]。

![添加开发人员组][api-management-add-developers-group]

选择**开发人员**复选框，然后单击**保存**。

## <a name="add-api"></a>为产品添加 API

在本教程的这一步，我们将回显 API 新的免费试用版产品。

>每个 API 管理服务实例都预先配置了与 Echo API，可用于试验和了解 API 管理。 有关详细信息，请参阅[管理 Azure API 管理在您第一组 API][]。

从**API 管理**菜单的左侧，单击**产品**，然后单击配置该产品的**免费试用版**。

![配置产品][api-management-configure-product]

单击**添加到产品的 API**。

![向产品中添加 API][api-management-add-api]

选择**回响 API**，然后再单击**保存**。

![添加回音 API][api-management-add-echo-api]

## <a name="policies"></a>配置调用速率限制和配额策略

在策略编辑器中配置速率限制和配额。 在左侧的**API 管理**菜单下单击**策略**。 在**产品**列表中，单击**免费试用**。

![产品策略][api-management-product-policy]

单击要导入的策略模板，并开始创建速率限制和配额策略的**添加策略**。

![添加策略][api-management-add-policy]

若要插入策略，将光标定位到的**入站**或**出站**部分的策略模板。 速率限制和配额策略是入站的策略，因此将光标定位在元素中入站。

![策略编辑器][api-management-policy-editor-inbound]

我们将在本教程中添加两个策略为[每个订阅限制调用速率](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate)和[设置每个订阅使用配额](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)策略。

![策略声明][api-management-limit-policies]

将光标定位在**入站**的策略元素后，单击**每个订阅限制调用速率**插入其策略模板旁边的箭头。

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**每个订阅限制调用速率**可以在产品级别使用，也可以使用 API 和单个操作名称级别。 在本教程中，习惯于只产品级别的策略，这样的**api**和**操作**元素从元素中删除**速率限制**，因此只有外部**速率限制**元素保留下来，如下面的示例中所示。

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

在免费试用版产品中，最大允许调用速率为每分钟 10 次调用，因此输入**10**作为**调用**属性的值和**60**的**续订期**特性。

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

若要配置的**设置每个订阅使用配额**策略，将光标放在**入站**的元素中，该新添加的**速率限制**元素的紧下方，然后单击**设置每个订阅使用配额**的左侧的箭头。

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

因为该策略也适用在产品级别，删除**api**和**操作**名称元素，如下面的示例中所示。

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

配额可基于每个时间间隔和 / 或带宽，调用的次数。 在本教程中，我们不会限制根据的带宽，因此删除**带宽**特性。

    <quota calls="number" renewal-period="seconds">
    </quota>

在免费试用版产品中，配额是每周 200 调用。 指定**调用**属性的值为**200** ，然后指定一个**续订周期**属性的值为**604800** 。

    <quota calls="200" renewal-period="604800">
    </quota>

>以秒为单位指定策略时间间隔。 若要计算的每周间隔，您可以乘以天数 (7) 按每小时 (60) (60) 一分钟中的秒数的分钟数一天 (24) 小时数︰ 7 *24* 60 * 60 = 604800。

完成配置策略，它应该与下面的示例匹配。

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

配置所需的策略后，单击**保存**。

![保存策略][api-management-policy-save]

## <a name="publish-product"></a>发布产品

既然 Api 添加和配置的策略，以便它可以由开发人员必须先发布该产品。 从**API 管理**菜单的左侧，单击**产品**，然后单击配置该产品的**免费试用版**。

![配置产品][api-management-configure-product]

单击**发布**，然后单击**是，将其发布**确认。

![发布产品][api-management-publish-product]

## <a name="subscribe-account"></a>订阅到该产品的开发人员帐户

既然已经发布了该产品，则可进行订阅和由开发人员使用。

>每种产品都自动订阅 API 管理实例的管理员。 在本教程的步骤中，我们将订阅到免费试用版的产品开发人员非管理员帐户之一。 如果开发者帐户是管理员角色的一部分，然后您可以按照此步骤中，即使您已订阅。

在左边， **API 管理**菜单上单击**用户**，然后单击您的开发人员帐户的名称。 在此示例中，我们将使用**敏 Gragg**开发者帐户。

![配置开发人员][api-management-configure-developer]

单击**添加订阅**。

![添加订阅][api-management-add-subscription-menu]

选择**免费试用版**，然后再单击**订阅**。

![添加订阅][api-management-add-subscription]

>[AZURE.NOTE] 在本教程中，多个同时订阅未启用的免费试用版产品。 时，您将会提示命名订阅，如下面的示例中所示。

![添加订阅][api-management-add-subscription-multiple]

之后，单击**订阅**，该产品将用户的**订阅**列表中出现。

![添加订阅][api-management-subscription-added]

## <a name="test-rate-limit"></a>调用的操作和测试的速率限制

现在，配置并发布免费试用产品，我们可以调用某些操作和测试速率限制策略。
切换到开发人员门户，单击右上角的菜单中的**开发人员门户**。

![开发人员门户][api-management-developer-portal-menu]

在顶部的菜单中，单击**Api** ，然后单击**打开回响 API**。

![开发人员门户][api-management-developer-portal-api-menu]

单击**获取资源**，然后单击**试一试**。

![打开的控制台][api-management-open-console]

保持默认的参数值，然后选择订阅免费试用产品密钥。

![订购密钥][api-management-select-key]

>[AZURE.NOTE] 如果您有多个订阅，请务必选择项**免费试用版**，否则前面的步骤中配置的策略不会生效。

单击**发送**，然后查看响应。 注意**响应状态**的**200 确定**。

![操作结果][api-management-http-get-results]

速度超过每分钟 10 次调用速率限制策略，请单击**发送**。 超出速率限制策略后，则返回**429 种太多的请求**响应状态。

![操作结果][api-management-http-get-429]

**响应内容**表示的剩余时间间隔之前重试次数将会成功。

当每分钟 10 次调用速率限制策略生效时，后续调用将失败，直到第一个经过 60 秒之后的 10 个成功求助前超过了速率限制的产品。 在此示例中，剩余的时间间隔为 54 秒。

## <a name="next-steps"></a>下一步行动

-   观看下面的视频中设置速率限制和配额。

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[管理您的第一个 API 在 Azure API 管理]: api-management-get-started.md
[如何创建和使用 Azure API 管理中的组]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[配置呼叫速率限制和配额策略]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
