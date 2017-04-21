> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>介绍

设备 twins 是 JSON 文档存储设备状态信息 （元数据、 配置，以及条件）。 IoT 中心仍然存在 IoT 集线器连接每个设备的设备双子星。

使用设备 twins 到︰

* 设备元数据从您的后端存储。
* 报告从您的设备应用程序的当前状态信息，例如可用功能和条件 （例如，所使用的连接方法）。
* 同步设备的应用程序和后端之间的长时间运行的工作流 （例如，固件和配置的更新） 的状态。
* 查询您设备元数据、 配置或状态。

> [AZURE.NOTE] 对于同步和查询设备配置和条件设计设备 twins。 使用[设备到云消息][lnk-d2c]序列的时间戳的事件 （例如，基于时间的传感器数据的遥测流） 和[云至设备方法]的[lnk-methods]交互控制的设备，如风扇从用户控制的应用程序打开。

设备 twins IoT 中心存储，并且包含︰

* *标记*，设备元数据只能访问后端;
* *所需的属性*，可由后端和观测设备的应用程序; 按修改的 JSON 对象和
* *报告的属性*，JSON 对象可由设备应用程序修改和更加可读后结束。 标记和属性不能包含数组，但可以嵌套对象。

![][img-twin]

另外后, 端应用程序可以查询设备 twins 基于所有上述数据。
请参阅[了解设备 twins] [lnk-twins]有关设备 twins 及[IoT 中心查询语言]的详细信息[lnk-query]查询参考。

> [AZURE.NOTE] 到目前为止，设备 twins 都只能访问使用 MQTT 协议 IoT 集线器连接的设备。 [MQTT 支持]，请参考[lnk-devguide-mqtt]如何转换现有的设备应用程序说明使用 MQTT 的文章。

本教程展示如何为︰

- 创建一个后端应用程序将*标记*添加到设备双子星和报告设备双子星*报告属性*作为其连接通道的模拟的设备。
- 查询从您使用标记和属性以前创建的筛选器的后端应用程序的设备。


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md