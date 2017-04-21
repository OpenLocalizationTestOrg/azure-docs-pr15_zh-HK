<properties
    pageTitle="在工作场所中使用 Windows 10 设备 |Microsoft Azure"
    description="为用户提供的功能快照和 IT，对比不同的方法可以提供设备，并将其用于 Windows 10 企业。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>在工作场所中使用 Windows 10 设备

适用于︰ Windows 10 台 Pc

Windows 10 提供了三种模型的组织，使用户能够对工时资源访问安全和方便的方式。

- **Azure 的 Active Directory 加入**（azure AD 连接） 的工作人员访问 Office 365 主要在云环境中的资源。 Azure AD 连接是自助服务工作提供 Windows 10 中的新增功能的体验。
- **加入域**的组织，必须在内部部署的应用程序和资源方面的投资。 加入域提供改进的体验 Windows 10 时连接到 Azure 的广告中。
- **新的简化的 BYOD 体验**，用户想要向窗口和方便地访问资源个人设备上的添加工作帐户或学校。

下表提供了快照，用户和 IT 管理员来说，对比不同的方法可以提供和使用与 Windows 10 企业中设备的功能︰

|                                                                                                                                                                 | 加入域     | Azure AD 联接 | 个人设备 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows 设备登录工作或学校的科目。                                                                                                                      | 是的             | 是的           | 不              |
| 用户单点登录 (SSO) 到 Office 365 和 Azure AD 的应用程序。 SSO 是只需一次登录以访问组织资源的能力。 | 是的             | 是的           | 是的             |
| Kerberos NTLM/应用程序用户 SSO。                                                                                                                                  | 是的             | 有限       | 是的通过 VPN         |
| 强身份验证和方便登录工作或学校科目与 Microsoft Passport 和 Windows Hello。                                                                   | 是的             | 是的           | 是的             |
| 对企业 Windows 应用商店的工作或学校的帐户 （而不是 Microsoft 帐户） 的访问。                                                                                    | 是的             | 是的           | 是的             |
| 在工作或学校的帐户与设备之间的用户设置符合企业的漫游。                                                                                 | 是的             | 是的           | 是的             |
| 限制对组织与组织设备策略兼容的设备的应用程序访问能力。                                                      | 是的             | 是的           | 是的             |
| 用户自助服务供应的设备"随时随地工作"。                                                                                                | 不              | 是的           | 是的             |
| 管理设备的能力。                                                                                                                                       | 是的通过 SCCM GP / | 是的           | 是的             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>使用工作负责设备使用 Azure AD 连接和域加入 Windows 10

Windows 10 提供工作拥有设备访问工时资源的两种的方法︰

- Azure AD 联接
- 加入域

 既可以是有效的选项，具体取决于组织的需要和要求。 在某些情况下，组织可能受益启用部署这两种方法。

## <a name="when-to-use-azure-active-directory-join"></a>何时使用 Azure 活动目录联接

Azure AD 连接是资源调配 Windows 10 中的体验新的自助服务工作。  本文的目标访问 Office 365 主要在云环境中的工时资源的工作人员。 它是轻量的方法，配置计算机、 平板电脑，以及企业的电话。 在 Windows 平台使用一致的控件，通过移动设备管理受控设备。

**使用 Azure 广告加入下列原因之一**︰

- 您想要启用自助服务资源调配设备的工作人员在路上。
- 用户提供的工作拥有平板电脑和手机等的移动设备上。
- 您想要在 Azure 广告而不是在 Active Directory 中的一组用户管理如季节性工人、 承包商或学生。
- 您想要提供有限的内部部署基础结构联接功能在远程分支办公室的工作人员。
- 没有内部活动目录。

某些组织将使用 Azure 广告加入作为部署工作所拥有的设备的主要方式尤其是在他们将大多数或所有其资源迁移到云。 此外可根据用户或部门部署一种方法或其他与 Active Directory 和 Azure AD 的混合组织。

