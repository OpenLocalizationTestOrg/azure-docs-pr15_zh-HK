<properties 
    pageTitle="创建一个使用 Azure 资源管理器模板和 Azure 应用程序服务中的逻辑应用程序 |Microsoft Azure" 
    description="使用 Azure 资源管理器模板用于定义工作流部署一个空的逻辑应用程序。" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>创建一个使用模板的逻辑应用程序

使用 Azure 资源管理器模板来创建一个空的逻辑应用程序，可以用于定义工作流。 您可以定义部署哪些资源以及如何定义执行部署时指定的参数。 可以将此模板用于您自己的部署中，或自定义以满足您的要求。

逻辑应用程序属性的详细信息，请参阅[逻辑应用程序工作流管理 API](https://msdn.microsoft.com/library/azure/mt643788.aspx)。 

该定义本身的示例，请参阅[作者逻辑应用程序定义](app-service-logic-author-definitions.md)。 

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。

有关完整的模板，请参阅[逻辑应用程序模板](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json)。

## <a name="what-you-will-deploy"></a>您将部署

使用此模板中，您将部署逻辑应用程序。

若要自动运行部署，请选择下面的按钮︰  

[![将部署到 Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>参数

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>若要部署的资源

### <a name="logic-app"></a>逻辑应用程序

创建逻辑应用程序。

模板使用的逻辑应用程序名参数值。 逻辑应用程序的位置设置为该资源组所在的位置。 

此特定定义运行一次一小时，并在**testUri**参数中指定的位置地址执行 ping 操作。 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>要运行部署命令

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
