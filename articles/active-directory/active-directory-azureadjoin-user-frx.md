<properties
    pageTitle="设置 Azure AD 安装过程中的新设备 |Microsoft Azure"
    description="解释如何用户设置 Azure 广告加入在其首次运行体验过程的主题。"
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>设置 Azure AD 安装过程中的新设备

Windows 10 中用户可以到 Azure 活动目录 (AD Azure) 加入他们的设备，在首次运行体验 (FRX)。 这使公司能够分发到他们的员工或学生，塑封的设备或让他们选择他们自己的设备 (CYOD)。
如果在设备上安装了 Windows 10 专业版或 Windows 10 企业版本，体验默认的公司拥有的设备的安装过程。

## <a name="to-join-a-device-to-azure-ad"></a>若要将设备加入到 Azure 的广告


1. 当您打开您的新设备并启动安装过程时，您应该看到**前的准备工作**的消息。 按照提示来设置您的设备。
2. 通过自定义您的地区和语言开始。 然后接受 Microsoft 软件许可条款。
![自定义您的区域的](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. 选择您要用于连接到 Internet 的网络。
4. 选择是否使用个人设备或公司所拥有的设备。 如果是公司所拥有，请单击**此设备属于我的组织**。 这将启动 Azure 广告加入体验。 下面是一个屏幕，您会看到是否您正使用 Windows 10 专业版。
<center>
![谁拥有此 PC 屏幕](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  输入由您的组织提供给您的凭据。
<center>
![登录屏幕](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  输入您的用户名后，匹配的租户位于 Azure 的广告。 如果您是在联盟的域中，您将重定向到内部安全令牌服务 (STS) 服务器--例如，多有 Active Directory 联合身份验证服务 (AD FS)。
7. 如果非联盟的域中的用户，直接在 Azure 的承载广告的页面上输入您的凭据。 如果公司品牌进行配置，也请参阅您的组织的徽标，并将支持文本。
8.  系统会提示您为多因素身份验证质询。 这一挑战是可以由 IT 管理员进行配置。
9.  Azure 广告检查此用户/设备是否需要在移动设备管理中的登记。
10. Windows Azure AD 中组织的目录中注册该设备和移动设备管理中进行注册，在适当情况下。
11. 如果您托管的用户，Windows 会将您带到桌面自动登录过程。
12. 如果您是联盟的用户，将您引导至 Windows 登录屏幕输入您的凭据。

> [AZURE.NOTE] 不支持联接内部 Windows 服务器的 Active Directory 域中 Windows 的全新体验。 因此，如果您打算将计算机加入到域，您应选择**将使用本地帐户的 Windows 设置**的链接。 如之前所做，然后可以在您的计算机上加入域的设置。

## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [没有通过 Microsoft Passport 密码身份验证身份](active-directory-azureadjoin-passport.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
