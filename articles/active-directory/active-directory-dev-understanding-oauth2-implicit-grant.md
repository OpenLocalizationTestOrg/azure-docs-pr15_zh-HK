<properties
   pageTitle="了解 OAuth2 隐式授予在 Azure Active Directory 中的流动 |Microsoft Azure"
   description="了解更多有关 Azure Active Directory 实现的隐式 OAuth2 授予流，以及是否是最适合您的应用程序。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>了解 OAuth2 隐式授予流在 Azure 活动目录 (AD)

OAuth2 隐式授予是臭名昭著的所用时间最长的 OAuth2 规范中的安全问题列表授予。 然而，这也由 ADAL JS 和建议编写 SPA 应用程序时的一个实现的方法。 提供什么？ 这是所有问题的折衷方案︰ 和隐式授权证明，都是您可以通过使用在浏览器中通过 JavaScript Web API 的应用程序的最佳方法。

## <a name="what-is-the-oauth2-implicit-grant"></a>什么是 OAuth2 隐式授予？

Quintessential [OAuth2 授权代码授予](https://tools.ietf.org/html/rfc6749#section-1.3.1)是授权授予它使用两个不同的终结点。 授权端点用于用户交互阶段，这将导致授权码。 该令牌的终结点然后由客户端用来交换一个访问令牌，并经常刷新标记的代码。 Web 应用程序所需提供的令牌的端点，自己应用程序凭据，以便授权服务器可以对客户端进行身份验证。

[OAuth2 隐式授予](https://tools.ietf.org/html/rfc6749#section-1.3.2)直接从授权的终结点，不联系的令牌的终结点，也不进行身份验证的客户端应用程序的情况下是允许客户端获取一个访问令牌 （和 id_token，对于[OpenId 连接](http://openid.net/specs/openid-connect-core-1_0.html)） 的变体。 此款式特别设计的 JavaScript 基于 Web 浏览器中运行的应用程序︰ 在原始的 OAuth2 规范中，令牌返回 URI 片段中。 这样的标记位供 JavaScript 代码在客户端，但它保证它们不会被包括在向服务器的重定向。 通过浏览器重定向直接从授权终结点返回标记。 它还具有消除交叉原点电话，如果联系令牌的终结点所需的 JavaScript 应用程序是必需的任何要求的优点。

OAuth2 隐式授权的一个重要特性是如流动永远不会返回刷新到客户端的令牌。 如我们将在下一节看到的这不是真的有必要，事实上是一个安全问题。

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 隐式授权的合适方案

根据 OAuth2 规范本身声明，已设计隐式授予启用用户代理应用程序 – 也就是说，在浏览器中执行的 JavaScript 应用程序。 此类应用程序的定义特征是 JavaScript 代码用于访问服务器的资源 (通常是 Web API) 和相应地更新应用程序的用户体验。 如 Gmail 或 Outlook Web Access 应用程序的思考︰ 时从收件箱中选择一条消息，消息的可视化效果面板更改以显示新的选定区域，而页面的其余部分仍未被修改。 它与传统重定向的基于 Web 的应用程序，其中每个用户交互导致整页回发和新的服务器响应完整页面呈现。

应用程序采用极端情况下其基于的 JavaScript 的做法被称为单页面应用程序或 SPAs︰ 这一想法是，这些应用程序只提供初始 HTML 网页和相关联的 JavaScript，通过 JavaScript Web API 调用的执行与后续的所有交互操作。 但是，混合方式，其中应用程序大多为导向回发，但偶尔 JS 调用执行，并不鲜见 — 隐式流使用情况有关的讨论也是相关的。

基于重定向的应用程序通常安全通过 cookie，但是，其做法不能也用于 JavaScript 应用程序的请求。 针对生成它们时所为，虽然 JavaScript 调用可能定向向其他域的域只能使用 cookie。 事实上，它经常会出现这种情况︰ 应用程序调用 Microsoft Graph API，Office API，Azure API--从应用程序提供服务的位置所在的域之外的所有驻留的思考。 JavaScript 应用程序日益增长的趋势是有没有后端，信赖第三方 Web Api 来实现业务功能上的 100%。

目前，保护 Web API 调用的首选的方法是使用 OAuth2 载体的标记方法，每次调用都伴 OAuth2 访问令牌。 Web API 将检查传入的访问令牌，并且如果它在它找到必要的范围，它授予访问所请求的操作。 隐式流提供了便捷的 JavaScript 应用程序获取 Web API 访问令牌提供相对于 cookie 的许多优点︰

- 令牌可以可靠地获得无需交叉原始调用 – 强制注册的重定向 URI 与其标记是返回保证标记将不替换
- JavaScript 应用程序可以获得尽可能多的访问令牌所需的任意多个 Web Api 目标 – 与在域上没有限制
- HTML5 功能如会话或本地存储 cookie 管理是对应用程序透明而授予全权处理令牌缓存和生存期管理
- 访问令牌不容易受到跨站点请求伪造 (: CSRF) 攻击

隐式授予流不会发出刷新令牌，主要是出于安全考虑。 刷新令牌的范围尽可能窄不访问令牌，授予因此 inflicting 远远更大的损害，它泄漏的情况下更多电源。 中的隐式流动，在 URL 中传递的标记，因此拦截的危险高于授权代码授予。

但是，请注意，JavaScript 应用程序另一个机制全权处理续订不反复提示用户提供凭据的访问令牌。 应用程序可以使用隐藏的 iframe 执行新令牌请求对 Azure AD 的授权终结点︰ 只要浏览器仍然具有活动会话 (阅读︰ 具有会话 cookie) 对 Azure AD 域的身份验证请求成功进行而无需用户交互。 

此模型允许 JavaScript 应用程序独立更新的访问令牌并甚至获得新的新的 api （前提是用户以前他们许可。 这样就避免了获取、 维护和保护高价值项目如刷新令牌添加的负担。 项目的使缄默续订 Azure 广告的会话 cookie，管理在应用程序之外。 这种方法的另一个优点是从 Azure 广告，使用任何登录到 Azure 的广告，在任何浏览器选项卡中运行的应用程序用户可以注销。 这会导致删除的 Azure 广告的会话 cookie，然后 JavaScript 应用程序将会自动丢失更新标记出签名的用户的能力。

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>适于隐式授予我的应用程序？

隐式授予会带来更多风险比其他授权。 需要特别注意的领域充分 (例如看到[滥用的访问令牌中隐式流动模拟资源所有者为][ OAuth2-Spec-Implicit-Misuse] [OAuth 2.0 威胁模型和安全注意事项]和[OAuth2-Threat-Model-And-Security-Implications])。 但是，更高的风险预测是很大程度上由于这样一个事实︰ 它旨在使执行活动的代码，由远程资源到浏览器的应用程序。 如果您计划在 SPA 的体系结构，有没有后端组件或打算调用 Web API 通过 JavaScript，隐式流用于标记的收购建议。

如果您的应用程序的本机客户端，则隐式流不是非常适合。 Azure AD 会话 cookie 的本机客户端上下文中缺少 deprives 应用程序维护一个长寿命的会话的方法。 在获取新资源的访问令牌时，这意味着您的应用程序将反复提示用户。

如果正在开发一个 Web 应用程序，其中包括后端，并使用它的后端代码从一个 API，隐式流，也不适合。 其他授权为您提供了更多的电源。 例如，OAuth2 客户端凭据授予提供获取令牌反映分配给应用程序本身，而不是用户的委派权限的功能。 这意味着客户端都能够以编程方式访问资源甚至当用户不积极从事会话，依次类推。 不仅仅是这些，但这种授权提供更高的安全保证。 例如，访问令牌不乘车通过用户浏览器，它们不在浏览器历史记录，保存和风险等。 客户端应用程序也可以执行强身份验证时请求令牌。

## <a name="next-steps"></a>下一步行动

- 有关开发人员资源的完整列表，包括参考信息的协议和 OAuth2 授权授予流支持的 Azure 的广告，请参阅[Azure 广告开发人员指南][AAD-Developers-Guide]
- [如何集成了 Azure AD 的应用程序]，请参阅 [ACOM-How-To-Integrate]的应用程序集成过程上获得更多好处。

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

