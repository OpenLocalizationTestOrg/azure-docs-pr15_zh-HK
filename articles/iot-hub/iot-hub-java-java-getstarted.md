<properties
    pageTitle="作为 Java 入门的 azure IoT 中心 |Microsoft Azure"
    description="与 Java 入门的 azure IoT 中心启动教程。 与 Microsoft Azure IoT Sdk 中使用 Azure IoT 中心和 Java，实施联网解决方案。"
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>开始使用 Java 的 Azure IoT 中心

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

在本教程结束时，您有三个 Java 控制台应用程序︰

* **创建设备标识**，它用于创建设备标识和关联的安全密钥，将模拟的设备连接。
* **d2c 的读取消息**，其中显示的发送的模拟设备的遥测数据。
* **模拟设备**，这与先前创建的设备标识 IoT 集线器连接并发送遥测消息每第二个使用 AMQP 协议。

> [AZURE.NOTE] 文章[IoT 集线器 Sdk] [lnk-hub-sdks]提供有关可用于在设备和解决方案后台运行两个应用程序生成的各种 Sdk 信息。

若要完成本教程，您需要以下各项︰

+ Java SE 8。 <br/> [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上本教程安装 Java。

+ Maven 3。  <br/> [准备您的开发环境][lnk-dev-setup]介绍了如何在 Windows 或 Linux 上安装本教程的 Maven。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

作为最后一步，记的**主关键字**值，然后单击**消息**。 **消息**刀片上, 记下的**中心兼容事件名称**和**事件中心兼容终结点**。 当您创建应用程序**读取 d2c 消息**时，您需要这三个值。

![Azure 门户 IoT 中心消息刀片式服务器][6]

现在，您已创建 IoT 网络集线器，并且必须 IoT 中心主机名、 IoT 集线器连接字符串、 IoT 集线器主键、 事件中心兼容名称和完成本教程所需的事件中心兼容终结点。

## <a name="create-a-device-identity"></a>创建设备标识

在本节中，您将创建 IoT 中心标识注册表中创建一个新的设备标识的 Java 控制台应用程序。 除非它设备标识注册表中有一个条目，设备无法连接到 IoT 集线器。 请参阅**设备标识注册表**部分[IoT 中心开发指南 》]的[lnk-devguide-identity]的详细信息。 当您运行该控制台应用程序时，它生成一个唯一的设备 ID 和密钥设备可以用来标识自身设备到云的消息发送到 IoT 集线器时。

1. 创建新的空文件夹称为 iot-java--入门。 在 iot-java-获取启动文件夹中，创建新的 Maven 项目调用您的命令提示符处使用以下命令**创建设备标识**。 请注意这是一个单一的、 长的命令︰

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. 在您的命令提示符下，导航到新文件夹中创建设备标识。

3. 使用文本编辑器，创建设备标识文件夹中打开 pom.xml 文件和**依赖项**节点添加以下依存关系。 这使您可以在您的应用程序中使用 iothub 服务 sdk 程序包︰

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. 保存并关闭 pom.xml 文件。

5. 使用文本编辑器，打开 create-device-identity\src\main\java\com\mycompany\app\App.java 文件。

6. 向文件中添加下面的**导入**语句︰

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. 将下面的类级变量添加到替换**{yourhubconnectionstring}**值您设定更早版本的**应用程序**类︰

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. 修改包含异常，如下所示的**main**方法的签名︰

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. 将下面的代码添加为**主要**方法的主体。 此代码将创建一种称为*javadevice* IoT 中心标识注册表中，如果设备不存在。 然后，它显示的设备 id 和密钥所需的更高版本︰

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. 保存并关闭 App.java 文件。

11. 若要生成使用 Maven 的**创建设备标识**应用程序，请创建设备标识文件夹中执行在命令提示符下执行以下命令︰

    ```
    mvn clean package -DskipTests
    ```

12. 运行**创建设备标识**应用程序使用 Maven，请在创建设备标识文件夹中执行在命令提示符下执行以下命令︰

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. 记下的**设备 id**和**设备项**。 您将需要这些更高版本创建的应用程序作为设备 IoT 集线器连接时。

> [AZURE.NOTE] 只是 IoT 中心标识注册表存储设备标识能够安全地访问到该中心。 它存储设备 Id 和密钥用作安全凭据和启用/禁用标记可用于禁用单个设备的访问权限。 如果您的应用程序需要将存储设备相关的其他元数据，它应使用的特定于应用程序的存储。 请参阅[IoT 中心开发人员指南][lnk-devguide-identity]的详细信息。

## <a name="receive-device-to-cloud-messages"></a>接收设备到云的消息

在本节中，您将创建 Java 控制台应用程序读取 IoT 集线器设备到云的消息。 IoT 中心公开[事件中心][lnk-event-hubs-overview]-兼容的终结点，以便您能够阅读设备到云的邮件。 为简单起见，本教程创建不适合高吞吐量部署基本读取器。 [处理设备到云消息][lnk-process-d2c-tutorial]教程展示如何处理设备到云在规模较大的消息。 [开始使用事件集线器][lnk-eventhubs-tutorial]教程提供了更多有关如何处理邮件从事件集线器和适用于 IoT 中心事件中心兼容终结点。

> [AZURE.NOTE] 事件中心兼容终结点始终阅读设备到云邮件使用 AMQP 协议。

1. 在*创建设备标识*部分中创建的 iot-java--入门的文件夹中，创建新的 Maven 项目名为**d2c 的读邮件**在您的命令提示符处使用以下命令。 请注意这是一个单一的、 长的命令︰

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. 在您的命令提示符下，导航到新文件夹中读取 d2c 消息。

3. 使用文本编辑器，打开读取 d2c 消息文件夹中 pom.xml 文件和**依赖项**节点添加以下依存关系。 这使您可以在您的应用程序中使用 eventhubs 客户端程序包读取事件中心兼容终结点︰

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. 保存并关闭 pom.xml 文件。

5. 使用文本编辑器，打开 read-d2c-messages\src\main\java\com\mycompany\app\App.java 文件。

6. 向文件中添加下面的**导入**语句︰

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. 将下面的类级变量添加到**应用程序**类。 **{Youriothubkey}**， **{youreventhubcompatibleendpoint}**， **{youreventhubcompatiblename}**替换以前记录的值︰

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. 将下面的**receiveMessages**方法添加到**应用程序**类。 此方法创建一个**EventHubClient**实例连接到事件中心兼容终结点，然后以异步方式创建一个**PartitionReceiver**实例读取事件中心分区。 它不断地循环，直到应用程序终止打印消息详细信息。

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] 此方法使用一个筛选器，以便接收方只读取消息接收方启动运行后发给 IoT 中心创建接收方时。 这是有用的测试环境中，因此您可以看到当前的消息集。 在生产环境中应确保您的代码，它处理所有邮件-[如何处理 IoT 集线器设备到云的消息]，请参阅[lnk-process-d2c-tutorial]教程的详细信息。

