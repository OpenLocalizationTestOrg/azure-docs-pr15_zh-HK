<properties
    pageTitle="如何配置应用程序服务环境 |Microsoft Azure"
    description="配置、 管理和监视应用程序服务环境"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>配置应用程序服务环境 #

## <a name="overview"></a>概述 ##

从较高层面，Azure 应用程序服务环境包括几个主要组件︰

- 应用程序服务环境中运行的计算资源承载的服务
- 存储
- 数据库
- Classic(V1) 或资源 Manager(V2) Azure 虚拟网络 (VNet) 
- 子网中运行的应用程序服务环境承载服务与

### <a name="compute-resources"></a>计算资源

使用四个资源池的计算资源。  每个应用程序服务环境 (ASE) 都有一套前端和三个可能的辅助池。 您不需要使用所有三个工作人员池-如果需要，可以只使用一个或两个。

资源池 （前端和工作人员） 中的主机不是承租人可直接访问的。 您不能使用远程桌面协议 (RDP) 连接到它们、 更改其资源调配，或作为一名管理员在其上。

您可以设置资源池的数量和大小。 在 ASE，您有四种大小选项，被标为 P4 通过 P1。 这些大小和其定价的详细信息，请参阅[应用程序服务的定价](../app-service/app-service-value-prop-what-is.md)。
更改大小或数量被称为缩放操作。  一次，只有一个缩放操作可以是正在进行中。

**前端**︰ 前端是您的应用程序保存在您的 ASE 的 HTTP/HTTPS 端点。 在前端，请不要运行工作负载。

- ASE 开头两个 P2s，这是开发/测试工作负载和低级生产工作负载足够。 我们强烈建议 P3s 为中等粗生产工作负载的要求。
- 对于中等粗生产工作负载的要求，我们建议您有至少四个 P3s，以确保有足够安排的维护发生时运行的前端。 一次，计划的维护活动将下一个前端。 这降低了整体维护事件期间可用前端的产能。
- 不能立即添加新的前端实例。 他们可以提供 2 至 3 小时之间。
- 进一步比例进行微调，您应当监视活动请求度量前端池、 CPU 百分比和内存百分比。 如果 CPU 或内存百分比是 70%以上在运行 P3s 时，，添加更多的前端。 如果活动请求值平均为 15000 到 20000 请求每个前端，还应添加更多的前端。 总体目标是要保持 70%以下的 CPU 和内存百分比和平均到前每 15000 请求下面的活动请求结束时运行的 P3s。  

**工作人员**︰ 工作人员的实际运行您的应用程序。 当您扩展您的应用程序服务的计划时，使用相关联的辅助池的工作人员组成。

- 不能立即添加工作人员。 他们可能需要从 2 到 3 小时提供，无论多少都被添加。
- 扩展的一个计算资源的任何池的大小将每个更新域需要 2 到 3 个小时。 ASE 中有 20 个更新域。 如果缩放辅助池 10 个实例的计算大小，可能需要完成 20 到 30 个小时之间。
- 如果要更改在辅助池使用的计算资源的大小，将导致冷启动该辅助池中正在运行的应用程序。

若要更改计算资源池大小的工作人员，没有运行任何应用程序的最快方法是︰

- 缩小的实例计数为 0。 它大约需要 30 分钟解除分配您的实例。
- 选择新的计算大小和实例数。 从这里，将花费 2 到 3 个小时才能完成之间。

如果您的应用程序需要更大的计算资源大小，则不能利用的早期指导。 而不是更改辅助池承载这些应用程序的大小，可以填充另一个辅助池与工作人员的所需大小和您的应用程序移到该池。

- 在另一个工作人员池中创建所需的计算大小的其他实例。 这将需要 2 到 3 个小时才能完成。
- 将承载的应用程序需要较大的新配置的辅助池到您的应用程序服务计划重新分配。 这是一项快速操作，应采取不超过一分钟的时间完成。  
- 如果您不再需要那些未使用的实例，第一辅助池缩小规模。 此操作完成所花费大约 30 分钟。

**自动缩放**︰ 一种工具，可帮助您管理您的计算资源消耗是自动缩放。 您可以使用自动缩放的前端或辅助池。 可以在上午执行诸如增加您的任何池类型的实例，并减少它在晚上。 或者，也许也可以在工作人员中工作池中的可用数量低于某一阈值时添加实例。

