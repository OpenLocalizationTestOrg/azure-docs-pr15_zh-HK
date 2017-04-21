<properties
    pageTitle="IoT 网关 SDK 中使用真实设备 |Microsoft Azure"
    description="使用德克萨斯仪器 SensorTag 设备将数据发送到 IoT 中枢，通过网关运行英特尔 Edison 计算模块上的 azure IoT 网关 SDK 演练"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT 网关 SDK （测试版） – 与真实设备使用 Linux 的发送设备与云消息

[Bluetooth 低能耗示例]本演练[lnk-ble-samplecode]向您显示如何使用[Azure IoT 网关 SDK] [lnk-sdk]到转发 IoT 集线器从物理设备，以及如何将路由命令从 IoT 集线器到一个物理设备的设备到云遥测。

本演练介绍︰

* **体系结构**︰ 示例有关的 Bluetooth 低能耗的重要结构信息。

* **生成和运行**︰ 生成并运行此示例所需的步骤。

## <a name="architecture"></a>体系结构

本演练演示如何生成并运行 Linux 的英特尔 Edison 计算模块运行 IoT 网关。 网关是使用 IoT 网关 SDK 生成的。 此示例使用德克萨斯仪器 SensorTag Bluetooth 低能量 （的） 设备来收集温度数据。

当您运行网关它︰

- 连接到 SensorTag 设备使用 Bluetooth 低能量 （的） 协议。
- 连接到使用 HTTP 协议 IoT 集线器。
- 将从 SensorTag 设备的遥测转发到 IoT 集线器。
- 将路由命令从 IoT 集线器到 SensorTag 设备。

网关还包含以下模块︰

- 使用的设备来接收设备和发送命令到设备温度数据接口*的模块*。
- 翻译成*的模块*的指令来自云 JSON 消息*的云至设备模块*。
- 记录所有网关消息*记录器模块*。
- 之间的设备的 MAC 地址和 Azure IoT 集线器设备标识转换*身份映射模块*。
- *IoT 集线器模块*遥测数据上载到 IoT 集线器并从 IoT 集线器接收设备命令。
- 是解释中的设备的遥测和打印*的打印机模块*格式设置控制台能够排除故障和调试数据。

### <a name="how-data-flows-through-the-gateway"></a>数据流通过该网关

下面的框图说明了遥测上载数据流量管道︰

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

遥测项采用 IoT 集线器的设备从旅行的步骤如下︰

1. 设备生成温度样本，并将其通过 Bluetooth 发送到网关的模块。
2. 模块接收该示例并将其发布到该中介以及设备的 MAC 地址。
3. 标识映射模块拾取此消息，并使用内部表来将设备的 MAC 地址转换为 IoT 集线器设备标识 （设备 ID 和设备密钥）。 然后，它可以发布包含温度样本数据、 设备、 设备 ID，并将设备项的 MAC 地址的新邮件。
4. IoT 集线器模块接收新消息 （由标识映射模块生成），并将其发布到 IoT 集线器。
5. 记录器模块从代理程序所有邮件都记录到磁盘文件。

下面的框图说明了设备命令数据流量管道︰

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. IoT 集线器模块定期轮询 IoT 中心新命令消息。
2. 当 IoT 集线器模块接收新的命令消息时，它将其发布到该中介。
3. 标识映射模块选取命令消息，并使用内部表翻译 IoT 集线器设备 ID 到一个设备的 MAC 地址。 它可以再发布新的邮件包含邮件的属性映射中的目标设备的 MAC 地址。
4. 对设备模块的云选取此消息并将其翻译成适当的指令的模块。 然后，它可以发布一封新邮件。
5. 模块选取该消息并执行 I/O 指令的设备进行通信。
6. 记录器模块从代理程序所有邮件都记录到磁盘文件。

## <a name="prepare-your-hardware"></a>准备硬件

本教程假定您正在使用连接到英特尔 Edison 板的[德克萨斯仪器 SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html)设备。

### <a name="set-up-the-edison-board"></a>设置 Edison 板

