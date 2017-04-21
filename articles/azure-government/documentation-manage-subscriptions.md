<properties
    pageTitle="Azure 的政府服务 |Microsoft Azure"
    description="管理您的订阅中 Azure 政府信息"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>管理和连接到您的订购 Azure 政府

Azure 的政府都有唯一的 Url 和用于管理您的环境的终结点。 请务必使用正确的连接来管理您的环境通过门户或 PowerShell。 一旦连接到 Azure 政府环境，如果已部署组件的效果为管理服务的正常运行。

## <a name="connecting-via-the-portal"></a>通过门户连接
该门户是大多数人连接到 Azure 政府的主要方式。  要连接，浏览到位于[https://portal.azure.us](https://portal.azure.us)的门户。  可以通过[https://manage.windowsazure.us](https://manage.windowsazure.us)Azure 门户的旧版本。

通过连接到[https://account.windowsazure.us](https://account.windowsazure.us)，可以为您的帐户创建订阅。

## <a name="connecting-via-powershell"></a>通过 PowerShell 连接

无论您使用 Azure PowerShell 管理大型订阅通过脚本或访问不是您需要连接到 Azure 政府而不是公用 Azure 的 Azure 门户中当前可用的功能。  如果您在 Azure 公共使用 PowerShell，它很是相同。  在 Azure 政府的区别是︰

+ 连接您的帐户
+ 地区名称

>[AZURE.NOTE] 如果您还没有使用 PowerShell，签出[Azure PowerShell 的简介](../powershell-install-configure.md)。

当您启动 PowerShell 时，您必须告诉 Azure PowerShell 若要通过指定环境参数连接到 Azure 政府。  该参数可确保 PowerShell 正在连接到正确的终结点。  当您连接到您的帐户的日志中的确定的终结点集合。  不同的 Api 需要不同版本的环境切换︰

连接类型 | 命令
---|----
[服务管理](https://msdn.microsoft.com/library/dn708504.aspx)命令 | `Add-AzureAccount -Environment AzureUSGovernment`
[资源管理](https://msdn.microsoft.com/library/mt125356.aspx)命令 | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx)命令 | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory 命令 v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

此外可以使用 New AzureStorageContext 存储帐户连接时使用的环境切换，指定 AzureUSGovernment。

### <a name="determining-region"></a>确定区域

一旦连接，没有其他区别 – 用于面向服务的地区。  每个 Azure 的云都有不同的地区。  您可以看到它们服务可用性页上列出。  通常情况下，命令需要使用位置参数中的区域。

这里有个问题。  Azure 政府区域需要不同于其通用名称格式︰

公用名称 | 命令
---|----
美国 Gov 弗吉尼亚 | USGov 弗吉尼亚
美国 Gov 爱荷华州 | USGov 爱荷华州

>[AZURE.NOTE] 使用位置参数时美国和 Gov 之间没有空格。

如果要验证的可用区域的 Azure 政府，可以运行下面的命令并打印当前列表︰

    Get-AzureLocation

如果您是好奇的可用环境跨 Azure，您可以运行︰

    Get-AzureEnvironment

## <a name="next-steps"></a>下一步行动

如果您正在寻找更多的信息，您可以签出︰

+ [在 GitHub PowerShell 文档](https://github.com/Azure/azure-powershell)
+ [在连接到资源管理的分步说明](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [在 MSDN 上的 azure PowerShell 文档](https://msdn.microsoft.com/library/mt619274.aspx)

有关的补充信息和更新的订阅 [Microsoft Azure 政府博客] (https://blogs.msdn.microsoft.com/azuregov/)
