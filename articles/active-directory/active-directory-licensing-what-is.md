<properties
    pageTitle="什么 Microsoft Azure Active Directory 授权？ |Microsoft Azure"
    description="Microsoft Azure Active Directory 授权、 它的工作原理、 如何开始，以及说明最佳做法，包括 Office 365、 Microsoft Intune Azure 活动目录津贴和基本版本"
    services="active-directory"
      keywords="Azure 广告许可"
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

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>什么 Microsoft Azure Active Directory 授权？

##<a name="description"></a>说明
Azure 活动目录 (AD Azure) 作为一种服务 (IDaaS) 的解决方案和平台是 Microsoft 的标识。 Azure 的广告将提供大量的功能和技术--从 Azure 广告自由，可以使用 Office 365、 动态、 Microsoft Intune 和 Azure 等任何 Microsoft 服务的版本 （Azure 的广告不会产生任何消耗费用在此模式下），到 Azure 广告支付如企业移动套件 (EMS)，Azure 广告高级 Basic，以及 Azure 多因素身份验证 (MFA) 的版本。 像许多微软的在线服务，因为它们是在 Office 365、 Microsoft Intune 和 Azure 广告最 Azure 支付的广告版本通过每用户权利中传递。 在这些情况下，服务采购表示与一个或多个订阅，而每个订阅您的组织中包括预先购买许可证数目。 每个用户权利被通过许可证分配，用户和产品，使用户的服务组件和使用预付的许可证之一，之间创建链接。

