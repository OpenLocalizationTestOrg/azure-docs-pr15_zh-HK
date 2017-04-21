<properties
pageTitle="如何更新云服务 |Microsoft Azure"
description="了解如何更新在 Azure 的云服务。 了解如何在云服务上的更新将继续确保可用性。"
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>如何更新云服务

## <a name="overview"></a>概述
在 10000 英尺，更新一个云服务，包括其角色和访客操作系统，是三个步骤。 首先，必须上载二进制文件和配置文件以使用新的云服务或操作系统的版本。 接下来，Azure 保留根据最新的云服务版本的需求的云服务的计算和网络资源。 最后，Azure 执行滚动升级增量租户更新到新版本或访客操作系统，同时保留您的可用性。 本文讨论了这一步--滚动升级的详细信息。

## <a name="update-an-azure-service"></a>更新 Azure 服务

Azure 将角色实例组织成逻辑组称为升级域 (UD)。 升级域 (UD) 是作为一组更新的角色实例的逻辑集合。  Azure 的云更新一次，它允许实例在其他 UDs 继续服务于通信服务一个 UD。

升级域的默认数量为 5。 通过该服务定义文件 (.csdef) 中包括 upgradeDomainCount 属性，可以指定升级域的数量不同。 有关 upgradeDomainCount 属性的详细信息，请参阅[WebRole 架构](https://msdn.microsoft.com/library/azure/gg557553.aspx)或[WorkerRole 架构](https://msdn.microsoft.com/library/azure/gg557552.aspx)。

当您执行就地更新的一个或多个角色服务中时，Azure 更新根据它们属于升级域角色实例集。 Azure 更新所有实例在给定的升级域 – 停止它们，对它们进行更新，让它们重新联机 – 然后移动到下一个域。 停止当前的升级域中运行的实例，通过 Azure 将确保更新时，尽量少影响到正在运行的服务。 有关详细信息，请参阅本文后面的[更新的方式进行](#howanupgradeproceeds)。

> [AZURE.NOTE] 术语**更新**和**升级**Azure 的上下文中有略有不同的含义，而这些可互换的进程和功能在此文档中的说明。

您的服务必须定义为要更新该角色的角色至少两个实例进行而无需停机。 如果服务包含的一个角色只有一个实例，您的服务将无法使用，直到就地更新已完成。

本主题介绍有关 Azure 更新的以下信息︰

-   [在更新期间允许服务更改](#AllowedChanges)
-   [如何升级过程](#howanupgradeproceeds)
-   [更新的回滚](#RollbackofanUpdate)
-   [启动多个变异操作正在进行部署](#multiplemutatingoperations)
-   [通过升级域角色的分布](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>在更新期间允许服务更改
下表显示了更新过程中对服务的允许的更改︰

|更改允许承载、 服务和角色|就地更新|分阶段 （VIP 交换）|删除和重新部署|
|---|---|---|---|
|操作系统版本|是的|是的|是的
|.NET 的信任级别|是的|是的|是的|
|虚拟机大小<sup>1</sup>|是<sup>2</sup>|是的|是的|
|本地存储区设置|提高仅<sup>2</sup>|是的|是的|
|在添加或删除角色服务|是的|是的|是的|
|某个特定角色的实例数|是的|是的|是的|
|服务终结点的数目或类型|是<sup>2</sup>|不|是的|
|配置设置的名称和值|是的|是的|是的|
|值 （而不是名称） 的配置设置|是的|是的|是的|
|添加新的证书|是的|是的|是的|
|更改现有证书|是的|是的|是的|
|部署新代码|是的|是的|是的|
<sup>1</sup>限制为云服务的可用大小的子集的大小变化。

<sup>2</sup>要求 Azure SDK 1.5 或更高版本。

> [AZURE.WARNING] 更改虚拟机大小将会破坏本地数据。


在更新期间不支持以下各项︰

-   更改角色的名称。 删除并添加具有新名称的角色。
-   更改的域中升级计数。
-   减小大小的本地资源。

如果要将其他更新向您的服务定义，如减少大小的本地资源，必须执行一个 VIP 交换更新。 有关详细信息，请参阅[替换部署](https://msdn.microsoft.com/library/azure/ee460814.aspx)。

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>如何升级过程
您可以决定您是否要更新所有服务中的角色或服务中的一个角色。 在任一情况下，正在升级，属于第一个升级域的每个角色的所有实例都停止、 升级，并重新联机。 一旦他们回到联机状态，第二个升级的域中的实例将停止、 升级，和重新联机。 云服务可以有最多一次升级一次活动。 根据最新版的云服务始终执行升级。

下图说明如何升级将继续如果您要升级所有服务中的角色︰

![升级服务](media/cloud-services-update-azure-service/IC345879.png "升级服务")

这下图说明了如果您要升级单个角色更新的进行︰

![升级角色](media/cloud-services-update-azure-service/IC345880.png "升级角色")  

> [AZURE.NOTE] 从单个实例升级服务，到多个实例时您的服务将被转入执行升级时由于方式 Azure 的升级服务。 Guaranteeing 服务可用性服务级别协议只适用于具有多个实例部署的服务中。 下面的列表描述了每个 Azure 服务升级方案如何影响每个驱动器上的数据︰
>
>虚拟机重新启动︰
>
-   C︰ 保留
-   D︰ 保留
-   E︰ 保留
>
>门户重新启动︰
>
-   C︰ 保留
-   D︰ 保留
-   E︰ 销毁
>
>门户重新映像︰
>
-   C︰ 保留
-   D︰ 销毁
-   E︰ 销毁

>原位升级︰
>
-   C︰ 保留
-   D︰ 保留
-   E︰ 销毁
>
>节点迁移︰
>
-   C︰ 销毁
-   D︰ 销毁
-   E︰ 销毁

>注意，在上面的列表中，e︰ 驱动器表示该角色的根驱动器中，并且不应硬编码。 相反，使用 %roleroot%环境变量来表示该驱动器。

>要升级单个服务时最小化停机时间，将一种新的多实例服务部署到临时服务器并执行 VIP 交换。

在一次自动更新，Azure 结构控制器定期评估无法确定何时可以安全地指导下一步 UD 云服务的运行状况。 此运行状况评估并执行在每个角色的基础上，认为只有最新版本 （即从已了解的 UDs 的实例） 中的实例。 它会验证角色实例，对于每个角色，最少已获得令人满意的终端状态。

### <a name="role-instance-start-timeout"></a>角色实例启动超时
结构控制器将等待 30 分钟每个角色实例，以达到已启动状态。 如果超时时间到期，结构控制器将继续遍历到的下一个角色实例。

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>更新的回滚
Azure 可以灵活地更新的过程中管理服务，让您在初始更新请求接受 Azure 结构控制器之后启动服务，对其他操作。 只能更新 （配置更改） 时执行回滚或升级是在部署上的**正在进行**状态。 更新或升级被视为正在进行中，只要还有尚未更新到最新版本的服务至少一个实例。 要测试是否允许回滚，请检查 RollbackAllowed 标志，[获取部署](https://msdn.microsoft.com/library/azure/ee460804.aspx)和[获得云服务属性](https://msdn.microsoft.com/library/azure/ee460806.aspx)的操作所返回的值，设置为 true。

> [AZURE.NOTE] 仅有意义上**就地**更新调用回滚或升级因为 VIP 交换升级涉及一个整个运行服务实例替换为另一个。

回滚正在进行更新的已部署如下影响︰

-   其中有尚未更新或升级到最新版本的任何角色实例不更新或升级，原因是这些实例已在运行该服务的目标版本。
-   它具有已更新或升级到新版本的服务包的任何角色实例 (\*.cspkg) 文件或服务配置 (\*.cscfg) 文件 （或这两个文件） 将还原为升级前这些文件的版本。

此功能由提供以下功能︰

-   [回滚更新或升级](https://msdn.microsoft.com/library/azure/hh403977.aspx)操作，只要还有尚未更新到最新版本的服务中至少一个实例可以称为上 （由调用[更改部署配置](https://msdn.microsoft.com/library/azure/ee460809.aspx)） 配置更新或升级 （通过调用[升级部署](https://msdn.microsoft.com/library/azure/ee460793.aspx)触发）。
-   锁定元素和 RollbackAllowed 元素，[获取部署](https://msdn.microsoft.com/library/azure/ee460804.aspx)和[获得云服务属性](https://msdn.microsoft.com/library/azure/ee460806.aspx)操作的响应正文的一部分返回的︰
    1.  锁定元素允许您检测变异操作可以调用在给定部署时。
    2.  RollbackAllowed 元素允许您检测时可以调用给定部署[回滚更新或升级](https://msdn.microsoft.com/library/azure/hh403977.aspx)操作。

    为了执行回滚，您不必检查锁定和 RollbackAllowed 元素。 足够了确认 RollbackAllowed 被设置为 true。 如果使用请求标头设置为调用这些方法才会返回这些元素"x-ms-版本︰ 2011年-10-01"或更高版本。 有关版本控制标头的详细信息，请参阅[服务管理版本控制](https://msdn.microsoft.com/library/azure/gg592580.aspx)。

有某些情况下在回滚更新或不支持升级，这些优点如下︰

-   减少了本地资源-如果更新增加角色 Azure 平台的本地资源不允许回滚。 
-   配额限制的规模下操作可能不再更新是否具有足够的计算配额来完成回滚操作。 每个 Azure 的预订已指定的内核可由属于该订阅的所有托管服务的最大数目的配额与之相关联。 如果执行更新给定的回滚操作会使您的订阅配额，然后通过回滚将不会启用。
-   争用条件-如果初始更新已完成，回滚不是可能的。

更新的回滚可能有用的一个示例是如果您在手动模式下控制主要的就地升级到您 Azure 的承载服务的速度推出使用[升级部署](https://msdn.microsoft.com/library/azure/ee460793.aspx)操作。

在升级的部署阶段您在手动模式下调用[升级部署](https://msdn.microsoft.com/library/azure/ee460793.aspx)，并开始逐步升级的域。 如果在某些时候，作为您监视升级、 您注意到您检查第一个升级域中的某些角色实例无法响应，可以调用[回滚更新或升级](https://msdn.microsoft.com/library/azure/hh403977.aspx)操作的部署，有尚未升级的实例和回滚实例必须已升级到以前服务打包和配置将保持不变。

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>启动多个变异操作正在进行部署
在某些情况下可能要启动多个并行的变异操作正在进行部署。 例如，您可以执行的服务更新，该更新被推出跨您的服务，而您想要做某些更改，例如若要回滚该更新后，应用不同的更新，或甚至删除部署。 在其中，这可能是一种必要情况是如果服务升级包含错误的代码，这会导致升级的角色实例反复崩溃。 在这种情况下，Azure 结构控制器将不能升级，因为所升级的域中的实例数目不足是正常在应用中取得进展。 这种状态被称为*卡部署*。 您可以通过回滚该更新，或者失败的顶部应用最新的更新 unstick 部署之一。

一旦通过 Azure 结构控制器已收到最初请求更新或升级服务，您可以启动后续变异操作。 也就是说，无需等待初始操作完成之前可以启动另一个变异操作。

正在进行第一次更新时启动第二个更新操作将执行类似于回滚操作。 如果第二个更新是在自动模式下，第一个升级域将升级立即，可能会导致实例，从多个升级域在同一点离线时间。

变异操作如下︰[配置更改部署](https://msdn.microsoft.com/library/azure/ee460809.aspx)、[升级部署](https://msdn.microsoft.com/library/azure/ee460793.aspx)、[更新部署状态](https://msdn.microsoft.com/library/azure/ee460808.aspx)、[删除部署](https://msdn.microsoft.com/library/azure/ee460815.aspx)和[回滚更新或升级](https://msdn.microsoft.com/library/azure/hh403977.aspx)。

两个操作，[获取部署](https://msdn.microsoft.com/library/azure/ee460804.aspx)和[获得云服务属性](https://msdn.microsoft.com/library/azure/ee460806.aspx)，返回一个可以对其进行检查，以确定是否可以在给定部署上调用变异操作的锁定标志。

若要调用这些方法返回锁定标志的版本，必须将请求标头设置为"x-ms-版本︰ 2011年-10-01"或更高版本。 有关版本控制标头的详细信息，请参阅[服务管理版本控制](https://msdn.microsoft.com/library/azure/gg592580.aspx)。

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>通过升级域角色的分布
Azure 间升级的域，可配置服务定义 (.csdef) 文件的一部分作为一组数均匀地分配角色的实例。 升级域的最大数目为 20，默认值为 5。 有关如何修改服务定义文件的详细信息，请参阅[Azure 服务定义架构 (.csdef 文件)](cloud-services-model-and-package.md#csdef)。

例如，如果您的角色有十个实例，默认情况下每个升级域包含两个实例。 如果您的角色有 14 实例，然后四个升级域包含三个实例，和第五个域包含两个。

使用从零开始的索引来标识升级域︰ 第一个升级域的 ID 为 0，并且第二个升级域的 ID 为 1，依次类推。

下图演示时该服务定义了两个升级的域不包含这两个角色服务的分布方式。 八个 web 角色实例和九个辅助角色实例，则该服务正在运行。

![升级域的通讯组](media/cloud-services-update-azure-service/IC345533.png "升级域的通讯组")

> [AZURE.NOTE] 请注意，Azure 控件实例升级域之间的分配方式。 不能指定哪些实例被分配到哪个域。

## <a name="next-steps"></a>下一步行动
[如何管理云服务](cloud-services-how-to-manage.md)  
[如何监控云服务](cloud-services-how-to-monitor.md)  
[如何配置云服务](cloud-services-how-to-configure.md)  