学区和大学，例如，可能管理在 Active Directory 中的员工和学生在 Azure 的广告。 一些公司可能希望在 Azure AD 管理远程办公室或销售部门。 可以混合组织内使用 Azure AD 连接和域联接方法。 Azure AD 连接可以是部署在工作环境中的设备加入域的极好补充。

**如果您的组织云，通过对企业资源的最一般访问是否可能享受益处**︰

- 您可以删除依赖内部身份基础结构。
- 您可以简化设备部署模型，通过允许自助服务配置获取从成像解决方案。
- 移动设备管理可用于跨不同平台来管理所有设备。

Azure AD 连接的详细信息，请参阅[Windows 10 设备通过 Azure 活动目录加入到扩展云功能](active-directory-azureadjoin-overview.md)。

## <a name="when-to-use-domain-join-or-keep-using-it"></a>何时使用域联接 （或使用它的保留）

过去 15 年中，对于许多组织已使用加入域连接工作设备。 它使用户能够登录到其设备与 Active Directory 工作或学校科目。 域加入还使 IT 部门能够集中和完全管理这些设备。 组织通常依赖于供应设备的图像处理方法，并经常使用系统中心配置管理器 (SCCM) 或组策略 (GP) 对其进行管理。

**企业应针对这些原因的任何使用域联接 （或使用它的保留）**︰

- 具有 Win32 应用程序部署到使用 NTLM/Kerberos 这些设备。
- 您需要 GP 或 SCCM/DCM 来管理设备。
- 您希望继续使用映像解决方案将设备配置为您的员工。

**域加入 Windows 10 中的还提供了连接到 Azure AD 时具有以下优点**︰

- 强大的设备绑定身份验证和方便登录工作或学校科目与 Microsoft Passport 和 Windows Hello。
- 对企业 Windows 应用商店设备使用工作或学校帐户 （不需要 Microsoft 帐户） 的访问。
- 在工作或学校帐户 （不需要 Microsoft 帐户） 使用的设备的用户设置符合企业的漫游。
- 限制对组织与组织设备策略兼容的设备的应用程序访问能力。

Azure AD 连接的详细信息，请参阅[Windows 10 设备通过 Azure 活动目录加入到扩展云功能](active-directory-azureadjoin-overview.md)。

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>使加入的个人所拥有的工作或学校的设备

为了支持 BYOD 企业中，Windows 10 允许用户将工作或学校的帐户添加到他们的电脑、 平板电脑或电话。 用户添加工作或学校的帐户后，该设备是注册 Azure 的广告，或者登记的单位已配置移动设备管理系统中。 目录将显示这些设备为已登记与加入 Azure 的广告。 IT administraors 可以应用不同的策略，根据此信息，如果需要比工作拥有的设备上的个人所拥有的设备上提供一个较浅的触摸。

用户可以添加某一工作或非常方便地学习到他们的个人设备的帐户。 它们可以执行此操作时访问工作应用程序第一次，或者它们可以手动进行通过设置菜单。 此帐户将组织资源提供 SSO。

Azure AD 连接的详细信息，请参阅[连接到 Windows 10 的 Azure AD 的加入域的设备体验](active-directory-azureadjoin-devices-group-policy.md)。

## <a name="enable-domain-join-or-azure-ad-join"></a>启用域连接或 Azure AD 连接

前面所述的所有方法 （域加入、 Azure AD 联接和外工作或学校帐户） 具有 Windows 10 用户体验中的入口点。 但是，所有需要的 IT 管理员来启用基础结构中的功能，体验工作之前。

## <a name="requirements-for-deploying-azure-ad-join"></a>用于部署 Azure 广告加入的要求

若要为任何一组的用户部署 Azure 广告加入您需要以下各项︰

