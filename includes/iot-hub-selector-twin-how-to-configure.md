> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>介绍

在[入门 IoT 集线器 twins][lnk-twin-tutorial]，您学习了如何从使用*标记*，报告设备条件从设备的应用程序使用*报告的属性*，您的解决方案后端设置设备的元数据，查询该信息使用的类似于 SQL 的语言。

在本教程中，您将学习如何使用结合*报告属性*，远程配置设备应用程序中的双子星的*所需的属性*。 更具体地说，本教程演示如何双子星的报告和所需的属性启用多个步骤配置的设备应用程序设置，并跨所有设备提供到此操作的状态的解决方案后端的可见性。

从较高层面，本教程后面设备管理*所需的状态模式*。 这种模式的基本思想是让解决方案后端指定为受管理的设备，而不是发送特定的命令所需的状态。 这将使负责建立最佳的方法，以达到所需的状态 （IoT 情况下，特定的设备条件会影响立即执行特定命令的能力非常重要），设备时不断报告到后端的当前状态和潜在的错误条件。 它使后端具有完全可见性状态的配置过程的所有设备的所需的状态模式是对于管理大型的设备，集器乐。
可以在[Azure IoT 集线器的概述设备管理]中的设备管理中找到有关角色的详细信息，所需的状态模式的[lnk-dm-overview]。

> [AZURE.NOTE] 在交互性更强的方式 （开启用户控制的应用程序中的风扇），控制设备的情况下，请考虑使用[云至设备方法][lnk-methods]。

在本教程中，应用程序的后端更改遥测配置目标设备的因此，该设备应用程序遵循一个多步骤过程应用配置更新 （例如需要软件模块重新启动），用于本教程模拟简单的延迟）。

后端存储配置设备双子星所需的属性中以下列方式︰

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] 由于配置可以为复杂对象，他们通常指定唯一的 id (哈希值或[Guid][lnk-guid]) 为简化其比较。

设备应用程序报告其镜像所需的属性**telemetryConfig**在报告属性的当前配置︰

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

请注意如何报告的**telemetryConfig**具有其他属性**状态**，用来报告配置更新进程的状态。

当收到新的所需的配置时，该设备应用程序将报告通过更改的信息挂起的配置︰

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

然后，在以后的某个时间，设备的应用程序将报告此操作失败的成功通过更新上述属性。
请注意后, 端是如何能够，在任何时候，若要跨所有设备查询配置过程的状态。

本教程展示如何为︰

- 创建从后端接收配置更新和报告多个更新作为*报告的属性*配置更新过程的模拟的设备。
- 创建一个后端应用程序，更新设备，所需的配置，然后查询配置更新过程。

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier