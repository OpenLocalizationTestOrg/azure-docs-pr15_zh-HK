> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [窗口](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

[模拟设备云上载示例]本演练演示如何使用[Azure IoT 网关 SDK] [lnk-sdk]遥测设备到云向从模拟设备 IoT 集线器。

本演练介绍︰

1. **体系结构**︰ 模拟设备云上载示例有关的重要结构信息。

2. **生成和运行**︰ 生成并运行此示例所需的步骤。

## <a name="architecture"></a>体系结构

模拟设备云上载示例演示了如何使用 SDK 创建一个网关将遥测从模拟设备发送到 IoT 集线器。 模拟的设备不能直接到 IoT 集线器连接，因为︰

- 设备不使用即理解 IoT 中心的通信协议。
- 设备没有足够的智能，记住 IoT 集线器分配给他们的身份。

网关能够以下列方式解决这些问题，模拟设备︰

- 网关可以理解模拟设备中，所使用的协议接收设备到云遥测设备，并将这些消息转发到 IoT 中心使用的是理解中心的协议。
- 网关存储 IoT 中心标识代表模拟设备和模拟的设备将消息发送到 IoT 集线器时充当代理。

下图显示的示例中，包括网关模块的主要组件︰

![][1]


> [AZURE.NOTE] 模块未通过直接向对方的消息。 模块将消息发布到内部代理将消息传送到其他模块使用订阅机制，如下图中所示。 有关详细信息，请参阅[入门 IoT 网关 SDK][lnk-gw-getstarted]。

### <a name="protocol-ingestion-module"></a>协议接收模块

本模块是从设备获取数据，通过网关，并到云的起始点。 在示例中，该模块将执行四项任务︰

1.  它将创建模拟的温度数据。
    
    注意︰ 如果您在使用实际的设备，模块将从读取数据的物理设备。

2.  它将模拟的温度数据放入邮件的内容。

3.  它将一个属性与一个虚假的 MAC 地址添加到包含模拟的温度数据的邮件。

4.  它使消息可用于下一个模块链中。

> [AZURE.NOTE] 在上图中称为**协议 X 摄取**的模块称为**模拟设备**的源代码中。

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT 集线器 ID 模块

本模块包括一个包含的 MAC 地址，通过协议接收模块，在模拟设备的添加的属性的邮件扫描。 如果找到这样的属性，它将另一个属性具有 IoT 集线器设备项添加到消息，然后将消息供下一个模块链中。 这是如何的示例将 IoT 集线器设备标识与模拟设备相关联。 开发人员设置手动作为模块配置 MAC 地址 IoT 中心标识之间的映射。 

> [AZURE.NOTE]  此示例使用 MAC 地址作为唯一的设备标识符并将它与 IoT 集线器设备标识关联起来。 但是，您可以编写自己的模块使用不同的唯一标识符。 例如，您可能必须具有唯一的序列号或已嵌入，您可以使用来确定 IoT 集线器设备标识唯一的设备名称的遥测数据的设备。

### <a name="iot-hub-communication-module"></a>IoT 中心通讯模块

本模块 IoT 集线器设备标识由以前的模块分配采用消息并将消息内容发送到使用 HTTP IoT 集线器。 HTTP 是一种理解 IoT 中心的三个协议。

而不是打开到 IoT 中心为每个模拟设备的连接，本模块从通往 IoT 集线器打开单个 HTTP 连接，并使用多路复用通过该连接的所有模拟设备的连接。 这使得单个网关连接许多更多设备，模拟或其他可比打开一个唯一的连接的每台设备。

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[模拟的设备云上载示例]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md