- 订阅了 Azure 的广告。
- Azure 广告高级订阅，如移动设备管理自动注册，如果您需要更多的功能。
- 移动设备管理--例如，Microsoft Intune 订阅、 Office 365 的移动设备管理或任何与 Azure AD 集成合作伙伴的移动设备管理供应商。 （请参阅本文的详细信息的末尾[常见问题解答部分](#frequently-asked-questions)）。

如果设施是混合，强烈建议您部署 Azure AD 连接，可以将内部目录扩大到 Azure 的广告。

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>要求使用 Azure AD 域加入

域加入继续起作用，因为它总是有。 但是，要获得 Azure 的广告收益将需要以下︰

- 订阅了 Azure 的广告。
- Azure 广告连接到 Azure AD 扩展的内部目录部署。
- 可以加入域的设备，有条件权 Azure 的广告策略。
- 如果您想要能够限制某些设备的访问权限允许对"加入域的"设备访问权限策略。
- 系统中心配置管理器版本 1509 技术预览，以启用规则要求兼容的设备。 （请参阅 TechNet 文档和博客文章。）

有关 Windows 10 中加入域的详细信息，请参阅[连接到 Windows 10 的 Azure AD 的加入域的设备体验](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>使用 BYOD 并"添加工作或学校的帐户"的要求

若要启用"携带您自己的设备"(BYOD) 工作或学校的帐户，您需要以下︰

- 订阅了 Azure 的广告。
- Azure 广告高级订阅，如移动设备管理自动注册，如果您需要更多的功能。

## <a name="requirements-for-using-microsoft-passport"></a>使用 Microsoft Passport 的要求

若要启用 Microsoft Passport，您需要︰

- 公钥基础结构 (PKI) 使用 Microsoft Passport 的基于证书的身份验证支持。
- 使用 Microsoft Passport Azure AD 联接和工作或学校科目的基于证书的身份验证支持 Intune 订阅。
- 系统中心配置管理器的技术预览版本 1509年 （请参阅 TechNet 文档和博客帖子） Microsoft Passport 用于加入域的基于证书的身份验证支持。
- 在组织中启用 Microsoft Passport 的策略。

作为 PKI 使用的替代方法，您可以启用基于公钥的 Microsoft Passport 通过以下︰

- 部署 Windows 服务器 2016年"生产预览 1"Dc （域或林的功能级别不需要; 几种 DCs 冗余服务每个 Active Directory 站点就足够了）。
- 设置策略以在组织中启用 Microsoft Passport。

有关 Microsoft Passport 和 Windows Hello Windows 10 中的详细信息，请参阅 < link-to-MS-Passport-and-Windows-Hello-document >。

## <a name="frequently-asked-questions"></a>常见问题及的解答
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>与 Azure AD 集成的合作伙伴的移动设备管理产品？

以下供应商产品将与 Azure 的统一的注册和 Windows 10 中的条件访问 AD 集成︰

- 通过 VMware 的 AirWatch
- Citrix Xenmobile
- Lightspeed 移动管理器
- SOTI 内部移动设备管理
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>加入在 Windows 中 10 工作场所呢？
工作场所连接中使用了 Windows 8.1 启用 BYOD。 在 Windows 10 BYOD 启用了通过"添加某一工作或学校科目"此文档中前面所述。 对于不使用 Azure AD 集成他们的移动设备管理的组织，用户可以通过**设置**手动管理到注册设备 > **帐户** > **的工作访问**。


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>用户可以连接到其域帐户在 Windows 10 的 Microsoft 帐户？
不在 Windows 10。 Windows 8.1 中加入域的设备的用户无法连接"Microsoft 帐户 （例如，Hotmail、 生存、 Outlook、 Xbox 等） 到其域帐户以启用 SSO 像到现场服务，使用 Windows 应用商店，并在设备之间漫游用户设置的某些经验。 在 Windows 10 中"连接"功能的 Microsoft 帐户已停用。 作为额外的帐户以启用 SSO，为消费者服务，例如 Windows 应用商店，用户可以添加一个或多个 Microsoft 帐户。 这是在**设置** > **帐户** > **您的帐户**。

用户谁正在升级 Windows 8.1 加入域的设备，以及曾自己 Microsoft 帐户连接，将自动具有连接的 Microsoft 帐户添加到使用其他帐户的列表。


## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
