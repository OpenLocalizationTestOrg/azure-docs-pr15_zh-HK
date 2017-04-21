<properties
 pageTitle="云服务的大小 |Microsoft Azure"
 description="列出了不同的虚拟机大小 （和 Id） Azure 的云服务 web 和辅助角色。"
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>云服务的大小

本主题介绍可用的大小和云服务角色实例 （web 角色和辅助角色） 的选项。 它还提供部署考虑事项时应注意的规划利用这些资源。 每个尺寸都有将[服务定义文件](cloud-services-model-and-package.md#csdef)中的 ID。

云服务是通过 Azure 提供计算资源的几种类型之一。 单击[此处](cloud-services-choose-me.md)为云服务的详细信息。

> [AZURE.NOTE]若要查看相关 Azure 的限制，请参阅[Azure 订阅和服务限制、 配额和约束](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Web 和辅助角色实例的大小

有多个标准大小，要从 Azure 上进行选择。 这些尺寸的注意事项包括︰

* D 系列虚拟机用于运行的应用程序要求更高的计算能力和临时磁盘性能。 D 系列虚拟机提供更快的处理器、 更高的内存到核率和固态驱动器 (SSD) 的临时磁盘。 有关详细信息，参见公告在 Azure 的博客中，[新的 D 系列虚拟机大小](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)

* Dv2 系列，原来的 D 系列，后续功能更强大的 CPU。 Dv2 系列 CPU 为大约比 D 系列的 CPU 速度快 35%。 基于新一代 2.4 g h z 英特尔 Xeon® E5 2673 v3 (Haswell) 处理器，并使用英特尔 Turbo Boost 技术 2.0 版可以转达 3.1 g h z。 Dv2 系列有 D 系列相同的内存和磁盘配置。

*   G 系列虚拟机提供的大多数内存，在具有英特尔至强 E5 V3 系列处理器主机上运行。

*   A 系列虚拟机可以在不同的硬件类型和处理器上进行部署。 被遏制的大小，取决于硬件，提供一致的处理器性能的正在运行的实例，而不考虑部署的硬件。 若要确定在其部署此大小的物理硬件，请查询从虚拟机中的虚拟硬件。

*   A0 大小是在物理硬件上过度订阅。 仅此特定大小，对于其他客户部署可能会影响您正在运行的工作负载的性能。 为预期的基线，大约 15%的变化受以下概述了的相对性能。


虚拟机的大小会影响价格。 大小也会影响虚拟机的处理、 内存和存储容量。 分别基于存储成本计算存储帐户中使用的页上。 有关详细信息，请参阅[虚拟机定价详细信息](https://azure.microsoft.com/pricing/details/virtual-machines/)和[Azure 存储定价](https://azure.microsoft.com/pricing/details/storage/)。 


以下注意事项可能会帮助您决定在大小︰


* A8 A11 和 H 系列大小也被称为*计算密集型实例*。 运行这些大小的硬件设计和优化的计算密集型和网络密集型应用程序，其中包括高性能计算 (HPC) 群集的应用程序、 建模和模拟。 A8 A11 系列使用英特尔至强 E5-2670年@2.6 g h Z 和 H 系列使用英特尔至强 E5 2667 v3 @ 3.2 g h z。 有关详细的信息以及有关使用这些大小注意事项，请参阅[关于 H 系列和计算密集型 A 系列虚拟机](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)。 

* Dv2 系列、 D 系列、 G 系列，非常适合应用程序要求更快的 Cpu、 更好的本地磁盘的性能，或具有更高的内存需求。  它们提供了功能强大的组合对于许多企业级应用程序。

*   一些 Azure 数据中心中的物理主机可能不支持虚拟机就越大，例如 A5 – A11。 因此，可能时，将显示错误消息**来配置虚拟机 {机器名称} 的失败**或**未能创建虚拟机 {机器名称}**调整到新的大小; 现有虚拟机在 2013 年 4 月 16 日; 之前创建的虚拟网络中创建新的虚拟机或将新的虚拟机添加到现有的云服务。 请参阅[错误:"无法配置虚拟机"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows)替代方法为每个部署方案的支持论坛上。  

* 您的订购还可能会限制您可以部署某些大小家族的内核数。 若要增加配额，请联系 Azure 支持。


## <a name="performance-considerations"></a>性能注意事项

我们已经创建的 Azure 计算单元 (ACU) 提供一种跨 Azure Sku 比较计算 (CPU) 性能的概念。 这将帮助您轻松识别这些 SKU 是最有可能满足您的性能要求。  ACU 当前标准化上一个小 (Standard_A1) 然后在 100 和所有其他 Sku 的 VM 表示大约如何更快地 SKU 可以运行标准的准则。 

>[AZURE.IMPORTANT] ACU 是只是一个准则。  您的工作负载的结果可能会有所不同。 

<br>

|SKU 系列 |ACU/核心 |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210-250 *|
|[G1-5](#g-series)  |180-240 *|
|[H](#h-series) |290 至 300 *|

标有 ACUs * 使用 Intel® Turbo 技术来提高 CPU 频率并提供性能大大提高。  根据 VM 大小、 工作量和其他在同一主机上运行的工作负载变化量大大提高。

## <a name="size-tables"></a>调整表大小

下表显示的大小和它们提供的容量。

* 存储容量所示钩或 1024年单位 ^3 个字节。 当比较磁盘以 GB 为单位 (1000年 ^3 个字节) 到磁盘以钩 (1024年 ^3) 记忆容量钩中给出的数字会较小。 例如，1023年钩 = 1098.4 GB

* 磁盘吞吐量以每秒 (IOPS) 的输入/输出操作 mbps 其中 MBps = 10 ^6 个字节/秒。

* 数据磁盘可以在缓存或不使用高速缓存模式下操作。 对于缓存的数据磁盘操作，主机缓存模式设置为**只读**或**读写**。  对于不使用高速缓存的数据磁盘操作，主机缓存模式被设置为**None**。

* 最大的网络带宽是最大的聚合的带宽分配，分配每个虚拟机的类型。 最大带宽提供了指南，选择正确的 VM 类型，以确保足够的网络容量才可用。 低、 中、 高和极高之间移动，吞吐量将会相应地增加。 实际网络性能取决于许多因素，包括网络和应用程序加载和应用程序网络设置。


## <a name="a-series"></a>A 系列

| 大小        | CPU 内核 | 内存︰ 钩 | 本地硬盘︰ 钩 | 最大数据磁盘 | 最大数据磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1/低                   |
| Standard_A1 | 1         | 1.75         | 70                    | 2              | 2 x 500              | 1/中等              |
| Standard_A2 | 2         | 3.5 GB       | 135                   | 4              | 4 x 500              | 1/中等              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2/高                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4/高                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1/中等              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2/高                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4/高                  |

## <a name="a-series---compute-intensive-instances"></a>A 系列的计算密集型实例

有关信息以及有关使用这些大小注意事项，请参阅[关于 H 系列和计算密集型 A 系列虚拟机](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)。


| 大小         | CPU 内核 | 内存︰ 钩 | 本地硬盘︰ 钩 | 最大数据磁盘 | 最大数据磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2/高                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4/非常高             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2/高                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4/非常高             |

* RDMA 支持

## <a name="d-series"></a>D 系列


| 大小         | CPU 内核 | 内存︰ 钩 | 本地的 SSD︰ 钩 | 最大数据磁盘 | 最大数据磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5 英寸          | 50                   | 2              | 2 x 500              | 1/中等              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2/高                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4/高                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8/高                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2/高                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4/高                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8/高                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8/非常高             |

## <a name="dv2-series"></a>Dv2 系列

| 大小            | CPU 内核 | 内存︰ 钩 | 本地的 SSD︰ 钩 | 最大数据磁盘 | 最大数据磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5 英寸          | 50                   | 2              | 2 x 500              | 1/中等              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2/高                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4/高                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8/高                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8/极高        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2/高                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4/高                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8/高                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8/极高        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8/极高        |

## <a name="g-series"></a>G 系列

| 大小        | CPU 内核 | 内存︰ 钩  | 本地的 SSD︰ 钩  | 最大数据磁盘 | 最大磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1/高                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2/高                  |
| Standard_G3 | 8         | 112          | 1536                | 16             | 16 x 500           | 4/非常高             |
| Standard_G4 | 16        | 224          | 3072                | 32             | 32 x 500           | 8/极高        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8/极高        |


## <a name="h-series"></a>H 系列

Azure H 系列虚拟机是下一代高性能计算 Vm 面向高端计算需求，如分子建模和计算流体动力学。 这些 8 和 16 的核心虚拟机基于英特尔 Haswell E5 2667 V3 处理器技术而采用 DDR4 内存和本地基于 SSD 存储。 

除了大量的 CPU 功率，H 系列提供低延迟 RDMA 网络使用 FDR 无限带宽和几种内存配置来支持内存密集型计算需求的不同选项。


| 大小           | CPU 内核 | 内存︰ 钩 | 本地的 SSD︰ 钩 | 最大数据磁盘 | 最大磁盘吞吐量︰ IOPS | 最大的 Nic / 网络带宽 |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8/高                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8/非常高                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8/高                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8/非常高                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8/非常高                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8/非常高                  |


* RDMA 支持

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>注意︰ 标准 A0 A4 使用 CLI 和 PowerShell 

在经典的部署模型中，某些虚拟机大小名称是在 CLI 和 PowerShell 稍有不同︰

* Standard_A0 是 ExtraSmall 
* Standard_A1 是小
* Standard_A2 是媒介
* Standard_A3 是大
* Standard_A4 是 ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>为云服务配置的大小

您可以指定角色实例的虚拟机大小为[服务定义文件](cloud-services-model-and-package.md#csdef)所描述的服务模型的一部分。 角色的大小确定 CPU 内核、 内存容量和本地文件系统的大小分配给正在运行的实例数。 选择角色的大小基于应用程序的资源要求。

下面是一个示例设置角色大小， [Standard_D2](#general-purpose-d)用于 Web 角色实例︰

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>下一步行动

- 请查阅[azure 订阅和服务限制、 配额和限制](../azure-subscription-service-limits.md)。
- 了解更多[关于 H 系列和计算密集型 A 系列虚拟机](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)等高性能计算 (HPC) 的工作负载。