在开始之前，应确保您可以连接到无线网络的 Edison 设备。 若要设置您 Edison 的设备，您需要将其连接到主机。 英特尔提供了入门指南的下列操作系统︰

- [开始使用 64 位 Windows 上的英特尔 Edison 开发板][lnk-setup-win64]。
- [开始使用 Windows 32 位英特尔 Edison 开发板][lnk-setup-win32]。
- [开始使用英特尔 Edison 开发板上 Mac OS X][lnk-setup-osx]。
- [开始使用 Linux 上的 Intel® Edison 板][lnk-setup-linux]。

若要设置 Edison 设备和熟悉它，应该完成所有除最后一步，"选择 IDE"即为当前教程不必要的步骤在这些"入门"文章。 在 Edison 安装过程结束时，您能够︰

- 已刷新您 Edison 与最新的固件。
- 建立从您的主机到 Edison 的串行连接。
- 运行**configure_edison**脚本来设置密码，并启用在您 Edison 的 WiFi。

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>启用连接到 SensorTag 设备从 Edison 板

之前运行该示例，您需要验证您 Edison 的主板可以连接到 SensorTag 设备。

首先，您需要验证您 Edison 可以连接到 SensorTag 设备。

1. 允许蓝牙的 Edison 并检查版本号是**5.37**。
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. 执行**bluetoothctl**命令。 现在，您已经在蓝牙交互式外壳程序。 

3. 打开蓝牙控制器的电源输入命令**打开电源**。 您应该看到类似的输出︰
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. 而仍在蓝牙交互式外壳程序中，输入命令**在扫描**扫描蓝牙设备。 您应该看到类似的输出︰
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. 按小按钮 （绿色的指示灯应闪烁），使 SensorTag 设备可检测到。 Edison 应发现 SensorTag 设备︰
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    在此示例中，您可以看到 SensorTag 设备的 MAC 地址是**A0:E6:F8:B5:F6:00**。

6. 请关闭扫描输入**扫描关闭**命令。
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. 连接到 SensorTag 设备通过其 MAC 地址输入**连接\<MAC 地址 >**。 请注意下面的示例输出缩写形式︰
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    注意︰ 您可以列出的 GATT 特征的设备再次使用**列表属性**命令。

8. 可现在使用的**断开连接**命令从设备断开连接并从蓝牙 shell 使用**quit**命令然后退出︰
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

您现在准备好 Edison 设备上运行的网关的示例。

## <a name="run-the-ble-gateway-sample"></a>运行网关的示例

您 Edison 上运行的示例，您需要完成三项任务︰

- 在 IoT 集线器配置两个示例设备。
- Edison 设备上生成 IoT 网关 SDK。
- 配置和 Edison 设备上运行的示例。

书写时，IoT 网关 SDK 仅支持使用 Linux 的模块的网关。

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>在 IoT 集线器中配置两个示例设备

- [创建 IoT 中心][lnk-create-hub]在 Azure 的订阅，您需要网络集线器来完成此操作实例的名称。 如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。
- 添加 IoT 集线器到名为**SensorTag_01**的一台设备并记下其 id 和设备的密钥。 您可以使用[设备资源管理器或资源管理器 iothub] [lnk-explorer-tools]工具以将此设备添加到您在上一步中创建 IoT 集线器并检索它的键。 配置网关时，您将此设备映射到 SensorTag 设备。

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Edison 设备上生成 IoT 网关 SDK

在 Edsion 上的**git**版本不支持 submodules。 为 Edison IoT 网关 sdk 下载的完整源代码，您有两种︰

- 选项 #1︰ 克隆[Azure IoT 网关 SDK] [lnk-sdk]您 Edison 在存储库中，然后手动克隆存储库的各个子模块。
- 选项 #2︰ 克隆[Azure IoT 网关 SDK] [lnk-sdk]在**git**支持 submodules，然后将复制到您的 Edison submodules 的完整资料库的桌面设备上的存储库。