9. 修改包含异常，如下所示的**main**方法的签名︰

    ```
    public static void main( String[] args ) throws IOException
    ```

10. 将下面的代码添加到**应用程序**类中的**主要**方法。 此代码创建两个**EventHubClient**和**PartitionReceiver**实例，并使您能够处理消息完成后关闭该应用程序︰

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] 此代码假定在 F1 （免费） 层中创建 IoT 集线器。 免费的 IoT 中心有两个名为"0"和"1"的分区。

11. 保存并关闭 App.java 文件。

12. 若要生成使用 Maven 的**读取 d2c 消息**应用程序，请读取 d2c 消息文件夹中执行在命令提示符下执行以下命令︰

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>创建一个模拟的设备应用程序

在本节中，您将创建一个 Java 控制台应用程序，模拟将设备与云消息发送到一个 IoT 集线器的设备。

1. 在*创建设备标识*部分中创建的 iot-java--入门的文件夹中，创建新的 Maven 项目称为**模拟设备**在您的命令提示符处使用以下命令。 请注意这是一个单一的、 长的命令︰

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. 在您的命令提示符下，导航到新文件夹中模拟设备。

3. 使用文本编辑器，打开模拟设备文件夹中 pom.xml 文件和**依赖项**节点添加以下依存关系。 这使您可以与 IoT 中心通信，并序列化 Java 对象到 JSON 在您的应用程序中使用 iothub 端 java 客户端程序包︰

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. 保存并关闭 pom.xml 文件。

