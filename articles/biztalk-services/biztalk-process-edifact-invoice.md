<properties
   pageTitle="教程︰ 处理 EDIFACT 发票使用 Azure BizTalk 服务 |Microsoft Azure BizTalk 服务"
   description="如何创建和配置框中接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>教程︰ 使用 Azure BizTalk 服务进程 EDIFACT 发票
可以使用 BizTalk 服务门户配置和部署 X12 和 EDIFACT 协议。 在本教程中，我们了解如何创建了 EDIFACT 协议，用它交换发票之间的贸易伙伴。 本教程编写围绕涉及两个贸易伙伴，罗斯和 Contoso 交换 EDIFACT 消息的端到端业务解决方案。  

## <a name="sample-based-on-this-tutorial"></a>在本教程中的示例
本教程编写周围的示例中，**发送 EDIFACT 发票使用 BizTalk 服务**，这是可以从[MSDN 代码库](http://go.microsoft.com/fwlink/?LinkId=401005)下载。 可以使用该示例，并通过本教程中了解如何生成示例。 或者，您可以使用本教程来创建您自己的解决方案地面向上。 本教程被面向第二种方法，以便您了解此解决方案的构建方式。 同时，尽最大可能，教程是与样本一致，所使用的示例中使用相同名称的项目 （例如，架构的转换）。  

>[AZURE.NOTE] 因为此解决方案涉及到从现有 EAI 桥将消息发送到 EDI 桥，它将重用该[BizTalk 服务桥链示例](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104)的示例。  

## <a name="what-does-the-solution-do"></a>解决方案是什么？

在此解决方案中，罗斯从 Contoso 接收 EDIFACT 发票。 这些发票不是标准的 EDI 格式。 因此，将发票发送给罗斯之前, 它必须转换为 EDIFACT （也称为 INVOIC） 的发票文档。 收据上, 罗斯必须处理 EDIFACT 张发票，并返回到 Contoso 的控制邮件 （也称为控制器三维）。

![][1]  

为了实现这个业务方案，Contoso 将使用与 Microsoft Azure BizTalk 服务提供的功能。

*   Contoso 使用 EAI 桥原始发票转换为 EDIFACT INVOIC。

*   EAI 桥然后到 EDI 发送桥协议配置 BizTalk 服务门户中的一部分发送的消息。

*   EDI 发送桥接器处理 EDIFACT INVOIC，并将其路由到罗斯文。

*   在收到发票，罗斯返回控制器二维消息到 EDI 接收桥部署作为协议的一部分。  

> [AZURE.NOTE] （可选），该解决方案还演示了如何使用批处理来发送批处理，而不是分别发送每个发票中的发票。  

若要完成该方案，我们使用服务总线队列将从 Contoso 的发票发送到罗斯文或罗斯文中得到确认。 可以使用客户端应用程序，可以下载并在可用示例包中包含创建这些队列作为本教程的一部分。  

## <a name="prerequisites"></a>系统必备组件

*   您必须拥有一个服务总线的命名空间。 创建命名空间的说明，请参阅[How To︰ 创建或修改服务总线服务 Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx)。 让我们假设您已经设置，服务总线命名空间称为**edifactbts**。

*   您必须具有一个 BizTalk 服务订阅。 有关说明，请参阅[使用 Azure 的传统门户网站创建 BizTalk 服务](http://go.microsoft.com/fwlink/?LinkID=302280)。 对于本教程，让我们假设您具有名为**contosowabs**的 BizTalk 服务订阅。

*   注册您在 BizTalk 服务门户上的 BizTalk 服务订阅。 有关说明，请参阅[注册 BizTalk 服务部署在 BizTalk 服务门户上](https://msdn.microsoft.com/library/hh689837.aspx)

*   您必须安装的 Visual Studio。

*   您必须安装 BizTalk 服务 SDK。 您可以从[http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)下载 SDK  

## <a name="step-1-create-the-service-bus-queues"></a>步骤 1︰ 创建服务总线队列  
本解决方案使用服务总线队列以贸易伙伴之间交换消息。 Contoso 和罗斯从 EAI 和/或 EDI 桥位置使用它们向队列发送消息。 对于此解决方案，您将需要三个服务总线队列︰

*   **northwindreceive** – Northwind 发票宏远公司通过接收此队列。

*   **contosoreceive** – Contoso 收到确认从罗斯对此队列。

*   所有**挂起**– 挂起邮件将发送到此队列。 如果在处理过程中发生了故障，都被挂起的消息。

可以通过使用示例包中包含的客户端应用程序来创建这些服务总线队列。  

1.  从您下载示例的位置，打开**教程发送发票使用 BizTalk 服务 EDI Bridges.sln**。

2.  按**f5 键**以生成并启动**教程客户端**应用程序。

3.  在屏幕中，输入服务总线 ACS 命名空间、 颁发者名称和颁发者的密钥。

    ![][2]  
4.  一个消息框提示您服务总线命名空间中，将创建三个队列。 单击**确定**。

5.  将教程客户端运行。 打开，单击**服务总线** > **_服务总线名称空间_** > **队列**，并验证所创建的三个队列。  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>步骤 2︰ 创建和部署贸易伙伴协议
创建 Contoso 和罗斯文贸易伙伴协议。 贸易伙伴协议定义了两个商业伙伴，如要使用，请使用等的消息传递协议的消息架构之间的贸易协定。贸易伙伴协议包括两个 EDI 桥，一个将消息发送到 （称为**EDI 发送桥**） 的贸易伙伴，一个从贸易伙伴 （称为**EDI 接收桥**） 接收消息。

在此解决方案的上下文中，EDI 发送桥对应于该协议在发送端，用于从 Contoso EDIFACT 发票发送给罗斯。 同样，EDI 接收桥对应于该协议接收端，用于接收确认从罗斯文。  

### <a name="create-the-trading-partners"></a>创建贸易伙伴

Contoso 和罗斯，创建的贸易伙伴。  

1.  在 BizTalk 服务门户中，**伙伴**选项卡上，单击**添加**。

2.  在新的伙伴页中，输入作为合作伙伴名称， **Contoso** ，然后单击**保存**。

3.  重复该步骤以创建第二个合作伙伴，**罗斯文**。  

### <a name="create-the-agreement"></a>创建协议
贸易合作伙伴的业务配置文件之间创建贸易伙伴协议。 本解决方案使用我们创建合作伙伴时自动创建的默认伙伴配置文件。  

1.  在 BizTalk 服务门户中，单击**协议** > **添加**。

2.  在新协议的**一般设置**页面，如图所示，指定的值，然后单击**继续**。

    ![][3]  

    单击**继续**后，两个选项卡，**设置接收**和**发送设置**才可用。

3.  创建 Contoso 和罗斯的发送协议。 本协议管辖 Contoso 将 EDIFACT 发票发送到罗斯文的方式。

    1.  单击**设置条件下发送**。

    2.  保留在**入站 URL**、**转换**和**批处理**选项卡上的默认值。

    3.  在**协议**选项卡的**架构**部分上, 传的**EFACT_D93A_INVOIC.xsd**架构。 此架构是可用示例包。

        ![][4]  
    4.  在**运输**选项卡上指定服务总线队列的详细的信息。 发送端协议，我们将使用**northwindreceive**队列将 EDIFACT 发票发送到罗斯文和**暂挂**队列路由任何邮件处理过程失败，都将被挂起。 创建这些队列中的**步骤 1︰ 创建服务总线队列**（在此主题）。

        ![][5]  

        在**传输设置 > 传输类型**和**消息挂起设置 > 传输类型**，选择 Azure 服务总线，提供的值，如图所示。

4.  创建 Contoso 和罗斯的接收协议。 本协议管辖 Contoso 罗斯从接收确认消息的方式。

    1.  单击**接收设置**。

    2.  保留**运输**和**转换**选项卡上的默认值。

    3.  在**协议**选项卡的**架构**部分上, 传的**EFACT_4.1_CONTRL.xsd**架构。 此架构是可用示例包。

    4.  在**路由**选项卡上创建一个筛选以确保只从罗斯文的确认送交 Contoso。 在**传送设置**单击**添加**以创建路由筛选器。

        ![][6]  
        1.  **规则名称**、**路由规则**，和**路由目标**提供值，如图所示。

        2.  单击**保存**。

    5.  在**路由**选项卡上，指定暂停的确认 （确认故障处理过程） 送交。 设置到 Azure 服务总线传输类型、 路由到**队列**中，身份验证类型的目标类型**共享访问签名**(SAS)，服务总线的命名空间，提供的 SAS 连接字符串和**挂起**然后输入的队列名称。

5.  最后，单击**部署**来部署协议。 请注意端点位置发送和接收协议完成部署。

    *   在**发送设置**选项卡上的**入站 URL**，请注意该终结点。 从 Contoso 向罗斯使用 EDI 发送桥将一条消息，必须到此终结点发送一条消息。

    *   在**接收设置**选项卡上的**传输**，请注意该终结点。 若要从罗斯文的消息发送到 Contoso 使用 EDI 接收桥，则必须对该终结点发送一条消息。  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>步骤 3︰ 创建和部署 BizTalk 服务项目

在前面的步骤中，您将部署 EDI 发送和接收协议处理 EDIFACT 发票和确认。 这些协议可以只处理符合标准 EDIFACT 消息架构的消息。 不过，此解决方案的情形，每 Contoso 发送发票给罗斯公司内部的专有架构中。 因此，消息发送到 EDI 发送桥之前，它必须能转换从内部架构为标准 EDIFACT 发票架构。 BizTalk 服务 EAI 项目会这样做。

该 BizTalk 服务项目中， **InvoiceProcessingBridge**的消息转换，也是示例的您下载的一部分。 该项目包括以下项目︰

*   **INHOUSEINVOICE。XSD** – 内部发票发送到罗斯文数据库的架构。

*   **EFACT_D93A_INVOIC。XSD** – 标准 EDIFACT 发票的架构。

*   **EFACT_4.1_CONTRL。XSD** -罗斯将发送给 Contoso EDIFACT 确认的架构。

*   **INHOUSEINVOICE_to_D93AINVOIC。TRFM** – 将内部发票架构映射到标准 EDIFACT 发票架构的转换。  

### <a name="create-the-biztalk-services-project"></a>创建的 BizTalk 服务项目
1.  在 Visual Studio 解决方案中，展开 InvoiceProcessingBridge 项目，然后打开**MessageFlowItinerary.bcs**文件。

2.  在画布上的任意位置单击并将**BizTalk 服务 URL**设置在属性框中，指定您的 BizTalk 服务订阅名称。 例如， `https://contosowabs.biztalk.windows.net`。

    ![][7]  
3.  从工具箱拖动现有**Xml 单向桥**到画布上。 将桥的**实体名称**和**相对地址**属性设置为**ProcessInvoiceBridge**。 双击**ProcessInvoiceBridge**打开桥接配置图面。

4.  在**消息类型**框中，单击加号 (**+**) 按钮以指定传入消息的架构。 由于 EAI 桥的传入消息始终是公司内部的发票，请将此设置改为**INHOUSEINVOICE**。

    ![][8]  
5.  单击**Xml 转换**形状，然后在**映射**属性的属性框中单击省略号 （**...**）。 在**映射选项**对话框中，选择**INHOUSEINVOICE_to_D93AINVOIC**的转换文件，然后单击**确定**。

    ![][9]  
6.  返回到**MessageFlowItinerary.bcs**，并从工具箱，将**双向外部服务终结点**拖到**ProcessInvoiceBridge**的右侧。 **实体名称**属性设置为**EDIBridge**。

7.  在解决方案资源管理器中，展开**MessageFlowItinerary.bcs** ，然后双击**EDIBridge.config**文件。 **EDIBridge.config**的内容替换为以下。

    > [AZURE.NOTE] 为什么需要编辑.config 文件？ 我们向桥设计器画布添加外部服务终结点表示我们前面部署 EDI 桥。 EDI 桥是双向的桥梁，具有发送和接收端。 但是，我们添加到桥设计的 EAI 桥是单向桥。 因此，若要处理不同的消息交换模式的两个桥接器，我们使用自定义桥行为按在.config 文件中包括其配置。 此外，自定义行为还处理对 EDI 发送桥终结点进行身份验证。此自定义行为是可作为独立样本在[BizTalk 服务桥链示例-对 EDI 的 EAI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104)。 此解决方案将重新使用此示例。  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  更新 EDIBridge.config 文件以包括配置详细信息

    *   在下， _<behaviors>_，提供 ACS 命名空间并与 BizTalk 服务订阅关联的键。

    *   在下， _<client>_，提供 EDI 发送协议部署位置的终结点。

    保存更改并关闭该配置文件。

9.  从工具箱，单击**连接器**，加入了**ProcessInvoiceBridge**和**EDIBridge**的组件。 选择连接线，然后在属性框中，设置为**匹配所有****筛选条件**。 这将确保通过 EAI 桥接器处理的所有消息被都路由到 EDI 桥。

    ![][10]  
10.  将更改保存到解决方案中。  

### <a name="deploy-the-project"></a>部署项目

1.  您在其中创建的 BizTalk 服务项目的计算机上下载并安装 BizTalk 服务订阅的 SSL 证书。 从，在 BizTalk 服务下，**仪表板**，请单击，然后单击**下载 SSL 证书**。 双击该证书，然后按照提示完成安装。 请确保您安装证书在**受信任的根证书颁发机构**证书存储下。

2.  在 Visual Studio 解决方案资源管理器，右键单击**InvoiceProcessingBridge**项目，然后单击**部署**。

3.  如所示，在图中，提供的值，然后单击**部署**。 通过单击 BizTalk 服务面板中的**连接信息**，可以 BizTalk 服务获得 ACS 凭据。

    ![][11]  

    从输出窗格复制终结点部署 EAI 桥是位置，例如， `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`。 稍后，您将需要此终结点的 URL。  

## <a name="step-4-test-the-solution"></a>步骤 4︰ 测试该解决方案


在本主题中，我们了解如何使用作为示例的一部分提供的**教程客户端**应用程序中测试该解决方案。  

1.  在 Visual Studio 中，按下 F5 键启动**教程客户端**。

2.  屏幕必须有步中预填充的值创建的服务总线队列的位置。 单击**下一步**。

3.  在下一个窗口，提供为 BizTalk 服务订阅的 ACS 凭据和终结点位置 EAI 和 EDI （接收） 部署的桥梁。

    您已在上一步中复制 EAI 桥终结点。 对于 EDI 接收桥终结点，在 BizTalk 服务门户，请转到协议 > 接收设置 > 传输 > 终结点。

    ![][12]  
4.  在下一个窗口中，在 Contoso，单击**发送内部发票**按钮。 在文件打开对话框，打开 INHOUSEINVOICE.txt 文件。 检查文件的内容，然后单击**确定**以发送发票。

    ![][13]  
5.  几秒钟后罗斯在收到发票。 单击**查看邮件**链接以查看发票接收的罗斯。 注意到罗斯通过接收的发票在 contoso 发送了一个内部架构标准 EDIFACT 架构中的是。

    ![][14]  
6.  选择的发票，然后单击**发送确认**。 在弹出对话框中，请注意交换 ID 是在收到的发票并发送确认相同。 在**发送确认**对话框中单击确定。

    ![][15]  
7.  几秒钟后，contoso 成功接收确认消息。

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>步骤 5 （可选）︰ 分批发送 EDIFACT 发票 
BizTalk 服务 EDI 桥还支持批处理传出的消息。 此功能可用于接收愿意接收一批而不是单个邮件消息 （满足某些条件） 的合作伙伴。

使用批处理时，最重要的方面是批处理，也称为发布标准的实际版本。 发布标准可以基于接收的伙伴希望接收邮件的方式。 如果启用批处理，EDI 桥不发送传出消息到接收的伙伴直到发布标准履行。 例如，批处理条件基于邮件大小派单批仅当 n 的邮件，您需要等待。 批次条件也可以是基于时间，以便在每天固定时间发送一批。 在此解决方案中，我们尝试的消息大小基于的条件。

1.  在 BizTalk 服务门户中，单击您在前面创建的协议。 单击发送设置 > 批处理 > 添加批处理。

2.  对于批次名称输入**InvoiceBatch**，提供说明，，然后单击**下一步**。

3.  指定批处理条件，用于定义哪些消息必须进行批处理。 在此解决方案中，我们的批的所有消息。 因此，选择使用高级定义选项，并输入**1 = 1**。 这是将始终为 true 的条件，因此批量处理的所有消息。 单击**下一步**。

    ![][17]  
4.  指定批次发行条件。 从下拉框中，选择**MessageCountBased**，并**计数**，指定**3**。 这意味着一批的这三封邮件将被发送到罗斯文。 单击**下一步**。

    ![][18]  
5.  查看摘要，然后单击**保存**。 单击**部署**重新部署协议。

6.  返回到**教程客户端**单击**发送内部发票**，按照提示发送发票。 您会注意到因为不满足批处理大小，则在罗斯文接收没有发票。 重复此步骤两次，以便您有三个发票邮件发送到罗斯文。 这满足邮件 3 批发布标准，您应看到罗斯在发票。


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

