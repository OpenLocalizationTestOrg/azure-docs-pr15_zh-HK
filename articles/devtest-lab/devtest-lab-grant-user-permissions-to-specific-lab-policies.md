<properties
    pageTitle="授予用户的权限与特定实验室策略 |Microsoft Azure"
    description="了解如何授予对特定实验室策略基于每个用户的需要在 DevTest 实验室中的用户权限"
    services="devtest-lab,virtual-machines,visual-studio-online"
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
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>授予用户对特定实验室策略的权限

## <a name="overview"></a>概述

本文说明如何使用 PowerShell 授予特定实验室策略的用户权限。 这样一来，权限可以应用基于每个用户的需求。 例如，您可能希望授予特定用户若要更改虚拟机的策略设置，但不是成本策略的能力。

## <a name="policies-as-resources"></a>作为资源的策略

作为[基于 Azure 角色的访问控制](../active-directory/role-based-access-control-configure.md)文章所述，RBAC 实现细粒度访问 Azure 的资源管理。 使用 RBAC，可以隔离 DevOps 团队内部的职责并授予执行各自作业所需的用户只能访问量。

在 DevTest 实验室中，策略是使 RBAC 操作**Microsoft.DevTestLab/labs/policySets/policies/**的资源类型。 实验室的每个策略是策略资源类型的资源，可作为 RBAC 角色作用域分配。

例如，为了授予用户读/写权限到**允许 VM 大小**策略中，将创建一个自定义角色的**Microsoft.DevTestLab/labs/policySets/policies/**的合作*操作，然后将相应的用户分配给此自定义角色范围内的* *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**。

若要了解更多有关在 RBAC 中的自定义角色，请参阅[自定义角色的访问控制](../active-directory/role-based-access-control-custom-roles.md)。

##<a name="creating-a-lab-custom-role-using-powershell"></a>创建使用 PowerShell 实验室自定义角色
首先，您需要请阅读下面的文章将介绍如何安装和配置的 Azure PowerShell cmdlet: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre)。

一旦您设置了 Azure PowerShell cmdlet，您可以执行以下任务︰

- 资源提供程序中列出的所有操作/操作
- 属于某个特定角色的列表操作︰
- 创建自定义角色

以下的 PowerShell 脚本说明了如何执行这些任务的示例︰

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>将权限分配给特定策略使用自定义角色中的用户
一旦定义您的自定义角色，可以将它们分配给用户。 为了给用户分配一个自定义角色，必须首先获得表示该用户**对象 Id** 。 要做到这一点，使用**Get AzureRmADUser** cmdlet。

在以下示例中， *SomeUser*用户**对象 Id**为 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3。

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

一旦用户和自定义角色名的**对象 Id** ，您可以**新建 AzureRmRoleAssignment** cmdlet 的向该用户分配该角色︰

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

在前面的示例中，使用**AllowedVmSizesInLab**策略。 您可以使用以下策略︰

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>下一步行动

一次已授予用户对特定实验室策略权限下面是一些需要考虑的下一步行动︰

- [安全地访问一个实验室](devtest-lab-add-devtest-user.md)。

- [设置实验室的策略](devtest-lab-set-lab-policy.md)。

- [创建实验室模板](devtest-lab-create-template.md)。

- [为您的 Vm 创建自定义项目](devtest-lab-artifact-author.md)。

- [添加虚拟机与实验室的项目](devtest-lab-add-vm-with-artifacts.md)。
