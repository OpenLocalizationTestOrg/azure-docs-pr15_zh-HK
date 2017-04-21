<properties
    pageTitle="RBAC︰ 内置角色 |Microsoft Azure"
    description="本主题描述内置的基于角色的访问控制 (RBAC) 的角色。"
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
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC︰ 内置角色

Azure 的基于角色的访问控制 (RBAC) 附带以下内置角色可以指派给用户、 组和服务。 您不能修改内置角色的定义。 但是，您可以创建[在 Azure RBAC 中的自定义角色](role-based-access-control-custom-roles.md)以适合您的组织的特定需要。

## <a name="roles-in-azure"></a>在 Azure 中的角色

下表提供了内置角色的简短说明。 单击角色名称可查看**操作**和**notactions**的角色的详细的列表。 **操作**属性指定在 Azure 资源允许的操作。 操作字符串可以使用通配字符。 **Notactions**属性指定的操作不允许的操作。

>[AZURE.NOTE] 不断发展的 Azure 角色定义。 这篇文章保持为最新，但您总是能找到新的角色定义在 Azure PowerShell。 使用 cmdlet`(get-azurermroledefinition "<role name>").actions`或`(get-azurermroledefinition "<role name>").notactions`一样适用。

| 角色名称 | 说明 |
| --------- | ----------- |
| [API 管理服务参与者](#api-management-service-contributor) | 可以管理 API 管理服务 |
| [应用程序组件参与者的见解](#application-insights-component-contributor) | 可以管理应用程序的见解组件 |
| [自动化运算符](#automation-operator) | 可以启动、 停止、 挂起和恢复作业 |
| [BizTalk 参与者](#biztalk-contributor) | 可以管理 BizTalk 服务 |
| [投稿人 ClearDB MySQL 数据库](#cleardb-mysql-db-contributor) | 可以管理 ClearDB MySQL 数据库 |
| [参与者](#contributor) | 可以管理访问之外的全部内容。 |
| [数据工厂参与者](#data-factory-contributor) | 可以创建和管理数据工厂，并在它们的子资源。 |
| [DevTest 实验室用户](#devtest-labs-user) | 可以查看所有内容和连接，启动、 重新启动和关闭虚拟机 |
| [DNS 区域参与者](#dns-zone-contributor) | DNS 区域和记录管理 |
| [投稿人 DocumentDB 帐户](#documentdb-account-contributor) | 可以管理 DocumentDB 帐户 |
| [智能系统帐户参与者](#intelligent-systems-account-contributor) | 可以管理智能系统帐户 |
| [网络参与者](#network-contributor) | 可以管理所有网络资源 |
| [新的 Relic APM 帐户参与者](#new-relic-apm-account-contributor) | 新的 Relic 应用程序性能管理帐户和应用程序可以管理 |
| [所有者](#owner) | 可以管理所有内容，包括访问 |
| [读取器](#reader) | 可以查看所有信息，但不能更改 |
| [Redis 高速缓存参与者](#redis-cache-contributor) | 可以管理 Redis 高速缓存 |
| [计划程序作业集合参与者](#scheduler-job-collections-contributor) | 可以管理调度程序作业集合 |
| [搜索服务参与者](#search-service-contributor) | 可以管理搜索服务 |
| [安全管理器](#security-manager) | 可以管理安全组件、 安全策略和虚拟机 |
| [SQL 数据库参与者](#sql-db-contributor) | 可以在 SQL 数据库中，但没有它们与安全相关的策略管理 |
| [SQL 安全管理器](#sql-security-manager) | 可以管理与安全相关的 SQL 服务器和数据库的策略 |
| [SQL Server 参与者](#sql-server-contributor) | 可以在 SQL 服务器和数据库，但不是及其与安全相关的策略管理 |
| [传统的存储帐户参与者](#classic-storage-account-contributor) | 可以管理经典存储帐户 |
| [存储帐户参与者](#storage-account-contributor) | 可以管理存储帐户 |
| [用户访问管理员](#user-access-administrator) | 可以管理用户对 Azure 的资源访问权限 |
| [传统虚拟机参与者](#classic-virtual-machine-contributor) | 经典的虚拟机，但不是在虚拟网络或存储帐户连接到可以管理 |
| [虚拟机参与者](#virtual-machine-contributor) | 可以管理虚拟机，但不是在虚拟网络或存储帐户连接到 |
| [传统的网络参与者](#classic-network-contributor) | 经典的虚拟网络和保留的 Ip，可以管理 |
| [网站计划参与者](#web-plan-contributor) | 可以管理网站计划 |
| [网站讨论参与者](#website-contributor) | 可以管理网站，但无法连接到的 web 计划 |

## <a name="role-permissions"></a>角色权限
下表描述了每个角色授予的特定权限。 这可能包括**操作**，它们授予的权限，并**NotActions**，对其加以限制。

### <a name="api-management-service-contributor"></a>API 管理服务参与者
可以管理 API 管理服务

| **操作** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | 创建和管理 API 管理服务 |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读的角色和角色分配 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="application-insights-component-contributor"></a>应用程序组件参与者的见解
可以管理应用程序的见解组件

| **操作** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.Insights/components/* | 创建和管理组件的见解 |
| Microsoft.Insights/webtests/* | 创建和管理 web 测试 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="automation-operator"></a>自动化运算符
可以启动、 停止、 挂起和恢复作业

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Automation/automationAccounts/jobs/read | 阅读科目作业的自动化 |
| Microsoft.Automation/automationAccounts/jobs/resume/action | 恢复自动帐户作业 |
| Microsoft.Automation/automationAccounts/jobs/stop/action | 停止自动帐户作业 |
| Microsoft.Automation/automationAccounts/jobs/streams/read | 读取客户工作流的自动化 |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | 挂起一个自动化科目作业 |
| Microsoft.Automation/automationAccounts/jobs/write | 编写客户作业的自动化 |
| Microsoft.Automation/automationAccounts/jobSchedules/read | 阅读自动化科目的作业调度 |
| Microsoft.Automation/automationAccounts/jobSchedules/write | 阅读自动化科目的作业调度 |
| Microsoft.Automation/automationAccounts/read | 读自动化帐户 |
| Microsoft.Automation/automationAccounts/runbooks/read | 读自动化运行手册 |
| Microsoft.Automation/automationAccounts/schedules/read | 阅读客户计划的自动化 |
| Microsoft.Automation/automationAccounts/schedules/write | 编写自动化客户计划 |
| Microsoft.Insights/components/* | 创建和管理组件的见解 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="biztalk-contributor"></a>BizTalk 参与者
可以管理 BizTalk 服务

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.BizTalkServices/BizTalk/* | 创建和管理 BizTalk 服务 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="cleardb-mysql-db-contributor"></a>投稿人 ClearDB MySQL 数据库
可以管理 ClearDB MySQL 数据库

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |
| successbricks.cleardb/databases/* | 创建和管理 ClearDB MySQL 数据库 |

### <a name="contributor"></a>参与者
可以管理访问之外的全部内容

| **操作** ||
| ------- | ------ |
| * | 创建和管理所有类型的资源 |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | 无法删除角色和角色分配 |  
| Microsoft.Authorization/*/Write | 无法创建角色和角色分配 |

### <a name="data-factory-contributor"></a>数据工厂参与者
创建和管理数据工厂，并在它们的子资源。

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.DataFactory/dataFactories/* | 创建和管理数据工厂，并在它们的子资源。 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="devtest-labs-user"></a>DevTest 实验室用户
可以查看所有内容和连接，启动、 重新启动和关闭虚拟机

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Compute/availabilitySets/read | 读取属性的可用性设置 |
| Microsoft.Compute/virtualMachines/*/read | 读取属性的虚拟机 （VM 大小、 运行状态、 VM 扩展等） |
| Microsoft.Compute/virtualMachines/deallocate/action | 取消分配虚拟机 |
| Microsoft.Compute/virtualMachines/read | 读取的虚拟机的属性 |
| Microsoft.Compute/virtualMachines/restart/action | 重启虚拟机 |
| Microsoft.Compute/virtualMachines/start/action | 启动虚拟机 |
| Microsoft.DevTestLab/*/read | 读取属性的一个实验室 |
| Microsoft.DevTestLab/labs/createEnvironment/action | 创建实验室环境 |
| Microsoft.DevTestLab/labs/formulas/delete | 删除公式 |
| Microsoft.DevTestLab/labs/formulas/read | 阅读的公式 |
| Microsoft.DevTestLab/labs/formulas/write | 添加或修改公式 |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | 评估实验室策略 |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | 加入一个负载平衡器后端地址池 |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | 加入一个负载平衡器站的 NAT 规则 |
| Microsoft.Network/networkInterfaces/*/read | 读取属性的网络接口 （例如，所有的负载平衡器的网络接口的一部分） |
| Microsoft.Network/networkInterfaces/join/action | 将虚拟机加入到网络接口 |
| Microsoft.Network/networkInterfaces/read | 读取网络接口 |
| Microsoft.Network/networkInterfaces/write | 编写网络接口 |
| Microsoft.Network/publicIPAddresses/*/read | 读取属性的公用 IP 地址 |
| Microsoft.Network/publicIPAddresses/join/action | 加入一个公用 IP 地址 |
| Microsoft.Network/publicIPAddresses/read | 读取网络公用 IP 地址 |
| Microsoft.Network/virtualNetworks/subnets/join/action | 加入虚拟网络 |
| Microsoft.Resources/deployments/operations/read | 读部署操作 |
| Microsoft.Resources/deployments/read | 阅读部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Storage/storageAccounts/listKeys/action | 列表中存储帐户关键字 |

### <a name="dns-zone-contributor"></a>DNS 区域参与者
可以管理 DNS 区域和记录。

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/\*/读取 | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/\* | 创建和管理预警规则 |
| Microsoft.Network/dnsZones/\* | 创建和管理 DNS 区域和记录 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/\* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/\* | 创建和管理支持票证 |


### <a name="documentdb-account-contributor"></a>投稿人 DocumentDB 帐户
可以管理 DocumentDB 帐户

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.DocumentDb/databaseAccounts/* | 创建和管理 DocumentDB 帐户 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="intelligent-systems-account-contributor"></a>智能系统帐户参与者
可以管理智能系统帐户

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.IntelligentSystems/accounts/* | 创建和管理智能系统帐户 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="network-contributor"></a>网络参与者
可以管理所有网络资源

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.Network/* | 创建和管理网络 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="new-relic-apm-account-contributor"></a>新的 Relic APM 帐户参与者
新的 Relic 应用程序性能管理帐户和应用程序可以管理

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |
| NewRelic.APM/accounts/* | 创建和管理新的 Relic 应用程序性能管理帐户 |

### <a name="owner"></a>所有者
可以管理所有内容，包括访问

| **操作** ||
| ------- | ------ |
| * | 创建和管理所有类型的资源 |

### <a name="reader"></a>读取器
可以查看所有信息，但不能更改

| **操作** ||
| ------- | ------ |
| * / 读取 | 读取所有的类型，除机密信息的资源。 |

### <a name="redis-cache-contributor"></a>Redis 高速缓存参与者
可以管理 Redis 高速缓存

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Cache/redis/* | 创建和管理 Redis 高速缓存 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="scheduler-job-collections-contributor"></a>计划程序作业集合参与者
可以管理调度程序作业集合

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Scheduler/jobcollections/* | 创建和管理作业集合 |
| Microsoft.Support/* | 创建和管理支持票证  |

### <a name="search-service-contributor"></a>搜索服务参与者
可以管理搜索服务

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Search/searchServices/* | 创建和管理搜索服务 |
| Microsoft.Support/* | 创建和管理支持票证  |

### <a name="security-manager"></a>安全管理器
可以管理安全组件、 安全策略和虚拟机

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.ClassicCompute/*/read | 阅读经典计算虚拟机的配置信息 |
| Microsoft.ClassicCompute/virtualMachines/*/write | 编写配置虚拟机 |
| Microsoft.ClassicNetwork/*/read | 阅读经典的网络配置信息  |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Security/* | 创建和管理安全的组件和策略 |
| Microsoft.Support/* | 创建和管理支持票证  |

### <a name="sql-db-contributor"></a>SQL 数据库参与者
可以管理 SQL 数据库但不是及其与安全相关的策略

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 读的角色和角色分配 |
| Microsoft.Insights/alertRules/* | 创建和管理预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Sql/servers/databases/* | 创建和管理 SQL 数据库 |
| Microsoft.Sql/servers/read | SQL 服务器中读取 |
| Microsoft.Support/* | 创建和管理支持票证 |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | 不能编辑审核策略 |
| Microsoft.Sql/servers/databases/auditingSettings/* | 不能编辑审核设置 |
| Microsoft.Sql/servers/databases/auditRecords/read  | 无法读取审核记录 |
| Microsoft.Sql/servers/databases/connectionPolicies/* | 不能编辑连接策略 |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | 不能编辑数据屏蔽策略 |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | 不能编辑安全警报策略 |
| Microsoft.Sql/servers/databases/securityMetrics/* | 不能编辑的安全度量标准 |

### <a name="sql-security-manager"></a>SQL 安全管理器
可以管理与安全相关的 SQL 服务器和数据库的策略

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 阅读 Microsoft 授权 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Sql/servers/auditingPolicies/* | 创建和管理 SQL 服务器的审核策略 |
| Microsoft.Sql/servers/auditingSettings/* | 创建和管理 SQL 服务器审核设置 |
| Microsoft.Sql/servers/databases/auditingPolicies/* | 创建和管理 SQL 服务器数据库审核策略 |
| Microsoft.Sql/servers/databases/auditingSettings/* | 创建和管理 SQL server 数据库审核设置 |
| Microsoft.Sql/servers/databases/auditRecords/read | 读取审计记录 |
| Microsoft.Sql/servers/databases/connectionPolicies/* | 创建和管理 SQL 服务器数据库连接策略 |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | 创建和管理 SQL server 数据库数据屏蔽策略 |
| Microsoft.Sql/servers/databases/read | 读 SQL 数据库 |
| Microsoft.Sql/servers/databases/schemas/read | 读 SQL 服务器数据库架构 |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | 读 SQL 服务器数据库表中的列 |
| Microsoft.Sql/servers/databases/schemas/tables/read | 读 SQL server 数据库表 |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | 创建和管理 SQL 服务器数据库安全警报策略 |
| Microsoft.Sql/servers/databases/securityMetrics/* | 创建和管理 SQL 服务器数据库安全指标 |
| Microsoft.Sql/servers/read | SQL 服务器中读取 |
| Microsoft.Sql/servers/securityAlertPolicies/* | 创建和管理 SQL 服务器安全警报策略 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="sql-server-contributor"></a>SQL Server 参与者
可以在 SQL 服务器和数据库，但不是及其与安全相关的策略管理

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取|
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Sql/servers/* | 创建和管理 SQL 服务器 |
| Microsoft.Support/* | 创建和管理支持票证 |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | 无法编辑 SQL 服务器的审核策略 |
| Microsoft.Sql/servers/auditingSettings/* | 无法编辑 SQL 服务器审核设置 |
| Microsoft.Sql/servers/databases/auditingPolicies/* | 无法编辑 SQL 服务器数据库审核策略 |
| Microsoft.Sql/servers/databases/auditingSettings/* | 无法编辑 SQL server 数据库审核设置 |
| Microsoft.Sql/servers/databases/auditRecords/read  | 无法读取审核记录 |
| Microsoft.Sql/servers/databases/connectionPolicies/* | 无法编辑 SQL 服务器数据库连接策略 |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | 无法编辑 SQL 服务器数据库数据屏蔽策略 |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | 无法编辑 SQL 服务器数据库安全警报策略 |
| Microsoft.Sql/servers/databases/securityMetrics/* | 无法编辑 SQL 服务器数据库安全指标 |
| Microsoft.Sql/servers/securityAlertPolicies/* | 无法编辑 SQL 服务器安全警报策略 |

### <a name="classic-storage-account-contributor"></a>传统的存储帐户参与者
可以管理经典存储帐户

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.ClassicStorage/storageAccounts/* | 创建和管理存储帐户 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="storage-account-contributor"></a>存储帐户参与者
可以管理存储帐户，但不是访问他们。

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 阅读所有的授权 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.Insights/diagnosticSettings/* | 管理诊断设置 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Storage/storageAccounts/* | 创建和管理存储帐户 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="user-access-administrator"></a>用户访问管理员
可以管理用户对 Azure 的资源访问权限

| **操作** ||
| ------- | ------ |
| * / 读取 | 读取所有的类型，除机密信息的资源。 |
| Microsoft.Authorization/* | 管理授权 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="classic-virtual-machine-contributor"></a>传统虚拟机参与者
经典的虚拟机，但不是在虚拟网络或存储帐户连接到可以管理

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.ClassicCompute/domainNames/* | 创建和管理经典计算域名 |
| Microsoft.ClassicCompute/virtualMachines/* | 创建和管理虚拟机 |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | 加入网络安全组 |
| Microsoft.ClassicNetwork/reservedIps/link/action | 链接保留的 IPs |
| Microsoft.ClassicNetwork/reservedIps/read | 读取的保留 IP 地址 |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | 加入虚拟网络 |
| Microsoft.ClassicNetwork/virtualNetworks/read | 虚拟网络中读取 |
| Microsoft.ClassicStorage/storageAccounts/disks/read | 读取存储帐户磁盘 |
| Microsoft.ClassicStorage/storageAccounts/images/read | 读取存储帐户图像 |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | 列表中存储帐户关键字 |
| Microsoft.ClassicStorage/storageAccounts/read | 阅读经典的存储帐户 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="virtual-machine-contributor"></a>虚拟机参与者
可以创建新的虚拟机，但不是在虚拟网络或存储帐户连接到

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.Compute/availabilitySets/* | 创建和管理计算可用性设置 |
| Microsoft.Compute/locations/* | 创建和管理计算位置 |
| Microsoft.Compute/virtualMachines/* | 创建和管理虚拟机 |
| Microsoft.Compute/virtualMachineScaleSets/* | 创建和管理虚拟机的扩展集 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | 加入网络应用程序网关后端地址池 |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | 加入负载平衡器的后端地址池 |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | 加入负载平衡器站 NAT 池 |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | 加入负载平衡入站 NAT 规则 |
| Microsoft.Network/loadBalancers/read | 读负载平衡器 |
| Microsoft.Network/locations/* | 创建和管理网络位置 |
| Microsoft.Network/networkInterfaces/* | 创建和管理网络接口 |
| Microsoft.Network/networkSecurityGroups/join/action | 加入网络安全组 |
| Microsoft.Network/networkSecurityGroups/read | 读取网络安全组 |
| Microsoft.Network/publicIPAddresses/join/action | 加入网络的公用 IP 地址 |
| Microsoft.Network/publicIPAddresses/read | 读取网络公用 IP 地址 |
| Microsoft.Network/virtualNetworks/read | 虚拟网络中读取 |
| Microsoft.Network/virtualNetworks/subnets/join/action | 加入虚拟网络的子网 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Storage/storageAccounts/listKeys/action | 列表中存储帐户关键字 |
| Microsoft.Storage/storageAccounts/read | 读取存储帐户 |
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="classic-network-contributor"></a>传统的网络参与者
经典的虚拟网络和保留的 Ip，可以管理

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.ClassicNetwork/* | 创建和管理经典网络 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Support/* | 创建和管理支持票证 |

### <a name="web-plan-contributor"></a>网站计划参与者
可以管理网站计划

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Support/* | 创建和管理支持票证 |
| Microsoft.Web/serverFarms/* | 创建和管理服务器场 |

### <a name="website-contributor"></a>网站讨论参与者
可以管理网站，但无法连接到的 web 计划

| **操作** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | 授权读取 |
| Microsoft.Insights/alertRules/* | 创建和管理见解预警规则 |
| Microsoft.Insights/components/* | 创建和管理组件的见解 |
| Microsoft.ResourceHealth/availabilityStatuses/read | 读取资源的运行状况 |
| Microsoft.Resources/deployments/* | 创建和管理资源组部署 |
| Microsoft.Resources/subscriptions/resourceGroups/read | 读取资源组 |  
| Microsoft.Support/* | 创建和管理支持票证 |
| Microsoft.Web/certificates/* | 创建和管理网站证书 |
| Microsoft.Web/listSitesAssignedToHostName/read | 阅读站点被指定到一个主机名 |
| Microsoft.Web/serverFarms/join/action | 加入服务器场 |
| Microsoft.Web/serverFarms/read | 读取服务器场 |
| Microsoft.Web/sites/* | 创建和管理网站 |

## <a name="see-also"></a>请参见
- [基于角色的访问控制](role-based-access-control-configure.md)︰ 开始使用 RBAC 在 Azure 的门户。
- [在 Azure RBAC 中的自定义角色](role-based-access-control-custom-roles.md)︰ 学习如何创建自定义角色以满足您的访问需求。
- [创建访问变更历史记录报告](role-based-access-control-access-change-history-report.md)︰ 跟踪的 RBAC 中不断变化的角色分配。
- [基于角色的访问控制故障排除](role-based-access-control-troubleshooting.md)︰ 获取有关如何解决常见的问题的建议。
