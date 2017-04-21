<properties
    pageTitle="在 Azure 的虚拟网络上安装 Active Directory 林 |Microsoft Azure"
    description="教程，说明如何在 Azure 虚拟网络上的虚拟机 (VM) 上创建一个新的活动目录林。"
    services="active-directory, virtual-network"
    keywords="活动目录虚拟机，安装活动目录林中，azure 的活动目录的视频 "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>在 Azure 的虚拟网络上安装一个新的活动目录林

本主题演示如何在 Azure 的虚拟网络的虚拟机 (VM) 上创建一个新的 Windows 服务器的 Active Directory 环境在[Azure 的虚拟网络](../virtual-network/virtual-networks-overview.md)。 在这种情况下，Azure 的虚拟网络未连接到内部网络。

您可能会感兴趣这些相关主题︰

- 该视频说明了这些步骤，请参阅[如何安装活动目录林中 Azure 的虚拟网络上](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- （可选） 您可以[配置站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) ，然后或者安装新的目录林或扩展到 Azure 的虚拟网络的内部林。 这些步骤，请参阅[安装副本活动目录域控制器在 Azure 虚拟网络](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)。
-  在 Azure 的虚拟网络上安装 Active Directory 域服务 (AD DS) 的概念性指导，请参阅[部署 Windows 服务器 Active Directory 在 Azure 的虚拟机的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)。

## <a name="scenario-diagram"></a>方案图

在这种情况下，外部用户需要访问在加入域的服务器运行的应用程序。 安装在他们自己的云服务 Azure 的虚拟网络中安装虚拟机中运行的应用程序服务器和运行域控制器虚拟机。 它们也包括在为改进的容错设置可用性。

![在 Azure 虚拟网络中的虚拟机上的活动目录林中][1]7
## <a name="how-does-this-differ-from-on-premises"></a>这有所不同内部有如何？

没有大区别与内部对 Azure 上安装域控制器。 下表中列出的主要区别。

若要配置...  | 内部部署  | Azure 的虚拟网络
------------- | -------------  | ------------
**域控制器的 IP 地址**  | 分配静态 IP 地址的网络适配器属性   | 运行一组 AzureStaticVNetIP 用于分配一个静态 IP 地址
**DNS 客户端解析程序**  | 在网络上的域成员的适配器属性设置首选和备用 DNS 服务器的地址   | 将 DNS 服务器地址设置虚拟网络属性
**活动目录数据库存储**  | （可选） 从 C:\ 更改的默认存储位置  | 您需要更改默认存储位置从 C:\



## <a name="create-an-azure-virtual-network"></a>创建了一个 Azure 的虚拟网络

1. 登录到 Azure 的传统门户网站。
2. 创建虚拟网络。 单击**网络** > **创建虚拟网络**。 使用下表中的值以完成该向导。

    在此向导页...  | 指定这些值
    ------------- | -------------
    **虚拟网络详细信息**  | <p>名称︰ 输入虚拟网络的名称</p><p>区域︰ 选择最近的区域</p>
    **DNS 和 VPN**  | <p>DNS 服务器为空</p><p>请不要选择任何一个 VPN 选项</p>
    **虚拟网络地址空间**  | <p>子网名称︰ 输入您的子网的名称</p><p>起始 IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>创建虚拟机运行的域控制器和 DNS 服务器角色

重复以下步骤来创建虚拟机主机的 DC 角色根据需要。 应部署至少两个虚拟的域控制器，以提供容错和冗余。 如果 Azure 的虚拟网络将包括至少两个域控制器都同样配置 （即，它们是两个 Gc，运行的 DNS 服务器，和既不持有任何 FSMO 角色等等） 然后将放在为改进的容错设置可用性运行那些域控制器虚拟机。

而不用户界面使用 Windows PowerShell 创建虚拟机，请参阅[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)。

1. 在经典的门户中，单击**新建** > **计算** > **虚拟机** > **剪辑库中**。 使用下列值来完成向导。 接受设置的默认值，除非另一个值是建议或要求。

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

## <a name="install-windows-server-active-directory"></a>安装 Windows 服务器活动目录

使用同一例程使用内部部署[安装 AD DS](https://technet.microsoft.com/library/jj574166.aspx) （即，可以使用用户界面、 应答文件或 Windows PowerShell）。 您需要提供管理员凭据才能安装新的林。 要指定 Active Directory 数据库、 日志和 SYSVOL 的位置，将从操作系统驱动器改向 VM 附加的其他数据磁盘的默认存储位置。

DC 安装完成后，重新连接到虚拟机并登录到域控制器上。 请记住，若要指定域凭据。

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>重置 Azure 的虚拟网络的 DNS 服务器

1. 重置新的 DC/DNS 服务器上的 DNS 转发器设置。
  1. 在服务器管理器中，单击**工具** > **DNS**。
  2. 在**DNS 管理器中**，用鼠标右键单击 DNS 服务器的名称，然后单击**属性**。
  3. 在**转发器**选项卡上单击转发器的 IP 地址，然后单击**编辑**。  选择 IP 地址，然后单击**删除**。
  4. 单击**确定**以关闭编辑器和**确定**以关闭 DNS 服务器属性。
2. 更新 DNS 服务器设置为虚拟网络。
  1. 单击**虚拟网络**> 双击您创建的虚拟网络 >**配置** > **DNS 服务器**，键入名称和一个运行 DC/DNS 服务器角色的虚拟机的 DIP 和单击**保存**。
  2. 选择虚拟机并单击**重新启动**，才能触发 VM 使用新的 DNS 服务器的 IP 地址配置 DNS 解析程序设置。


## <a name="create-vms-for-domain-members"></a>创建为域成员虚拟机

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


## <a name="see-also"></a>请参见

-  [如何在 Azure 的虚拟网络上安装一个新的活动目录林](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [部署 Windows Azure 的虚拟机服务器 Active Directory 的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [配置站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [安装在 Azure 的虚拟网络中复制 Active Directory 域控制器](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) 虚拟机基础知识](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) 创建虚拟网络和跨内部连接](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [虚拟网络概述](../virtual-network/virtual-networks-overview.md)
-  [如何安装和配置 Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Cmdlet 引用](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [设置 Azure VM 静态 IP 地址](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [如何为 Azure VM 分配静态 IP](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [安装一个新的活动目录林](https://technet.microsoft.com/library/jj574166.aspx)
-  [活动目录域服务 (AD DS) 虚拟化 (级别 100) 简介](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
