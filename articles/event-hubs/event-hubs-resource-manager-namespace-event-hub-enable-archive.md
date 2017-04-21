<properties
    pageTitle="向事件中心创建事件集线器的命名空间并启用存档使用 Azure 资源管理器模板 |Microsoft Azure"
    description="向事件中心创建事件集线器的命名空间并启用存档使用 Azure 资源管理器模板"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>向事件中心创建事件集线器的命名空间并启用存档使用 Azure 资源管理器模板

本文介绍如何使用 Azure 资源管理器模板创建事件中心事件集线器命名空间并使事件集线器上的存档。 您将了解如何定义部署哪些资源以及如何定义参数指定当执行部署。 可以将此模板用于您自己的部署，也可以进行自定义以满足您的需求

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板][]。

Azure 资源命名约定的模式和做法上的详细信息，请参阅[Azure 资源命名约定][]。

完整的模板，请参阅在 GitHub 上的[事件中心和启用存档模板][]。

>[AZURE.NOTE]
>若要检查新的模板，请访问[Azure 快速启动模板][]库和搜索事件集线器。

## <a name="what-you-deploy"></a>您的部署？

使用此模板，您部署与事件中心事件集线器命名空间并启用存档。

[事件集线器](../event-hubs/event-hubs-what-is-event-hubs.md)是事件处理服务用于向事件和遥测入口到 Azure 在巨大规模较大，提供低延迟和高可靠性。 集线器存档事件将使您能够自动在指定的时间或您选择的大小间隔内到您选择的 Azure Blob 存储您事件集线器中提供流数据。

若要部署都自动运行，请单击下面的按钮︰

[![将部署到 Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>参数

使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名的部分`Parameters`，其中包含所有参数值。 应定义为那些值，根据要部署的项目或根据要部署到的环境参数。 未定义参数的值总是保持不变。 每个参数值用于在模板中定义部署的资源。

模板定义以下参数。

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

要创建的事件集线器命名空间名称。

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

事件中集线器命名空间创建的事件中心的名称。

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

您想保留这些邮件事件中心天数。 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

在事件中心所需的分区数。

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

启用事件集线器上的归档文件。

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

指定要序列化事件数据编码格式。

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

归档文件启动存档 Azure blob 存储中的数据的时间间隔。

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

归档文件启动存档 Azure blob 存储中的数据大小间隔。

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

归档文件需要的存储帐户资源 id，以使您所需的 Azure 存储到存档。

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

存档事件数据的 blob 容器。

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

API 版本的模板。

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>若要部署的资源

与事件中心创建命名空间的类型**EventHubs**，并启用存档。

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>要运行部署命令

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[创作的 Azure 资源管理器模板]: ../resource-group-authoring-templates.md
[Azure 快速启动模板]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure 的资源命名约定]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[事件中心和启用存档模板]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
