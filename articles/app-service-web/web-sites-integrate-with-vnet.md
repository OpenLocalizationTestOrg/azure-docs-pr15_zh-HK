<properties 
    pageTitle="与 Azure 的虚拟网络集成的应用程序" 
    description="演示如何在 Azure 应用程序服务的应用程序连接到新的或现有 Azure 的虚拟网络" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>将与 Azure 的虚拟网络集成在您的应用程序 #

本文档描述 Azure 应用程序服务虚拟网络集成功能，并演示如何将其设置在[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)的应用程序。  如果您熟悉 Azure 虚拟网络 (VNETs)，这是一个功能，允许您控制访问的非互联网路由网络中放置的 Azure 的资源很多。  这些网络可以连接上部署网络使用不同的 VPN 技术。  若要了解更多关于 Azure 虚拟网络入手此处的信息︰ [Azure 虚拟网络概述][VNETOverview]。  

Azure 应用程序服务有两种形式。  

1. 支持全部的价格计划的多租户系统
1. 应用程序服务环境 (ASE) 高级功能部署到您的 VNET。  

本文经过 VNET 集成和应用程序的服务环境。  如果您想要了解更多有关 ASE 功能，然后开始与此处的信息︰[应用程序服务环境简介][ASEintro]。

VNET 集成到虚拟网络中的资源使您 web 应用程序的访问权限，但不会授予私人访问您的 web 应用程序与虚拟的网络。  与配置与内部负载平衡器 (ILB) ASE 专用网站访问权限才可用。  有关使用 ILB ASE 的详细信息，文章在此处开始︰[创建和使用 ILB ASE][ILBASE]。 

可以将 VNET 集成一个常见方案使从您的 web 应用程序访问数据库或在 Azure 虚拟网络中的虚拟机上运行的 web 服务。  与 VNET 集成不需要但可以 VM 上的应用程序使用专用的非互联网路由地址改为公开一个公共的终结点。  

VNET 集成功能︰

- 要求使用标准或最优定价计划 
- 将使用 Classic(V1) 或资源 Manager(V2) VNET 
- 支持 TCP 和 UDP
- 与 Web、 移动和 API 的应用程序的工作原理
- 使应用程序每次连接到仅 1 VNET
- 允许达 5 VNETs 与集成的应用程序服务计划中 
- 允许相同的 VNET，可供多个应用程序在应用程序服务计划
- 支持由于依赖 VNET 网关 99.9%SLA

有一些 VNET 集成不支持包括︰

- 装配驱动器
- 广告集成 
- NetBios
- 私人网站访问权限

### <a name="getting-started"></a>入门教程 ###
下面是要到虚拟网络连接您的 web 应用程序之前，请记住的一些事项︰

- VNET 集成只适用于**标准**或**特优**定价计划在应用程序中。  如果您启用此功能，然后扩展到不受支持的定价计划您的应用程序服务计划您的应用程序将丢失它们的连接到他们使用的 VNETs。  
- 如果您的目标虚拟网络已经存在，则必须点到站点 VPN 前它可以连接到应用程序启用与动态路由的网关。  如果您的网关配置静态路由，则不能启用点到站点虚拟专用网络 (VPN)。
- VNET 必须在为您的应用程序服务 Plan(ASP) 相同的订阅。  
- 与 VNET 集成的应用程序将使用为该 VNET 指定的 DNS。
- 默认情况下您集成的应用程序将只将通信路由到您的 VNET 基于您 VNET 中所定义的路由。  


## <a name="enabling-vnet-integration"></a>实现 VNET 集成 ##

本文档主要重点为 VNET 集成使用 Azure 门户。  要启用您的应用程序使用 PowerShell VNET 集成，请按照下面的说明︰[连接到虚拟网络使用 PowerShell 您应用程序][IntPowershell]。

您可以选择将您的应用程序连接到新的或现有的虚拟网络。  如果您创建一个新的网络作为您然后在刚创建的 VNET 的集成的一部分，将为您预配置动态路由的网关和将启用指向站点 VPN。  

