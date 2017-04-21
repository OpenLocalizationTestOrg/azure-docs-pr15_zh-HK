#### <a name="prerequisites"></a>系统必备组件
- Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)
- [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx)的帐户 

逻辑应用程序中使用动态帐户之前, 授权逻辑应用程序连接到您的 CRM 联机帐户。 在 Azure 的门户网站上的逻辑应用程序可以轻松地执行此操作。 

授权您的逻辑应用程序连接到在线 CRM 帐户使用以下步骤︰

1. 创建一个逻辑应用程序。 在逻辑应用程序设计器中，在下拉列表中，选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入"动态"。 选择触发器或操作之一︰  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. 如果您以前没有创建任何连接到动态，则会提示您使用动态凭据进行登录︰  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. 选择**登录**，然后输入您的用户名和密码。 选择**登录**。 

    这些凭据用于授权逻辑应用程序要连接和访问动态帐户中的数据。 
4. 注意到已创建的连接。 现在，继续逻辑应用程序中的其他步骤︰  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
