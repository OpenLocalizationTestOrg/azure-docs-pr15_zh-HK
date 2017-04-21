<properties 
    pageTitle="新架构版本 2015年-08-01-预览" 
    description="学习如何编写逻辑应用程序最新版本的 JSON 定义" 
    authors="stepsic-microsoft-com" 
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
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>新架构版本 2015年-08-01-预览

新的架构和 API 用于逻辑的应用程序版本具有大量的改进，可以提高可靠性和易用性，逻辑的应用程序。 有 4 个主要区别︰

1. **APIApp**操作类型已被更新为新的**APIConnection**操作类型。
2. **重复**已给**Foreach**重命名。
3. **HTTP 侦听程序**API 应用程序不再是必需的。
4. 调用子工作流将使用一个新架构。

## <a name="1-moving-to-api-connections"></a>1.转向 API 连接

最大变化是，不再需要将 API 的应用程序部署到 Azure 订阅以使用 API 的。 有 2 种方法可以使用 Api:
* 托管的 API
* 您自定义的 Web API

每个处理方式稍有不同，因为他们的管理和承载模型不同。 这种模型的优点之一是您正在不再受限于资源与部署在资源组中。 

### <a name="managed-apis"></a>托管的 Api

我们有大量的 API 的程序由 Microsoft 管理的以您的名义，例如 Office 365、 销售队伍、 Twitter、 FTP 等... 这些托管 API 的某些可用作-是，如翻译 Bing，而其他人则要求配置。 这种配置称为*连接*。

例如，当您使用 Office 365，您需要创建包含您登录令牌 Office 365 的连接。 此标记将安全地存储和刷新，以便您的逻辑应用程序总是可以调用 Office 365 API。 或者，如果您想要连接到您的 SQL 或 FTP 服务器，您需要创建连接字符串的连接。 

在定义这些操作被称为`APIConnection`。 下面是一个示例调用 Office 365 可以发送一封电子邮件的连接︰

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

特有的 API 连接输入的部分是`host`对象。 这包含两个部分︰ `api` ， `connection`。

`api`具有运行时承载 URL 的位置管理的 API。 您可以看到的所有可用的托管 Api 为您通过调用`GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`。

当您使用一个 API 时，它可能或没有定义任何**连接参数**。 如果不是，则需要没有**连接**。 如果是这样，您将需要创建一个连接。 当创建该连接时它可以有选择，名称，然后引用，在`connection`对象内`host`对象。 若要创建的资源组中的连接，调用︰

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

与下面的正文中︰


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>在 Azure 资源管理器模板部署托管 Api

只要它不要求交互登录，您可以在 ARM 模板中创建完整的应用程序。 如果它需要登录，您可以设置一切与 ARM 的模板中，但仍须对授权连接该门户访问。 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

您可以看到在此示例中的连接是只正常居住在资源组的资源。 他们引用您的订阅中提供的 managedAPIs。

### <a name="your-custom-web-apis"></a>您自定义的 Web Api

如果您使用您自己的 API 的 （特别是，不受 Microsoft 管理规则），然后应使用内置**HTTP**操作来调用它们。 为了获得理想的体验，您应该为您的 API 公开 swagger 终结点。 这将使逻辑应用程序设计器呈现您的 API 的输入和输出。 而 swagger，设计器将仅能显示的输入和输出为不透明的 JSON 对象。

下面是一个示例，演示新`metadata.apiDefinitionUrl`属性︰
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

如果要承载您**的应用程序**服务的 Web API，然后它将自动地显示在设计器中可用的操作列表。 如果没有，您必须直接粘贴在 URL 中。 才会在逻辑应用程序设计器中可用 （尽管可能安全 API 本身使用任何方法得到支持，在 Swagger），swagger 终结点必须未经验证。

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>已部署的 API 应用程序中使用 2015年-08-01-预览

如果您以前部署 API 的应用程序，可以通过**HTTP**操作来调用它。

例如，如果列出文件使用收存箱，您可能必须如下所示在**2014年-12-01-预览**架构版本定义︰

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

