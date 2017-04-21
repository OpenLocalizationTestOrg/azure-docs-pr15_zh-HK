<properties 
    pageTitle="在五分钟后将第一个 Java web 应用程序部署到 Azure |Microsoft Azure" 
    description="了解部署示例应用程序的应用程序服务中运行 web 应用程序是多么容易。 启动快速进行实际的开发，并立即查看结果。" 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>在五分钟后将第一个 Java web 应用程序部署到 Azure

本教程将帮助您将一个简单的 Java web 应用程序部署到[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)。
您可以使用应用程序服务创建 web 应用程序、[移动应用程序的后端](/documentation/learning-paths/appservice-mobileapps/)，和[API 的应用程序](../app-service-api/app-service-api-apps-why-best-platform.md)。

您将能够︰ 

- 在 Azure 应用程序服务创建一个 web 应用程序。
- 部署示例 Java 应用程序。
- 请参阅您在生产环境中实时运行的代码。

## <a name="prerequisites"></a>系统必备组件

- 获取 FTP/FTPS 客户端，如[FileZilla](https://filezilla-project.org/)。
- 获取 Microsoft Azure 帐户。 如果您没有帐户，您可以[注册免费试用版](/pricing/free-trial/?WT.mc_id=A261C142F)或[激活您的 Visual Studio 的订阅者权益](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)。

>[AZURE.NOTE] 没有 Azure 帐户，您可以[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)。 创建初学者应用程序和长达一个小时，播放获得，不需要信用卡，没有承诺。

<a name="create"></a>
## <a name="create-a-web-app"></a>创建 web 应用程序

1. 登录到[Azure 门户](https://portal.azure.com)使用 Azure 帐户。

2. 从左侧的菜单中，单击**新建** > **Web + 移动** > **Web 应用程序**。

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. 在应用程序创建刀片，用于新的应用程序中使用下列设置︰

    - **应用程序名称**︰ 键入唯一的名称。
    - **资源组**︰ 选择**新建**，然后为资源组指定一个名称。
    - **应用程序服务计划/位置**︰ 它要配置，请单击，然后单击**创建新**设置的名称、 位置和定价层的应用程序服务计划。 随意使用**自由**定价层。

    当您完成时，您的应用程序创建刀片式服务器应如下所示︰

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. 单击底部的**创建**。 您可以单击**通知**图标上方来查看进度。

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. 完成部署后，您应看到此通知消息。 单击该消息以打开部署的刀片式服务器。

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. 在**部署已成功**刀片式服务器，单击**资源**链接以打开新建 web 应用程序的刀片。

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>将 Java 应用程序部署到您的 web 应用程序

现在，让我们将 Java 应用程序部署到使用 FTPS Azure。

5. 在 web 应用程序刀片式服务器，向下滚动到**应用程序设置**或搜索，然后单击它。 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. 在**Java 版本**中，选择**Java 8** ，然后单击**保存**。

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    获取**已成功更新 web 应用程序设置**通知，请导航到 http://*&lt;应用程序名 >*。 azurewebsites.net 查看默认 JSP servlet 中操作。

7. 在 web 应用程序刀片式服务器，向下滚动到**部署凭据**或搜索它，然后单击它。

8. 设置您部署的凭据并单击**保存**。

7. 在 web 应用程序刀片式服务器，请单击**概述**。 旁边的**FTP/部署用户名**和**FTPS 主机名**，请单击**复制**按钮将复制这些值。

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    您现在准备好部署 FTPS 与 Java 应用程序。

8. 在 FTP/FTPS 客户机，登录到 Azure 的 web 应用程序的 FTP 服务器使用您在上一步中复制的值。 使用您在前面创建的部署密码。

    下面的屏幕快照显示了使用 FileZilla 中的日志记录。

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    您可能会看到从 Azure 的无法识别的 SSL 证书的安全警告。 继续，继续。

9. 单击[此链接](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war)可将 WAR 文件下载到您的本地计算机。

9. 在 FTP/FTPS 客户端，定位到**/site/wwwroot/webapps**的远程站点，并将在您的本地计算机上下载的 WAR 文件拖动到该远程目录。

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    单击**确定**以覆盖 Azure 中的文件。

    >[AZURE.NOTE] 根据 Tomcat 的默认行为，在 /site/wwwroot/webapps 中的文件名**ROOT.war**为您提供了根 web 应用程序 (http://*&lt;应用程序名 >*。 azurewebsites.net)，和文件名***&lt;anyname >*.war**为您提供了指定的 web 应用程序 (http://*&lt;应用程序名 >*.azurewebsites.net/*&lt;anyname >*)。

就是这样！ 您的 Java 应用程序开始运行实时 Azure 中。 在浏览器中，导航到 http://*&lt;应用程序名 >*。 azurewebsites.net 可以在操作中看到它。 

## <a name="make-updates-to-your-app"></a>对您的应用程序进行更新

无论何时需要进行更新，只是将新的 WAR 文件上载到 FTP/FTPS 客户端与同一个远程目录。

## <a name="next-steps"></a>下一步行动

[创建 Java web 应用程序从 Azure 市场中的模板](web-sites-java-get-started.md#marketplace)。 您可以获取自己完全可自定义的 Tomcat 容器并得到熟悉管理器用户界面。 

Azure 的 web 应用程序，直接在[IntelliJ](app-service-web-debug-java-web-app-in-intellij.md)或[Eclipse](app-service-web-debug-java-web-app-in-eclipse.md)中进行调试。

或者，用做更多您的第一个 web 应用程序。 例如︰

- 尝试[其他方法来部署您的代码到 Azure](../app-service-web/web-sites-deploy.md)。 
- 将 Azure 应用程序带到下一个级别。 对用户进行身份验证。 它基于请求的比例。 设置某些性能警报。 所有与单击几。 请参阅[与您的第一个 web 应用程序添加功能](app-service-web-get-started-2.md)。

