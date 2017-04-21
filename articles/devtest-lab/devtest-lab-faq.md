<properties
    pageTitle="Azure DevTest 实验室常见问题 |Microsoft Azure"
    description="查找 Azure DevTest 实验室的常见问题的解答"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest 实验室的常见问题解答

本文解答一些有关 Azure DevTest 实验室最常见的问题。

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>常规
- [如果没有这里解答我的问题吗？](#what-if-my-question-isnt-answered-here)
- [为什么应该使用 Azure DevTest 实验室？](#why-should-i-use-azure-devtest-labs) 
- ["无忧，自助服务"是什么意思？](#what-does-quotworry-free-self-servicequot-mean)
- [如何使用 Azure DevTest 实验室？](#how-can-i-use-azure-devtest-labs) 
- [我如何是记帐的 Azure DevTest 实验室？](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>安全 
- [在 Azure DevTest 实验室不同的安全级别是什么？](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [如何创建一个允许用户执行特定任务的角色？](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD 集成和自动化 
 
- [与我 CI/CD toolchain 可以集成 Azure DevTest 实验室吗？](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>虚拟机 
 
- [为什么看不到 Azure DevTest 实验室内看到 Azure 虚拟机刀片中的某些虚拟机？](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [自定义图像与公式之间的区别是什么？](#what-is-the-difference-between-custom-images-and-formulas) 
- [如何创建多个虚拟机从同一模板一次？](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [如何将我现有的 Azure Vm 移动到我 Azure DevTest 实验的实验室？](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [可以将多个磁盘连接到我的虚拟机？](#can-i-attach-multiple-disks-to-my-vms) 
- [如何自动完成上载 VHD 文件来创建自定义映像的过程？](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [如何自动删除我的实验室中的所有虚拟机的过程？](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>项目 
 
- [项目有哪些？](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>实验室配置 
 
- [如何从 Azure 资源管理器模板创建一个实验室？](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [为什么在具有任意名称的不同的资源组中创建我的虚拟机？可以重命名或修改这些资源组？](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [在相同的订阅可以创建多少实验室？](#how-many-labs-can-i-create-under-the-same-subscription)
- [每个实验室可以创建多少虚拟机？](#how-many-vms-can-i-create-per-lab)
- [如何向我的实验室共享一个直接链接？](#how-do-i-share-a-direct-link-to-my-lab)
- [什么是 Microsoft 帐户？](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>故障排除 
 
- [在 VM 创建过程失败，我的项目。如何解决它？](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [为什么不是我现有的虚拟网络正确保存？](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>如果没有这里解答我的问题吗？
如果未列出您的问题，让我们知道，我们将帮助您找到答案。

- 本常见问题解答中的结尾处的[Disqus 线程](#comments)中提出问题并与 Azure 缓存团队和其他社区成员了解这篇文章。
- 要扩大宣传范围，将问题张贴在[Azure DevTest 实验室 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)，并与 Azure DevTest 实验室小组和社区的其他成员。
- 若要使功能请求，提交您的请求和[Azure DevTest 实验室用户语音](https://feedback.azure.com/forums/320373-azure-devtest-labs)的意见。

### <a name="why-should-i-use-azure-devtest-labs"></a>为什么应该使用 Azure DevTest 实验室？ 
Azure DevTest 实验室可以保存您的团队的时间和金钱。 开发人员可以创建他们自己的环境使用几种不同的基，并使用项目快速部署和配置应用程序。 使用自定义图像和公式，虚拟机可以另存为模板，并且轻松地重现。 此外，实验室提供实验室管理员可减少浪费和管理一个团队环境的几个可配置的策略。 这些策略包括自动关闭，成本阈值，最大的虚拟机，每个用户和 VM 大小最大值。 Azure DevTest 实验室的详细说明，请阅读[概述](devtest-lab-overview.md)或签出[介绍性视频](/documentation/videos/videos/what-is-azure-devtest-labs)。 

### <a name="what-does-worry-free-self-service-mean"></a>"无忧，自助服务"是什么意思？
"无忧，自助服务"意味着开发人员和测试人员根据需要创建他们自己的环境，并且管理员具有安全性的 Azure DevTest 实验有助于最小化浪费并控制成本。 管理员可以指定允许哪些 VM 大小，最大数量的虚拟机，以及虚拟机的启动和关闭时。 Azure DevTest 实验室还便于监控成本和设置警报，始终知道在实验室中的资源的使用方式。 

### <a name="how-can-i-use-azure-devtest-labs"></a>如何使用 Azure DevTest 实验室？ 
每当您需要开发或测试环境中，并且您想要迅速重现它们和/或管理它们的成本节约策略，azure DevTest 实验非常有用。 

下面是我们的客户使用 Azure DevTest 实验室的一些情况︰ 

- 管理开发和测试环境，在一个地方，利用策略，以减少成本和共享的自定义图像生成在团队。
- 在开发应用程序使用的自定义图像保存磁盘状态的开发阶段。
- 跟踪进度与成本。 
- 创建质量保证测试的质量测试环境。
- 使用工件和公式，可以方便地配置并重新生成应用程序在不同的环境。 
- Hackathons （协作开发或测试工作），用于分发虚拟机，然后轻松地消除资源调配这些事件结束时。 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>我如何是记帐的 Azure DevTest 实验室？ 
Azure DevTest 实验是一项免费服务，这意味着创建实验室和配置策略、 模板和项目免费。 您只能支付您的实验室，如虚拟机、 存储帐户和虚拟网络中使用的 Azure 资源。 成本的实验室资源的详细信息，阅读[Azure DevTest 实验室定价](https://azure.microsoft.com/pricing/details/devtest-lab/)。 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>在 Azure DevTest 实验室不同的安全级别是什么？  
[Azure Role-Based 的访问控制 (RBAC)](../active-directory/role-based-access-built-in-roles.md)决定了安全访问。 若要了解如何访问工作，它有助于理解权限、 角色和范围所定义的 RBAC 的区别。

- **权限**的权限是为特定的操作定义的访问。 例如，某个权限可能是读访问到所有虚拟机。 
- **角色**的角色是一组可以分组和分配给用户的权限。 例如，"订阅所有者"有权在预订中的所有资源。 
- **作用域**的作用域是 Azure 资源的层次结构中的级别。 例如，范围可以是资源组或单个实验室或整个订阅。 
 
在 Azure DevTest 实验室的范围内,，有两种类型的角色来定义用户权限︰ 实验室负责人和实验室用户。

- **实验室负责人**-实验室所有者拥有对实验室内的所有资源的访问权限。 因此，他们可以修改策略、 读取和写入任何 Vm、 更改虚拟网络，等等。 
- **实验室用户**-实验室用户可以查看所有的实验室资源，如虚拟机、 策略和虚拟网络，但不是能修改策略或由其他用户创建的任何虚拟机。 还有可能在 Azure DevTest 实验室，创建自定义角色，您可以了解如何在文章中，[特定实验室策略将用户权限授予](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)。 
 
当用户在一定范围内具有的权限范围是分层的因为它们被自动授予这些权限包含每个较低级别范围内。 例如，如果用户被指派给订阅者的角色，然后他们必须访问所有资源在订阅中。 这些资源包括所有虚拟机、 所有的虚拟网络，以及所有的实验。 因此，预订所有者将自动继承实验室所有者的角色。 但是，反过来也不成立。 实验室负责人有权访问一个实验室中，这是比订阅级别较低的范围。 因此，实验室负责人将不能查看虚拟机的虚拟网络或在实验室以外的任何资源。 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>如何创建一个允许用户执行特定任务的角色？
可以在此处找到有关如何创建自定义角色并为该角色指定权限的完整文章。 下面是脚本的创建角色"DevTest 实验室高级用户"，它有权启动和停止所有虚拟机在实验室中示例︰
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>与我 CI/CD toolchain 可以集成 Azure DevTest 实验室吗？ 
如果您正在使用 VSTS，没有[Azure DevTest 实验任务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)程序使您能够自动释放管道在 Azure DevTest 实验室。 此扩展的用法包括︰

- 创建和部署虚拟机自动配置有最新版本使用 Azure 文件副本或 PowerShell VSTS 的任务。 
- 自动测试重现 bug 对同一个 VM 作进一步调查后捕获虚拟机的状态。 
- 当不再需要时删除末尾发行管道的 VM。 

下面的博客张贴内容提供指导和信息使用 VSTS 扩展︰
 
- [Azure DevTest 实验室 – VSTS 扩展](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [部署新虚拟机在 VSTS 从现有 AzureDevTestLab](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [对连续部署到 AzureDevTestLabs 使用 VSTS 发布管理](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
对于其他 CI/CD toolchains，可以通过 VSTS 任务扩展实现的所有前面提到的方案可以通过部署[Azure 资源管理器模板](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)使用[Azure PowerShell cmdlet](../resource-group-template-deploy.md)和[.NET 的 Sdk](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/)同样获得。 [DevTest 实验室的 REST Api](http://aka.ms/dtlrestapis)也可用于与您 toolchain 集成。  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>为什么看不到 Azure DevTest 实验室内看到 Azure 虚拟机刀片中的某些虚拟机？
在 Azure DevTest 实验室中创建虚拟机后，被授予的权限访问该虚拟机。 您将能够查看它在实验室刀片式服务器和**虚拟机**刀片。 DevTest 实验室角色中的用户可以看到通过实验室的**所有虚拟机**刀片在实验室中创建的所有虚拟机。 但是，DevTest 实验室角色中的用户不会自动授予其他人创建的 VM 资源的读取访问权限。 因此，**虚拟机**刀片中不显示这些虚拟机。 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>自定义图像与公式之间的区别是什么？ 
自定义图像是一个 VHD （虚拟硬盘），而公式是可以配置的其他设置，您可以保存和再现的图像。 自定义图像可能更可取，如果您想要使用相同的基本的、 不可变图像快速创建多个环境。 公式可能更好，如果您想要重现最新位、 虚拟网络/子网或一个特定的大小与 VM 的配置。 更深入的说明，请参阅文章[进行比较自定义图像和 DevTest 实验中的公式](devtest-lab-comparing-vm-base-image-types.md)。 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>如何创建多个虚拟机从同一模板一次？ 
在创建虚拟机和[部署 Windows PowerShell 的 Azure 资源管理器模板](../resource-group-template-deploy.md)时，可以使用[VSTS 任务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)或[生成 Azure 资源管理器模板](devtest-lab-add-vm-with-artifacts.md#save-arm-template)。 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>如何将我现有的 Azure Vm 移动到我 Azure DevTest 实验的实验室？ 
我们正在设计一个解决方案，可以直接将虚拟机移动到 Azure DevTest 实验，但目前可以复制现有的虚拟机到 Azure DevTest 实验室，如下所示︰ 

1. 将您现有的虚拟机使用该[Windows PowerShell 脚本](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)的 VHD 文件复制 
1. 在 Azure DevTest 实验室实验室内的[创建自定义图像](devtest-lab-create-template.md)。 
1. 在实验室中从自定义映像创建虚拟机 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>可以将多个磁盘连接到我的虚拟机？ 
支持附加到虚拟机的多个磁盘。  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>如何自动完成上载 VHD 文件来创建自定义映像的过程？ 
有两个选项︰

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload)可用于复制或向与实验室相关的存储帐户上载 VHD 文件。
- [Microsoft Azure 存储浏览器](../vs-azure-tools-storage-manage-with-storage-explorer.md)是 Windows，OSX 和 Linux 运行的独立应用程序。   
 
若要查找与实验室相关的目标存储帐户，请执行以下步骤︰

1. 登录到[Azure 的门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)。 
1. 从左面板中选择**资源组**。 
1. 找到并选择与您的实验室相关联的资源组。 
1. 在**概述**刀片式服务器，选择一种存储帐户。 
1. 选择**Blob**。
1. 查找列表中的上载。 如果不存在，返回到步骤 #4 并尝试另一种存储帐户。
1. 使用 AzCopy 命令在目标**URL** 。


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>如何自动删除我的实验室中的所有虚拟机的过程？

除了从实验室 Azure 门户中删除虚拟机，您可以删除所有虚拟机在实验室使用 PowerShell 脚本中。 在以下示例中，只需修改的**值来更改**注释下的参数值。 您可以检索`subscriptionId`， `labResourceGroup`，和`labName`Azure 门户中实验室刀片式服务器中的值。 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>项目有哪些？ 
项目是可用于将您最新的位或开发工具到一个虚拟机上的部署的自定义元素。 给您的 VM 在几个简单的点击创建附加，一旦配置 VM 时，工件部署和配置 VM。 我们[公共 Github 存储库](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)中，在各种现有项目，但可以方便地[编写您自己的项目](devtest-lab-artifact-author.md)。 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>如何从 Azure 资源管理器模板创建一个实验室？ 
我们提供您可以部署为[Github 的 Azure 资源管理器的实验室模板的存储库](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)-它或修改来创建自定义模板，以便您的实验室。 每个模板都有一个链接，单击后可以部署为实验室-下自己 Azure 的订阅，或您可以自定义模板，并[使用 PowerShell 或 Azure CLI 进行部署](../resource-group-template-deploy.md)。
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>为什么在具有任意名称的不同的资源组中创建我的虚拟机？ 可以重命名或修改这些资源组？ 
通过这种方式使 Azure DevTest 实验室管理用户权限和访问虚拟机创建资源组。 与您所需的名称，可以将 VM 移动到另一个资源组，而这样做所以不推荐。 我们正在改进这种体验，以允许更大的灵活性。   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>在相同的订阅可以创建多少实验室？ 
没有特定的实验，可以创建每个订阅的数量限制。 但是，使用的资源是有限的每个订阅。 您可以阅读有关[施加 Azure 的订阅限制和配额](../azure-subscription-service-limits.md)以及[如何增加这些限制](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)。 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>每个实验室可以创建多少虚拟机？ 
没有特定限制可以每实验室创建的虚拟机的数量。 但是，目前实验室支持仅约 40 个虚拟机运行在标准的存储，同时和 25 高级存储中同时运行的虚拟机。 我们目前正在提高这些限制。 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>如何向我的实验室共享一个直接链接？

共享直接链接到您的实验室用户可以执行以下过程。

1. 浏览到 Azure 门户中实验室。
2. 从您的浏览器复制实验室 URL 并与实验室用户共享。 

>[AZURE.NOTE] 如果您的实验室用户是具有[MSA 帐户](#what-is-a-microsoft-account)的外部用户，它们不属于公司的 Active directory 他们导航到所提供的链接时可能会收到错误。 如果他们出现错误，指示他们单击其姓名的 Azure 门户右上角并选择要从菜单上的**目录**部分所在实验室的目录。

### <a name="what-is-a-microsoft-account"></a>什么是 Microsoft 帐户？

Microsoft 帐户是您对几乎一切您与 Microsoft 设备和服务的使用。 它是一个电子邮件地址和密码用于登录 Skype、 Outlook.com、 OneDrive、 Windows Phone 和 Xbox LIVE –，它意味着您的文件、 照片、 联系人和设置可以按照您到任何设备。 

>[AZURE.NOTE] 被称为"Windows Live™ ID"使用 Microsoft 帐户。
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>在 VM 创建过程失败，我的项目。 如何解决它？ 
请参阅博客文章[如何解决失败的工件的 AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) -编写我们的 Mvp 之一-若要了解如何获得有关您失败的项目日志。 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>为什么不是我现有的虚拟网络正确保存？  
一种可能是您的虚拟网络名称包含句点。 如果是这样，请尝试删除期间或代之以连字符，并再尝试保存虚拟网络。
