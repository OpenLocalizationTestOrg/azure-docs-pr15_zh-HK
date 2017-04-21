<properties 
    pageTitle="了解如何为企业集成包创建 AS2 协议" 
    description="了解如何为企业集成包创建 AS2 协议 |Microsoft Azure 应用程序服务" 
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
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>AS2 与企业集成

## <a name="create-an-as2-agreement"></a>创建了 AS2 协议
为了在逻辑应用程序中使用的企业级功能，您必须首先创建协议。 

### <a name="heres-what-you-need-before-you-get-started"></a>以下是您需要在开始之前
- 在 Azure 订阅定义的[集成帐户](./app-service-logic-enterprise-integration-accounts.md)  
- 至少两个[合作伙伴](./app-service-logic-enterprise-integration-partners.md)集成帐户中已定义了  

>[AZURE.NOTE]当创建一个协议，协议文件中的内容必须匹配的协议类型。    


您已经[创建了一个集成帐户](./app-service-logic-enterprise-integration-accounts.md)并[添加合作伙伴](./app-service-logic-enterprise-integration-partners.md)后，您可以按照下面的步骤创建了协议︰  

### <a name="from-the-azure-portal-home-page"></a>从 Azure 门户主页

在您登录到[Azure 门户](http://portal.azure.com "Azure 门户")︰  
1. 从左侧的菜单中选择**浏览**。  

>[AZURE.TIP]如果您看不到**浏览**链接，您可能需要首先展开该菜单。 这样做的选择**显示菜单**链接，它位于折叠菜单的左上方。  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. 筛选器的搜索框中键入*的集成*，然后从结果列表中选择**集成帐户**。       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. 在打开**集成帐户**刀片式服务器，选择将在其中创建该协议集成帐户。 如果您看不到任何集成帐户列表，[创建一个第一个](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts")。  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  选择**协议**麻将牌。 如果您看不到平铺的协议，将第一次对其进行添加。   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. 打开刀片式服务器协议中选择**添加**按钮。  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. 输入您的协议的**名称**，然后打开刀片式服务器协议中选择**主机伙伴**、**主机标识**、**来宾合作伙伴**、**来宾身份**。  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

下面是配置您的协议的设置时您可能会发现有用的几个细节︰ 
  
|属性|说明|
|----|----|
|主机合作伙伴|协议需要的主机和来宾的合作伙伴。 主机伙伴代表正在配置协议的组织。|
|主机标识|主机伙伴的的标识符。 |
|访客合作伙伴|协议需要的主机和来宾的合作伙伴。 来宾合作伙伴代表正在执行的操作与主机合作伙伴的业务组织。|
|访客身份|访客合作伙伴标识符。|
|接收设置|这些属性将应用于通过协议接收的所有消息|
|发送设置|这些属性将应用于通过协议发送的所有消息|  
让我们继续︰  
7. 选择**接收设置**来配置此协议接收到的邮件的处理方式。  
 
 - （可选） 您可以重写传入消息中的属性。 若要执行此操作，选择**重写消息属性**复选框。
  - 如果您想要要求所有传入消息进行签名，请选择**应签名邮件**复选框。 如果您选择此选项，您还需要选择将用来验证消息签名的**证书**。
  - （可选） 您可以要求以及加密的邮件。 若要执行此操作，选择**应该加密消息**复选框。 然后，您需要选择将用来解码传入消息的**证书**。
  - 您还可以要求要压缩的消息。 若要执行此操作，选择**邮件应压缩**复选框。  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

如果您想要了解更多关于哪些接收设置，请参阅下表。  

|属性|说明|
|----|----|
|重写邮件属性|选择此选项可指示可以接收的消息中的属性中重写 |
|应该对消息进行签名|启用此选项要求消息进行数字签名|
|应加密消息|启用此选项要求消息进行加密。 非加密的消息将被拒绝。|
|应压缩邮件|启用此选项要求消息被压缩。 非压缩邮件将被拒绝。|
|MDN 文本|这是默认设置 MDN 发送给邮件发件人|
|发送 MDN|启用此设置以允许发送的 MDNs。|
|发送签名 MDN|启用此项会要求 MDNs 要签名。|
|MIC 算法||
|发送异步 MDN|启用此要求以异步方式发送邮件。|
|URL|这是将邮件发送到的 URL。|
现在，让我们继续︰  
8. 选择**发送设置**来配置此协议发送消息的处理方式。  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

如果您想要了解更多有关发送何种设置，请参阅下表。  

|属性|说明|
|----|----|
|启用邮件签名|选中此复选框可以启用从签署的协议发送的所有消息。|
|MIC 算法|选择用于对消息签名的算法|
|证书|选择用于对消息签名的证书|
|启用邮件加密|选中此复选框，从本协议发送的所有消息进行加密。|
|加密算法|选择要在邮件加密中使用的加密算法|
|展开的 HTTP 标头|选中此复选框可以展开成一行的 HTTP 内容类型标头。|
|请求 MDN|启用此复选框可以请求 MDN 从本协议发送的所有邮件|
|请求签名 MDN|启用请求发送到本协议的所有 MDNs 进行都签名|
|请求异步 MDN|启用请求异步 MDN 发送到本协议|
|URL|MDNs 将发送到的 URL|
|启用 NRR|选中此复选框以启用接收的非否认性|
我们几乎已完成 ！  
9. 选择**协议**图块集成帐户刀片式服务器上的，您将看到新添加的协议列出。  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

