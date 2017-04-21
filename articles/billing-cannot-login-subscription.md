<properties
    pageTitle="无法登录到 Azure 订阅 |Microsoft Azure"
    description="介绍如何解决一些常见的 Azure 订阅登录问题。"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>我不能登录以进行管理 Azure 订阅

本文将引导您通过一些最常见的方法来解决登录问题。

## <a name="page-hangs-in-the-loading-status"></a>在加载状态挂起页

如果您的 internet 浏览器页面挂起，尝试以下步骤的每一个，直到可以获取到[Azure 的门户](https://portal.azure.com)。

-   刷新页面。
-   使用不同的 Internet 浏览器。
-   如果您使用的 Microsoft Internet Explorer，使用浏览到 Azure 门户 InPrivate 浏览模式。 

    答︰  单击**工具**![工具按钮](./media/billing-cannot-login-subscription/Toolsbutton.png) > **安全** > **InPrivate 浏览**。

    B。  浏览到[Azure 的门户网站](https://portal.azure.com)，然后再登录到门户。

## <a name="error-message-no-subscriptions-found"></a>错误消息"未找到订阅内容"

如果您的帐户没有足够的权限，您可能会看到**找不到订阅**错误消息。 只有帐户管理员可以获得对[客户中心](https://account.windowsazure.com/)，不是服务管理员 (SA) 或共同管理员 (CA)。

**方案 1︰ 在[Azure 门户](https://portal.azure.com)中收到错误消息**

若要解决此问题，请为帐户[添加联管理员或所有者的角色](billing-add-change-azure-subscription-administrator.md)。

**方案 2︰ 在[Azure 帐户中心](https://account.windowsazure.com/Subscriptions)收到错误消息**

请检查您使用的帐户是否是帐户管理员联系。 要验证帐户管理员是谁，请执行以下步骤︰

1.  登录到[Azure 的门户](https://portal.azure.com)。
2.  在集线器菜单选择**订阅**。
3.  选择您要检查，该订阅，然后选择**设置**。
4.  选择**属性**。 在**帐户管理**框中显示订阅的帐户管理员。

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>自动以其他用户身份登录

如果您使用 Internet 浏览器中的多个用户帐户，可以出现此问题。

若要解决此问题，请尝试下列方法之一︰

-   清除缓存并删除互联网的 cookie。 在 Internet Explorer 中单击**工具**![工具按钮](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internet 选项** > **删除**。 请确保选择临时文件、 cookie、 密码和浏览历史记录所对应的复选框，然后单击删除。

-   若要还原所做的任何个人设置 Internet Explorer 设置重置。 单击**工具**![工具按钮](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internet 选项** > **高级**>**删除个人设置**框中选择 >**重置**。

-   浏览到 Azure 门户 InPrivate 浏览模式。 单击**工具**![工具按钮](./media/billing-cannot-login-subscription/Toolsbutton.png) > **安全** > **InPrivate 浏览**。

## <a name="need-help-contact-support"></a>需要帮助吗？ 与支持部门联系。 

如果您仍然需要帮助，请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)以获取快速解决您的问题。 