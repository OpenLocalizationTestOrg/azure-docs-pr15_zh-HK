<properties
 pageTitle="云服务部署问题的疑难解答 |Microsoft Azure"
 description="有几个部署到 Azure 的云服务时可能会遇到的常见问题。 这篇文章提供了一些解决方案。"
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>云服务部署问题的疑难解答

当您部署到 Azure 的云服务应用程序包时，您可以从 Azure 门户中的**属性**窗格中获得有关部署的信息。 可以使用此窗格中的详细信息以帮助您解决问题的云服务中，并打开一个新的支持请求时，您可以为 Azure 支持提供此信息。

**属性**窗格中可以找到，如下所示︰

* 在 Azure 的门户中，单击部署云服务，**所有设置**，请单击，然后单击**属性**。
* 在 Azure 经典门户中，单击部署云服务，单击**仪表板**，位于右下角的页 （显示在**快速浏览**）。 请注意此窗格中是没有的"属性"标签。

> [AZURE.NOTE] 通过单击右上角的窗格中的图标，可以将**属性**窗格中的内容复制到剪贴板。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>问题︰ 我不能访问我的网站，但我的部署已启动并且所有角色实例都都准备好

显示在门户网站 URL 链接不包含端口。 网站的默认端口为 80。 如果您的应用程序被配置为运行在不同的端口，您必须正确的端口号添加到 URL 访问该网站时的情况。

1. 在 Azure 的门户中，单击您的云服务的部署。
2. 在 Azure 门户的**属性**窗格中，检查角色实例 （在**输入终结点**） 的端口。
3. 如果端口不是 80，添加到 URL 正确的端口值，当您访问该应用程序。 若要指定非默认端口，请键入 URL 后, 跟冒号 （:） 后, 跟端口号，不能有空格。

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>问题︰ 回收而无需我做任何我角色实例

康复服务时将自动检测问题节点 Azure，并因此将角色实例移动到新的节点。 在这种情况下，您可能会看到您的角色实例自动回收。 若要查找发生是否康复服务︰

1. 在 Azure 的门户中，单击您的云服务的部署。
2. 在 Azure 门户的**属性**窗格中，查看信息并确定是否康复服务期间观察到回收的角色的时间发生。

角色还回收大约每月一次在主机操作系统和来宾操作系统更新的过程。  
有关详细信息，请参阅博客文章[角色实例重新启动由于 OS 升级](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>问题︰ 我不能做 VIP 交换，收到一个错误

如果在部署更新正在进行，则不允许 VIP 交换。 部署更新可以自动进行时︰

* 新的访客操作系统也可被配置为自动更新。
* 康复服务时发生。

确定是否自动更新阻止您进行 VIP 交换︰

1. 在 Azure 的门户中，单击您的云服务的部署。
2. 在 Azure 门户的**属性**窗格中查看**状态**的值。 如果**准备就绪**后，请检查**最后一次操作**，以查看如果一个最近发生了变化，可能会阻止 VIP 交换。
3. 重复步骤 1 和 2 用于生产部署。
4. 如果自动更新过程中，请等待其完成，然后再尝试做 VIP 交换。

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>问题︰ 角色实例循环之间已启动、 初始化、 忙碌，并已停止

这种情况可能表示您的应用程序代码、 文件包或配置文件有问题。 在这种情况下，您应该能够查看状态更改每隔几分钟，Azure 的门户可能说像**回收**、**忙**或**正在初始化**。 这表明，没有让角色实例中运行的应用程序出现问题。

有关如何解决此问题的详细信息，请参阅博客文章[Azure PaaS 计算诊断数据](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)和[常见的问题，导致回收的角色](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)。

## <a name="problem-my-application-stopped-working"></a>问题︰ 我的应用程序停止工作

1. 在 Azure 的门户中，请单击角色实例。
2. 在 Azure 门户的**属性**窗格中，请考虑以下条件来解决问题︰
   * 如果最近已停止的角色实例 （可以检查**中止计数**的值），可能会更新部署。 等待，查看是否角色实例将继续运行在它自己。
   * 如果角色实例正**忙**，请检查应用程序代码，以查看是否已处理[StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck)事件。 您可能需要添加或修复一些代码来处理该事件。
   * 经过诊断数据并在[Azure PaaS 计算诊断数据](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)撰写的博客文章中的故障排除方案。

>[AZURE.WARNING] 如果回收站云服务，您属性重置为部署，有效地清除原始问题的信息。

## <a name="next-steps"></a>下一步行动

查看更多的[疑难解答文章](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services)的云服务。

若要了解如何通过使用 Azure PaaS 计算机诊断数据解决云服务的角色问题，请参阅[古柯 Williamson 博客系列](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)。
