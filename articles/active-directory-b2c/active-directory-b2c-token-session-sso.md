<properties
    pageTitle="Azure 的活动目录 B2C︰ 标记、 会话和单一登录配置 |Microsoft Azure"
    description="标记、 会话和 Azure 活动目录 B2C 中的单一登录配置"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure 的活动目录 B2C︰ 标记、 会话和单一登录配置

此功能为您提供[每个策略的基础](active-directory-b2c-reference-policies.md)，对这样的细粒度控制的︰
 
1. 发射的 Azure 活动目录 (AD Azure) B2C 的安全令牌的生存期。
2. 由 Azure AD B2C 的 web 应用程序会话的生存期。
3. 跨多个应用程序和策略在 B2C 租户的单一登录 (SSO) 程序行为。

您可以使用此功能在 B2C 租户中，如下所示︰

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 单击**签入策略**。 *注意︰ 您可以使用此功能在任何策略类型，不只是在**签入策略** *。
3. 单击它打开一个策略。 例如，单击**B2C_1_SiIn**。
4. 单击**编辑**顶部的刀片式服务器。
5. 单击**标记、 会话和单一登录配置**。
6. 进行所需的更改。 了解如何在后续部分中的可用属性。
7. 单击**确定**。
8. 顶部刀片式服务器，请单击**保存**。

![标记、 会话和单一登录配置的屏幕抓图](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>令牌生存期配置

Azure AD B2C 支持[OAuth 2.0 授权协议](active-directory-b2c-reference-protocols.md)允许对受保护资源的安全访问。 若要实现此支持，Azure AD B2C 发出各种[安全令牌](active-directory-b2c-reference-tokens.md)。 这些是可用于管理 Azure AD B2C 发出安全令牌的生存期的属性︰

- **访问和 ID 令牌生存期 （分钟）**︰ OAuth 2.0 载体令牌的生存期内用来获得对受保护资源的访问。 这次将 azure AD B2C 发出只有 ID 标记。 此值将应用于访问令牌，我们添加对它们的支持。
   - 默认值为 60 分钟。
   - 最小值 （含） = 5 分钟。
   - 最大值 （含） = 1440 分钟。
- **刷新令牌生存期 （天）**︰ 在其之前刷新令牌可用来获得一个新的访问或 ID 标记的最大时间段 (和 （可选） 新刷新令牌后，如果您的应用程序被授予`offline_access`范围内)。
   - 默认值 = 14 天。
   - 最小值 （含） = 1 天。
   - 最大值 （含） = 90 天。
- **刷新令牌滑动窗口生存期 （天）**︰ 此时间段已过期的用户之后强制重新进行身份验证，irrespective of 最新版本的有效期内刷新由应用程序获取令牌。 它只提供如果此开关设置为**Bounded**。 它必须是大于或等于**刷新令牌生存期 （天）**值。 如果此开关设置为**Unbounded**，不能提供一个特定的值。
   - 默认值 = 90 天。
   - 最小值 （含） = 1 天。
   - 最大值 （含） = 365 天。

以下是几个用例，则可以使用这些属性︰

- 只要他或她正在不断地对应用程序使用户能够保持下去，到移动应用程序签名。 你可以通过设置**刷新令牌的滑动窗口生存期 （天）**在您的签入策略切换到**Unbounded** 。
- 通过设置适当的访问令牌生存期中满足行业的安全性和法规遵从性要求。

## <a name="session-configuration"></a>会话配置

Azure AD B2C 安全登录到 web 应用程序启用支持[OpenID 连接的身份验证协议](active-directory-b2c-reference-oidc.md)。 这些是可用于管理 web 应用程序会话的属性︰

- **Web 应用程序会话寿命 （分钟）**︰ Azure AD B2C 会话 cookie 存储在用户的浏览器在身份验证成功时的寿命。
   - 默认值 = 1440 分钟。
   - 最小值 （含） = 15 分钟。
   - 最大值 （含） = 1440 分钟。
- **Web 应用程序会话超时**︰ 如果此开关设置为**绝对**，强制用户在通过**Web 应用程序会话寿命 （分钟）**经过指定的时间段后重新进行身份验证。 如果此开关设置为**滚动**（默认设置） 时，用户保持登录，只要用户在 web 应用程序中的活动不断。

以下是几个用例，则可以使用这些属性︰

- 符合行业的安全性和法规遵从性要求，通过设置适当的 web 应用程序会话的生存期。
- 强制重新进行身份验证与 web 应用程序的安全性较高部分的用户交互设置的时间段之后。 

## <a name="single-sign-on-sso-configuration"></a>单一登录 (SSO) 配置

如果您有多个应用程序和策略在 B2C 租户中，您可以通过使用**单一登录配置**属性管理用户交互。 您可以将属性设置为下列设置之一︰

- **租户**︰ 这是默认设置。 使用此设置在您 B2C 租户共享相同的用户会话中允许多个应用程序和策略。 例如，一旦用户签入应用程序，Contoso 购物，他或她可以也无缝登录时访问它的另一个，Contoso 药房。
- **应用程序**︰ 这使您可以维护用户会话的应用程序，独立于其他应用程序以独占方式。 例如，如果您希望用户能够登录到 Contoso 药房 （使用相同的凭据），即使他或她已登录到 Contoso 购物，另一个应用程序在相同 B2C 租户。 
- **策略**︰ 这使您可以维护策略，独立于使用它的应用程序以独占方式的用户会话。 例如，如果用户已登录并完成多因素身份验证 (MFA) 步，他或她可以被赋予访问更高级别的安全部件的多个应用程序，只要不过期会话与该策略相关联。
- **禁用**︰ 这会强制用户在每次执行该策略的遍历整个用户之旅。 例如，这将允许多个用户登录您的应用程序 （桌面共享方案），甚至在单个用户保持登录的整个期间。
