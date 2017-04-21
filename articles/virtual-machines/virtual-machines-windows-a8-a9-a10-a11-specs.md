<properties
 pageTitle="有关 windows 计算密集型 Vm |Microsoft Azure"
 description="获得背景信息和对于 Windows 虚拟机和云服务使用 Azure H 系列和 A8、 A9、 A10 和 A11 计算密集型大小注意事项"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>关于 H 系列和计算密集型的 A 系列虚拟机

这里是背景信息和使用较新的 Azure H 系列和早期 A8、 A9、 A10 和 A11 实例，也称为*计算密集型*实例的一些注意事项。 本文着重于对 Windows 虚拟机使用这些实例。 这篇文章也是适用于[Linux 虚拟机](virtual-machines-linux-a8-a9-a10-a11-specs.md)。


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>RDMA 网络访问权限

您可以创建 RDMA 支持 Windows 服务器实例的群集和部署支持的 MPI 实现，利用 Azure RDMA 网络之一。 此低延迟、 高吞吐量的网络被保留的 MPI 通信流。

* **操作系统**
    * **虚拟机**-Windows Server 2012 R2，Windows Server 2012
    * **云服务**的 Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2 来宾操作系统家族

* **MPI**的 Microsoft MPI (MS-MPI) 2012 R2 或更高版本，英特尔 MPI 库 5.x

受支持的 MPI 实现使用 Microsoft 网络直接接口实例之间进行通信。 请参阅[设置 HPC 包运行 MPI 应用程序的 Windows RDMA 群集](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)并[使用多实例任务运行在 Azure 批处理中的消息传递接口 (MPI) 应用程序](../batch/batch-mpi.md)的部署选项和配置的示例步骤。


>[AZURE.NOTE]RDMA 能够计算密集型 Vm，在 HpcVmDrivers 扩展名必须添加到要安装 Windows 网络设备驱动程序所需的 RDMA 连接的 Vm。 在大多数部署中，将自动添加扩展名为 HpcVmDrivers。 如果您需要自己添加扩展名，请参阅[管理虚拟机的扩展](virtual-machines-windows-classic-manage-extensions.md)。

## <a name="considerations-for-hpc-pack-and-windows"></a>HPC 包和窗口时的注意事项

[Microsoft HPC 包](https://technet.microsoft.com/library/jj899572.aspx)，Microsoft 的免费 HPC 群集和作业管理解决方案，不需要您使用 Windows 服务器的计算密集型实例。 但是，它是一个选项可以在 Azure 创建计算群集运行 MPI 基于 Windows 的应用程序和其他 HPC 的工作负载。 HPC 包 2012 R2 及更高版本包括一个运行时环境可以使用 RDMA 支持虚拟机的 Azure RDMA 网络部署时的 MS MPI。

有关详细信息和计算密集型实例使用 HPC 在 Windows 服务器上的包的清单，请参阅[设置 HPC 包运行 MPI 应用程序的 Windows RDMA 群集](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)。




## <a name="next-steps"></a>下一步行动

* 有关可用性和价格计算密集型大小的详细信息，请参阅[虚拟机定价](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)和[云服务定价](https://azure.microsoft.com/pricing/details/cloud-services/)。

* 存储容量和磁盘的详细信息，请参阅[虚拟机大小](virtual-machines-linux-sizes.md)。

* 若要开始部署和使用在 Windows HPC 包计算密集型实例，请参阅[设置 HPC 包运行 MPI 应用程序的 Windows RDMA 群集](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)。

* 有关使用 A8 和 A9 实例使用 Azure 批处理运行 MPI 应用程序的信息，请参阅[使用多实例任务运行在 Azure 批处理中的消息传递接口 (MPI) 应用程序](../batch/batch-mpi.md)。