>[AZURE.NOTE] 配置新的虚拟网络集成可能需要几分钟。  

若要启用 VNET 集成打开您的应用程序设置，然后选择网络。  向上打开的用户界面提供了三种网络选项。  本指南只会到 VNET 集成但混合连接并在本文档的后面部分讨论了应用程序服务的环境。  

如果您的应用程序不在正确的定价计划 UI 还为您将使您能够扩展到您所选的高定价计划您的计划。


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>启用预先存在的 VNET 与 VNET 集成###
VNET 集成用户界面允许您从您的 VNETs 的列表中选择。  经典的 VNETs 将表明它们与 VNET 名称旁边的括号中的"古典"一词是这样。  对列表进行排序，首先列出的资源管理器 VNETs。  如下所示的图像中可以看到，可以选择一个 VNET。  有多个原因，VNET 将显示为灰色包括︰

- VNET 是在您的帐户有权访问的另一个订阅
- VNET 并没有点网站启用
- VNET 没有动态路由的网关


![][2]

若要启用集成只需单击您想要与集成 VNET。  选择 VNET 之后，您的应用程序将自动重新启动以使更改生效。  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>启用指向站点中经典的 VNET #####
如果您 VNET 没有网关，也不具有对网站的点然后必须首先设置的。  要这样做对于经典的 VNET，请转到[Azure 门户网站][AzurePortal]和调出虚拟的 Networks(classic) 的列表。  从网络上的此处单击您想要与集成在一起，然后单击下名为 VPN 连接的精要大框。  从这里，您可以创建您的点来站点 VPN，甚至将它创建一个网关。  通过点转到网关创建经验的网站后，可以将其大约 30 分钟之前就可以。  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>启用到站点中的资源管理器 VNET 点 #####

资源管理器 VNET 配置的网关和指向的网站需要使用 PowerShell 述此处，[配置点到站点连接到虚拟网络使用 PowerShell][V2VNETP2S]。  若要执行此功能的 UI 尚不可用。 

### <a name="creating-a-pre-configured-vnet"></a>创建预先配置的 VNET ###
如果您想要创建新的 VNET 配置的网关和点到网站，网络用户界面的应用程序服务之后，将有能力做到这一点但只为资源管理器 VNET。  如果您想要创建经典 VNET 网关和点到站点，则需要手动执行此操作通过网络连接的用户界面。 

若要创建通过 VNET 集成用户界面资源管理器 VNET，只需选择**创建新的虚拟网络**，并提供:

- 虚拟网络名称
- 虚拟网络地址块
- 子网名称
- 子网地址块
- 网关地址块
- 点到站点地址块

如果您希望此 VNET 连接到任何其他网络则应避免领与这些网络重叠的 IP 地址空间。  

>[AZURE.NOTE] 资源管理器 VNET 创建一个网关与大约需要 30 分钟，目前将与您的应用程序集成 VNET。  您 VNET 创建与网关后您需要回到您应用程序 VNET 集成的用户界面，然后选择您新的 VNET。

![][3]

Azure VNETs 通常创建在专用网络地址。  默认情况下，VNET 集成功能将这些 IP 地址范围的任何通信到您的 VNET。  专用 IP 地址范围是︰

- -这是 10.0.0.0 相同-10.0.0.0/8 10.255.255.255
- -这是与 172.16.0.0 相同-172.16.0.0/12 172.31.255.255 
- 192.168.0.0/16-这是 192.168.0.0 相同-192.168.255.255 添加
 
需要指定 CIDR 表示法在 VNET 地址空间。  如果您熟悉使用 CIDR 表示法，它是用于指定使用 IP 地址和一个整数，表示网络掩码的地址块的方法。 作为快速参考，请考虑 256 地址是 10.1.0.0/24，和 10.1.0.0/25 是 128 个地址。  / 32 的 IPv4 地址将只需 1 个地址。  

