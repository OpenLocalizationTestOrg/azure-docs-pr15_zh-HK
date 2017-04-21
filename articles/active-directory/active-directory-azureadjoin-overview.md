<properties
    pageTitle="扩展到 Windows 10 设备通过 Azure 活动目录加入云功能 |Microsoft Azure"
    description="提供 Windows 10 设备可以利用 Azure 广告加入来获取在 Azure Active Directory 中注册的详细的概述。"
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
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>扩展到 Windows 10 设备通过 Azure 活动目录加入云功能

## <a name="what-is-azure-active-directory-join"></a>什么是 Azure 活动目录加入？
Azure 活动目录联接 （Azure 广告加入） 是 Azure 启用设备的集中的管理的 Active Directory 中注册的公司拥有设备的功能。 它使用户连接到企业云通过 Azure Active Directory 的员工和学生等。 这可简化的 Windows 部署和访问组织的应用程序和资源从任何 Windows 设备，公司拥有和个人拥有的 (BYOD)。

Azure AD 连接适用于企业，仅云第一个/云--通常小型和中等规模的公司不具有内部 Windows 服务器的 Active Directory 基础结构。 所说，Azure AD 联接可以，也会由大型组织不能做传统域联接 （移动设备，例如），或主要需要访问 Office 365 或其他 Azure AD SaaS 应用程序的用户的设备上使用。

虽然传统的域加入仍提供了最佳的内部部署经验能够加入域的设备上，Azure AD 连接是合适的设备，不能加入域。 Azure AD 连接也是适合于管理云中的用户。 它是通过使用移动设备管理功能，而不是使用传统的域管理工具如组策略和系统中心配置管理器 (SCCM)。

