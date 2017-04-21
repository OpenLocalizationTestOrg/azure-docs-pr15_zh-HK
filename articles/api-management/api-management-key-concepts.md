<properties 
    pageTitle="API 管理的关键概念" 
    description="了解有关 Api、 产品、 角色、 组和其他 API 管理的关键概念。" 
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
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>什么是 API 管理？

API 管理可帮助组织发布到外部 Api、 伙伴和内部开发人员来解锁他们的数据和服务的潜力。 企业无处不在是希望扩展为一个数字平台运营、 创建新通道、 寻找新用户并推动与现有的更深层次合作。 API 管理提供了核心能力，以确保成功的 API 程序通过开发人员接洽、 业务洞察力、 分析、 安全和保护。

观看下面的视频的 Azure API 管理概述，并了解如何使用 API 管理可以将多个功能添加到您的 API，包括访问控制、 速率限制、 监视、 事件日志记录和响应缓存，您的最小工作。

> [AZURE.VIDEO azure-api-management-overview]

若要使用 API 管理，管理员创建 Api。 每个 API 包含一个或多个操作，每个 API 可以添加到一个或多个产品。 若要使用的 API，订阅产品包含该 API，开发人员，然后它们可以调用 API 的操作，如有任何可能生效的使用策略。

本主题概述了 API 管理的关键概念。

>[AZURE.NOTE] 有关详细信息，请参阅[云 API 管理︰ 利用 Api 电源](http://j.mp/ms-apim-whitepaper)PDF 白皮书。 介绍了 API 的管理 — 通过 CITO 研究此简介白皮书︰ 
>
> - 公共 API 要求和难题
>     - 分离的 Api 和呈现外观
>     - 开发人员启动并快速运行
>     - 加强访问安全
>     - 分析和指标
> - 控制并使用 API 管理平台的洞察力
> - 使用与后端的云解决方案
> - Azure API 管理

## <a name="apis"> </a>Api 和操作

Api 是 API 管理服务实例的基础。 每个 API 表示一组开发人员可用的操作。 每个 API 包含参考实现 API，后端服务和其操作映射到后端服务实现的操作。 操作 API 管理中的是高度可配置的对 URL 映射、 查询和路径参数、 请求和响应的内容和操作响应缓存的控制。 此外可以在 API 级别或单个操作级别实现速率限制、 配额和 IP 限制策略。

有关详细信息，请参阅[如何创建 Api][] ，以及[如何添加操作的 api][]。


## <a name="products"></a>产品

产品是 Api 于开发人员的出现方式。 API 管理中的产品具有一个或多个 Api，并配置了标题、 描述和使用条款。 产品可以是**开放**的或**受保护**。 受保护的产品必须订阅了，才可以使用，打开时可以不订阅使用的产品。 产品可供开发人员使用时才能发布。 一旦发布，它可以查看 （和在受保护的产品的情况下同意） 的开发。 订阅电子审批流程在产品级别配置和可以需要管理员核准，或者被自动批准。

组用于管理产品给开发人员的可见性。 产品授予组的可见性，开发人员可以查看和订阅的产品，都可以看到在它们所属的组。 

有关详细信息，请参阅[创建和发布产品][]和下面的视频。

> [AZURE.VIDEO using-products]

## <a name="groups"></a>组

组用于管理产品给开发人员的可见性。 API 管理有以下不可变的系统组。

-   **管理员**-Azure 预订管理员是此组的成员。 管理员管理 API 管理服务实例，创建 Api、 操作和开发人员所使用的产品。
-   **开发人员**的经过身份验证的开发人员门户用户属于此组。 开发人员可以使用您的 Api 构建应用程序的客户。 开发人员对开发人员门户进行访问并生成调用操作的 API 的应用程序。
-   **客人**的未经身份验证的开发人员门户的用户，如潜在客户访问开发人员门户的 API 管理实例属于此组。 他们可以授予某些只读访问权限，如查看 Api，但不是调用它们的能力。

除了这些系统组，管理员可以创建自定义组或[利用外部组中关联的 Azure Active Directory 承租人](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group)。 可以与系统组让开发人员可见性和访问 API 产品同时使用自定义和外部组。 例如，可以为特定的伙伴组织与相关的开发人员创建一个自定义组，允许它们访问 Api 从产品包含相关的 Api。 用户可以是多个组的成员。

有关详细信息，请参阅[如何创建和使用组][]。

## <a name="developers"></a>开发人员

开发人员表示 API 管理服务实例中的用户帐户。 开发人员可以创建或邀请加入由管理员，或者它们可以从[开发人员门户][]注册。 每个开发人员将一个或多个组的成员，并且可以订阅授予这些组的可见性的产品。

当开发人员订阅产品授予产品的主、 辅密钥。 进行产品的 Api 调用时使用此参数。

有关详细信息，请参阅[如何创建或邀请开发人员][]，以及[如何将组与开发人员相关联][]。

## <a name="policies"></a>策略

策略是管理的一个强大的功能的 API 允许发布者更改通过配置 API 的行为。 策略是在请求按顺序执行的语句的集合或一个 API 的响应。 常用语句包含 XML 格式转换为 JSON 并调用速率限制限制的开发人员，从传入呼叫量和很多其他的策略都将显示。

策略表达式可作为特性值或文本值放到任何 API 管理策略，除非该策略指定，否则。 某些策略，如[控制流](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose)和[将变量设置](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable)策略基于策略表达式。 有关详细信息，请参阅[高级策略](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies)，[策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)，并观看下面的视频。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

API 管理策略的完整列表，请参阅[策略参考][]。 有关使用和配置策略的详细信息，请参阅[API 管理策略][]。 速率限制和配额策略创建产品的教程，请参阅[如何创建和配置高级的产品设置][]。 有关演示，请参见下面的视频。

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a>开发人员门户

开发人员门户，开发人员可以了解您的 Api、 查看和调用操作和订阅产品。 潜在客户可以访问开发人员门户中，查看 Api 和操作，并注册。 开发人员门户网站的 URL 位于 Azure 传统门户网站 API 管理服务实例中的仪表板上。

通过添加自定义内容、 自定义样式，并添加您的品牌，可以自定义您的开发人员门户的外观和感觉。

## <a name="api-management-and-the-api-economy"></a>API 管理和 API 经济

要了解更多有关 API 管理，观察以下 Microsoft Ignite 2015 会议演示文稿。

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[开发人员门户]: #developer-portal

[如何创建 Api]: api-management-howto-create-apis.md
[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何创建和发布产品]: api-management-howto-add-products.md
[如何创建和使用组]: api-management-howto-create-groups.md
[如何将组与开发人员相关联]: api-management-howto-create-groups.md#associate-group-developer
[如何创建和配置高级的产品设置]: api-management-howto-product-with-rules.md
[如何创建或邀请开发人员]: api-management-howto-create-or-invite-developers.md
[策略引用]: api-management-policy-reference.md
[API 管理策略]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