然后如果您在此处设置 DNS 服务器信息，将为您的 VNET 设置。  VNET 创建之后可以编辑此信息从 VNET 的用户体验。

当您创建传统的 VNET 使用 VNET 集成用户界面时，它将在您的应用程序所在的资源组中创建 VNET。 

## <a name="how-the-system-works"></a>系统的工作原理 ##
在幕后此功能生成点到站点 VPN 技术，以将您的应用程序连接到您的 VNET 上。   在 Azure 应用程序服务的应用程序有多租户系统体系结构，它通过虚拟机调配直接在 VNET 应用程序，不能。  通过基于点到网站技术我们限制只是承载该应用程序虚拟机的网络访问。  对网络的访问进一步限制这些应用程序主机上，以便您的应用程序只能访问您将其配置为访问网络。  

![][4]
 
如果没有与虚拟网络将需要使用 IP 地址来配置 DNS 服务器。  在使用 IP 地址时，请记住此功能的主要好处是它使您能够使用您的专用网络中的专用地址。  如果您将您的应用程序设置为使用公用 IP 地址为您的 Vm 的一个然后没有使用 VNET 集成功能和通过 internet 进行通信。


##<a name="managing-the-vnet-integrations"></a>VNET 集成管理##

连接和断开连接到 VNET 的能力是在应用程序级别。  在 ASP 级别都可以影响 VNET 集成跨多个应用程序的操作。  从显示在应用程序级别的用户界面可以在您的 VNET 来获取详细信息。  在 ASP 级别也显示大部分相同的信息。  

![][5]

从网络功能状态页中，您可以看到是否您的应用程序连接到您的 VNET。  VNET 网关故障时为各种原因然后这将会显示为未连接。  

现在已可供您在应用程序级别的 VNET 集成 UI 是获得 ASP 的细节信息相同的信息。  以下是这些项目︰

- VNET 名称-此链接会打开网络用户界面
- 位置-这反映了您的 VNET 的位置。  很可能与在另一个地点 VNET 集成。
- 证书状态-有证书用于保护应用程序和 VNET 之间的 VPN 连接。  这反映了一个测试，以确保它们同步。
- 网关状态-您的网关应下无论出于什么原因然后您的应用程序无法访问 VNET 中的资源。  
- VNET 地址空间--这是您的 VNET 的 IP 地址空间。  
- 指向站点地址空间--这是为您 VNET 网站 IP 地址空间的点。  您的应用程序将显示为从一个在此地址空间的 Ip 通信。  
- 站点到站点地址空间--您可以使用站点到站点 Vpn 连接您的 VNET 到您的内部资源或其他 VNETs。  如果您有配置 VPN 连接将显示在此处使用定义 IP 地址范围，然后。
- DNS 服务器-如果您有配置与您的 VNET，则此处列出的 DNS 服务器。
- Ip 路由到 VNET-那里是您 VNET 已定义路由的 IP 地址的列表。  这些地址将显示在此处。  

VNET 集成的应用程序视图中可以采取的唯一操作是您的应用程序断开当前连接到 VNET。  要做这只需单击断开连接在顶部。  此操作不会更改您的 VNET。  其中包括网关 VNET 和它的配置保持不变。  如果您想删除您的 VNET，您需要首先删除的资源包括网关。  

应用程序服务计划视图有很多其他的操作。  它还以不同的方式比从访问应用程序。  要达到 ASP 网络用户界面只打开您的 ASP 用户界面和滚动下。  还有一个名为网络功能状态的 UI 元素。  它将给周围 VNET 集成一些次要细节。  在此用户界面上单击打开网络功能状态用户界面。  如果您单击"单击此处可管理"上将打开列出在此 ASP VNET 集成的用户界面。

![][6]

ASP 的位置是很有必要看一看您正在集成与 VNETs 的位置时，请记住。  VNET 在另一个位置时您将看到滞后时间问题的可能性更大。  

与集成 VNETs 是您的应用程序集成在一起在此 ASP 和多少可以有多少 VNETs 上提醒。  

