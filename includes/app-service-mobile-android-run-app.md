
1. 请访问[Azure 的门户]。 单击**浏览所有** > **移动应用程序**> 您刚刚创建的后端。 在移动应用程序设置中，单击**快速启动** > **Android)**。 在**配置客户端应用程序**，单击**下载**。 这会将下载完整的 Android 项目预配置为连接到您的后端应用程序。 

2. 打开使用**Android Studio**，使用**导入项目 （Eclipse ADT，Gradle 等）**的项目。 请确保您导入选择此选项，以避免任何 JDK 错误。

3. 按**运行应用程序**按钮以生成项目并在 Android 模拟器中启动该应用程序。

4. 在应用程序中，键入有意义的文本，例如_完成本教程_，然后单击添加按钮。 这将 POST 请求发送到 Azure 的后端早部署。 从请求的后端插入数据到 TodoItem SQL 表中，并返回到移动应用程序的新存储项目的信息。 移动应用程序在列表中显示此数据。 

    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure 门户]: https://portal.azure.com/
