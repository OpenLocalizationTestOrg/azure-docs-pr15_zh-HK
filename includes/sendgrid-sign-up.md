Azure 客户每个月可以解锁 25000 的免费电子邮件。 这些 25000 免费每月电子邮件将为您提供访问高级报告和分析以及[所有 Api][] （Web、 SMTP、 事件、 分析等）。 由 SendGrid 提供的其他服务的信息，请参阅[SendGrid 功能][]页。

### <a name="to-sign-up-for-a-sendgrid-account"></a>若要注册为 SendGrid 帐户

1. 登录到[Azure 的管理门户][]。

2. 在管理门户的下部窗格中，单击**新建**。

    ![新命令栏的][command-bar-new]

3. 单击**市场**。

    ![sendgrid 存储][sendgrid-store]

4. 在**选择应用程序和服务**对话框中，选择**SendGrid** ，并单击向右箭头。

5. 在**自定义应用程序和服务**对话框中选择您想要注册的**SendGrid**计划。

6. 输入一个名称来标识您的**SendGrid**服务在 Azure 设置，或使用**SendGridEmailDelivery.Simplified.SMTPWebAPI**的默认值。 名称必须是 1 到 100 个字符之间并且包含字母数字字符、 短划线、 圆点和下划线。 名称必须是唯一的订阅 Azure 存储库项目列表中。

    ![存储-屏幕-2][store-screen-2]

7. 选择的值的区域;例如，西部美国。

8. 单击向右箭头。

9. **查看购买**选项卡上，检查计划和价格信息，并审查法律的条款。 如果您同意这些条款，请单击该复选标记。 单击复选标记后，您的 SendGrid 帐户将开始在[SendGrid 资源调配过程]。

    ![存储屏幕 3][store-screen-3]

10. 确认您的购买后您将被重定向到附件面板，您将看到消息**购买 SendGrid**。

    ![-购买 sendgrid 的消息][sendgrid-purchasing-message]

    立即配置您的 SendGrid 帐户时，您将看到**成功购买附加 SendGrid**的消息。 现在创建您的帐户和凭据。 现在可以在此时发送电子邮件。 

    若要修改的订阅计划或查看联系人设置 SendGrid，单击 SendGrid 服务打开 SendGrid 市场仪表板的名称。 

    ![sendgrid-添加-上-仪表板][sendgrid-add-on-dashboard]

    若要发送一封电子邮件，使用 SendGrid，您必须提供您的帐户凭据 （用户名和密码）。

### <a name="to-find-your-sendgrid-credentials"></a>若要查找您的 SendGrid 凭据 ###

1. 单击**连接信息**。

    ![sendgrid 连接信息按钮][sendgrid-connection-info-button]

2. 在*连接信息*对话框中，复制以后在本教程中使用的**密码**和用户名。

    ![sendgrid 连接信息][sendgrid-connection-info]

    若要设置您的电子邮件可交付性设置，请单击**管理**按钮。 这将重定向到 SendGrid 控制面板。 

    ![sendgrid-控制面板][sendgrid-control-panel]

    开始使用 SendGrid 的详细信息，请参阅[SendGrid 入门][]。

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[SendGrid 功能]: http://sendgrid.com/features
[Azure 的管理门户]: https://manage.windowsazure.com
[SendGrid 入门 》]: http://sendgrid.com/docs
[SendGrid 资源调配过程]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[所有 Api]: https://sendgrid.com/docs/API_Reference/index.html

