<properties
    pageTitle="开始使用 IoT 中心网关 SDK |Microsoft Azure"
    description="本 Azure IoT 网关 SDK 演练使用 Linux 来说明使用 Azure IoT 网关 SDK 时应该了解的关键概念。"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT 网关 SDK （测试版）-使用 Linux 入门

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>如何生成示例

在开始之前，您必须[设置您的开发环境][lnk-setupdevbox]使用 SDK 在 Linux 上的。

1. 打开外壳。
2. 导航到本地副本中的**azure iot 网关 sdk**存储库的根文件夹。
3. 运行**tools/build.sh**脚本。 此脚本使用**cmake**实用程序创建文件夹本地副本的**azure iot 网关 sdk**存储库的根文件夹中名为**构建**并生成生成文件。 然后，脚本生成解决方案并运行测试。

> [AZURE.NOTE]  每次您运行**build.sh**脚本，它删除，然后重新创建本地副本的**azure iot 网关 sdk**存储库的根文件夹中的**生成**文件夹。

## <a name="how-to-run-the-sample"></a>如何运行该示例

1. **Build.sh**脚本**azure iot 网关 sdk**存储库中的本地副本中的**生成**文件夹中生成相应的输出。 这包括在此示例中使用两个模块。

    生成脚本放置在**liblogger_hl.so** **生成/模块/记录器/**文件夹，并在**libhello_world_hl.so** **生成/模块/hello_world/**文件夹。 这些路径用于**模块路径**值，如下面的 JSON 设置文件中所示。

2. **示例/hello_world/src**文件夹中文件**hello_world_lin.json**是可用于运行该示例的 Linux 设置文件的示例 JSON。 下面显示的示例 JSON 设置假定您将从您的本地副本**azure iot 网关 sdk**存储库的根运行该示例。

3. 为**logger_hl**模块中的**参数**部分中，**文件名**将值设置为的名称和包含这些日志数据的文件的路径。

    这是用于将写入运行示例文件夹到**log.txt**的 Linux 的 JSON 设置文件的示例。

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. 在外壳程序中，导航到**azure iot 网关 sdk**文件夹。
4. 运行以下命令︰
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
