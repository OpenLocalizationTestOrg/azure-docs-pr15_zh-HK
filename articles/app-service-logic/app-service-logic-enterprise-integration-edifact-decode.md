<properties 
    pageTitle="了解企业集成包解码 EDIFACT 消息连接器 |Microsoft Azure 应用程序服务 |Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>开始使用解码 EDIFACT 消息

验证 EDI 和特定于合作伙伴的属性，生成每个事务组的 XML 文档并生成处理事务的确认。

## <a name="create-the-connection"></a>创建连接

### <a name="prerequisites"></a>系统必备组件

* Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)

* 使用解码 EDIFACT 消息连接器需要集成帐户。 有关如何创建[集成客户](./app-service-logic-enterprise-integration-create-integration-account.md)、[合作伙伴](./app-service-logic-enterprise-integration-partners.md)和[EDIFACT 协议](./app-service-logic-enterprise-integration-edifact.md)请参阅详细信息

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>连接到解码 EDIFACT 邮件使用以下步骤︰

1. [创建一个逻辑应用程序](./app-service-logic-create-a-logic-app.md)提供了一个示例。

2. 此接口不具有任何触发器。 使用其他触发器来启动逻辑的应用程序，如请求触发器。  在逻辑应用程序设计器中，添加触发器并添加一个动作。  选择显示 Microsoft 托管的 Api，在下拉列表中列出，然后在搜索框中输入"EDIFACT"。  选择解码 EDIFACT 消息

    ![搜索 EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. 如果您以前没有创建任何连接到集成的帐户，您的连接的详细信息

    ![创建集成帐户](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. 输入集成帐户详细信息。  带星号的属性是必需的

  	| 属性 | 详细信息 |
  	| -------- | ------- |
  	| 连接名称 * | 输入您的连接的任何名称 |
  	| 集成帐户 * | 输入集成帐户名称。 确保您的集成帐户和应用程序逻辑都是在 Azure 所在 |

    完成后，您的连接详细信息看起来类似于以下

    ![创建集成帐户](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. 选择**创建**

6. 注意到已创建的连接

    ![集成的帐户连接详细信息](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. 选择 EDIFACT 为平面文件消息进行解码

    ![提供了必填字段](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT 解码 does 下

* 通过匹配的发件人限定符与标识符以及接收器限定符与标识符解析协议
* 拆分在单个邮件中入单独的多个交换。
* 验证对贸易伙伴协议的信封
* 分解的交换。
* 验证 EDI 和特定于合作伙伴的属性包括
    * 验证结构的交换信封。
    * 针对控制架构的信封架构验证。
    * 根据消息架构的事务集数据元素的架构验证。
    * EDI 验证交易记录集数据元素上执行
* 验证交换、 组和事务组控制号不重复 （是否配置） 
    * 检查对照先前收到交换的交换控制编号。 
    * 检查组控件数与交换中的其他组控制数量。 
    * 检查交易记录对该组中其他事务组控制号设置控件数。
* 生成 XML 文档为每个交易记录集。
* 将整个交换转换为 XML 
    * 剥离互换为交易记录设置的错误上挂起事务集︰ 分析设置交换到单独的 XML 文档中的每个交易记录。 如果交换中的一个或多个事务集未通过验证，然后 EDIFACT 解码中止仅这些事务集。 
    * 剥离互换为交易记录设置的错误暂停交换︰ 分析设置交换到单独的 XML 文档中的每个交易记录。  如果一个或多个交易记录的设置交换中未通过验证，然后 EDIFACT 解码会挂起整个交换。
    * 保留的交换-在错误上挂起事务集︰ 创建用于在整个批交换 XML 文档。 EDIFACT 解码挂起未通过验证，而同时可以继续处理所有其他交易记录集这些交易记录集
    * 保留交换-挂起错误交换︰ 创建用于在整个批交换 XML 文档。 如果交换中的一个或多个事务集未通过验证，然后 EDIFACT 解码会挂起整个交换， 
* （如果配置） 生成技术 （控制） 和/或功能确认。
    * 技术确认或控制器二维 ACK 报告完成接收交换语法检查的结果。
    * 功能确认承认接受或拒绝接收的交换或组

## <a name="next-steps"></a>下一步行动

[了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包") 