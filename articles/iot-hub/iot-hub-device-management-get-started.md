<properties
 pageTitle="着手实施设备管理 |Microsoft Azure"
 description="本教程展示如何开始使用 Azure IoT 集线器上的设备管理"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-get-started-with-device-management-preview"></a>有关设备管理 （预览） 入门教程︰

## <a name="introduction"></a>介绍
IoT 云应用程序可以使用基元中 Azure IoT 中枢，即设备双子星和直接的方法，以远程启动和监控设备管理操作的设备上。  本文提供的指导和代码 IoT 云应用程序和设备的工作方式组合在一起以启动并监视远程设备重新启动使用 IoT 集线器。

若要远程启动和监控设备管理操作基于云的后端应用程序从您的设备上，使用 Azure IoT 集线器基元 （例如[设备 twin）] [lnk-devtwin]和[云至设备 (C2D) 方法][lnk-c2dmethod]。 本教程展示如何后端应用程序和设备可以协同工作，使您启动和监控 IoT 集线器从远程设备重新启动。

您使用的 C2D 方法启动设备管理操作 （如重新启动、 出厂重置和固件更新） 从一个后端应用程序在云中。 该设备负责︰

- 处理方法请求发送从 IoT 集线器。
- 启动设备上相应的设备特定操作。
- 提供通过设备双子星状态更新报告给 IoT 集线器属性。

可用于后端应用程序在云中运行设备的双子星查询报告设备管理操作的进度。

本教程展示如何为︰

- 使用 Azure 门户创建 IoT 集线器和 IoT 中心中创建设备的标识。
- 创建具有直接的方法，以便重新启动可以称为云的模拟的设备。
- 创建一个控制台应用程序调用重新启动直接模拟设备通过 IoT 集线器上。

在本教程结束时，您有两个 Node.js 控制台应用程序︰

**dmpatterns_getstarted_device.js**，与前面创建的设备标识连接到 IoT 集线器，收到重新启动直接方法、 模拟物理重新启动，并报告在最后一次重新启动的时间。

**dmpatterns_getstarted_service.js**，这对模拟设备调用直接方法，显示响应，并显示更新的设备双子星报告属性。

若要完成本教程，您需要以下各项︰

Node.js 版本 0.12.x 或更高版本， <br/>  [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上安装本教程 Node.js。

活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>创建一个模拟的设备应用程序

在本节中，您创建 Node.js 控制台应用程序响应称为云，从而触发模拟的设备重启的直接方法和使用设备双子星报告属性以启用设备双子星查询来识别设备和当他们最后一次重新启动。

1. 创建一个名为**manageddevice**的新空文件夹。  在**manageddevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。  接受所有默认值︰

    ```
    npm init
    ```
    
2. 在您的**manageddevice**文件夹中的命令提示符处，运行以下命令以安装**azure-iot-device@dtpreview**设备 SDK 包和**azure-iot-device-mqtt@dtpreview**软件包︰

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. 使用文本编辑器，在**manageddevice**文件夹中创建一个新的**dmpatterns_getstarted_device.js**文件。

4. 添加以下 '要求' **dmpatterns_getstarted_device.js**文件开头的语句︰

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. 添加**连接字符串**变量，并使用它来创建设备的客户机。  连接字符串替换为您的设备连接字符串。  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. 添加下面的函数在该设备上实现直接方法

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. 打开 IoT 集线器连接，并启动直接方法侦听器︰

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. 保存并关闭**dmpatterns_getstarted_device.js**文件。

 [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，您应该实现重试策略 （如指数退避算法），建议在 MSDN 文章︰[瞬时性故障处理][lnk-transient-faults]。

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>触发器使用直接方法的设备上远程重新启动 

在本节中，您将创建 Node.js 控制台应用程序启动远程重新启动使用直接方法的设备和设备两路输出查询用于查找该设备的上一次重新启动时间。

1. 创建一个名为**triggerrebootondevice**的新空文件夹。  在**triggerrebootondevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。  接受所有默认值︰

    ```
    npm init
    ```
    
2. 在您的**triggerrebootondevice**文件夹中的命令提示符处，运行以下命令以安装**azure-iothub@dtpreview**设备 SDK 包和**azure-iot-device-mqtt@dtpreview**软件包︰

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. 使用文本编辑器，在**triggerrebootondevice**文件夹中创建一个新的**dmpatterns_getstarted_service.js**文件。

4. 添加以下 '要求' **dmpatterns_getstarted_service.js**文件开头的语句︰

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. 添加以下变量声明并替换占位符值︰

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. 添加下面的函数调用设备方法可重新启动目标设备︰

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. 添加以下函数查询设备并获得最后的重新启动时间︰

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. 添加以下代码，以调用函数将为最后的重新启动时间触发重新启动直接方法和查询︰

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. 保存并关闭**dmpatterns_getstarted_service.js**文件。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1. 在**manageddevice**文件夹中的命令提示符处，运行以下命令以开始侦听重启的直接方法。

    ```
    node dmpatterns_getstarted_device.js
    ```

2. 在**triggerrebootondevice**文件夹中的命令提示符下，运行的触发器的远程重启和查询来查找最后一个设备双子星的重新启动时间下面的命令。

    ```
    node dmpatterns_getstarted_service.js
    ```

3. 您将看到直接的方法反应通过打印出消息

## <a name="customize-and-extend-the-device-management-actions"></a>自定义和扩展设备管理操作

IoT 解决方案可以扩展定义的一组设备管理模式或启用自定义通过使用设备双子星和 C2D 方法基元模式。 其他设备管理操作的示例包括出厂重置、 固件更新、 软件更新、 电源管理、 网络和连接管理和数据加密。

## <a name="device-maintenance-windows"></a>设备维护窗口

通常情况下，您可以配置设备以执行一次的中断和停机时间减到最小的操作。  设备维护窗口是一种常用的模式，以定义设备应该在何时更新其配置的时间。 后端解决方案可以使用设备双子星所需的属性来定义并激活设备能使维护窗口上的策略。 当一个设备接收维护窗口策略时，它可以使用设备双子星报告的属性报告策略的状态。 后端应用程序可以使用设备双子星查询证明设备和每个策略的法规遵从性。

## <a name="next-steps"></a>下一步行动

在本教程中，用直接方法触发在设备上，使用远程重新启动设备双子星属性报告从设备，重新启动上次报告和查询发现云从设备的上次重新启动设备双子星。

若要继续入门 IoT 中心和设备管理模式，如远程通过空气固件更新，请参阅︰

[教程︰ 如何执行的固件更新][lnk-fwupdate]

若要了解如何扩展您 IoT 解决方案和时间表的方法调用多个设备，请参阅[计划和广播的作业][lnk-tutorial-jobs]教程。

要继续入门 IoT 集线器，请参阅[入门 IoT 网关 SDK][lnk-gateway-SDK]。


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx