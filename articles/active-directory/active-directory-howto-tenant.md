<properties
    pageTitle="如何获取 Azure AD 租户 |Microsoft Azure"
    description="Azure Active Directory 租户获得注册和构建应用程序的方式。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>如何获取 Azure Active Directory 租户

在 Azure 活动目录 (AD Azure)[租户](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)是一个组织的代表。  它是 Azure AD 服务组织接收，并拥有当 Microsoft 云服务，例如 Azure，Microsoft Intune 或 Office 365 的其签署的专用的实例。  每个 AD Azure 租户是截然不同且相互独立的其他 Azure AD 承租人。  

租户可容纳公司中的用户和他们的他们的密码、 用户配置文件数据、 权限等有关的信息。  它还可以包含组、 应用程序和其他属于组织和其安全性的信息。

允许 Azure AD 用户登录到您的应用程序，您必须注册您的应用程序在您自己的组织。  在 Azure AD 租户发布应用程序是**完全免费**的。  事实上，大多数开发人员将创建多个承租人和试验，开发，应用程序转移和测试目的。  企业注册和使用您的应用程序可以选择购买许可证，如果他们想要利用高级的目录功能。

因此，如何转有关获取 Azure AD 租户？  该进程可能很少的不同如果您︰

- [具有现有 Office 365 订阅](#use-an-existing-office-365-subscription)
- [具有现有 Azure 订阅 Microsoft 帐户关联](#use-an-msa-azure-subscription)
- [具有现有 Azure 订阅与组织的帐户](#use-an-organizational-azure-subscription)
- [没有任何以上和想要从头开始](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>使用现有的 Office 365 订阅
如果您有现有的 Office 365 订阅，您已经有 Azure AD 租户 ！ 您可以与您的 O365 帐户登录到[Azure 门户](https://portal.azure.com)并开始使用 Azure 的广告。

## <a name="use-an-msa-azure-subscription"></a>使用 MSA Azure 订阅
如果您曾经注册 Azure 的订阅使用单独的 Microsoft 帐户，您已经有一个租户 ！  当您登录到[Azure 门户](https://portal.azure.com)时，您将自动登录到默认组织。 您可以自由地使用本组织根据-但您可能希望创建组织的管理员帐户。

为此，请执行以下步骤。  或者，您可能想要创建一个新的租户，该租户遵循相似的过程中创建管理员。

1.  登录到[Azure 门户](https://portal.azure.com)使用单独的帐户
2.  导航到 （在左侧的导航栏中，在**更多的服务**中找到） 门户"Azure 活动目录"部分
3.  您应该会自动登录到"默认目录"，如果不是您可以通过在您的帐户名，在右上角中单击切换目录。
4.  从**快速任务**部分中，选择**添加用户**。
5.  在添加用户表单中，提供了以下详细信息︰

    - 名称: （选择一个适当的值）
    - 用户名: （选择此管理员的用户名）
    - 配置文件: （名字、 姓氏、 职务和部门为适当的值填充）
    - 角色︰ 全局管理员

6.  当您完成添加用户窗体中，并收到的新的管理用户的临时密码时，请务必记录此密码，因为您将需要使用该新用户登录方能更改密码。 您还可以直接对用户来说，使用其他的电子邮件中发送密码。
7.  在**创建**要创建新用户，请单击。
8.  若要更改该临时密码，登录到[https://login.microsoftonline.com](https://login.microsoftonline.com)用这个新的用户帐户并更改密码请求时。


## <a name="use-an-organizational-azure-subscription"></a>使用组织的 Azure 订阅
如果您曾经注册 Azure 的订阅与您组织的帐户，您已经有一个租户 ！  在[Azure 门户网站](https://portal.azure.com)中，您应该发现租户，当您导航到"其他服务"和"Azure 活动目录。  您可以自由地使用本组织根据。 


## <a name="start-from-scratch"></a>从头开始
如果上述所有杂乱给您，请不要担心。  只需访问[https://account.windowsazure.com/organization](https://account.windowsazure.com/organization)注册 Azure 与新的组织。  完成此过程后，您必须与域名注册时选择了自己非常 Azure AD 租户。  在[Azure 门户](https://portal.azure.com)中，可以通过导航到"Azure Active Directory"左手 nav.中找到您的租户

作为注册 Azure 的过程的一部分，您将需要提供信用卡详细信息。  可以继续，不用担心-您将不会收取在 Azure 广告发布的应用程序或创建新的租户。
