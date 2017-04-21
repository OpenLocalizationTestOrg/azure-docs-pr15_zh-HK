现在，您已经添加了一个触发器，它的时间去做某事有趣的触发器生成的数据。 请按照以下步骤添加**SFTP-解压缩文件夹**操作。 满足所定义的条件时，此操作将提取文件的内容。 

若要配置此操作，您需要提供以下信息。 您会注意到它是易于使用的数据作为输入的某些属性为新的文件生成的触发器︰

|SFTP-解压缩文件夹属性|说明|
|---|---|
|源存档文件路径|这是为提取的文件的路径。 您可以从早期的操作选择一个标记或浏览 SFTP 服务器找不到文件路径。|
|目标文件夹路径|这是放置提取的文件的位置的路径。 可以从早期操作作为目标路径中选择一个标记或 SFTP 服务器浏览并选择一个路径。|
|是否覆盖？|指示应覆盖现有的文件是否是否目标文件夹路径中找到已提取的文件同名的文件。|

让我们开始添加前面定义的条件计算结果为*True*，则提取的文件的操作。 

1. 选择**添加操作**。        
![SFTP 操作条件图像 6](./media/connectors-create-api-sftp/condition-6.png)   
- 选择**SFTP-解压缩文件夹**操作      
![SFTP 操作条件图像 7](./media/connectors-create-api-sftp/condition-7.png)   
- 选择**源归档文件路径**              
![SFTP 操作条件图像 9](./media/connectors-create-api-sftp/condition-9.png)   
- 选择**文件路径**标记。 这表示您将使用触发器找到的文件的文件路径作为源归档文件的文件路径。           
![SFTP 操作条件图像 10](./media/connectors-create-api-sftp/condition-10.png)   
- 选择**目标文件夹路径**           
![SFTP 操作条件图像 11](./media/connectors-create-api-sftp/condition-11.png)   
- 选择**文件路径**标记。 这表明，将提取的文件作为目标路径使用触发器找到的文件的文件路径。   
- **目标文件夹路径**控件中输入*\ExtractedFile* 。 目标文件夹路径控制中的文件路径令牌之后执行此操作。         
![SFTP 操作条件图像 12](./media/connectors-create-api-sftp/condition-12.png)   
- 输入*True*在 **覆盖？*控件来指示它们为提取的文件具有相同的名称时应覆盖现有的文件。      
![SFTP 操作条件图像 13](./media/connectors-create-api-sftp/condition-13.png)   
- 将更改保存到您的工作流  