[现在尝试 Azure AD 津贴。](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD 管理门户是 Azure 的传统门户网站的一部分。 虽然使用 Azure 广告不需要任何 Azure 的采购，访问该门户需要活动 Azure 订购或[Azure 的试用订阅](https://azure.microsoft.com/pricing/free-trial/)。

Azure AD 服务能力的全面概述，请参阅[什么是 Azure 的广告](active-directory-whatis.md)。
[了解更多关于 Azure AD 服务级别](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure 付费订阅是不同︰ 虽然也可以表示目录中，这些订阅启用 Azure 的资源创建并将它们映射到您的付款方式。 在这种情况下，没有与此订阅关联的许可证计数。 与此订阅，用户访问管理订阅资源，用户的关联被通过向其授予对 Azure 的资源映射到订阅操作的权限。


##<a name="how-does-azure-ad-licensing-work"></a>Azure 广告许可如何工作？

基于许可证 （基于权利） 的 Azure 广告通过激活订阅您 Azure 的广告目录/服务租户的服务工作。 激活订阅后可以由目录/服务管理员管理服务能力，并已授权的用户使用。

当您购买或激活企业移动套件、 Azure AD 津贴，或 Azure AD 基本时，使用订阅，包括它的有效期和预付的许可证更新您的目录。 您的订阅信息，包括状态、 下一个生命周期事件，并已分配或可用的许可证数是可通过特定的目录的许可证选项卡下的 Azure 经典门户。 也是到最佳位置管理分配给您的许可证。

每个订阅包含一个或多个服务计划，每个映射的服务类型; 包括功能级别例如，Azure 广告，Azure MFA、 Microsoft Intune、 Exchange 联机或 SharePoint Online。 Azure 广告许可证管理不需要服务计划级别管理。 这是不同于 Office 365，它依赖于此高级的配置模式，以管理对包括服务的访问。 在服务配置中，启用的功能和管理单个权限依赖于 azure 的广告。

一般情况下，通过经典 Azure 的门户，为特定的目录的许可证选项卡下管理 Azure 广告订阅信息。 Azure 广告订阅，Azure AD 津贴，但不会显示在办公室门户中。

> [AZURE.IMPORTANT] Azure AD 津贴和基本的企业移动套件订阅以及局限于其提供的目录/租户。 不能拆分目录之间或使用以允许在其他目录中的用户订阅。 目录之间移动订阅是可能的但需要提交支持票或取消和重新购买在直接购买的情况下。

> 当购买 Azure 广告或通过批量许可订阅激活企业移动套件将协议时包括其他 Microsoft 联机服务，例如 Office 365 自动发生。

支付 Azure 广告功能跨越广泛的目录。 示例包括︰
- 基于组的分配给应用程序，这下您所管理的特定应用程序启用。
- 高级和自助服务组管理功能下的目录配置或特定组内。
- 高级安全报告中，都在报告选项卡
- 在 Azure 门户中标识下显示云应用程序发现。

###<a name="assigning-licenses"></a>指定的许可证
虽然获得订阅是所有您需要配置付费的功能，使用 Azure 广告支付功能要求分发许可证给正确的人员。 一般情况下，任何人应该有权访问或通过 Azure 广告管理的人员支付功能的用户，必须分配许可证。 许可证分配的用户和采购的服务，例如 Azure AD 津贴、 基本和企业移动套件之间的映射。

管理目录中的哪些用户具有许可证是简单的。 通过分配给一组创建分配规则通过 Azure AD 管理门户或通过直接向通过门户网站、 PowerShell 或 Api 的合适人员分配许可证可以完成它。 将许可证分配给一个组，组的所有成员将被都分配一个许可证。 如果添加或从组中删除用户时它们将被分配或删除适当的许可证。 组分配可利用任何可用的组管理和与应用程序的基于组的分配是一致的。 使用此方法，可以设置规则，以便在您的目录中的所有用户将自动都分配，确保每个人都具有相应的职务有许可证或甚至委托给组织中的其他经理的决定。

基于组的许可分配缺少使用位置的任何用户将在分配过程中继承的目录位置。 可以在任何时间由管理员更改此位置。 其中自动的分配失败错误的情况下下该类型许可证, 的用户信息将反映出这种状态。

##<a name="getting-started-with-azure-ad-licensing"></a>开始使用 Azure 广告许可

开始使用 Azure 的广告很容易;您始终可以创建您的目录作为注册试用免费 Azure 的一部分。 [了解更多关于作为一个组织注册](sign-up-organization.md)。 以下说明可帮助您确保您的目录最佳对齐可能会占用或规划占用，其他 Microsoft 服务和获得服务的目标。

下面是几种最佳实践︰
- 如果您已经在使用的任何 Microsoft 的组织服务，您已经有一个 Azure 的广告目录。 在这种情况下，应继续使用相同的目录的其他服务，以使核心标识管理，包括资源调配和混合 SSO，可以利用各种服务。 您的用户将获得单一的登录体验和跨服务将受益于更丰富的功能。 因此，如果您决定购买 Azure 广告支付员工的服务，我们建议使用相同的目录要这样做。
- 如果您计划使用一组不同的用户 （合作伙伴、 客户等），或如果您想要评估 Azure 广告服务并且想要为此在隔离的生产服务，或者如果您希望设置一个沙箱环境服务，我们建议您首先创建一个新目录通过 Azure Azure 传统门户 Azure 的广告。 [了解更多有关创建新 Azure 的广告目录在 Azure 的传统门户网站](active-directory-licensing-directory-independence.md)。 为外部用户使用全局管理员权限，将与您的帐户创建新目录。 当您登录到此帐户的 Azure 传统门户时，您将能够查看该目录并访问所有目录管理任务。 我们建议您创建一个本地帐户具有适当的权限来管理其他 Microsoft 服务 （那些通过 Azure 的传统门户网站不能访问）。 [了解更多有关创建用户帐户在 Azure 的广告](active-directory-create-users.md)。

> [AZURE.NOTE] Azure 的广告支持"外部用户，"哪些用户帐户位于 Azure 广告，使用 Microsoft 帐户 (MSA) 或从其他目录 Azure 广告标识所创建的实例。 虽然我们很忙来扩展到所有微软组织服务，此功能现在这些帐户不支持某些服务的经验;例如，Office 365 管理门户当前不支持这些用户。 结果是，与 Microsoft 客户的外部用户将不能时将忽略来自其他 Azure 的广告目录的外部用户，访问 Office 365 管理门户。 后一种情况下，只有用户的本地帐户，Azure 广告或 Office 365 目录在最初创建用户，将可通过这些体验。

如所示，Azure AD 具有不同的付费的版本。 这些版本具有一些细微的差别，在其采购可用性︰


| 产品   | EA/VL     | 打开  |   CSP |   MPN 使用权利  |   直接采购 | 试用版 |
|---|---|---|---|---|---|---|
| 企业移动套件 |   X | X | X | X |  |      X |
| Azure AD 津贴  | X | X | X |   | X | X |
| Azure AD 基本    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>选择一个或多个许可证试验
 在所有情况下，可以通过选择所需目录中的许可证选项卡的特定试用版激活 Azure AD 津贴或企业移动套件的试用订阅。 任何一个试用版包含 100 份许可证 30 天预订。

![Azure Active Directory 试用许可证计划](./media/active-directory-licensing-what-is/trial_plans.png)

![企业移动套件试用许可证计划](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![活动的试用许可证计划](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>指定的许可证
激活订阅后，应给自己分派许可证并刷新浏览器以确保您看到的所有功能。 下一步是将许可证分配给需要访问或被包含在用户支付 Azure 的广告功能。 如我们上面提到"分配许可证"，这样做的最佳办法是确定表示所需的访问群体的组并将其分配给该许可证;以这种方式，将分配给或从许可证中删除用户添加或通过生命周期从组中删除。

要为组或单个用户许可证，请选择您想要分配，然后单击**分配**的命令栏上的许可证计划。

![活动的试用许可证计划](./media/active-directory-licensing-what-is/assign_licenses.png)

一次在所选计划的分配对话框中，可以选择用户并将它们添加到右侧的**分配**列。 您可以翻阅的用户列表或搜索使用查找特定人员玻璃顶部右侧的用户网格。 要分配组，请从**显示**菜单中选择"组"，然后单击刷新显示的分配的权限的复选按钮。

![将许可证分配给组](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

现在，您可以搜索或浏览组并将它们添加到**指定**列中相同的方式。 您可以使用这些来分配单个操作中用户和组的组合。 若要完成分配过程，请单击检查按钮在页面的右下角。

![许可证分配进度消息](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

在一组分配时，其成员将在 30 分钟内，但通常在 1-2 分钟之内继承许可证。

分配错误 Azure 广告许可证分配，期间可能会发生，但它们相对较少。 潜在的分配错误仅限于︰
- 工作分配冲突-当用户先前已指派当前许可证与不兼容的许可证。 在这种情况下，分配新的许可证将要求删除上一个。
- 超出可用的许可证的分配组中的用户数超过可用的许可证时用户的工作分配状态将反映由于缺少许可证分配失败。

###<a name="view-assigned-licenses"></a>查看分配许可证

已分配的许可，包括订阅可用、 已分配，而且下一步生命周期事件的摘要视图将显示在**许可证**选项卡。

![查看已分配的许可数量](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

当导航到许可证计划，指派的用户和组的详细的列表，包括分配的状态和路径 （直接的或从一个或多个组继承的） 才可用。

![许可证计划分配的许可详细信息显示](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

删除许可证就将他们分配一样简单。 如果直接分派给用户或组分配，您可以通过选择许可证类型、 选择**删除**，将用户或组添加到删除列表中，并确认该操作删除许可证。 或者，可以打开许可证类型、 选择特定用户或组，并点击**删除**命令栏上。 若要结束的许可证从一组用户的继承，只是从组中删除用户。

###<a name="extending-trials"></a>扩展试验

可以使用客户的试用版扩展为自助服务通过 Office 365 的门户。 客户管理可以导航到[办公室门户](https://portal.office.com/#Billing)（访问权限取决于 Office 门户的权限） 并选择 Azure AD 最优试验。 单击**扩展试用**链接并按照说明进行操作。 您将需要输入信用卡号，但它不会付费。

![扩展在办公室入口许可证试用版](./media/active-directory-licensing-what-is/extend_license_trial.png)

客户还可以通过提交支持请求请求一个试用版的扩展。 客户管理可以导航到 Office 365 门户[支持页面](http://aka.ms/extendAADtrial)（访问权限取决于 Office 支持页面的权限）。 在此页上选择"订阅和试验"功能下，并在故障现象下的"试用问题"。 最后，在环境中输入的信息

![扩展使用支持请求许可试用版](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>下一步行动

现在您可能准备好配置和使用某些 Azure 广告高级功能。

- [自助服务密码重置](active-directory-manage-passwords.md)
- [自助服务组管理](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
- [对应用程序的组分配](active-directory-manage-groups.md)
- [Azure 的多因素身份验证](../multi-factor-authentication/multi-factor-authentication.md)
- [直接购买的 Azure AD 特优许可证](http://aka.ms/buyaadp)