5. 使用文本编辑器，打开 simulated-device\src\main\java\com\mycompany\app\App.java 文件。

6. 向文件中添加下面的**导入**语句︰

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. 将下面的类级变量添加到**应用程序**类，替换 IoT 集线器姓名和在*创建设备的标识*部分中生成的设备关键值**{yourdevicekey}** **{youriothubname}** :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    此示例应用程序使用的**协议**变量时它实例化一个**DeviceClient**对象。 您可以使用 HTTP 或 AMQP 协议与 IoT 中心进行通信。

8. 添加下列嵌套的**TelemetryDataPoint**类来指定您的设备发送到 IoT 集线器的遥测数据的**应用程序**类︰

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. 添加下面的嵌套的**EventCallback**类内显示 IoT 中心返回时处理消息从模拟设备的确认状态的**应用程序**类。 在处理邮件时，此方法还通知中应用程序的主线程︰

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. 添加下面的嵌套的**MessageSender**类**的应用程序**类中。 **Run**方法在此类中的生成示例遥测数据将发送到您 IoT 中枢并等待发送下一封邮件前的确认信息︰

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    此方法将新设备到云消息发送一秒钟，直到 IoT 中心承认上一封邮件。 该消息包含 deviceId 并随机生成的编号来模拟风速度传感器的 JSON 序列化对象。

11. 用下面的代码创建一个线程来发送 IoT 集线器设备到云的替换**main**方法︰

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. 保存并关闭 App.java 文件。

13. 若要生成使用 Maven 的**模拟设备**应用程序，请在模拟设备文件夹中执行在命令提示符下执行以下命令︰

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，您应该实现重试策略 （如指数退避算法），建议在 MSDN 文章︰[瞬时性故障处理][lnk-transient-faults]。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1. 在读取 d2c 文件夹中的命令提示符处，运行以下命令以开始监视 IoT 集线器中的第一个分区︰

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT 中心服务客户端应用程序来监视设备到云的消息][7]

2. 在模拟设备文件夹中的命令提示符处，运行以下命令以开始到 IoT 集线器发送遥测数据︰

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT 集线器设备客户端应用程序发送设备到云消息][8]

3. **使用**拼贴在[Azure 的门户网站][lnk-portal]显示的集线器发送的消息数︰

    ![Azure 门户使用平铺显示发送的消息数 IoT 集线器][43]

## <a name="next-steps"></a>下一步行动

在本教程中，您在 Azure 的门户中，配置一个新 IoT 集线器，然后在中枢的标识注册表中创建一个设备标识。 此设备标识用于启用设备到云的消息发送到中心的模拟的设备应用程序。 您还可以创建显示由集线器接收的消息的应用程序。 

继续入门 IoT 集线器和开拓其他 IoT 情况请参见︰

- [连接您的设备][lnk-connect-device]
- [设备管理入门][lnk-device-management]
- [IoT 网关 sdk 快速入门][lnk-gateway-SDK]

若要了解如何扩展 IoT 解决方案和处理设备到云在规模较大的邮件，请参阅[处理设备到云消息][lnk-process-d2c-tutorial]教程。

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/