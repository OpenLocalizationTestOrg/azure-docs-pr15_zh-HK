<properties 
    pageTitle="使用混合连接管理器 |Microsoft Azure" 
    description="安装和配置混合连接管理器并连接到应用程序逻辑中的内部连接器" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>连接使用混合连接管理器的内部接头

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。 逻辑应用程序一般可用性 (GA) 用于内部连接的网关。 阅读有关新[网关](app-service-logic-gateway-connection.md)和[逻辑应用程序正式提供](https://azure.microsoft.com/documentation/services/logic-apps/)详细信息。

若要使用内部部署系统，逻辑应用程序，请使用混合连接管理器。 某些连接器可以连接到内部部署的系统，如 SQL Server、 SAP、 SharePoint，等等。 

混合管理器 (HCM) 是嗒-一次安装在防火墙后面，您的网络内的 IIS 服务器的安装程序。 HCM 使用 Azure 服务总线中继，验证在 Azure 中的连接器的内部系统。 

> [AZURE.NOTE] 仅当您要连接到的内部资源位于防火墙后面，混合连接管理器是必需的。 如果您未连接到内部系统，您不需要混合连接管理器。

若要开始，您需要︰

- Azure 服务总线中继命名空间 SAS 连接字符串。 请参阅[服务总线定价](https://azure.microsoft.com/pricing/details/service-bus/)来确定哪个层包括继电器。
- 内部系统登录的信息，包括用户名称和密码。 例如，如果您要连接到内部部署 SQL Server，您需要的 SQL Server 登录帐户和密码。
- 内部服务器信息，包括端口号和服务器名称。 例如，如果您要连接到内部部署 SQL Server，您需要的 SQL Server 名称和 TCP 端口号。

## <a name="get-the-service-bus-connection-string"></a>获取服务总线连接字符串

在 Azure 的门户中，复制服务总线根 SAS 连接字符串。 此连接字符串连接到内部系统将 Azure 的连接器。 

1. 在[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)中，选择您服务总线的命名空间，并选择**连接信息**:

    ![][SB_ConnectInfo]

2. 将复制的 SAS 连接字符串︰

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>安装混合连接管理器

1. 在[Azure 的门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)中，选择您创建的连接器。 要打开它，可以选择**浏览**，选择**API 的应用程序**，然后选择您的接口或 API 的应用程序。 
<br/><br/>
在**混合连接**中，安装程序**不完整**︰
<br/>
![][2] 

2. 选择**混合连接**。 列出以前输入的服务总线连接字符串。
3. 复制**主配置字符串**︰
<br/>
![][PrimaryConfigString]

4. 在**内部部署混合连接管理器**，可以下载混合连接管理器或直接从门户安装。 
<br/><br/>
若要直接从门户安装，请转到内部部署 IIS 服务器上，浏览到门户，然后选择**下载和配置**。
<br/><br/>
下载混合连接管理器，请转到您的部署的 IIS 服务器，然后转到**ClickOnce 应用程序**(http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application)。 因此，您可以运行该安装将自动启动。

5. 在**侦听器设置**窗口中，输入**主配置字符串**以前粘贴 （步骤 3） 中，选择**安装**。

安装程序完成后，下面的显示︰
<br/>
![][3] 

现在当您再次浏览到该连接器，混合连接状态为**已连接**。 您可能需要关闭连接器，然后重新打开它︰
<br/>
![][4] 

> [AZURE.NOTE] 若要切换到辅助连接字符串，重新运行混合连接安装程序，然后输入**辅助配置字符串**。


## <a name="tcp-ports-and-security"></a>TCP 端口和安全

创建混合连接时，在内部部署本地 IIS 服务器上创建网站。 IIS 服务器可以在 DMZ 中。 在 IIS 服务器上的 TCP 端口要求如下︰

TCP 端口 | 为什么
--- | ---
 | 没有传入的 TCP 端口是必需的。
9350-9354 | 这些端口用于数据传输。 服务总线中继管理器探测端口 9350 TCP 连接可用。 如果可用，它假定端口 9352 也有。 数据流量会通过端口 9352。 <br/><br/>允许出站连接到这些端口。
5671 | 当端口 9352 用于数据通信时，为使用端口 5671 控制通道。 <br/><br/>允许到此端口的出站连接。 
80、 443 | 如果 9352 和 5671 的端口不可用，*然后*端口 80 和 443 是用于数据传输和控制通道的备用端口。<br/><br/>允许出站连接到这些端口。
在 prem 系统端口 | 在本地系统上，打开由系统使用的端口。 例如，SQL Server 通常使用端口 1433年。 打开此 TCP 端口。

[托管服务总线防火墙后面](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>故障排除

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>部署疑难解答

1. 在 IIS 服务器上，请确认安装了 IIS web 角色和所有 IIS 服务都已都启动。
2. 在 IIS 服务器上，确认混合连接管理器已安装并且正在运行︰
 - 在 IIS 管理器 (inetmgr)， ***MicrosoftAzureBizTalkHybridListener***网站应列出和运行。 
 - 该网站使用***HybridListenerAppPool***作为*网络服务*本地内置用户帐户运行。 此外应启动此应用程序池。
3. 在 IIS 服务器上，检查接头已安装并正在运行︰ 
 - 为您的连接器创建网站。 例如，如果您创建的 SQL 接口，是***MicrosoftSqlConnector_nnn***网站。 在 IIS 管理器 (inetmgr)，确认该网站列出并启动。 
 - 该网站使用名为***HybridAppPoolnnn***自己 IIS 应用程序池。 作为*网络服务*本地内置用户帐户运行该应用程序池。 此网站和应用程序池应同时启动。 
 - 浏览本地连接器。 例如，如果您的连接器网站使用端口 6569，浏览到 http://localhost:6569。 未配置默认文档以便`HTTP Error 403.14 - Forbidden error`预计。
4. 在防火墙中，请确认在本主题中列出的 TCP 端口已打开。
5. 查看源系统或目标系统︰
 - 一些内部系统需要附加依赖项文件。 例如，如果您要连接到内部部署 SAP，其他一些 SAP 文件必须安装 IIS 服务器上。
 - 请检查登录帐户的系统连接。 例如，系统使用的 TCP 端口必须打开，像为 SQL Server 端口 1433年。 在 Azure 门户输入的登录帐户必须具有对系统的访问。
6. 在 IIS 服务器上，请检查事件日志中任何错误的。 
7. 清除并重新安装混合连接管理器︰ 
 - 在 IIS 中，手动删除此连接器网站，其应用程序池。 
 - 重新运行混合连接管理器并确认您进入**主配置字符串**正确的连接器。



### <a name="in-the-azure-classic-portal"></a>在 Azure 经典门户

1. 确认服务总线命名空间具有**活动**状态。
2. 创建连接器时，请输入服务总线 SAS 连接字符串。 不要输入 ACS 的连接字符串。


## <a name="faq"></a>常见问题

**问题**︰ 有两个混合连接管理器。 区别是什么？ 

**答案**︰ 没有主要由 Web 应用程序 （以前称为网站） 和移动应用程序 （以前称为移动服务） 连接到内部的[混合连接](../biztalk-services/integration-hybrid-connection-overview.md)技术。 此混合连接管理器是其自己的[安装程序](../biztalk-services/integration-hybrid-connection-create-manage.md)并使用 Azure BizTalk 服务 （在后台）。 它支持 TCP 和 HTTP 协议。

使用 Azure 应用程序服务连接器，我们还必须混合连接管理器。  此混合连接管理器执行*不*使用 Azure 的 BizTalk 服务 （在后台），并支持超过 TCP 和 HTTP 协议。 请参阅[连接器和 API 的应用程序列表](app-service-logic-connectors-list.md)。

两者都使用 Azure 服务总线连接到内部系统。

**问题**︰ 当创建一个自定义的 API 应用程序时，可以使用应用程序服务混合连接管理器连接到内部？ 

**答案**︰ 不在传统意义上。 您可以使用内置连接线，配置应用程序服务混合连接管理器连接到内部系统。 然后，使用您自定义的 API 应用程序，可能会使用逻辑应用程序使用此连接器。 目前，不能制定或创建您自己混合 API 的应用程序 （如 SQL 连接器或文件连接器）。

如果您自定义的 API 使用 TCP 或 HTTP 端口，您可以使用[混合连接](../biztalk-services/integration-hybrid-connection-overview.md)和其混合连接管理器。 在这种情况下，使用 Azure BizTalk 服务。 [连接到一个 web 应用程序从本地 SQL Server](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)可能会有所帮助。  


## <a name="read-more"></a>阅读更多

[监视您的逻辑应用程序](app-service-logic-monitor-your-logic-apps.md)<br/>
[服务总线定价](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
