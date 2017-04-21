<properties
    pageTitle="将复制 Active Directory 域控制器安装在 Azure |Microsoft Azure"
    description="教程，说明如何从内部部署 Active Directory 林 Azure 的虚拟机上安装域控制器。"
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>安装在 Azure 的虚拟网络中复制 Active Directory 域控制器

为内部部署 Active Directory 域 Azure 的虚拟网络中的 Azure 虚拟机 (Vm)，本主题演示如何安装其他域控制器 （也称为副本域控制器）。

您可能会感兴趣这些相关主题︰

-  在 Azure 的虚拟网络上，可以选择安装一个新的活动目录林。 这些步骤，请参阅[安装新的活动目录林中 Azure 的虚拟网络上](../active-directory/active-directory-new-forest-virtual-machine.md)。
-  在 Azure 的虚拟网络上安装 Active Directory 域服务 (AD DS) 的概念性指导，请参阅[部署 Windows 服务器 Active Directory 在 Azure 的虚拟机的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)。


## <a name="scenario-diagram"></a>方案图

在这种情况下，外部用户需要访问在加入域的服务器运行的应用程序。 虚拟机中运行的应用程序服务器并复制副本域控制器安装在 Azure 的虚拟网络。 虚拟的网络可以连接到内部网络的[站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)连接，如下面的关系图中所示，也可以使用[ExpressRoute](../expressroute/expressroute-locations-providers.md)为更快地连接。

应用程序服务器和域控制器部署在单独的云服务来分配计算处理和内改进的容错功能[可用性设置](../virtual-machines/virtual-machines-windows-manage-availability.md)。
域控制器复制使用 Active Directory 复制并与内部部署域控制器。 需要未同步的工具。

![图 pf 复制 Active Directory 域控制器 Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>创建 Azure 的虚拟网络的 Active Directory 站点

最好到虚拟的网络表示对应的网络区域的 Active Directory 中创建网站。 帮助优化身份验证、 复制和其他 DC 的位置操作。 以下步骤说明如何创建一个网站，和有关更多背景信息，请参见[添加新的网站](https://technet.microsoft.com/library/cc781496.aspx)。

1. 打开 Active Directory 站点和服务︰**服务器管理器** > **工具** > **Active Directory 站点和服务**。
2. 创建一个网站来表示您在其中创建 Azure 的虚拟网络的区域︰ 单击**网站** > **操作** > **新站点**> 键入新的站点，如 Azure 美国西部的名称 > 选择站点链接 >**确定**。
3. 创建子网，并将与新网站︰ 双击**站点**> 用鼠标右键单击**子网** > **新子网**> 键入的 IP 地址范围 （例如方案图中的 10.1.0.0/16) 的虚拟网络 > 选择新的 Azure 站点 >**确定**。

## <a name="create-an-azure-virtual-network"></a>创建了一个 Azure 的虚拟网络

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，单击**新建** > **网络服务** > **虚拟网络** > **自定义创建**和使用以下值，以完成该向导。

    在此向导页...  | 指定这些值
    ------------- | -------------
    **虚拟网络详细信息**  | <p>名称︰ 键入虚拟网络 （如 WestUSVNet） 的名称。</p><p>区域︰ 选择最近的区域。</p>
    **DNS 和 VPN 连接**  | <p>DNS 服务器︰ 指定的名称和 IP 地址的一个或多个内部 DNS 服务器。</p><p>连接︰ 选择**站点到站点 VPN 配置**。</p><p>本地网络︰ 指定一个新的本地网络。</p><p>如果您正在使用 ExpressRoute，而不 VPN，请参阅[配置通过 Exchange 提供程序连接到 ExpressRoute](../expressroute/expressroute-locations-providers.md)。</p>
    **站点到站点连接**  | <p>名称︰ 键入内部网络的名称。</p><p>VPN 设备 IP 地址︰ 指定将连接到虚拟网络设备的公用 IP 地址。 VPN 设备不能位于后面 nat。</p><p>地址︰ 指定的地址范围的内部网络 （如方案图中的 192.168.0.0/16)。</p>
    **虚拟网络地址空间**  | <p>地址空间︰ 您想要在 Azure 的虚拟网络 （如方案图中的 10.1.0.0/16) 中运行的虚拟机指定的 IP 地址范围。 此地址范围不能重叠与内部网络的地址范围。</p><p>子网︰ 指定的名称和应用程序服务器的子网的地址 (如前端，10.1.1.0/24) 和 Dc (如后端，10.1.2.0/24)。</p><p>单击**添加子网网关**。</p>

