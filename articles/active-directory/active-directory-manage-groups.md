<properties
    pageTitle="使用 Azure Active Directory 组管理对资源的访问 |Microsoft Azure"
    description="如何在 Azure Active Directory 中使用组来管理用户对内部和云应用程序和资源的访问。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>使用 Azure Active Directory 组管理对资源的访问

Azure 活动目录 (AD Azure) 是全面的身份和访问管理解决方案，它提供一套可靠的能力，以管理对内部和云应用程序和资源，包括 Microsoft Office 365 和 Microsoft SaaS 应用程序的世界等的在线服务的访问。 本文概述，但如果您想要开始使用 Azure AD 组，请按照[在 Azure 广告管理安全组](active-directory-accessmanagement-manage-groups.md)中的说明进行操作。 如果您想要看一下如何使用 PowerShell 管理在 Azure Active directory 组您可以阅读更多的[组管理 Azure Active Directory 预览 cmdlet](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)。


> [AZURE.NOTE] 若要使用 Azure Active Directory，您需要 Azure 帐户。 如果您没有帐户，则可以[注册一个免费的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。


在 Azure 的广告，主要的功能之一是能够管理对资源的访问。 这些资源可以是一部分的目录，如管理通过角色中的目录或目录，如 SaaS 应用程序、 Azure 服务和 SharePoint 网站或对内部资源与外部资源的对象的权限的情况。 有四种方法可将用户分配到的资源的访问权限︰


1. 直接分配

    用户可以直接对资源分配由该资源的所有者。

2. 组成员身份

    一个组可以被分配给资源由资源的所有者，并通过这种方式，授予访问资源的组的成员。 然后可以按组的所有者管理组的成员身份。 实际上，资源所有者委托将用户分配给组的所有者对其资源的权限。

3. 基于规则的

    资源所有者可以使用规则来表示应将分配给的用户访问资源。 规则的结果取决于该规则和它们的值中使用的特定用户的属性，这样，资源所有者有效委托管理权威来源于其资源对规则中使用的属性的访问权限。 资源所有者仍管理本身的规则，并确定哪些属性和值提供对其资源的访问。

4. 外部机构

    对资源的访问权限被从外部源;例如，一组从权威的来源，例如内部目录或如工作日 SaaS 应用程序同步。 资源所有者分配的组提供对资源的访问和外部源管理组的成员。

  ![访问管理图的概述](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>观看视频，解释访问管理

您可以观看详细解释这一个简短视频︰

**动态组的成员资格的 azure 广告︰ 简介**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>如何 does 访问管理 Azure Active Directory 工作吗？
在 Azure 广告的中心访问管理解决方案是安全组。 使用安全组来管理对资源的访问是众所周知的范例，从而提供了一种灵活且易于理解的方法，为预期的用户组提供对资源的访问。 资源所有者 （或目录管理员） 可以分配一组提供直接到他们所拥有的资源的访问。 该组的成员将获得访问权限，并且资源所有者可以委派给其他人，如部门经理或帮助台管理员组的成员列表管理权限。

![Azure Active Directory 访问管理图](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

组的所有者还可以使该组可用于自助服务请求。 这样，最终用户可以搜索并找到组和发出请求加入，有效地寻求通过组访问进行管理的资源的权限。 组的所有者可以对组进行设置，以便加入请求自动批准或由所有者组的批准。 当用户请求加入一组，加入请求将被转发到组的所有者。 如果所有者之一批准该请求，发出请求的用户会收到通知，用户加入到组。 如果一个物主拒绝该请求，发出请求的用户通知，但未加入到组。


## <a name="getting-started-with-access-management"></a>访问管理入门
准备好开始了吗？ 您应该尝试一些可以使用 Azure AD 组完成的基本任务。 使用这些功能以便为您的组织中不同资源的专用的访问不同组的人员。 下面列出了一份基本的第一步。

* [创建一个简单的规则来配置动态组的成员身份](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [使用组来管理对 SaaS 应用程序的访问](active-directory-accessmanagement-group-saasapps.md)

* [提供一组用于最终用户自助服务](active-directory-accessmanagement-self-service-group-management.md)

* [同步到 Azure 使用 Azure AD 连接的内部组](active-directory-aadconnect.md)

* [管理组的所有者](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>下一步行动来访问管理
既然已经理解访问管理的基本知识，下面是一些其他高级的功能 Azure Active Directory 中可用来管理您的应用程序和资源的访问。

* [使用属性可创建高级的规则](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [在 Azure AD 管理安全组](active-directory-accessmanagement-manage-groups.md)

* [在 Azure 广告设置专用组](active-directory-accessmanagement-dedicated-groups.md)

* [Graph API 引用组](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [配置组设置 azure Active Directory cmdlet](active-directory-accessmanagement-groups-settings-cmdlets.md)
