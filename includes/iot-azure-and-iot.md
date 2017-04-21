
# <a name="azure-and-internet-of-things"></a>Azure 和物联网

欢迎使用 Microsoft Azure 和物 (IoT) 联网。 本文介绍 IoT 解决方案体系结构的描述可能会部署使用 Azure 服务 IoT 解决方案的一些共性。 IoT 解决方案需要保护，可能数以百万计和后端解决方案中编号的设备之间的双向通信。 例如，解决方案后端可以使用自动化的预测性分析发现从设备到云事件流的见解。

当您实现此 IoT 解决方案体系结构的使用 Azure 服务时，azure IoT 集线器将是关键的构造块。 IoT 套件提供了完整、 端到端实现这种体系结构特定 IoT 方案。 例如︰ 

- *远程监控*解决方案使您能够监视设备，如自动贩卖机的状态。 
- *预测性维护*解决方案可帮助您预测维护的设备，如泵泵送的远程工作站的需求并避免计划外的停机时间。

## <a name="iot-solution-architecture"></a>IoT 解决方案体系结构

下图显示了典型的 IoT 解决方案体系结构。 图中不包括任何特定的 Azure 服务名称，但描述泛型 IoT 解决方案体系结构中的关键元素。 在此体系结构中，IoT 设备收集它们向云网关发送的数据。 云关使数据可用于处理其他后端服务使用从其他业务线应用程序或操作员通过仪表板或其他演示文稿设备的数据传送到的位置。

![IoT 解决方案体系结构][img-solution-architecture]

> [AZURE.NOTE] 有关 IoT 体系结构的深入讨论，请参阅[Microsoft Azure IoT 参考体系结构][lnk-refarch]。

### <a name="device-connectivity"></a>设备连接

在此 IoT 解决方案体系结构、 设备发送遥测，如传感器读数从泵送站，云端点的存储和处理。 在预防性维护方案后, 端可能使用的传感器数据的流来确定特定泵时需要维护。 设备可以接收和响应云至设备命令通过云端点读取消息。 例如，在预防性维护方案解决方案后端可能会将命令发送到其他泵泵送站开始前维护的截止日期开始，以确保当她到达时，维护工程师可以开始重新路由流中。

面对 IoT 项目的最大挑战之一是如何安全可靠地将设备连接到后端的解决方案。 IoT 设备具有不同的特征，而其他客户端浏览器和移动应用程序等。 IoT 设备︰

- 往往与没有人工接线员的嵌入式的系统。
- 可以部署在远程位置，其中的物理访问是昂贵。
- 只能通过解决方案后端到达。 没有其他办法可以与设备交互。
- 可能拥有有限的能力和处理资源。
- 可能有间歇性、 速度很慢，或昂贵的网络连接。
- 可能需要使用专有的、 自定义的或行业特定的应用程序协议。
- 可以创建使用大量流行的硬件和软件平台。

除了上述的要求，任何 IoT 解决方案还必须提供规模、 安全性和可靠性。 结果集的连接要求非常硬且耗时实现使用传统技术，如 web 容器和消息经纪人。 Azure IoT 集散 IoT 设备 Sdk 使易于实施解决方案，以满足这些要求。

如果该设备不能使用任何云网关支持的通信协议，它可以通过中间网关连接或设备可以用云关终结点，直接进行通信。 例如， [Azure IoT 协议网关][lnk-protocol-gateway]可以执行协议转换，如果设备不能使用任何 IoT 集线器支持的协议。

### <a name="data-processing-and-analytics"></a>数据处理和分析

在云中，回到解决方案结束 IoT 是大部分数据处理出现的位置，如过滤和聚合遥测和传送给其他的服务。 IoT 解决方案后结束︰

- 从您的设备接收遥测在规模较大，决定如何处理和存储这些数据。 
- 可能使您能够从云中向特定设备发送命令。
- 提供了到供应设备和控制设备都可以连接到您的基础结构使您的设备注册功能。
- 使您可以跟踪您的设备的状态并监视他们的活动。

在预防性维护方案，解决方案后端存储历史的遥测数据。 后端可以使用此数据来标识图案表明维护已截止于一个特定的泵。

IoT 解决方案可以包括自动反馈循环。 例如，在后台分析模块可以识别特定设备的温度是正常运营水平以上的遥测。 解决方案可以将发送命令给设备，指导其采取纠正措施。

### <a name="presentation-and-business-connectivity"></a>演示文稿和业务连接性

演示文稿和业务连接性层使最终用户与 IoT 解决方案和设备进行交互。 它使用户可以查看和分析从他们的设备收集的数据。 这些视图可以采取的形式为仪表板或 BI 报表可显示这两个历史数据或实时数据接近。 例如，操作员可以查看特定的泵送站的状态并查看任何系统发出的警报。 这一层还允许 IoT 解决方案后端与现有的业务线应用程序绑定到企业业务流程或工作流的集成。 例如，预测性维护解决方案可以预定访问时解决方案标识需要维护一个泵的泵送站工程的调度系统集成。

![IoT 解决方案仪表板][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
