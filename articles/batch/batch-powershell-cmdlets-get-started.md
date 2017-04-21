<properties
   pageTitle="开始使用 Azure 批 PowerShell |Microsoft Azure"
   description="获取可用于管理 Azure 批服务 Azure PowerShell cmdlet 简介"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>开始使用 Azure 批 PowerShell cmdlet
使用 Azure 批 PowerShell 的 cmdlet，可以执行和编写脚本的批处理 Api 和 Azure 的门户，Azure 的命令行界面 (CLI) 执行相同的任务很多。 这是快速了解 cmdlet，您可以使用管理批次帐户并使用批处理资源池、 作业和任务等。

批处理的 cmdlet 和详细的 cmdlet 语法的完整列表，请参阅[Azure 批 cmdlet 参考](https://msdn.microsoft.com/library/azure/mt125957.aspx)。

本文基于 Azure PowerShell 3.0.0 版的 cmdlet。 我们建议您更新您的 Azure PowerShell 经常能够利用服务更新和增强功能。

## <a name="prerequisites"></a>系统必备组件

执行下列操作以使用 Azure PowerShell 管理批次资源。

* [安装和配置 Azure PowerShell](../powershell-install-configure.md)

* 运行**登录 AzureRmAccount**用于连接到您的订阅 （Azure 批 cmdlet 运输 Azure 资源管理器模块中）︰

    `Login-AzureRmAccount`

* **注册与批次提供程序命名空间**。 此操作只需要执行的**一次每个订阅**。

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>管理批次帐户和键

### <a name="create-a-batch-account"></a>创建一个批处理帐户

**新建 AzureRmBatchAccount**指定的资源组中创建一个批次的帐户。 如果您没有创建一个运行[新建 AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) cmdlet 的资源组。 **位置**参数，如"美国中心"中指定的 Azure 的地区之一。 例如︰

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

然后，创建一个批次的帐户 <*account_name*> 中的帐户的位置和资源组的名称为指定名称的资源组。 创建批处理帐户可能要花一些时间才能完成。 例如︰

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] 批帐户名称必须是唯一的资源组的 Azure 区域、 包含 3 和 24 个字符之间，并且使用小写字母和数字。

### <a name="get-account-access-keys"></a>获取帐户访问键
**获得 AzureRmBatchAccountKeys**显示了 Azure 批帐户相关联的访问键。 例如，运行以下操作以获取您所创建的帐户的主、 辅键。

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>生成一个新的访问键
**新 AzureRmBatchAccountKey**生成一个新的主要或辅助帐户密钥 Azure 批帐户。 例如，要生成批帐户的新主要密钥，请键入︰

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] 若要生成一个新的第二项，指定"次"为**关键字类型**参数。 您必须单独重新生成主、 辅密钥。

### <a name="delete-a-batch-account"></a>删除批次帐户
**删除 AzureRmBatchAccount**中删除批次帐户。 例如︰

    Remove-AzureRmBatchAccount -AccountName <account_name>

出现提示时，确认要删除该帐户。 帐户删除可能需要一些时间才能完成。

## <a name="create-a-batchaccountcontext-object"></a>创建一个 BatchAccountContext 对象

要使用批 PowerShell cmdlet，当您创建和管理批处理池、 作业、 任务和其他资源进行身份验证，首先创建一个 BatchAccountContext 对象来存储您的帐户名称和密钥︰

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

BatchAccountContext 对象传入使用**BatchContext**参数的 cmdlet。

> [AZURE.NOTE] 默认情况下，该帐户的主关键字用于身份验证，但您可以显式选择要通过更改 BatchAccountContext 对象的**KeyInUse**属性使用的参数︰ `$context.KeyInUse = "Secondary"`。

## <a name="create-and-modify-batch-resources"></a>创建和修改批资源
使用 cmdlet 如**新建 AzureBatchPool**， **New AzureBatchJob**， **New AzureBatchTask**创建批处理帐户下的资源。 对应**Get**和**集的**cmdlet 来更新现有的资源的属性和**删除的**cmdlet 去批帐户下的资源。

当使用很多这些 cmdlet，除了传递一个 BatchContext 对象，您需要创建或传递对象，其中包含详细的资源设置，如下面的示例中所示。 有关其他示例每个 cmdlet 的详细的帮助，请参阅。

### <a name="create-a-batch-pool"></a>创建一个批处理池

