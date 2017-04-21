<properties
    pageTitle="自定义域名 Azure Active Directory 中的概念性概述 |Microsoft Azure"
    description="介绍了在 Azure Active directory，包括用于单一登录的联合身份验证中使用自定义域名的概念框架"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>自定义域名 Azure Active Directory 中的概念性概述

域名是许多目录资源标识符的一个重要部分︰ 它是用户组的地址的一部分用户名或电子邮件地址的一部分并可以是应用程序的应用程序 ID URI 的一部分。 在 Azure 活动目录 (AD Azure) 资源可以包括已验证包含该资源的目录拥有一个域名。 只有全局管理员可以在 Azure AD 中执行域管理任务。

在 Azure 广告中的域名都是全局唯一的。 可由一个单一的 Azure 广告使用域名。 一个 Azure 的广告目录已经验证的域名称，如果没有其他 Azure 的广告目录可以验证或使用该相同的域名。

## <a name="initial-and-custom-domain-names"></a>初始和自定义域名

在 Azure 广告每个域名是初始的域名或自定义的域名。

每个 Azure AD 附带了窗体 contoso.onmicrosoft.com 中初始域名。 此第三级域名，在此示例中"contoso.onmicrosoft.com，"建立该目录创建时，通常由管理员创建该目录。 不能更改或删除目录初始的域名。 初始的域名，而完全起作用，主要用于之前验证自定义域名用作引导机制。

在大多数生产环境中，一个目录都有至少一个经过验证的自定义域，如"contoso.com"，并且该域对最终用户可见的自定义。 自定义域名是域名的拥有和使用的该组织，如"contoso.com"，以用于承载的网站等。 此域名是用户名的熟悉的员工，因为是用户名的他们使用，以用于登录到企业网络，或发送和检索电子邮件的一部分。

由 Azure 广告之前，必须添加到您的目录并验证的自定义域名。

## <a name="verified-and-unverified-domain-names"></a>经验证和未经验证的域名

为已验证的 Azure AD 隐式计算初始域目录名称。 当管理员到 Azure 广告添加自定义域名时，它处于最初未验证状态。 Azure 的广告将不允许任何目录资源使用未经验证的域名。 这将确保只有一个目录可以使用一个特定的域名，并组织使用的域名称实际拥有该域名。

Azure AD 验证域名的所有权通过查看特定域的域名的名称服务 (DNS) 区域文件中的项。 要验证域名的所有权，请管理员获取 DNS 条目从 Azure AD Azure 广告将寻找机会，并将该条目添加到域名的 DNS 区域文件。 由该域的域名称注册方维护 DNS 区域文件。 若要验证域的步骤所示[添加自定义到 Azure 的广告目录域](active-directory-add-domain.md)的文章。

将 DNS 条目添加到域名的区域文件不会影响其他域服务，如电子邮件或 web 托管。

## <a name="federated-and-managed-domain-names"></a>联合和托管域名

Azure AD 中的自定义域名可以配置为用户提供了一个联合的号内部部署 Active Directory 和 Azure 的广告之间的经验。 配置联合身份验证的域需要更新到 Azure AD 中的特权资源以及 Windows 服务器活动目录。 配置联盟的域必须完成从 Azure AD 连接或使用 PowerShell。 无法从 Azure 经典门户启动联盟自定义域。 [观看此视频以了解如何配置 AD FS 的 Azure AD 连接的用户登录](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)。

域不联盟有时被称为托管的域。 Azure 的广告目录的初始域隐式计算为托管域。

## <a name="primary-domain-names"></a>主要域的名称

目录的主域名是预先选定为的默认值为域部分的用户名称，当管理员创建新用户在[Azure 的传统门户网站](https://manage.windowsazure.com/)或另一个门户网站，如 Office 365 管理门户的域名称。 目录可以有一个主域名。 管理员可以更改主域名是没有联合的自定义域的任何验证或初始域。

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>在 Azure 广告中的域名和其他 Microsoft Online Services

在 Azure 广告之前由其他 Microsoft 在线服务，比如 Exchange Online，SharePoint Online Intune 必须验证域名。 这些其他服务通常需要由管理员来添加一个或多个特定于服务的 DNS 条目。

Azure 的 web 应用程序使用其自己的机制来验证域的所有权。 必须使用 Azure AD 验证域，即使它以前已用于通过 Azure 的 web 应用程序依赖于该 Azure 广告预订中。 Azure 的 web 应用程序可以使用域名保护 web 应用程序目录中的不同目录中已被确认。

## <a name="managing-domain-names"></a>管理域的名称

从 Azure 的传统门户网站和 PowerShell，域管理任务可以完成。 （在公共预览） 使用 Azure 广告图形 API，可以完成许多任务。

-   [添加和验证自定义域名](active-directory-add-domain.md)

-   [在 Azure 的传统门户网站中管理域](active-directory-add-manage-domain-names.md)

-   [使用 PowerShell 在 Azure AD 管理域名称](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [使用 Azure 广告图形 API 在 Azure AD 管理域名称](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
