<properties
    pageTitle="没有通过 Microsoft Passport 密码身份验证 |Microsoft Azure"
    description="提供有关部署 Microsoft Passport 的 Microsoft Passport 和其他信息的概述。"
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

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>没有通过 Microsoft Passport 密码身份验证身份

使用单独的密码进行身份验证的当前方法都不能保证用户的安全。 用户重用，并且忘记了密码。 密码是 breachable，phishable，容易产生裂纹，并猜出。 他们还将获取难以记住并且易于遭受类似"[通过哈希](https://technet.microsoft.com/dn785092.aspx)"的攻击。

## <a name="about-microsoft-passport"></a>有关 Microsoft Passport
Passport 是专用/公共密钥或超出密码的基于证书的身份验证方法为企业和消费者。 这种形式的身份验证依赖于密钥对凭据，可以替换密码被破坏、 被盗案中和网络钓鱼的抵抗。

 Passport 允许用户向 Microsoft 帐户、 Windows 服务器的 Active Directory 帐户、 一个 Microsoft Azure 活动目录 (AD Azure) 的帐户或非 Microsoft 服务支持快速标识在线 (FIDO) 身份验证进行身份验证。 最初的两步验证之后 Microsoft Passport 注册时，在用户的设备上，设置 Microsoft Passport 并用户设置笔势，它可以是 Windows 你好或 PIN。 用户提供的笔势来验证他们的身份。 Windows 使用 Microsoft Passport 来对用户进行身份验证，并帮助他们访问受保护的资源和服务。

专用密钥由只能通过"用户笔势"如针、 生物或类似的智能卡，用户使用登录到该设备的远程设备。 此信息将链接到证书或非对称密钥对。 私钥是 attested 设备是否受信任的平台模块 (TPM) 芯片的硬件。 私钥从不离开设备。

公共密钥注册 Azure Active Directory 和 Windows 服务器的 Active Directory （用于内部部署）。 身份提供程序 (IDPs) 通过映射的用户的公钥的私钥，验证用户，并提供登录信息通过一次性密码 (OTP)、 PhoneFactor 或不同的通知机制。

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>为什么企业应该采用 Microsoft Passport

通过启用 Microsoft Passport，企业可以进行他们的资源通过更加安全︰

* 使用硬件首选选项设置 Microsoft Passport。 这意味着将 TPM 1.2 或 TPM 2.0 可用时生成的键。 当 TPM 不可用时，软件将生成密钥。

* 定义的复杂性和长度的 PIN，以及是否在您的组织中启用呼叫使用。

* 配置 Microsoft Passport 来支持智能卡式方案，通过使用基于证书的信任。

## <a name="how-microsoft-passport-works"></a>Passport 的工作原理
1. 由 TPM 或软件在硬件上生成密钥。 许多设备都被集成到设备加密密钥保护硬件的内置 TPM 芯片。 TPM 1.2 或 TPM 2.0 生成密钥或从生成的密钥创建的证书。

2. TPM 证明这些硬件绑定项。

3. 解锁单个笔势解锁设备。 此笔势允许访问多个资源域加入或 Azure 设备是否加入广告的。

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Passport 生命周期原理

![Passport 生命周期](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

前面的关系图阐释了专用/公共密钥对和标识提供程序的验证。 这里详细介绍每个步骤︰

1. 用户通过 （手势，物理的智能卡，多因素身份验证） 的多个内置校对方法证明他们的身份，并将此信息发送到身份提供程序 (IDP) 类似 Azure Active Directory 或部署 Active Directory。

2. 设备然后创建密钥、 证明该项、 采用此密钥的公共部分，将其附加与站语句、 登陆，并发送到 IDP 注册密钥。

4. 只要 IDP 注册密钥的公共部分，IDP 挑战设备要用专用部分密钥进行签名。

5. IDP 然后验证并发出身份验证令牌，使用户和设备访问受保护的资源。 IDPs 可以编写跨平台的应用程序，或使用浏览器支持 （通过 JavaScript/Webcrypto Api) 来创建和对他们的用户使用 Microsoft Passport 凭据。

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Passport 配置要求
### <a name="at-the-enterprise-level"></a>在企业级

* 企业的 Azure 的订阅。

### <a name="at-the-user-level"></a>在用户级别

* 用户的计算机将运行 Windows 10 专业版或者企业。

有关详细的部署说明，请参阅[工作在组织中启用 Microsoft Passport](active-directory-azureadjoin-passport-deployment.md)。


## <a name="additional-information"></a>其他信息

* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