要查看每个 VNET 添加详细信息，只需单击您感兴趣的 VNET。  除细节外，提到前面您还会看到使用该 VNET 在此 ASP 应用程序的列表。  

对于操作有两个主要措施。  第一种是添加驱动器通信离开您的 VNET 到您的应用程序的路由的能力。  第二个操作是同步证书和网络信息的能力。

![][7]

**路由选择**正如前面指出您 VNET 中所定义的路由是什么用于将通信定向到您从您应用程序 VNET。  有一些使用提供了客户希望发送额外的出站通信从应用程序到 VNET，并为他们这种能力也是如此。  怎么样的通信后，由客户将其 VNET 的配置。  

**证书**证书的状态反映了正在执行的应用程序服务来验证我们的 VPN 连接使用这些证书仍然完好的检查。  VNET 集成启用时，如果第一个集成到该 VNET 是从任何应用程序在此 ASP，则需的交换证书，以确保连接的安全性。  证书以及我们得到的 DNS 配置、 工艺路线和描述网络其他类似事情。
如果更改这些证书或网络的信息，则您需要单击"同步网络"。  **注意**︰ 当您单击"同步网络"，然后将连接导致短暂的停机，您的应用程序和您 VNET 之间。  而不会重新启动您的应用程序连接中断导致网站无法正常工作正常。  

##<a name="accessing-on-premise-resources"></a>访问本地资源##

VNET 集成功能的好处之一是，如果您 VNET 您的应用程序可以访问到您的内部资源已从您的应用程序，然后在内部网络有站点到站点 VPN 连接。  对于这种方式，但您可能需要更新的您指向站点 IP 路由链接上部署 VPN 网关的范围。  当站点到站点 VPN 第一次启动时使用对其进行配置的脚本应设置工艺路线包括您指向站点 VPN。  如果将点添加到站点 VPN 您创建站点到站点 VPN 则将需要手动更新路由。  如何做到这一点的详细信息将随网关和这里未描述。  

>[AZURE.NOTE] 尽管 VNET 集成功能适用于站点到站点 VPN 访问内部资源上它目前不会与 ExpressRoute VPN 来执行相同的操作。  与经典或资源管理器 VNET 集成时也是如此。  如果您需要通过 ExpressRoute VPN 访问的资源，然后可以使用可以运行在您 VNET ASE。 

##<a name="pricing-details"></a>定价详细信息##
还有一些定价应该使用 VNET 集成功能时的注意的细微差别。  有 3 相关的费用与使用此功能︰

- ASP 定价层要求
- 数据传输费用
- VPN 网关成本。

您可以使用此功能的应用程序，需要为标准或津贴应用程序服务计划。  这里的这些成本上可以看到更多详细信息︰[应用程序服务定价][ASPricing]。 

由于站点 Vpn 的方式点处理，您始终可以用于出站数据的费用通过 VNET 集成连接即使在同一个数据中心 VNET。  若要了解这些费用是来看看这里︰[数据转移定价详细信息][DataPricing]。  

最后一个项目是 VNET 网关的开销。  如果不需要为其他内容如站点到站点 Vpn 网关然后将支付网关支持 VNET 的集成功能。  这里的这些成本上有详细信息︰ [VPN 网关定价][VNETPricing]。  

##<a name="troubleshooting"></a>故障排除##

虽然可以很容易地设置了该功能并不意味着您的体验，将是自由的问题。  您会遇到访问您需的终结点存在的问题是某些实用程序可用于测试应用程序控制台连接。  有两个可以使用的控制台体验。  一个从 Kudu 控制台，另一种是可以在 Azure 门户访问控制台。  若要从您的应用程序进入 Kudu 控制台转到工具-> Kudu。  这是转到 [站点名] 相同。 scm.azurewebsites.net。  只需打开后请转到调试控制台选项卡。  若要获取到 Azure 门户托管控制台，然后从您的应用程序转到工具-> 控制台。  


####<a name="tools"></a>工具####

