### <a name="app-service-plan"></a>应用程序服务计划

创建用于承载该 web 应用程序的服务计划。 提供计划通过**hostingPlanName**参数的名称。 该计划的位置是资源组使用相同的位置。 **Sku**和**workerSize**参数中指定了定价层和工作人员大小

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

