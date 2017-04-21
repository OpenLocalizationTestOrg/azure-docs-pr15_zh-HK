<properties
    pageTitle="管理基于角色的访问控制 (RBAC) 与 Azure PowerShell |Microsoft Azure"
    description="如何管理 RBAC 与 Azure PowerShell，包括列出角色、 分配角色、 和删除角色分配。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>管理基于角色的访问控制与 Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API，](role-based-access-control-manage-access-rest.md)


可以使用基于角色的访问控制 (RBAC) 在 Azure 门户和 Azure 资源管理 API 来管理访问细粒度级别上订阅。 使用此功能，您可以通过在特定的范围内，向其分配一些角色授予访问 Active Directory 用户、 组或服务主体。

您可以使用 PowerShell 管理 RBAC 之前，必须具有下列︰

- Azure PowerShell 版本 0.8.8 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

- Azure 的资源管理器 cmdlet。 安装 PowerShell [Azure 资源管理器的 cmdlet](https://msdn.microsoft.com/library/mt125356.aspx) 。

## <a name="list-roles"></a>列表中的角色

### <a name="list-all-available-roles"></a>列出所有可用的角色
列表可用于分配并检查它们向其授予访问权限，操作使用的 RBAC 角色为`Get-AzureRmRoleDefinition`。

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell 获取 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>角色的列表操作
要列出为特定角色的操作，请使用`Get-AzureRmRoleDefinition <role name>`。

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell 的特定角色获取 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>谁可以访问，请参阅
RBAC 访问分配的列表，请使用`Get-AzureRmRoleAssignment`。

### <a name="list-role-assignments-at-a-specific-scope"></a>列表在特定范围内的角色分配
您可以查看分派给指定的订阅、 资源组或资源的所有访问权限。 例如，若要查看所有的活动分配的资源组，请使用`Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`。

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell 4 为资源组获取 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>列表分配给用户的角色
若要列出分配给指定用户的所有角色和角色分配给该用户所属的组，请使用`Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`。

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell 的用户获取 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>列表中经典的服务管理员和 coadmin 角色分配
对经典预订管理员和 coadministrators，使用列表访问分配︰

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>授予访问权限
### <a name="search-for-object-ids"></a>对象 Id 的搜索
要指定角色，您需要标识 （用户、 组或应用程序） 的对象和范围。

如果您不知道订阅 ID，您可以在 Azure 的门户网站上**预订**刀片式服务器中发现。 若要了解如何订阅 ID 的查询，请参阅 MSDN 上的[获取 AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) 。

若要获取的对象 ID Azure AD 组，请使用︰

    Get-AzureRmADGroup -SearchString <group name in quotes>

若要获取 Azure AD 服务主体或应用程序的对象 ID，请使用︰

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>将角色分配给应用程序的订阅范围在
若要授予访问订阅范围内应用程序，请使用︰

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell 的新 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>将角色分配给资源组作用域上的用户
要在资源组作用域的用户授予访问权限，请使用︰

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell 的新 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>为一组资源范围内分配角色
在资源范围中的组授予访问权限，请使用︰

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell 的新 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>删除访问权限
若要删除用户、 组和应用程序的访问权限，请使用︰

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell 删除 AzureRmRoleAssignment 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>创建自定义角色
若要创建自定义角色，请使用`New-AzureRmRoleDefinition`命令。

当您使用 PowerShell 创建自定义角色时，您需要启动与[内置角色](role-based-access-built-in-roles.md)之一。 编辑要添加*操作*、 *notActions*或*作用域*所需的属性，然后将所做的更改保存为新的角色。

下面的示例启动与*虚拟机参与者*角色，并使用此信息来创建自定义角色称为*虚拟机器操作员*。 新角色授予对所有读取操作的*Microsoft.Compute*、 *Microsoft.Storage*和*Microsoft.Network*的资源提供程序的访问权限，并授予访问权限来启动、 重新启动和监控虚拟机。 两个预订中，可以使用自定义角色。

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell 获取 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>修改自定义角色
若要修改自定义角色，首先，使用`Get-AzureRmRoleDefinition`命令以检索角色定义。 第二，对角色定义中进行所需的更改。 最后，使用`Set-AzureRmRoleDefinition`命令以保存修改的角色定义。

下面的示例添加`Microsoft.Insights/diagnosticSettings/*`*虚拟机器操作员*自定义角色的操作。

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell 组-AzureRmRoleDefinition-屏幕抓图](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

下面的示例将 Azure 订阅添加到*虚拟机操作员*自定义角色的分配范围。

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![RBAC PowerShell 组-AzureRmRoleDefinition-屏幕抓图](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>删除自定义角色

若要删除一个自定义角色，请使用`Remove-AzureRmRoleDefinition`命令。

下面的示例移除*虚拟机器操作员*自定义角色。

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell 删除 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>列表的自定义角色
若要列出可用于在一个作用域分配的角色，请使用`Get-AzureRmRoleDefinition`命令。

下面的示例列出可用于所选的订阅中分配的所有角色。

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell 获取 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

在下面的示例中，*虚拟机器操作员*自定义角色并不在*Production4*订阅可用因为角色的**AssignableScopes**中不存在该订阅。

![RBAC PowerShell 获取 AzureRmRoleDefinition 的屏幕抓图](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>请参见
- [使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
