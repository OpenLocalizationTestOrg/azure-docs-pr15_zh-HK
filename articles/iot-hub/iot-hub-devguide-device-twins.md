<properties
 pageTitle="开发人员指南-了解设备 twins |Microsoft Azure"
 description="Azure IoT 中心开发指南的使用设备 twins IoT 集线器之间的设备状态和配置数据同步"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>了解设备 twins-预览

## <a name="overview"></a>概述

*设备 twins*是 JSON 文档存储设备状态信息 （元数据、 配置，以及条件）。 IoT 中心仍然存在 IoT 集线器连接每个设备的设备双子星。 这篇文章将介绍︰

* 设备双子星的结构︰*标记*、*需要*和*报告属性*，并且
* 设备应用程序和后端设备 twins 可以执行的操作。

> [AZURE.NOTE] 到目前为止，设备 twins 都只能访问使用 MQTT 协议 IoT 集线器连接的设备。 [MQTT 支持]，请参考[lnk-devguide-mqtt]如何转换现有的设备应用程序说明使用 MQTT 的文章。

### <a name="when-to-use"></a>何时使用

使用设备 twins 到︰

* 将设备特定的元数据存储在云中，例如部署位置的自动贩卖机。
* 报告当前的状态信息，如可用的功能和从您的设备应用程序，例如通过移动电话或 wifi 连接设备的条件。
* 同步设备的应用程序和后端，如后端指定新的固件版本，若要安装，并报告更新过程的各个阶段的设备应用程序之间的长时间运行工作流的状态。
* 查询您设备元数据、 配置或状态。

使用[设备到云消息][lnk-d2c]序列的时序的传感器数据或警报的时间戳的事件。 使用[云至设备][lnk-methods]交互控制的设备，如风扇打开。

## <a name="device-twins"></a>设备 twins

设备 twins 存储设备相关信息的︰

- 设备和后端可用来同步设备条件和配置。
- 后端应用程序可用于查询和目标长时间运行的操作。

设备双子星的生命周期已链接到相应的[设备标识][lnk-identity]。 Twins 隐式地创建和删除时创建或删除 IoT 中心新设备标识。

设备双子星是一个 JSON 文档，包括︰

* **标记**。 读取和写入由后端的 JSON 文档。 标记是不可见的设备应用程序。
* **所需属性**。 结合报告属性用于同步设备配置或条件。 所需的属性只能设置由应用程序返回结束和设备的应用程序可以读取。 设备应用程序还可以实时所需的属性更改的通知。
* **报告属性**。 结合所需的属性用于同步设备配置或条件。 报告的属性只可以通过设备应用程序设置，并可以读取和查询的应用程序的后端。

此外，设备双子星的根包含只读属性与相应的标识，为[设备标识注册表]中包含[lnk-identity]。

![][img-twin]

以下是设备双子星 JSON 文档的示例︰

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

根对象中的系统属性中，并且容器对象的`tags`和`reported`， `desired properties`。 `properties`容器不包含一些只读元素 (`$metadata`， `$etag`，和`$version`) 的[双子星元数据]中分别描述[lnk-twin-metadata]和[开放式并发][lnk-concurrency]节。

### <a name="reported-property-example"></a>报告的属性的示例

在上面的示例中，包含设备 twin`batteryLevel`所报告的设备应用程序的属性。 此属性使查询并根据上一次报告的电池级别的设备上运行。 另一个示例必须的设备应用程序报告设备功能或连接性选项。

请注意如何报告的属性简化的方案后端主要兴趣属性的上一个已知值。 使用[设备到云消息][lnk-d2c]后端需要处理设备遥测中的时间戳的事件，如时序序列的形式。

### <a name="desired-configuration-example"></a>所需的配置的示例

在上面的示例中，`telemetryConfig`需和报告属性由后端和设备应用程序进行同步此设备的遥测配置。 例如︰

1. 应用程序的后端设置所需的属性所需的配置值。 下面是具有所需属性的文档的部分︰

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. 通知设备应用程序的连接，如果立即更改或在第一个重新连接。 设备应用程序然后报告更新的配置 (或错误条件使用`status`属性)。 以下是报告属性的部分︰

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. 保持应用程序的后端可以跨多个设备，跟踪配置操作的结果，通过[查询][ lnk-query] twins。

> [AZURE.NOTE] 上面的代码段是示例中，为了提高可读性，编码设备配置及状态的可能方法进行了优化。 IoT 集线器不强加特定设备 twins 中的所需和报告属性架构。

在许多情况下 twins 用于同步长时间运行的操作，例如更新固件。 请参阅[使用所需的属性来配置设备][lnk-twin-properties]有关如何使用属性来同步和长时间跟踪不同的设备运行操作。

## <a name="back-end-operations"></a>后端操作

使用下面的原子操作，通过 HTTP 公开双子星上的后端操作︰

