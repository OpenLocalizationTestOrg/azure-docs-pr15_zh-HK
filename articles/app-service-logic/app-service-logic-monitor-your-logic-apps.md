<properties 
    pageTitle="监视您在 Azure 应用程序服务的逻辑应用程序 |Microsoft Azure" 
    description="如何查看您的逻辑应用程序做了什么" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>监视您的逻辑应用程序

之后[创建逻辑应用程序](app-service-logic-create-a-logic-app.md)时，可以看到它在 Azure 门户执行的完整历史记录。  您还可以设置类似 Azure 诊断和 Azure 警报服务监视实时、 事件和警报的事件您喜欢"在 5 个以上运行失败在一个小时内。

## <a name="monitor-in-the-azure-portal"></a>Azure 门户中的监视器

要查看历史记录，请选择**浏览**，并选择**逻辑应用程序**。 将显示在您的订阅中的所有逻辑应用程序的列表。  选择您要监视的逻辑应用程序。  您将看到的所有操作和触发器已发生此逻辑应用程序的列表。

![概述](./media/app-service-logic-monitor-your-logic-apps/overview.png)

有此刀片式服务器上的一些有用的部分︰

- **摘要**列出了**所有运行**和**触发器历史记录**
    - **所有运行**列表中最新的逻辑应用程序运行。  您可以单击任意行的详细信息在运行，或单击该图块可以列出多个。
    - **触发器历史记录**列出此逻辑应用程序中的所有触发器记录。  触发活动可能被"跳过"检查新数据 （例如查找新文件是否已添加到 FTP），意味着返回数据触发逻辑的应用程序，"成功"或"失败"，相应的配置中的错误。
- **诊断程序**使您可以查看运行时详细信息和事件，并订阅[Azure 警报](#adding-azure-alerts)

>[AZURE.NOTE] 逻辑应用程序服务中的静态加密所有的运行库的详细信息和事件。 他们只是从用户的查看请求后进行解密。 此外可以控制对这些事件访问 Azure Role-Based 的访问控制 (RBAC)。

### <a name="view-the-run-details"></a>查看运行的详细信息

运行此列表显示**状态**、**开始时间**和**持续时间**的特定运行。 选择任意行以查看运行的详细信息。

监视视图显示了每个步骤的运行、 输入和输出，并可能有 occurre 任何错误消息。

![运行和操作](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

如果您需要运行**关联 ID** （即可以使用 REST API） 像任何其他详细信息，您可以单击**运行详细信息**按钮。  这包括所有步骤、 状态和输入/输出的运行。

## <a name="azure-diagnostics-and-alerts"></a>Azure 诊断和警报

除了通过 Azure 门户和 REST API，上面提供的详细信息，您可以配置您的逻辑的应用程序更丰富的细节和调试使用 Azure 诊断。

1. 单击逻辑应用程序刀片式服务器的**诊断**部分
1. 单击此处，配置**诊断设置**
1. 配置事件集线器或存储帐户发出的数据

    ![Azure 诊断设置](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>添加 Azure 的警报

一旦配置了诊断程序，您可以添加 Azure 跨越特定阈值时触发警报。  在**诊断**刀片式服务器，选择**警报**拼贴和**添加通知**。  这将引导您完成配置警报基于多阈值和衡量标准。

![Azure 预警指标](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

您可以根据需要配置**条件**、**阈值**和**期**。  最后，您可以配置一个电子邮件地址发送通知，或配置 webhook。  可以使用逻辑应用程序中的[请求触发](../connectors/connectors-native-reqres.md)警报以及 （若要执行操作，例如[发送到可宽延时间](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)、[发送文本](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)或[添加到队列的消息](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)） 上运行。

### <a name="azure-diagnostics-settings"></a>Azure 诊断设置

每个事件包含有关应用程序逻辑和状态之类的事件的详细信息。  以下是*ActionCompleted*事件的一个示例︰

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

尤其是用于跟踪和监视这两个属性是*clientTrackingId*和*trackedProperties*。  

#### <a name="client-tracking-id"></a>客户端跟踪 ID

客户端跟踪 ID 是将跨逻辑应用程序运行，包括嵌套调用作为逻辑的应用程序的一部分的工作流关联事件的值。  此 ID 将自动生成，如果不被提供，但您可以手动指定客户端通过从触发器跟踪 ID `x-ms-client-tracking-id` （请求触发器、 HTTP 触发器或 webhook 触发器） 的触发器请求中的 ID 值的标头。

#### <a name="tracked-properties"></a>跟踪的属性

跟踪的属性可以添加到跟踪输入工作流定义中的操作或在诊断数据的输出。  这很有用，如果您想要跟踪数据，例如您遥测中的"订单 ID"。  若要添加跟踪的属性，包括`trackedProperties`采取某一操作的属性。  跟踪的属性可以只跟踪单个操作的输入和输出，但是您可以使用`correlation`跨运行中的操作关联的事件的属性。

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>扩展您的解决方案

您可以利用从事件集线器或存储此遥测到其他服务，如[操作管理套件](https://www.microsoft.com/cloud-platform/operations-management-suite)、 [Azure 流分析](https://azure.microsoft.com/services/stream-analytics/)和[电源 BI](https://powerbi.com) ，能够实时监视您的集成工作流。

## <a name="next-steps"></a>下一步行动
- [常见的示例和应用场景的逻辑应用程序](app-service-logic-examples-and-scenarios.md)
- [创建逻辑应用程序部署模板](app-service-logic-create-deploy-template.md)
- [企业集成功能](app-service-logic-enterprise-integration-overview.md)
