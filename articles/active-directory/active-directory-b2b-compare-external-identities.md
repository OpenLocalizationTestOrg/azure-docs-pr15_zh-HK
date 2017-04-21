<properties
   pageTitle="比较用于管理外部标识使用 Azure Active Directory 功能 |Microsoft Azure"
   description="为支持身份验证和授权的外部标识比较协作 Azure 活动目录 B2B、 B2C 和多租户应用程序"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>比较用于管理外部标识使用 Azure Active Directory 功能

除了管理流动的劳动力对 SaaS 应用程序的访问权限，Azure 活动目录 (AD Azure) 可以帮助您的组织与业务合作伙伴共享资源并交付给企业和消费者的应用程序。

## <a name="developing-applications-for-businesses"></a>开发企业应用程序

您提供的服务或应用程序中的，如工资单服务，公司？ Azure 的广告提供了允许您构建的应用程序无缝集成与数以百万计的组织的已配置为部署 Office 365 或其他企业服务的一部分 Azure 广告标识平台。

**示例︰**药品分销商提供医疗用品和保健行业的信息系统。 他们需要字段分析应用程序到医疗机构，并希望的客户能够管理他们自己的身份。 该公司选择了 Azure 广告标识平台作为他们练习管理应用程序，Azure AD 身份及其客户提供 at 符号向上在必要的时候。 有关详细信息，请参阅[Azure Active Directory 开发人员指南 》](active-directory-developers-guide.md)。

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>启用业务合作伙伴对公司资源访问

您是否有业务合作伙伴或其他公司需要访问您的企业资源，如 SharePoint 网站或 ERP 系统的外部？ Azure 的广告使管理员能够授予外部用户 （他们可能或可能不存在在 Azure AD） 单一登录到企业应用程序的访问。 这提高了安全性，因为用户无法访问在离开时，合作伙伴组织中，当您在您的组织内控制访问权限策略。 这也简化了管理，因为您不需要管理外部合作伙伴目录或每个伙伴的联盟关系。

**示例︰**成像公司为零售商提供了照片图像处理服务，而且运行打印信息亭的最大的零售网络。 他们选择 Azure 的广告，以使成千上万的用户在他们的零售业务合作伙伴使用他们自己的凭据来下载最新的合作伙伴市场营销材料和重新排序处理来自外部网络公司的供应商提供的照片。 有关详细信息，请参见[Azure AD B2B 协作](active-directory-b2b-what-is-azure-ad-b2b.md)。

## <a name="developing-applications-for-consumers"></a>开发应用程序的使用者

您是否需要安全且经济高效地发布在线应用程序，例如零售商店前面，数百万的消费者？ Azure 的广告提供了平台，使社会以及用户名/密码登录、 品牌自助服务注册和自助服务密码重置为您的应用程序的使用者。 这增加了方便您的使用者同时降低您的开发人员的负担。

**示例︰**\#1 体育世界需要直接参与了其 450 万个风扇中的特许经营。 为此，他们构建使用 Azure AD 用户身份验证和配置文件存储的移动应用程序。 风扇得到简化的注册和登录到 Facebook 等社交帐户的使用或为用户提供无缝体验跨 iOS 和 Android，使用传统的用户名/密码电话。 基于建立 Azure 的广告平台大大降低自定义代码时给予特许经营定制品牌，并缓解了安全性、 数据泄露和可扩展性方面的顾虑。 有关详细信息，请参阅[Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)。

## <a name="comparison-of-azure-ad-capabilities"></a>Azure 广告能力的比较

每个在这篇文章已经讨论过的方案是在 Azure AD 功能通过解决您的问题。 此表可以帮助阐明哪些功能是与您最相关︰

| **请考虑此产品...**       | [Azure 的广告多租户 SaaS 应用程序](active-directory-developers-guide.md)    | [Azure AD B2B 协作](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **如果我需要提供...** | 为企业服务 | 合作伙伴与我的应用程序的访问  | 为消费者服务 |
| **和我很相似...**  | 制药分发服务器      | 图像处理公司            | 体育特许经营       |
| **正在部署应用的程序...**  | 实践管理     | 外部供应商          | 足球迷的喜爱            |
| **针对...**        | 医生的办公室        | 核准的业务合作伙伴 | 任何人使用电子邮件      |
| **可访问的时...**      | 客户管理员同意 | 我管理的邀请           | 使用者注册      |
