<properties
    pageTitle="使用方案和部署考虑事项 Azure 广告加入 |Microsoft Azure"
    description="解释如何管理员可以设置 Azure 广告加入为其最终用户 （员工、 学生、 其他用户）。 它还讨论了使用 Azure 广告加入不同的现实情况。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< 标签"主目录"ms.service= ms.workload="identity"ms.tgt_pltfrm="na"ms.devlang="na"ms.topic="article"ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>使用案例和 Azure AD 连接的部署注意事项

## <a name="usage-scenarios-for-azure-ad-join"></a>Azure AD 连接的使用情形
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>方案 1︰ 业务很大程度上在云中

Azure 活动目录联接 （Azure 广告加入） 可以让您受益如果当前操作并为您的业务在云环境中管理标识或很快迁移到云。 您可以使用在 Azure 的广告，以用于登录到 Windows 10 中创建帐户。 通过[第一个运行的经验 (FRX) 过程](active-directory-azureadjoin-user-frx.md)中，也可以通过加入 Azure 的广告，从[设置菜单](active-directory-azureadjoin-user-upgrade.md)，用户可以将他们的计算机加入到 Azure 的广告。  您的用户还可以享受单一登录 (SSO) 对资源的访问云 Office 365 等在他们的浏览器或 Office 应用程序中。

### <a name="scenario-2-educational-institutions"></a>方案 2︰ 教育机构

教育机构通常有两种用户类型︰ 教职员和学生。 参与试验的教职员被视为该组织的长期成员。 为其创建本地帐户是可取的。 但学生是 shorter-term 组织的成员，可以在 Azure AD 管理他们的帐户。 这意味着目录比例可被推送到云环境而不是存储在本地。 这还意味着，学生将能够利用其 Azure 的广告帐户登录到 Windows 和 Office 应用程序中可以访问 Office 365 提供资源。

### <a name="scenario-3-retail-businesses"></a>方案 3︰ 零售企业

零售企业都有季节性的工作人员和长期雇员。 您通常创建本地帐户并加入域的计算机用于长期的全职员工。 但季节性工人的组织，shorter-term 成员，最好能够管理其帐户的用户许可证可以更轻松地来回移动。 在 Office 365 许可证云中创建用户帐户时，这些用户获取登录到 Windows 和 Office 应用程序使用 Azure 的广告帐户，而在他们离开后维护更加灵活地针对其许可证的好处。

### <a name="scenario-4-additional-scenarios"></a>方案 4︰ 其他方案

前面所讨论的好处，让您受益让其设备加入 Azure 广告由于简化加入体验、 高效的设备管理、 自动移动设备管理登记和单一登录到 Azure AD 用户和内部部署的资源。  


## <a name="deployment-considerations-for-azure-ad-join"></a>Azure AD 连接的部署注意事项

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>使用户可以直接到 Azure AD 加入公司所拥有的设备


企业可以向合作伙伴公司和组织提供云专用帐户。 这些合作伙伴可以轻松地访问公司的应用程序和资源使用单一登录。 这种情况下是适用于访问资源主要在云中，例如 Office 365 或 SaaS 应用程序依赖于 Azure AD 身份验证的用户。

### <a name="prerequisites"></a>系统必备组件
**在企业级别 （管理员）**

*   使用 Azure Active Directory azure 订阅  

**在用户级别**

*   Windows 10 （专业版和企业版）

### <a name="administrator-tasks"></a>管理员任务
* [设置设备注册](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>用户任务
* [设置 Azure AD 安装过程中使用新的 Windows 10 设备](active-directory-azureadjoin-user-frx.md)
* [从设置菜单设置 Azure AD 的 Windows 10 设备](active-directory-azureadjoin-user-upgrade.md)
* [个人的 Windows 10 设备加入您的组织](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>在 Windows 10 您的组织中启用 BYOD
您可以设置您的用户和员工要使用他们的个人 Windows 设备 (BYOD) 访问公司的应用程序和资源。 您的用户可以添加个人 Windows 设备安全和兼容的方式访问资源及其 Azure 的广告帐户 （工作或学校）。

### <a name="prerequisites"></a>系统必备组件
**在企业级别 （管理员）**

*   Azure 广告订阅

**在用户级别**

*   Windows 10 （专业版和企业版）


### <a name="administrator-tasks"></a>管理员任务

* [设置设备注册](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>用户任务
* [个人的 Windows 10 设备加入您的组织](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [没有通过 Microsoft Passport 密码身份验证身份](active-directory-azureadjoin-passport.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
