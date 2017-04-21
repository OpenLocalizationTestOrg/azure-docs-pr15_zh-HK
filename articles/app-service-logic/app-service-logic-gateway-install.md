<properties
   pageTitle="逻辑应用程序安装内部数据网关 |Microsoft Azure"
   description="有关如何安装逻辑应用程序中使用的内部数据网关信息。"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>为逻辑应用程序安装的内部数据网关

## <a name="installation-and-configuration"></a>安装和配置

### <a name="prerequisites"></a>系统必备组件

最小值︰

* .NET 4.5 框架
* 64 位版本的 Windows 7 或 Windows Server 2008 R2 （或更高版本）

建议︰

* 8 核心 CPU
* 8 GB 内存
* 64 位版本的 Windows 2012 R2 （或更高版本）

相关的注意事项︰

* 无法在域控制器上安装网关。
* 您不应安装网关的计算机上，这样的便携式计算机，可能会关闭、 睡眠，或者没有连接到互联网，因为网关不能在任何这些情况下运行。 此外，网关性能可能会降低通过无线网络。

### <a name="install-a-gateway"></a>安装网关

您可以[安装程序的内部数据关](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)。

指定**内部数据网关**与模式，登录到您的工作或学校科目，然后配置一个新的网关或迁移、 还原，或接管现有的网关。

* 要配置网关，请键入一个**名称**并**恢复密钥**，然后单击或点击**配置**。

    指定恢复密钥包含至少 8 个字符，并将其保存在安全的地方。 如果您要迁移、 还原或接管其网关，您将需要此项。

* 若要迁移、 还原或接管现有的网关，提供网关创建时指定的恢复密钥。

### <a name="restart-the-gateway"></a>重新启动该网关

网关作为 Windows 服务运行，并与任何其他 Windows 服务，可以启动和停止它在多个方面。 例如，可以使用提升的权限运行网关的计算机上打开一个命令提示符，然后运行下列命令之一︰

* 若要停止该服务，请运行以下命令︰

    `net stop PBIEgwService`

* 要启动服务，请运行以下命令︰

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>配置防火墙或代理服务器

有关如何提供网关代理信息，请参阅[配置代理服务器设置](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/)。

您可以验证是否您的防火墙或代理服务器上，可能会阻止连接 PowerShell 提示符下运行以下命令。 这将测试到 Azure 服务总线连接。 这只测试网络连接并没有任何与云服务器服务或网关。 它可帮助确定是否您计算机实际上可以到互联网。

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

结果应类似于以下示例。 如果**TcpTestSucceeded**不是这样，防火墙可能会阻止您。

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

