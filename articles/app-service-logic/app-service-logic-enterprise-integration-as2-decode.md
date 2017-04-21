<properties 
    pageTitle="了解企业集成包解码 AS2 消息 Connctor |Microsoft Azure 应用程序服务 |Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>开始使用解码 AS2 消息

连接到解码 AS2 消息在传输消息时建立的安全性和可靠性。 它提供了数字签名、 解密和确认通过邮件处置通知 (MDN)。

## <a name="create-the-connection"></a>创建连接

### <a name="prerequisites"></a>系统必备组件

* Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)

* 使用解码 AS2 消息连接器需要集成帐户。 有关如何创建[集成帐户](./app-service-logic-enterprise-integration-create-integration-account.md)、[合作伙伴](./app-service-logic-enterprise-integration-partners.md)和[AS2 协议](./app-service-logic-enterprise-integration-as2.md)查看详细信息

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>连接到解码 AS2 邮件使用以下步骤︰

1. [创建一个逻辑应用程序](./app-service-logic-create-a-logic-app.md)提供了一个示例。

2. 此接口不具有任何触发器。 使用其他触发器来启动逻辑的应用程序，如请求触发器。  在逻辑应用程序设计器中，添加触发器并添加一个动作。  选择显示 Microsoft 托管的 Api，在下拉列表中列出，然后在搜索框中输入"AS2"。  选择 AS2 – AS2 消息解码

    ![搜索 AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. 如果您以前没有创建任何连接到集成的帐户，您的连接的详细信息

    ![创建集成的连接](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. 输入集成帐户详细信息。  带星号的属性是必需的

  	| 属性   | 详细信息 |
  	| --------   | ------- |
  	| 连接名称 *    | 输入您的连接的任何名称 |
  	| 集成帐户 * | 输入集成帐户名称。 确保您的集成帐户和应用程序逻辑都是在 Azure 所在 |

    完成后，您的连接详细信息看起来类似于以下

    ![集成的连接](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. 选择**创建**
    
6. 注意到已创建的连接。  现在，继续执行逻辑应用程序中的其他步骤

    ![创建集成连接](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. 从请求输出选择正文和标题

    ![提供了必填字段](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>AS2 解码执行以下

* AS2/HTTP 标头的处理
* 验证签名 （如果配置）
* 对邮件进行解密 （如果配置）
* 解压缩该消息 （如果配置）
* 协调与原始的出站邮件接收的 MDN
* 更新和不可否认性数据库中的记录进行关联
* 写入记录 AS2 状态报告
* 输出负载内容是 base64 编码
* 确定是否 MDN 是必需的和 MDN 应该是同步还是异步基于配置 AS2 协议中
* 生成同步或异步 MDN （根据协议配置）
* MDN 上设置相关的标记和属性

##<a name="try-it-for-yourself"></a>试试看自己

为什么不试一试。 单击[此处](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/)来部署您自己使用的逻辑应用程序 AS2 功能的一个完全可操作的逻辑应用程序 

## <a name="next-steps"></a>下一步行动

[了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包") 