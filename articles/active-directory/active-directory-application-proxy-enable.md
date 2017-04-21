<properties
    pageTitle="启用 AD Azure 应用程序代理服务器 |Microsoft Azure"
    description="在 Azure 的传统门户网站，打开应用程序代理，为反向代理服务器安装连接器。"
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
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>在 Azure 门户中启用应用程序代理

这篇文章将引导您完成在 Azure AD 中启用 Microsoft Azure 云目录广告应用程序代理的步骤。

如果您熟悉哪些应用程序代理可以帮助您了解[如何提供安全地远程访问部署应用程序](active-directory-application-proxy-get-started.md)的更多信息。

## <a name="application-proxy-prerequisites"></a>应用程序代理系统必备组件
您可以启用和使用应用程序代理服务器服务之前，您需要︰

- [Microsoft Azure 广告基本或特优订阅](active-directory-editions.md)和 Azure 的广告目录作为全局管理员。
- 运行 Windows Server 2012 R2 或 Windows 8.1 或更高版本的服务器，在其上可以安装应用程序代理服务器连接器。 该服务器将请求发送到云中的应用程序代理服务，它需要与您要发布的应用程序的 HTTP 或 HTTPS 连接。

    - 单一登录到您已发布的应用程序，这台机器应该会加入域的将要发布的应用程序相同的 AD 域中。

- 如果路径中有防火墙，请确保它已打开以便连接器能够 HTTPS (TCP) 请求应用程序代理。 路由组连接器使用这些端口与子域的高级域 msappproxy.net 和 servicebus.windows.net 的一部分。 请确保打开下列端口的**出站**通信︰

  	| 端口号 | 说明 |
  	| --- | --- |
  	| 80 | 启用出站 HTTP 通信的安全验证。 |
  	| 443 | 启用用户身份验证 Azure 广告 （仅对连接器注册过程必需的） |
  	| 10100 — 10120 | 启用的 LOB HTTP 响应发送回代理 |
  	| 9352、 5671 | 启用传入请求的 Azure 服务向连接器之间的通信。 |
  	| 9350 | 可选的以便更好的性能，为传入的请求 |
  	| 8080 | 启用连接器引导自陷和接头的自动更新 |
  	| 9090 | 启用连接器注册 （仅对连接器注册过程必需的） |
  	| 9091 | 启用连接器信任证书自动续订 |

    如果您的防火墙会强制执行根据来自用户的通信，打开这些端口通信来自 Windows 服务，作为网络服务运行。 另外，请确保为 NT Authority\System 启用端口 8080。

- 如果贵组织使用代理服务器来连接到 internet，请看一看的博客张贴内容[使用现有内部部署代理服务器](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/)的详细信息，如何对它们进行配置。

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>步骤 1︰ 启用在 Azure AD 的应用程序代理
1. 在[Azure 经典门户](https://manage.windowsazure.com/)管理员身份登录。
2. 转到活动目录并选择要在其中启用应用程序代理的目录。

    ![Active Directory 的图标](./media/active-directory-application-proxy-enable/ad_icon.png)

3. 从目录页中，选择**配置**，向下滚动到**应用程序代理**。
4. 切换到**启用****启用此目录的应用程序代理服务**。

    ![启用应用程序代理](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. 选择**立即下载**。 这将您带到**Azure AD 应用程序代理服务器连接器下载**。 阅读并接受许可条款和单击**下载**以保存该连接器的 Windows 安装程序文件 (.exe)。

## <a name="step-2-install-and-register-the-connector"></a>步骤 2︰ 安装和注册该连接器
1. 您准备好根据系统必备组件的服务器上运行**AADApplicationProxyConnectorInstaller.exe** 。
2. 按照向导中的说明进行安装。
3. 在安装期间，您将会提示您注册该应用程序代理的 Azure AD 租户连接器。

  - 提供了 Azure AD 全局管理员凭据。 全局管理员租户可能不同于您的 Microsoft Azure 凭据。
  - 请确保注册连接器管理员启用应用程序代理服务位置位于同一目录中。 例如，如果 contoso.com 租户域，则该管理员应为admin@contoso.com或域上的任何其他别名。
  - 如果**IE 增强的安全配置**设置为**在**服务器上要安装连接器，则可能会阻止注册屏幕。 按照以允许访问该错误消息中的说明进行操作。 请确保 Internet Explorer 增强的安全性已关闭。
  - 如果连接器注册不成功，请参阅[故障诊断应用程序代理](active-directory-application-proxy-troubleshoot.md)。  

4. 安装完成后，两个新的服务添加到您的服务器︰

    - **Microsoft AAD 应用程序代理服务器连接器**可以连接
    - **Microsoft AAD 应用程序代理服务器连接器更新**是自动的更新服务，它定期检查连接器的新版本，并根据需要更新该连接器。

    ![应用程序代理服务器连接器服务-屏幕抓图](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. 单击安装窗口中**完成**。

为了获得高可用性，应部署至少两个连接器。 若要部署多个连接器，请重复上面的步骤 2 和 3。 每个连接器都必须单独注册。

如果您想要卸载该连接器，卸载连接器服务和更新服务。 重新启动计算机才能完全删除该服务。


## <a name="next-steps"></a>下一步行动

现在您可以为[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)。

如果应用程序位于不同的网络或不同的位置，您可以使用连接器组将不同的连接器组织到逻辑单元。 了解有关[使用应用程序代理连接器](active-directory-application-proxy-connectors.md)的详细信息。
