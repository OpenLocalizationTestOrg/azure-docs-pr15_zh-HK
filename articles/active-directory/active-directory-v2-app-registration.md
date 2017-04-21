<properties
    pageTitle="2.0 版应用程序注册 |Microsoft Azure"
    description="如何启用登录和访问 Microsoft 服务使用 v2.0 端点向 Microsoft 注册应用程序"
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

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>如何注册使用 2.0 版的终结点的应用程序

若要生成应用程序接受 MSA 和 Azure 的广告登录，首先需要向 Microsoft 注册应用程序。  到目前为止，您将无法使用任何现有的应用程序，您可能必须使用 Azure 广告或 MSA-您将需要创建一个新。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="visit-the-microsoft-app-registration-portal"></a>请访问 Microsoft 应用程序注册门户
第一件事第一次的定位到[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)中。  这是新的应用程序注册门户，您可以您的 Microsoft 应用程序。

登录个人或工作或学校 Microsoft 帐户。  如果您没有或者注册新的个人帐户。 接下来，它不需要长时间-我们将在这里等待。

做呢？ 您应该现在关注的 Microsoft 应用程序，您的列表可能为空。  让我们来更改它。

单击**添加应用程序**，然后为它指定一个名称。  门户将分配您的应用程序在您的代码的后面您将使用全局唯一的应用程序 Id。  如果您的应用程序包含一个服务器端组件，需要调用 Api 的访问令牌 (认为︰ 办公室、 Azure 或您自己的 web API)，您需要同时创建下面的**应用程序密码**。
<!-- TODO: Link for app secrets -->

接下来，添加您的应用程序将使用的平台。

- 对于基于 web 的应用程序，提供了登录消息可以发送到**重定向 URI** 。
- 移动应用程序，将复制下自动为您创建的默认重定向 uri。

（可选） 您可以自定义在配置文件部分登录页的外观和感觉。  请确保在移动之前单击**保存**。

> [AZURE.NOTE] 当您创建一个使用[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)应用程序时，应用程序将注册中的帐户，用于登录到门户的住宅租户。  这意味着您可以在您使用 Microsoft 个人帐户的 Azure AD 租户中注册应用程序。  如果您明确想要在特定租户中注册应用程序，登录该租户在最初创建的帐户。

## <a name="build-a-quick-start-app"></a>构建一个快速启动应用程序
既然您已经有一个 Microsoft 应用程序，您可以完成我们 v2.0 快速入门教程之一。  下面是一些建议︰

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
