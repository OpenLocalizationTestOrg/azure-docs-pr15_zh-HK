<properties
    pageTitle="管理基于角色的访问控制 (RBAC) 使用 Azure CLI |Microsoft Azure"
    description="了解如何管理基于角色的访问控制 (RBAC) 与 Azure 的命令行界面，通过列出角色和角色的操作，并通过将角色分配给订阅和应用范围。"
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>管理 Azure 的命令行界面使用基于角色的访问控制

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API，](role-based-access-control-manage-access-rest.md)

您可以使用基于角色的访问控制 (RBAC) 在 Azure 门户和 Azure 资源管理器 API 来管理您的订阅和细粒度级别的资源访问。 使用此功能，您可以通过在特定的范围内，向其分配一些角色授予访问 Active Directory 用户、 组或服务主体。

使用 Azure 的命令行界面 (CLI) 来管理 RBAC 之前，您必须具有以下︰

- Azure CLI 版本 0.8.8 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参见[安装和配置 Azure CLI](../xplat-cli-install.md)。
- 在 Azure CLI 的 azure 资源管理器。 有关详细信息，请转到[使用 Azure CLI 使用资源管理器](../xplat-cli-azure-resource-manager.md)。

## <a name="list-roles"></a>列表中的角色

### <a name="list-all-available-roles"></a>列出所有可用的角色
若要列出所有可用的角色，请使用︰

        azure role list

下面的示例显示*所有可用的角色*的列表。

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure 命令行 azure 角色列表屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>角色的列表操作
若要列出一个角色的操作，请使用︰

    azure role show "<role name>"

下面的示例显示的*参与者*和*虚拟机参与者*角色的操作。

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure 命令行 — azure 角色显示的屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>列表的访问
### <a name="list-role-assignments-effective-on-a-resource-group"></a>角色分配在资源组中有效
若要列出存在的资源组中的角色分配，请使用︰

    azure role assignment list --resource-group <resource group name>

下面的示例演示在*制药-销售-projecforcast*组的角色分配。

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure 命令行-azure 角色分配列表按组屏幕快照](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>用户的分配角色的列表
若要列出指定给特定用户的角色分配给用户组分配，使用︰

    azure role assignment list --signInName <user email>

您还可以查看通过修改命令从组继承的角色分配︰

    azure role assignment list --expandPrincipalGroups --signInName <user email>

下面的示例演示被授予的角色分配*sameert@aaddemo.com*用户。 这包括直接指派给用户的角色和从组继承的角色。

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure 命令行的 azure 角色分配由用户列表-屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>授予访问权限
要授予访问权限，您确定您想要分配的角色后，请使用︰

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>将角色指派给订阅范围组
将角色分配给一组订阅范围，请使用︰

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

下面的示例将*读者*角色分配给*Christine Koch 团队*在*订阅*范围。


![RBAC Azure 命令行-角色分配 azure 创建组屏幕快照](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>将角色分配给应用程序的订阅范围在
将角色分配给应用程序在订阅范围，请使用︰

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

下面的示例将授予*参与者*角色到*Azure AD*应用程序上选择的订阅。

 ![RBAC Azure 命令行-角色分配 azure 创建应用程序](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>将角色分配给资源组作用域上的用户
若要将角色分配给资源组作用域上的用户，请使用︰

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

下面的示例将授予对*虚拟机参与者*角色*samert@aaddemo.com*在*制药-销售-ProjectForcast*资源组作用域的用户。

![RBAC Azure 命令行-角色分配 azure 创建由用户屏幕快照](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>为一组资源范围内分配角色
将角色分配给一组资源范围内，请使用︰

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

下面的示例将*虚拟机参与者*角色授予*子网*网上*Azure AD*组。

![RBAC Azure 命令行-角色分配 azure 创建组屏幕快照](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>删除访问权限
若要删除角色分配，请使用︰

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

下面的示例移除从*虚拟机投稿人*角色分配*sammert@aaddemo.com*对*制药-销售-ProjectForcast*资源组的用户。
该示例然后从订阅上的组中移除角色分配。

![RBAC Azure 命令行的 azure 角色分配删除-屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>创建自定义角色
若要创建自定义角色，请使用︰

    azure role create --inputfile <file path>

下面的示例创建一个自定义角色称为*虚拟机器操作员*。 自定义角色授予对所有读取操作的*Microsoft.Compute*、 *Microsoft.Storage*和*Microsoft.Network*的资源提供程序的访问权限，并授予访问权限来启动、 重新启动和监控虚拟机。 两个预订中，可以使用自定义角色。 此示例使用 JSON 文件作为输入。

![JSON 的自定义角色定义的屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure 命令行-azure 角色创建的屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>修改自定义角色

若要修改自定义角色，请首先使用`azure role show`命令以检索角色定义。 第二，对角色定义文件进行所需的更改。 最后，使用`azure role set`若要保存修改的角色定义。

    azure role set --inputfile <file path>

下面的示例将*Microsoft.Insights/diagnosticSettings/*操作添加到**操作**和自定义虚拟机操作员角色的**AssignableScopes** Azure 订阅。

![JSON-修改自定义角色定义的屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure 命令行 azure 角色集-屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>删除自定义角色

若要删除一个自定义角色，请首先使用`azure role show`命令来确定角色的**ID** 。 然后，使用`azure role delete`命令，通过指定**ID**删除角色。

下面的示例移除*虚拟机器操作员*自定义角色。

![RBAC Azure 命令行 azure 角色删除屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>列表的自定义角色

若要列出可用于在一个作用域分配的角色，请使用`azure role list`命令。

下面的示例列出可用于所选的订阅中分配的所有角色。

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure 命令行 azure 角色列表屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

在下面的示例中，*虚拟机器操作员*自定义角色并不在*Production4*订阅可用因为角色的**AssignableScopes**中不存在该订阅。

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure 命令行的自定义角色的 azure 角色列表-屏幕抓图](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC 的主题
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
