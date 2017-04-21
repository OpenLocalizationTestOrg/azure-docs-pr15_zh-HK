<properties 
    pageTitle="使用内部部署 Active Directory Azure 应用程序中进行身份验证 |Microsoft Azure" 
    description="了解有关在 Azure 应用程序服务的业务线应用程序的不同选项来使用内部部署 Active Directory 进行身份验证" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>使用内部部署 Active Directory Azure 应用程序中进行身份验证 #

这篇文章演示了如何使用内部部署[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)中的活动目录 (AD) 进行身份验证。 Azure 应用程序承载于云，但没有进行身份验证的方法内部 AD 用户安全。 

## <a name="authenticate-through-azure-active-directory"></a>通过 Azure 的 Active Directory 进行身份验证
Azure Active Directory 租户可以与内部部署目录同步广告。 此方法使 AD 用户从 internet 访问您的应用程序并使用其本地凭据进行身份验证。 此外，Azure 应用程序服务提供了[此方法的交钥匙解决方案](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)。 按钮的单击几下，可以为 Azure 应用程序启用身份验证与目录同步的租户。 这种方法具有以下优点︰

-   不需要在您的应用程序中的任何身份验证代码。 让应用程序服务，请不要为您的身份验证和提供的功能在您的应用程序花费您的时间。
-   [Azure 广告图形 API](http://msdn.microsoft.com/library/azure/hh974476.aspx)从 Azure 应用程序实现对目录数据的访问。
-   提供到[Azure Active Directory 所支持的所有应用程序](/marketplace/active-directory/)，包括 Office 365、 Dynamics CRM Online，Microsoft Intune 和数以千计的非 Microsoft 云应用程序的 SSO。 
-   Azure 的活动目录支持基于角色的访问控制。 到您的代码可以使用的最小更改 [Authorize(Roles="X")] 模式。

若要了解如何编写使用 Azure Active Directory 进行验证业务线的 Azure 应用程序，请参阅[创建使用 Azure Active Directory 验证业务线的 Azure 应用程序](web-sites-dotnet-lob-application-azure-ad.md)。

## <a name="authenticate-through-an-on-premises-sts"></a>通过内部 STS 进行身份验证
如果您的内部安全令牌服务 (STS) 如 Active Directory 联合身份验证服务 (AD FS)，可以使用的联盟的 Azure 应用程序的身份验证。 这种方法最好，如果公司策略不允许在 Azure 存储广告数据。 但是，请注意以下事项︰

-   STS 拓扑必须部署的内部，使用成本和管理开销。
-   唯一的 AD FS 管理员可以配置[信赖方信任和声明的规则](http://technet.microsoft.com/library/dd807108.aspx)，这可能会限制开发人员的选项。 另一方面，就可以管理和自定义[声明](http://technet.microsoft.com/library/ee913571.aspx)在每个应用程序的基础上。
-   访问内部对 AD 数据需要通过公司防火墙单独的解决方案。

若要查看如何使用内部 STS 编写验证业务线的 Azure 应用程序，请参阅[创建 AD FS 身份验证与业务线的 Azure 应用程序](web-sites-dotnet-lob-application-adfs.md)。
 
