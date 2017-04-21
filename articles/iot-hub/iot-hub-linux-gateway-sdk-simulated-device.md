<properties
    pageTitle="模拟具有 IoT 网关 SDK 的设备 |Microsoft Azure"
    description="使用 Linux 来说明发送遥测模拟设备使用 Azure IoT 网关 SDK 中的 azure IoT 网关 SDK 演练。"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT 网关 SDK （测试版） – 使用 Linux 的模拟设备的发送设备与云消息

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>生成并运行示例

在开始之前，您必须︰

- [设置开发环境][lnk-setupdevbox]使用 SDK 在 Linux 上的。
- [创建 IoT 中心][lnk-create-hub]在 Azure 的订阅，您需要网络集线器来完成此操作实例的名称。 如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。
- 将两个设备添加到您 IoT 中枢和记下他们的 id 和设备键。 您可以使用[设备资源管理器或资源管理器 iothub] [lnk-explorer-tools]工具添加到您在上一步中创建 IoT 集线器设备并检索它们的键。

若要生成示例︰

1. 打开外壳。
2. 导航到本地副本中的**azure iot 网关 sdk**存储库的根文件夹。
3. 运行**tools/build.sh**脚本。 此脚本使用**cmake**实用程序创建文件夹本地副本的**azure iot 网关 sdk**存储库的根文件夹中名为**构建**并生成生成文件。 然后，脚本生成解决方案并运行测试。

> [AZURE.NOTE]  每次您运行**build.sh**脚本，它删除，然后重新创建本地副本的**azure iot 网关 sdk**存储库的根文件夹中的**生成**文件夹。

若要运行此示例︰

在文本编辑器中，打开文件**samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** **azure iot 网关 sdk**存储库中的本地副本中。 此文件中的示例网关配置模块︰

- **IoTHub**模块连接到 IoT 集线器。 必须配置为将数据发送到 IoT 集线器。 具体而言，将**IoTHubName**值设置为 IoT 中心的名称，并将**IoTHubSuffix**值设置为**azure devices.net**。 将**传输**值设置之一:"HTTP"、"AMQP"或"MQTT"。 请注意，通过目前唯一的"HTTP"将共享一个 TCP 连接的设备的所有消息。 如果您将值设置为"AMQP"或"MQTT"，该网关将维护单独的 TCP 连接到 IoT 集线器的每个设备。
- **映射**模块映射到您 IoT 集线器设备 id 的模拟设备的 MAC 地址。 请确保**deviceId**值匹配的两个设备添加到您的 IoT 中枢，id 和**deviceKey**值，包含键的两个设备。
- **BLE1**和**BLE2**模块的模拟的设备。 请注意它们的 MAC 地址匹配**映射**模块中的那些方式。
- **记录器**模块将网关活动记录到一个文件。
- 下面显示的**模块路径**值假定您将从您的本地副本**azure iot 网关 sdk**存储库的根运行该示例。
- JSON 文件底部的**链接**数组连接**BLE1**和**BLE2**模块**映射**模块和**IoTHub**模块**映射**模块。 它还确保所有邮件都会都记录由**记录器**模块。

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

保存您对配置文件所做的任何更改。

若要运行此示例︰

1. 在外壳程序中，导航到本地副本中的**azure iot 网关 sdk**存储库的根文件夹。
2. 运行以下命令︰

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. 您可以使用[设备资源管理器或资源管理器 iothub] [lnk-explorer-tools]工具，用于监视 IoT 集线器从网关收到的邮件。

## <a name="next-steps"></a>下一步行动

如果您想进一步了解 IoT 网关 SDK 并试验一些代码示例，请访问下面的开发教程和资源︰

- [从 IoT 网关 SDK 的真实设备发送设备到云邮件][lnk-physical-device]
- [Azure IoT 网关 SDK][lnk-gateway-sdk]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [设置安全 IoT 解决方案从零开始][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md