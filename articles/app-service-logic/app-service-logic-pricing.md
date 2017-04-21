<properties 
    pageTitle="定价模型的逻辑应用程序 |Microsoft Azure" 
    description="有关定价的逻辑应用程序中的工作方式的详细信息" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>逻辑应用程序定价模型

Azure 的逻辑应用程序允许您扩展并在云中执行集成工作流。  以下是计费和定价的逻辑应用程序计划的详细信息。

## <a name="consumption-pricing"></a>消耗定价

新创建的逻辑应用程序使用消耗计划。 与逻辑应用程序消耗的定价模型，只需付费为您的使用。  逻辑应用程序不会阻止使用消耗计划时。
计量是逻辑的应用程序实例运行中执行的所有操作。

### <a name="what-are-action-executions"></a>什么是操作执行？

逻辑应用程序定义中的每一步都是操作。  这包括触发器，直到循环，联络接头和调用的本机操作条件，范围，for each 循环，一样的控制流步骤。
触发器是只需特殊操作可用于某个特定事件发生时实例化逻辑的应用程序的新实例。  有多种不同的触发器可能会影响如何测量逻辑应用程序的行为。

-   直到收到符合条件创建逻辑应用程序的新实例的消息进行**轮询触发器**– 此触发器不断轮询终结点。  在触发器逻辑应用程序设计器中，可以配置轮询间隔。  每个轮询请求，即使它不会创建新实例的逻辑的应用程序，将算作操作执行。

-   **Webhook 触发器**– 此触发器等待客户端发送其请求对特定终结点。  每个请求发送到 webhook 的终结点可以算是操作执行。 请求和 HTTP Webhook 触发器都是这两个 webhook 触发器。

-   **定期触发器**– 此触发器将创建基于重复出现的间隔配置触发器中的逻辑应用程序的新实例。  例如，可以配置定期触发器运行每隔 3 天或甚至每一分钟。

逻辑应用程序资源刀片式服务器触发器历史部分中，可以看到触发器的执行。

已执行的所有操作是否已成功或失败计量作为操作执行。  不作为操作执行计算操作跳过由于不符合条件或未执行，因为逻辑应用程序终止之前完成的动作。

每次循环迭代计数是在循环中执行的操作。  例如，在单个动作 10 项列表的每个循环迭代将算作乘以循环 (1) 中的操作的数目的列表 (10) 中的项目数加上一个循环的在此示例中，将会是 (10 * 1) 启动 + 1 = 11 操作执行。

被禁用的应用程序的逻辑不能实例化新的实例，因此在它们被禁用时不获取付费。  请注意禁用逻辑应用程序后可能要花一点时间来停止实例之前被完全禁用。

## <a name="app-service-plans"></a>应用程序服务计划

创建逻辑应用程序不再需要的应用程序服务计划。  您也可以引用现有的逻辑应用程序与应用程序服务计划。  逻辑应用程序以前创建的应用程序服务计划将继续像以前一样，根据该计划选择，将获得来控制每日攻击的次数超过了并不会使用操作执行计量计费后的行为。

应用程序服务计划，每日允许的操作执行︰

| |自由/共享/基本|标准|高级|
|---|---|---|---|
|每日操作执行| 200|10000|50000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>从消耗转换为应用程序服务计划定价

若要引用逻辑应用程序消耗的计划应用程序服务，您可以只是[运行下面 PowerShell 脚本](https://github.com/logicappsio/ConsumptionToAppServicePlan)。  请确保您首先必须安装了[Azure PowerShell 工具](https://github.com/Azure/azure-powershell)。

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>从应用程序服务计划定价消耗转换

要改变计划到消耗模型与之相关联的应用程序服务都有一个逻辑应用程序逻辑的应用程序定义中删除应用程序服务计划的参考。  这可以为 PowerShell cmdlet 的调用︰

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>定价

有关定价的详细信息请参阅[逻辑应用程序定价](https://azure.microsoft.com/pricing/details/logic-apps/)。

## <a name="next-steps"></a>下一步行动

- [逻辑应用程序概述][whatis]
- [创建您的第一个逻辑应用程序][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

