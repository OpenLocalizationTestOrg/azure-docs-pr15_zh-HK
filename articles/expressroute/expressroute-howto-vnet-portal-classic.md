<properties
   pageTitle="在传统的门户网站中的 ExpressRoute 配置虚拟网络和网关 |Microsoft Azure"
   description="这篇文章将引导您完成为 ExpressRoute 使用传统部署模型和传统门户网站设置的虚拟网络。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>在传统的门户网站 ExpressRoute 创建虚拟网络

这篇文章中的步骤将引导您完成配置虚拟网络和虚拟网络网关用于与使用传统部署模型和经典的门户网站 ExpressRoute。

如果您正在查找有关资源管理器部署模型，您可以使用下列文章︰[创建虚拟网络使用 PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md)和[添加到资源管理器 VNet ExpressRoute 的 VPN 网关](expressroute-howto-add-gateway-resource-manager.md)。

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>创建一个经典的 VNet 和网关

以下步骤将创建传统的 VNet 和虚拟网络网关。 如果您已经拥有经典的 VNet，请参阅本文中[配置现有的经典 VNet](#config)部分。

1. 登录到[Azure 的传统门户网站](http://manage.windowsazure.com)。

2. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。

3. 在**虚拟的网络详细信息**页上，输入以下命令︰

    - **名称**– 虚拟网络名称。 部署您的虚拟机和 PaaS 的实例，因此您可能不想要太复杂的名称时，您将使用此虚拟网络名称。
    - **位置**– 位置直接关系到希望资源 (Vm) 所在的物理位置 （区域）。 例如，如果您希望将 Vm 部署到物理上位于东亚美国此虚拟网络，选择该位置。 不能更改所创建与虚拟网络的地区。

4. 在**DNS 服务器和 VPN 连接**页面上，输入以下信息，然后单击右下方的下箭头。 

    - **DNS 服务器**-输入 DNS 服务器名称和 IP 地址，或从快捷菜单中选择先前已注册的 DNS 服务器。 此设置不会创建一个 DNS 服务器。 它允许您指定要使用的虚拟网络名称解析的 DNS 服务器。
    - **站点到站点连接**的**站点到站点 VPN 配置**为选择复选框。
    - **ExpressRoute** – 选中此复选框，**使用 ExpressRoute**。 只有所选**站点到站点 VPN 配置**时，此选项才会显示。
    - **本地网络**-您都需要有本地网络网站为 ExpressRoute。 但是，如果 ExpressRoute 连接，将忽略指定为本地网络站点的地址前缀。 相反，将为进行路由而使用 ExpressRoute 电路通过公布给 Microsoft 的地址前缀。<BR>如果已创建的 ExpressRoute 连接本地网络，可以从下拉列表中选择。 否则，请选择**指定一个新的本地网络**。

5. 如果您选择在上一步中指定新的本地网络，此时将显示**站点对站点连接**页上。 要配置您的本地网络，请输入以下信息，然后单击向下箭头。 

    - **名称**-想要调用本地 （内部部署） 的名称的网络站点。
    - **地址空间**-包括起始 IP 和 CIDR （地址计数）。 只要它不与虚拟网络的地址范围重叠，您可以指定任何地址范围。 通常，这会指定的地址范围，为您的内部网络，但 ExpressRoute，如果不使用这些设置。 但是，此设置需要创建本地网络时使用的经典的门户。
    - **添加地址空间**-此设置不适用于 ExpressRoute。


6. 在**虚拟的网络地址空间**页上，输入以下信息，然后单击右下方以配置您的网络上的选中标记。 

    - **地址空间**-包括启动 IP 和地址计数。 请验证您指定的地址空间不重叠任何您有在您的本地网络的地址空间。
    - **添加子网**-包括起始 IP 和地址数。 其他的子网不是必需的。
    - **添加网关网**-单击以添加网关网。 网关网仅用于虚拟网络网关和对于这种配置是必需的。<BR>网关网 ExpressRoute 为 CIDR （地址数） 必须是 /28 或更大 (/ 27 日/26 等等。)。 这样足够该子网中的 IP 地址来允许配置工作。 在经典的门户中，如果选择了此复选框可使用 ExpressRoute，门户网站指定一个网关的子网 /28。  不能调整中经典的门户的 CIDR 地址计数。 网关网将作为**网关**在经典的门户中，虽然创建网关网的实名实际上**GatewaySubnet**。 通过使用 PowerShell 或在 Azure 的门户网站中，您可以查看此名称。

7. 单击页面底部的选中标记和虚拟网络将开始创建。 完成后，您将看到**创建**传统门户中的**网络**页上列出**状态**下。

## <a name="gw"></a>创建网关

1. 在**网络**页面中，单击刚创建的虚拟网络然后单击页面顶部的**仪表板**。

2. 在**仪表板**页的底部，单击**创建网关**和**动态路由**选择。 单击**是**以确认您想要创建一个网关。

3. 网关在启动时创建，您将看到消息让您知道网关已启动。 它可能需要最多 45 分钟来创建网关。

4. 将您的网络链接到电路。 请按照本文[如何链接到 ExpressRoute 电路的 VNets](expressroute-howto-linkvnet-classic.md)中的说明进行操作。

## <a name="config"></a>配置现有的经典 VNet ExpressRoute

如果您已经拥有经典的 VNet，您可以配置以连接到 ExpressRoute 中经典的门户。 设置是相同的部分上面，因此通读这些部分熟悉所需的设置。 如果您想要创建共存 ExpressRoute/站点到站点连接，请参阅[本文](expressroute-howto-coexist-classic.md)的步骤。 它们是不同于本文中的步骤操作。
 
1. 您需要更新您的 VNet 设置的其余部分之前创建本地网络。 单击**新建**以创建新的本地网络，通过经典的门户配置 ExpressRoute 时是必需的**>****网络服务** **>** **虚拟网络** **>** **添加本地网络**。 按照向导步骤创建本地网络。

2. 使用**配置**页来更新您的 VNet 设置的其余部分并关联到本地网络 VNet。

3. 配置设置后，请转至本文创建网关的[创建网关](#gw)部分。


## <a name="next-steps"></a>下一步行动

- 如果您想要添加到虚拟网络的虚拟机，请参阅[虚拟机的学习途径](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)。
- 如果您想要了解有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 概述](expressroute-introduction.md)。


 
