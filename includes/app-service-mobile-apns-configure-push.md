

1. 在您的 Mac，启动**钥匙链访问**。 在左的 navigationn 栏上打开**我的证书****类别**下。 查找上一节中下载的 SSL 证书和泄露其内容。 选择的证书 （不选择专用密钥），然后[将其导出](https://support.apple.com/kb/PH20122?locale=en_US)。

2. 在[Azure 的门户网站](https://portal.azure.com/)中，请单击**浏览所有** > **应用程序服务**> 您移动应用程序的后端。 在**设置**上**推入应用程序服务**，请单击，然后单击您通知中心的名称。 转到**苹果推送通知服务** > **上载证书**。 将上载.p12 文件，选择正确的**模式**（具体取决于您的客户端 SSL 证书从以前的是否是生产或沙箱）。 保存任何更改。

您的服务现在已配置为使用推式通知 iOS 上 ！

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
