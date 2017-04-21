<properties
    pageTitle="单一登录应用程序代理使用 |Microsoft Azure"
    description="介绍如何提供单一登录使用 Azure AD 应用程序代理服务器。"
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>单一登录和应用程序代理

单一登录是 Azure AD 应用程序代理的一个关键要素。 它为最佳的用户体验提供了以下步骤︰

1. 用户签入云  
2. 所有的安全验证发生在云中 （预身份验证）  
3. 当请求发送到在 prem 应用程序时，应用程序代理服务器连接器模拟的用户。 后端应用程序将认为这是常规用户来自域内的设备。

![从最终用户，通过应用程序代理到公司网络的访问关系图](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure AD 应用程序代理服务器可帮助您为您的用户提供单一登录 (SSO) 的体验。 请按照以下说明来发布使用 SSO 应用程序︰


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>在 prem IWA 应用程序与应用程序代理使用 KCD SSO
您可以启用单一登录应用程序使用集成 Windows 身份验证 (IWA) 通过在 Active Directory 中为应用程序代理服务器连接器权限来模拟用户，并发送和接收标记代表他们。


### <a name="network-diagram"></a>在网络图中

当用户尝试访问使用 IWA prem 上应用程序时，此关系图解释流。

![Microsoft AAD 身份验证流程关系图](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. 用户输入的 URL 来访问 prem 上应用程序到应用程序代理。
2. 应用程序代理将请求重定向到 Azure AD 身份验证服务，以对。 此时，Azure AD 应用任何可用的身份验证和授权策略，例如多因素身份验证。 如果用户通过验证，Azure 广告创建标记并将其发送给用户。
3. 用户将该标记传递给应用程序代理。
4. 应用程序代理服务器验证该令牌并检索用户主体名称 (UPN)，然后通过双重身份验证的安全通道的接口发送请求、 UPN 和服务主体名称 (SPN)。
5. 该连接器执行上 prem 与 Kerberos 约束委派 (KCD) 协商广告，模拟用户以获取 Kerberos 令牌给应用程序。
6. 活动目录发送到连接器应用程序的 Kerberos 令牌。
7. 连接器原始请求发送到应用程序服务器上，使用 Kerberos 令牌来自广告。
8. 应用程序将响应发送到连接器，然后返回到该应用程序代理服务并最后给用户。

### <a name="prerequisites"></a>系统必备组件

您开始使用 SSO 应用程序代理服务器之前，请确保您的环境已准备好与下面的设置和配置︰

- 您的应用程序，如 SharePoint Web 应用程序中，都设置为使用集成 Windows 身份验证。 有关详细信息，请参阅[启用 Kerberos 身份验证的支持](https://technet.microsoft.com/library/dd759186.aspx)，或 SharePoint 请参阅[SharePoint 2013 在 Kerberos 身份验证计划](https://technet.microsoft.com/library/ee806870.aspx)。

- 所有应用程序的服务主体名称。

- 运行连接器的服务器，并运行该应用程序的服务器是加入域和相同的域或信任域的一部分。 在加入域的详细信息，请参阅[连接到域的计算机](https://technet.microsoft.com/library/dd807102.aspx)。

- 运行连接器的服务器有权读取 TokenGroupsGlobalAndUniversal 的用户。 这是默认设置，可能会影响通过加强环境安全。 获取与此[KB2009157](https://support.microsoft.com/en-us/kb/2009157)中的更多帮助。

### <a name="active-directory-configuration"></a>活动目录配置

Active Directory 配置各不相同，具体取决于您的应用程序代理服务器连接器和已发布的服务器是否位于同一域中。

#### <a name="connector-and-published-server-in-the-same-domain"></a>连接器和发布的服务器位于同一域中

1. 在活动目录中，请转到**工具** > **的用户和计算机**。
2. 选择运行连接器的服务器。
3. 用鼠标右键单击并选择**属性** > **委派**。
4. 选择**信任此计算机来委派指定的服务**并在**到此帐户可以提出委派的凭据的服务**，将添加 SPN 标识的应用程序服务器的值。
5. 这使应用程序代理服务器连接器列表中定义的应用程序针对 AD 中模拟用户。

![连接器 SVR 属性窗口屏幕快照](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>连接器和发布不同域中的服务器

1. 有关先决条件 KCD 使用跨域的列表，请参阅[跨域 Kerberos 约束委派](https://technet.microsoft.com/library/hh831477.aspx)。
2. 在 Windows 2012 R2，使用`principalsallowedtodelegateto`连接器服务器以启用发布的服务器所在的连接器服务器委派应用程序代理上的属性`sharepointserviceaccount`和代理服务器是`connectormachineaccount`。

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`可以是 SP 机器帐户或服务帐户运行 SPS 应用程序池。


### <a name="azure-classic-portal-configuration"></a>Azure 经典的入口配置

1. 发布您的应用程序，根据所述[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)的说明。 请确保选择**Azure Active Directory**作为**预身份验证方法**。
2. 您的应用程序出现在应用程序列表后，选择它并单击**配置**。
3. 在**属性**，将**内部身份验证方法**设置为**集成 Windows 身份验证**。  
  ![高级应用程序配置](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. 输入应用程序服务器的**内部应用程序 SPN** 。 在此示例中，我们已发布的应用程序中的 SPN 是 http/lob.contoso.com。  

>[AZURE.IMPORTANT] 如果本地 UPN 和 Azure Active Directory 中的 UPN 不相同，需要配置[委派登录标识](#delegated-login-identity)预身份验证工作。

|  |  |
| --- | --- |
| 内部身份验证方法 | 如果您使用预身份验证的 Azure 的广告，可以设置一个内部身份验证方法，使用户能够从单一登录 (SSO) 受益于此应用程序。 <br><br> 如果您的应用程序使用 IWA 并可以配置 Kerberos 约束委派 (KCD) 以启用此应用程序的 SSO，请选中**集成 Windows 身份验证**(IWA)。 必须使用 KCD 配置使用 IWA 的应用程序，否则应用程序代理服务器将不能将这些应用程序发布。 <br><br> 如果您的应用程序不使用 IWA，选择**None** 。 |
| 内部应用程序 SPN | 这是内部应用程序的服务主体名称 (SPN)，prem 在 Azure 广告中进行配置。 SPN 是应用程序代理服务器连接器用于读取应用程序使用 KCD Kerberos 令牌。 |


## <a name="sso-for-non-windows-apps"></a>对于非 Windows 应用程序的 SSO
Azure 广告对云中的用户进行身份验证时，将启动 Azure AD 应用程序代理中的 Kerberos 委派流。 一旦在请求到达内部，Azure 的广告应用程序代理服务器连接器会发出通过与本地 Active Directory 交互代表用户的 Kerberos 票证。 这一过程被称为作为 Kerberos 约束委派 (KCD)。 在下一阶段，请求将发送到后端应用程序使用此 Kerberos 票证。 有多个定义如何发送此类请求的协议。 大多数非 Windows 服务器预期在 Azure AD 应用程序代理服务器现在支持的协商/可。

![非 Windows SSO 图](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>委派的登录身份
委派的登录标识可以帮助您处理两个不同的登录方案︰

- 非 Windows 应用程序通常在窗体中的用户名或 SAM 帐户名称，不是电子邮件地址的用户标识，获取(username@domain)。
- 替代登录配置在 Azure AD 中的 UPN 与您的内部部署 Active Directory 中的 UPN 不同。

使用应用程序代理，您可以选择哪个标识用于获取 Kerberos 票证。 此设置是每个应用程序。 这些选项都适用于不接受电子邮件地址格式的系统，其他旨在替代登录。

![委派的登录标识参数屏幕抓图](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

如果使用委派的登录标识，则该值可能不是唯一的所有域或您的组织中的目录林。 您可以通过发布两次使用两个不同的连接器组这些应用程序来避免此问题。 因为每个应用程序都有不同的用户群体，可以到不同的域加入其连接器。


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>使用 SSO 时内部和云的身份并不完全相同
除非另外配置，否则应用程序代理假定用户已在云和内部相同的标识。 您可以配置，对于每个应用程序，执行单一登录时应使用的标识。  

此功能允许具有不同 prem 上和云的身份让 SSO 从云上 prem 应用程序而无需用户输入不同的用户名和密码的许多组织。 这包括组织的︰

- 内部有多个域(joe@us.contoso.com,joe@eu.contoso.com)和单个域中云(joe@contoso.com)。

- 内部有非可路由的域的名称(joe@contoso.usa)和一个法律在云中。

- 内部不使用域名 （李先生）

- 使用不同别名 prem 并在云中。 例如 joe-johns@contoso.comvs。joej@contoso.com  

它还会帮助应用程序不接受形式的电子邮件地址，这是很常见的情况，对于非 Windows 后端服务器的地址。


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>对于不同的云和 prem 上标识设置 SSO

1. 因此主标识将 （邮件） 的电子邮件地址，则配置 Azure AD 连接设置。 这是自定义过程中，通过更改同步设置中的**用户主体名称**字段。 请注意，这些设置还决定如何在用户登录 Office365，Windows10 设备和其他应用程序作为其标识存储使用 Azure 的广告。  
  ![标识用户屏幕抓图的用户主体名称下拉列表](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. 在应用程序的应用程序配置设置中，您想要修改，请选择要使用的**委派登录身份**︰
  - 用户主体名称︰joe@contoso.com  
  - 可选的用户主体名称︰joed@contoso.local  
  - 用户名的用户主体名称的一部分︰ 李先生  
  - 用户名部分备用的用户主体名称︰ joed  
  - 本地 SAM 帐户名称︰ 具体上 prem 域控制器配置

  ![委派的登录标识下拉菜单中的屏幕快照](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>不同身份的 SSO 疑难解答
如果 SSO 过程中的错误将似乎连接器计算机事件日志中[疑难解答](active-directory-application-proxy-troubleshoot.md)中所述。
但是，在某些情况下，请求将成功地发送到后端应用程序时此应用程序将回复各种其他 HTTP 响应中。 解决这种情况下应通过检查连接器上的应用程序代理会话事件日志中的事件编号 24029 开始。 事件详细信息中的"用户"字段中将显示用于委派的用户标识。 要打开会话日志，请选择**显示分析日志和调试日志**的事件查看器查看菜单中。


## <a name="see-also"></a>请参见

- [发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)
- [解决您在具有应用程序代理时遇到的问题](active-directory-application-proxy-troubleshoot.md)
- [使用声明感知应用程序](active-directory-application-proxy-claims-aware-apps.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
