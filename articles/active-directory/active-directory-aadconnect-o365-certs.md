<properties
    pageTitle="证书续订 Office 365 和 Azure Active Directory 用户指南 |Microsoft Azure"
    description="本文介绍如何解决问题的电子邮件，通知他们有关续订证书给 Office 365 的用户。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>更新的 Office 365 和 Azure Active Directory 联合身份验证证书

##<a name="overview"></a>概述

为 Azure 活动目录 (AD Azure) 和 Active Directory 联合身份验证服务 (AD FS) 之间成功联盟，AD FS 用于对 Azure AD 安全令牌签名证书应在 Azure AD 配置相匹配。 任何不匹配会导致破坏的信任。 Azure AD 确保此信息保持同步 （用于访问外部网） 中部署 AD FS 和 Web 应用程序代理。

这篇文章为您提供其他信息来管理您的令牌签名证书并使它们保持同步，使用 Azure 的广告，在以下情况中︰

* 不想要部署 Web 应用程序代理，并因此不能在外部网联合元数据。
* 您没有使用默认配置 AD FS 的令牌签名证书。
* 您正在使用第三方身份提供程序。

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>令牌签名证书的 AD FS 的默认配置

令牌签名和解密证书的标记通常是自签名的证书，并且一年好。 默认情况下，AD FS 包括一个名为**AutoCertificateRollover**的自动续订过程。 如果您正在使用 AD FS 2.0 或更高版本，Office 365 和 Azure 广告自动更新您的证书到期之前将它。

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>续订从 Office 365 门户网站或电子邮件的通知

