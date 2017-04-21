现在，您已添加一个条件，它的时间去做某事有趣由触发器生成的数据。 请按照以下步骤添加的**队伍-获取对象**操作。 此操作将在每次创建新的潜在顾客时获取的数据。 您还将添加第二个操作将使用从队伍的 Get 发送一封电子邮件，使用 Office 365 连接器对象操作的数据。  

若要配置此操作，您需要提供以下信息。 您会注意到它是易于使用的数据作为输入的某些属性为新的文件生成的触发器︰

|创建文件属性|说明|
|---|---|
|对象类型|这是您感兴趣的销售对象的类型。 示例包括潜在顾客、 帐户等。|
|对象 ID|这表示该对象的标识符。|


1. 选择**添加操作**链接。 搜索框，您可以搜索的任何操作您想要此打开。 对于此示例，队伍操作感兴趣。      
![销售队伍操作图像 1](./media/connectors-create-api-salesforce/action-1.png)  
- 输入*销售队伍*要搜索与销售相关的操作。
- 选择要执行的操作**队伍-获取对象**。   **注意**︰ 您将被提示以授权您访问您销售队伍的帐户，如果您还没有这样做以前的逻辑应用程序。    
![销售队伍操作图像 2](./media/connectors-create-api-salesforce/action-2.png)    
- **获取对象**的控件将打开。  
- 对象类型为选择*潜在顾客*。
- 选择**对象 ID**控件。
- 选择**...**展开可以用作操作输入的标记的列表。       
![销售队伍操作图像 3](./media/connectors-create-api-salesforce/action-3.png)    
- 打开选择的**潜在客户 ID**控件。   
![销售队伍操作图像 4](./media/connectors-create-api-salesforce/action-4.png)     
- 注意到潜在客户 ID 标记现在是控件中的对象 ID，指示获取对象操作将搜索潜在顾客 id 等于触发此逻辑应用程序中的潜在顾客的潜在客户 ID。  
![销售队伍操作图像 5](./media/connectors-create-api-salesforce/action-5.png)  
- 保存您的工作。 就是这样，获取对象操作添加到您的应用程序逻辑。 获取对象控件应如下所示︰    
![销售队伍操作图像 6](./media/connectors-create-api-salesforce/action-6.png)  

现在，您已经添加的操作来获取潜在顾客，您可以做一些有趣的事情与新创建的潜在顾客。 在企业中，您可以发送电子邮件通知的通讯组列表已创建新的潜在顾客。 让我们使用 Office 365 连接器从队伍中的新潜在顾客对象发送一封电子邮件，与某些相关信息。  

1. 选择**添加操作**，然后在搜索控件中输入*的电子邮件*。 此筛选对那些相关的发送和接收电子邮件的操作。  
- 选择**Office 365 Outlook-发送一封电子邮件**的列表项。 如果您还没有到 Office 365 帐户创建*连接*，将提示您输入您的 Office 365 提供凭据立即创建它。 完成后，**将发送一封电子邮件**控件将打开。        
![销售队伍操作图像 7](./media/connectors-create-api-salesforce/action-7.png)  
- 请输入您想要添加**到**控件中发送到的电子邮件的电子邮件地址。
-  在**主题**控件中输入*新建潜在顾客创建*-然后选择*公司*标记。 这将显示*公司*字段从队伍中创建新的潜在顾客。  
-  在**正文**控件中，可以从新的潜在顾客对象中选择任何标记，您也可以输入任何您想要显示在电子邮件的正文中的文本。 下面是一个示例︰  
![销售队伍操作图像 8](./media/connectors-create-api-salesforce/action-8.png)   
- 保存您的工作流。  

就是这样。 逻辑应用程序现已完成。  

现在，您可以测试您的逻辑应用程序︰ 在销售队伍，创建新的潜在顾客，以满足您创建的条件。  如果完全按照此预排，只是在电子邮件地址包含在*amazon.com*创建潜在顾客。 几秒钟后应触发逻辑应用程序和结果可能看起来像这样︰  
![销售队伍操作图像 9](./media/connectors-create-api-salesforce/action-9.png)  
