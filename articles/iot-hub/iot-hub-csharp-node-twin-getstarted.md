<properties
    pageTitle="Twins 开始 |Microsoft Azure"
    description="本教程展示如何使用 twins"
    services="iot-hub"
    documentationCenter="node"
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

# <a name="tutorial-get-started-with-device-twins-preview"></a>有关设备 twins （预览） 入门教程︰

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

在本教程结束时，您将得到的.NET 版本和 Node.js 控制台应用程序︰

* **AddTagsAndQuery.sln**，旨在从后端，添加标签和查询设备 twins 运行.NET 应用程序。
* **TwinSimulatedDevice.js**，Node.js 应用程序，它可以模拟先前创建的设备标识 IoT 集线器连接的设备并报告其连接条件。

> [AZURE.NOTE] 文章[IoT 集线器 Sdk] [lnk-hub-sdks]提供有关各种 Sdk 信息可用于生成设备和后端应用程序。

若要完成本教程，您需要︰

+ Microsoft Visual Studio 2015年。

+ Node.js 版本 0.10.x 或更高版本。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>创建服务应用程序

在本节中，您将创建 Node.js 控制台应用程序添加位置的元数据与**myDeviceId**相关联的双子星。 然后它查询存储在选择设备的集线器 twins 位于美国，然后是报告蜂窝连接。

1. 在 Visual Studio 中，通过使用**控制台应用程序**项目模板添加到当前解决方案 Visual C# Windows 经典桌面项目。 **AddTagsAndQuery**为项目命名。

    ![新的视觉 C# Windows 经典桌面项目][img-createapp]

2. 在解决方案资源管理器中，右击**AddTagsAndQuery**项目，然后单击**管理 Nuget 程序包**。

3. 在**Nuget 程序包管理器**窗口中，确保已**包括预发布版**， **microsoft.azure.devices**搜索，选择**安装**以安装**Microsoft.Azure.Devices**的*预发布*版本进行包装，并接受使用条款。 此过程将下载、 安装，并将引用添加到[Microsoft Azure IoT 服务 SDK] [ lnk-nuget-service-sdk] Nuget 程序包及其依赖项。

    ![Nuget 程序包管理器窗口][img-servicenuget]

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.Azure.Devices;

5. 将以下字段添加到**程序**类。 占位符值替换为 IoT 中心在上一节中创建的连接字符串。

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. 将下面的方法添加到**程序**类︰

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    **RegistryManager**类公开与服务的设备 twins 交互所需的所有方法。 上面这段代码首先初始化**registryManager**对象中，然后检索双子星的**myDeviceId**，并最后用所需的位置信息来更新其标记。

    在更新之后，它将执行两个查询︰ 第一个选择仅设备位于**Redmond43**的工厂，设备 twins 和第二个精炼查询以选择还会通过手机网络连接的设备。

    请注意上面的代码中，创建**查询**对象时指定的最大返回的文档数。 **查询**对象包含**HasMoreResults**布尔值属性，您可以使用多次调用**GetNextAsTwinAsync**方法来检索所有结果。 调用**GetNextAsJson**方法用于不是设备 twins 等结果的聚合查询的结果。

7. 最后，将以下行添加到**Main**方法︰

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. 运行此应用程序，您应看到索要位于**Redmond43**和无查询，将结果限制为使用移动电话网络的设备的所有设备的查询结果中的一个设备。

    ![在窗口中的查询结果][img-addtagapp]

下一节中，您将创建一个设备应用程序报告连接信息并更改上一节中的查询的结果。

## <a name="create-the-device-app"></a>创建设备应用程序

在本节中，您将创建 Node.js 控制台应用程序连接到**myDeviceId**，作为网络集线器，然后更新其孪生报告属性包含已连接使用的是移动通信网络的信息。

1. 创建一个名为**reportconnectivity**的新空文件夹。 在**reportconnectivity**文件夹中，创建一个新的 package.json 文件，您的命令提示符处使用以下命令。 接受所有默认值︰

    ```
    npm init
    ```

2. 在您的**reportconnectivity**文件夹中的命令提示符处，运行以下命令以安装**azure iot 设备**和**azure iot 设备 mqtt**包︰

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. 使用文本编辑器，在**reportconnectivity**文件夹中创建一个新的**ReportConnectivity.js**文件。

4. 将以下代码添加到**ReportConnectivity.js**文件中，并替换**{设备连接字符串}**占位符使用复制时创建的**myDeviceId**设备标识的连接字符串︰

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    **客户端**对象公开所有需要与设备 twins 从设备进行交互的方法。 前面的代码中，它初始化**客户端**对象后, 检索双子星的**myDeviceId** ，并更新其报告的属性的连接信息。

5. 运行设备应用程序

        node ReportConnectivity.js

    您应该看到该消息`twin state reported`。

6. 现在，设备报告其连接性的信息，它应该出现在这两个查询。 运行.NET **AddTagsAndQuery**应用程序再次运行查询。 **MyDeviceId**此时间应显示在这两个查询结果。

    ![][img-addtagapp2]

## <a name="next-steps"></a>下一步行动
在本教程中，您在 Azure 的门户中，配置一个新 IoT 集线器，然后在中枢的标识注册表中创建一个设备标识。 从后端应用程序，添加为标签的设备元数据和模拟的设备应用程序写入报告中设备两路输出的设备连接信息。 您还学习了如何查询使用 IoT 集线器类似于 SQL 的查询语言中的此信息。

使用下列资源了解如何︰

- 从[入门 IoT 集线器]的设备发送遥测[lnk-iothub-getstarted]教程，
- 通过[使用所需的属性来配置设备]利用两路输出的所需的属性配置设备[lnk-twin-how-to-configure]教程，
- 与[使用直接方法]控制设备以交互方式 （例如，打开一个用户控制的应用程序中的风扇）[lnk-methods-tutorial]教程。

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

