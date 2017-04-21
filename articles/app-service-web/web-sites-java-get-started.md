<properties
    pageTitle="在 Azure 应用程序服务创建一个 Java web 应用程序 |Microsoft Azure"
    description="本教程展示如何将 Java web 应用程序部署到 Azure 应用程序服务。"
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>在 Azure 应用程序服务创建一个 Java web 应用程序

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

本教程展示如何使用[Azure 门户]创建 Java[在 Azure 应用程序服务 web 应用程序]。 Azure 门户是一个 web 界面，您可以使用管理 Azure 资源。

> [AZURE.NOTE] 若要完成本教程，您需要一个 Microsoft Azure 帐户。 如果您没有帐户，则可以[激活您的 Visual Studio 订户权益]或[注册免费试用版]。
>
> 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务]。 那里，可以立即短期初学者 web 应用程序中创建应用程序服务;没有信用卡是必需的并没有承诺。

## <a name="java-application-options"></a>Java 应用程序选项

有几种方法，您可以设置一个 Java 应用程序，应用程序服务 web 应用程序中。 

1. 创建一个应用程序，然后配置**应用程序设置**。

    应用程序服务提供几个 Tomcat 和 Jetty 版本，使用默认配置。 如果您将承载该应用程序将使用内置的版本之一，此方法设置 web 容器的最简单的方法，并很好，当您想要做的就是 war 文件上载到 web 容器。 对于此方法，您在 Azure 门户中，创建应用程序，然后转到您的应用程序，以选择您的 Java 版本以及所需的 Java web 容器的**应用程序设置**刀片式服务器。 当您使用此方法从程序文件都运行 Java 和 web 容器。 其他方法将 web 容器和 JVM 可能放在您的磁盘空间。 使用此模型时，您没有访问要编辑的文件在这一部分中的文件系统。 这意味着您无法执行操作，例如配置*server.xml*文件，或将库文件放在*/lib*文件夹。 有关详细信息，请参阅[创建和配置 Java web app](#appsettings)本教程的后面部分。
    
2. 从 Azure 市场使用的模板。

    Azure 市场包括自动创建并使用 Tomcat 或 Jetty web 容器配置 Java web 应用程序的模板。 可以配置的模板创建的 web 容器。 有关详细信息，请参阅本教程[使用 Java 模板从 Azure 市场](#marketplace)部分。
  
3. 创建一个应用程序，然后手动复制和编辑配置文件 

    您可能想要承载自定义的 Java 应用程序不会在任何应用程序服务提供的 web 容器中部署。 例如︰
    
    * Java 应用程序需要 Tomcat 或 Jetty 不直接支持的应用程序服务或在库中提供的版本。
    * Java 应用程序接受 HTTP 请求，并不会不为战争部署到已存在的 web 容器。
    * 您想要配置的 web 容器从头自己。 
    * 您想要使用的应用程序服务中不受支持的 Java 版本并且想要将其上传您自己。

    像这样的情况下，可以创建一个应用程序使用 Azure 门户，然后手动提供适当的运行时文件。 在这种情况下，文件将对您的应用程序服务计划存储空间配额计算在内。 有关详细信息，请参阅[将上载到 Azure 自定义 Java web 应用程序]。

## <a name="portal"></a>创建和配置 Java web 应用程序

本节演示如何创建一个 web 应用程序并将其配置为 Java 使用门户的**应用程序设置**刀片式服务器。

1. 登录到[Azure 的门户]。

2. 单击**新 > Web + 移动 > Web 应用程序**。

    ![新的 Web 应用程序][newwebapp]

4. 在**Web 应用程序**框中输入的 web 应用程序的名称。

    此名称必须是唯一的 azurewebsites.net 域中，因为 web 应用程序的 URL 将 {name}。 azurewebsites.net。 如果您输入的名称不唯一，文本框中将显示一个红色感叹号。

5. 选择一个**资源组**，或创建一个新。

    有关资源组的详细信息，请参阅[使用 Azure 门户管理 Azure 的资源]。

6. 选择某个**应用程序服务计划/位置**或新建一个。

    有关应用程序服务计划的详细信息，请参阅[Azure 应用程序服务计划概述]。

7. 单击**创建**。

    ![创建 Web 应用程序][newwebapp2]
 
8. 当创建 web 应用程序后时，请单击**Web 应用程序 > {您 web 应用程序}**。
 
    ![选择 Web 应用程序][selectwebapp]

9. 在**Web 应用程序**刀片式服务器，单击**设置**。

10. 单击**应用程序设置**。

11. 选择所需的**Java 版本**。 

12. 选择所需的**Java 的次要版本**。 如果选择**升序**，您的应用程序将使用应用程序服务中该 Java 主要版本提供的最新次要版本。 **最新**项目是唯一的 Java 应用程序创建的**应用程序设置**。 如果从库中创建您的 Java 应用程序，然后您需要管理自己的容器和 JVM 更改。 

12. 选择所需的**Web 容器**。 如果您选择启动具有**最新**的容器名称，您的应用程序将保留最新版本的应用程序服务中提供该 web 容器主要版本。 

    ![Web 容器版本][versions]

13. 单击**保存**。

    几分钟后，在您的 web 应用程序将成为基于 Java 的并配置为使用您选择的 web 容器。

14. 单击**Web 应用程序 > {新 web 应用程序}**。

15. 单击以浏览到新网站的**URL** 。

    该网页将确认您已创建了一个基于 Java 的 web 应用程序。

## <a name="marketplace"></a>从 Azure 市场使用 Java 模板

本节说明如何使用 Azure 市场创建一个 Java web 应用程序。 此外可以使用相同的常规流来创建基于 Java 的移动或 API 的应用程序。 

1. 登录到[Azure 门户]

2. 单击**新 > 市场**。

    ![新市场][newmarketplace]

3. 请单击**Web + 移动**。

    您可能需要向左滚动以查看**市场**刀片式服务器，您可以在其中选择**Web + 移动**。

4. 在搜索文本框中，输入的 Java 应用程序服务器，如**Apache Tomcat**或**Jetty**，名称，然后按 enter 键。

5. 在搜索结果中，单击 Java 应用程序服务器。

    ![Web 移动 Jetty][webmobilejetty]

6. 在第一个**Apache Tomcat**或**Jetty**刀片式服务器，请单击**创建**。

    ![Jetty 门户刀片][jettyblade]

7. 在下一步的**Apache Tomcat**或**Jetty**刀片式服务器，请输入在**Web 应用程序**中的 web 应用程序的名称。

    此名称必须是唯一的 azurewebsites.net 域中，因为 web 应用程序的 URL 将 {name}。 azurewebsites.net。 如果您输入的名称不唯一，文本框中将显示一个红色感叹号。

8. 选择一个**资源组**，或创建一个新。

    有关资源组的详细信息，请参阅[使用 Azure 门户管理 Azure 的资源]。

9. 选择某个**应用程序服务计划/位置**或新建一个。

    有关应用程序服务计划的详细信息，请参阅[Azure 应用程序服务计划概述]。

10. 单击**创建**。

    ![Jetty 门户网站创建][jettyportalcreate2]

    短时间内，通常小于一分钟，Azure 完成创建新的 web 应用程序。

11. 单击**Web 应用程序 > {新 web 应用程序}**。

12. 单击以浏览到新网站的**URL** 。

    ![Jetty URL][jettyurl]

    Tomcat 附带了一组默认的页面; 例如︰如果您选择了 Tomcat，您会看到与以下示例类似的页。

    ![使用 Apache Tomcat 的 web 应用程序][tomcat]

    如果您选择 Jetty，您会看到与以下示例类似的页。 Jetty 没有默认页集合，因此此处重复使用空的 Java 网站都使用同一个 JSP。

    ![Web 应用程序使用 Jetty][jetty]

现在，已经使用的应用程序容器创建 web 应用程序，请参阅有关如何上载到 web 应用程序应用程序信息的[后续步骤](#next-steps)部分。

## <a name="next-steps"></a>下一步行动

在这种情况下，可以在 Azure 应用程序服务 web 应用程序中运行的 Java 应用程序服务器。 要将您自己的代码部署到 web 应用程序，请参阅[添加应用程序或网页到您的 Java web 应用程序]。

有关开发在 Azure 中的 Java 应用程序的详细信息，请参见[Java 开发人员中心]。

<!-- URL List -->

[将应用程序或网页添加到您的 Java web 应用程序]: ./web-sites-java-add-app.md
[Azure 应用程序服务计划概述]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure 门户]: https://portal.azure.com/
[激活您的 Visual Studio 订户权益]: http://go.microsoft.com/fwlink/?LinkId=623901
[注册免费试用版]: http://go.microsoft.com/fwlink/?LinkId=623901
[试用应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=523751
[在 Azure 应用程序服务 web 应用程序]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java 开发人员中心]: /develop/java/
[使用 Azure 门户管理 Azure 资源]: ../azure-portal/resource-group-portal.md
[将自定义的 Java web 应用程序上传到 Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