>[AZURE.NOTE] 如果您收到一封电子邮件或门户通知要求您的 Office 续订您的证书，请参阅[管理变为令牌签名证书](#managecerts)来检查您是否需要执行任何操作。 Microsoft 已意识到可能的问题，可能会导致证书续订发送，即使不不需要任何操作的通知。

Azure 广告尝试监视联合元数据，并更新签名证书，由这种元数据标记。 令牌签名证书的到期前 30 天 Azure 的广告会检查新的证书是否可通过轮询联合元数据。

* 如果成功，它可以轮询联合元数据和检索新证书，没有电子邮件通知或警告在 Office 365 门户被颁发给用户。
* 如果它不能检索新的令牌签名证书，或者由于不能访问联合元数据或未启用自动证书翻转，Azure 广告发出电子邮件通知和 Office 365 门户中的警告。

![Office 365 门户通知](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] 如果您使用 AD FS 中，以确保业务连续性，请验证您的服务器有以下更新程序，以便不会发生的已知问题的身份验证失败。 这也会降低此续订和将来的续订期已知的 AD FS 代理服务器问题︰
>
>Server 2012 R2- [Windows Server 可能 2014年汇总](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 和 2012- [Windows Server 2012 或 Windows 2008 R2 SP1 通过代理服务器身份验证失败](http://support.microsoft.com/kb/3094446)

## 检查证书是否需要更新<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>步骤 1︰ 检查 AutoCertificateRollover 状态

在 AD FS 服务器上，打开 PowerShell。 检查 AutoCertificateRollover 的值被设置为 True。

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] 如果您正在使用 AD FS 2.0，第一次运行添加 Pssnapin Microsoft.Adfs.Powershell。

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>第 2 步︰ 确认 AD FS 和 Azure 广告同步

在 AD FS 服务器上，打开 Azure AD PowerShell 提示符，并连接到 Azure 的广告。

>[AZURE.NOTE] 您可以下载 Azure AD PowerShell[此处](https://technet.microsoft.com/library/jj151815.aspx)。

    Connect-MsolService

检查证书在 AD FS 和 Azure AD 信任指定的域的属性中配置。

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![获得 MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

如果在两个输出端的指纹匹配，是 Azure AD 同步您的证书。

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>步骤 3︰ 检查您的证书已过期

在 Get MsolFederationProperty 或获取 AdfsCertificate 的输出中，检查该日期在下"不之后。" 如果该日期是不少于 30 天之内，则应采取措施。

| AutoCertificateRollover | 证书与 Azure AD 同步 | 被公开访问联合元数据 | 有效期 | 操作 |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| 是的 | 是的 | 是的 | - | 不需要任何操作。 [更新标记自动签名证书](#autorenew)，请参阅。 |
| 是的 | 不  | - | 不少于 15 天 | 立即续订。 请参阅[续订令牌签名证书手动](#manualrenew)。 |
| 不 | - | - | 少于 30 天 | 立即续订。 请参阅[续订令牌签名证书手动](#manualrenew)。 |

\[-] 并不重要

## 令牌签名证书的自动更新 （推荐）<a name="autorenew"></a>

您不需要执行任何手动步骤，如果具备以下两个条件︰
- 已部署 Web 应用程序代理，可以启用访问联合元数据与从外部网。
- 您正在使用 AD FS 默认配置 （AutoCertificateRollover 处于启用状态）。

检查以下项目，确认，可以自动更新该证书。

**1.AutoCertificateRollover AD FS 属性必须设置为 True。** 这表明，AD FS 将自动生成新的令牌签名和令牌解密证书之前旧, 的过期。

**2.AD FS 联合身份验证元数据是可公开访问的。** 请检查您联合元数据 （从公司网络中） 公用的 internet 上的计算机从导航到下面的 URL 是公共可访问︰


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

其中`(your_FS_name) `替换为您的组织使用，如 fs.contoso.com 的联合身份验证服务主机名。  如果您能够进行验证这些设置成功，则无需执行任何其他操作。  

示例︰ https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## 续订签名证书手动标记<a name="manualrenew"></a>

您可以选择要续订签名证书手动标记。 例如，以下情况可能会更好地工作，手动续订︰
* 令牌签名证书是不是自签名的证书。 这最常见的原因是企业管理 AD FS 登记的组织的证书颁发机构颁发的证书。
* 网络安全不允许联合元数据公开可用。

在这些情况下，每次更新的令牌签名证书，则还必须更新 Office 365 域使用 PowerShell 命令，更新 MsolFederatedDomain。

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>第 1 步︰ 确保 AD FS 具有新的令牌签名证书

**非默认配置**

如果您正在使用非默认配置 AD fs （其中**AutoCertificateRollover**设置为**False**），可以将使用自定义的证书 （不自签名）。 有关如何更新 AD FS 令牌签名证书的详细信息，请参阅[客户不使用 AD FS 指南自签名证书](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)。

**是不公开的联合元数据**

另一方面，如果**AutoCertificateRollover**设置为**真**，但不是公开访问联合元数据，首先要确保新的令牌签名证书已经由 AD FS。 确认您具有新的令牌签名证书，通过执行以下步骤︰

1. 请验证您登录到主的 AD FS 服务器。
2. 检查当前签名证书在 AD FS 中的，请打开 PowerShell 命令窗口，然后运行以下命令︰

    PS c:\>Get ADFSCertificate — CertificateType 令牌签名

    >[AZURE.NOTE] 如果您正在使用 AD FS 2.0，您应该首先运行添加 Pssnapin Microsoft.Adfs.Powershell。

3. 查看命令输出中，列出的任何证书。 如果 AD FS 已生成新的证书，您会看到输出中的两个证书︰ 一个**IsPrimary**值的**真**和**NotAfter**日期是在 5 天内，一个**IsPrimary**的**假**和**NotAfter**是关于将来的一年。

4. 如果您仅看到一份证书，并且**NotAfter**日期是在 5 天内，您需要生成新的证书。

5. 若要生成新的证书，请执行以下命令 PowerShell 命令提示符处︰ `PS C:\>Update-ADFSCertificate –CertificateType token-signing`。

6. 通过再次运行下面的命令来验证此更新︰ PS c:\>Get ADFSCertificate — CertificateType 令牌签名

现在应列出两个证书，其中之一已经大约一年以后， **NotAfter**日期和为其**IsPrimary**的值为**False**。

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>步骤 2︰ 更新新的令牌签名证书的 Office 365 信任

更新新的令牌签名证书信任，如下所示使用 Office 365。

1.  打开 Windows PowerShell Microsoft Azure Active Directory 模块。
2.  运行 $cred = 获取凭据。 当此 cmdlet 将提示您输入凭据时，请键入您的云服务管理员帐户凭据。
3.  运行连接 MsolService – 凭据 $cred。 此 cmdlet 将您连接到云服务。 运行任何其他 cmdlet 由此工具安装之前必须创建连接到云服务的上下文。
4.  如果您不是 AD FS 联合主服务器的计算机上运行这些命令，运行一组 MSOLAdfscontext-计算机<AD FS primary server>，其中<AD FS primary server>是主要的 AD FS 服务器内部 FQDN 名称。 此 cmdlet 创建的上下文中将您连接到 AD FS。
5.  运行更新 MSOLFederatedDomain – 域名<domain>。 此 cmdlet 将更新到云服务中，从 AD FS 设置并配置两个之间的信任关系。


>[AZURE.NOTE] 如果您需要支持多个顶级域，如 contoso.com 和 fabrikam.com，您必须与任何 cmdlet 使用**SupportMultipleDomain**开关。 有关详细信息，请参阅[支持多个顶层级别域](active-directory-aadconnect-multiple-domains.md)。

## 使用 Azure AD 连接修复 Azure AD 信任<a name="connectrenew"></a>

如果您配置您的 AD FS 服务器场和 Azure AD 信任使用 Azure AD 连接，可以使用 Azure AD 连接来检测您是否需要执行任何操作，您的令牌签名证书。 如果您需要续订证书，可以使用 Azure AD 连接来执行此操作。

有关详细信息，请参阅[修复了信任](./active-directory-aadconnect-federation-management.md#repairing-the-trust)。