如果您选择选项 2，使用以下的**git**命令克隆 IoT 网关 SDK 以及所有其 submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

然后应压缩整个本地存储库到一个单独的存档文件之前将其复制到 Edison。 您可以使用**pscp**附带**Putty**可以将存档文件复制到 Edison 之类的实用程序。 例如︰

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

当您 Edison IoT 网关 SDK 资料库的完整副本，可以生成包含 SDK 文件夹中使用下面的命令︰

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>配置和 Edison 设备上运行的示例

要引导并运行该示例，您需要在网关配置参与的每个模块。 在 JSON 文件中提供了此配置，您需要配置所有五个参与模块。 没有存储库调用**gateway_sample.json**可用作起始点构建您自己的配置文件中提供的示例 JSON 文件。 此文件的**示例/ble_gateway_hl/src**文件夹中是 IoT 网关 SDK 存储库中的本地副本中。

以下各节介绍如何编辑此配置文件的示例，假设 IoT 网关 SDK 资料库在**/home/root/azure-iot-gateway-sdk /** Edison 设备上的文件夹。 如果存储库中其他地方，则应相应地调整路径︰

#### <a name="logger-configuration"></a>记录器配置

假定的网关存储库位于文件夹**/home/root/azure-iot-gateway-sdk /**，配置记录器模块，如下所示︰

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>模块配置

设备的示例配置假定德克萨斯仪器 SensorTag 设备。 可以运行工作外围 GATT 应任何标准的设备，但需要更新的 GATT 特征 Id 和 （写指令） 数据。 添加 SensorTag 设备的 MAC 地址︰ 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT 集线器模块

添加 IoT 中心的名称。 后缀值通常是**azure devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>标识映射模块配置

将 MAC 地址 SensorTag 设备和设备 Id 和密钥添加到 IoT 中心的**SensorTag_01**设备︰

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>打印机的模块配置

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>路由配置

以下配置将确保以下︰
- **记录器**模块接收并记录所有消息。
- **SensorTag**模块将消息发送到的**映射**和**的打印机**模块。
- **映射**模块将消息发送给要发给在 IoT 中心的**IoTHub**模块。
- **IoTHub**模块将消息发送到**映射**模块。
- **映射**模块将消息发送回的**SensorTag**模块。

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

若要运行该示例运行时二进制将路径传递到 JSON 配置文件**ble_gateway_hl** 。 如果您使用的**gateway_sample.json**文件，则要执行的命令如下所示︰

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

您可能需要按 SensorTag 设备，以使其可检测到您在运行此示例之前上的小按钮。

当您运行此示例时，可以使用[设备资源管理器或资源管理器 iothub] [lnk-explorer-tools]工具，用于监视网关转发从 SensorTag 设备的消息。

## <a name="send-cloud-to-device-messages"></a>发送云至设备信息

模块还支持向设备发送从 Azure IoT 集线器的操作说明。 您可以使用[Azure IoT 集线器设备资源管理器](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md)或[IoT 中心资源管理器](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer)发送到的设备的网关模块转手的 JSON 消息。 例如，如果您使用的德克萨斯仪器 SensorTag 设备然后您可以下列 JSON 向设备发送消息从 IoT 集线器。

- 重置所有指示灯和蜂鸣器 （将其关闭）

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- 将 I/O 配置为远程

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- 开启的红色指示灯亮起

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- 开启绿色指示灯

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- 打开蜂鸣器

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

使用 HTTP 协议 IoT 集线器连接的设备的默认行为是为一个新的命令每隔 25 分钟检查一次。 因此，如果您在发送多个单独的命令需要等待设备接收的每个命令的 25 分钟。

> [AZURE.NOTE] 网关也会检查新的命令使您可以强制它来停止和启动网关通过处理命令启动时。

## <a name="next-steps"></a>下一步行动

如果您想进一步了解 IoT 网关 SDK 并试验一些代码示例，请访问下面的开发教程和资源︰

- [Azure IoT 网关 SDK][lnk-sdk]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
