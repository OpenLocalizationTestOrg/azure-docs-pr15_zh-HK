使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名参数，其中包含所有参数值的部分。
您应定义这些值，将基于您正在部署的项目或根据要部署到的环境而变化的参数。 未定义的值始终保持相同的参数。 每个参数值用于在模板中定义部署的资源。 

在定义参数时，使用**allowedValues**字段指定哪些用户可以在部署过程中提供的值。 使用**默认值**字段将值赋给该参数，如果未提供值在部署过程中。

我们将介绍每个模板中的参数。

### <a name="sitename"></a>站点名

您想要创建的 web 应用程序的名称。

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

应用程序服务计划用于承载该 web 应用程序的名称。
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>sku

托管计划定价层。

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

模板定义了该参数，允许的值，并指派默认值 (S1)，如果未不指定任何值。

### <a name="workersize"></a>workerSize

实例大小 （小、 中或大） 承载的计划。

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
模板定义允许 （0、 1 或 2），此参数的值，并指派默认值 (0)，如果未不指定任何值。 值与小型，中型和大型。
