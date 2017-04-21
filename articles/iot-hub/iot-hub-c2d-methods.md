<properties
 pageTitle="直接使用 |Microsoft Azure"
 description="本教程展示如何使用直接方法"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>教程︰ 使用直接方法

## <a name="introduction"></a>介绍

Azure IoT 集线器是一种完全托管的服务，使可靠和安全数百万 IoT 设备和应用程序之间的双向通信的后端。 上一教程 （[入门 IoT 集线器]和[云至设备使用发送邮件 IoT 集线器]） 说明 IoT 集线器的基本设备到云，云至设备消息传递功能。 IoT 中心还提供了非耐用设备从云上的方法调用的能力。 方法表示与类似于 HTTP 调用的设备交互的请求-回复，无论成功或失败 （紧跟用户指定的超时） 以使用户知道呼叫的状态。 [在设备上的直接方法调用][lnk-devguide-methods]介绍中详细的方法，并提供了有关何时使用方法而不是云至设备信息的指南。

本教程展示如何为︰

- 使用 Azure 门户创建 IoT 集线器和 IoT 中心中创建设备的标识。
- 创建一个具有一个直接的方法，可以称为云的模拟的设备。
- 创建一个控制台应用程序调用上 IoT 中心通过在模拟设备的直接方法。

> [AZURE.NOTE] 到目前为止，直接的方法是只能从连接到使用 MQTT 协议 IoT 集线器设备的访问。 [MQTT 支持]，请参考[lnk-devguide-mqtt]如何转换现有的设备应用程序说明使用 MQTT 的文章。

在本教程结束时，您有两个 Node.js 控制台应用程序︰

* **CallMethodOnDevice.js**，这对模拟设备调用方法并显示响应。
* **SimulatedDevice.js**，其中 IoT 集线器连接与先前创建的设备标识并响应由云调用该方法。

> [AZURE.NOTE] 文章[IoT 集线器 Sdk] [lnk-hub-sdks]提供有关可用于在设备和解决方案后台运行两个应用程序生成的各种 Sdk 信息。

若要完成本教程，您需要以下各项︰

+ Node.js 版本 0.10.x 或更高版本。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>创建一个模拟的设备应用程序

在本节中，您将创建 Node.js 控制台应用程序中，响应由云调用的方法。

1. 创建一个名为**simulateddevice**的新空文件夹。 在**simulateddevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。 接受所有默认值︰

    ```
    npm init
    ```

2. 在您的**simulateddevice**文件夹中的命令提示符处，运行以下命令以安装**azure iot 设备**设备 SDK 包和**azure iot 设备 mqtt**包︰

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. 使用文本编辑器，在**simulateddevice**文件夹中创建一个新的**SimulatedDevice.js**文件。

4. 添加以下`require` **SimulatedDevice.js**文件开头的语句︰

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. 添加**连接字符串**变量，并使用它来创建设备的客户机。 **{设备连接字符串}**替换为您在*创建设备标识*部分中生成的连接字符串︰

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. 添加下面的函数在设备上实现方法︰

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. 打开的连接到您 IoT 集线器并开始初始化方法侦听程序︰

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. 保存并关闭**SimulatedDevice.js**文件。

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，您应该实现重试策略 （如重试连接），建议在 MSDN 文章︰[瞬时性故障处理][lnk-transient-faults]。

## <a name="call-a-method-on-a-device"></a>在设备上调用的方法

在本节中，您将创建 Node.js 控制台应用程序模拟设备上调用方法，然后显示响应。

1. 创建一个名为**callmethodondevice**的新空文件夹。 在**callmethodondevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。 接受所有默认值︰

    ```
    npm init
    ```

2. 在您的**callmethodondevice**文件夹中的命令提示符处，运行以下命令以安装**azure iothub**软件包︰

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. 使用文本编辑器，创建的**callmethodondevice**文件夹中的**CallMethodOnDevice.js**文件。

4. 添加以下`require` **CallMethodOnDevice.js**文件开头的语句︰

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. 添加以下变量声明，并将占位符值替换为 IoT 集线器的连接字符串︰

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. 创建客户端打开到 IoT 集线器连接。

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. 添加下面的函数调用设备方法和打印到控制台设备响应︰

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. 保存并关闭**CallMethodOnDevice.js**文件。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1. 在**simulateddevice**文件夹中的命令提示符处，运行以下命令以启动侦听 IoT 集线器从方法调用︰

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. 在**callmethodondevice**文件夹中的命令提示符处，运行以下命令以开始监视 IoT 中心︰

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. 您将看到设备响应通过打印出消息和从设备调用方法显示响应的应用程序的方法︰

    ![][9]
    
## <a name="next-steps"></a>下一步行动

在本教程中，您在 Azure 的门户中，配置一个新 IoT 集线器，然后在中枢的标识注册表中创建一个设备标识。 此设备标识用于启用模拟的设备的应用程序响应调用云的方法。 您还可以创建一个应用程序，调用方法在设备上的，并显示来自设备的响应。 

若要继续入门 IoT 集线器并探索其他 IoT 方案，请参阅︰

- [开始使用 IoT 集线器]
- [多台设备上的计划作业][lnk-devguide-jobs]

若要了解如何扩展您 IoT 解决方案和时间表的方法调用多个设备，请参阅[计划和广播的作业][lnk-tutorial-jobs]教程。

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[发送 IoT 中枢云至设备信息]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[开始使用 IoT 集线器]: iot-hub-node-node-getstarted.md
