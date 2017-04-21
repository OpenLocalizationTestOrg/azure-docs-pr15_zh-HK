在此示例中，我将介绍如何使用**SharePoint Online 日-创建新项目时，**触发器来启动逻辑应用程序工作流时在线 SharePoint 列表中创建一个新项。

>[AZURE.NOTE]您将得到提示登录到您的 SharePoint 帐户，如果尚未创建*连接*到 SharePoint Online。  

1. 逻辑应用程序设计器上的搜索框中输入*sharepoint*然后选择**SharePoint Online 日-创建新项时**触发。  
![SharePoint 在线触发图像](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- **创建新项目时，**控件显示。  
![SharePoint 在线触发图像 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- 选择一个**网站 URL**。 这是您想要监视的新项目可触发工作流 SharePoint online 网站。  
![SharePoint 在线触发图像 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- 选择**列表的名称**。 这是在 SharePoint Online 网站想要监视将触发工作流的新项的列表。  
![SharePoint 在线触发图像 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

此时，您的逻辑应用程序已配置一个触发器，它将开始运行的触发器和操作工作流中的使用。 这会发生每次您选择联机 SharePoint 列表中创建一个新项。  