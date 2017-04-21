## <a name="connect-to-outlookcom"></a>连接到 Outlook.com

### <a name="prerequisites"></a>系统必备组件
- Outlook.com 帐户

逻辑应用程序中使用您的 Outlook.com 帐户之前，您必须授权逻辑应用程序连接到您的 Outlook.com 帐户。 幸运的是，你可以很容易地从 Azure 门户上的逻辑应用程序内。 

以下是授权逻辑应用程序连接到您的 Outlook.com 帐户的步骤︰

1. 所有的逻辑应用程序需要由触发器启动，以便创建您的逻辑应用程序设计器将打开并显示一个列表触发，后可用来启动您的逻辑应用程序︰

  ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. 在搜索框中输入"outlook"。 请注意列表进行筛选，列出在名称中使用"Outlook"所有触发器︰![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. 选择**Office 365 Outlook 的新电子邮件上**。   
  如果还没有创建任何连接到 Outlook 之前，您将得到提示您提供 Outlook.com 凭据。 这些凭据将用于授权您的逻辑应用程序连接到并访问您的 Outlook.com 帐户的数据︰![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Outlook 提供您的凭据并登录︰![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
  就是这样。 现在，您已创建一个连接到 Outlook。 可在您创建任何其他逻辑应用程序中使用此连接。


