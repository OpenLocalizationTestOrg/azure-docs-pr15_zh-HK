<properties 
    pageTitle="调试中 IntelliJ Azure 上的一个 Java Web 应用程序 |Microsoft Azure" 
    description="本教程演示了如何使用 IntelliJ 的 Azure Toolkit 调试在 Azure 上运行一个 Java Web 应用程序。" 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>在 IntelliJ 中的 Azure 上一个 Java Web 应用程序调试

本教程展示如何调试使用[IntelliJ 的 Azure Toolkit]对 Azure 上运行一个 Java Web 应用程序。 为了简单起见，您将使用一个基本的 Java 服务器页 (JSP) 示例对于本教程，但在 Azure 上进行调试时，应类似 Java servlet 的步骤。

完成本教程后，您的应用程序将类似于下面的插图 IntelliJ 在调试时︰

![][01]
 
## <a name="prerequisites"></a>系统必备组件

* Java 开发人员工具箱 (JDK)，v 1.8 或更高版本。
* IntelliJ 的想法最终版。 这可以从<https://www.jetbrains.com/idea/download/index.html>下载。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat 或 Jetty 的分布。
* 还可以从<https://azure.microsoft.com/en-us/free/>或<http://azure.microsoft.com/pricing/purchase-options/>获得 Azure 订阅。
* IntelliJ Azure Toolkit。 有关详细信息，请参阅[安装的 IntelliJ Azure Toolkit]。
* 动态 Web 项目中创建并部署到 Azure 应用程序服务;有关示例，请参见[创建你好世界 Web 应用程序的 IntelliJ 在 Azure]。

## <a name="to-debug-a-java-web-app-on-azure"></a>若要调试在 Azure 上的一个 Java Web 应用程序

若要完成此部分中的这些步骤，可以用作在 Azure 上一个 Java Web 应用程序已经部署现有动态 Web 项目，下载[示例动态 Web 项目]和执行中将其部署在 Azure 上[创建你好世界 Web 应用程序在 IntelliJ Azure 的]步骤。 

1. 打开您部署到 Azure IntelliJ 中的 Java 的 Web 应用程序项目。

1. 单击**运行**菜单，然后单击**编辑配置...**。

    ![][02]

1. 当打开**运行/调试配置**对话框中︰ 

    1. 选择**Azure 的 Web 应用程序**。
    1. 单击**+**若要添加新的配置。
    1. 提供对配置的**名称**。
    1. 接受剩余的默认值通过 Azure Toolkit 中，建议的然后单击**确定**。

        ![][03]

1. 选择您刚创建的菜单右上 Azure Web 应用程序调试配置，然后单击在**调试**

    ![][04]

1. 当系统提示**启用远程调试在远程 Web 应用程序现在？**，请单击**确定**。

1. 当提示**您的 web 应用程序现在就可以进行远程调试**时，请单击**确定**。

    ![][05]

1. 选择您刚创建该菜单的右上的 Azure Web 应用程序调试配置，然后单击在**调试**。

1. Windows 命令提示符或 Unix 外壳程序将打开，并准备必需的连接进行调试;您需要等待直到在继续操作之前成功连接到远程 Java Web 应用程序。 如果您使用的 Windows，它看起来如下图︰

    ![][06]

1. 在 JSP 页面中，插入一个断点，然后为您的 Java Web 应用程序在浏览器中打开的 URL:

    1. 在 IntelliJ **Azure 资源管理器中**打开。
    1. 导航到**Web 应用程序**和您想要调试 Java Web 应用程序。
    1. 右键单击 Web 应用程序，然后单击**在浏览器中打开**。
    1. IntelliJ 现在将进入调试模式。

## <a name="next-steps"></a>下一步行动

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心]。

有关创建 Azure Web 应用程序的其他信息，请参阅[Web 应用程序概述]。

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[对于 IntelliJ azure Toolkit]: ../azure-toolkit-for-intellij.md
[安装 IntelliJ Azure Toolkit]: ../azure-toolkit-for-intellij-installation.md
[在 IntelliJ azure 创建呼叫世界 Web 应用程序]: ./app-service-web-intellij-create-hello-world-web-app.md
[示例动态 Web 项目]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Web 应用程序概述]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