您可以构建 HTTP 操作像下面 （逻辑的应用程序定义保持不变的参数部分）︰

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

遍历一个地这些属性︰

| 操作属性 |  说明 |
| --------------- | -----------  |
| `type` | `Http`而不是`APIapp` |
| `metadata.apiDefinitionUrl` | 如果您想要在逻辑应用程序设计器中使用该操作，您需要包含的元数据终结点。 通过构造︰`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | 通过构造︰`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | 始终`POST` |
| `inputs.body` | 与 api 的应用程序参数 | 
| `inputs.authentication` | 与 api 应用程序身份验证 |

这种方法也适用于所有 API 的应用程序操作。 但是，请记住，这些以前的 API 应用程序不再受支持，并应移动到两个其他选项之一 （托管的 API 或承载您自定义的 Web API） 之上。

## <a name="2-repeat-renamed-to-foreach"></a>2.重命名为 Foreach 的重复

以前我们收到很多客户反馈的架构版本程序**重复**混乱不清和未正确捕获确实是有关每个循环。 因此，我们有给**Foreach**更名它。 例如︰

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

现在会写成︰

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

以前的函数`@repeatItem()`用于循环访问当前项引用。 这已得到简化，仅`@item()`。 

### <a name="referencing-the-outputs-of-the-foreach"></a>引用 Foreach 的输出
要进一步简化， **Foreach**操作的输出将不包括在名为**repeatItems**的对象。 这意味着，而上述重复的输出是︰

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

现在，它将为︰

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

当引用这些输出，获取到行动的主体必须做︰

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

现在，您可以改为︰

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

进行这些更改，函数`@repeatItem()`， `@repeatBody()` ，`@repeatOutputs()`被删除。

## <a name="3-native-http-listener"></a>3.本机 HTTP 侦听程序 
HTTP 侦听程序功能是现在内置的因此您不再需要部署 HTTP 侦听程序 API 的应用程序。 了解[如何在此处进行逻辑应用程序终结点可调用的所有详细信息](app-service-logic-http-endpoint.md)。 

进行这些更改，该函数`@accessKeys()`中删除并已替换为`@listCallbackURL()`函数获取终结点 （如果需要） 的目的。 此外，您现在必须定义至少一个触发器逻辑应用程序中立即。 如果您希望对`/run`工作流，您将需要有一种`manual`，`apiConnectionWebhook`或`httpWebhook`触发器。 

## <a name="4-calling-child-workflows"></a>4.电话子工作流

以前，调用子工作流要求转到该工作流，获取访问令牌，然后将程序中粘贴到要调用该子级的逻辑应用程序定义。 在新的架构版本，逻辑应用程序引擎将自动生成 SAS 子工作流，这意味着您不必将任何机密信息粘贴到该定义的运行时。  下面是一个示例︰

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

提高了第二个是我们将为您做的子工作流的完全访问权限对传入的请求。 这意味着，您可以将参数传递和*标题*对象的*查询*部分中，您可以完全定义的整个正文。

最后，有为的子工作流所需的更改。 而之前您可以只调用子工作流直接;现在，您将需要调用父工作流中定义的触发器终结点。 通常，这意味着将添加的类型**手动**触发，然后父定义中使用它。 请注意，`host`属性明确的`triggerName`，因为您必须始终指定要调用的触发器。

## <a name="other-changes"></a>其他更改

### <a name="new-queries-property"></a>新的查询属性
所有操作类型现在都支持新调用**查询**的输入。 这可以是结构化的对象，而不是您无需手动配置字符串。

### <a name="parse-function-renamed"></a>parse() 函数已重命名
我们将很快添加更多的内容类型，如`parse()`函数重命名为`json()`。

## <a name="coming-soon-enterprise-integration-apis"></a>即将推出︰ 企业集成 Api
在此时间点，我们不这样做，但具有托管的企业集成 Api （如 AS2) 可用的版本。 这些将即将根据[路线图](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)中的介绍。 在同时，您可以使用通过 HTTP 操作中，您现有的部署的 BizTalk Api，如覆盖上面的"使用 API 已部署应用程序。"
