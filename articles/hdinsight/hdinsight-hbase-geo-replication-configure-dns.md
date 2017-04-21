<properties 
   pageTitle="两个 Azure 的虚拟网络之间配置 DNS |Microsoft Azure" 
   description="了解如何配置 VPN 连接和域两个虚拟网络，之间的名称解析，以及如何配置 HBase geo 复制。" 
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

# <a name="configure-dns-between-two-azure-virtual-networks"></a>两个 Azure 的虚拟网络之间配置 DNS

> [AZURE.SELECTOR]
- [配置 VPN 连接](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [配置 DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [配置 HBase 复制](hdinsight-hbase-geo-replication.md) 


了解如何添加和配置到 Azure 的虚拟网络来处理虚拟机内和跨虚拟网络名称解析的 DNS 服务器。

本教程是该[系列]的第二部分[hdinsight-hbase-geo-replication]上创建 HBase geo 复制︰

- [配置两个虚拟网络间的 VPN 连接][hdinsight-hbase-geo-replication-vnet]
- 将 DNS 配置为虚拟网络 （本教程）
- [配置 HBase geo 复制][hdinsight-hbase-geo-replication]


下图说明了在[配置两个虚拟网络间的 VPN 连接]中创建两个虚拟网络[hdinsight-hbase-geo-replication-vnet]:

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

- **两个 Azure 使用 VPN 连接的虚拟网络**。  有关说明，请参阅[配置两个 Azure 的虚拟网络间的 VPN 连接][hdinsight-hbase-geo-replication-vnet]。

>[AZURE.NOTE] Azure 服务名称和虚拟机名称必须是唯一的。 在本教程中使用的名称是 Contoso-[Azure 服务虚拟机名称]-[欧盟 / 美国]。 例如，Contoso-VNet-欧盟是 Azure 的虚拟网络中北美欧洲数据中心;Contoso DNS 美国是在东亚的美国数据中心的虚拟机的 DNS 服务器。 您必须拿出自己的名称。
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>创建要用作 DNS 服务器的 Azure 虚拟机

**若要创建虚拟机 Contoso-VNet-欧盟内称为 Contoso-DNS-EU**

1.  单击**新建**、**计算**、**虚拟机**、**从库**。
2.  请选择**Windows Server 2012 R2 数据中心**。
3.  输入︰
    - **虚拟机名称**︰ Contoso-DNS-EU
    - **新的用户名**︰ 
    - **新密码**︰ 
4.  输入︰
    - **云服务**︰ 创建一个新的云服务
    - **地区/关系组/虚拟网络**: （选择 Contoso-VNet-欧盟）
    - **虚拟的网络子网**︰ 子网 1
    - **存储帐户**︰ 使用自动生成的存储帐户
    
        云服务名称将虚拟机名称相同。 在这种情况下，这就是 Contoso-DNS-欧盟。 对于后续的虚拟机，我可以选择使用相同的云服务。  在同一个云服务下的所有虚拟机都共享同一个虚拟网络和域后缀。

        存储帐户用于存储虚拟机的图像文件。 
    - **终结点**: （向下滚动并选择**DNS**） 

创建虚拟机后，找出内部 IP 和外部的 IP。

1.  单击虚拟机的名称， **Contoso 的 DNS-欧盟**。
2.  单击**仪表板**。
3.  记下︰
    - 公用虚拟 IP 地址
    - 内部 IP 地址


**若要创建虚拟机在 Contoso VNet 美国称为 Contoso DNS 美国** 

- 重复相同的步骤来使用下面的值创建虚拟机︰
    - 虚拟机名称︰ Contoso DNS 美国
    - 地区/关系组/虚拟网络︰ 选择 Contoso-VNET-美国
    - 虚拟的网络子网︰ 子网 1
    - 存储帐户︰ 使用自动生成的存储帐户
    - 终结点: （选择 DNS）

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>设置静态 IP 地址用于两个虚拟机

DNS 服务器需要静态 IP 地址。  此步骤不能完成从 Azure 经典门户。 您将使用 Azure PowerShell。

**若要配置静态 IP 地址用于两个虚拟机**

1. 打开 Windows PowerShell ISE。
2. 运行以下 cmdlet。  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    服务是云服务名称。 因为该 DNS 服务器是第一个虚拟机的云服务，云服务名称是虚拟机名称相同。

    您可能需要更新服务和名称以匹配您的名称。


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>添加 DNS 服务器角色这两个虚拟机

**Contoso-DNS-欧盟为添加 DNS 服务器角色**

1.  从 Azure 传统门户网站，请单击左边的**虚拟机**。 
2.  单击**Contoso-DNS-欧盟**。
3.  从顶部单击**仪表板**。
4.  从底部单击**连接**并按照说明进行操作以连接到虚拟机通过 RDP。
2.  在 RDP 会话中，单击左下角打开开始屏幕上的窗口按钮。
3.  单击**服务器管理器**拼贴。
4.  单击**添加角色和功能**。
5.  单击**下一步**
6.  选择**基于角色或功能的安装**，，然后单击**下一步**。
7.  选择 DNS 虚拟机 （它应突出显示已），，然后单击**下一步**。
8.  检查**DNS 服务器**。
9.  单击**添加功能**，然后单击**继续**。
10. 单击三次**下, 一步**，然后单击**安装**。 

**Contoso DNS 美国为添加 DNS 服务器角色**

- 重复步骤向**Contoso DNS 美国**添加 DNS 角色。

##<a name="assign-dns-servers-to-the-virtual-networks"></a>将 DNS 服务器分配给虚拟网络

**要注册的两个 DNS 服务器**

1.  从 Azure 的传统门户网站，单击**新建**、**网络服务**、**虚拟网络**、**注册 DNS 服务器**。
2.  输入︰
    - **名称**︰ Contoso-DNS-EU
    - **DNS 服务器的 IP 地址**︰ 10.1.0.4--的 IP 地址必须与 DNS 服务器虚拟机的 IP 地址匹配。
     
3.  重复此过程以注册 Contoso DNS 美国具有以下设置︰
    - **名称**︰ Contoso DNS 美国
    - **DNS 服务器的 IP 地址**︰ 10.2.0.4

**若要为两个虚拟网络分配两个 DNS 服务器**

1.  在门户中经典的左侧窗格中，单击**网络**。
2.  单击**Contoso-VNet-欧盟**。
3.  单击**配置**。
4.  在**dns 服务器**部分中选择**Contoso-DNS-欧盟**。
5.  单击底部的页**将保存**，请单击**是**确认。
6.  重复此过程以将**Contoso DNS 美国**DNS 服务器分配给**Contoso-VNet-美国**虚拟网络。

已部署到虚拟的网络中的所有虚拟机都必须重新都启动更新的 DNS 服务器配置。

**若要重新启动虚拟机**

1. 从 Azure 传统门户网站，请单击左边的**虚拟机**。
2. 单击**Contoso-DNS-欧盟**。
3. 从顶部单击**仪表板**。
4. 单击底部的**重新启动**。
5. 重复相同的步骤来重新启动**Contoso DNS 美国**。


##<a name="configure-dns-conditional-forwarders"></a>配置 DNS 条件转发器

每个虚拟的网络上的 DNS 服务器只能解决该虚拟网络中的 DNS 名称。 您需要配置为指向对等虚拟网络中的名称解析的对等 DNS 服务器的条件转发器。

若要配置条件转发器，您需要知道两个 DNS 服务器的域后缀。 可以取决于云服务配置时创建虚拟机使用不同的 DNS 后缀。 为了在虚拟的网络中使用的每个 DNS 后缀，您必须添加条件转发器。 

**若要查找两个 DNS 服务器的域后缀**

1. **Contoso-DNS-欧盟**到 RDP。
2. 打开 Windows PowerShell 控制台或命令提示符。
3. 运行**ipconfig**，然后记下**特定于连接的 DNS 后缀**。
4. 请不要关闭 RDP 会话，您仍需要它。 
5. 重复相同的步骤，查明**Contoso DNS 美国****特定连接的 DNS 后缀**。


**要配置 DNS 转发器**
 
1.  从**Contoso-DNS-EU**的 RDP 会话，请单击左下角的 Windows 键。
2.  单击**管理工具**。
3.  单击**DNS**。
4.  在左窗格中，展开**DSN**， **Contoso 的 DNS-欧盟**。
5.  输入以下信息︰
    - **DNS 域**︰ 输入 Contoso DNS 美国的 DNS 后缀。 例如︰ Contoso-DNS-US.b5.internal.cloudapp.net。
    - **主服务器的 IP 地址**︰ 输入 10.2.0.4，这是 Contoso DNS-美国的 IP 地址。
6.  按**enter 键**，并单击**确定**。  现在您将能够解决从 Contoso-DNS-EU Contoso DNS-美国的 IP 地址。
7.  重复步骤将 DNS 转发器添加到 DNS 服务在 Contoso DNS 美国虚拟机上使用下面的值︰
    - **DNS 域**︰ 输入在 Contoso DNS 欧盟的 DNS 后缀。 
    - **主服务器的 IP 地址**︰ 输入 10.2.0.4，这是对 Contoso-DNS-欧盟的 IP 地址。

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>在虚拟的网络测试名称解析

现在您可以在虚拟的网络测试主机名称解析。 默认情况下，Ping 禁止通过防火墙。  可以使用 nslookup 解析对等网络中的 DNS 服务器虚拟机 （必须使用 FQDN）。  


##<a name="next-steps"></a>下一步行动

在本教程中，您学习了如何通过 VPN 连接与虚拟网络配置名称解析。 该系列中的其他两篇文章介绍的内容︰

- [配置两个 Azure 的虚拟网络间的 VPN 连接][hdinsight-hbase-geo-replication-vnet]
- [配置 HBase geo 复制][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
