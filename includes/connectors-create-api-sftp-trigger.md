让我们添加一个触发器。

1. 逻辑应用程序设计器上的搜索框中输入*sftp*然后选择**SFTP-添加或修改文件时**触发   
![SFTP 触发器图像 1](./media/connectors-create-api-sftp/trigger-1.png)  
- **添加或修改文件时**控件打开  
![SFTP 触发器图像 2](./media/connectors-create-api-sftp/trigger-2.png)  
- 选择**...**位于控件的右侧。 这将打开的文件夹选取器控件  
![SFTP 触发器图像 3](./media/connectors-create-api-sftp/action-1.png)  
- 选择**SFTP**若要选择要监视的新的或修改的文件的文件夹的根文件夹。 请注意现在显示在**文件夹**控件中的根文件夹。  
![SFTP 触发器图像 4](./media/connectors-create-api-sftp/action-2.png)   

此时，您的逻辑应用程序已配置修改或在特定的 SFTP 文件夹中创建文件时，将开始一连串的其他触发器和操作工作流中的触发器。 

>[AZURE.NOTE]逻辑应用程序才能正常工作，它必须包含至少一个触发器和一个操作。 请按照下一节中添加操作。  