当创建或更新批处理池，您可以选择云服务配置或 （请参阅[批处理功能概述](batch-api-basics.md#pool)） 的计算节点上的操作系统的虚拟机配置。 您的选择将决定是否计算节点映像之一[Azure 来宾操作系统释放](../cloud-services/cloud-services-guestos-update-matrix.md#releases)或在 Azure 市场上受支持的 Linux 或 Windows 虚拟机图像中的一个。

当您运行**新建 AzureBatchPool**时，PSCloudServiceConfiguration 或 PSVirtualMachineConfiguration 对象中传递操作系统设置。 例如，以下 cmdlet 用在云服务配置中，使用最新的操作系统版本系列 3 (Windows Server 2012) 的映像的大小小计算节点创建新批池。 在这里， **CloudServiceConfiguration**参数指定的*$configuration*变量与 PSCloudServiceConfiguration 对象。 **BatchContext**参数指定的 BatchAccountContext 对象以前定义的变量*$context* 。

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

由自动缩放公式确定目标计算中的节点数的新池。 在这种情况下，公式就是**$TargetDedicated = 4**，池中的计算节点的数目最多为 4。

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>为池、 作业、 任务和其他详细信息的查询

使用批处理帐户下创建的实体的 cmdlet 例如**Get AzureBatchPool**、**获取 AzureBatchJob**和**获取 AzureBatchTask**查询。

### <a name="query-for-data"></a>查询数据

举一个例子，使用**Get AzureBatchPools**来查找您的池。 默认情况下所有池，您的帐户，假设您已为此查询存储在*$context*中的 BatchAccountContext 对象︰

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>使用 OData 筛选器

您可以提供 OData 筛选器，**筛选器**参数用于查找您感兴趣的对象。 例如，您可以找到所有池 id"myPool"开始︰

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

此方法不是在本地管道中使用"Where 对象"不如灵活。 但是，查询获取直接发送给批量服务，使所有筛选发生在服务器端，节约了互联网的带宽。

### <a name="use-the-id-parameter"></a>使用 Id 参数

OData 筛选器的替代方法是使用**Id**参数。 要查询一个特定的池与 id"myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

**Id**参数支持全 id 搜索、 没有通配符或 OData 式过滤器。

### <a name="use-the-maxcount-parameter"></a>使用 MaxCount 参数

默认情况下，每个 cmdlet 返回 1000年对象的最大值。 如果达到此限制时，优化您的筛选器将返回更少的对象，或者将最多使用**MaxCount**参数显式设置。 例如︰

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

若要删除上限，将**MaxCount**设置为 0 或更少。

### <a name="use-the-powershell-pipeline"></a>使用 PowerShell 管线

批处理的 cmdlet 可以利用 PowerShell 管道以 cmdlet 之间发送数据。 这具有指定参数，相同的效果，但与多个实体的工作更轻松。

例如，查找并显示在您的帐户下的所有任务︰

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

重新启动 （重新启动） 在池中的每个计算节点︰

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>应用程序数据包管理

应用程序包提供简化的方法来部署应用程序池中的计算节点。 与批处理 PowerShell 的 cmdlet，可以上载和批处理帐户，管理应用程序软件包和部署包版本，若要计算节点。

**创建**应用程序︰

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**添加**一个应用程序包︰

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

设置应用程序的**默认版本**︰

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**列表中**应用程序的包

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**删除**一个应用程序包

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**删除**应用程序

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] 删除应用程序之前，必须删除所有应用程序的应用程序的包版本。 如果您尝试删除当前已应用程序包的应用程序，您将收到一个冲突错误。

### <a name="deploy-an-application-package"></a>部署一个应用程序包

在您创建池时，您可以指定用于部署的一个或多个应用程序包。 在池中创建时指定一个包时，它被部署到每个节点作为节点连接池。 在重新引导或重新映像的节点时，还将部署包。

指定`-ApplicationPackageReference`选项时创建池，池中加入到该池的节点部署一个应用程序包。 首先，创建一个**PSApplicationPackageReference**对象，并将其配置与想要部署到该池的计算节点的应用程序 Id 和包版本︰

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

现在创建池，并指定为参数的包引用对象`ApplicationPackageReferences`选项︰

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)中，可以找到应用程序包的详细信息。

>[AZURE.IMPORTANT] 必须为要使用应用程序软件包的批处理帐户[链接 Azure 存储帐户](#linked-storage-account-autostorage)。

### <a name="update-a-pools-application-packages"></a>更新池的应用程序包

要更新分配给现有池的应用程序，请首先具有所需属性 （应用程序 Id 和包版本） 中创建 PSApplicationPackageReference 对象︰

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

接下来，池中获得批、 清除任何现有的文件包、 添加我们新的包引用和使用新的池设置更新批处理服务︰

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

现在，您已经更新批次服务中的池的属性。 实际部署新的应用程序包，以计算节点在池中，但是，您必须重新启动或重新映射这些节点。 在池中使用此命令，可以重新启动每个节点︰

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] 可以将多个应用程序包部署到池中的计算节点。 如果您想*添加*到一个应用程序包，而不是替换当前部署的包，省略`$pool.ApplicationPackageReferences.Clear()`上线。

## <a name="next-steps"></a>下一步行动

* 有关详细的 cmdlet 的语法和示例，请参阅[Azure 批 cmdlet 参考](https://msdn.microsoft.com/library/azure/mt125957.aspx)。

* 有关应用程序和批处理中的应用程序包的详细信息，请参阅[使用 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)。
