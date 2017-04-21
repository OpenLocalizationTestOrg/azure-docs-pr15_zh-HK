<properties
    pageTitle="资源调配管理 Azure Active Directory 预览中的企业应用程序的用户 |Microsoft Azure"
    description="了解如何管理用户帐户使用 Azure Active Directory 预览的企业应用程序的资源调配"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>预览︰ 管理用户帐户新的 Azure 门户中的企业应用程序的资源调配

本文介绍如何使用[Azure 门户](https://portal.azure.com)管理资源调配和消除资源调配的支持它，尤其是来自[Active Directory 的 Azure 应用程序库](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)的"特色"类别添加了应用程序的自动用户帐户。 此新的 Azure 门户中的管理经验正在公共预览，和这篇文章描述的新功能，以及均设置在预览期间的一些临时限制。 [在预览中是什么？](active-directory-preview-explainer.md)

若要了解有关自动用户帐户设置和它是如何工作的详细信息，请参阅[自动化用户的资源调配和 Deprovisioning 到 Azure Active Directory 的 SaaS 应用程序](active-directory-saas-app-provisioning.md)。

##<a name="finding-your-apps-in-the-new-portal"></a>在新的门户中查找您的应用程序

9 月 2016 年所有的应用程序被配置成单一登录的目录，目录管理员使用[Azure Active Directory 应用库](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)内[Azure 的传统门户网站](https://manage.windowsazure.com)中，可现在查看和管理 Azure 的新门户。

新 Azure 门户，可以通过在左侧的导航区域的**更多服务**菜单来访问**企业应用程序**一节中可以找到这些应用程序。 企业应用程序是应用程序已部署并正在由您的组织内的用户使用。

![刀片式服务器的企业应用程序][0]

选择左侧的**所有应用程序**链接，将显示已配置，包括从库已添加的应用程序的所有应用程序的列表。 选择一个应用程序加载资源刀片式服务器为该应用程序，该应用程序，即可查看报告，并可管理的各种设置。

可以通过选择左侧的**资源调配**管理提供设置的用户帐户。

![应用程序资源刀片式服务器][1]


##<a name="provisioning-modes"></a>资源调配模式

**资源调配**刀片开头**模式**菜单，它显示为企业应用程序支持何种资源调配模式，并允许它们进行配置。 可用的选项包括︰

* **自动**-此选项显示如果 Azure 的广告支持自动基于 API 的提供和/或消除资源调配的用户帐户添加到此应用程序。 选择此模式显示引导管理员完成配置 Azure 广告连接到应用程序的用户管理 API、 创建帐户映射和工作流定义 Azure 广告之间应如何流动用户帐户数据的接口和应用程序，以及管理 Azure 资源调配服务的广告。

* **手动**-此选项显示是否 Azure 广告不支持自动提供给此应用程序的用户帐户。 此选项意味着必须使用一个外部进程，根据该应用程序 （其中包括 SAML 实时资源调配） 所提供的用户管理和资源调配功能来管理存储在应用程序中的用户帐户记录。


##<a name="configuring-automatic-user-account-provisioning"></a>配置自动用户帐户设置

选择**自动**选项将显示一个屏幕，分为四个部分︰

###<a name="admin-credentials"></a>管理凭据
这是其中个 Azure 广告连接到应用程序的用户管理 API 输入所需凭据。 所需的输入取决于应用程序。 若要了解有关凭据类型和特定应用程序的要求，请参阅[配置特定的应用程序的教程](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning)。

选择**测试连接**按钮允许您测试凭据通过 Azure 广告尝试连接到该应用程序的资源调配使用提供的凭据的应用程序。

###<a name="mappings"></a>映射
这是管理员可以查看和编辑之间 Azure AD 哪些用户属性流位置和目标应用程序，当用户帐户进行配置或更新。

还有 Azure AD 用户对象和每个 SaaS 应用程序的用户对象之间的映射的预配置的组。 一些应用程序来管理对象组或联系人的其他类型。 选择中的一个这些映射表显示映射编辑器的右侧，可以在其中查看和自定义它们。

![应用程序资源刀片式服务器][2]

在第一个预览的过程受支持的自定义项包括︰

* 启用和禁用映射为特定对象，如对 SaaS 应用程序的用户对象的 Azure AD 用户对象。

* 编辑的属性从 Azure AD 用户对象流到应用程序的用户对象。 有关属性映射的详细信息，请参阅[了解属性映射类型](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types)。

* 筛选资源调配 Azure 的广告应是 Azure 门户中的新功能的目标应用程序执行的操作。 而不是让 Azure 广告完全同步的对象，您可以限制所执行的操作。 例如，通过仅选择**更新**，Azure 广告只更新现有用户帐户的应用程序中，并不会创建新的。 通过仅选择**创建**，Azure 只创建新的用户帐户，但不会更新现有的。 此功能允许管理员创建的帐户创建的不同映射和更新工作流。 以后在预览期间计划创建多个映射每个应用程序的完整功能。

###<a name="settings"></a>设置
本部分允许管理员启动和停止 Azure 广告设置所选的应用程序，服务以及 （可选） 清除设置缓存并重新启动该服务。

如果第一次为应用程序启用资源调配，启用服务的**资源调配的状态**更改为**上**。 这导致 Azure 广告资源调配服务来执行初始同步，它会在**用户和组**部分中指定的用户的读取、 查询目标应用程序，并随后会执行在 Azure AD**映射**部分中定义的资源调配操作。 在此过程中，配置服务存储有关哪些用户帐户管理，缓存的数据，因此永远不会在指派的作用域的目标应用领域内的非管理帐户不受消除资源调配操作。 在初始同步后配置服务自动同步用户和组对象上十分钟的时间间隔。

**设置状态**更改为**关闭**只是暂停供应服务。 在这种状态，Azure 不创建、 更新或删除该应用程序中的任何用户或组对象。 将状态更改为 on，则会导致从它离开服务。

选择**清除当前状态，然后重新启动同步**复选框并保存停止供应服务，转储缓存的数据，关于哪些帐户 Azure 广告管理、 重新启动这些服务和执行初始同步。 此选项允许管理员重新启动设置的部署过程。

###<a name="synchronization-details"></a>同步详细信息
本节提供了有关配置服务，包括资源调配服务运行针对应用程序和管理数量的用户和组对象的第一个和最后一个时间的操作添加细节。

链接被提供到**资源调配活动报告**，其中提供了所有用户的日志和组创建、 更新和删除之间的 Azure AD 和目标应用程序和**资源调配错误报告**提供更详细的错误消息的用户和组对象无法读取，创建，更新，或删除。 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
