<properties
   pageTitle="来宾操作系统系列 1 退休注意到 |Microsoft Azure"
   description="提供有关 Azure 的来宾操作系统系列 1 退休的发生以及如何确定是否会影响信息"
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



# <a name="guest-os-family-1-retirement-notice"></a>来宾操作系统系列 1 退休通知

OS 系列 1 的退休前是在 2013 年 6 月 1 日宣布的。

**到 2014 年 9 月 2，**Azure 来宾操作系统 (来宾 OS) 家族 1.x，基于 Windows Server 2008 操作系统已正式退休。 所有尝试部署新的服务或升级现有的服务使用系列 1 将都失败并显示错误消息，通知您已取消访客操作系统系列 1。

**到 2014 年 11 月 3，**扩展的支持来宾操作系统系列 1 的结束，它完全退休。 仍在家族 1 上的所有服务将受到都影响。 我们可能会在任何时候停止这些服务。 就您的服务将继续运行，除非您手动升级它们自己不能保证。

如果您还有其他问题，请访问[云服务论坛](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc)或[与 Azure 支持部门联系](https://azure.microsoft.com/support/options/)。




## <a name="are-you-affected"></a>您会受到影响？

如果下列任一适用，云服务会受到影响︰

1. 只有为"osFamily ="1"显式指定的 ServiceConfiguration.cscfg 文件中为您的云服务。
2. 没有显式指定的 ServiceConfiguration.cscfg 文件中为您的云服务的 osFamily 的值。 目前，系统将在这种情况下使用"1"的默认值。
3. Azure 的传统门户网站作为"Windows Server 2008"列出您的来宾操作系统系列值。

若要查找哪些云服务正在运行的 OS 系列，可以运行下面的脚本在 Azure PowerShell，尽管您必须[设置 Azure PowerShell](../powershell-install-configure.md)首先。 该脚本的详细信息，请参阅[Azure 来宾操作系统系列 1 的淘汰︰ 6 月 2014年](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx)。 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

云服务将受操作系统系列 1 退休，如果脚本输出中的 osFamily 列的为空或包含"1"。

## <a name="recommendations-if-you-are-affected"></a>如果受影响的建议

我们建议您将云服务角色迁移到受支持的来宾操作系统系列之一︰

**来宾操作系统家族 4.x** -Windows Server 2012 R2 *（推荐）*

1. 请确保您的应用程序使用.NET framework 4.0、 4.5 或 4.5.1 2.1 或更高版本的 SDK。
2. 设置 osFamily 属性设置为"4"，在 ServiceConfiguration.cscfg 文件中，并重新部署您的云服务。


**来宾操作系统家族 3.x** -Windows Server 2012

1. 请确保您的应用程序使用.NET framework 4.0 或 4.5 1.8 或更高版本的 SDK。
2. 设置 osFamily 属性设置为"3"中的 ServiceConfiguration.cscfg 文件，并重新部署您的云服务。


**来宾操作系统家族 2.x** -Windows Server 2008 R2

1. 确保您的应用程序正在使用 SDK 1.3 和上述.NET framework 3.5 或 4.0。
2. 设置 osFamily 属性设置为"2"，在 ServiceConfiguration.cscfg 文件中，并重新部署您的云服务。


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>扩展的支持的来宾操作系统系列 1 结束 2014，11 月 3
不再支持来宾操作系统系列 1 上的云服务。 请关闭系列 1 越早越好，以避免服务中断迁移。  

## <a name="next-steps"></a>下一步行动
查看最新[访客操作系统释放](cloud-services-guestos-update-matrix.md)。
