<properties
   pageTitle="如何配置安全警报 |Microsoft Azure"
   description="了解如何配置 Azure 特权身份管理扩展的安全警报。"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>如何在 Azure AD 特权身份管理中配置安全警报

## <a name="security-alerts"></a>安全警报
Azure 特权身份管理 (PIM) 环境中可疑的或不安全的活动时，将生成警报。 当触发警报时，它显示 PIM 操控板上的。 选择要查看一个报表，列出的用户或角色触发警报的警报。

![PIM 仪表板安全警报-屏幕抓图][1]



| 提醒 | 触发器 | 建议 |
| ----- | ------- | -------------- |
| **角色分配在 PIM 之外** | 管理员已永久地分配给角色，PIM 界面以外。 | 查看新的角色分配。 因为其他的服务只可分配永久的管理员，对符合条件的分配根据需要更改它。 |
| **过于频繁地激活角色** | 在设置中所允许的时间内没有太多的重新激活的角色相同的角色。 | 用户看到为什么他们有激活作用次数太多，请与联系。 也许，时间限制是太短，他们能够完成其任务，或者他们也许正在使用脚本来自动激活角色。 |
| **角色不需要激活的多因素身份验证** | 有没有 MFA 设置中启用角色。 | 我们需要为最高权限的角色，MFA，但强烈建议您为所有角色激活启用 MFA。 |
| **管理员不使用其特权的角色** | 有资格尚未最近激活他们的角色的管理员。 | 启动访问权限检查以确定不再需要访问的用户。 |
| **有太多的全局管理员** | 有更多全局管理员不是建议。 | 如果您有大量的全局管理员，很可能用户要获取权限超过他们所需的权限。 将用户移到低限制的角色，或者使它们中的一些适合的角色，而不是永久地分配。 |

## <a name="configure-security-alert-settings"></a>配置安全警报设置

您可以自定义一些 PIM 来处理您的环境和安全目标中的安全警报。 请按照以下步骤达到设置刀片式服务器︰

1. 登录到[Azure 门户](https://portal.azure.com/)并从仪表板选择**Azure AD 特权身份管理**图块。
2. 选择**管理特权的角色** > **设置** > **通知设置**。

    ![导航到安全警报设置][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"角色都过于频繁地激活"警报

如果用户指定的时间内多次激活相同特权的角色，将触发此警报。 您可以配置的时间段和激活次数。

- **激活续订时间范围**︰ 指定以天、 小时、 分钟和第二个要用于跟踪可疑的产品更新的时间段。

- **激活产品更新的数量**︰ 指定的激活，从 2 到 100，您认为值得的警报，您选择的时间范围。 您可以更改此设置通过移动滑块，或在文本框中键入一个数字。


### <a name="there-are-too-many-global-administrators-alert"></a>"有太多的全局管理员"警报

如果两个其他条件都满足，并且您可以配置这两个将 PIM 触发此警报。 首先，您需要达到某一阈值的全局管理员。 第二，总的角色分配给您的一定百分比必须是全局管理员。 如果仅满足这些量化指标之一，不显示警报。  

- **全局管理员的最小数目**︰ 指定全局管理员，从 2 到 100，您考虑的不安全的金额数目。

- **全局管理员的百分比**︰ 指定管理员全局管理员的百分比，从 0%到 100%，这就是不安全的环境。

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"管理员并不使用其特权的角色"警报

如果无需激活角色，用户就可以一定的时间，将触发此警报。

- **天数**︰ 指定的天数，从 0 到 100，用户可以转而不激活一个角色。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
