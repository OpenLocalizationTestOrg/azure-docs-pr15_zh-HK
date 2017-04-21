<properties 
   pageTitle="Azure 来宾操作系统可支持性和退休政策指南 |Microsoft Azure" 
   description="提供有关哪些 Microsoft 支持作为方面到 Azure 来宾操作系统使用云服务的信息。" 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure 的来宾操作系统可支持性和退休政策
在此页上的信息与 Azure 来宾操作系统 ([来宾 OS](cloud-services-guestos-update-matrix.md)) 云服务工作人员和 web 角色 (PaaS)。 它不适用于虚拟机 (IaaS)。 

Microsoft 已发布[支持来宾操作系统的策略](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq)。 现在正在阅读的页面介绍了如何实现策略。

策略 

1. Microsoft 将支持**至少来宾操作系统的最新两个系列**。 当退休族时，客户可以从正式退休日期更新为较新的受支持来宾操作系统家族的 12 个月。
2. Microsoft 将支持**至少到受支持的来宾操作系统家族的最新两个版本**。 
3. Microsoft 将支持在**Azure SDK 的最新最少两个版本**。 当 SDK 版本报废以后时，客户可以从正式退休日期更新为较新版本的 12 个月。 

有时多两个家族或版本可能受支持。 官方的来宾操作系统支持信息将出现在[Azure 来宾 OS 版本和 SDK 兼容性矩阵](cloud-services-guestos-update-matrix.md)。


## <a name="when-a-guest-os-family-or-version-is-retired"></a>当来宾操作系统家族或版本被退休 


Windows 服务器操作系统的新官方版本发行后一段时间来引入新来宾操作系统**系列**。 只要引入新的来宾操作系统系列，Microsoft 将淘汰旧的来宾操作系统系列。 

有关合并的最新更新的 MSRC 每月引入了新访客操作系统**版本**。 由于定期每月更新，来宾操作系统版本是通常禁用的发布后的 60 天。 这将保持为可用于每个家族至少两个来宾操作系统版本。 

### <a name="process-during-a-guest-os-family-retirement"></a>在来宾操作系统家族退休的过程 


一旦宣布退休时，客户将有 12 个月"过渡"期间之前正式从服务中删除较旧的家族。 此过渡时间可能会延长的 Microsoft 自己决定。 在[Azure 来宾 OS 版本和 SDK 兼容性矩阵](cloud-services-guestos-update-matrix.md)上发布更新。

逐步退休过程将开始过渡期为 6 个月。 在这段时间︰

1. Microsoft 将通知客户的退休。 
2. Azure SDK 的较新版本不支持废弃的来宾操作系统家族。
3. 新的部署和重新部署一下而已，云服务将不允许在废弃的家族

Microsoft 将继续引入新来宾 OS 版本包含最新的 MSRC 更新，直到最后一天的过渡期内，称为"到期日期"。 届时，将在 Azure SLA 下支持任何云服务仍在运行。 Microsoft 已应自行决定要强制升级、 删除或在该日期之后停止这些服务。



### <a name="process-during-a-guest-os-version-retirement"></a>在来宾操作系统版本退休的过程 
如果客户设置自动更新其来宾 OS，他们永远不需要担心处理访客操作系统版本。 它们将始终使用最新的来宾操作系统版本。

来宾操作系统版本发布每月。 由于定期发行的收益率，而每个版本都有固定的使用期限。

在 60 天内，使用期限到版本是"*禁用*"。 "已禁用"，则意味着版本从 Azure 的传统门户网站。 它还可以不再设置从 CSCFG 配置文件。 现有部署保持运行，但不是会允许新的部署和现有部署的代码和配置更新。 

在以后的某个时间、 来宾 OS 版本"*过期*"和任何安装仍在运行该版本是强制升级并设置为自动更新来宾操作系统在将来。 过期是分批进行，因此从禁用的到期时间内可以改变。 

这些时间可能拉长 Microsoft 的决定，以减轻客户过渡。 [Azure 来宾 OS 版本和 SDK 兼容性矩阵](cloud-services-guestos-update-matrix.md)，将传达任何更改。



### <a name="notifications-during-retirement"></a>在退休期间的通知 

* **家庭退休** <br>Microsoft 将使用博客张贴内容和 Azure 的经典门户通知。 通过对分配的服务管理员直接通信 （电子邮件、 门户的邮件、 电话联络），客户仍在使用过时的来宾操作系统家族将收到通知。 所有更改将都过帐到此页面，此页面的开头列出的 RSS 源。 


* **版退休** <br>所有更改将都过帐到此页，此页，包括发行版中，禁用和过期日期的开始部分列出的 RSS 源。 服务管理员将收到电子邮件，如果他们有家人或禁用的来宾操作系统版本上运行的部署。 这些电子邮件的时间可能会有所不同。 通常它们至少是一个月之前禁用，但这个时间不是正式的 SLA。 


## <a name="frequently-asked-questions"></a>常见问题及的解答

**如何减少迁移的影响？**

您应该使用最新访客操作系统系列设计云服务。 

1. 开始规划尽早迁移到较新的家人。 
2. 设置临时测试部署，测试新系列上运行云服务。 
3. 将来宾 OS 版本设置为**自动**(osVersion = * [.cscfg](cloud-services-model-and-package.md#cscfg)文件中) 以便迁移到新的访客操作系统版本会自动发生。

**如果我的 web 应用程序需要与操作系统的更深度集成？**

如果您的 web 应用程序体系结构需要更深的依赖底层操作系统，支持使用平台的功能，如[启动任务](cloud-services-startup-tasks.md)或者将来可能存在的其他扩展性机制。 或者，您还可以使用[Azure 虚拟机](https://azure.microsoft.com/documentation/scenarios/virtual-machines/)(IaaS — 基础结构即服务)，您是负责维护基础操作系统。
 
## <a name="next-steps"></a>下一步行动
查看最新[访客操作系统释放](cloud-services-guestos-update-matrix.md)。
