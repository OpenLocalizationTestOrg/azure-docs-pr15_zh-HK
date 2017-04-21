### <a name="prerequisites"></a>系统必备组件

您必须具有一个[服务总线](https://azure.microsoft.com/services/service-bus/)的帐户。  

逻辑应用程序中使用 Azure 服务总线帐户之前，您必须授权逻辑应用程序连接到您的服务总线帐户。 幸运的是，你可以很容易地从 Azure 的门户网站上的逻辑应用程序内。  

以下是授权逻辑应用程序连接到服务总线帐户的步骤︰  

1. 若要创建连接到服务总线逻辑应用程序设计器中，在下拉列表中选择**显示 Microsoft 托管 Api** 。 然后在搜索框中输入**服务总线**。 选择您要使用的操作的触发器。  
    ![服务总线连接图像 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. 如果您没有创建任何连接到服务总线之前，将提示您提供您的服务总线凭据。 这些凭据用于授权逻辑应用程序将连接到并访问服务总线帐户的数据。 服务总线连接器需要的服务总线命名空间的连接字符串。 它还需要**管理**权限。 最好知道您的连接字符串是否为命名空间或特定实体是如果其中包含`EntityPath`参数。 如果是这样，它不是逻辑应用程序的正确的连接字符串。  
    ![服务总线连接字符串](./media/connectors-create-api-servicebus/connectionstring.png)

1. 收到该命名空间的连接字符串后，可以使用它在逻辑应用程序的 API 连接。  
    ![服务总线连接图像 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. 注意到已创建的连接，并且您现在可以自由地逻辑应用程序中的其他步骤。  
    ![服务总线连接图像 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