如果您想要设置自动缩放比例规定计算资源池指标，则请记住资源调配所需的时间。 有关自动缩放的应用程序服务环境的更详细信息，请参阅[如何配置应用程序服务环境中的自动缩放][ASEAutoscale]。

### <a name="storage"></a>存储

每个 ASE 配置为使用 500 gb 的存储空间。 在 ASE 的所有应用程序使用此空间。 此存储空间是 ASE 的部分，目前不能切换为使用存储空间。 如果要向虚拟的网络路由或安全进行调整，需要仍允许访问 Azure 存储 — 或 ASE 不能正常工作。

### <a name="database"></a>数据库

该数据库存储了定义环境中，以及在其中运行的应用程序有关的详细信息的信息。 这也是 Azure 保留订阅的一部分。 它不是一个可以直接操作的能力。 如果要向虚拟的网络路由或安全进行调整，需要仍允许访问 SQL Azure — 或 ASE 不能正常工作。

### <a name="network"></a>网络

用您的 ASE VNet 可以做当您创建了 ASE 或提前进行。 在 ASE 创建过程中创建子网时，它会强制 ASE 虚拟网络同一个资源组中。 如果您需要您 ASE 用于为您 VNet 不同的资源组，然后您需要创建使用资源管理器模板您 ASE。

有用于 ASE 虚拟网络上的一些限制︰
 
- 虚拟网络必须是区域性的虚拟网络。
- 需要子网 8 或多个地址与 ASE 的部署位置。
- 子网被用来承载 ASE 后，无法更改子网的地址范围。 因此，我们建议子网包含至少 64 的地址，以适应任何未来的 ASE 增长。
- 没有其他的子网而 ASE。

与包含 ASE，[虚拟的网络]托管服务[virtualnetwork]和子网是受用户的控制。  您可以管理您通过虚拟的网络用户界面或 PowerShell 的虚拟网络。  在标准或资源管理器 VNet，可以部署 ASE。  门户和 API 体验经典和资源管理器 VNets 之间稍有不同，但 ASE 体验是相同的。

用于承载 ASE VNet 可以使用任一专用 RFC1918 IP 地址，或者它可以使用公用 IP 地址。  如果您想要使用不受 RFC1918 IP 范围 （10.0.0.0/8、 172.16.0.0/12、 192.168.0.0/16） 则需要创建可供您 ASE ASE 创建之前您 VNet 和子网。

此功能将放在虚拟网络的 Azure 应用程序服务，因为它意味着您您 ASE 中承载的应用程序现在可以访问可直接供通过 ExpressRoute 或站点到站点虚拟专用网络 (Vpn) 的资源。 您的应用程序的服务环境中的应用程序不需要附加网络功能来访问资源可用到虚拟网络承载您的应用程序的服务环境。 这意味着您不需要使用 VNET 集成或混合连接到资源中或连接到您的虚拟网络。 您仍然可以使用这些功能的两个于访问资源虽然在不连接到虚拟网络的网络。

例如，可以使用 VNET 集成在您的订阅，但没有连接到您的 ASE 处于虚拟网络的虚拟网络集成。 也可以仍然使用混合连接到其他网络，就像通常情况下，您可以访问资源。  

如果没有获配 ExpressRoute VPN 虚拟网络，您应该知道某些有 ASE 的路由需求。 有一些不与 ASE 兼容的用户定义工艺路线 (UDR) 配置。 有关运行 ASE ExpressRoute，使用的虚拟网络中的更多详细信息，请参阅[运行的应用程序服务环境中使用 ExpressRoute 的虚拟网络]的[ExpressRoute]。

#### <a name="securing-inbound-traffic"></a>保护的入站的通信

有两种主要的方法来控制对您的 ASE 的入站的通信。  您可以使用网络安全 Groups(NSGs) 来控制哪些 IP 地址可以访问您的 ASE，如此处所述[如何控制应用程序服务环境中的入站的通信](app-service-app-service-environment-control-inbound-traffic.md)，还可以用内部负载 Balancer(ILB) 配置您 ASE。  如果您想要使用 NSGs 到 ILB ASE 限制访问权限，可也是一起使用这些功能。