Nslookup 工具 ping，tracert 无法通过控制台由于安全约束。  以填充 void 那里已经添加的两个独立的工具。  若要测试 DNS 功能我们添加了一个名为 nameresolver.exe 的工具。  语法是︰

    nameresolver.exe hostname [optional: DNS Server]

您可以使用 nameresolver 检查取决于您的应用程序的主机名。  这种方法可以测试，如果有任何错误地使用您的 DNS 配置或也许不能访问到您的 DNS 服务器。

下一步的工具允许您测试的主机和端口组合的 TCP 连接。  该工具称为 tcpping.exe 的语法是︰

    tcpping.exe hostname [optional: port]

此工具将告诉您是否可以到达特定的主机和端口，但不是会执行相同的任务可以使用 ICMP 的 ping 实用程序。  ICMP ping 实用程序将告诉您您的主机是否向上。  Tcpping 与您了解您是否可以访问主机上的特定端口。  


####<a name="debugging-access-to-vnet-hosted-resources"></a>对 VNET 的访问调试托管资源####

有许多事情，可以防止您的应用程序到达特定的主机和端口。  大多数情况下它是三件事之一︰

- **没有防火墙的方式** 如果您的防火墙的方式您将击中 TCP 超时。  这就在这种情况下是 21 秒。  使用 tcpping 工具来测试连接性。  TCP 超时可以是由于防火墙之外的很多事情，但那里开始。  
- **DNS 是不可访问** DNS 超时为 3 秒，每个 DNS 服务器。  如果您有 2 6 秒的 DNS 服务器。  使用 nameresolver 来查看 DNS 是否工作。  请记住您不能使用 nslookup，因为它不使用您 VNET 配置了 DNS。
- **无效的 P2S IP 范围**指向站点 IP 范围需要在 RFC 1918 专用 IP 地址范围 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) 如果范围以外，使用 IPs 则不起作用的事情。  

如果这些项不回答您的问题，简单事情首先查找︰  

- 网关是否显示为处于门户？
- 是否为正在进行的同步显示的证书？
- 任何人都没有不执行受影响 Asp 中"同步网络"更改网络配置？ 

如果您的网关是向下再使其恢复。  如果您的证书不同步的转到 ASP 视图的 VNET 集成，并点击"同步网络"。  如果您怀疑已被 VNET 配置所做的更改，并且它未被同步处理您的 Asp 然后转到 ASP 视图的 VNET 集成和命中"同步网络"只是作为一个提醒，这将导致短暂的停机，VNET 连接与您的应用程序。  

如果所有的都很好，则需要深入了解一些更深层次︰

- 是否有任何其他使用 VNET 集成以达到相同的 VNET 中的资源的应用程序？ 
- 您可以转到控制台应用程序并使用 tcpping 到达您的 VNET 中的任何其他资源？  

如果上述任一条件 VNET 集成则是很好，问题是其他地方。  这是得是更具挑战性，因为没有简单方法，请参阅为什么您不能访问主机 / 端口。  一些原因包括︰

- 阻止来自您指向站点 IP 范围内对应用程序端口访问主机上有防火墙。  经常跨越子网要求公共访问。
- 目标主机已关闭
- 您的应用程序已关闭
- 有的不正确的 ip 地址或主机名
- 您的应用程序正在侦听不同的端口比您预期的方式。  您可以通过转到该主机上并使用"netstat aon"选中此 cmd 提示符下。  这将显示的进程 ID 什么端口侦听。  
- 以这种方式，从而导致访问到您的应用程序主机和端口从您指向站点 IP 范围配置网络安全组

请记住，您不知道哪些 IP 在您的观点对站点 IP 范围，将使用您的应用程序，因此您需要允许访问整个范围内。  

其他调试步骤包括︰