2. 接下来，您将配置虚拟网络网关创建安全的站点到站点 VPN 连接。 有关说明，请参阅[配置虚拟网络网关](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md)。
3. 创建新的虚拟网络和内部部署 VPN 设备之间站点到站点 VPN 连接。 有关说明，请参阅[配置虚拟网络网关](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md)。


## <a name="create-azure-vms-for-the-dc-roles"></a>DC 角色创建 Azure 的虚拟机

重复以下步骤来创建虚拟机主机的 DC 角色根据需要。 应部署至少两个虚拟的域控制器，以提供容错和冗余。 如果 Azure 的虚拟网络将包括至少两个域控制器都同样配置 （即，它们是两个 Gc，运行的 DNS 服务器，和既不持有任何 FSMO 角色等等） 然后将放在为改进的容错设置可用性运行那些域控制器虚拟机。
而不用户界面使用 Windows PowerShell 创建虚拟机，请参阅[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，单击**新建** > **计算** > **虚拟机** > **剪辑库中**。 使用下列值来完成向导。 接受设置的默认值，除非另一个值是建议或要求。

    在此向导页...  | 指定这些值
    ------------- | -------------
    **选择图像**  | Windows Server 2012 R2 数据中心
    **虚拟机配置**  | <p>虚拟机名称︰ 键入单标签名称 （例如 AzureDC1)。</p><p>新用户名称︰ 键入用户的名称。 此用户将 VM 上本地管理员组的成员。 您将需要此名称来第一次登录到虚拟机。 内置命名管理员帐户将无法工作。</p><p>新密码确认︰ 键入密码</p>
    **虚拟机配置**  | <p>云服务︰ 用于第一个 VM 选择<b>创建一个新的云服务</b>，并创建多个虚拟机将承载 DC 角色时选择相同的云服务名称。</p><p>云服务 DNS 名称︰ 指定一个全局唯一名称</p><p>地区/关系组/虚拟网络︰ 指定的虚拟网络名称 （例如 WestUSVNet)。</p><p>为第一个虚拟的存储帐户︰ 选择<b>使用自动生成的存储帐户</b>，然后选择相同的存储帐户名称，当您创建多个虚拟机将承载 DC 角色。</p><p>可用性组︰ 请选择<b>创建可用性设置</b>。</p><p>可用性设置名称︰ 键入可用性设置在创建第一个虚拟机，然后选择时，相同的名称，当您创建多个虚拟机的名称。</p>
    **虚拟机配置**  | <p>选择<b>安装虚拟机代理</b>和任何其他所需的扩展。</p>
2. 将磁盘连接到每个虚拟机将运行的 DC 的服务器角色。 需要额外的磁盘来存储 AD 数据库、 日志和 SYSVOL。 指定的大小 （如 10 GB) 的磁盘，并将**主机缓存首选项**设置为**无**。 有关的步骤，请参阅[如何将附加到 Windows 虚拟机数据磁盘](../virtual-machines/virtual-machines-windows-classic-attach-disk.md)。
3. 您首次登录到虚拟机后，打开**服务器管理器** > **文件和存储服务**来使用 NTFS 该磁盘上创建卷。
4. 将运行的 DC 角色的虚拟机保留静态 IP 地址。 若要保留一个静态的 IP 地址，下载的 Microsoft Web 平台安装程序和[安装 Azure PowerShell](../powershell-install-configure.md) ，运行一组 AzureStaticVNetIP cmdlet。 例如︰

    获得 AzureVM 服务-AzureDC1-命名 AzureDC1 |一组 AzureStaticVNetIP ip 地址 10.0.0.4 |更新 AzureVM

有关设置静态 IP 地址的详细信息，请参阅[配置 vm 内部一个静态 IP 地址](../virtual-network/virtual-networks-reserved-private-ip.md)。

## <a name="install-ad-ds-on-azure-vms"></a>在 Azure 的虚拟机上安装 AD DS