1. **检索由 id 的双子星**。 此操作将返回内容的双子星的文档，包括标记和所需的报告和系统属性。
2. **部分更新的双子星**。 此操作使后端部分更新两路输出的标记或所需的属性。 添加或更新提到任何属性的 JSON 文档表示部分更新。 属性设置为`null`被删除。 例如，以下代码将创建一个新的所需的属性值与`{"newProperty": "newValue"}`，将覆盖现有的值`existingProperty`与`"otherNewValue"`，并完全删除`otherOldProperty`。 其他所需的现有属性或标记不发生任何更改︰

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **更换所需的属性**。 此操作使后端要完全覆盖所有现有所需的属性，并替换为新的 JSON 文档`properties/desired`。
4. **替换标记**。 Analogously 为了替换所需的属性，此操作可以使后端完全覆盖所有现有标记并替换为新的 JSON 文档`tags`。

所有上述操作支持[开放式并发][lnk-concurrency]需要**ServiceConnect**的权限，规定[安全]和[lnk-security]文章。

除了这些操作后, 端可以查询使用类似于 SQL 的[查询语言]twins[lnk-query]，并对大型的 twins 使用[作业]集执行操作[lnk-jobs]。

## <a name="device-operations"></a>设备操作

设备应用程序上使用下面的原子操作两路输出的操作︰

1. **检索双子星**。 此操作将返回两路输出的文档的内容 (包括标记和所需的报告和系统属性) 对于当前已连接的设备。
2. **更新报告属性的部分**。 此操作使部分更新的报告的属性的当前已连接的设备。 这将面临部分所需的属性更新的后端使用 JSON 更新格式相同。
3. **遵守所需的属性**。 当前已连接的设备可以选择所需属性的更新发生时获得通知。 该设备接收由后端执行同一形式的更新 （部分或全部替代）。

所有上述操作需要**DeviceConnect**的权限，规定[安全][lnk-security]文章。

[Azure IoT 设备 Sdk] [lnk-sdks]方便地使用多语言和平台从上述操作。 [设备重新连接流]中找不到 IoT 中枢的基元以供所需的属性同步的详细信息详细信息[lnk-reconnection]。

> [AZURE.NOTE] 目前，设备 twins 是只能从连接到使用 MQTT 协议 IoT 集线器设备的访问。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关控制访问 IoT 中心的详细信息。

## <a name="tags-and-properties-format"></a>标记和属性的格式

标记，需和报告属性的 JSON 对象具有以下限制︰

* JSON 对象中的所有键都是区分大小写 128 字符的 UNICODE 字符串。 允许字符排除 UNICODE 控制字符 （段 C0 和 C1） 和`'.'`， `' '`，和`'$'`。
* JSON 对象中的所有值可以都是下面的 JSON 类型︰ 布尔值、 数字、 字符串、 对象。 不允许使用数组。

## <a name="twin-size"></a>双子星大小

IoT 中心强制使用 8KB 的大小限制值的`tags`， `properties/desired`，和`properties/reported`，不包括只读元素。
计算大小的方法是计算所有的字符，不包括 UNICODE 控制字符 （段 C0 和 C1） 和空间`' '`当它出现在字符串常数之外。
IoT 中心将会增加的大小超过限制这些文档的所有操作，错误都拒绝。

## <a name="twin-metadata"></a>双子星元数据

IoT 中心维护需和报告属性中每个 JSON 对象的上次更新的时间戳。 时间戳以 utc 格式和[ISO8601]格式编码`YYYY-MM-DDTHH:MM:SS.mmmZ`。
例如︰

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

此信息保存在每个级别 （而不仅仅是 JSON 结构的叶片） 保留删除对象键的更新。

## <a name="optimistic-concurrency"></a>开放式并发

标记，需和报告属性都支持开放式并发。
标记有 etag，根据[RFC7232]，表示标记的 JSON 表示。 您可以使用这中从后端的条件更新操作以确保一致性。

需和报告属性不具有 etag，但`$version`保证是增量的值。 Analogously 到 etag，版本可用 （设备应用程序的报告的属性） 和后端的所需属性的更新当事方通过强制执行更新的一致性。

版本还非常有用当观察代理 （例如，观察所需的属性的设备应用程序） 必须协调检索操作的结果和更新通知之间的比赛。 部分[设备重新连接流][lnk-reconnection]提供了详细信息。

## <a name="device-reconnection-flow"></a>设备重新连接流

IoT 集线器不保留已断开连接的设备所需的属性更新通知。 它遵循所连接的设备必须检索全部所需的属性文档，和订阅更新通知。 给出完整的检索更新通知之间的比赛的可能性，必须确保以下流︰

1. 设备应用程序连接到一个 IoT 集线器。
2. 设备应用程序所需的属性更新通知的订阅。
3. 设备应用程序中检索所需的属性的完整文档。

设备应用程序可以忽略所有通知`$version`小于或等于比完整的检索文档的版本。 这是可能的因为 IoT 中心确保版本总是增加。

> [AZURE.NOTE] 在[Azure IoT 设备 Sdk]中已实现此逻辑[lnk-sdks]。 仅当设备应用程序不能使用任何 Azure IoT 设备的 Sdk 和必须直接编程 MQTT 界面，此说明非常有用。

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在您所了解的关于设备 twins，您可能会感兴趣以下开发人员指南 》 的主题︰

- [在设备上的直接方法调用][lnk-methods]
- [多台设备上的计划作业][lnk-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣下面 IoT 集线器教程︰

- [如何使用设备双子星][lnk-twin-tutorial]
- [如何使用两路输出属性][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png