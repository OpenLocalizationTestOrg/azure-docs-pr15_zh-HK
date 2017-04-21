<properties
    pageTitle="高级的请求使用 Azure API 管理限制"
    description="了解如何创建并应用灵活的配额和速率限制使用 Azure API 管理策略。"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>高级的请求使用 Azure API 管理限制

能够限制传入的请求是 Azure API 管理的关键角色。 无论是通过控制请求或总请求/数据传输的速率，API 管理允许 API 提供商，以防止滥用其 Api 并创建不同的 API 产品层的值。

## <a name="product-based-throttling"></a>基于产品限制
到目前为止，速度调节功能已被限于正在应用范围限定为特定产品订购 （实质上是键），API 管理发布门户中定义。 这是供应商 API 的开发人员已经注册，可使用他们的 API 上的应用限制非常有用，但是，它没有帮助，例如，在限制单个最终用户的 api。 很可能，对于单个开发人员的应用程序的用户占用整个配额并防止其他客户的开发人员能够使用该应用程序。 此外，一些客户可能会生成大量的请求可能会限制偶尔用户的访问。

## <a name="custom-key-based-throttling"></a>自定义键基于带宽限制
新[通过密钥速率限制](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey)和[配额由密钥](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey)策略提供对通讯控制显著更灵活的解决方案。 这些新的策略，可以定义表达式，来标识可用于跟踪通信使用的键。 易于举例说明其工作原理了。 

## <a name="ip-address-throttling"></a>IP 地址限制
下面的策略限制只有 10 个电话每分钟 10000 千字节为单位的每月带宽 1000000 调用总数与一个单个客户端的 IP 地址。 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

如果在 Internet 上的所有客户机都使用唯一的 IP 地址，这可能是一种限制的用户使用情况的有效方法。 但是，很可能是多个用户将共享单个公用 IP 地址相比，因为它们是通过 NAT 设备上网。 尽管如此，对于允许未经身份验证的访问的 Api`IpAddress`可能是最佳的选择。

## <a name="user-identity-throttling"></a>带宽限制用户标识
如果最终用户进行身份验证，然后调节键可以生成基于信息用于唯一标识该用户。

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

在此示例中，我们提取授权标头、 将其转换为`JWT`对象并使用该标记的主题标识用户并将其用作速率限制密钥。 如果用户标识存储在`JWT`与其他然后声称在原地无法使用该值。

## <a name="combined-policies"></a>组合的策略
尽管新限制策略提供了更多的控制，比现有限制策略，是仍然结合两种功能的值。 按产品的订购密钥 （[通过订阅限制调用速率](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate)和[设置订阅使用配额](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)） 限制是启用 monetizing 的 API 所基于的使用级别的好方法。 能够按用户限制更细致精细的控制起到补充作用，并防止一个用户的行为降低的另一个经验。 

## <a name="client-driven-throttling"></a>客户端驱动调节
调节键定义时使用[策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)，它是如何调节的作用范围就选择的 API 提供。 但是，开发人员可能想要控制它们给多少限制他们自己的客户。 这是可启用由 API 提供商通过引入自定义标头允许开发人员的客户端应用程序通信的 api 密钥。

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

这使开发人员的客户端应用程序可以选择他们希望如何创建速率限制密钥。 少量的独创性与客户端开发者可以通过向用户分配组密钥并旋转的密钥用法创建自己率层。

## <a name="summary"></a>摘要
Azure 的 API 管理提供率和限制保护的同时将值添加到您的 API 服务的报价。 带有自定义的作用域规则的新限制策略允许您更细致的精细的控制这些策略，以使您的客户能够构建更好的应用程序。 本文中的示例演示这些新策略通过制造速率限制客户端 IP 地址、 用户标识和客户端生成的值的键。 但是，有很多其他部分无法使用用户代理、 URL 路径片段、 邮件大小的邮件。

## <a name="next-steps"></a>下一步行动
请给我们一个反馈 Disqus 线程中此主题。 那就太棒以接收有关已被合理的选择方案中其他潜在关键字值。

## <a name="watch-a-video-overview-of-these-policies"></a>观看视频这些策略概述
在这篇文章中介绍的[由密钥速率限制](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey)和[配额由密钥](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey)策略的详细信息，请观看下面的视频。

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
