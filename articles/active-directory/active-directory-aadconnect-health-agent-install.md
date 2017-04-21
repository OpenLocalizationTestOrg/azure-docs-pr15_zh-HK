<properties
    pageTitle="Azure AD 连接健康代理安装 |Microsoft Azure"
    description="这是介绍了 AD FS 和同步代理安装 Azure AD 连接健康页。"
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD 连接健康代理安装

本文档将指导您在安装和配置 Azure AD 连接健康代理。 您可以从[此处](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent)下载代理。

##  <a name="requirements"></a>要求
下表是使用 Azure AD 连接运行状况要求的列表。

| 要求 | 说明|
| ----------- | ---------- |
|Azure AD 津贴| Azure AD 连接的健康是 Azure 广告高级功能，要求 Azure AD 津贴。 </br></br>有关详细信息，请参阅[入门 Azure AD 津贴](active-directory-get-started-premium.md) </br>要启动 30 天免费试用版，请参阅[开始试验。](https://azure.microsoft.com/trial/get-started-active-directory/)|
|您必须是全局管理员的 Azure 广告入门 Azure AD 连接运行状况|默认情况下，只有全局管理员可以安装和配置健康代理开始，访问门户网站，并执行任何操作在 Azure AD 连接的健康。 有关详细信息，请参阅[管理 Azure 的广告目录](active-directory-administer.md)。 <br><br> 使用基于角色的访问控制可以允许组织中的 Azure AD 连接运行状况的其他用户访问。 有关详细信息，请参阅[Azure AD 连接状况的角色基于访问控制。](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**重要︰**在安装代理时使用的帐户必须是一个工作或学校的帐户。 它不能为 Microsoft 帐户。 有关详细信息，请参阅[注册 Azure 作为一个组织](sign-up-organization.md)
|在每个目标服务器上安装了 Azure AD 连接健康代理| Azure AD 连接运行状况要求代理安装在目标服务器上，以提供在门户中查看的数据。 </br></br>例如，若要从您的 AD FS 内部部署基础结构中获取数据，必须安装代理的 AD FS、 AD FS 代理和 Web 应用程序代理服务器上。 同样，若要获得有关您对内部的数据 AD DS 的基础结构，代理程序必须安装在域控制器上。 </br></br>**重要︰**在安装代理时使用的帐户必须是一个工作或学校的帐户。 它不能为 Microsoft 帐户。 有关详细信息，请参阅[注册 Azure 作为一个组织](sign-up-organization.md)|
|出站连接到 Azure 服务终结点|在安装和运行时，代理需要与 Azure AD 连接健康服务终结点的连接。 如果被阻止出站连接，请确保下列终结点被添加到允许列表︰ </br></br><li>& #42;。blob.core.windows.net </li><li>& #42;。queue.core.windows.net</li><li>adhsprodwus.servicebus.windows.net-端口︰ 5671 </li><li>https://management.azure.com </li><li>https://s1.adhybridhealth.azure.com/</li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|运行代理的服务器上的防火墙端口。| 代理需要下列防火墙端口会打开以使代理与 Azure 广告健康服务终结点进行通信。</br></br><li>TCP/UDP 端口 443</li><li>5671 的 TCP/UDP 端口</li>
|如果启用 IE 增强的安全性，则，允许以下网站| 如果启用 IE 增强的安全性，则必须要安装代理的服务器上允许在以下网站上找到。</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>组织信任的 Azure Active Directory 联合身份验证服务器。 例如︰ https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>安装 Azure AD 连接 AD FS 代理运行状况
若要启动代理安装，请双击您下载.exe 文件。 在第一个屏幕上，单击安装。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install1.png)

一旦完成安装，请单击立即配置。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install2.png)

启动命令提示符，跟一些 PowerShell 执行注册 AzureADConnectHealthADFSAgent。 当系统提示您登录到 Azure，继续操作，并在每次登录。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install3.png)


登录后，将继续 PowerShell。 一旦完成，您可以关闭 PowerShell 和配置已完成。

