<properties
    pageTitle="如何以静默方式安装 Azure 的广告应用程序代理服务器连接器 |Microsoft Azure"
    description="介绍了如何执行无提示安装的 Azure AD 应用程序代理服务器连接器，以提供内部应用程序安全地远程访问。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>如何以静默方式安装 Azure 的广告应用程序代理服务器连接器

您想要将能够发送到多个 Windows 服务器或没有用户界面启用的 Windows 服务器的安装脚本。 本主题说明如何创建 Windows PowerShell 脚本，使无人参与的安装来安装并注册 Azure 的广告应用程序代理服务器连接器。

## <a name="enabling-access"></a>启用访问
应用程序代理服务器的工作原理是安装超薄 Windows Server 服务调用在您的网络的接口。 应用程序代理服务器连接器工作它已使用 Azure 的广告目录使用全局管理员和密码进行注册。 通常这是安装过程中输入连接器中弹出对话框。 或者，您可以使用 Windows PowerShell 创建凭据对象以输入您的注册信息，或创建您自己的标记，并使用它来输入您的注册信息。

## <a name="step-1--install-the-connector-without-registration"></a>步骤 1︰ 安装连接器而无需注册


安装接头 Msi 而无需注册，如下所示的连接器︰


1. 打开命令提示符。
2. 运行下面的命令在其中 /q 意味着安静模式安装的安装不会提示您接受最终用户许可协议。

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>步骤 2︰ 注册 Azure Active Directory 连接器
这可通过以下方法之一︰


- 注册使用 Windows PowerShell 凭据的连接器
- 注册使用脱机创建标记的连接器

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>注册使用 Windows PowerShell 凭据的连接器


1. 通过运行以下命令，创建 Windows PowerShell 凭据对象，"<username>"和"<password>"应被替换的用户名和密码为您的目录︰

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. 转到**C:\Program 数值 AAD 的应用程序代理服务器连接器**并运行该脚本，使用 PowerShell 凭据对象创建，其中 $cred 是 PowerShell 名称凭据创建的对象︰

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>注册使用脱机创建标记的连接器

1. 创建脱机令牌使用 AuthenticationContext 类中的代码段中使用的值︰


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. 一旦您有创建使用标记 SecureString 令牌︰ <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. 运行以下 Windows PowerShell 命令，其中 SecureToken 是上述步骤中创建的标记的名称，tenantID 是租户的 GUID: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>请参见

- [启用 Azure 的活动目录的应用程序代理](active-directory-application-proxy-enable.md)
- [发布应用程序使用您自己的域名](active-directory-application-proxy-custom-domains.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [解决您在具有应用程序代理时遇到的问题](active-directory-application-proxy-troubleshoot.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