当您创建 ASE 时，它将在您 VNet 中创建 VIP。  有外部和内部两个 VIP 类型。  当您创建与外部的 VIP ASE 您的应用程序中您 ASE 将可通过互联网可路由的 IP 地址进行访问。 当您选择内部您 ASE 将使用 ILB 配置并不会直接访问互联网。  ILB ASE 仍然需要外部的 VIP，但它仅用于 Azure 的管理和维护访问。  

ILB ASE 创建过程提供了使用 ILB ASE 的子域，并需要管理您自己为您指定的子域的 DNS。  因为您设置您还需要管理证书用于 HTTPS 访问子域名。  ASE 创建之后将提示您提供该证书。  若要了解更多有关创建和使用 ILB ASE 阅读[使用与应用程序服务环境内部负载平衡器][ILBASE]。 


## <a name="portal"></a>门户网站

您可以管理和监视应用程序服务环境中使用 Azure 门户中的用户界面。 如果您有 ASE，您就可能会看到您的侧栏上的应用程序服务符号。 该符号用于表示在 Azure 的门户应用程序服务的环境︰

![应用程序服务环境符号][1]

若要打开列出所有您的应用程序服务环境的用户界面，可以使用图标或选择 v 形展开按钮 (">"符号) 底部的侧栏选择的应用程序服务的环境。 通过选择列出 ASEs 之一，您可以打开用来监视和管理它的用户界面。

![用于监控和管理您的应用程序服务环境的用户界面][2]

此第一个刀片以及每个资源池的指标图表显示您 ASE 的某些属性。 一些**基本**块中显示的属性是将打开刀片式服务器，与它相关联的超链接。 例如，您可以选择要打开与您的 ASE 运行中的虚拟网络相关联的用户界面的**虚拟网络**名称。 **应用程序服务的计划**和**应用程序**每个打开刀片式服务器列表中您 ASE 这些项目。  

### <a name="monitoring"></a>监视

图表，可以看到大量的数据中每个资源库的性能指标。 对于前端的池，您可以监视的平均 CPU 和内存。 对于工作人员池，您可以监视使用的数量和可用的数量。

计划可以使多个应用程序服务使用的辅助池的工作人员。 工作负荷不分配以相同的方式使用前端服务器，因此 CPU 和内存使用量没有提供多少有用的信息。 尤其是如果您管理的其他人使用此系统是跟踪使用过的工作人员和均可，数量更重要。  

也可以使用所有可跟踪的度量标准的组织结构图中设置通知 设置警报这里以相同方式工作为其他地方中应用程序服务。 从**通知**UI 部件或钻取到任何标准用户界面，然后选择**添加警报**，您可以设置警报。

![标准用户界面][3]

刚才所讨论的指标是应用程序服务环境指标。 也有一些有计划的应用程序服务级别的衡量标准。 这是在其中监视 CPU 和内存很有意义。

在 ASE，所有应用程序服务计划是专用的应用程序服务计划。 这意味着，只有应用程序分配给主机上的应用程序服务计划在该应用程序服务计划中将应用程序正在运行。 若要查看您的应用程序服务的计划的详细信息，调出您的应用程序服务的计划，从任何 ASE UI 中的列表或**浏览应用程序服务计划**（其中列出了所有这些）。   

### <a name="settings"></a>设置

在 ASE 刀片式服务器，没有**设置**部分中包含几个重要功能︰

**设置** > **属性**︰**设置**刀片式服务器时启动 ASE 刀片自动打开。 在顶部是**属性**。 这里是冗余的**精要**中, 看到的内容的项目数，但非常有用的是**虚拟 IP 地址**，以及**出站 IP 地址**。

![刀片式服务器设置和属性][4]

**设置** > **IP 地址**︰ 在您 ASE 创建 IP 安全套接字层 (SSL) 的应用程序时，您需要一个 SSL IP 地址。 为了获取一个数字证书，您 ASE 需要 SSL IP 地址它拥有可以分配。 当创建 ASE 时，为此目的，有一个 SSL IP 地址，但可以添加更多。 没有一项费用为解决其他 IP SSL，[定价的应用程序服务]中所示[AppServicePricing]（在 SSL 连接上的部分）。 其他价格是 IP SSL 价格。

