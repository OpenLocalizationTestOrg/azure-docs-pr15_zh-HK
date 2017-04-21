> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [窗口](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

这篇文章提供的[Hello World 示例代码]的详细的演练[lnk-helloworld-sample]来说明[Azure IoT 网关 SDK]的基本组件[lnk-gateway-sdk]的体系结构。 此示例使用 IoT 中心网关 SDK 来生成简单的网关的"你好世界"消息记录到文件每隔五秒。

本演练介绍︰

- **概念**︰ 撰写与 IoT 网关 SDK 创建任何网关组件的概念性概述。  
- **Hello World 示例体系结构**︰ 介绍的概念适用于 Hello World 示例的方式以及这些组件如何协同。
- **如何生成示例**︰ 构建该示例所需的步骤。
- **如何运行该示例**︰ 若要运行该示例所需的步骤。 
- **典型输出**︰ 希望运行此示例的输出的示例。
- **代码段**︰ 代码段可以显示 Hello World 示例如何实现关键的网关组件的集合。

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT 网关 SDK 概念

检查示例代码或创建自己域的网关使用 IoT 网关 SDK 之前，您应该了解支持的 SDK 的体系结构的关键概念。

### <a name="modules"></a>模块

也可以通过创建和装配*模块*生成 Azure IoT 网关 SDK 的网关。 模块使用*邮件*交换彼此的数据。 模块收到一条消息、 对其执行某些操作，或者将其转换为新邮件中，和再发布它的其他模块，来处理。 某些模块可能只生成新的消息，并永远不会处理传入消息。 链模块创建的每个模块一个点，该管道中的数据进行变换数据处理管道。

![中网关使用 Azure IoT 网关 SDK 生成模块的一串][1]
 
SDK 包含以下项目︰

- 预先编写的模块执行的通用网关功能。
- 开发人员可以使用它来编写自定义模块接口。
- 部署和运行一套模块所需的基础结构。

SDK 提供了一个抽象层，使您能够构建网关在不同的操作系统和平台上运行。

![Azure IoT 网关 SDK 抽象层][2]

### <a name="messages"></a>消息

尽管思考一下模块传递彼此的邮件是概念化网关工作方式一种简便方法，它不准确地反映会发生什么情况。 模块使用一个代理来相互通信，它们将消息发布到该中介 （总线、 pubsub 或任何其他消息传递模式），然后让代理程序将消息发送给连接到该模块。

模块使用**Broker_Publish**函数将消息发布到该中介。 代理程序通过调用的回调函数将消息传送到一个模块。 一条消息，包含一组键/值属性和内容作为内存块。

![Azure IoT 网关 SDK 中经纪人的角色][3]

### <a name="message-routing-and-filtering"></a>消息路由和筛选

有两种方法可以将定向到正确的模块的消息。 一组链接可以传递到中介，使中介知道源和接收器的每一个模块，或者可以根据消息的属性进行过滤模块。 模块应该只采取一条消息，如果消息用于。 链接和邮件筛选是什么有效地创建消息管道。

## <a name="hello-world-sample-architecture"></a>大家好世界的示例体系结构

Hello World 示例说明了在上一节中描述的概念。 Hello World 示例实现了两个模块组成的管道都有一个网关︰

-   *你好世界*模块创建一条消息，每隔五秒，并将其传递给记录器模块。
-   *记录器*模块将接收的消息写入的文件。

![Hello World 示例使用 Azure IoT 网关 SDK 构建的体系结构][4]

如前一节中所述，Hello World 模块未通过邮件直接到记录器模块每隔五秒。 相反，它可以发布一条消息到中介每隔五秒。

记录器模块从代理接收消息并作用于它，消息的内容写入一个文件。

记录器模块只消耗来自中介的消息，永远不会将新消息发布到该中介。

![中介将 Azure IoT 网关 SDK 中的模块之间的消息的路由][5]

上图显示 Hello World 示例和实现[存储库]中的示例的不同部分的源代码文件的相对路径的体系结构[lnk-gateway-sdk]。 浏览代码依靠您自己，或使用下面的代码段，作为参考。

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk