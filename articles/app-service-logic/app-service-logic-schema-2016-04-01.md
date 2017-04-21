<properties 
    pageTitle="新架构版本 2016年-06-01 |Microsoft Azure" 
    description="学习如何编写逻辑应用程序最新版本的 JSON 定义" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>新架构版本 2016年-06-01

新的架构和 API 用于逻辑的应用程序版本具有大量的改进，可以提高可靠性和易用性，逻辑的应用程序。 有 3 个主要区别︰

1. 添加的作用域，其中包含的操作集合的操作。
1. 条件和循环是一类操作
1. 执行顺序详细通过`runAfter`属性 (的替换`dependsOn`)

有关逻辑应用程序能从 2015年-08-01-预览架构升级到 2016年-06-01 架构中，[签出下面的升级部分。](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1.范围

此架构中的最大变化之一就是添加的作用域和嵌套相互操作的能力。  当将一组操作组合在一起，或无需嵌套 （例如，条件可以包含其他条件） 相互操作，这是很有帮助。  作用域的语法上的更多详细信息可找到[这里](app-service-logic-loops-and-scopes.md)，但一个简单的作用域的示例可在下面找到︰


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2.条件和循环的更改

在以前版本的架构中，条件和循环已与单一操作关联的参数。  此架构中解除了这一限制，现在条件和循环显示为一种类型的操作。  [在这篇文章中](app-service-logic-loops-and-scopes.md)，找不到更多的信息和条件操作的简单示例如下所示︰

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3.RunAfter 属性

新`runAfter`属性替换`dependsOn`来帮助运行订购允许更高的精度。  `dependsOn`但是很多时候需要执行某个操作，如果上一步操作是成功，失败，或者跳过已与"操作运行和成功，"同义。  `runAfter`允许这一灵活性。  它是一个指定的操作名称，它将运行后，所有的对象，并定义状态的可接受到从触发器的数组。  例如，如果您想要运行后步骤成功 A 和 B 是成功还是失败，对构造以下`runAfter`属性︰

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>升级到 2016年-06-01 架构

升级到新的 2016年-06-01 架构只需几个步骤。  [在这篇文章中](app-service-logic-schema-2016-04-01.md)找不到架构中的更改的详细信息。  升级过程包括运行升级脚本另存为一个新的逻辑应用程序，并且可能会覆盖掉旧的逻辑应用程序，如果需要。

1. 打开您当前的逻辑应用程序。
1. 单击工具栏中的**更新架构**按钮
   
    ![][1]
   
    将返回已升级的定义。  可以复制并粘贴到一个资源定义，如果需要但我们**强烈建议**您使用**另存为**按钮以确保所有连接引用是否有效升级的逻辑应用程序中。
1. 单击升级刀片式服务器的工具栏中的**另存为**按钮。
1. 填写名称和逻辑应用程序状态并单击**创建**来部署您升级逻辑的应用程序。
1. 验证已升级的逻辑应用程序工作正常。

    >[AZURE.NOTE] 如果您使用的手动或请求触发器，将新的逻辑应用程序中更改回调 URL。  使用新的 URL 验证端到端运行方式、 您可以克隆到您现有的逻辑应用程序保留先前的 Url。

1. *可选*使用 （如上图所示**更新模式**图标旁） 工具栏中的**克隆**按钮覆盖您先前的逻辑应用程序与新的架构版本。  这是有必要的只有当您想要保持相同的资源 ID，或请求触发器逻辑应用程序的 URL。

### <a name="upgrade-tool-notes"></a>升级工具说明

#### <a name="condition-mapping"></a>状态映射

该工具将尽最大努力组一起在已升级的定义范围中的 true 和 false 的分支操作。  特别是设计器模式的`@equals(actions('a').status, 'Skipped')`应显示为`else`操作。  但是如果该工具检测到无法识别它的模式可能会创建单独的条件为 true 和 false 的分支。  操作可以重新映射，如果需要发送升级。

#### <a name="foreach-with-condition"></a>有条件的 ForEach
  
筛选器操作的新架构中，可以复制之前的模式，每个项目的条件的 foreach 循环。  这种情况会自动升级。  筛选器操作之前 （以返回仅与条件匹配的项的数组） 的 foreach 循环，则该条件成为，该数组传递给 foreach 操作。  您可以查看此[本文中](app-service-logic-loops-and-scopes.md)的示例

#### <a name="resource-tags"></a>资源标记

在升级，资源标记将被删除，您需要升级工作流程的重新设置。

## <a name="other-changes"></a>其他更改

### <a name="manual-trigger-renamed-to-request-trigger"></a>手动重命名为请求触发器的触发器

该类型`manual`被否决，重命名为`request`类的`http`。  这是更符合该触发器用于生成的图案的类型。

### <a name="new-filter-action"></a>新的筛选器操作

如果您正在使用大型数组以及需要较小的一组项目下对其进行筛选，您可以使用新的筛选器类型。  它接受一个数组和一个条件和将评估每一项的条件并返回满足条件的项的数组。

### <a name="foreach-and-until-action-restrictions"></a>ForEach 且直到操作限制

Foreach，直到循环并且限制为单个操作。

### <a name="trackedproperties-on-actions"></a>在操作上的 TrackedProperties

操作可以拥有的其他属性 (同级`runAfter`， `type`) 调用`trackedProperties`。  它是一个对象，指定某些操作的输入或输出包括在 Azure 诊断遥测发出的作为工作流的一部分。  例如︰

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>下一步行动
- [使用逻辑应用程序工作流定义](app-service-logic-author-definitions.md)
- [创建逻辑应用程序部署模板](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
