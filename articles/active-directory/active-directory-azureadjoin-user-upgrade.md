<properties
    pageTitle="从设置设置 Azure AD 的 Windows 10 设备 |Microsoft Azure"
    description="解释如何可以将用户加入到 Azure AD 通过设置菜单。"
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>从设置设置 Azure AD 的 Windows 10 设备
如果您已经在使用 Windows 7 或 Windows 8，并且您的计算机或设备已升级到 Windows 10，可以通过设置菜单加入到 Azure 活动目录 (AD Azure)。

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>加入到 Azure 的广告，从设置菜单


1. 从**开始**菜单上，单击**设置**超级按钮。
2. 从**设置**，选择**系统**->**有关**->**加入 Azure 的广告**。
<center>
![加入从设置菜单的 Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. 单击**继续**Azure 广告加入消息窗口中。
<center>
![联接 Azure 广告邮件窗口](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. 提供您的登录凭据。 此登录体验将包括完整的身份验证所需的所有步骤。 如果联盟组织的一部分，您的管理员将为您提供由您的组织的联合身份验证体验。
<center>
![提供的登录凭据](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. 如果您的组织已加入到 Azure AD 配置 Azure 多因素身份验证，提供之前的第二个因素。
6. 在**允许管理此设备**屏幕上单击**接受**。
7. 您应该看到该消息"您的设备现在加入到您的组织在 Azure 的广告"。


## <a name="additional-information"></a>其他信息
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
* [没有通过 Microsoft Passport 密码身份验证身份](active-directory-azureadjoin-passport.md)
