<properties
   pageTitle="创建 EAI 的逻辑应用程序逻辑在 Azure 应用程序服务的应用程序中使用 VETR |Microsoft Azure"
   description="验证、 编码和转换的 BizTalk XML 服务功能"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>创建 EAI 的逻辑应用程序使用 VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

大多数企业应用程序集成 (EAI) 情况下的媒介在源和目标之间的数据。 这类方案通常有一组常见的要求︰

- 确保来自不同系统的数据格式正确。
- "查找"对传入的数据做出决定。
- 将数据从一种格式转换到另一个。 例如，将数据从 CRM 系统的数据格式转换为 ERP 系统的数据格式。
- 将数据路由到所需的应用程序或系统。

本文介绍一个通用的集成模式:"单向消息中介"或 VETR （验证，Enrich，转换，工艺路线）。 VETR 模式调节临近范围源实体与目标实体之间的数据。 通常的源和目标是数据源。

考虑接受订单的网站。 用户投递到系统使用 HTTP 的订单。 在后台系统验证传入的数据的正确性、 标准化，并将其保持在服务总线队列中进行进一步处理。 系统将从队列，它应为某一特定格式的订单。 因此，端到端流程为︰

**HTTP** →**验证**→**变形**→**服务总线**

![基本的 VETR 流][1]

以下的 BizTalk API 应用程序可帮助生成此模式︰

* **HTTP 触发器**的触发器消息事件源
* **验证**-验证传入的数据的正确性
* **转换**的转换数据来自下游系统所需的传入格式的格式
* **服务总线连接器**的目标实体发送数据的位置


## <a name="constructing-the-basic-vetr-pattern"></a>建立基本的 VETR 模式
### <a name="the-basics"></a>基础知识

在 Azure 的门户中，选择**+ 新建**、 选择**Web + 移动**，然后选择**逻辑应用程序**。 选择名称、 位置、 订阅、 资源组和起作用的位置。 资源组作为容器的应用程序;所有您的应用程序的资源转到同一个资源组。

接下来，让我们添加触发器和操作。


## <a name="add-http-trigger"></a>添加 HTTP 触发器
1. 在**逻辑应用程序模板**中，选择**从零开始创建**。
1. 从库来创建一个新的侦听器选择**HTTP 侦听程序**。 **HTTP1**调用它。
2. 设置**自动发送响应？**设置为 false。 配置设置开机_自检_的_HTTP 方法_和_/OneWayPipeline__相对 URL_设置触发器操作︰  
    ![HTTP 触发器][2]
3. 选择绿色的复选标记来完成该触发器。

## <a name="add-validate-action"></a>添加验证操作

现在，让我们输入运行时触发触发器的操作 — — 也就是说，每当 HTTP 终结点上收到呼叫。

1. 从库中添加**BizTalk XML 验证器**并将其命名为_(Validate1)_来创建的实例。
2. 配置 XSD 架构验证传入的 XML 消息。 选择_验证_操作并选择_triggers('httplistener').outputs。内容_作为_inputXml_参数的值。

现在，验证操作将 HTTP 侦听程序后的第一个操作︰ 

![BizTalk XML 验证器][3]

同样，让我们添加操作的其余部分。 

## <a name="add-transform-action"></a>添加转换操作
让我们来配置传入数据进行正态化转换。

1. 从库中添加**BizTalk 转换服务**。
2. 若要配置转换来转换传入的 XML 消息，选择作为此 API 调用时执行的操作的**转换**操作。 选择```triggers(‘httplistener’).outputs.Content```作为_inputXml_的值。 *地图*是一个可选参数，因为传入的数据相匹配的所有已配置的转换，并将应用仅显示那些与架构相匹配。
3. 最后，验证成功时才会运行转换。 要配置此条件，请选择齿轮图标在右上角，并选择_添加条件得以满足_。 将条件设置为```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk 转换][4]


## <a name="add-service-bus-connector"></a>添加服务总线连接器
接下来，让我们添加目标 — — 服务总线队列 — — 数据写入。

1. 从库中添加**服务总线连接器**。 将**名称**设置为_Servicebus1_、 到您的服务总线实例的连接字符串中设置**连接字符串**、 设置为_队列_中的**实体名称**和跳过**订阅名称**。
2. 选择**发送消息**的操作并设置为_actions('transformservice').outputs 的**内容**字段的操作。OutputXml_。
3. 将**内容类型**字段设置为*应用程序/xml*:  

![服务总线][5]


## <a name="send-http-response"></a>发送的 HTTP 响应
完成管道处理后，重新发送 HTTP 响应的成功和失败与以下步骤︰

1. 从库中添加**HTTP 侦听程序**并选择**发送 HTTP 响应**操作。
2. 设置**响应 ID**发送*邮件*。
2. 将设置**响应内容**为*管道处理完成*。
3. **响应状态代码**为*200*来指示 HTTP 200 确定。
4. 选择在右上角，下拉菜单并选择**添加条件得以满足**。  将条件设置为以下表达式︰  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. 重复上述步骤以失败以及发送的 HTTP 响应。 将**条件**更改为下面的表达式︰  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. 选择**确定**，然后**创建**。



## <a name="completion"></a>完成
每次有人向 HTTP 端点发送消息，它将触发应用程序并执行您刚刚创建的操作。 管理此类逻辑的应用程序创建、 在 Azure 门户中，选择**浏览**并选择**逻辑应用程序**。 选择您的应用程序的详细信息，请参阅。

某些帮助主题︰

[管理和监视您的 API 的应用程序，连接器](app-service-logic-monitor-your-connectors.md)  <br/>
[监视您的逻辑应用程序](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
