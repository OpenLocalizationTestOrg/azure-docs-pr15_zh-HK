### <a name="prerequisites"></a>系统必备组件
- [SendGrid](https://www.SendGrid.com/)帐户 

逻辑应用程序中使用您的 SendGrid 帐户之前，您必须授权逻辑应用程序连接到您的 SendGrid 帐户。 幸运的是，你可以很容易地从 Azure 门户上的逻辑应用程序内。 

以下是授权逻辑应用程序连接到您的 SendGrid 帐户的步骤︰

1. 若要创建连接到 SendGrid，在逻辑的应用程序设计器中，在下拉列表中选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入*SendGrid* 。 选择触发器的操作会喜欢使用︰  
  ![SendGrid 第 1 步](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. 如果您没有创建任何连接到 SendGrid 之前，您将获得提示您提供 SendGrid 凭据。 这些凭据将用于授权您的逻辑应用程序连接到并访问您的 SendGrid 帐户的数据︰  
  ![SendGrid 步骤 2](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. 注意到已创建的连接，您现在可以自由地逻辑应用程序中的其他步骤︰  
  ![SendGrid 第 3 步](./media/connectors-create-api-sendgrid/sendgrid-3.png)   
