<properties 
    pageTitle="调试在 Eclipse 中的 Azure 上一个 Java Web 应用程序 |Microsoft Azure" 
    description="本教程展示如何使用 Eclipse 的 Azure Toolkit 调试在 Azure 上运行一个 Java Web 应用程序。" 
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

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>在 Eclipse 中的 Azure 上一个 Java Web 应用程序调试

本教程展示如何调试使用[Eclipse 的 Azure Toolkit]对 Azure 上运行一个 Java Web 应用程序。 为了简单起见，您将使用一个基本的 Java 服务器页 (JSP) 示例对于本教程，但在 Azure 上进行调试时，应类似 Java servlet 的步骤。

完成本教程后，您的应用程序将类似于下面的插图在 Eclipse 中调试时︰

![][01]
 
## <a name="prerequisites"></a>系统必备组件

* Java 开发人员工具箱 (JDK)，v 1.8 或更高版本。
* 日蚀 IDE 式的 Java EE 开发，靛蓝色或更高版本。 这可以从<http://www.eclipse.org/downloads/>下载。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat 或 Jetty 的分布。
* 还可以从<https://azure.microsoft.com/en-us/free/>或<http://azure.microsoft.com/pricing/purchase-options/>获得 Azure 订阅。
* 日蚀式 Azure Toolkit。 有关详细信息，请参阅[安装 Eclipse 的 Azure Toolkit]。
* 动态 Web 项目中创建并部署到 Azure 应用程序服务;有关示例，请参见[创建你好世界 Web 应用程序为在 Eclipse 中的 Azure]。

## <a name="to-debug-a-java-web-app-on-azure"></a>若要调试在 Azure 上的一个 Java Web 应用程序

若要完成此部分中的这些步骤，可以用作在 Azure 上一个 Java Web 应用程序已经部署现有动态 Web 项目，下载[示例动态 Web 项目]和执行中将其部署在 Azure 上[Hello 世界 Web 应用程序的 Azure 在 Eclipse 中创建]步骤。 

1. 打开 Eclipse。

1. 配置用于远程调试的超时︰

    1. 单击在 Eclipse，**窗口**菜单，然后单击**首选项**。
    1. 展开**Java**节点，然后选择**调试**。
    1. 配置**调试程序超时 （毫秒）**和**启动超时 （毫秒）**设置为`120000`。

        ![][02]

    1. 单击**确定**以关闭**首选项**对话框中。

1. 在 Eclipse 的项目资源管理器视图中，右键单击动态 Web 项目部署到 Azure。 显示上下文菜单时，选择**调试以**，，然后单击**Azure Web 应用程序**。

    ![][03]

1. 如果是第一次调试动态 Web 项目时，将打开**调试配置**对话框;您可以接受默认值所指定的**连接**选项卡上 Toolkit。 在**源**选项卡上，单击**添加**，然后选择**Java 项目**，选择**动态 Web 项目**，然后单击**确定**。 完成这些步骤后，请单击**调试**。

    ![][04]

1. 当系统提示**启用远程调试在远程 Web 应用程序现在？**，请单击**确定**。

1. 当提示**您的 web 应用程序现在就可以进行远程调试**时，请单击**确定**。

    ![][05]

1. **调试配置**对话框再次出现，请单击**调试**。

1. Windows 命令提示符或 Unix 外壳程序将打开，并准备必需的连接进行调试;您需要等待直到在继续操作之前成功连接到远程 Java Web 应用程序。 如果您使用的 Windows，它看起来如下图。

    ![][06]

1. 在 JSP 页面中，插入一个断点，然后为您的 Java Web 应用程序在浏览器中打开的 URL:

    1. 在 Eclipse 中的**Azure 资源管理器中**打开。
    1. 导航到**Web 应用程序**和您想要调试 Java Web 应用程序。
    1. 右键单击 Web 应用程序，然后单击**在浏览器中打开**。
    1. 日蚀式现在将进入调试模式。

## <a name="next-steps"></a>下一步行动

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心]。

有关创建 Azure Web 应用程序的其他信息，请参阅[Web 应用程序概述]。

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[日蚀式的 azure Toolkit]: ../azure-toolkit-for-eclipse.md
[日蚀式安装 Azure Toolkit]: ../azure-toolkit-for-eclipse-installation.md
[Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]: ./app-service-web-eclipse-create-hello-world-web-app.md
[示例动态 Web 项目]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Web 应用程序概述]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