**设置** > **前结束池** / **辅助池**︰ 每个这些资源池刀片式服务器提供的功能只能在该资源库，并提供控件来完全调整该资源池上看到的信息。  

对于每个资源库基刀片式服务器提供该资源库指标的图表。 就像与 ASE 刀片式服务器，从图表可以进入图表并设置为所需的通知。 从特定的资源池的 ASE 刀片式服务器设置的警报作为从资源库中执行的操作相同。 从辅助池**设置**刀片式服务器，您有权访问所有应用程序或应用程序服务计划此工作池中运行。

![工作人员池设置用户界面][5]

### <a name="portal-scale-capabilities"></a>门户的扩展能力  

有三种刻度操作︰

- 更改 ASE 中所提供的 IP SSL 使用的 IP 地址的数。
- 更改计算资源在资源库中使用的大小。
- 更改用于资源池，手动或自动缩放到的计算资源的数量。

在门户中，有三种方法来控制您在资源池中有多少台服务器︰

- 从顶部主 ASE 刀片式服务器缩放操作。 可以对前端和辅助池进行比例多配置更改。 所有单个操作的形式应用它们。
- 从单个资源池**比例**刀片式服务器，它是在**设置**下手动缩放操作。
- 自动缩放，从单个资源池**比例**刀片式服务器设置。

要使用缩放操作 ASE 刀片上的，拖动滑块，想和保存的数量。 此用户界面还支持更改大小。  

![扩展用户界面][6]

若要在特定的资源池中使用手动或自动缩放功能，请转到**设置** > **前结束池** / 根据**辅助池**。 然后打开您想要更改该池。 转到**设置** > **向外的扩展**或**设置** > **来扩大规模**。 **向外扩展**刀片式服务器使您可以控制实例的数量。 **向上扩展**使您能够控制资源的大小。  

![缩放设置用户界面][7]

## <a name="fault-tolerance-considerations"></a>容错的注意事项

您可以配置要使用达 55 总计算资源的应用程序服务环境。 这些 55 的计算资源，只有 50 可用于主机的工作负载。 这样做的原因是双重的。 还有至少 2 前端的计算资源。  这就给予了达 53，从而支持工作池分配。 为了提供容错功能，您需要根据以下规则分配更多的计算资源︰

- 每个工作人员池需要至少 1 个附加的计算资源不可用，要分配工作负荷。
- 当在辅助池的计算资源量超过某个特定值时，则需要容错另一个计算资源。 这不是这种情况在前端的池。

在任何单个辅助池，容错要求包括分配给工作人员池资源 X 的给定值为︰

- 如果 X 是介于 2 到 20 之间，您可以使用工作负载的可用计算资源量 X-1。
- 如果 X 是 21 和 40 之间，您可以使用工作负载的可用计算资源量 X-2。
- 如果 X 在 41 到 53 之间，您可以使用工作负载的可用计算资源量 X-3。

最小的占地面积都有 2 个前端服务器和 2 的工作人员。  使用上面的语句然后，下面是几个示例来说明︰  

- 如果单个池中有 30 的工作人员，其中 28 可以习惯主机工作负载。
- 如果在单个池 2 的工作人员，1 可以使用主机工作负载的要求。
- 如果单个池中有 20 工人，19 可以使用主机工作负载的要求。  
- 如果在一个池中有 21 的工作人员，之后仍然只有 19 可用主机工作负载的要求。  

容错能力方面是很重要，但需要请记住当您扩展超过特定阈值。 如果您想要添加更多容量从 20 个实例，然后转到 22 或更高版本因为 21 不会添加任何更多的容量。 这同样适用将高于 40，其中添加容量下一个数字 42。  

## <a name="deleting-an-app-service-environment"></a>删除应用程序服务环境 ##

如果您想要删除一个应用程序的服务环境，然后只需使用**删除**操作的应用程序服务环境刀片顶端。 执行此操作时，将提示您输入您的应用程序服务环境，以确认您确实要执行此操作的名称。 请注意，当您删除一个应用程序的服务环境，您删除所有以及其中的内容。  

![删除用户界面的应用程序服务环境][9]  

## <a name="getting-started"></a>入门教程

若要开始使用应用程序服务的环境，了解[如何创建一个应用程序的服务环境](app-service-web-how-to-create-an-app-service-environment.md)。

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
