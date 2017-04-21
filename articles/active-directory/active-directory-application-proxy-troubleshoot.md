<properties
    pageTitle="解决应用程序代理服务器 |Microsoft Azure"
    description="介绍如何解决在 Azure AD 应用程序代理中的错误。"
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
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>解决应用程序代理

如果在访问发布的应用程序或发布应用程序中发生错误，检查以下选项来查看 Microsoft Azure AD 应用程序代理服务器是否正常工作︰

- 打开 Windows 服务控制台并验证是否已启用**Microsoft AAD 应用程序代理服务器连接器**服务和运行。 您可能还想要看一看应用程序代理服务的属性页中，如下图所示︰  
  ![Microsoft AAD 应用程序代理服务器连接器属性窗口屏幕快照](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- 打开事件查看器并在**应用程序和服务日志**的应用程序代理服务器连接器事件查找 > **Microsoft** > **AadApplicationProxy** > **连接器** > **管理**。
- 如果需要请更详细的日志均可通过上分析和调试日志和应用程序代理服务器连接器会话日志打开关闭。

## <a name="the-page-is-not-rendered-correctly"></a>不正确地呈现页面

如果您未收到具体的错误消息，则可能仍产生问题，与应用程序呈现或不能正常运行。 如果您已发布的文章的路径，但该应用程序需要存在该路径之外的内容，这会发生。

例如，如果您发布路径 https://yourapp/app，但应用程序调用 https://yourapp/media 中的图像，它们不会呈现。 请确保您发布应用程序使用最高级别路径需要包含所有相关内容。 在此示例中，它将是 http://yourapp/。

如果您更改了您的路径中包含被引用的内容，但仍需要用户上的更深层链接路径中，请参阅博客文章[设置在 Azure 广告接入面板和 Office 365 提供应用程序启动程序的应用程序代理服务器应用程序的正确链接](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/)。

## <a name="general-errors"></a>常规错误

错误 | 说明 | 解析
--- | --- | ---
无法访问此企业应用程序中。 您无权访问此应用程序。 授权失败。 请确保将具有访问权限的用户分配给此应用程序。 | 您可能未分配的用户对此应用程序。 | 转到**应用程序**选项卡，然后在**用户和组**下，将该用户或用户组分配对此应用程序。
无法访问此企业应用程序中。 您无权访问此应用程序。 授权失败。 请确保用户具有许可证 Azure 活动目录津贴或基本。 | 当试图访问该应用程序，如果他们未显式分配津贴/基本许可证由订阅服务器的管理员发布您的用户可能会发生此错误。 | 转到订阅服务器的 Active Directory**许可证**选项卡并确保津贴或基本许可证分配此用户或用户组。


## <a name="connector-troubleshooting"></a>连接器疑难解答
如果注册失败连接器向导安装过程中，有两种方法查看失败的原因。 通过在**应用程序和服务 Logs\Microsoft\AadApplicationProxy\Connector\Admin**，事件日志中查找或者运行以下 Windows PowerShell 命令。

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| 错误 | 说明 | 解析 |
| --- | --- | --- |
| 连接器注册失败︰ 请确保启用 Azure 管理门户中的应用程序代理，并正确地输入您的 Active Directory 用户名称和密码。 错误: 一个或多个错误出现。 | 您无需执行登录到 Azure 广告关闭注册窗口。 | 再次运行连接器向导并注册该连接器。 |
| 连接器注册失败︰ 请确保启用 Azure 管理门户中的应用程序代理，并正确地输入您的 Active Directory 用户名称和密码。 错误: AADSTS50001︰ 资源`https://proxy.cloudwebappproxy.net /registerapp`处于禁用状态。 | 应用程序代理已禁用。  | 请确保在尝试注册该连接器之前在 Azure 的传统门户网站中启用应用程序代理。 启用应用程序代理的详细信息，请参阅[启用应用程序代理服务](active-directory-application-proxy-enable.md)。 |
| 连接器注册失败︰ 请确保启用 Azure 管理门户中的应用程序代理，并正确地输入您的 Active Directory 用户名称和密码。 错误: 一个或多个错误出现。 | 如果注册窗口将打开，并立即关闭而使你无法以用户身份登录，则可能会出现此错误。 您的系统上的网络错误时，将发生此错误。 | 确保就能通过浏览器连接到公共网站的端口已打开[应用程序代理系统必备组件](active-directory-application-proxy-enable.md)中指定。 |
| 连接器注册失败︰ 请确保您的计算机连接到互联网。 错误: 没有终结点侦听处`https://connector.msappproxy.net :9090/register/RegisterConnector`，可以接受消息。 这通常是由不正确的地址或 SOAP 操作引起的。 如果存在，有关更多详细信息，请参见内部异常，。 | 如果您使用 Azure AD 用户名和密码进行登录，但再收到此错误消息，则可能是 8081 以上的所有端口被都阻止。 | 请确保所需的端口已打开。 有关详细信息，请参阅[应用程序代理的先决条件](active-directory-application-proxy-enable.md)。 |
| 清除错误也会出现在注册窗口。 无法继续操作 – 仅以关闭该窗口。 | 您输入了错误的用户名或密码。 | 再试一次。 |
| 连接器注册失败︰ 请确保启用 Azure 管理门户中的应用程序代理，并正确地输入您的 Active Directory 用户名称和密码。 错误: AADSTS50059︰ 没有租户标识的信息中找到该请求，或者暗示任何提供的凭据和按服务原则 URI 搜索已失败。 | 您正在使用 Microsoft 帐户并不是您试图访问的目录组织 ID 的一部分的域登录。 | 确保管理是租户域与域同名的一部分，例如，如果 contoso.com Azure AD 域，则该管理员应为admin@contoso.com。 |
| 未能检索当前运行 PowerShell 脚本的执行策略。 | 如果连接器安装失败，请进行检查以确保 PowerShell 执行策略未被禁用。 | 打开组策略编辑器。 转到**计算机配置** > **管理模板** > **Windows 组件** > **Windows PowerShell** ，然后双击**打开脚本执行**。 这可以将设置为**未配置**或**已启用**。 如果设置为**启用**，请确保在选项执行策略设置为任一**允许本地脚本和远程签名的脚本**或**允许的所有脚本**。 |
| 连接器的配置下载失败。 | 连接器的客户端证书用于身份验证已过期。 如果您有安装在代理服务器后面的接头，同样会发生此问题。 在这种情况下，连接器无法访问互联网并不能提供给远程用户的应用程序。 | 更新信任使用手动`Register-AppProxyConnector`在 Windows PowerShell 的 cmdlet。 如果您的连接器位于代理之后，则需要授予 Internet 访问连接器帐户"网络服务"和"本地系统"。 这可以通过授予其访问代理服务器或通过设置其绕过代理服务器实现。 |
| 连接器注册失败︰ 请确保您是注册该连接器将 Active Directory 的全局管理员。 错误: 注册请求被拒绝。 | 您正在尝试登录用的别名不是此域的管理员。 您的连接器始终安装拥有用户所在的域的目录中。 | 请确保，管理要有到 Azure AD 租户的全局权限的身份登录。|


## <a name="kerberos-errors"></a>找出 Kerberos 错误

| 错误 | 说明 | 解析 |
| --- | --- | --- |
| 未能检索当前运行 PowerShell 脚本的执行策略。 | 如果连接器安装失败，请进行检查以确保 PowerShell 执行策略未被禁用。 | 打开组策略编辑器。 转到**计算机配置** > **管理模板** > **Windows 组件** > **Windows PowerShell** ，然后双击**打开脚本执行**。 这可以将设置为**未配置**或**已启用**。 如果设置为**启用**，请确保在选项执行策略设置为任一**允许本地脚本和远程签名的脚本**或**允许的所有脚本**。 |
| 12008-azure 广告超出允许向后端服务器的 Kerberos 身份验证尝试的最大数目。 | 此事件可能表明 Azure 的广告之间的配置不正确和后端应用程序服务器上或在这两台计算机的时间和日期配置问题。 | 后端服务器拒绝了 Azure 广告所创建的 Kerberos 票证。 请验证该 Azure 的广告，并正确配置后端应用程序服务器。 请确保在 Azure 广告的时间和日期配置和后端应用程序服务器进行同步。 |
| 13016-因为没有任何 UPN 或访问 cookie 中的边缘标记 azure AD 无法检索代表用户的 Kerberos 票证。 | 没有 STS 配置有问题。 | 在 STS 中修复 UPN 声明配置。 |
| 13019-azure 的广告不能因为以下常规 API 错误检索代表用户的 Kerberos 票证。 | 此事件可能表明 Azure 的广告之间的正确配置和域控制器服务器或在这两台计算机的时间和日期配置问题。 | 域控制器拒绝了 Azure 广告所创建的 Kerberos 票证。 验证该 Azure 的广告和后端应用程序服务器的配置正确，尤其是该 SPN 配置。 请确保 Azure AD 域加入相同的域的域控制器以确保域控制器建立信任与 Azure 的广告。 请确保在 Azure 广告的时间和日期配置和域控制器进行同步。 |
| 13020-因为没有定义后端服务器 SPN azure AD 无法检索代表用户的 Kerberos 票证。 | 此事件可能表明 Azure 的广告之间的正确配置和域控制器服务器或在这两台计算机的时间和日期配置问题。 | 域控制器拒绝了 Azure 广告所创建的 Kerberos 票证。 验证该 Azure 的广告和后端应用程序服务器的配置正确，尤其是该 SPN 配置。 请确保 Azure AD 域加入相同的域的域控制器以确保域控制器建立信任与 Azure 的广告。 请确保在 Azure 广告的时间和日期配置和域控制器进行同步。 |
| 13022-azure 的广告不能验证用户身份，因为后端服务器响应 HTTP 401 错误使用 Kerberos 身份验证尝试。 | 此事件可能表明 Azure 的广告之间的配置不正确和后端应用程序服务器上或在这两台计算机的时间和日期配置问题。 | 后端服务器拒绝了 Azure 广告所创建的 Kerberos 票证。 请验证该 Azure 的广告，并正确配置后端应用程序服务器。 请确保在 Azure 广告的时间和日期配置和后端应用程序服务器进行同步。 |
| 网站无法显示的页面。 | 尝试访问该应用程序，如果应用程序是一个 IWA 应用程序发布时，您的用户可能会出现此错误。 此应用程序的已定义的 SPN 可能不正确。 | 对于 IWA 的应用︰ 确保为此应用程序配置的 SPN 设置正确。 |
| 网站无法显示的页面。 | 尝试访问该应用程序，如果应用程序是一个 OWA 应用程序发布时，您的用户可能会出现此错误。 可能是下列情况之一所致︰ <br> 的此应用程序已定义的 SPN 不正确。 <br> 的试图访问该应用程序用户正在使用 Microsoft 帐户而不是正确的公司帐户进行登录，或来宾用户。 <br> 的试图访问该应用程序用户定义不正确一侧上 prem 此应用程序。 | 减轻相应的步骤︰ <br> -请确保为此应用程序配置的 SPN 设置正确。 <br> -确保用户在使用其公司的帐户相匹配的已发布的应用程序域进行签名。 Microsoft 帐户用户和来宾不能访问 IWA 的应用程序。 <br> -请确保此用户具有适当的权限，如 prem 在机器上该后端应用程序的定义。 |
| 无法访问此企业应用程序中。 您无权访问此应用程序。 授权失败。 请确保将具有访问权限的用户分配给此应用程序。 | 当试图访问该应用程序，如果他们使用 Microsoft 帐户而不是他们公司的帐户进行签名的发布，您的用户可能会出现此错误。 来宾用户还可能会出现此错误。 | Microsoft 帐户用户和来宾不能访问 IWA 的应用程序。 确保用户在使用其公司的帐户相匹配的已发布的应用程序域中签署。 |
| 现在无法访问此企业应用程序中。 请稍后再试...连接器已超时。 | 尝试访问时未正确定义此应用程序上 prem 侧发布应用程序时，您的用户可能会出现此错误。 | 请确保您的用户拥有适当的权限，如 prem 在机器上该后端应用程序的定义。 |


## <a name="see-also"></a>请参见

- [启用 Azure 的活动目录的应用程序代理](active-directory-application-proxy-enable.md)
- [发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
