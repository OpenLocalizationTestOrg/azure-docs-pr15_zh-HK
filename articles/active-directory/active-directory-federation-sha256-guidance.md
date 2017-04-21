<properties
    pageTitle="更改签名的哈希算法用于 Office 365 回复方信任 |Microsoft Azure"
    description="此页提供用于更改与 Office 365 的联合身份验证信任的 SHA 算法准则"
    keywords="SHA1 SHA256、 O365，联盟、 aadconnect、 adf，ad fs、 更改 sha，联合身份验证信任，信赖方信任"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>针对 Office 365 提供答复方信任更改签名的哈希算法

## <a name="overview"></a>概述

Azure 的 Active Directory 联合身份验证服务 (AD FS) 签名令牌到 Microsoft Azure Active Directory 以确保它们不能被篡改。 此签名可以基于 SHA1 或 SHA256。 Azure 的 Active Directory 现在支持使用 sha256 散列算法，签名的标记，我们建议设置令牌签名算法 SHA256 到最高的安全级别。 本文介绍了设置为更安全 SHA256 令牌签名算法级别所需的步骤。

## <a name="change-the-token-signing-algorithm"></a>更改令牌签名算法

设置具有下面两个过程之一的签名算法后，AD FS 将信赖方信任使用 SHA256 Office 365 的签名标记。 不需要进行任何额外的配置更改，并且此更改对您能够访问 Office 365 或其他 Azure AD 应用程序无任何影响。

### <a name="ad-fs-management-console"></a>AD FS 管理控制台

1. 打开上主的 AD FS 服务器的 AD FS 管理控制台。
2. 展开 AD FS 节点，然后单击**配置成信赖方信任**。
3. 用鼠标右键单击您 Office 365/Azure 信赖方信任并选择**属性**。
4. 选择**高级**选项卡，然后选择安全哈希算法 SHA256。
5. 单击**确定**。

![Sha256 散列签名算法 — MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell cmdlet

1. 在任何 AD FS 服务器上，管理员权限打开 PowerShell。
2. 通过使用**一组 AdfsRelyingPartyTrust** cmdlet 设置安全哈希算法。

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>请参阅

* [修复 Office 365 的 Azure AD 连接与信任](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
