<properties
   pageTitle="添加和管理多个 Azure Active Directory 目录 |Microsoft Azure"
   description="指导和最佳的添加和管理 Azure Active Directory 目录，说明作为完全独立的资源目录"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>添加和管理多个 Azure Active Directory 目录

在 Azure 活动目录 (AD Azure)，每个目录是一个完全独立的资源︰ 对等、 全功能型和逻辑上独立于您管理的其他目录。 目录之间没有任何父-子关系。 此目录之间的独立性包括资源独立性，管理的独立性和同步独立性。

##<a name="resource-independence"></a>资源的独立性

如果您创建或删除一个目录中的资源，它具有外部用户，如下所述的部分异常的另一个目录中的任何资源上没有任何影响。 如果在一个目录使用一个自定义的域 'contoso.com'，它不能用于任何其他目录。

##<a name="administrative-independence"></a>管理的独立性

如果非管理用户的目录 Contoso 然后创建一个测试目录 Test:
- 默认情况下，创建一个目录的用户添加为该新目录的外部用户并分配该目录中的全局管理员角色。
- 目录 Contoso 的管理员有没有直接的管理权限目录 Test，除非 Test 的管理员专门授予他们这些权限。 Contoso 的管理员可以控制访问目录 Test，如果它们控制的用户帐户创建 Test。
- 如果您更改 （添加或删除） 一个目录中的用户管理员角色，更改不会影响任何管理员角色的用户可能有另一个目录中。

##<a name="synchronization-independence"></a>同步的独立性

您可以配置每个 Azure 的广告目录分别以获取数据的单个实例同步︰
  - 目录同步目录 （同步） 的工具，使用单个 AD 林同步数据。
  - Azure Active Directory 连接器为前沿标识管理器，可以将数据与一个或多个内部林和/或非 Azure AD 数据源同步。

##<a name="add-an-azure-ad-directory"></a>将 Azure 的广告目录添加

在 Azure 的传统门户网站中添加 Azure 的广告目录，请选择左侧的 Azure Active Directory 扩展并点击**添加**。

> [AZURE.NOTE]   与其他 Azure 的资源，您的目录不是 Azure 订阅的子资源。 如果取消或允许 Azure 订阅过期，您仍然可以访问目录数据如 Office 365 管理中心使用 Azure PowerShell，Azure 图形 API 或其他接口。 您可以将另一个订阅与目录相关联。

Azure 广告许可方面的问题和最佳做法的全面概述，请参阅[什么 Azure Active Directory 授权？](active-directory-licensing-what-is.md)。