如果您想要彻底，用在本主题后面列出在[配置端口](#configure-ports)下替代**计算机名**和**端口**值。

防火墙可能也会阻止 Azure 服务总线对 Azure 数据中心的连接。 如果是这种情况，您都需要到白名单 （取消阻止） 所有的这些数据中心区域的 IP 地址。 您可以[在此处 Azure IP 地址](https://www.microsoft.com/download/details.aspx?id=41653)的列表。

### <a name="configure-ports"></a>配置端口

网关创建到 Azure 服务总线出站连接。 出站端口上进行通信︰ TCP 443 （默认），5671，5672，至 9354 9350。 网关不需要入站的端口。

了解更多关于[混合解决方案](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)。

| 域名称 | 出站端口 | 说明 |
| ----- | ------ | ------ |
| *。 analysis.windows.net | 443 | HTTPS |
| *。 login.windows.net | 443 | HTTPS |
| *。 servicebus.windows.net |5671 5672 | 高级的消息队列协议 (AMQP) |
| *。 servicebus.windows.net | 443，9350 9354 | 侦听器上通过 TCP （访问控制标记购置需要 443） 服务总线中继 |
| *。 frontend.clouddatahub.net | 443 | HTTPS |
| *。 core.windows.net | 443 | HTTPS |
| login.microsoftonline.com | 443 | HTTPS |
| *。 msftncsi.com | 443 | 用于测试互联网连接，如果网关电源 BI 服务不可访问。 |

如果您需要为白名单而不是域的 IP 地址，您可以下载并使用[Microsoft Azure 数据中心 IP 范围列表](https://www.microsoft.com/download/details.aspx?id=41653)。 在某些情况下，Azure 服务总线进行连接 IP 地址而不是完全限定的域名。

### <a name="sign-in-account"></a>登录帐户

用户将登录某一工作或学校的帐户。 这是您的组织的帐户。 如果您注册 Office 365 提供，并不能提供您实际工作的电子邮件，它可能类似于jeff@contoso.onmicrosoft.com。 您的帐户内云服务，, 将存储在一个租户在 Azure 活动目录 (AAD)。 在大多数情况下，AAD 帐户的 UPN 将匹配的电子邮件地址。

### <a name="windows-service-account"></a>Windows 服务帐户

内部数据网关配置为使用 NT SERVICE\PBIEgwService 的 Windows 服务的登录凭据。 默认情况下，它对日志的权利作为一项服务。 这是正在其安装网关计算机的上下文中。

这不是用来连接到内部数据源或与您登录至云服务的工作或学校帐户的帐户。

##<a name="frequently-asked-questions"></a>常见问题及的解答

### <a name="general"></a>常规

**问题**︰ 网关支持哪种数据源？<br/>
**答案**︰ 本书截稿时，SQL Server。

**问题**︰ 我需要一个网关的云，如 SQL Azure 中的数据源？ <br/>
**答案**︰ 否。 网关连接到内部部署数据源。

**问题**︰ 什么实际调用 Windows 服务？<br/>
**答案**︰ 在服务网关称为电源 BI 企业网关服务。

**问题**︰ 是否有任何入站的连接到网关从云？ <br/>
**答案**︰ 否。 网关将使用到 Azure 服务总线出站连接。

**问题**︰ 我阻止出站连接的怎么办？ 打开需要什么？ <br/>
**答案**︰ 请参阅端口和主机的网关使用。


**问题**︰ 不该网关必须在数据源的同一台计算机上安装？ <br/>
**答案**︰ 否。 网关会将连接到数据源，使用提供的连接信息。 网关则可被看作是在这一点上的客户端应用程序。 它只需要能够连接到提供的服务器名称。


**问题**︰ 什么是用于运行到数据源的查询，从网关的反应？ 最佳的体系结构是什么？ <br/>
**答案**︰ 为了减少网络延迟，尽可能靠近数据源尽可能安装网关。 如果在实际的数据源，您可以安装网关，将最大限度地引入了延迟。 可以考虑的数据中心。 例如，如果您的服务正在使用西美国数据中心，具有承载 Azure VM 中的 SQL Server 需要也有 Azure VM 中西部美国。 这会最小化滞后时间并避免在 Azure VM 的出口费用。


**问题**︰ 是否有任何对网络带宽的要求？ <br/>
**答案**︰ 建议要有很好的吞吐量，为您的网络连接。 每个环境都是不同的并正在发送的数据量会影响结果。 使用 ExpressRoute 可以帮助保证内部和 Azure 数据中心之间的吞吐量级别。

可以使用第三方工具 Azure 速度测试应用程序以帮助衡量吞吐量是什么。


**问题**︰ 可以使用 Azure Active Directory 帐户运行 Windows 服务的网关？ <br/>
**答案**︰ 否。 Windows 服务必须具有有效的 Windows 帐户。 默认情况下，它将运行使用服务 SID，NT SERVICE\PBIEgwService。


**问题**︰ 如何将结果发送回云？ <br/>
**答案**︰ 这是通过 Azure 服务总线。 有关详细信息，请参阅它的工作原理。


**问题**︰ 我的凭据的存储位置？ <br/>
**答案**︰ 输入数据源的凭据存储加密网关云服务中。 在内部部署网关，凭据被解密。

### <a name="high-availabilitydisaster-recovery"></a>高可用性/灾难恢复

**问题**︰ 是否有任何计划使高可用性方案中的与网关？ <br/>
**答案**︰ 这是在规划之中，但我们没有时间线。


**问题**︰ 哪种选项都可用于灾难恢复？ <br/>
**答案**︰ 恢复密钥可用于恢复或移动网关。 安装网关时，指定的恢复密钥。


**问题**︰ 恢复密钥的好处是什么？ <br/>
**答案**︰ 它提供一种方法迁移或灾难后恢复您的网关设置。

### <a name="troubleshooting"></a>故障排除

**问题**︰ 网关日志在哪里？ <br/>
**答案**︰ 请参见本主题后面部分中的工具。


**问题**︰ 如何查看查询将被发送到内部数据源？ <br/>
**答案**︰ 您可以启用查询跟踪，其中包括正在发送的查询。 请记住要将其更改回原始值时进行故障排除。 使查询启用跟踪将导致日志变大。

您还可以查看数据源有跟踪查询的工具。 例如，您可以使用扩展事件或 SQL 事件探查器为 SQL Server 和 Analysis Services。

## <a name="how-the-gateway-works"></a>网关的工作原理

当连接到内部数据源元素与用户交互︰

1. 云服务创建的查询，以及数据源中的加密凭据，并将查询发送到网关来处理队列。
1. 服务分析查询并将该请求推送到 Azure 服务总线。
1. 内部数据网关进行轮询的 Azure 服务总线挂起的请求。
1. 网关获取查询、 解密凭据，并连接到数据源中使用这些凭据。
1. 网关发送到数据源执行的查询。
1. 结果从数据源发送回网关，然后拖到云服务。 然后，该服务使用的结果。

## <a name="troubleshooting"></a>故障排除

### <a name="update-to-the-latest-version"></a>请更新至最新版本

当网关版已过期，可以表面大量的问题。  它是很好的常规做法，以确保所使用的最新版本。  个月或更长时间，如果没有更新该网关，您可以考虑安装最新版本的网关，是否可以重现该问题，请参阅。

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>错误︰ 无法将用户添加到组。 （-2147463168 PBIEgwService 性能日志用户）

如果您试图在不支持的域控制器上安装网关，您可能会收到此错误。 您将需要部署在一台计算机不是域控制器上的网关。

## <a name="tools"></a>工具

### <a name="collecting-logs-from-the-gateway-configurator"></a>从网关配置器收集日志

您可以收集多个日志中的网关。 始终从日志开始 ！

#### <a name="installer-logs"></a>安装程序日志

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>配置日志

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>企业网关服务日志

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>事件日志

数据管理网关和 PowerBIGateway 日志是出现在**应用程序和服务日志**。

### <a name="fiddler-trace"></a>Fiddler 跟踪

[Fiddler](http://www.telerik.com/fiddler)是从监视 HTTP 通信量的 Telerik 的免费工具。  可以看到后面，并规定与电源 BI 服务从客户端计算机。 这可能显示的错误和其他相关的信息。

## <a name="next-steps"></a>下一步行动
- [创建内部连接到应用程序逻辑](app-service-logic-gateway-connection.md)
- [企业集成功能](app-service-logic-enterprise-integration-overview.md)
- [逻辑应用程序连接器](../connectors/apis-list.md)