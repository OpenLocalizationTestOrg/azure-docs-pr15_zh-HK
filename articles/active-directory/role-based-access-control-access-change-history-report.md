<properties
    pageTitle="创建访问变更历史记录报告 |Microsoft Azure"
    description="生成一个报表，列出在 Azure 订阅与过去的 90 天中基于角色的访问控制权限的所有更改。"
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
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>创建访问变更历史记录报告

人授予或撤消访问您的订阅，在任何时候 Azure 事件中记录所做的更改。 您可以创建的访问变更历史记录报告，在过去的 90 天内将看到的所有更改。

## <a name="create-a-report-with-azure-powershell"></a>使用 Azure PowerShell 创建报表
若要创建在 PowerShell 的访问变更历史记录报告，使用`Get-AzureRMAuthorizationChangeLog`命令。 [PowerShell 库](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1)提供了有关此 cmdlet 的更多详细信息。

当调用该命令时，您可以指定您希望列出的包括所分配的属性︰

| 属性 | 说明 |
| -------- | ----------- |
| **操作** | 是否访问被授予或废除 |
| **调用方** | 负责访问更改所有者 |
| **日期** | 日期和时间访问已更改 |
| **目录名称** | Azure Active Directory 目录 |
| **PrincipalName** | 用户、 组或应用程序的名称 |
| **PrincipalType** | 工作分配是否为用户、 组或应用程序 |
| **RoleId** | 已授予或废除的角色的 GUID |
| **角色名** | 角色已授予或废除 |
| **ScopeName** | 订购、 资源组或资源的名称 |
| **ScopeType** | 工作分配是否在订购、 资源组或资源范围 |
| **SubscriptionId** | GUID 的 Azure 的订阅 |
| **SubscriptionName** | Azure 订阅的名称 |

此示例命令将列出在过去 7 天的订阅中的所有访问更改︰

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell 获取 AzureRMAuthorizationChangeLog 的屏幕抓图](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>使用 Azure CLI 创建报表
若要创建访问变更历史记录报告在 Azure 的命令行界面 (CLI)，使用`azure role assignment changelog list`命令。

## <a name="export-to-a-spreadsheet"></a>导出到电子表格
若要保存该报告，或操作数据时，导出为.csv 文件的访问权限更改。 然后，您可以查看报表审电子表格中。

![作为电子表格的屏幕抓图查看更改日志](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>请参见
- 开始使用[Azure Role-Based 访问控制](role-based-access-control-configure.md)
- 使用[Azure RBAC 中的自定义角色](role-based-access-control-custom-roles.md)
