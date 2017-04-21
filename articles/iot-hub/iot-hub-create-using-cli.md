<properties
    pageTitle="创建使用 Azure CLI IoT 中心 |Microsoft Azure"
    description="按照这篇文章，以创建使用 Azure 命令行界面 IoT 集线器。"
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>创建使用 Azure CLI IoT 集线器

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>介绍

可以使用 Azure 命令行界面来创建并以编程方式管理 Azure IoT 集线器。 这篇文章演示了如何使用 Azure CLI 创建 IoT 集线器。

若要完成本教程，您需要︰

- 活动的 Azure 帐户。 如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。
- [Azure CLI 0.10.4] [lnk-CLI-install]或更高版本。 如果您已经有了 Azure CLI 可以验证在命令提示符下使用以下命令的当前版本︰
```
    azure --version
```

> [AZURE.NOTE] Azure 具有用于创建和使用资源的两种不同的部署模型︰ [Azure 资源管理器和经典](../resource-manager-deployment-model.md)。 Azure CLI 必须是 Azure 资源管理器模式︰
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>设置 Azure 帐户和订阅 

1. 在命令提示符下键入以下命令的登录
```
    azure login
```
使用建议的 web 浏览器和代码进行身份验证。

2. 如果您有多个 Azure 的订阅，则连接到 Azure 将授予访问权限与凭据关联的所有 Azure 订阅。 您可以查看 Azure 的订阅，以及这两种程序默认情况下，使用命令
```
    azure account list 
```

设置要在其下运行的命令使用的其余部分订阅上下文

```
    azure account set <subscription name>
```

3. 如果您不具有资源组您可以创建一个命名的**exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] 文章[使用 Azure CLI 管理 Azure 的资源和资源组][lnk-CLI-arm]提供了有关如何使用 Azure CLI 管理 Azure 的资源的详细信息。 


## <a name="create-an-iot-hub"></a>创建 IoT 集线器

必需的参数︰

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
若要查看所有的参数可用于创建可以使用帮助命令在命令提示符处
```
    azure iothub create -h 
```
快速的示例︰

 若要创建 IoT 中心资源组**exampleResourceGroup**只需运行下面的命令中调用**exampleIoTHubName**
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] 此 Azure CLI 命令创建为其付费 S1 标准 IoT 集线器。 您可以删除使用以下命令 IoT 中心**exampleIoTHubName** 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>下一步行动
若要了解有关 IoT 中心开发的详细信息，请参阅以下︰
- [IoT 集线器 Sdk][lnk-sdks]

要进一步探索 IoT 中心的功能，请参阅︰

- [使用 Azure 门户管理 IoT 集线器][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
