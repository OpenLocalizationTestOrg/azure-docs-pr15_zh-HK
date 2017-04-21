<properties 
   pageTitle="配置两个虚拟网络间的 VPN 连接 |Microsoft Azure" 
   description="了解如何配置 VPN 连接和域之间两个 Azure 的虚拟网络名称解析，以及如何配置 HBase geo 复制。" 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>配置两个 Azure 的虚拟网络间的 VPN 连接  

> [AZURE.SELECTOR]
- [配置 VPN 连接](hdinsight-hbase-geo-replication-configure-vnets.md)
- [配置 DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [配置 HBase 复制](hdinsight-hbase-geo-replication.md) 

Azure 的虚拟网络站点对站点连接使用的 VPN 网关来提供使用 Ipsec/IKE 安全隧道。 VNets 可以在不同的订阅和不同的地区。 您甚至可以到 VNet 通信具有多站点配置组合 VNet。 有几个 VNet 与 VNet 连接的原因︰

- 跨区域地理冗余和地理状态 
- 而强大的隔离能力的边界区域的多层应用程序 
- 跨组织间通信在 Azure 中的订阅

有关详细信息，请参阅[配置 VNet 连接到 VNet](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。 

若要查看该视频上︰

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

本教程是属于[系列][hdinsight-hbase-replication]上创建 HBase geo 复制。 

- 配置 VPN 连接之间有两个虚拟网络 （本教程）
- [将 DNS 配置为虚拟网络][hdinsight-hbase-geo-replication-dns]
- [配置 HBase geo 复制][hdinsight-hbase-geo-replication]

下图说明了您在本教程中将创建两个虚拟网络︰

![HDInsight HBase 复制虚拟网络图][img-vnet-diagram]
 

##<a name="prerequisites"></a>系统必备组件
在开始本教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **带有 Azure PowerShell 的工作站**。

    运行之前 PowerShell 脚本，请确保您已连接到您使用以下 cmdlet 的 Azure 订购︰

        Add-AzureAccount

    如果您有多个 Azure 的订阅，可以使用以下 cmdlet 设置当前订阅︰

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Azure 服务名称和虚拟机名称必须是唯一的。 在本教程中使用的名称是 Contoso-[Azure 服务虚拟机名称]-[欧盟 / 美国]。 例如，Contoso-VNet-欧盟是 Azure 的虚拟网络中北美欧洲数据中心;Contoso DNS 美国是东亚的美国数据中心中的 DNS 服务器虚拟机。 您必须拿出自己的名称。
 

##<a name="create-two-azure-vnets"></a>创建两个 Azure VNets



**若要创建虚拟网络称为 Contoso-VNet-EU 北美欧洲**

1.  登录到[Azure 的传统门户网站][azure-portal]。
2.  单击**新建**、**网络服务**、**虚拟网络**、**创建自定义**。
3.  输入︰

    - **名称**︰ Contoso-VNet-EU
    - **位置**︰ 北欧

        本教程使用北美欧洲和东亚的美国数据中心。 您可以选择您自己的数据中心。
4.  输入︰

    - **DNS 服务器**: （留空） 
    
        您需要自己的 DNS 服务器进行名称解析，在虚拟的网络中。 有关何时使用 Azure 提供名称解析和何时使用 DNS 服务器的详细信息，请参阅[域名解析 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。 配置名称解析 VNets 之间的说明，请参阅[配置两个 Azure 的虚拟网络之间的 DNS][hdinsight-hbase-dns]。
  
    - **点到站点 VPN 配置**: （未选中）

        这种情况下不适用于点到网站。

    - **配置站点到站点 VPN**: （未选中）
    
        您将在东亚的美国数据中心配置站点到站点 VPN 连接到 Azure 的虚拟网络。
5.  输入︰

    -   **地址空间起始 IP**: 10.1.0.0
    -   **地址空间 CIDR**: / 16
    -   **子网 1 起始 IP**: 10.1.0.0
    -   **CIDR 子网 1**: / 24

    在美国的虚拟网络，地址空间不能重叠。  

**若要创建虚拟网络称为 Contoso-VNet-EU 西欧洲**

- 重复最后一个过程具有以下值︰

    - **名称**︰ Contoso-VNet-美国
    - **位置**︰ 东亚美国
     
    - **DNS 服务器**: （留空）
    - **点到站点 VPN 配置**: （未选中）
    - **配置站点到站点 VPN**: （未选中）
     
    - **地址空间起始 IP**: 10.2.0.0
    - **地址空间 CIDR**: / 16
    - **子网 1 起始 IP**: 10.2.0.0
    - **CIDR 子网 1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>配置两个 VNets 之间的 VPN 连接

###<a name="create-local-networks"></a>创建本地网络

当您创建 VNet 到 VNet 的配置时，您需要配置每个 VNet 相互识别为本地网络站点。 在本节中，您将为本地网络配置每个 VNet。 本地网络与相应的 VNet 共享同一个 IP 地址空间。

![配置 Azure VPN 站点到站点配置-azure 的本地网络][img-vnet-lnet-diagram]


**若要创建本地网络称为 Contoso-VNet-欧盟网络地址空间匹配的 Contoso-LNet-欧盟**

1. 从 Azure 的传统门户网站，单击**新建**、**网络服务**、**虚拟网络**、**添加本地网络**。
3. 输入︰

    - **名称**︰ Contoso-LNet 欧盟
    - **VPN 设备 IP 地址**︰ 192.168.0.1 （该地址将在以后更新）

        通常情况下，您将使用 VPN 设备实际的外部 IP 地址。 对于 VNet VNet 的配置，您将使用 VPN 网关的 IP 地址。 假设您具有的 VPN 网关的两个 VNets 尚未创建，您输入任意 IP 地址并回来来修复它。
4.  输入︰

    - **地址空间起始 IP:** 10.1.0.0
    - **地址空间 CIDR:** /16
    
    这必须完全对应 Contoso-VNet-欧盟为前面所指定的范围。

**若要创建本地网络称为 Contoso-VNet-美国网络地址空间匹配的 Contoso-LNet-美国**

- 重复最后一个过程使用以下参数︰

    - **名称**︰ Contoso LNet 美国
    - **VPN 设备 IP 地址**︰ 192.168.0.1 （该地址将在以后更新）
     
    - **地址空间起始 IP**: 10.2.0.0
    - **地址空间 CIDR**: / 16


###<a name="create-vpn-gateways"></a>创建 VPN 网关

在此配置中有两个部分。 您在配置 VNet 站点到站点连接到本地网络，第一次，然后创建动态路由 VPN。 对 VNet VNet 要求与动态路由 Vpn 的 Azure VPN 网关。 不支持 azure 的静态路由选择 Vpn。

**若要配置与 Contoso LNet 美国 Contoso-VNet-EU 站点对站点连接**

1.  从 Azure 传统门户网站，在左侧的窗格中，单击**网络**
2.  单击**Contoso-VNet-欧盟**。
3.  单击**CONFIGUE**选项卡。
4.  请检查**连接到本地网络**。
5.  在**本地网络**中，选择**Contoso LNet 美国**。
6.  单击**添加网关的子网**中的虚拟网络地址空间部分。
7.  单击**保存**。
8.  单击**确定**以确认。


**若要为 Contoso-VNet-欧盟创建的 VPN 网关**

1.  从 Azure 传统门户网站，请单击**仪表板**选项卡。
4.  单击底部的页，**创建网关**，然后单击**动态路由**。
5.  单击**是**确认。 注意到在页上的网关图形更改为黄色而称创建网关。 通常要花费大约 15 分钟来创建网关。

    当网关状态更改为连接时，每个网关的 IP 地址将显示在仪表板中。 每个 VNet，注意不要混淆了它们写下对应的 IP 地址。 这些都是您的本地网络中的 VPN 设备编辑占位符 IP 地址时，将使用的 IP 地址。

6.  制作一份**网关 IP 地址**。 将用于为 Contoso-VNet-欧盟在下一节中配置 VPN 网关的 IP 地址。

**若要为 Contoso-VNet-欧盟创建的 VPN 网关**

- 重复最后两个过程来配置 Contoso-Vnet-美国对 Contoso LNet 欧盟和创建的 VPN 网关的 Contoso-VNet-美国站点对站点连接。 操作完成后，您将拥有 Contoso-VNet-美国 VPN 网关的 IP 地址。


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>设置 VPN 设备的本地网络 IP 地址
在最后一节中，您创建的 VPN 网关 VNets 的每个。 有了 VPN 网关的 IP 地址。 现在您可以重新配置局域网 VPN 设备 IP 地址。

**Contoso-LNet 欧盟为配置 VPN 设备的 IP 地址** 

1.  从 Azure 经典门户，左侧窗格中单击**网络**。
2.  从顶部单击**本地网络**。
3.  单击**Contoso-LNet 欧盟**，然后再单击底部的**编辑**。
4.  更新**VPN 设备的 IP 地址**。  这是来自 Contoso-VNET-欧盟的仪表板选项卡上的地址。
5.  单击右侧的按钮。
6.  单击检查按钮。

**若要配置 VPN 设备 IP 地址为 Contoso LNet 美国** 

- 重复最后一个过程为 Contoso LNet 美国配置 VPN 设备的 IP 地址。

###<a name="set-vnet-gateway-keys"></a>设置 VNet 网关的键

Vnet 网关使用共享的密钥身份验证的虚拟网络之间的连接。 不能从 Azure 经典门户配置密钥。 您必须使用 PowerShell 或.NET SDK。

**若要设置键**

1. 从工作站，打开**Windows PowerShell ISE**或 Windows PowerShell 控制台。
2. 更新此按照脚本中的参数，然后运行它︰

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>检查 VPN 连接 

没有部署到 VNets 的任何虚拟机，可以使用虚拟网络可视化图 VNet 仪表板页在 Azure 传统门户网站来检查连接状态︰

![HDInsight HBase 复制虚拟网络 VPN 连接状态][img-vpn-status]
  



##<a name="next-steps"></a>下一步行动

在本教程中，您已了解如何配置两个 Azure 的虚拟网络间的 VPN 连接。 该系列中的其他两篇文章介绍的内容︰

- [两个 Azure 的虚拟网络之间配置 DNS][hdinsight-hbase-geo-replication-dns]
- [配置 HBase geo 复制][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