登录到虚拟机并验证已连接到资源的站点到站点 VPN 或 ExpressRoute 连接跨内部网络上。 然后在 Azure 虚拟机上安装 AD DS。 您可以使用相同的过程，用来在您的内部网络 （UI、 Windows PowerShell 或应答文件） 上安装其他 DC。 随着安装 AD DS 时，请确保您指定位置 AD 数据库、 日志和 SYSVOL 的新卷。 如果您需要刷新器在 AD DS 安装，请参阅[安装 Active Directory 域服务 (级别 100)](https://technet.microsoft.com/library/hh472162.aspx)或[安装在现有的域 (级别 200) 复制 Windows Server 2012 域控制器](https://technet.microsoft.com/library/jj574134.aspx)。

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>重新配置虚拟网络的 DNS 服务器

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，单击在虚拟的网络的名称，然后单击**配置**选项卡[的虚拟网络的 DNS 服务器 IP 地址重新配置](../virtual-network/virtual-networks-manage-dns-in-vnet.md)为使用静态 IP 地址分配给复制副本域控制器而不是内部 DNS 服务器的 IP 地址。

2. 若要确保在虚拟网络上的复制副本 DC Vm 配置与虚拟的网络上使用 DNS 服务器，单击**虚拟机**，每台虚拟机，单击状态列，然后单击**重新启动**。 等待，直到 VM 显示之前尝试登录到它的**运行**状态。

## <a name="create-vms-for-application-servers"></a>应用程序服务器创建虚拟机

1. 重复以下步骤来创建虚拟机作为应用程序服务器运行。 接受设置的默认值，除非另一个值是建议或要求。

    在此向导页...  | 指定这些值
    ------------- | -------------
    **选择图像**  | Windows Server 2012 R2 数据中心
    **虚拟机配置**  | <p>虚拟机名称︰ 键入单标签名称 （例如 AppServer1)。</p><p>新用户名称︰ 键入用户的名称。 此用户将 VM 上本地管理员组的成员。 您将需要此名称来第一次登录到虚拟机。 内置命名管理员帐户将无法工作。</p><p>新密码确认︰ 键入密码</p>
    **虚拟机配置**  | <p>云服务︰ 用于第一个 VM 选择**创建一个新的云服务**，并创建多个虚拟机将承载该应用程序时选择相同的云服务名称。</p><p>云服务 DNS 名称︰ 指定一个全局唯一名称</p><p>地区/关系组/虚拟网络︰ 指定的虚拟网络名称 （例如 WestUSVNet)。</p><p>为第一个虚拟的存储帐户︰ 选择**使用自动生成的存储帐户**，当您创建将承载该应用程序的多个虚拟机，然后选择相同的存储帐户名称。</p><p>可用性组︰ 请选择**创建可用性设置**。</p><p>可用性设置名称︰ 键入可用性设置在创建第一个虚拟机，然后选择时，相同的名称，当您创建多个虚拟机的名称。</p>
    **虚拟机配置**  | <p>选择<b>安装虚拟机代理</b>和任何其他所需的扩展。</p>

2. 每个 VM 配置后，请登录并加入到域。 在**服务器管理器**中，单击**本地服务器** > **工作组** > **更改...** 然后选择**域**并键入您的内部域的名称。 提供域用户的凭据，然后重新启动虚拟机以完成加入域。

而不用户界面使用 Windows PowerShell 创建虚拟机，请参阅[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)。

有关使用 Windows PowerShell 的详细信息，请参阅[开始使用 Azure Cmdlet](https://msdn.microsoft.com/library/azure/jj554332.aspx)和[Azure Cmdlet 参考](https://msdn.microsoft.com/library/azure/jj554330.aspx)。

## <a name="additional-resources"></a>其他资源

-  [部署 Windows Azure 的虚拟机服务器 Active Directory 的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [如何通过使用 Azure PowerShell 上载到 Azure 的现有内部部署 Hyper-V 域控制器](http://support.microsoft.com/kb/2904015)
-  [在 Azure 的虚拟网络上安装一个新的活动目录林](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure 的虚拟网络](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) 虚拟机基础知识](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) 创建虚拟网络和跨内部连接](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure 管理 Cmdlet](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
