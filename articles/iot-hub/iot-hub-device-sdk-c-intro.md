<properties
    pageTitle="C 为使用 Azure IoT 设备 SDK |Microsoft Azure"
    description="了解并开始使用 Azure IoT 设备 SDK 中的示例代码为 c。"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>对于 C 推出 Azure IoT 设备 SDK

**Azure IoT 设备 SDK**是一组库旨在简化发送事件和接收的邮件从**Azure IoT 中心**服务的过程。 有不同的变体的 SDK，每个目标特定的平台，但这篇文章描述的**Azure IoT 设备用于 c 语言的 SDK**。

在 ANSI C 中 (C99) 最大限度的可移植性地编写 C Azure IoT 设备 SDK。 这使得它适合运行在多个平台和设备，尤其是在最小化磁盘和内存占用量是头等大事。  

有范围广泛的平台的 SDK 已经过测试 （请参见[Azure 认证 IoT 设备目录](https://catalog.azureiotsuite.com/)的详细信息）。 尽管这篇文章包含在 Windows 平台上运行的示例代码的演练，请注意，本文中介绍的代码是完全相同的范围内支持的平台。

在本文中您将介绍到 Azure IoT 设备 SDK 的体系结构为 c。我们将演示如何初始化设备库、 将事件发送到 IoT 集线器以及从中接收消息。 这篇文章中的信息应该足够用来开始使用 SDK，但还提供了指向有关这些库的其他信息。

## <a name="sdk-architecture"></a>SDK 的体系结构

可以在[Microsoft Azure IoT Sdk](https://github.com/Azure/azure-iot-sdks) GitHub 储存库中查找**Azure IoT 设备用于 c 语言的 SDK**并查看[C API 参考](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)中的 API 的详细信息。

此资料库的**主**分支中找不到最新版本的库︰

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

该存储库包含整个产品系列的 Azure IoT 设备的 Sdk。 但是，这篇文章是有关 Azure IoT 设备 SDK*为 C* ，可以在**c**文件夹中找到。

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* 可在 SDK 的核心实现**iothub\_客户端**文件夹，其中包含在 SDK 中的最低 API 层的实现︰ **IoTHubClient**库。 **IoTHubClient**库中包含实现将消息发送到 IoT 集线器以及从中接收消息的原始消息的 Api。 在使用此库时，您有责任实现消息序列化 （最终使用的序列化程序示例，如下所述），但需要处理与 IoT 中心进行通信的其他详细信息。
* **序列化程序**文件夹中包含帮助程序函数和示例演示了如何将数据序列化发送到 Azure IoT 中心使用客户端库之前。 注意使用的序列化程序不是必需的也是唯一的提供方便。 如果您使用的**序列化程序**库，您首先定义指定您想要发送到 IoT 集线器以及您希望从其接收邮件的事件模型。 一旦模型定义，SDK 提供了 API 面，使您可以轻松地处理事件和消息而无需担心有关序列化的详细信息。
库取决于实现使用多种协议 （AMQP MQTT） 传输其他开放源代码库。
* **IoTHubClient**库取决于其他开源库︰
   * 提供通用的功能为基本任务的[Azure C 共享实用程序](https://github.com/Azure/azure-c-shared-utility)库 （如字符串列表操作、 IO，等...） 需要跨多个 Azure 相关 C Sdk
   * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c)库即 AMQP 针对资源约束设备而优化的客户端实现方案。
   * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c)库，从而实现 MQTT 协议是通用库和资源约束的设备进行了优化。

所有这些是更容易通过查看示例代码来了解。 以下各节将指导您完成几个 SDK 中包含的示例应用程序。 这会为 SDK 的体系结构层，以及 Api 的工作原理的介绍的各种功能使您很好的感觉。

## <a name="before-running-the-samples"></a>之前运行示例

C 中可以运行在 Azure IoT 设备 SDK 示例之前您必须 Azure 订阅创建服务的实例，如果不已经有一个并且完成 2 的任务︰
* 准备您的开发环境
* 获取设备的凭据。

如果需要在 Azure 订阅创建 Azure IoT 集线器的一个实例，请按照说明[此处](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md)。

Sdk 随附的[自述文件](https://github.com/Azure/azure-iot-sdks/tree/master/c)说明如何准备您的开发环境并获得设备的凭据。
以下各节包含一些其他说明，这些说明。

### <a name="preparing-your-development-environment"></a>准备您的开发环境

虽然包提供了一些操作平台 （例如 Windows 的 NuGet 或 Debian 和 Ubuntu 的 apt_get） 和示例使用这些软件包时，以下说明详细说明了如何生成库并直接示例窗体的代码。

首先，您需要从 GitHub 获取 SDK 的副本，然后再生成源。 您应该从[GitHub 资料库](https://github.com/Azure/azure-iot-sdks)的**主**分支获取源文件的副本。

当您已经下载源文件的副本时，您必须完成 SDK 文章["准备您的开发环境"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)中介绍的步骤。


以下是一些提示，帮助您完成准备指南中介绍的步骤︰

-   安装**CMake**实用程序时，选择选项以将**CMake**添加到系统路径中的**所有用户**（添加到**当前用户**工作方式）︰

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   在打开**的 VS2015 的开发人员命令提示符**之前，安装 Git 命令行工具。 若要安装这些工具，请完成以下步骤︰

    1. 启动**Visual Studio 2015年**安装程序 （或从**程序和功能**控制面板和选择**更改**选择**Microsoft Visual Studio 2015年**）。
    
    2. 请确保**Git 的 Windows**功能在安装程序中选择，但可能还要检查提供 IDE 集成的**Visual Studio 的 GitHub 扩展**选项︰

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. 完成安装向导来安装该工具。

    4. 将 Git 工具**bin**目录添加到系统**PATH**环境变量中。 在 Windows 上，这看起来如下所示︰

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


当您已完成所有["准备您的开发环境"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)页中所述的步骤时，就可以编译示例应用程序。

### <a name="obtaining-device-credentials"></a>获取设备凭据

现在，您的开发环境的设置，下一步就是凭据的获得一套设备。  若要能够访问 IoT 集线器的设备，必须先添加到 IoT 集线器设备注册表设备。 当您添加您的设备将获得一套设备凭据，这将需要能够连接到 IoT 集线器上的设备的顺序。 我们来看下一节中的示例应用程序期望的**设备连接字符串**形式这些凭据。

有在 SDK 开放源代码存储库中，以帮助管理 IoT 集线器提供的一些工具。 一个是的 Windows 应用程序调用设备资源管理器，第二个是 node.js 基于名为 iothub 的资源管理器中的跨平台 CLI 工具。 您可以了解有关这些工具的详细信息[在此处](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md)。

随着我们将通过在 Windows 上运行示例，在这篇文章中，我们使用设备管理器工具。 但是，您还可以使用 iothub 资源管理器中，如果您更愿意使用 CLI 工具。

[设备资源管理器](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer)工具使用 Azure IoT 服务库 IoT 集线器，包括添加设备上执行各种功能。 如果您使用设备资源管理器中添加设备，您将获得相应的连接字符串。 此连接字符串以使应用程序运行此示例，您需要。

如果您并不熟悉此过程，下面的过程介绍如何使用设备资源管理器添加一个设备并获取设备的连接字符串。

Windows 安装程序可以查找上[SDK 版本页](https://github.com/Azure/azure-iot-sdks/releases)的设备资源管理器工具。 但是，也可以运行该工具，直接从其代码在**2015 Visual Studio**中打开**[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** ，并生成解决方案。

当您运行该程序时，您将看到此接口︰

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

在第一个字段中输入您**IoT 集线器连接字符串**并单击**更新**。 这将配置该工具，以便它可以与 IoT 中心通信。

IoT 集线器连接字符串配置后，请单击**管理**选项卡︰

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

这是将在其中管理中 IoT 中心注册的设备。

您可以通过单击**创建**按钮创建一个设备。 将显示一个对话框，其中一组预先填充键 （主和次）。 您所要做的就是输入**设备 ID** ，然后单击**创建**。

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

创建设备后，设备列表更新所有已注册的设备，包括您刚刚创建的一个。 如果您用鼠标右键单击您的新设备，您将看到此菜单︰

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

如果您选择**复制所选设备的连接字符串**选项，则将您的设备的连接字符串复制到剪贴板。 保存连接字符串的副本。 下一节中描述的示例应用程序运行时，您将需要它。

完成上述步骤后，您就可以开始运行某些代码。 这两个示例允许您输入的连接字符串的主源文件顶部具有常数。 例如，相应行从**iothub\_客户端\_示例\_amqp**应用程序如下所示。

```
static const char* connectionString = "[device connection string]";
```

如果您想要跟着，输入设备连接字符串，请重新编译该解决方案，您可以运行该示例。

## <a name="iothubclient"></a>IoTHubClient

在**iothub\_客户端**文件夹 iot 的 azure sdk 在存储库中，，没有包含调用的应用程序的**示例**文件夹**iothub\_客户端\_示例\_amqp**。

在 Windows 版本**iothub\_客户端\_示例\_ampq**应用程序包括以下 Visual Studio 解决方案︰

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

此解决方案只包含一个项目。 值得注意的，有四个 NuGet 程序包安装此解决方案中︰

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

当您正在使用 SDK 时，总是需要**Microsoft.Azure.C.SharedUtility**软件包。 由于此示例依赖于 AMQP，还必须包括 （有等效软件包 MQTT 和 HTTP） 的**Microsoft.Azure.uamqp**和**Microsoft.Azure.IoTHub.AmqpTransport**软件包。 由于此示例使用**IoTHubClient**库，您还必须在解决方案中包括**Microsoft.Azure.IoTHub.IoTHubClient**软件包。

您可以查找的示例应用程序中实现**iothub\_客户端\_示例\_amqp.c**源文件。

我们将使用此示例应用程序来指导您需要使用**IoTHubClient**库。

### <a name="initializing-the-library"></a>初始化库

> [AZURE.NOTE] 与库开始工作之前，您可能需要执行一些平台特定的初始化。 例如，如果您打算使用 Linux AMQP 必须初始化 OpenSSL 库。 [GitHub 存储库](https://github.com/Azure/azure-iot-sdks)中的示例调用实用程序函数**platform_init** ，当客户端启动并在退出之前调用**platform_deinit**函数。 在"platform.h"头文件中声明这些函数。 您的目标平台，以确定是否需要在您的客户端中包含任何平台初始化代码[存储库](https://github.com/Azure/azure-iot-sdks)中，您应检查这些函数的定义。

若要开始处理与库必须首先分配 IoT 中心客户端句柄︰

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

请注意我们正在将我们设备连接字符串的副本传递给此函数 （一个我们从设备资源管理器中获得）。 我们还可以指定要使用的协议。 此示例使用 AMQP，但 MQTT 和 HTTP 也是一个选项。

具有有效**IOTHUB\_客户端\_处理**，您可以启动调用 Api 来发送事件和接收来自 IoT 集线器。 我们来看下一步。

### <a name="sending-events"></a>发送事件

将事件发送到 IoT 中心需要您完成以下步骤︰

首先，创建一条消息︰

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

接下来，发送邮件︰

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

每次发送邮件时，您可以指定发送数据时调用的回调函数的引用︰

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

请注意调用**IoTHubMessage\_销毁**函数完成后的消息。 必须进行此调用，以释放创建消息时所分配的资源。

### <a name="receiving-messages"></a>接收消息

收到一条消息是一个异步操作。 首先，您注册该设备收到消息时要调用的回调︰

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

最后一个参数是随意的 void 指针。 在示例中，它是指向整数的指针，但它可能是更复杂的数据结构的指针。 这样的回调函数对此函数的调用方具有共享状态的操作。

当设备接收到一条消息时，则将调用注册的回调函数︰

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

请注意，使用**IoTHubMessage\_GetByteArray**函数以检索邮件，它在本例中是一个字符串。

### <a name="uninitializing-the-library"></a>取消初始化库

当您完成时发送事件和接收的邮件时，您可以取消初始化 IoT 库。 为此，请发出下面的函数调用︰

```
IoTHubClient_Destroy(iotHubClientHandle);
```

这将释放以前分配的资源**IoTHubClient\_CreateFromConnectionString**函数。

正如您所看到的很容易地发送事件和接收与**IoTHubClient**库的消息。 库处理与 IoT 中心，包括要使用哪个协议进行通信的详细信息 （从开发人员的角度来看，这是一个简单的配置选项）。

**IoTHubClient**库还提供了精确控制如何序列化您的设备发送到 IoT 集线器的事件。 在某些情况下这是一大优势，但在其他情况下这是不希望关心与其实现详细信息。 如果真是这样，您可以考虑使用**序列化程序**库中，我们将在下一节中介绍。

## <a name="serializer"></a>序列化程序

从概念上讲，**序列化程序**库之上在 SDK 中的**IoTHubClient**库。 它使用**IoTHubClient**库 IoT 集线器与基础通信但会删除消息序列化处理的负担，从开发人员的建模能力。 此库的工作原理是最佳的方式通过一个示例来演示。

**序列化程序**中 iot 的 azure sdk 存储库中的文件夹是包含应用程序调用的**示例**文件夹**simplesample\_amqp**。 此示例的 Windows 版本包括以下 Visual Studio 解决方案︰

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

与上面的示例中，此示例包含几个 NuGet 程序包︰

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

我们已经看到在上面的示例中，其中的大部分，但**Microsoft.Azure.IoTHub.Serializer**是新的。 当我们使用的**序列化程序**库，这是必需的。

您可以查找的示例应用程序中实现**simplesample\_amqp.c**文件。

以下各节将指导您完成此示例中的关键部分。

### <a name="initializing-the-library"></a>初始化库

若要开始处理与**序列化程序**库，您必须调用初始化 Api:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

对**的序列化程序\_初始化**函数是一次性调用，用来初始化基础库。 然后，调用**IoTHubClient\_CreateFromConnectionString**函数，如下所示的**IoTHubClient**示例相同的 API。 此调用设置您的设备连接字符串 (这也是您选择的协议在您想要使用)。 请注意，此示例 AMQP 用作传输，但可以使用 HTTP。

最后，调用**创建\_模型\_实例**函数。 请注意， **WeatherStation**的模型命名空间，并且**ContosoAnemometer**模型的名称。 创建模型实例后，可以用于启动发送事件和接收消息。 但是，请务必了解是何种型号的。

### <a name="defining-the-model"></a>定义模型

**序列化程序**库中的模型定义您的设备可以将发送到 IoT 中心和邮件，在建模语言，它可以接收调用*操作*的事件。 您定义模型使用一组 C 宏如下所示的**simplesample\_amqp**示例应用程序︰

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

**开始\_命名空间**和**结束\_命名空间**宏都采用作为参数模型的命名空间。 预计这些宏之间的任何内容是您的模型和模型使用的数据结构的定义。

在此示例中，没有一个名为**ContosoAnemometer**模型。 该模型定义了您的设备可以将发送到 IoT 中心的两个事件︰ **DeviceId**和**WindSpeed**。 它还定义了三种操作 （消息），您的设备可以接收︰ **TurnFanOn**、 **TurnFanOff**和**SetAirResistance**。 每个事件都有一个类型，并且每个操作名称 （和可选参数的一组）。

事件和模型中定义的操作定义是可用于将事件发送到 IoT 集线器以及响应消息被发送到设备的 API 表面。 这最好通过一个示例来理解。

### <a name="sending-events"></a>发送事件

模型中定义的事件，您可以发送到 IoT 集线器。 在此示例中，这意味着使用**WITH_DATA**宏所定义的两个事件之一。 例如，如果您想要发送一个**WindSpeed**事件 IoT 集线器，有几个步骤在使这一点。 第一种是我们想要发送的数据集︰

```
myWeather->WindSpeed = 15;
```

我们在前面部分定义的模型，我们可以通过设置**结构**的成员。 接下来，我们将序列化为我们要发送的事件︰

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

此代码序列化到缓冲区 （由**目标**引用） 的事件。 最后，我们会向事件发送到 IoT 集线器与此代码︰

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

这是一个 helper 函数，将序列化的事件发送到 IoT 中心示例应用程序中︰

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

此代码是非常类似于我们所看到的在**iothub\_客户端\_示例\_amqp**应用程序中，我们从字节数组中创建一条消息，然后使用**IoTHubClient\_SendEventAsync**以将其发送给 IoT 集线器。 之后，我们就必须释放消息的句柄，序列化我们先前分配的数据缓冲区。

第二行到最后一个参数**IoTHubClient\_SendEventAsync**是成功发送数据时调用的回调函数的引用。 下面是一个回调函数的示例︰

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

第二个参数是一个指针，指向用户上下文;相同的指针我们传递给**IoTHubClient\_SendEventAsync**。 在这种情况下的环境是一个简单的计数器，但它可以是任何需要的内容。

这就是全部就是发送事件。 唯一有待讨论的是如何接收消息。

### <a name="receiving-messages"></a>接收消息

接收工作同样到消息处理**IoTHubClient**库中的消息。 首先，您将注册消息回调函数︰

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

然后，您将编写在接收消息时调用的回调函数︰

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

此代码是样板化--它是相同的任何解决方案。 此函数接收消息并负责传送给通过调用适当的函数**执行\_命令**。 此时，调用的函数取决于我们的模型中的操作的定义。

当您在模型中定义的操作时，则需要实现设备接收相应的消息时所调用的函数。 例如，如果您的模型定义了此操作︰

```
WITH_ACTION(SetAirResistance, int, Position)
```

必须定义使用该签名的函数︰

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

请注意，函数的名称与模型中的操作的名称，并且该函数的参数匹配指定为该操作的参数。 第一个参数始终是必需的并且包含指向我们模型的实例的指针。

当设备接收到一条消息，此签名匹配时，调用对应的函数。 因此，无需包括来自**IoTHubMessage**的样板代码，除了接收消息是只需定义模型中定义的每个操作的简单函数。

### <a name="uninitializing-the-library"></a>取消初始化库

当完成时发送数据和接收的邮件时，您可以取消初始化 IoT 库︰

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

这些三个函数的每个符合前面所述的三种初始化函数。 调用这些 Api 可以确保您释放以前分配的资源。

## <a name="next-steps"></a>下一步行动

本文介绍了使用**Azure IoT 设备 SDK 的 C**库的基本知识。它为您提供足够的信息来了解 SDK 中包含的内容及其体系结构，以及如何使用 Windows 示例开始。 下一篇文章将 SDK 的说明继续解释[有关 IoTHubClient 库的详细信息](iot-hub-device-sdk-c-iothubclient.md)。

有关 IoT 中心开发的详细信息，请参阅[IoT 集线器 Sdk][lnk-sdks]。

要进一步探索 IoT 中心的功能，请参阅︰

- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