- 登录到您的 VNET 中的另一个虚拟机并尝试达到从那里您资源主机 / 端口。  有某些 TCP ping 实用程序您可以使用为此目的，或甚至可以使用 telnet，如果需要。  这里的目的只是确定从该其他 VM 是否存在连接。 
- 从您的应用程序在控制台中弹出该主机和端口上另一个虚拟机和测试访问的应用程序  
####<a name="on-premise-resources"></a>部署资源####
如果您不能到达上部署的资源，则应检查的第一件事是如果可以到达您的 VNET 资源。  如果工作，接下来的步骤是很容易。  从您 VNET 在一个虚拟机，您需要尝试到达上部署应用程序。  您可以使用 telnet 或 TCP ping 实用程序。  如果 VM 找不到达您的内部资源，请首先确保您的站点到站点 VPN 连接工作。  如果它工作然后签上部署网关配置和状态以及前面提到的事情。  

现在如果您 VNET 承载虚拟机可以访问您在本地部署系统，但您的应用程序不能的原因是可能以下选项之一，然后︰
- 您的路由未配置有您指向站点中打开部署网关的 IP 地址范围
- 网络安全组被阻止访问您指向站点 IP 范围
- 在内部防火墙会阻止从您的点到网站 IP 范围的通讯
- 您有在您达到您在部署网络基于网站流量阻止您点的 VNET 用户定义 Route(UDR)

## <a name="hybrid-connections-and-app-service-environments"></a>混合连接和应用程序服务环境##
有 3 的功能，使 VNET 托管资源的访问。  它们是︰

- VNET 集成
- 混合连接
- 应用程序服务环境

混合的连接要求安装在您的网络中称为混合连接 Manager(HCM) 中继代理。  HCM 需要能够连接到 Azure 以及您的应用程序。  此解决方案是从远程网络上部署网络例如特别好或甚至其他云承载网络，因为它不需要 internet 访问终结点。  HCM 只能在 Windows 上运行，您可以有最多 5 个实例运行以提供高可用性。  混合连接仅支持 TCP 通过并且每个 HC 终结点匹配到特定的主机 / 端口组合。  

应用程序服务环境功能允许您以运行您 VNET Azure 应用程序服务的实例。  这使您的应用程序访问而无需任何额外的步骤您 VNET 中的资源。  一些应用程序服务环境的其他好处是您可以 14 gb 的 RAM 使用 8 专用的核心工作人员。  另一个好处是您可以扩展系统以满足您的需要。  与不同的多租户环境中，您的 ASP 有大小限制，ASE 您控制您希望授予对系统的资源数量。  对于此文档的网络焦点，获得不需要 VNET 集成了 ASE 的事情之一是它可以使用 ExpressRoute VPN。  

虽然一些使用案例的重叠，没有这些功能可以替换任何其他人。  知道要使用哪种功能依赖于您的需要和将如何使用它。  例如︰

- 如果您是开发人员并且只想在 Azure 中运行一个网站并使其访问在您的办公桌下工作站上的数据库则使用简单的方式就是混合连接。  
- 如果您想要放大量的大组织 web 属性在公共云，以及您自己网络中管理它们，然后在您想要应用程序服务环境。  
- 如果您有多个应用程序服务承载的应用程序，并且只想访问您 VNET 中的资源，则 VNET 集成的途径。  

有的用例之外一些简单相关方面。  如果您的 VNET 已连接到您然后使用 VNET 集成内部网络或应用程序服务环境是使用内部资源的简便方法。  另一方面，如果您 VNET 未连接到内部网络上，则更多的开销来设置站点到站点 VPN 与您 VNET 将与安装 HCM 比较。  

除了功能差异有也定价差异。  应用程序服务环境功能是特优服务但提供最大网络配置以及其他重要功能的可能性。  VNET 集成可与标准或特优 Asp 和非常适合于安全地消耗多租户应用程序服务从您 VNET 中的资源。  当前，混合连接取决于的 BizTalk 量需要基于客户的价格水平，开始免费，然后获取变得越来越昂贵。  尽管使用跨多个网络时，没有其他功能如混合连接可以使您能够访问超过 100 的独立网络中的资源。    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