![公司的设备和个人使用内部部署 Active Directory 和 Azure 广告设备概述](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>为什么应该企业采用 Azure 广告加入？

* **很大程度上在云环境中的企业**︰ 如果您已经移动或模型将降低您的内部空间和希望获得更多云到移动，Azure 广告加入可以让您受益。 也许您已经创建 Azure 的广告帐户手动或者通过内部活动目录同步。 这两种方式，您有一个帐户在 Azure 的广告，并可用来登录到 Windows 10。 您的用户可以将计算机加入到 Azure 广告通过的全新体验 (OOBE) 或通过设置菜单。 加入后，用户将享受到单一登录 (SSO) 对资源的访问云 Office 365 等在他们的浏览器或 Office 应用程序中。

* **教育机构**︰ 我们经常听到有关的方案之一就是教育机构，有两种用户类型︰ 教职员和学生。 参与试验的教职员被视为长期组织的成员，因此需要为其创建本地帐户。 但学生 shorter-term 成员的组织，因此可以在 Azure 广告管理。 这意味着目录比例可被推送到云环境而不是存储在本地。 这还意味着学生可以其 Azure 的广告帐户登录到 Windows 和 Office 应用程序中或者在他们的浏览器获得对 Office 365 提供资源的访问。

* **零售企业**︰ 我们听说来自客户的另一个方案是其愿望季节性工作人员更方便地进行管理。  再次，长期、 全职雇员的帐户通常创建为内部部署在加入域的计算机上的帐户。 但季节性工人是 shorter-term 成员的组织，因此，最好是管理它们在用户许可证可以更轻松地来回移动。 在 Office 365 许可证云中创建这些用户帐户允许用户获取登录到使用 Azure 的广告帐户的 Windows 和 Office 应用程序的好处。 同时，在他们离开后维护更加灵活地针对其许可证。
* **其他企业**︰ 即使要维护内部活动目录中的用户，您可能仍然受益让用户能加入 Azure 的广告。 这是因为 Azure 广告为 Azure AD 提供简化加入体验、 高效的设备管理、 自动移动设备管理登记和单点登录功能和内部资源。  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Azure 广告加入提供哪些功能呢？
使用 Azure 加入广告，您可以获取以下︰

* **公司拥有设备的自我供应**︰ 使用 Windows 10，用户可配置一个全新、 塑封设备的现成经验，而无需 IT 部门参与。


* **现代的外形的支持**︰ Azure AD 联接工作没有传统域加入功能的设备上。  


* **对现有组织帐户的支持**︰ 用户不再需要创建和维护个人的 Microsoft 帐户与 Windows 8 一样公司颁发的设备上获取最佳的体验。 他们可以使用其现有的工作帐户在 Azure 的广告相反。 对于许多公司来说，这基本上意味着用户设置权限，并且可以通过访问 Office 365 提供给他们使用相同的凭据登录到 Windows。


* **自动移动设备管理注册**︰ 设备可以自动加入到 Azure AD 连接的移动设备管理。 此过程适用于 Microsoft Intune 及其合作伙伴的移动设备管理解决方案。 完成与 Intune 进行设备管理，是 IT 管理员可监视/管理加入域的 SCCM 管理控制台中的设备旁边的 Azure 加入广告的设备。


* **对公司资源的单一登录**︰ 用户享受单一登录从 Windows 桌面应用程序和资源在云中，例如 Office 365 和数以千计的业务应用程序都依赖于 Azure 广告通过[Azure AD 连接](active-directory-azureadjoin-deployment-aadjoindirect.md)的身份验证。 企业拥有的设备加入到 Azure 的广告还享受 SSO 对内部资源时设备时，可以在公司网络上，并且从任何地方通过[Azure AD 应用程序代理](https://msdn.microsoft.com/library/azure/Dn768219.aspx)公开这些资源。


* **操作系统状态漫游**︰ 辅助功能设置、 网站、 Wi-fi 密码和其他设置不同的企业所拥有的设备同步无需个人 Microsoft 帐户。


* **企业准备 Windows 应用商店**︰ Windows 应用商店支持应用程序获取和使用 Azure 的广告帐户授权。 组织可以批量许可的应用程序，并使其组织中的用户。

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>不同的设备如何使用 Azure 广告加入工作？

| 公司设备 （连接到内部域）                                                                                                                                                                                         | 公司设备 （加入到云）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 个人设备                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 用户可以登录到 Windows 以工作身份 （像现在）。                                                                                                                                                                        | 用户可以登录到 Windows 以工作身份管理的 Azure 的广告。 这是相关企业设备这三种情况︰ 1) 组织没有 Active Directory 部署 （例如，小型企业）。 2) 组织不会在 Active Directory 中创建所有用户帐户 （例如，为学生、 顾问、 或季节性工人不创建帐户在 Active Directory 中）。 3) 的组织具有企业设备不能加入到域 （内部部署），像手机或平板电脑运行 （例如，辅助设备进入工厂/零售基底） 一种移动的 SKU。 Azure AD 连接支持的企业设备管理和联合组织加入。 | 用户登录到 Windows 与他们个人的 Microsoft 帐户凭据 （无更改）。                                                |
| 用户具有访问漫游的设置和企业 Windows 应用商店。 这些服务使用工作帐户，并且不需要个人的 Microsoft 帐户。 这就要求企业能够将其内部活动目录连接到 Azure 的广告。                                        | 用户可以进行自助服务安装。 他们可以通过首次运行体验 (FRX) 通过其工作帐户除了让 IT 供应设备，尽管这两种方法可以得到支持。                                                                                                                                                                                                                                                                                                                                                                             | Active Directory 或 Azure 的广告中，用户可以方便地添加管理工作帐户。                                                      |
| 用户具有 SSO 能够从桌面应用程序、 站点和资源，其中包括内部资源和云应用程序使用 Azure 广告进行身份验证。                                                                                                            | 设备是企业目录 (Azure AD) 中自动注册和自动注册移动设备管理。 （azure 广告高级功能）。                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 跨应用程序和网站/资源与此工作帐户，用户具有 SSO 功能。                                              |
| 用户可以添加他们个人的 Microsoft 帐户，而不会影响企业数据访问他们的个人图片和文件。 （漫游的设置能够继续其工作帐户。）Microsoft 客户不再驱动器的漫游的设置和启用 SSO。  | 用户可以执行在 winlogon、 自助服务密码重置 (SSPR) 意味着他们可以重新设置忘记的密码。 （azure 广告高级功能）。                                                                                                                                                                                                                                                                                                                                                                                                                                   | 用户有权访问企业 Windows 应用商店，以便他们可以获取，并在其个人设备上使用业务线应用程序。 |                                                               |


## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [没有通过 Microsoft Passport 密码身份验证身份](active-directory-azureadjoin-passport.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
