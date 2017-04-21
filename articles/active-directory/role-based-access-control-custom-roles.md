<properties
    pageTitle="自定义 Azure 的 RBAC 角色 |Microsoft Azure"
    description="了解如何在 Azure 订阅中定义自定义角色与 Azure Role-Based 访问控制更精确标识管理。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>在 Azure RBAC 中的自定义角色


创建自定义角色中 Azure Role-Based 的访问控制 (RBAC) 的内置角色不符合您的特定访问需求。 可以使用[Azure PowerShell](role-based-access-control-manage-access-powershell.md)， [Azure 命令行界面](role-based-access-control-manage-access-azure-cli.md)(CLI) 和[REST API](role-based-access-control-manage-access-rest.md)来创建自定义角色。 就像内置角色，可以为用户、 组和订购、 资源组和资源范围的应用程序分配自定义角色。 自定义角色存储在 Azure AD 租户，可以共享该租户为 Azure 的广告目录用于订阅的所有订阅。

以下是自定义监视并重新启动虚拟机用于角色的示例︰

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>操作
自定义角色的**操作**属性指定的角色授予访问权限的 Azure 运算。 它是标识 Azure 的资源提供程序的安全操作的操作字符串的集合。 操作字符串，其中包含通配符 (\*) 授予访问权限的操作字符串匹配的所有操作。 例如︰

-   `*/read`授权访问来读取所有 Azure 的资源提供程序的所有资源类型的操作。
-   `Microsoft.Network/*/read`授权访问来读取 Microsoft.Network 资源提供程序中的 Azure 的所有资源类型的操作。
-   `Microsoft.Compute/virtualMachines/*`授予对所有操作的虚拟机和它的子资源类型的访问。
-   `Microsoft.Web/sites/restart/Action`授予访问重新启动网站。

使用`Get-AzureRmProviderOperation`（以 PowerShell) 或`azure provider operations show`（在 Azure CLI) 到 Azure 的资源提供程序的列表操作。 若要验证操作字符串是有效的并展开通配符操作字符串，也可以使用这些命令。

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell screnshot-获取 AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action |FT 操作，操作名称](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI 屏幕截图的 azure 提供程序操作显示"Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
如果您要允许的操作的一套更轻松地定义通过排除受限制的操作，请使用**NotActions**属性。 通过减去从**操作**运营的**NotActions**操作来计算自定义角色授予访问权限。

> [AZURE.NOTE] 如果为用户分配角色， **NotActions**，在操作中排除并被指定为第二个角色授予访问权限的相同操作，将允许用户执行此操作。 **NotActions**不是拒绝规则，它是只是方便地创建一组允许的操作时，需要排除特定的操作。

## <a name="assignablescopes"></a>AssignableScopes
自定义角色的**AssignableScopes**属性指定在其中自定义角色是可用于分配的范围 （订阅、 资源组或资源）。 可以使自定义角色分配可以在订阅或资源组的要求，并不使其余的订阅或资源组的用户体验变得混乱。

有效分配的作用域的示例包括︰

-   "/ 订阅/c276fc76-9cd4-44c9-99a7-4fd71546436e"，"/ 订阅/e91d47c4-76f3-4271-a796-21b4ecfe3624"的两个预订中使该角色可用于分配。
-   "/ 订阅/c276fc76-9cd4-44c9-99a7-4fd71546436e"-在单个订阅使角色可用于分配。
-  "/ 订阅/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/网络"-仅在网络的资源组中进行角色可用于分配。

> [AZURE.NOTE] 您必须使用至少一个订购、 资源组或资源 id。

## <a name="custom-roles-access-control"></a>自定义角色的访问控制
自定义角色的**AssignableScopes**属性还控制谁可以查看、 修改和删除该角色。

- 谁可以创建一个自定义角色？
    所有者 （和用户访问管理员） 的订阅，资源组和资源可以用于自定义角色中创建这些范围。
    创建角色的用户需要能够执行`Microsoft.Authorization/roleDefinition/write` **AssignableScopes**操作的角色。

- 可以修改自定义角色的用户
    所有者 （和用户访问管理员） 的订阅、 资源组和资源可以修改这些范围中的自定义角色。 用户需要能够执行`Microsoft.Authorization/roleDefinition/write` **AssignableScopes**上的自定义角色的操作。

- 哪些人可以查看自定义的角色？
    在 Azure RBAC 的所有内置角色允许查看可用于分配的角色。 用户可以执行`Microsoft.Authorization/roleDefinition/read`范围内的操作可以查看可用于在该范围内分配的 RBAC 角色。

## <a name="see-also"></a>请参见
- [基于角色的访问控制](role-based-access-control-configure.md)︰ 开始使用 RBAC 在 Azure 的门户。
- 了解如何管理具有的访问权限︰
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST API，](role-based-access-control-manage-access-rest.md)
- [内置角色](role-based-access-built-in-roles.md)︰ 获得有关标准在 RBAC 角色的详细信息。
