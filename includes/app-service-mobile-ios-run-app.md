
1. 在您的 Mac，请访问[Azure 门户]。 单击**浏览所有** > **移动应用程序**> 您刚刚创建的后端。 在移动应用程序设置中，单击**快速启动** > **Io (目标 C)**。 如果您愿意 Swift，单击**快速启动** > **Io (Swift)**相反。 在**下载和运行 iOS 项目**，单击**下载**。 这会将下载完整的 Xcode 项目预配置为连接到您的后端应用程序。 打开使用 Xcode 项目。

2. 按**运行**按钮以生成项目并在 iOS 模拟器中启动该应用程序。

3. 在应用程序中，键入有意义的文本，例如，_完成本教程_，然后单击加号 (**+**) 图标。 这将 POST 请求发送到 Azure 的后端早部署。 从请求的后端插入数据到 TodoItem SQL 表中，并返回到移动应用程序的新存储项目的信息。 移动应用程序在列表中显示此数据。 

    ![](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure 门户]: https://portal.azure.com/
