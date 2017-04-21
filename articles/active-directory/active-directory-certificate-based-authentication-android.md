<properties 
    pageTitle="开始使用基于证书的身份验证，在 Android 上 |Microsoft Azure" 
    description="了解如何使用 Android 设备解决方案中配置基于证书的身份验证" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>开始使用 Android 的公共预览基于证书身份验证  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


本主题演示如何配置和使用基于证书的身份验证 (CBA) 用户的 Android 设备上承租人 Office 365 企业、 商业和教育的计划中。 

CBA 允许您进行身份验证通过 Azure Active Directory Android 或 iOS 设备上的客户端证书时连接到 Exchange 联机帐户︰ 

- 例如，Microsoft Outlook 和 Microsoft Word 办公移动应用程序   
- Exchange ActiveSync (EAS) 客户端 

配置此功能无需输入用户名和密码组合到某些邮件和 Microsoft Office 应用程序在您的移动设备上。 
 

## <a name="supported-scenarios-and-requirements"></a>受支持的方案和要求  



### <a name="general-requirements"></a>一般要求 


本主题中的所有方案，都需要以下任务︰  

- 对证书 authority(s) 以颁发客户端证书的访问。  

- 必须在 Azure Active Directory 配置证书 authority(s)。 您可以找到有关如何完成[入门](#getting-started)部分中的配置的详细的步骤。  

- 必须在 Azure Active Directory 配置根证书颁发机构和中间证书颁发机构。  

- 每个证书颁发机构必须具有可通过 Internet 面临 URL 引用一个证书吊销列表 (CRL)。  

- 必须颁发客户端身份验证的客户端证书。  


- 对于 Exchange ActiveSync 仅客户端，客户端证书必须具有用户的可路由的电子邮件地址在 Exchange 联机主体名称或 RFC822 名称值主题备用名称字段中。 Azure 的 Active Directory 将 RFC822 值映射到目录中的代理服务器地址属性。  



### <a name="office-mobile-applications-support"></a>移动应用程序支持 

| 应用程序                      | 支持      |
| ---                       | ---          |
| 单词 / Excel / PowerPoint | ![检查][1]  |
| OneNote                   | 即将推出  |
| OneDrive                  | ![检查][1]  |
| Outlook                   | ![检查][1]  |
| Yammer                    | ![检查][1]  |
| Skype 的业务        | ![检查][1]  |


### <a name="requirements"></a>要求  

设备操作系统版本必须为 Android 5.0 （棒糖形） 及以上。 

必须配置联合身份验证服务器。  


对 Azure Active Directory 吊销客户端证书，ADFS 标记都必须有下列索赔︰  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
（客户端证书的序列号） 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
（对于客户端证书的颁发者的字符串） 

Azure 的 Active Directory 添加到刷新令牌这些声明如果有在 ADFS 标记 （或任何其他 SAML 令牌）。 当刷新标记需要进行验证时，此信息用于检查吊销。 

作为最佳实践，应更新 ADFS 错误页，其中说明如何获得用户的证书。 

有关更多详细信息，请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync 客户端支持 


支持某些 Exchange ActiveSync 应用程序，Android 5.0 （棒糖形） 或更高版本。 若要确定是否您的电子邮件应用程序支持此功能，请联系您的应用程序开发人员。 



## <a name="getting-started"></a>入门教程 


若要开始，您需要配置在 Azure Active Directory 中的证书颁发机构。 每个证书颁发机构，上载以下︰ 

- 证书，以*.cer*格式的公用部分 

- 互联网面临的 Url 驻留证书吊销列表 (Crl)
 

下面是一个证书颁发机构的架构︰ 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


若要上载的信息，您可以使用通过 Windows PowerShell 的 Azure 的广告模块。  
下面是示例的添加、 删除或修改证书颁发机构。 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>配置证书您 Azure AD 租户基于身份验证 

1. 使用管理员特权启动 Windows PowerShell。 

2. Azure AD 模块安装。 您需要安装版本[1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0)或更高版本。  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. 连接到目标租户︰ 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>添加新的证书颁发机构

1. 设置证书颁发机构的各种属性，并将其添加到 Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. 获得证书颁发机构︰ 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>检索列表中的证书颁发机构

检索当前为您的租户存储 Azure Active Directory 中的证书颁发机构︰ 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>删除证书颁发机构

1.  检索证书颁发机构︰ 

        $c=Get-AzureADTrustedCertificateAuthority 


2. 删除证书颁发机构的证书︰ 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying 的证书颁发机构 

1.  检索证书颁发机构︰ 

        $c=Get-AzureADTrustedCertificateAuthority 


2. 修改证书颁发机构的属性︰ 

        $c[0].AuthorityType=1 

3. 设置**证书颁发机构**︰ 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>测试办公室移动应用程序  

若要测试证书上移动 Office 应用程序的身份验证︰ 

1.  在测试设备上，从 Google 播放存储区中安装 Office 移动应用程序 (例如 OneDrive)。

2.  验证用户证书具有已设置为您的测试设备。 

3.  启动应用程序。 

4.  输入您的用户名，然后选择要使用的用户证书。 

您应在成功登录。 





## <a name="testing-exchange-activesync-client-applications"></a>测试 Exchange ActiveSync 客户端应用程序

若要访问 Exchange ActiveSync 通过基于证书的身份验证，包含客户端证书 EAS 配置文件必须是可用于应用程序。 EAS 配置文件必须包含以下信息︰

- 要用于身份验证的用户证书 

- EAS 终结点必须是 outlook.office365.com （如当前仅在 Exchange 在线多租户环境中支持此功能）

EAS 配置文件可以配置，并且可以放在通过 MDM 例如 Intune 或者通过手动将该证书放在 EAS 配置文件的设备上使用的设备上。  


### <a name="testing-eas-client-applications-on-android"></a>测试 EAS 在 Android 上的客户端应用程序 

若要测试一个应用程序，Android 5.0 （棒糖形） 或更高版本的证书身份验证，请执行以下步骤︰  

1. 能够满足上述要求的应用程序中配置 EAS 配置文件。  


2. 一旦正确配置了配置文件后，打开的应用程序，并验证邮件处于正在同步状态。 




## <a name="revocation"></a>吊销

若要吊销客户端证书，Azure Active Directory 从证书颁发机构的信息的一部分上载 Url 获取证书吊销列表 (CRL)，并将其缓存。 上一次发布的 CRL 的时间戳 （**生效日期**属性） 用于确保 CRL 仍然有效。 CRL 定期参考以撤销访问权限的列表中的证书。

如果需要 （例如，如果用户丢失设备） 更即时吊销，授权令牌的用户将失效。 要使之无效的授权令牌，将**StsRefreshTokenValidFrom**字段设置使用 Windows PowerShell 此特定用户。 您必须更新您想要废除访问权的每个用户的**StsRefreshTokenValidFrom**字段。
 
为了确保吊销仍然存在，您必须设置 CRL 的**生效日期**为日期的值由**StsRefreshTokenValidFrom**设置，确保该证书后问题是 CRL 中。
 
以下步骤概述了更新，并通过将**StsRefreshTokenValidFrom**字段设置无效授权标记的过程。 

1. 使用管理员凭据 MSOL 服务连接︰ 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  检索当前用户的 StsRefreshTokensValidFrom 值︰ 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  配置新的 StsRefreshTokensValidFrom 值的用户相同的当前时间戳︰ 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


您设置的日期必须在将来。 该日期不是在将来，如果未设置**StsRefreshTokensValidFrom**属性。 如果该日期是在将来， **StsRefreshTokensValidFrom**设置为当前时间 （不是由一组 MsolUser 命令日期）。 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png