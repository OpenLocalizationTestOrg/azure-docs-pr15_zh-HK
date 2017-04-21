<properties
   pageTitle="Azure 的活动目录的身份验证协议 |Microsoft Azure"
   description="支持通过 Azure 活动目录 (AD) 的身份验证协议的概述"
   documentationCenter="dev-center-name"
   authors="bryanla"
   services="active-directory"
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

# <a name="azure-active-directory-authentication-protocols"></a>Azure 的活动目录的身份验证协议

Azure 活动目录 (AD Azure) 支持几个最广泛使用的身份验证和授权协议。 本节中的主题描述了受支持的协议和 Azure 广告及其实现。 主题包括支持的声明类型，简介使用联合身份验证元数据的回顾，详细 OAuth 2.0。 SAML 2.0 协议参考文档和故障排除部分。

## <a name="authentication-protocols-articles-and-reference"></a>身份验证协议的文章，并引用

- [重要信息关于签名密钥变换在 Azure 广告](active-directory-signing-key-rollover.md)– 了解 Azure 广告的签名密钥变换节奏，也可以的更改自动更新密钥和如何更新最常用的应用程序方案的讨论。


- [支持令牌和声明类型](active-directory-token-and-claims.md)-Azure AD 问题了解在令牌中的声明。


- [联合元数据](https://msdn.microsoft.com/library/azure/dn195592.aspx)-了解如何查找和解释 Azure AD 生成元数据文档。


- [在 Azure 广告 OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx) -了解 OAuth 2.0 Azure AD 中的实现。


- [OpenID 连接 1.0](https://msdn.microsoft.com/library/azure/dn645541.aspx) -了解如何使用 OAuth 2.0，授权协议进行身份验证。


- [SAML 协议引用](https://msdn.microsoft.com/library/azure/dn195591.aspx)— 了解 Azure AD 的单一登录和单个 Sign-out SAML 配置文件。


- [WS 联合身份验证 1.2](https://msdn.microsoft.com/library/azure/dn903702.aspx) -Azure AD 中了解 WS 联合身份验证 1.2。


- [解决身份验证协议](https://msdn.microsoft.com/library/azure/dn195584.aspx)-了解如何避免问题和解释并解决错误时使用 Azure 的广告。



## <a name="see-also"></a>请参见

[Azure 的 Active Directory 开发人员指南 》](active-directory-developers-guide.md)

[为身份验证使用 Azure 的广告](../app-service-web/web-sites-authentication-authorization.md)

[Active Directory 代码示例](active-directory-code-samples.md)
