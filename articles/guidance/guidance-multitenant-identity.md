<properties
   pageTitle="对于多租户应用程序的标识管理 |Microsoft Azure"
   description="在多租户应用程序的身份验证、 授权和身份管理的最佳做法。"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>在 Microsoft Azure 中的多租户应用程序的标识管理

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

这一系列的多租赁，介绍最佳做法时使用的身份验证和身份管理 Azure 的广告。

当您正在构建一个多租户应用程序时，第一个挑战之一管理用户的身份，因为每个用户属于一个租户的现在。 例如︰

- 用户使用其组织的凭据登录。
- 用户应该能够访问其组织的数据，而不是属于其他租户的数据。
- 组织可以注册该应用程序，然后将应用程序角色分配给其成员。

Azure 活动目录 (AD Azure) 已支持所有这些方案中的一些重要功能。

伴随这一系列文章，我们还创建了一个完整、[端到端实现][tailspin]的多租户应用程序。 文章反映了我们在构建应用程序的过程中所学习的了。 要开始使用该应用程序，请参见[运行调查应用程序](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md)。

这是本系列文章的列表︰

- [对于多租户应用程序的标识管理简介](guidance-multitenant-identity-intro.md)
- [关于 Tailspin 调查应用程序](guidance-multitenant-identity-tailspin.md)
- [使用 Azure AD 身份验证和 OpenID 连接](guidance-multitenant-identity-authenticate.md)
- [使用基于声明的身份](guidance-multitenant-identity-claims.md)
- [注册和租户板载](guidance-multitenant-identity-signup.md)
- [应用程序角色](guidance-multitenant-identity-app-roles.md)
- [基于角色和基于资源的授权](guidance-multitenant-identity-authorize.md)
- [保护后端 web API](guidance-multitenant-identity-web-api.md)
- [OAuth2 的访问令牌缓存](guidance-multitenant-identity-token-cache.md)
- [与客户的 AD FS Azure 中的多租户应用程序的联盟](guidance-multitenant-identity-adfs.md)
- [使用客户端声明从 Azure AD 获取访问令牌](guidance-multitenant-identity-client-assertion.md)
- [使用密钥存储库来保护应用程序的机密](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
