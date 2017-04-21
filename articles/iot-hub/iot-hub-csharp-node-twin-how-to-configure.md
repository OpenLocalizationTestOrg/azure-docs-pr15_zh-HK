<properties
    pageTitle="使用两路输出属性 |Microsoft Azure"
    description="本教程展示如何使用两路输出属性"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>教程︰ 使用所需的属性来配置设备 （预览）

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

在本教程结束时，您将拥有两个 Node.js 控制台应用程序︰

* **SimulateDeviceConfiguration.js**，一个模拟的设备应用程序的等待所需的配置更新和报告模拟的配置更新过程的状态。
* **SetDesiredConfigurationAndQuery**，旨在从后端，将所需的配置设置设备和查询配置更新过程运行.NET 控制台应用程序。

> [AZURE.NOTE] 文章[IoT 集线器 Sdk] [lnk-hub-sdks]提供有关各种 Sdk 信息可用于生成设备和后端应用程序。

若要完成本教程，您需要︰

+ Microsoft Visual Studio 2015年。

+ Node.js 版本 0.10.x 或更高版本。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

如果您遵循[入门设备 twins] [lnk-twin-tutorial]教程中，您已经有一个设备启用管理集线器和设备标识调用**myDeviceId**;您可以跳到[模拟的设备应用程序创建][lnk-how-to-configure-createapp]部分。

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>创建模拟的设备应用程序

在本节中，您将创建 Node.js 控制台应用程序连接到网络集线器作为**myDeviceId**，等待所需的配置更新，然后报告模拟的配置更新过程与更新。

1. 创建一个名为**simulatedeviceconfiguration**的新空文件夹。 在**simulatedeviceconfiguration**文件夹中，创建一个新的 package.json 文件，您的命令提示符处使用以下命令。 接受所有默认值︰

    ```
    npm init
    ```

2. 在您的**simulatedeviceconfiguration**文件夹中的命令提示符处，运行以下命令以安装**azure iot 设备**和**azure iot 设备 mqtt**包︰

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. 使用文本编辑器，在**simulatedeviceconfiguration**文件夹中创建一个新的**SimulateDeviceConfiguration.js**文件。

4. 将以下代码添加到**SimulateDeviceConfiguration.js**文件中，并替换**{设备连接字符串}**占位符使用复制时创建的**myDeviceId**设备标识的连接字符串︰

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    **客户端**对象公开与设备 twins 从设备进行交互所需的所有方法。 前面的代码中，它初始化**客户端**对象后, 检索双子星的**myDeviceId**，并将附加的处理程序所需的属性上的更新。 该处理程序验证有实际的配置更改请求通过比较 configIds，则调用方法来启动配置更改。

    请注意，为了简单起见，前面的代码中使用硬编码的默认初始配置。 一个真正的应用程序可能会从本地存储区加载该配置。
    
    > [AZURE.IMPORTANT] 请务必检查实际更改所需的属性中执行任何操作之前，所需的属性更改事件总是一次发射在设备连接。

5. 添加以下方法之前`client.open()`调用︰

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    **InitConfigChange**方法与配置更新请求更新报告本地双子星对象上的属性并将状态设置为**挂起**，然后更新服务上的设备双子星。 已成功更新后的双子星，它来模拟长时间运行的进程终止执行过程中的**completeConfigChange**。 此方法将更新本地双子星报告的属性将状态设置为**成功**和删除**pendingConfig**对象。 然后，更新服务上的双子星。

    请注意，为了节省带宽，该报告更新属性通过指定只进行修改 (命名的**修补程序**在上面的代码)，而不是替换整个文档的属性。

    > [AZURE.NOTE] 本教程不模拟并发配置更新的任何行为。 某些配置更新进程也许能够适应目标配置的更改，并更新正在运行，其他人可能需要排队，其他人无法拒绝它们并显示错误条件。 一定要考虑所需的行为，为您的特定配置过程，并在开始进行配置更改之前添加相应的逻辑。

6. 运行设备的应用程序︰

        node SimulateDeviceConfiguration.js

    您应该看到该消息`retrieved device twin`。 请运行该应用程序。

## <a name="create-the-service-app"></a>创建服务应用程序

在本节中，您将创建.NET 控制台应用程序更新*所需的属性*上与**myDeviceId**具有一个新的遥测配置对象相关联的双子星。 然后查询存储在中心设备 twins，并显示该设备的所需，并报告配置之间的差异。

1. 在 Visual Studio 中，通过使用**控制台应用程序**项目模板添加到当前解决方案 Visual C# Windows 经典桌面项目。 **SetDesiredConfigurationAndQuery**为项目命名。

    ![新的视觉 C# Windows 经典桌面项目][img-createapp]

2. 在解决方案资源管理器中，右击**SetDesiredConfigurationAndQuery**项目，然后单击**管理 Nuget 程序包**。

3. 在**Nuget 程序包管理器**窗口中，确保已**包括预发布版**， **microsoft.azure.devices**搜索，选择**安装**以安装**Microsoft.Azure.Devices**的*预发布*版本进行包装，并接受使用条款。 此过程将下载、 安装，并将引用添加到[Microsoft Azure IoT 服务 SDK] [ lnk-nuget-service-sdk] Nuget 程序包及其依赖项。

    ![Nuget 程序包管理器窗口][img-servicenuget]

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. 将以下字段添加到**程序**类。 占位符值替换为 IoT 中心在上一节中创建的连接字符串。

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. 将下面的方法添加到**程序**类︰

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    **注册表**对象公开与服务的设备 twins 交互所需的所有方法。 前面的代码中，它初始化**注册表**对象后, 检索为**myDeviceId**，双子星和与新的遥测配置对象更新其所需的属性。
    在此之后，每隔 10 秒钟，它查询存储在中心 twins 和打印所需和报告遥测配置。 请参阅[IoT 中心查询语言][lnk-query]以了解如何在所有设备生成丰富的报告。

    > [AZURE.IMPORTANT] 此应用程序查询 IoT 集线器用于说明目的每隔 10 秒。 跨多个设备，生成面向用户的报表并不进行检测的更改，请使用查询。 如果您的解决方案需要实时的设备事件通知使用[设备到云消息][lnk-d2c]。

7. 最后，将以下行添加到**Main**方法︰

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. **SimulateDeviceConfiguration.js**运行时，运行使用**F5**和您的 Visual Studio.NET 应用程序应该可以看到从**成功**到**挂起**的**成功**再次更改与活动的新报告的配置发送频率而不是 24 小时五分钟。

    > [AZURE.IMPORTANT] 没有设备报告操作和查询结果之间一分钟的延迟。 这是为了使查询基础结构能够在很高的比例。 若要检索单个双子星的一致视图**注册表**类中使用**getDeviceTwin**方法。

## <a name="next-steps"></a>下一步行动

在本教程中，您可以从后端，将所需的配置设置为*所需属性*，编写设备应用程序检测到所做的更改并模拟多步更新进程向双子星作为*报告的属性*报告其状态。

使用下列资源了解如何︰

- 从[入门 IoT 集线器]的设备发送遥测[lnk-iothub-getstarted]教程，
- 计划或执行操作的设备请参阅大集上[使用作业调度和广播设备操作][lnk-schedule-jobs]教程。
- 与[使用直接方法]控制设备以交互方式 （例如，打开一个用户控制的应用程序中的风扇），[lnk-methods-tutorial]教程。

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
