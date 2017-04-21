<properties
    pageTitle="Azure AD 连接︰ 解决连接问题 |Microsoft Azure"
    description="解释如何解决 Azure AD 连接的连接问题。"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure AD 连接的连接问题的疑难解答
本文介绍了 Azure AD 连接和 Azure 的广告之间的连接的工作原理，以及如何解决连接问题。 这些问题最有可能在代理服务器环境中看到。

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>安装向导中的连接问题的疑难解答
Azure AD 连接正在使用现代的身份验证 （使用 ADAL 库） 进行身份验证。 安装向导和正确同步引擎需要 machine.config，因为这些是.NET 应用程序进行正确配置。

在这篇文章中我们将展示 Fabrikam 通过其代理的 Azure AD 连接的方式。 代理服务器名称为 fabrikamproxy，使用端口 8080。

首先，我们需要确保正确配置了[**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) 。  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
一些非 Microsoft 博客中介绍的更改应该对 miiserver.exe.config 进行。 但是，该文件已被覆盖，因此，如果它能够工作在初始安装期间，系统将停止从事先升级每个升级上。 为此建议是更新 machine.config 相反。

代理服务器还必须打开所需的 Url。 官方的列表介绍了[Office 365 的 Url 和 IP 地址范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)。

其中下, 表是绝对的最小配置，以便能够在所有连接到 Azure 的广告。 此列表不包含任何可选功能，如密码写回或 Azure AD 连接的状况。 它是这里记录以帮助诊断故障的初始配置。

URL | 端口 | 说明
---- | ---- | ----
mscrl.microsoft.com | HTTP/80 | 用于下载的 CRL 列表。
\*。 verisign.com | HTTP/80 | 用于下载的 CRL 列表。
\*。 entrust.com | HTTP/80 | 用于为 MFA 下载 CRL 列表。
\*。 windows.net | HTTPS/443 | 用于登录到 Azure 的广告。
secure.aadcdn.microsoftonline p.com | HTTPS/443 | 用于 MFA。
\*。 microsoftonline.com | HTTPS/443 | 用来配置 Azure 的广告目录和导入/导出数据。

## <a name="errors-in-the-wizard"></a>在该向导中的错误
安装向导正在使用两种不同的安全上下文。 在**连接到 Azure 的广告**页上它使用当前登录的用户。 在页上**配置**它正在改变[帐户运行同步引擎的服务](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts)。 我们的代理服务器配置是全局到计算机的所以如果有问题，该问题很有可能出现在向导中**连接到 Azure 的广告**页已。