在这种情况下，应启动服务自动允许代理来监视并收集数据。 如果没有满足前面各节中列出的所有先决条件，PowerShell 窗口中会出现警告。 请务必在安装代理之前完成[的要求](active-directory-aadconnect-health-agent-install.md#requirements)。 下面的屏幕快照是这些错误中的一个示例。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install4.png)

若要验证已安装代理，请查找以下服务在服务器上。 如果您完成了配置，它们应该已经运行。 否则，它们会停止直到配置完成。

- Azure AD 连接健康 AD FS 诊断服务
- Azure AD 连接健康 AD FS 见解服务
- Azure AD 连接健康 AD FS 监视服务

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>在 Windows Server 2008 R2 服务器上的代理安装

Windows Server 2008 R2 服务器的步骤︰

1. 请确保该服务器正在运行 Service Pack 1 或更高。
1. 关闭代理安装的 IE esc 键︰
1. 在每个服务器之前安装 AD 健康代理安装 Windows PowerShell 4.0。 若要安装 Windows PowerShell 4.0:
 - 安装[Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779)使用下面的链接下载脱机安装程序。
 - 安装 PowerShell ISE （从 Windows 功能）
 - 安装[Windows 管理框架 4.0。](https://www.microsoft.com/download/details.aspx?id=40855)
 - 安装 Internet Explorer 版本 10 或更高的服务器上。 （所必需的健康服务进行身份验证，使用 Azure Admin 凭据。）
1. 在 Windows Server 2008 R2 上安装 Windows PowerShell 4.0 的详细信息，请参阅 wiki 文章[下面](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx)。

### <a name="enable-auditing-for-ad-fs"></a>启用 AD FS 的审核

> [AZURE.NOTE] 此部分仅适用于 AD FS 联合身份验证服务器。

为了使使用率分析功能来收集和分析数据，Azure AD 连接健康代理需要用 AD FS 审核日志中的信息。 默认情况下未启用这些日志。 启用 AD FS 的审核和 AD FS 服务器上找到 AD FS 审核日志，请使用以下过程。

#### <a name="to-enable-auditing-for-ad-fs-20"></a>若要启用审核的 AD FS 2.0

1. 单击**开始**，指向**程序**，指向**管理工具**，然后单击**本地安全策略**。
2. 导航到**安全设置 \ 本地策略 \ 用户权限管理**文件夹，然后双击生成安全审核。
3. 在**本地安全设置**选项卡上，验证列出了 AD FS 2.0 服务帐户。 如果不存在，单击**添加用户或组**并将其添加到列表中，然后单击**确定**。
4. 若要启用审核，请使用提升的权限打开命令提示符并运行以下命令︰<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. 关闭本地安全策略，然后再打开管理管理单元中。 若要打开管理管理单元中，单击**开始**，指向**程序**，指向**管理工具**，然后单击 AD FS 2.0 管理。
6. 在操作窗格中，单击编辑联合身份验证服务属性。
7. 在**联合身份验证服务属性**对话框中，单击**事件**选项卡。
8. 选中**成功审核**和**失败审核**复选框。
9. 单击**确定**。

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>若要启用 Windows Server 2012 R2 上的 AD FS 的审核

1. 通过在任务栏上桌面，打开**服务器管理器**在启动屏幕上或服务器管理器中打开**本地安全策略**，然后单击**工具 / 本地安全策略**。
2. 导航到**安全设置 \ 本地策略 \ 用户权限分配**文件夹，然后双击**生成安全审核**。
3. 在**本地安全设置**选项卡上，验证列出了 AD FS 服务帐户。 如果不存在，单击**添加用户或组**并将其添加到列表中，然后单击**确定**。
4. 若要启用审核，请使用提升的权限打开命令提示符并运行以下命令︰<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. 关闭**本地安全策略**，然后再打开**AD FS 管理**管理单元中 （在服务器管理器中，单击工具，然后选择 AD FS 管理）。
6. 在操作窗格中，单击**编辑联合身份验证服务属性**。
7. 在联合身份验证服务属性对话框中，单击**事件**选项卡。
8. 选中的**成功审核和失败审核**复选框，然后单击**确定**。






#### <a name="to-locate-the-ad-fs-audit-logs"></a>若要找到 AD FS 审核日志


1. 打开**事件查看器**。
2. 转到 Windows 日志，然后选择**安全**模式。
3. 在右侧，单击**筛选当前日志**。
4. 在事件源下选择**AD FS 审核**。

![AD FS 审核日志](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] 如果存在一个组策略，则禁用审核的 AD FS，Azure AD 连接健康代理就无法收集信息。 确保您没有一个组策略，可能会禁用审计。

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>安装同步的 Azure AD 连接健康代理
在 Azure AD 连接的最新版本同步的 Azure AD 连接健康代理会自动安装。 若要使用 Azure AD 连接的同步，您需要下载最新版本的 Azure AD 连接并将其安装。 您可以下载最新版本[在此处](http://www.microsoft.com/download/details.aspx?id=47594)。

若要验证已安装代理，请查找以下服务在服务器上。 如果您完成了配置，它们应该已经运行。 否则，它们会停止直到配置完成。

- Azure AD 连接健康同步见解服务
- Azure AD 连接的运行状况监视服务的同步

![验证 Azure AD 同步的连接运行状况](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] 请记住，使用 Azure AD 连接运行状况要求 Azure AD 津贴。 如果您没有 Azure AD 津贴，不能完成在 Azure 门户配置。 有关详细信息，请参阅[要求页](active-directory-aadconnect-health-agent-install.md#requirements)。


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>对于同步注册手动 Azure AD 连接运行状况
如果 Azure AD 连接的运行状况同步代理程序注册失败成功安装 Azure AD 连接后，您可以使用以下 PowerShell 命令手动注册代理。

>[AZURE.IMPORTANT] 使用此 PowerShell 命令才需要代理注册安装 Azure AD 连接后如果出现故障。

即使成功地安装和配置的 Azure AD 连接时失败只健康代理注册以下 PowerShell 命令所必需。 代理已成功注册后，将启动 Azure AD 连接的健康服务。

您可以手动注册 Azure AD 连接健康代理使用以下 PowerShell 命令同步︰

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

这个命令需要下列参数︰

- AttributeFiltering: $true （默认）-如果 Azure AD 连接不同步的默认属性设置和自定义用于筛选的属性集。 $false 以其他方式。
- StagingMode: $false （默认）-如果 Azure AD 连接的服务器不是在调试模式下，$true，如果服务器被配置为可在调试模式。

当系统提示您输入身份验证时应使用相同的全局管理员帐户 (如admin@domain.onmicrosoft.com)，用于配置 Azure AD 连接。

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>安装 Azure AD 连接为 AD DS 的健康代理
若要启动代理安装，请双击您下载.exe 文件。 在第一个屏幕上，单击安装。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

一旦完成安装，请单击立即配置。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

启动命令提示符，跟一些 PowerShell 执行注册 AzureADConnectHealthADDSAgent。 当系统提示您登录到 Azure，继续操作，并在每次登录。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

登录后，将继续 PowerShell。 一旦完成，您可以关闭 PowerShell 和配置已完成。

在这种情况下，应启动服务自动允许代理来监视并收集数据。 如果没有满足前面各节中列出的所有先决条件，PowerShell 窗口中会出现警告。 请务必在安装代理之前完成[的要求](active-directory-aadconnect-health-agent-install.md#requirements)。 下面的屏幕快照是这些错误中的一个示例。

![验证 Azure AD 连接为 AD DS 的健康](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

要验证已安装代理，请查找以下服务在域控制器上。

- Azure AD 连接健康 AD DS 的见解服务
- Azure AD 连接健康 AD DS 监视服务

如果您完成了配置，应该已经在运行这些服务。 否则，它们会停止直到配置完成。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>安装 Azure AD 连接上服务器核心的 AD DS 的健康代理。
安装后的.exe 文件，可以使用下面的 PowerShell 命令完成注册过程︰

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>配置 Azure AD 连接健康代理程序以使用 HTTP 代理服务器
您可以配置 Azure AD 连接健康代理要使用 HTTP 代理服务器。

>[AZURE.NOTE]
- 不支持使用"Netsh WinHttp 设置 ProxyServerAddress"，因为该代理使用 System.Net 发出 web 请求，而不是 Microsoft Windows HTTP 服务。
- 通过加密的 Https 消息用于配置的 Http 代理服务器地址。
- 不支持已通过身份验证的代理服务器 （使用 HTTPBasic）。

### <a name="change-health-agent-proxy-configuration"></a>健康代理代理服务器配置的更改
有以下选项可 Azure AD 连接状况将代理配置为使用 HTTP 代理服务器。

>[AZURE.NOTE]所有的 Azure AD 连接健康代理服务必须重新启动，以便进行更新的代理服务器设置。 运行以下命令︰<br>
请重新启动服务 AdHealth *

#### <a name="import-existing-proxy-settings"></a>导入现有的代理服务器设置

##### <a name="import-from-internet-explorer"></a>从 Internet Explorer 导入
Internet Explorer HTTP 代理设置可以导入，将由 Azure AD 连接健康代理。 在每个运行健康代理的服务器上执行以下 PowerShell 命令︰

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>WinHTTP 从导入
WinHTTP 代理设置可以导入，将由 Azure AD 连接健康代理。 在每个运行健康代理的服务器上执行以下 PowerShell 命令︰

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>手动指定代理服务器地址
在每个运行健康代理中，通过执行以下 PowerShell 命令的服务器上，可以手动指定一个代理服务器︰

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

例如︰*组 AzureAdConnectHealthProxySettings HttpsProxyAddress myproxyserver: 443*

- "地址"可以是 DNS 解析服务器名或一个 IPv4 地址
- "端口"，则可以省略。 如果省略则选择默认端口为 443。

#### <a name="clear-existing-proxy-configuration"></a>清除现有代理配置
您可以通过运行以下命令清除现有的代理服务器配置︰

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>读取当前的代理服务器设置
可以通过运行下面的命令来读取当前配置代理服务器设置︰

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>测试连接到 Azure AD 连接健康服务
很可能，可能会出现问题，使 Azure AD 连接健康代理会失去与 Azure AD 连接的健康服务。 其中包括网络问题、 权限有问题或其他的各种原因。

如果代理不能将数据发送到 Azure AD 连接运行状况服务时间超过两个小时，它指示在门户中下面的警告:"健康服务数据不是最新的。" 您可以确认受影响的 Azure AD 连接健康代理是否能够将数据上载到 Azure AD 连接的健康服务，通过运行以下 PowerShell 命令︰

    Test-AzureADConnectHealthConnectivity -Role ADFS

目前，角色参数采用下列值︰

- ADFS
- 同步
- 添加

可以在命令中使用-ShowResults 标志以查看详细的日志。 使用以下示例︰

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]使用连接工具，您必须首先完成代理注册。 如果您不能完成代理注册，请确保已满足 Azure AD 连接运行状况[要求](active-directory-aadconnect-health-agent-install.md#requirements)。 此连接测试默认执行代理注册过程。



## <a name="related-links"></a>相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [对于同步使用 Azure AD 连接运行状况](active-directory-aadconnect-health-sync.md)
* [与 AD DS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adds.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
