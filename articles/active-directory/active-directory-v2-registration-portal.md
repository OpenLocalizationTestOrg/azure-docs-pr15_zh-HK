<properties
    pageTitle="应用程序注册门户帮助主题 |Microsoft Azure"
    description="在 Microsoft 应用程序注册门户中各种功能的说明。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>注册应用程序的引用
本文档提供上下文和 Microsoft 应用程序注册门户[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)中找到的各种功能的说明。

## <a name="my-applications"></a>我的应用程序
此列表包含所有应用程序注册为 Azure AD v2.0 终结点与使用。  这些应用程序必须能够使用来自 Microsoft 帐户和工作/学校 Azure Active Directory 帐户两个人帐户的用户提供登录。  若要了解有关 AD Azure v2.0 终结点的详细信息，请参阅我们的[2.0 版概述](active-directory-appmodel-v2-overview.md)。  此外可以使用这些应用程序可以与 Microsoft 帐户身份验证终结点，将集成`https://login.live.com`。

## <a name="live-sdk-applications"></a>实时的 SDK 应用程序
此列表包含所有应用程序注册为与 Microsoft 客户单独使用。  未启用使用 Azure Active Directory 无论如何。  这是将在其中查找以前具有已注册的 MSA 开发人员门户的任何应用程序`https://account.live.com/developers/applications`。  在先前进行的所有函数`https://account.live.com/developers/applications`现在在新的门户中，可以执行`https://apps.dev.microsoft.com`。  如果您有任何进一步的问题关于 Microsoft 帐户应用程序，请与我们联络。

## <a name="application-secrets"></a>应用程序密码
应用程序的机密是允许应用程序执行 Azure 广告与可靠的[客户端身份验证](http://tools.ietf.org/html/rfc6749#section-2.3)的凭据。  在 OAuth & OpenID 连接，应用机密通常称为`client_secret`。  在 2.0 版协议，接收安全令牌在 web 可寻址位置的任何应用程序 (使用`https`方案) 必须使用应用程序密码到 Azure AD 时对安全标记的兑换标识自身。  此外，任何本机客户端接收标记设置在设备将禁止使用应用程序密码来执行客户端身份验证，以防止机密信息在不安全环境中的存储。

每个应用程序可以在时间中包含任意给定时刻的两个有效的应用程序机密。  通过维护两个秘密，必须跨应用程序的整个环境中执行定期的密钥翻转 ablilty。  已有迁移整个应用程序与新的机密，一旦可以删除旧密钥，提供一个新。

到目前为止，只有两种类型的应用程序的机密允许应用程序注册门户中。  选择**生成的新密码**将生成并存储在各自的数据存储中，可以在应用程序中使用的共享的机密。  选择**生成新的密钥对**，则将创建新的公共/专用密钥对可以下载并使用 Azure ad 客户端身份验证。

## <a name="profile"></a>配置文件
应用程序注册门户的配置文件部分可用于自定义登录页面，您的应用程序。  此时，您可以更改登录页面的应用程序徽标、 服务 URL 和隐私声明中的条款。  徽标必须是透明 48x48 或 50x50 像素图像 GIF，PNG 或 JPEG 文件中为 15 KB 或更小。  请尝试更改这些值并在页中查看结果的符号 ！

## <a name="live-sdk-support"></a>实时的 SDK 支持
启用"实时 SDK 支持"时，您创建的任何应用程序机密将提供到 Azure 的 AD 和 Microsoft 客户数据存储区。  这将允许您的应用程序直接与 Microsoft 客户服务 (login.live.com) 进行集成。  如果您想要生成应用程序直接 （而不使用 Azure AD v2.0 终结点） 使用 Microsoft 帐户，应确保已启用实时 SDK 支持。

禁用实时 SDK 支持可确保应用程序密码仅写入 Azure 的广告数据存储。  Azure 的广告数据存储集成了企业级管理法规，使其符合某些标准，如 FISMA 法规遵从性。  如果您启用实时 SDK 支持，您的应用程序可能不获得遵守这些标准的一些。

如果您只打算使用 Azure AD v2.0 端点，可安全禁用实时 SDK 支持。

