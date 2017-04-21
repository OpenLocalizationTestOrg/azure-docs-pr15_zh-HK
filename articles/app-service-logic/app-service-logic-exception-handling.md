<properties
   pageTitle="逻辑应用程序异常处理 |Microsoft Azure"
   description="了解错误和异常处理使用 Azure 逻辑应用程序模式"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>逻辑应用程序错误和异常处理

逻辑应用程序提供一组丰富的工具和模式，以帮助确保您的集成是强健和针对故障复原能力。  与任何集成体系结构的挑战之一确保停机时间或从属系统的问题能够得到正确处理。  逻辑应用程序可以处理错误第一类的经验，为您提供了您需要处理异常以及在工作流中的错误的工具。

## <a name="retry-policies"></a>重试策略

异常和错误处理的最基本类型是一个重试策略。  此策略定义如果初始请求超时或失败，应该重试该操作 (429 所导致的任何请求或 5xx 响应)。  默认情况下，所有操作请重试 4 次超过 20 秒的间隔。  因此，如果第一次请求接收`500 Internal Server Error`响应，工作流引擎会暂停为 20 秒，并尝试请求再次。  如果在所有重试后响应仍异常或故障，工作流将继续并标记操作状态为`Failed`。

您可以配置重试策略中的特定操作**的输入**。  可以配置重试策略尝试多达 4 次以上 1 小时间隔。  输入属性的详细信息可以[在 MSDN 上找到][retryPolicyMSDN]。

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

如果您希望您的 HTTP 操作重试 4 次，并等待 10 分钟，每次尝试之间将具有以下定义︰

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

支持的语法的详细信息，查看[MSDN 上的重试策略部分]的[retryPolicyMSDN]。

## <a name="runafter-property-to-catch-failures"></a>RunAfter 属性来捕获失败

每个逻辑应用程序操作声明哪些操作需要完成，才会开始操作。  您可以将此视为工作流中的步骤的顺序。  这种排序被称为`runAfter`操作定义中的属性。  它是描述的操作和操作状态执行操作的对象。  默认情况下，通过设计器添加的所有操作都设置为`runAfter`如果上一步一步`Succeeded`。  但是，您可以自定义此值前面的操作时触发的操作`Failed`， `Skipped`，或一组可能这些值。  如果您想要在特定的操作后将项目添加到指定的服务总线主题`Insert_Row`失败，您将使用以下`runAfter`配置︰

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

请注意`runAfter`属性设置为如果触发`Insert_Row`操作是`Failed`。  运行操作，如果行动的状态为`Succeeded`， `Failed`，或`Skipped`语法可能是︰

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] 操作运行后前面的操作已经失败，并成功完成，将被标记为`Succeeded`。  这种现象意味着，如果您在工作流中成功捕捉所有故障运行本身被标记为`Succeeded`。

## <a name="scopes-and-results-to-evaluate-actions"></a>要计算操作的作用域和结果

类似于如何运行后的各个操作，也可以组操作[范围](app-service-logic-loops-and-scopes.md)-内一起充当操作的逻辑分组。  作用域可同时用于组织您的逻辑应用程序操作和执行聚合计算作用域的状态。  在作用域内的所有操作均已都完成后，本身的范围将接收状态。  如果执行分支中的最后一个操作是，确定作用域状态与相同条件与运行-`Failed`或`Aborted`状态是`Failed`。

您可以`runAfter`作用域已被标记`Failed`以触发特定操作的范围内发生的任何错误。  运行一个作用域失败后使您可以创建一个操作以便捕获失败，如果范围内的*任何*操作都失败。

### <a name="getting-the-context-of-failures-with-results"></a>获取上下文的结果与故障

捕获失败从范围非常有用，但您可能还想要了解完全失败，哪些操作的上下文和任何错误或返回的状态代码。  `@result()`工作流函数提供上下文为作用域内的所有操作的结果。

`@result()`采用单个参数、 作用域名称，然后返回数组的所有操作结果从该范围内。  这些行动的对象包括相同的特性`@actions()`对象，包括操作开始时间、 行动结束时间、 操作状态、 操作的输入、 操作相关 Id，以及操作输出。  您可以轻松地对`@result()`作用`runAfter`在一个范围内发送的任何失败的操作的上下文。

如果您想要在作用域中执行的*每个*操作动作， `Failed`，您可以对`@result()`使用**[滤镜数组](../connectors/connectors-native-query.md)**的操作和**[ForEach](app-service-logic-loops-and-scopes.md)**循环。  这使您可以筛选结果为失败的操作的数组。  可以采用筛选的结果数组，并使用**ForEach**循环每次失败的执行某项操作。  下面是一个示例，后面的详细说明。  本示例将范围内发送 HTTP POST 请求响应正文的所有失败的操作`My_Scope`。

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

这是正在发生的事情的详细的演练︰

1. 要筛选的**滤镜数组**操作`@result('My_Scope')`以获取内的所有操作的结果`My_Scope`
1. **滤镜数组**的条件是任何`@result()`项状态等于`Failed`。  这将筛选的所有操作结果的数组`My_Scope`到仅失败的操作结果的数组。
1. 在**筛选数组**输出来执行**每个**操作。  这将执行操作*为每个*失败操作结果我们筛选的上方。
    - 如果没有失败，在操作的作用域中的单个操作`foreach`将只运行一次。  许多失败的操作会导致每个失败的一个行动。
1. 发送 HTTP POST`foreach`项响应正文或`@item()['outputs']['body']`。  `@result()`项目的形状是相同`@actions()`形状，并可以相同的方式分析。
1. 此外包含两个自定义标头，包含失败的操作名称`@item()['name']`和故障运行客户端跟踪 ID `@item()['clientTrackingId']`。

为了便于参考，下面是一个示例`@result()`项。  您可以看到`name`， `body`，和`clientTrackingId`属性分析在上面的示例。  值得注意的是，外部的`foreach`，`@result()`返回一个数组，这些对象。

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

上面的表达式可用于执行不同的异常处理模式。  您可以选择要执行一个异常处理接受失败和删除整个筛选的数组范围之外的行动`foreach`。  您还可以包括其他有用的属性，从`@result()`上面所示的响应。

## <a name="azure-diagnostics-and-telemetry"></a>Azure 诊断和遥测

上面的图案是处理错误和运行中的异常的好方法，但还可以识别并响应独立运行本身的错误。  [Azure 诊断程序](app-service-logic-monitor-your-logic-apps.md)提供了一个简单的方法来发送到 Azure 存储帐户或 Azure 事件中心的所有工作流事件 （包括所有的运行和操作状态）。  您可以监视日志和衡量标准，或将它们发布到任何希望，以评估运行的状态的监视工具。  一种可能的选择是入[流分析](https://azure.microsoft.com/services/stream-analytics/)流通过 Azure 事件集线器的所有事件。  您可以在流分析从诊断日志编写出任何异常情况、 求平均值或故障的实时查询。  流分析可以方便地输出到其他数据源，如队列、 主题、 SQL、 DocumentDB 和电源 BI。

## <a name="next-steps"></a>下一步行动
- [请参阅如何一个客户构建可靠的错误处理逻辑的应用程序使用](app-service-logic-scenario-error-and-exception-handling.md)
- [查找更多的逻辑应用程序示例和应用场景](app-service-logic-examples-and-scenarios.md)
- [了解如何创建逻辑应用程序的自动化的部署](app-service-logic-create-deploy-template.md)
- [设计和部署从 Visual Studio 的逻辑应用程序](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9