这些是最常见的错误，您将看到安装向导中。

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>未正确配置安装向导
此向导本身无法连接代理服务器时，将出现此错误。
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- 如果您看到此操作，请验证是否已正确配置[machine.config](active-directory-aadconnect-prerequisites.md#connectivity) 。
- 如果该号码正确，请按照[验证代理服务器连接](#verify-proxy-connectivity)以查看问题是否存在以及该向导中的步骤操作。

### <a name="the-mfa-endpoint-cannot-be-reached"></a>不能访问的 MFA 终结点
如果不能访问终结点**https://secure.aadcdn.microsoftonline-p.com** ，您的全局管理员已启用的 MFA，将出现此错误。  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- 如果您看到此操作，请确认已将终结点 secure.aadcdn.microsoftonline p.com 添加到代理服务器。

### <a name="the-password-cannot-be-verified"></a>无法验证该密码
如果安装向导已成功地连接到 Azure 的广告，但无法验证密码本身，您将看到︰ ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- 临时密码是密码，必须更改？ 它是真正正确的密码？ 尝试登录到 https://login.microsoftonline.com （在 Azure AD 连接服务器之外的另一台计算机），并验证该帐户可用。

### <a name="verify-proxy-connectivity"></a>验证代理服务器的连接
要验证 Azure AD 连接服务器是否实际连接代理服务器与互联网我们将使用一些 PowerShell 请参阅代理服务器是否这样的 web 请求。 在 PowerShell 的提示符下，运行`Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`。 （从技术上讲的第一个调用是 https://login.microsoftonline.com 和这将正常工作，但其他的 URI 是更快地作出响应）。

PowerShell 将使用 machine.config 中配置代理服务器，请与联系。 Winhttp/netsh 的设置应不影响这些 cmdlet。

如果代理服务器配置正确，您应该获取成功状态︰ ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

如果您收到**不能连接到远程服务器**然后 PowerShell 试图进行直接调用，而不使用代理服务器或 DNS 配置不正确。 请确保正确配置了**machine.config**文件。
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

如果代理服务器配置不正确，我们将收到一个错误︰ ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

错误 | 错误文本 | 注释
---- | ---- | ---- |
403 | 禁止访问 | 代理请求的 URL 不打开。 重新访问代理服务器配置，确保已打开[的 Url](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) 。
407 | 要求代理身份验证 | 代理服务器要求登录并无提供。 如果您的代理服务器要求身份验证，请确保已配置在这。 此外请确保运行向导以及与该服务帐户的用户使用的域帐户。

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD 连接和 Azure 的广告之间的通信模式
如果您已执行所有这些步骤上面，但是仍无法连接您可能此时开始筹划网络日志。 这一节记录正常成功和不成功的连接模式。 它还会列出常见的红色 herrings 如果您正在阅读的网络日志可以忽略这些。

- 将对 https://dc.services.visualstudio.com 的调用。 它不需要已经成功安装的代理在此打开并可以忽略这些。
- 您将看到 dns 解析将列出实际的主机将 DNS 名称空间 nsatc.net 和 microsoftonline.com 不受其他命名空间中。 但是，不会出现任何 web 服务请求的实际的服务器名称和您不需要将它们添加到代理服务器。
- 终结点 adminwebservice 和 provisioningapi （参见下面日志中） 发现的终结点和用来查找实际终结点使用，并将根据您所在的区域不同。

### <a name="reference-proxy-logs"></a>引用代理日志
下面是一个实际的代理日志转储及拍摄时从安装向导页 （到相同的终结点的重复项已被删除）。 这可作为引用自己的代理服务器和网络日志。 实际的终结点可能会在您的环境中不同 (尤其是那些在*倾斜*)。

**连接到 Azure 的广告**

时间 | URL
--- | ---
1/11/2016 8:31 | connect://login.microsoftonline.com:443
1/11/2016 8:31 | connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | 连接: / /*bba800 定位*。 microsoftonline.com:443
1/11/2016 8:32 | connect://login.microsoftonline.com:443
1/11/2016 8:33 | connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | 连接: / /*bwsc02 中继*。 microsoftonline.com:443

**配置**

时间 | URL
--- | ---
1/11/2016 8:43 | connect://login.microsoftonline.com:443
1/11/2016 8:43 | 连接: / /*bba800 定位*。 microsoftonline.com:443
1/11/2016 8:43 | connect://login.microsoftonline.com:443
1/11/2016 8:44 | connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | 连接: / /*bba900 定位*。 microsoftonline.com:443
1/11/2016 8:44 | connect://login.microsoftonline.com:443
1/11/2016 8:44 | connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | 连接: / /*bba800 定位*。 microsoftonline.com:443
1/11/2016 8:44 | connect://login.microsoftonline.com:443
1/11/2016 8:46 | connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | 连接: / /*bwsc02 中继*。 microsoftonline.com:443

**初始同步**

时间 | URL
--- | ---
1/11/2016 8:48 | connect://login.windows.net:443
1/11/2016 8:49 | connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:49 | 连接: / /*bba900 定位*。 microsoftonline.com:443
1/11/2016 8:49 | 连接: / /*bba800 定位*。 microsoftonline.com:443

## <a name="authentication-errors"></a>身份验证错误
本节介绍可以从 ADAL （使用 Azure AD 连接的身份验证库） 和 PowerShell 返回的错误。 所述的错误可以帮助您在理解您下一步的行动。

### <a name="invalid-grant"></a>无效的授权
无效的用户名或密码。 有关详细信息，请参阅[无法验证该密码](#the-password-cannot-be-verified)。

### <a name="unknown-user-type"></a>未知的用户类型
无法找到或解决 Azure 的广告目录。 也许您尝试使用未经验证的域中的用户名进行登录吗？

### <a name="user-realm-discovery-failed"></a>用户领域发现失败
网络或代理服务器配置问题。 网络不能达到，请参阅[安装向导中的解决连接问题](#troubleshoot-connectivity-issues-in-the-installation-wizard)。

### <a name="user-password-expired"></a>用户密码已过期
您的凭据已过期。 更改您的密码。

### <a name="authorizationfailure"></a>AuthorizationFailure
未知的问题。

### <a name="authentication-cancelled"></a>身份验证被取消
多因素身份验证 (MFA) 的挑战已被取消。

### <a name="connecttomsonline"></a>ConnectToMSOnline
身份验证是成功的但 Azure AD PowerShell 有身份验证问题。

### <a name="azurerolemissing"></a>AzureRoleMissing
身份验证成功。 您不是全局管理员。

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
身份验证成功。 特权的身份管理功能已启用，您当前不是全局管理员。 详细信息，请参阅[特权身份管理](active-directory-privileged-identity-management-getting-started.md)。

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
身份验证成功。 无法检索从 Azure AD 公司的信息。

### <a name="retrievedomains"></a>RetrieveDomains
身份验证成功。 无法检索从 Azure AD 域信息。

## <a name="troubleshooting-steps-for-previous-releases"></a>以前的版本中的故障排除步骤。
与版本开始生成号为 1.1.105.0 （发布 2 月 2016年） 登录的助手在停用。 这一节和配置应该不再是必需的但仍保留作为参照。

单一登录助理工作中，必须配置 winhttp。 这可以使用[**netsh**](active-directory-aadconnect-prerequisites.md#connectivity)。  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>未正确配置登录助手
登录的助手不能进入该代理或代理不允许该请求时，将出现此错误。
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- 如果您看到此操作，请在[netsh](active-directory-aadconnect-prerequisites.md#connectivity)的代理配置查看并验证它是否正确。
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- 如果该号码正确，请按照[验证代理服务器连接](#verify-proxy-connectivity)以查看问题是否存在以及该向导中的步骤操作。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
