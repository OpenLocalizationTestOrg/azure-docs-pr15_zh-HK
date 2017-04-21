<properties
   pageTitle="故障排除: Active Directory 项目已丢失或不可用 |Microsoft Azure "
   description="当 Active Directory 菜单项不会出现在 Azure 管理门户做什么。"
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>故障排除: Active Directory 项目已丢失或不可用

许多使用 Azure Active Directory 功能和服务的说明开始使用"转到 Azure 的管理门户并单击**撤销"**。 但是如果 Active Directory 扩展或菜单项不会出现，或者如果它被标记为**不可用**怎么办呢？ 本主题旨在帮助。 它描述的**Active Directory**未出现或不可用，说明如何继续的条件。

## <a name="active-directory-is-missing"></a>活动目录已丢失

通常情况下， **Active Directory**项目出现在左边的导航菜单中。 Azure Active Directory 过程中的说明假定此项目是在您的视图。

![屏幕快照︰ Azure 中的活动目录](./media/active-directory-troubleshooting/typical-view.png)

下列任一情况为真时，Active Directory 项目将出现在左边的导航菜单。 否则，该项目也不会出现。

* 当前用户登录使用 Microsoft 帐户 （以前称为 Windows Live™ ID）。

    OR

* Azure 的租户有一个目录和当前帐户目录管理员。

    OR

* Azure 租户具有至少一个 Azure AD 的访问控制 (ACS) 命名空间。 有关详细信息，请参阅[访问控制 Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx)。

    OR

* Azure 的租户有至少一个 Azure 多因素身份验证提供程序。 有关详细信息，请参阅[管理 Azure 多因素身份验证提供程序](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)。

若要创建访问控制命名空间或多因素身份验证提供程序，请单击**+ 新建** > **应用程序服务** > **活动目录**。

若要获取对某个目录的管理权限，有管理员分配给您的帐户管理员角色。 有关详细信息，请参阅[分配管理员角色](active-directory-assign-admin-roles.md)。

## <a name="active-directory-is-not-available"></a>活动目录不可用

当您单击**+ 新建** > **应用程序服务**， **Active Directory**项目出现。 具体而言，在任何 Active Directory 功能，如目录、 访问控制、 或多因素身份验证提供程序，当前用户可用时，将出现 Active Directory 项目。

但是，在加载页时该项目将显示为灰色并被标记为**不可用**。 这是一种临时状态。 等待几秒钟，如果该项目可用。 如果长时间的延迟，经常刷新该 web 页能够解决问题。

![屏幕快照︰ 活动目录不可用](./media/active-directory-troubleshooting/not-available.png)
