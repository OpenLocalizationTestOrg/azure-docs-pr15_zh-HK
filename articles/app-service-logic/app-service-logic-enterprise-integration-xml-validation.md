<properties 
    pageTitle="概述了企业集成包中的 XML 验证 |Microsoft Azure 应用程序服务 |Microsoft Azure" 
    description="了解验证在企业集成包和逻辑的应用程序的运行方式" 
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

# <a name="enterprise-integration-with-xml-validation"></a>XML 验证与企业集成

## <a name="overview"></a>概述
通常情况下，在 B2B 方案协议的伙伴需要验证在相互之间交换的消息在开始数据的处理之前的有效。 在企业集成包，您可以使用 XML 验证连接器可以根据预定义的架构验证文档。  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>如何验证 XML 验证连接器的文档
1. 创建逻辑应用程序并[将其链接到集成帐户](./app-service-logic-enterprise-integration-accounts.md "了解链接到逻辑应用程序集成的帐户")包含将用于验证 XML 数据的架构。
2. 将**接收到请求-当 HTTP 请求**触发器添加到逻辑应用程序  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. 添加的第一个选择**添加操作**的**XML 验证**操作  
4. 在搜索框中输入*xml* ，以便筛选到您想要使用的所有操作 
5. 选择**XML 验证**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. 选择**内容**文本框中  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. 选择正文标记将验证的内容。   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. 选择**架构名称**列表框，选择想要用于验证的输入的*内容*上面的架构     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. 保存您的工作  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

此时，您已完成设置验证连接器。 在实际应用中，您可能需要将经验证的数据存储在 LOB 应用程序如队伍。 您可以轻松添加要验证的输出发送到队伍的动作。 

现在可以通过请求的 HTTP 端点来测试您的验证操作。  

## <a name="next-steps"></a>下一步行动

[了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包")   