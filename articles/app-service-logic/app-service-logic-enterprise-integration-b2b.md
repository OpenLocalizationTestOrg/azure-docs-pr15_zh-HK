<properties 
    pageTitle="与企业集成包创建 B2B 解决方案 |Microsoft Azure 应用程序服务 |Microsoft Azure" 
    description="了解有关使用企业集成包的 B2B 功能接收数据" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>了解有关使用企业集成包的 B2B 功能接收数据#

## <a name="overview"></a>概述 ##

本文档是逻辑应用企业集成包的一部分。 查看概述以了解更多有关[企业集成包功能](./app-service-logic-enterprise-integration-overview.md)。

## <a name="prerequisites"></a>系统必备组件 ##

若要使用 AS2 和 X12 操作需要企业集成帐户

[如何创建企业集成帐户](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>如何使用逻辑应用程序 B2B 连接器 ##

一旦您创建了一个集成帐户并添加合作伙伴，您就可以创建一个逻辑应用程序到该协议，实现了企业到企业 (B2B) 工作流。

该 walkthru 中，您将看到如何使用 AS2 和 X12 操作创建一个业务到业务逻辑应用程序从贸易伙伴接收数据。

1. 创建新的逻辑应用程序并[将其链接到您的集成帐户](./app-service-logic-enterprise-integration-accounts.md)。  
2. 将**接收到请求-当 HTTP 请求**触发器添加到逻辑应用程序  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. 添加的第一个选择**添加操作****解码 AS2**操作  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. 在搜索框中输入单词**as2** ，以便筛选到您想要使用的所有操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. 选择**AS2 的解码 AS2 消息**的操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. 如所示，添加**正文**需要较长的作为输入。 在此示例中，选择触发逻辑应用程序的 HTTP 请求的正文。 或者，您可以输入表达式输入中的**标头**字段的标头︰

    @triggerOutputs()['headers']

8. 添加所需的 AS2**头**。 这些消息将在 HTTP 请求标头中。 在此示例中，选择触发逻辑应用程序的 HTTP 请求的标的头。
9. 现在，再次选择**添加操作**将添加解码 X12 消息操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. 在搜索框中输入单词**x12** ，以便筛选到您想要使用的所有操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. 选择**X12 的解码 X12 消息**操作以将其添加到应用程序逻辑  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. 现在，您需要指定将 AS2 操作上面的输出此操作的输入。 实际消息内容是一个 JSON 对象，进行 base64 编码。 因此，您需要输入因此**X12 平面文件消息到解码**的输入字段中输入下面的表达式指定一个表达式  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. 此步骤将解码的 X12 从贸易伙伴接收数据，并将输出一个 JSON 对象中的项目数。 知道的数据接收的伙伴，这样您可以发送回包含 AS2 消息处置通知 (MDN) 在 HTTP 响应操作的响应  
14. 通过选择**添加操作**添加**响应**操作   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. 在搜索框中输入单词**响应**，以便筛选到您想要使用的所有操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. 选择要添加它的**响应**操作  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. 通过使用下面的表达式访问 MDN**解码 X12 消息**操作的输出设置响应**正文**字段  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. 保存您的工作  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

此时，您已完成设置您 B2B 逻辑的应用程序。 在实际应用中，您可能想要存储已解码的 X12 LOB 应用程序或数据存储区中的数据。 您可以轻松添加进一步的操作来执行此操作或编写自定义的 Api 连接到 LOB 应用程序和逻辑应用程序中使用这些 Api。

## <a name="features-and-use-cases"></a>功能和使用案例 ##

- AS2 和 X12 解码和编码操作，可以接收数据并将数据发送到贸易伙伴使用行业标准协议使用逻辑应用程序  
- 您可以使用 AS2 和 X12 带有或不带相互交换数据与贸易伙伴的要求
- B2B 操作方便地集成帐户中创建合作伙伴和协议和逻辑应用程序中使用它们  
- 通过扩展逻辑应用程序与其他操作您可以发送和接收数据与其他应用程序和服务，例如销售队伍  

## <a name="learn-more"></a>了解更多信息 ##

[了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md)  