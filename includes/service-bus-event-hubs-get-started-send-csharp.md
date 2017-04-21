## <a name="send-messages-to-event-hubs"></a>将消息发送到事件集线器

在本节中，您将编写 Windows 控制台应用程序将事件发送到事件中心。

1. 在 Visual Studio 中，创建一个新的视觉 C# 桌面应用程序项目使用**控制台应用程序**项目模板。 **发件人**为项目命名。

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. 在解决方案资源管理器，该解决方案中，用鼠标右键单击，然后单击**解决方案管理 NuGet 程序包**。 

3. 单击**浏览**选项卡，然后搜索`Microsoft Azure Service Bus`。 确保在**版本**框中指定的项目名称 （**发件人**）。 单击**安装**，并接受使用条款。 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio 的下载、 安装，并将引用添加到[Azure 服务总线库 NuGet 程序包](https://www.nuget.org/packages/WindowsAzure.ServiceBus)。

4. 添加以下`using` **Program.cs**文件顶部的语句︰

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. 将以下字段添加到**程序**类，替换占位符值与您在上一节中创建事件中心和以前保存的命名空间级连接字符串的名称。

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. 将下面的方法添加到**程序**类︰

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    此方法不断地将事件发送到 200 毫秒延迟事件网络集线器。

7. 最后，将以下行添加到**Main**方法︰

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
