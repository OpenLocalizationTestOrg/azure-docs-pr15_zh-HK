<properties 
    pageTitle="添加到 Azure 应用程序服务 Web 应用程序的 Java 应用程序" 
    description="本教程展示如何添加页面或应用程序到 Azure 应用程序服务 Web 应用程序已配置为使用 Java 实例。" 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>添加到 Azure 应用程序服务 Web 应用程序的 Java 应用程序

一旦初始化您的 Java web 应用程序在[Azure 应用程序服务][]在[创建 Java web 应用程序在 Azure 应用程序服务中](web-sites-java-get-started.md)所述，可以上载您的应用程序通过在**webapps**文件夹中放置您的战争。

**Webapps**文件夹的导航路径不同取决于您如何设置 Web 应用程序实例。

- **Webapps**文件夹的路径如果使用 Azure 市场设置了您的 web 应用程序，请在窗体中是**d:\home\site\wwwroot\bin\application\_server\webapps**，其中**应用程序\_服务器**是作用于 Web 应用程序实例的应用程序服务器的名称。 
- 如果您设置了您的 web 应用程序使用 Azure 配置 UI， **webapps**文件夹的路径是在窗体**d:\home\site\wwwroot\webapps**。 

请注意，您可以使用源代码管理上载您的应用程序或 web 页中，包括[连续的集成方案](app-service-continuous-deployment.md)。 FTP 也是一个用于上载您的应用程序或 web 页的选项。

Tomcat web 应用程序的注意︰ 一旦您已上载到**webapps**文件夹的 WAR 文件，Tomcat 应用程序服务器会检测您已经添加并将自动加载。 请注意，是否将文件 （而不是 WAR 文件） 复制到根区目录，应用程序服务器需要重新启动才能使用这些文件。 在 Azure 上运行的 Tomcat Java web 应用程序的自动加载功能取决于所添加新的 WAR 文件或新文件或目录添加到**webapps**文件夹。 

## <a name="next-steps"></a>下一步行动

有关详细信息，请参见[Java 开发人员中心](/develop/java/)。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure 应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=529714
