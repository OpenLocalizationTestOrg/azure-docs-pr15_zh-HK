<properties
    pageTitle="混合连接概述 |Microsoft Azure"
    description="了解如何混合连接、 安全、 TCP 端口和支持的配置。 MABS，WABS。"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>混合连接概述
介绍混合连接列出了受支持的配置，并列出所需的 TCP 端口。


## <a name="what-is-a-hybrid-connection"></a>什么是混合连接

混合连接是 Azure BizTalk 服务的功能。 混合连接提供一种简单又方便的方法来 Azure 应用程序服务 （以前称为网站） 中的 Web 应用程序功能和 Azure 应用程序服务 （以前称为移动服务） 中的移动应用程序功能连接到防火墙后面的内部资源。

![混合连接][HCImage]

混合连接的好处包括︰

- Web 应用程序和移动应用程序可以访问现有的内部数据和服务安全。
- 多个 Web 应用程序和移动应用程序可以共享一个混合连接以访问本地资源。
- 访问网络所需最少的 TCP 端口。
- 使用混合连接的应用程序通过混合连接访问发布的特定内部部署资源。
- 可以连接到任何内部资源，它使用一个静态的 TCP 端口，如 SQL Server、 MySQL，HTTP Web Api 和大多数自定义的 Web 服务。

    > [AZURE.NOTE] 当前不支持使用动态端口 （如被动模式的 FTP 或扩展被动模式） 的基于 TCP 的服务。 也不支持 LDAP。 LDAP 使用静态 TCP 端口，但它也可能会使用 UDP。 因此，不支持它。

- 可用于所有支持的 Web 应用程序 （.NET、 PHP、 Java、 Python，Node.js） 和移动应用程序 （.NET Node.js） 的框架。
- Web 应用程序和移动应用程序可以访问内部资源完全相同的方式就像 Web 或移动应用程序位于您的本地网络上。 例如，还可以在 Azure 上使用同一连接字符串使用内部。


混合连接还提供企业管理员与控制以及混合应用程序，包括访问公司资源的可见性︰

- 使用组策略设置，管理员可以允许网络上的混合连接并指定的混合应用程序可以访问的资源。
- 在企业网络上的事件和审核日志提供可见性的混合连接访问资源。


## <a name="example-scenarios"></a>示例方案

混合连接支持下列框架和应用程序组合︰

- 对 SQL Server 的.NET 框架访问
- .NET 框架访问 HTTP/HTTPS WebClient 服务
- PHP 访问 SQL Server，MySQL
- Java 访问 SQL Server，MySQL 和 Oracle
- Java 访问 HTTP/HTTPS 服务

当使用混合连接来访问内部部署 SQL Server，考虑以下方面︰

- SQL 直通命名实例必须配置为使用静态端口。 默认情况下，SQL Express 命名实例使用动态端口。
- SQL 直通默认实例都使用静态端口，但必须启用 TCP。 默认情况下，不启用 TCP。
- 当使用群集或可用性组`MultiSubnetFailover=true`模式目前不支持。
- `ApplicationIntent=ReadOnly`当前不受支持。
- 作为 Azure 应用程序和本地 SQL 服务器支持的端到端授权方法可能需要使用 SQL 身份验证。


## <a name="security-and-ports"></a>安全和端口

混合连接使用共享访问签名 (SAS) 授权保护混合连接从 Azure 应用程序和内部混合连接管理器的连接。 为应用程序和内部混合连接管理器创建单独的连接键。 这些连接键可以转存和独立地吊销。

混合连接提供无缝、 安全分发应用程序和内部混合连接管理器的关键。

请参阅[创建和管理混合连接](integration-hybrid-connection-create-manage.md)。

*应用程序授权是分开混合连接*。 可以使用任何适当的授权方法。 授权方法取决于在 Azure 云和内部组件支持的端到端授权方法。 例如，Azure 应用程序访问本地 SQL Server。 在这种情况下，SQL 的授权可能会被授权方法支持的端到端。

#### <a name="tcp-ports"></a>TCP 端口
混合的连接要求仅出站 TCP 或 HTTP 连接您的专用网络。 您不需要打开任何防火墙端口或更改您的网络外围配置为允许所有入站的连接到您的网络。

通过混合连接使用的以下 TCP 端口︰

端口 | 为什么需要它
--- | ---
9350-9354 | 这些端口用于数据传输。 服务总线中继管理器探测端口 9350 TCP 连接可用。 如果可用，它假定端口 9352 也有。 数据流量会通过端口 9352。 <br/><br/>允许出站连接到这些端口。
5671 | 当 9352 端口用于数据通信时，则端口 5671 用作控制通道。 <br/><br/>允许到此端口的出站连接。
80、 443 | 这些端口用于某些数据请求到 Azure。 此外，如果 9352 和 5671 的端口不可用，*然后*端口 80 和 443 是用于数据传输和控制通道的备用端口。<br/><br/>允许出站连接到这些端口。 <br/><br/>**请注意**不建议使用这些备用端口的其他 TCP 端口的位置。 HTTP/WebSocket 用作数据通道而不是本机的 TCP 协议。 它可能会导致较低的性能。



## <a name="next-steps"></a>下一步行动

[创建和管理混合连接](integration-hybrid-connection-create-manage.md)<br/>
[将 Azure 的 Web 应用程序连接到内部资源](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[从 Azure 的 web 应用程序连接到 SQL Server 内部](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>请参见

[REST API，用于管理在 Microsoft Azure 上的 BizTalk 服务](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk 服务︰ 版本的图表](biztalk-editions-feature-chart.md)<br/>
[创建使用 Azure 门户的 BizTalk 服务](biztalk-provision-services.md)<br/>
[BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
