
1. 在 Visual Studio 解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**存储** > **相关联的应用程序与存储...**。

    ![将应用程序与 Windows 存储关联](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. 在向导中，单击**下一步**，使用您的 Microsoft 帐户登录、**保留新的应用程序名称**，键入您的应用程序的名称，然后单击**保留**。

3. 成功创建的应用程序注册后，选择新的应用程序名称，请单击**下一步**，，然后单击**关联**。 将所需的 Windows 应用商店注册信息添加到应用程序清单。

7. 为 Windows 应用商店应用程序使用您以前创建的同一个注册 Windows Phone 存储应用程序项目，请重复步骤 1 到 3。  

7. 导航到[Windows 开发人员中心](https://dev.windows.com/en-us/overview)，登录与您的 Microsoft 帐户，请单击新的应用程序注册，在**我的应用程序**，然后展开**服务** > **推式通知**。

8. 在**推式通知**页上，在**Windows 推送通知服务 (WNS) 和 Microsoft Azure 移动应用程序**，单击**Live 服务网站**并记下**包 SID**的值和*当前*值在**应用程序的机密**。 

    ![开发人员中心中的应用程序设置](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] 应用程序密码和包 SID 是重要的安全凭据。 不要与任何人共享这些值或将其与您的应用程序一起分发。
