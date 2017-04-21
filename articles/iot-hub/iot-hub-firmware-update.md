<properties
 pageTitle="如何执行固件更新 |Microsoft Azure"
 description="本教程展示如何执行的固件更新"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>教程︰ 如何执行固件更新 （预览）

## <a name="introduction"></a>介绍
在[着手实施设备管理][lnk-dm-getstarted]教程中，您看到了如何使用[设备双子星][lnk-devtwin]和[云至设备 (C2D) 方法][lnk-c2dmethod]基元远程重启设备。 本教程使用同一 IoT 集线器基元和提供指导，并向您显示如何进行端到端模拟的固件更新。  此模式用于在固件更新实现英特尔 Edison 设备示例。

本教程展示如何为︰

- 创建一个控制台应用程序调用 firmwareUpdate 直接模拟设备通过 IoT 集线器上。
- 创建一个模拟的设备，实现 firmwareUpdate 直接方法它经历一个多阶段过程，等待下载固件映像下载固件镜像，并最后应用 th 固件映像。  在整个执行双子星报告要更新属性的设备使用的每个阶段进行。

在本教程结束时，您有两个 Node.js 控制台应用程序︰

**dmpatterns_fwupdate_service.js**，调用在模拟设备的直接方法，其中显示该响应中，并定期 (每个 500) 显示已更新的设备两路输出报告属性。

**dmpatterns_fwupdate_device.js**，与前面创建的设备标识连接到 IoT 集线器，接收 firmwareUpdate 直接方法，通过模拟固件更新包括一个多状态进程运行︰ 等待图像下载、 下载新的映像，并且最后应用映像。


若要完成本教程，您需要以下各项︰

Node.js 版本 0.12.x 或更高版本， <br/>  [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上安装本教程 Node.js。

活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

按照[设备管理入门](iot-hub-device-management-get-started.md)文章创建 IoT 中心并获得您的连接字符串。

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>创建一个模拟的设备应用程序

在本节中，您创建 Node.js 控制台应用程序中，响应由云，触发模拟的设备固件更新调用的直接方法和使用设备双子星报告属性以启用设备双子星查询来识别设备和当他们最后一次重新启动。

1. 创建一个名为**manageddevice**的新空文件夹。  在**manageddevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。  接受所有默认值︰

    ```
    npm init
    ```
    
2. 在您的**manageddevice**文件夹中的命令提示符处，运行以下命令以安装**azure-iot-device@dtpreview**设备 SDK 包和**azure-iot-device-mqtt@dtpreview**软件包︰

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. 使用文本编辑器，在**manageddevice**文件夹中创建一个新的**dmpatterns_fwupdate_device.js**文件。

4. 添加以下 '要求' **dmpatterns_fwupdate_device.js**文件开头的语句︰

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. 添加**连接字符串**变量，并使用它来创建设备的客户机。  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. 添加以下函数将用于更新双子星报告属性

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. 添加以下函数将模拟下载并应用的固件映像。

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. 添加以下函数将更新通过双子星的固件更新状态报告给正在等待下载属性。  通常情况下，设备可供使用更新的通知，管理员定义策略原因要开始下载并应用更新的设备。  这是，将运行的逻辑以启用该策略。  为简单起见，我们延迟 4 秒钟，继续下载固件映像。 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. 添加以下函数将更新通过双子星的固件更新状态报告给下载固件映像属性。  它通过模拟固件下载后面，最后固件更新的状态更新来下载成功或失败的通知。

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. 添加以下函数将更新通过双子星的固件更新状态报告给固件映像应用属性。  它通过模拟固件映像应用后面，最后更新的固件更新状态应用成功或失败的通知。

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. 添加下面的 functoin 处理的 firmwareUpdate 方法，并启动了多阶段固件更新过程。

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. 最后，添加下面的代码将连接到 IoT 中心作为一种设备， 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，您应该实现重试策略 （如指数退避算法），建议在 MSDN 文章︰[瞬时性故障处理][lnk-transient-faults]。

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>触发使用直接方法的设备上远程固件更新 

在本节中，您创建一个 Node.js 控制台应用程序来启动远程固件更新使用直接方法的设备并使用设备双子星查询以定期获得在该设备上的当前固件更新的状态。


1. 创建一个名为**triggerfwupdateondevice**的新空文件夹。  在**triggerfwupdateondevice**文件夹中，创建 package.json 文件在您的命令提示符处使用以下命令。  接受所有默认值︰

    ```
    npm init
    ```
    
2. 在您的**triggerfwupdateondevice**文件夹中的命令提示符处，运行以下命令以安装**azure iothub**设备 SDK 包和**azure iot 设备 mqtt**包︰

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. 使用文本编辑器，在**triggerfwupdateondevice**文件夹中创建一个新的**dmpatterns_getstarted_service.js**文件。

4. 添加以下 '要求' **dmpatterns_getstarted_service.js**文件开头的语句︰

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. 添加以下变量声明并替换占位符值︰

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. 添加以下函数来查找和显示的 firmwareUpdate 值报告属性。

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. 添加下面的函数调用 firmwareUpdate 方法可重新启动目标设备︰

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. 最后，添加以下函数代码以启动固件更新序列和开始定期显示两路输出报告属性︰

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. 保存并关闭**dmpatterns_fwupdate_service.js**文件。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1. 在**manageddevice**文件夹中的命令提示符处，运行以下命令以开始侦听重启的直接方法。

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. 在**triggerfwupdateondevice**文件夹中的命令提示符下，运行的触发器的远程重启和查询来查找最后一个设备双子星的重新启动时间下面的命令。

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. 您将看到直接的方法反应通过打印出消息

## <a name="next-steps"></a>下一步行动

在本教程中，用直接方法触发的设备上远程固件更新和定期使用双子星报告属性以了解进度的固件更新过程。  

若要了解如何扩展您 IoT 解决方案和时间表的方法调用多个设备，请参阅[计划和广播的作业][lnk-tutorial-jobs]教程。

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
