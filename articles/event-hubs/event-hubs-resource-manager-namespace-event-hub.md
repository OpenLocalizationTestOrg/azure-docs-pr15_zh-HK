<properties
    pageTitle="与事件中心和使用者组使用 Azure 资源管理器模板创建事件集线器的命名空间 |Microsoft Azure"
    description="使用事件集线器和使用 Azure 资源管理器模板的使用者组创建事件集线器的命名空间"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>与事件中心和使用者组使用 Azure 资源管理器模板创建事件集线器的命名空间

本文介绍如何使用创建事件集线器命名空间与事件中心的使用者组 Azure 资源管理器模板。 您将了解如何定义部署哪些资源以及如何定义参数指定当执行部署。 可以将此模板用于您自己的部署，也可以进行自定义以满足您的需求

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器的模板][]。

完整的模板，请参阅在 GitHub 上的[事件中心和使用者组模板][]。

>[AZURE.NOTE]
>若要检查新的模板，请访问[Azure 快速启动模板][]库和搜索事件集线器。

## <a name="what-will-you-deploy"></a>将部署的内容？

使用此模板，您将部署与事件中心的使用者组事件集线器命名空间。

[事件集线器](../event-hubs/event-hubs-what-is-event-hubs.md)是事件处理服务用于向事件和遥测入口到 Azure 在巨大规模较大，提供低延迟和高可靠性。

若要部署都自动运行，请单击下面的按钮︰

[![将部署到 Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>参数

使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名的部分`Parameters`，其中包含所有参数值。 您应定义这些值，将基于您正在部署的项目或根据要部署到的环境而变化的参数。 未定义的值始终保持相同的参数。 每个参数值用于在模板中定义部署的资源。

模板定义以下参数。

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

要创建的事件集线器命名空间名称。

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

事件中集线器命名空间创建的事件中心的名称。

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

创建事件中心的使用者组的名称。

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

API 版本的模板。

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>若要部署的资源

创建命名空间的类型为**EventHubs**，与事件中心的使用者组。

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>要运行部署命令

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[创作的 Azure 资源管理器模板]: ../resource-group-authoring-templates.md
[Azure 快速启动模板]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[活动中心和使用者组模板]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
