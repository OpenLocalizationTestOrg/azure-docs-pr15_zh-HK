<properties
    pageTitle="在部署 Azure 堆栈 POC 之前 |Microsoft Azure"
    description="查看 Azure 堆栈 POC （服务管理员） 的环境和硬件要求。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Azure 堆栈部署系统必备组件

部署 Azure 堆栈 POC （[概念](azure-stack-poc.md)） 之前，请确保您的计算机满足以下要求。
POC 的技术预览 2 部署要求是相同的所需的技术预览 1。 因此，您可以使用相同的硬件用于以前的单框预览。

## <a name="hardware"></a>硬件

| 组件 | 最小值  | 建议 |
|---|---|---|
| 磁盘驱动器︰ 操作系统 | 1 个操作系统磁盘与 200 gb 可用系统分区 （SSD 或硬盘） 的最小值 | 1 个操作系统磁盘与 200 gb 可用系统分区 （SSD 或硬盘） 的最小值 |
| 磁盘驱动器︰ Azure 堆栈 POC 的一般数据 | 4 个磁盘。 每个磁盘提供 140 GB 的容量 （SSD 或硬盘） 最小。 将使用所有可用的磁盘。 | 4 个磁盘。 每个磁盘提供最少的 250 GB 的容量 （SSD 或硬盘）。 将使用所有可用的磁盘。|
| 计算︰ CPU | 双插座︰ 12 物理核心 （总数）  | 双插座︰ 16 个物理核心 （总数） |
| 计算︰ 内存 | 96 GB RAM  | 128 GB RAM |
| 计算︰ BIOS | Hyper-V 启用 （含 SLAT 支持）  | Hyper-V 启用 （含 SLAT 支持） |
| 网络︰ 网卡 | Windows Server 2012 R2 认证所需的 NIC。没有所需的特殊的功能 | Windows Server 2012 R2 认证所需的 NIC。没有所需的特殊的功能 |
| 硬件徽标认证 | [Windows Server 2012 r2 认证](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2012 r2 认证](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**数据的磁盘驱动器配置︰**所有数据驱动器必须都是同一类型 （所有 SAS 或 SATA） 和容量。 如果使用 SAS 磁盘驱动器，则必须通过单个路径 （提供没有 MPIO，多路径支持） 连接磁盘驱动器。

**HBA 配置选项**
 
- （首选）简单的 HBA
- 必须在"通过"模式下配置 RAID HBA-适配器
- 磁盘应配置为 RAID HBA-单磁盘，RAID 0

**受支持的总线和媒体类型组合**

-   SATA 硬盘

-   SAS 硬盘

-   RAID 硬盘

-   RAID SSD (如果介质类型是未指定的未知\*)

-   SATA SSD + SATA 硬盘

-   SAS SSD + SAS 硬盘

\*没有直通功能的 RAID 控制器无法识别的媒体类型。 这种控制器会将硬盘和 SSD 标记为未指定。 在这种情况下，SSD 会用作持久性存储区而不是缓存设备。 因此，您可以部署 Microsoft Azure 堆栈 POC 那些 SSDs 上。

**示例 Hba**: LSI 9207 8i、 LSI 9300 8i 或 LSI 9265-8i 直通模式

OEM 配置示例是可用的。

## <a name="operating-system"></a>操作系统

| | **要求**  |
|---|---|
| **操作系统版本** | Windows Server 2012 R2 或更高版本。 操作系统版本并不重要之前部署启动后，当您将引导至 VHD 安装 zip Azure 堆栈中包含的主机。 操作系统和所有必需的修补程序已集成到图像中。 不要使用任何键来激活 POC 中使用任何 Windows 服务器实例。|

## <a name="deployment-requirements-check-tool"></a>部署要求检查工具

在安装操作系统之后, 可以使用[Azure 堆栈技术预览 2 部署检查](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b)以确认您的硬件满足所有的要求。



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory 帐户

Microsoft Azure 堆栈 POC 部署必须连接到 Azure。 因此，您必须运行部署 PowerShell 脚本之前准备 Microsoft Azure Active Directory 帐户。 此帐户为 Azure Active Directory 租户成为全局管理员。 它将用来提供和委派的应用程序和与 Azure Active Directory 和图形 API 进行交互的所有堆栈 Azure 服务的服务主体。 它还将用作默认提供程序订阅 （这可以在以后更改） 的所有者。 您可以使用此帐户登录到 Azure 堆栈系统的管理门户。

1. 创建至少一个 Azure Active Directory 目录管理员 Azure 的广告帐户。 如果您已经有一个，您可以使用的。 否则，您可以创建一个免费在[http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) （在中国，请访问<http://go.microsoft.com/fwlink/?LinkID=717821>相反。）

    在步骤 6 中的[运行 PowerShell 部署脚本](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script)保存为使用这些凭据。 此*服务管理员*帐户可以配置和管理资源群、 用户帐户、 组织计划、 配额和定价。 在门户中，它们可以创建的网站群，虚拟机的私有云，创建计划，并管理用户订阅。

2. [创建](azure-stack-add-new-user-aad.md)至少一个帐户，以便您可以登录到 Azure 堆栈 POC 作为租户。

  	| **Azure Active Directory 帐户**  | **支持？** |
  	|---|---| 
  	| 具有有效公共 Azure 订阅的工作或学校帐户  | 是的 |
  	| 有效的公共 Azure 订阅 Microsoft 帐户  | 不 |
  	| 具有有效的中国 Azure 订阅的工作或学校帐户  | 是的 |
  	| 工作或学校与有效美国政府 Azure 订阅的帐户  | 是的 |


## <a name="network"></a>网络

### <a name="switch"></a>开关

POC 机连接的交换机上的一个可用端口。  

Azure 堆栈 POC 机器支持连接到交换机的访问端口或中继端口。 没有专门的功能需要在交换机上。 如果您正在使用的中继端口或您需要配置 VLAN ID，您需要提供的 VLAN ID 作为部署参数。 您可以看到示例[部署参数列表](azure-stack-run-powershell-script.md)中。

### <a name="subnet"></a>子网

不能连接到下面的子网的 POC 计算机︰
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

这些子网被保留用于 Microsoft Azure 堆栈 POC 环境内的内部网络。

### <a name="ipv4ipv6"></a>IPv4/IPv6

仅 IPv4 支持。 不能创建 IPv6 网络。

### <a name="dhcp"></a>DHCP

请确保没有 DHCP 服务器可用的网卡连接到网络上。 如果没有 DHCP，必须准备额外的静态 IPv4 网络除了使用的主机。 您必须提供该 IP 地址和网关作为部署参数。 您可以看到示例[部署参数列表](azure-stack-run-powershell-script.md)中。

### <a name="internet-access"></a>互联网访问

Azure 堆栈需要接入互联网，直接或通过透明代理。 Azure 堆栈不支持启用 Internet 访问 web 代理服务器的配置。 主机 IP 和新的 IP （通过 DHCP 或静态 IP） 分配给 MAS BGPNAT01 必须能够访问互联网。 在 graph.windows.net 和 login.windows.net 域下使用端口 80 和 443。

### <a name="telemetry"></a>遥测

为了支持遥测数据流量，必须打开您的网络中端口 443 (HTTPS)。 客户端终结点是 https://vortex-win.data.microsoft.com。


## <a name="next-steps"></a>下一步行动

[下载的 Azure 堆栈 POC 部署程序包](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[部署 Azure 堆栈 POC](azure-stack-run-powershell-script.md)
