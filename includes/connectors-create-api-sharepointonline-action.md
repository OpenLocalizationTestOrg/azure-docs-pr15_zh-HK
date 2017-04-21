现在，您已经添加了一个触发器，它的时间去做某事有趣的触发器生成的数据。 请按照下列步骤添加**SharePoint Online 的创建文件**的操作。 此操作将创建一个文件在 SharePoint Online 中每个新项触发器触发的时间。 

若要配置此操作，您需要提供以下信息。 为您提供此信息，您会注意到它是易于使用的数据作为输入的某些属性为新的文件生成的触发器︰

|创建文件属性|说明|
|---|---|
|站点的 URL|这是 SharePoint Online 网站想要创建新文件的 URL。 从显示的列表中选择的站点。|
|文件夹路径|这是 （在上一步中所选的网站 URL) 的文件夹放置新文件的位置。 浏览并选择文件夹。|
|文件名称|这是文件的正在创建的名称。|
|文件内容|将被写入该文件的内容。|

1. 选择**+ 新步骤**添加操作。  
![SharePoint 联机操作图像 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- 选择**添加操作**链接。 搜索框，您可以搜索的任何操作您想要此打开。 对于此示例，SharePoint 操作感兴趣。    
![SharePoint 联机操作图像 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- 输入要搜索的操作与 SharePoint *sharepoint* 。
- 为要执行的操作，请选择**SharePoint Online-创建文件**。   **注意**︰ 您将被提示以授权您访问 SharePoint 帐户，如果您以前没有创建连接到 SharePoint Online 的逻辑应用程序。    
![SharePoint 联机操作图像 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- **创建文件**控件将打开。   
![SharePoint 联机操作图像 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- 选择**网站 URL** ，并浏览以查找您想要创建该文件的站点。     
![SharePoint 联机操作图像 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- 选择**文件夹的路径**并浏览以查找放置新文件的文件夹。  
![SharePoint 联机操作图像 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- 选择**文件名称**控件并输入您想要创建的文件的名称。 在这里，您可以直接输入文件名称，或您可以使用任何从您以前创建的触发器的属性。 通过从**输出时创建新的项从**列表中选择属性来执行此操作。 此列表是只显示您选择的**文件的名称**控件之后。 在本演练中，我选择 ID (新的列表项 ID) 作为**SharePoint Online-创建文件**执行的操作所创建的文件的名称。    
![SharePoint 联机操作图像 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- 选择的**文件的内容**控件，然后输入将被写入将创建该文件的内容。 文件内容，请注意您可以使用任何从您以前创建的触发器属性。 只是从显示的列表中选择的属性。 或者，您可以直接在控件中输入**文件内容**的文本。 在此示例中，我选择一些属性并添加空格和连字符分隔每个属性。        
![SharePoint 联机操作图像 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- 将更改保存到您的工作流  
- 祝贺您，您现在可以向 SharePoint Online 列表中添加新项时，会触发一个完备的逻辑应用程序。 应用程序然后将创建一个文件，使用新的列表项的属性的部分。  现在可以通过在 SharePoint 列表中创建新的项目来测试它。 
