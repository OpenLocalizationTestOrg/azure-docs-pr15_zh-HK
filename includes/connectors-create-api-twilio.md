### <a name="prerequisites"></a>系统必备组件
- Twilio 帐户
- 可以接收 SMS 验证的 Twilio 电话号码
- 已验证的 Twilio 电话号码可以发送短消息

>[AZURE.NOTE] 如果您使用的 Twilio 试用帐户，您只能向**已验证**电话号码发送 SMS。  

逻辑应用程序中使用您的 Twilio 帐户之前，您必须授权逻辑应用程序连接到您的 Twilio 帐户。 幸运的是，你可以很容易地从 Azure 门户上的逻辑应用程序内。 

以下是授权逻辑应用程序连接到您的 Twilio 帐户的步骤︰

1. 若要创建连接到 Twilio，在逻辑的应用程序设计器中，在下拉列表中选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入*Twilio* 。 选择触发器的操作会喜欢使用︰  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. 如果您没有创建任何连接到 Twilio 之前，您将获得提示您提供 Twilio 凭据。 这些凭据将用于授权您的逻辑应用程序连接到并访问您的 Twilio 帐户的数据︰  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. 您将需要将**Twilio 帐户 id**和**Twilio 访问令牌**从仪表板中 Twilio，因此登录到您 Twilio 帐户现在要抓住以下两条信息︰  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio 和逻辑的应用程序使用不同的名称来标识这两部分的信息。 下面是如何您必须将它们映射到逻辑应用程序对话框︰![](./media/connectors-create-api-twilio/twilio-3.png)  
5. 选择**创建连接**按钮︰  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. 注意到已创建的连接，您现在可以自由地逻辑应用程序中的其他步骤︰  
  ![](./media/connectors-create-api-twilio/twilio-5.png)