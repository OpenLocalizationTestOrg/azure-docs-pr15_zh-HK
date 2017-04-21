<properties 
    pageTitle="创作应用程序逻辑定义 |Microsoft Azure" 
    description="学习如何编写应用程序逻辑的 JSON 定义" 
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
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>作者逻辑应用程序定义
本主题演示如何使用[Azure 逻辑应用程序](app-service-logic-what-are-logic-apps.md)定义，它是一种简单的声明性 JSON 语言。 您还没有这样操作，请先检查了[如何创建新的逻辑应用程序](app-service-logic-create-a-logic-app.md)。 您还可以阅读[MSDN 上定义语言的完整参考资料](http://aka.ms/logicappsdocs)。

## <a name="several-steps-that-repeat-over-a-list"></a>一组重复的几个步骤

您可以利用[foreach 类型](app-service-logic-loops-and-scopes.md)重复对达 10k 的数组项并为每个执行的操作。

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>如果出现错误的故障处理步骤

您通常想要能够编写一个*补救步骤*— — 一些逻辑来执行，如果**且仅当**、 一个或多个您的调用失败。 在此示例中，我们得到的数据从不同的地方，但如果调用失败，我想这样我就可以捕捉该故障以后某处发布的消息︰  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

您可以使用`runAfter`属性指定`postToErrorMessageQueue`应该只运行`readData`为**失败**。  这也可能是一份可能的值，因此`runAfter`可能是`["Succeeded", "Failed"]`。

最后，现在已处理错误，因为我们不再将标记运行**出现故障**。 可以在此处看到，此次运行是**成功**即使一个步骤失败，因为我编写的步骤来处理此错误。

## <a name="two-or-more-steps-that-execute-in-parallel"></a>两个 （或更多） 并行执行的步骤

有多个操作执行并行，`runAfter`属性必须在运行时等效。 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

您可以看到在上面的示例，两个`branch1`和`branch2`设置为将要运行`readData`。 因此，这两个这些分支将并行运行︰

![并行](./media/app-service-logic-author-definitions/parallel.png)

您可以看到两个分支的时间戳是相同的。 

## <a name="join-two-parallel-branches"></a>加入两个并行分支

您可以加入两个设置可以通过将项添加到并行执行的操作`runAfter`属性类似于上面。

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![并行](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>将列表中的项映射到一些不同的配置

下一步，假设我们想要获得完全不同的内容，具体取决于属性的值。 我们可以创建映射到目标值的参数为︰  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

在这种情况下，我们首先获取文章的列表，然后第二步，查找映射，基于作为参数，以获取中的内容的 URL 定义的类别中。 

这里要注意两个项目︰[`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection)函数用于检查类别匹配定义的已知类别之一。 第二，一旦我们获取类别，我们可以提取使用方括号的映射的项︰ `parameters[...]`。 

## <a name="working-with-strings"></a>使用字符串

有多种可用于操作字符串的函数。 让我们看一个例子，我们有我们想要传递到系统中，一个字符串，但我们没有把握，字符编码都可以正确处理。 一种选择是为 base64 编码此字符串。 但是，若要避免在 URL 中转义我们要更换几个字符。 

我们还想要的子字符串的顺序命名，因为未使用的前五个字符。

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

从内向外的工作︰

1. 获得[`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length)的订货人的名称，此方法返回回总字符数

2. （因为我们需要较短的字符串） 中减去 5

3. 实际采用[`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring)。 我们开始索引处`5`和转到该字符串的其余部分。

4. 将转换为此子字符串[`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64)字符串

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)所有的`+`与字符`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)所有的`/`与字符`_`

## <a name="working-with-date-times"></a>使用日期时间

日期时间很有用，尤其是当您试图从自然不支持**触发器**的数据源提取数据。  您可以使用日期时间算出花费的时间长度不同的步骤。 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

在此示例中，我们抽取`startTime`的一步。 然后我们会获取当前时间减去一秒︰[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (无法使用其他时间单位，如`minutes`或`hours`)。 最后，我们可以将这两个值进行比较。 如果第一种是小于第二个，然后这意味着多一秒后顺序是第一个经过放置。 

另外请注意，我们可以使用字符串格式化日期格式︰ 在使用查询字符串[`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow)以获取 RFC1123。 所有的日期[记录在 MSDN 上](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow)的格式设置。 

## <a name="using-deployment-time-parameters-for-different-environments"></a>使用不同的环境的部署时间参数

它是通常有部署生命周期中必须开发环境、 一个临时环境和生产环境。 在所有这些可能需要相同的定义，但使用不同的数据库，例如。 同样，您可能需要获得高可用性，在许多不同的地区使用相同的定义，但希望可以与该区域数据库每个逻辑应用程序实例。 

请注意，这是不同的则应使用采用不同参数在*运行时*，`trigger()`与上面加注。 

您可以从这个例子非常简单的定义开始︰

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

然后，在实际`PUT`请求的逻辑应用程序可以提供参数`uri`。 请注意，由于不再是逻辑应用程序负载的需要此参数的默认值︰

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

然后可以在每个环境中提供不同的值`connection`参数。 

请参阅[REST API 文档](https://msdn.microsoft.com/library/azure/mt643787.aspx)所有您具有用于创建和管理逻辑的应用程序的选项。 
