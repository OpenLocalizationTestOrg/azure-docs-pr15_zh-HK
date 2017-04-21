<properties
pageTitle="SMTP |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 连接到 SMTP 发送电子邮件。"
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>开始使用 SMTP 连接器

连接到 SMTP 发送电子邮件。

使用[任何连接器](./apis-list.md)，您首先需要创建一个逻辑应用程序。 您可以通过[创建一个逻辑应用程序现在](../app-service-logic/app-service-logic-create-a-logic-app.md)开始。

## <a name="connect-to-smtp"></a>连接到 SMTP

逻辑应用程序可以访问的任何服务之前，您首先需要创建一个*连接*到服务。 [连接](./connectors-overview.md)提供一个逻辑应用程序和其他服务之间的连接。 例如，若连接到 SMTP，您首先需要 SMTP*连接*。 要创建连接，您需要提供的凭据通常用于访问要连接到的服务。 因此，在 SMTP 的示例中，您需要连接名称、 SMTP 服务器的地址和用户登录信息的凭据才能创建连接到 SMTP。 [了解有关连接的详细信息]()  

### <a name="create-a-connection-to-smtp"></a>创建 SMTP 连接

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>使用 SMTP 触发器

触发器是可以用来启动工作流的逻辑应用程序中定义的事件。 [了解更多有关触发器](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)的信息。

在此示例中，因为 SMTP 并没有触发器本身，我们将使用**队伍-在创建对象时**触发。 在队伍中创建新的对象时，将激活此触发器。 对于我们的示例中，我们将设置它，每次在销售队伍中创建新的潜在顾客时，*发送电子邮件*的操作是通过创建新的潜在顾客的通知的 SMTP 连接器。

1. 逻辑应用程序设计器上的搜索框中输入*销售队伍*，然后选择**队伍-在创建对象时**触发。  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. **在创建对象时**控件显示。
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. 选择**对象类型**，然后*会*从列表中选择的对象。 在此步骤中将表示您正在创建一个触发器，它会通知您的逻辑应用程序只要在队伍中创建新的潜在顾客。  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. 已创建触发器。  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>使用 SMTP 的操作

操作是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)信息。

在添加触发器后，请按照上述步骤以添加 SMTP 动作会出现在队伍中创建新的潜在顾客时。

1. 选择**+ 新的步骤**来添加您想要创建新的潜在顾客时所采取的操作。  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. 选择**添加操作**。 搜索框，您可以搜索的任何操作您想要此打开。  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. 输入*smtp*来搜索与 SMTP 相关的操作。  

4. 选择**SMTP-发送电子邮件**创建新的潜在顾客时所采取的操作。 打开操作控制块。 您将需要建立 smtp 连接设计器块中的，如果您还没有这样做以前。  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. 输入您所需的电子邮件信息中**SMTP-发送电子邮件**阻止。  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. 保存所做的工作，以激活工作流。  

## <a name="technical-details"></a>技术详细信息

以下是有关触发器、 操作和支持此连接的响应的详细信息︰

## <a name="smtp-triggers"></a>SMTP 触发器

SMTP 已没有触发器。 

## <a name="smtp-actions"></a>SMTP 的操作

SMTP 具有以下操作︰


|操作|说明|
|--- | ---|
|[发送电子邮件](connectors-create-api-smtp.md#send-email)|此操作将电子邮件发送给一个或多个收件人。|

### <a name="action-details"></a>操作详细信息

下面是此连接器，同时其响应的操作的详细信息︰


### <a name="send-email"></a>发送电子邮件
此操作将电子邮件发送给一个或多个收件人。 


|属性名称| 显示名称|说明|
| ---|---|---|
|自|自|指定由类似的分号分隔的电子邮件地址recipient1@domain.com;recipient2@domain.com|
|抄送|抄送|指定由类似的分号分隔的电子邮件地址recipient1@domain.com;recipient2@domain.com|
|主题|主题|电子邮件主题|
|正文|正文|电子邮件正文|
|从|从|如发件人的电子邮件地址sender@domain.com|
|IsHtml|为 Html|发送电子邮件以 html 格式 （真/假）|
|密件抄送|密件抄送|指定由类似的分号分隔的电子邮件地址recipient1@domain.com;recipient2@domain.com|
|重要性|重要性|（高、 正常或低） 的电子邮件的重要性|
|ContentData|附件内容数据|内容数据 (base64 编码流和为的是字符串)|
|ContentType|附件内容类型|内容类型|
|ContentTransferEncoding|附件内容传输编码|内容传输编码 （base64 或无）|
|文件名|附件文件的名称|文件名称|
|ContentId|附件内容 ID|内容 id|

* 指示属性是必选


## <a name="http-responses"></a>HTTP 响应

操作和上面的触发器可以返回一个或多个以下 HTTP 状态代码︰ 

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误。|
|默认|操作失败。|

## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)