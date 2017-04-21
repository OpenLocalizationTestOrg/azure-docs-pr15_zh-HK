<properties
   pageTitle="Azure 架构引用 — IaaS︰ 实施 Azure 和 Internet 之间的 DMZ |Microsoft Azure"
   description="如何在 Azure 中实现与 Internet 访问的安全的混合网络体系结构。"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>实现一个 DMZ Azure 和 Internet 之间

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍了用于实现扩展您接受来自 internet 网络通信到 Azure 的内部网络的安全混合网络的最佳做法。 此参考体系结构扩展[实现 Azure 和本地数据中心之间的 DMZ]文章中描述的体系结构[implementing-a-secure-hybrid-network-architecture]。 建议您阅读该文档并理解该引用之前阅读本文档的结构。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。 

这种体系结构的典型使用情况包括︰

- 混合应用程序中的工作负载运行部分内部和部分在 Azure。

- 路由入站通信从内部和 internet 的 azure 基础结构。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示此体系结构中重要的组成部分︰

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此关系图位于"DMZ-公用"的页面上。

[![0]][0]

- **公共 IP 地址 (PIP)。** 这是公共的终结点的 IP 地址。 外部连接到 Internet 的用户可以访问系统，通过此地址。

- **网络虚拟设备 (NVA)。**  NVA 是个通用术语，用于描述虚拟机执行任务，如允许或拒绝为防火墙、 优化 WAN 的操作 （包括网络压缩），定制路由选择事件，或其他网络功能的访问。

- **Azure 负载平衡器。** 来自 internet 的所有传入请求传递到该负载平衡器和分发给公共 DMZ 中的 NVAs 站的网。

- **公共 DMZ 的入站网。** 此子网接受从 Azure 负载平衡器的请求。 传入请求传递给 NVAs 在 DMZ 中的一个。

- **公共 DMZ 站网。** 经 NVA 的请求通过该子 web 层内部负载平衡器。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。

### <a name="nva-recommendations"></a>NVA 建议

实现上互联网，另一个用于发出内部通信发出通信 NVAs 的一套。 正是因为这种设计提供了网络流量的两组之间没有安全边界都使用一套 NVAs 会带来安全风险。 它是一个优点，使用这种设计，因为它减少了检查安全规则的复杂性并使其更清楚哪些规则对应于每个传入的网络请求。 例如，一套 NVAs 实现仅在另一套内部通信流的 NVAs 实现规则的 internet 通讯的规则。

包括第 7 层 NVA 终止 NVA 级别的应用程序连接和维护使用的后端层的关系。 这保证从后端层的响应通信量通过 NVA 返回对称连接。  

### <a name="public-load-balancer-recommendations"></a>公共的负载平衡器的建议 ###

为了维护可伸缩性和可用性，部署公共 DMZ 中[可用性设置]站 NVAs[availability-set]并使用[internet 面临的负载平衡器][ load-balancer] internet 请求分发中可用性组 NVAs。  

配置负载平衡器可以接受请求只能在所需的互联网通信端口上。 例如，可以限制对端口 80 的入站的 HTTP 请求和到端口 443 入站的 HTTPS 请求。

## <a name="scalability-considerations"></a>可伸缩性注意事项

使用负载平衡器前面站的公共 DMZ 子网从一开始就面临着互联网设计您的基础结构。 即使您的体系结构最初要求单个 NVA，它会更容易扩展到多个 NVAs 将来如果已部署互联网面临负载平衡器。

## <a name="availability-considerations"></a>可用性考虑事项

互联网面临负载平衡器要求在公共 DMZ 中的每个 NVA 站子网以实现[健康探测器][lb-probe]。 无法响应此终结点上健康探测器被视为不可用，并且负载平衡器将请求定向到其他 NVAs 在同一可用性组。 请注意，是否所有的 NVAs 不能响应，应用程序将失败因此能够监视配置为通知 DevOps 健康 NVA 实例数降到阈值以下时，重要的是。

## <a name="manageability-considerations"></a>可管理性注意事项

限制入站公共 DMZ NVA 的响应请求的跳转框中仅管理子网的监视和管理功能。 如在[实施 Azure 和本地数据中心之间的 DMZ]中讨论[implementing-a-secure-hybrid-network-architecture]文档，定义从内部网络到跳转框中的网关的单个网络路由中管理子网限制访问。

如果从内部网络到 Azure 的网关连接处于断开状态时，仍可以通过部署骰，将其添加到跳转框中和中的远程处理从互联网通过与跳转框。

## <a name="security-considerations"></a>安全注意事项

此参考体系结构实现的多个级别的安全︰

- 互联网面临负载平衡器将请求定向到 NVAs 站公共 DMZ 子网中，并且只在应用程序所需的端口。

- 入站和出站公共 DMZ 子网的 NSG 规则防止 NVAs 遭到阻止超出 NSG 规则的请求。

- NVAs 的 NAT 路由配置指示传入的请求，在端口 80 和 443 到 web 层负载平衡器，但将忽略所有其他的端口上的请求。

请注意，您应该记录所有的所有端口上的传入请求。 定期审核日志，重视为这些可能表明入侵尝试超出预期参数的请求。

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>使用 NSGs 应用程序层之间的块密码/通信

每个 web、 业务层和数据层限制它们之间的通信使用 NSGs。 即在业务层使用 NSG 来阻止所有通信在 web 层中，并不是源自和数据层使用 NSG 都阻止不是源自在业务层中的所有通信。 如果您有需要扩展 NSG 规则，以允许更广泛访问这些存储层，权衡这些要求的安全风险。 每个新入站的路径表示意外或有意的数据泄露或应用程序损坏的机会。

## <a name="solution-deployment"></a>解决方案部署

为实现这些建议的参考体系结构部署是在 Github 上可用。 此参考体系结构包括一个虚拟网络 (VNet)、 网络安全组 (NSG)、 负载平衡器和两个虚拟机 (Vm)。

参考体系结构可以按照下面的说明使用 Windows 或 Linux 虚拟机部署︰ 

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-public-dmz-network-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 选择从下拉列表框， **windows**或**linux**的**操作系统类型**。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

3. 等待部署完成。

4. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-public-dmz-wl-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

6. 等待部署完成。

7. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**使用现有**然后输入`ra-public-dmz-network-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

9. 等待部署完成。

10. 参数文件包括硬编码的管理员用户名和密码适用于所有虚拟机，以及强烈建议您立即更改两个。 部署中的每台虚拟机，Azure 门户中将其选中，然后单击**支持 + 疑难解答**刀片式服务器中的上**重设密码**。 在**模式**下拉列表框中，选择**重置密码**，然后选择一个新**的用户名**和**密码**。 单击**更新**按钮来保持不变。


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "安全的混合网络体系结构"
