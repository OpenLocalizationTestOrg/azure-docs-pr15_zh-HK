<properties
    pageTitle="专门在 Azure Active Directory 组 |Microsoft Azure"
    description="在 Azure Active Directory 和如何创建它们的工作方式专用组的概览。"
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

# <a name="dedicated-groups-in-azure-active-directory"></a>在 Azure Active Directory 中的专用的组

在 Azure 活动目录 (AD Azure) 专用的组功能自动创建并填充预定义 Azure AD 组的成员资格。 不能添加或删除使用 Azure 的传统门户网站、 Windows PowerShell 的 cmdlet，专用组成员或以编程方式。

>[AZURE.NOTE] 专用的组需要赋予 Azure AD 特优许可证
>- 管理员负责管理组中的规则
>- 规则选择组的成员的所有用户

**若要启用专用的组**

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，，然后打开您的组织的目录。

2. 选择**组**选项卡，然后打开要编辑的组。

3. 选择**配置**选项卡，然后再将**启用专用组**设置为**是**。

一旦启用专用组开关设置为**是**，您可以进一步启用要通过设置自动创建所有用户专用的组的目录**启用"所有用户"组**切换为**是**。 然后还可以编辑此专用组的名称通过键入**显示名称为"所有用户"组**字段。

所有用户组可用于分配给您的目录中的所有用户相同的权限。 例如，可以将所有用户专用组的访问权限分配给此应用程序中您到 SaaS 应用程序的目录访问权限授予所有用户。

专用的所有用户组中的目录，包括来宾和外部用户包括了所有用户。 如果需要一组，排除外部用户，则可以通过与基于属性的动态规则如下所示创建组来实现此目的︰

                (user.userPrincipalName -notContains "#EXT#@")

排除所有来宾组，使用规则如下所示︰

                (user.userType -ne "Guest")

若要了解有关如何为动态组成员身份创建*高级*规则 （规则可以包含多个比较），请参阅[使用属性来创建高级的规则](active-directory-accessmanagement-groups-with-advanced-rules.md)。


这些文章在 Azure Active Directory 提供附加信息。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)
* [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
* [什么是 Azure 活动目录？](active-directory-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
