<properties 
    pageTitle="了解企业集成包编码 X12 消息 Connctor |Microsoft Azure 应用程序服务 |Microsoft Azure" 
    description="了解如何使用与企业集成包和逻辑的应用程序合作伙伴" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-encode-x12-message"></a>开始使用 X12 编码消息

验证 EDI 和特定于合作伙伴的属性、 将 XML 编码的消息转换成在互换 EDI 事务集并请求技术和/或功能确认

## <a name="create-the-connection"></a>创建连接

### <a name="prerequisites"></a>系统必备组件

* Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)

* 使用编码 x12 消息连接器需要集成帐户。 详细信息查看有关如何创建[集成客户](./app-service-logic-enterprise-integration-create-integration-account.md)、[合作伙伴](./app-service-logic-enterprise-integration-partners.md)和[X12 协议](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>连接到编码 X12 邮件使用以下步骤︰

1. [创建一个逻辑应用程序](./app-service-logic-create-a-logic-app.md)提供了一个示例

2. 此接口不具有任何触发器。 使用其他触发器来启动逻辑的应用程序，如请求触发器。  在逻辑应用程序设计器中，添加触发器并添加一个动作。  在搜索框中选择显示 Microsoft 托管的 Api 中下拉列表，然后输入"x12"。  选择任一 X12 的编码 X12 消息按协议名称或 X12-通过标识编码为 x12 的消息。  

    ![搜索 x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. 如果您以前没有创建任何连接到集成的帐户，您的连接的详细信息

    ![集成的帐户连接](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. 输入集成帐户详细信息。  带星号的属性是必需的

  	| 属性 | 详细信息 |
  	| -------- | ------- |
  	| 连接名称 * | 输入您的连接的任何名称 |
  	| 集成帐户 * | 输入集成帐户名称。 确保您的集成帐户和应用程序逻辑都是在 Azure 所在 |

    完成后，您的连接详细信息看起来类似于以下

    ![创建集成帐户连接](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. 选择**创建**

6. 注意到已创建的连接。

    ![集成的帐户连接详细信息](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12 的编码 X12 消息按协议名称

7. 选择 X12 协议中下拉列表和 xml 消息进行编码。

    ![提供了必填字段](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12 的编码 X12 消息的标识

7.  提供发件人标识符、 限定符发件人、 收件人标识符和接收器限定符，如 X12 中配置协议。  选择要进行编码的 xml 消息

    ![提供了必填字段](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 编码后的那样︰

* 通过匹配的发件人和收件人的上下文属性的协议解决。
* 序列化的 EDI 交换，将 XML 编码的消息转换成在互换 EDI 事务集。
* 应用事务组标头和尾端段
* 生成交换控制编号、 一组控制数和每个传出交换的事务集中控制编号
* 有效负载数据替换分隔符
* 验证 EDI 和特定于合作伙伴的属性
    * 对该邮件架构的事务集数据元素的架构验证
    * EDI 验证执行事务组数据元素。
    * 扩展的验证交易记录集数据元素上执行
* （如果配置），则请求技术和/或功能确认。
    * 由于标头验证生成技术确认。 技术确认报告交换标头和尾端地址接收方处理的状态
    * 由于主体验证生成功能确认。 功能确认报告每个处理接收到的文档时出现的错误

## <a name="next-steps"></a>下一步行动

[了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包") 

