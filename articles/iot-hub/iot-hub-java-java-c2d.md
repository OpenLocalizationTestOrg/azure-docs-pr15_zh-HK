<properties
    pageTitle="发送 IoT 中枢云至设备信息 |Microsoft Azure"
    description="按照本教程来学习如何将 Java 中使用 Azure IoT 集线器的云至设备消息发送。"
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>教程︰ 如何发送 IoT 集线器和 Java 云至设备信息

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>介绍

Azure IoT 集线器是完全托管的服务，可帮助使数以百万计的 IoT 设备之间的可靠和安全双向通信和应用程序后结束。 [IoT 中心入门]教程演示如何创建 IoT 集线器、 调配，设备标识和模拟的设备的发送设备与云消息的代码。

本教程为基础[入门 IoT 集线器]。 它为您显示方式为︰

- 从您应用程序的云后端，向单个设备通过 IoT 集线器云至设备信息。
- 接收设备上的云至设备信息。
- 应用云从后端，消息发送到设备从 IoT 集线器请求交货确认 （*反馈*）。

您可以找到[IoT 中心开发人员指南 》]中云至设备信息的详细信息[IoT Hub Developer Guide - C2D]。

在本教程结束时，您可以运行两个 Java 控制台应用程序︰

* **模拟设备**，在[入门 IoT 集线器]，它连接到您 IoT 中枢并接收云至设备信息中创建应用程序的修改后的版本。
* **c2d 的发送邮件**，这些云至设备信息将发送到模拟设备通过 IoT 集线器，然后接收其交货确认。

> [AZURE.NOTE] IoT 集线器有 SDK 支持很多设备平台和语言 （包括 C、 Java 和 Javascript） 通过 Azure IoT 设备 Sdk。 有关如何将设备连接到本教程中的代码，通常到 Azure IoT 集线器的分步说明，请参阅[Azure IoT 开发中心]。

若要完成本教程，您需要以下各项︰

+ Java SE 8。 <br/> [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上本教程安装 Java。

+ Maven 3。  <br/> [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上安装本教程的 Maven。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

## <a name="receive-messages-on-the-simulated-device"></a>在模拟的设备上接收消息

在本节中，您可以修改模拟的设备应用程序[入门 IoT 集线器]从 IoT 集线器接收云至设备信息中创建。

1. 使用文本编辑器，打开 simulated-device\src\main\java\com\mycompany\app\App.java 文件。

2. 将下面的**MessageCallback**类添加为**应用程序**类中的嵌套类。 设备从 IoT 集线器接收消息时调用**执行**的方法。 在此示例中，设备始终通知中心完成邮件︰

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. 修改之前它会打开客户端，如下所示调用**setMessageCallback**方法来创建一个**MessageCallback**实例的**main**方法︰

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] 而不是 MQTT 或 AMQP 使用 HTTP 作为传输时，如果**DeviceClient**实例检查邮件不常 IoT 集线器 （不少于每 25 分钟）。 MQTT、 AMQP 和 HTTP 支持和 IoT 中枢调节之间的区别的更多信息，请参见[IoT 中心开发人员指南 》][IoT Hub Developer Guide - C2D]。

## <a name="send-a-cloud-to-device-message"></a>发送云至设备信息

在本节中，您将创建 Java 控制台应用程序中，将云至设备信息发送到模拟的设备应用程序。 您需要添加[IoT 中心入门]教程中的设备 Id 的设备。 您可以找到在[Azure 门户]的 IoT 中枢的还要连接字符串。

1. 创建一个 Maven 项目称作**c2d 的发送邮件**命令提示符处使用以下命令。 请注意这是一个单一的、 长的命令︰

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. 在您的命令提示符下，导航到新的 c2d 的发送邮件文件夹。

3. 使用文本编辑器，打开 c2d 的发送邮件文件夹中 pom.xml 文件和**依赖关系**节点添加以下依存关系。 添加依赖项使您能够在应用程序中使用**iothub java-服务客户机**软件包与 IoT 中心服务通信︰

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. 保存并关闭 pom.xml 文件。

5. 使用文本编辑器，打开 send-c2d-messages\src\main\java\com\mycompany\app\App.java 文件。

6. 向文件中添加下面的**导入**语句︰

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. 将下面的类级变量添加到**{yourhubconnectionstring}**和**{yourdeviceid}**使用的值替换您设定更早版本的**应用程序**类︰

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. 用下面的代码，它连接到 IoT 中心、 向移动设备发送消息，然后等待设备接收和处理消息的确认替换**main**方法︰

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，应该实现重试策略 （如指数退避算法），根据 MSDN 文章︰[瞬时性故障处理]中的建议。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1. 在模拟设备文件夹中的命令提示符处，运行以下命令开始到 IoT 集线器发送遥测和云至设备发送的邮件从您的中枢侦听︰

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![运行模拟的设备应用程序][img-simulated-device]

2. 在 c2d 的发送邮件文件夹中的命令提示符处，运行以下命令来发送云至设备信息，并等待反馈确认︰

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![运行该命令以发送云至设备信息][img-send-command]

## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何发送和接收云至设备信息。 

若要查看使用 IoT 集线器的完整的端到端解决方案的示例，请参见[Azure IoT 套件]。

若要了解有关使用 IoT 中心开发解决方案的详细信息，请参阅[IoT 中心开发指南 》]。


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[开始使用 IoT 集线器]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT 中心开发指南]: iot-hub-devguide.md
[Azure IoT 开发人员中心]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[瞬时故障处理]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure 门户]: https://portal.azure.com
[Azure IoT 套件]: https://azure.microsoft.com/documentation/suites/iot-suite/