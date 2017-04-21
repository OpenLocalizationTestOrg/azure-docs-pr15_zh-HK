下面介绍了如何使用**服务总线队列中收到一条消息时**触发一个新项发送到服务总线队列时启动逻辑应用程序流。  

>[AZURE.NOTE]系统将提示您要登录服务总线连接字符串，如果已创建一个连接到服务总线。  

1. 逻辑应用程序设计器在搜索框中，输入**服务总线**。 然后选择**服务总线队列中收到一条消息时**触发。  
![服务总线触发器图像 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- **当队列中接收消息**对话框。  
![服务总线触发器图像 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- 请输入您想要监视的触发器服务总线队列的名称。   
![服务总线触发器图像 3](./media/connectors-create-api-servicebus/trigger-3.png)   

此时，您的逻辑应用程序已配置触发器。 您所选的队列中收到新的项目时，触发器会开始一连串的其他触发器和操作工作流中。    
