在此演练中，您将学习如何使用**队伍-在创建对象时**触发器来启动逻辑应用程序工作流时在您的销售队伍中创建新的潜在顾客。

>[AZURE.NOTE]您将得到提示登录到您销售队伍的帐户，如果尚未创建*连接*到队伍。  

1. 逻辑应用程序设计器上的搜索框中输入*销售队伍*，然后选择**队伍-在创建对象时**触发。  
![销售队伍触发器图像 1](./media/connectors-create-api-salesforce/trigger-1.png)   
- **在创建对象时**控件显示。  
![销售队伍触发器图像 2](./media/connectors-create-api-salesforce/trigger-2.png)   
- 选择**对象类型**，然后*会*从列表中选择的对象。 在此步骤中将表示您正在创建一个触发器，它会通知您的逻辑应用程序只要在队伍中创建新的潜在顾客。   
![销售队伍触发器图像 3](./media/connectors-create-api-salesforce/trigger-3.png)   
- 就是这样。 您已经创建触发器。 但是，您需要创建至少一个操作，以使其有效的逻辑应用程序。    
![销售队伍触发器图像 4](./media/connectors-create-api-salesforce/trigger-4.png)   

此时，您的逻辑应用程序已配置与您的销售队伍中创建新项目时将开始一连串的其他触发器和操作工作流中的触发器。  