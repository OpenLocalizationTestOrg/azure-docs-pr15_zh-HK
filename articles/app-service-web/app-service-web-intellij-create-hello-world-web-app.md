<properties 
    pageTitle="在 IntelliJ azure 创建呼叫世界 Web 应用程序 |Microsoft Azure" 
    description="本教程演示了如何使用 IntelliJ 的 Azure Toolkit 创建你好世界 Web 应用程序的 Azure。" 
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
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>在 IntelliJ azure 创建呼叫世界 Web 应用程序

本教程展示如何创建和部署基本 Hello World 应用到 Azure 作为 Web 应用程序使用[IntelliJ 的 Azure Toolkit]。 一个基本的 JSP 示例显示为简单起见，但非常相似的步骤就适用于 Java servlet，来说是 Azure 的部署。

完成本教程后，您的应用程序将类似于下面的插图在 web 浏览器中查看时︰

![][01]
 
## <a name="prerequisites"></a>系统必备组件

* Java 开发人员工具箱 (JDK)，v 1.8 或更高版本。
* IntelliJ 的想法最终版。 这可以从<https://www.jetbrains.com/idea/download/index.html>下载。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat 或 Jetty 的分布。
* 还可以从<https://azure.microsoft.com/free/>或<http://azure.microsoft.com/pricing/purchase-options/>获得 Azure 订阅。
* IntelliJ Azure Toolkit。 有关详细信息，请参阅[安装的 IntelliJ Azure Toolkit]。

## <a name="to-create-a-hello-world-application"></a>若要创建一个 Hello World 应用程序

首先，我们将首先创建一个 Java 项目。

1. 启动 IntelliJ，并在菜单**文件**，单击，然后单击**新建**，然后单击**项目**。

   ![][02]

1. 在新建项目对话框中，选择**Java**，然后为**Web 应用程序**，，然后单击**下一步**。

   ![][03a]

   如果系统提示您不继续分配任何 SDK，请单击**是**。

   ![][03b]

1. 出于本教程的目的，项目**Java 的 Web-应用程序-在-Azure**，命名，然后单击**完成**。

   ![][04]

1. 在 IntelliJ 的项目资源管理器视图中，展开**Java 的 Web-应用程序-在-Azure**，然后展开**web**，然后双击**index.jsp**。

   ![][05]

1. Index.jsp 文件打开时在 IntelliJ 中，在要动态显示的文字添加**Hello World ！** 在现有的`<body>`元素。 您更新`<body>`内容应类似于下面的示例︰

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. 保存 index.jsp。

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>若要部署到 Azure 的 Web 应用程序容器应用程序

有几种方法，通过它您可以部署到 Azure Java web 应用程序。 本教程介绍一个最简单的︰ 应用程序将部署到 Azure Web 应用程序容器-所需任何特殊项目类型或其他工具。 JDK 和 web 容器软件将为您提供通过 Azure，因此无需上载您自己;您只需要为您的 Java Web 应用程序。 因此，您的应用程序的发布过程将秒，不分钟。

1. 在 IntelliJ 的项目资源管理器中，右键单击**Java 的 Web-应用程序-在-Azure**项目。 显示上下文菜单时，选择**Azure**，，然后单击**发布为 Azure Web 应用程序...**

   ![][06]

1. 如果没有已注册到 Azure 从 IntelliJ，将提示您登录到您的 Azure 帐户︰

   ![][07]

   注意︰ 如果您有多个 Azure 帐户，某些期间注册过程的提示可能会显示一次以上，即使他们看起来是相同的。 这种情况下，继续按照中说明的符号。

1. 您已经成功地注册到 Azure 帐户后，**管理订阅**对话框将显示与您的凭据的预订列表。 如果列出多个订阅，并且想要使用它们的一个特定子集，可能还可以取消选中要使用的。 选择了您的订阅，请单击**关闭**。

   ![][08]

1. 当出现**部署到 Azure Web 应用程序容器**对话框时，它会显示任何 Web 应用程序容器，您已经创建。如果尚未创建任何容器，则该列表将为空。   

   ![][09]

1. 如果您没有创建 Azure Web 应用程序容器之前，或者如果您想要发布到新的容器应用程序，使用以下步骤。 否则为选择现有的 Web 应用程序容器并跳至下面的步骤 6。

  1. 单击**+**

        ![][10]

  1. 将显示**新建 Web 应用程序容器**对话框，它将用于接下来的几个步骤。

        ![][11]

  1. 为您的 Web 应用程序容器; 输入一个**DNS 标签**这将构成 Azure 中的 web 应用程序的主机 URL 的叶 DNS 标签。 注意︰ 名称必须可用并且符合 Azure 命名要求的 Web 应用程序。

  1. 在**Web 容器**下拉列表菜单中，选择适当的软件应用程序。

        目前，您可以选择从 Tomcat 8、 Tomcat 7 或 Jetty 9。 将通过 Azure，提供所选软件的最新分布，它将运行 JDK 8 由 Oracle 和 Azure 所提供的新分布。

  1. 在**订阅**下拉列表菜单中，选择要用于此部署的订阅。

  1. 在**资源组**下拉列表菜单中，选择您想要将您的 Web 应用程序相关联的资源组。

        注意︰ Azure 资源组允许您这样，例如，可以删除它们协同相关的资源组合在一起。

        您可以选择现有资源组 （如果有） 和跳到步骤 g 下，或使用的以下这些步骤来创建新的资源组︰

      * 单击**新...**

      * 将显示**新建资源组**对话框中︰

            ![][12]

      * 在**名称**文本框中，指定新资源组的名称。

      * 在**地区**下拉菜单中，选择适当的 Azure 数据中心资源组的位置。

      * 单击**确定**。

  1. **应用程序服务计划**下拉菜单中列出与所选资源组相关联的应用程序服务计划。

        注意︰ 应用程序服务计划指定的 Web 应用程序、 定价层和计算实例的大小的信息，如位置。 单个应用程序服务规划可以用于多个 Web 应用程序，这是从特定的 Web 应用程序部署单独维护的原因。

        可以选择现有应用程序服务计划 （如果有） 和跳到步骤 h，或使用的下列这些步骤来创建新的应用程序服务计划︰

      * 单击**新...**

      * 将显示**新建应用程序服务计划**对话框中︰

            ![][13]

      * 在**名称**文本框中，指定您新的应用程序服务计划的名称。

      * 在中，选择**位置**下拉菜单中适当的 Azure 数据中心计划的位置。

      * 在**定价层**下拉菜单中，选择适当的定价计划。 出于测试目的，您可以选择**自由**。

      * 在**实例大小**下拉菜单中，选择相应的实例大小的计划。 出于测试目的，您可以选择**小**。

  1. 完成所有上述步骤操作后，新的 Web 应用程序容器对话框应类似于下图︰

        ![][14]

  1. 单击**确定**完成创建新的 Web 应用程序容器。

        等待几秒钟的 Web 应用程序容器的列表，刷新，和现在应从列表中选择您新创建的 web 应用程序容器。

1. 现在您可以完成初始部署 Web 应用程序到 Azure。单击**确定**以部署到所选的 Web 应用程序容器的 Java 应用程序。

    ![][15]

    注意︰ 默认情况下，您的应用程序将部署为应用程序服务器的子目录。 如果您希望其作为根应用程序进行部署，然后单击**确定**检查**到根部署**复选框。

1. 接下来，您应该看到**Azure 活动日志**视图中，将指示您的 Web 应用程序的部署状态。

    ![][16]

    将您的 Web 应用程序部署到 Azure 的进程应在几秒钟之内完成。 当您应用程序已准备好，您会看到一个名为**状态**列中的**已发布**链接。 当您单击链接时，它将指导您为您已部署的 Web 应用程序的主页上，或者可以使用下一节中的步骤以浏览到您的 web 应用程序。

## <a name="browsing-to-your-web-app-on-azure"></a>浏览到您的 Web 应用程序在 Azure 上

到配置到您的 Web 应用程序在 Azure 上浏览器，您可以使用**Azure 浏览器**视图。

如果**Azure 浏览器**视图尚未打开，您可以打开它然后单击 IntelliJ，在**视图**菜单上然后**工具窗口**，请单击，然后单击**服务资源管理器**。 如果您以前未记录，会提示您这样做。

**Azure 浏览器**视图显示时，使用请按照以下步骤停止您的 Web 应用程序︰ 

1. 展开**Azure**节点。

1. 展开**Web 应用程序**节点。 

1. 用鼠标右键单击所需的 Web 应用程序。

1. 上下文菜单显示时，单击**用浏览器打开**。

    ![][17]

## <a name="updating-your-web-app"></a>更新您的 Web 应用程序

更新现有的运行 Azure Web 应用程序是一个快速而简单的过程，并且有两个选项可用于更新︰

* 您可以更新现有的 Java Web 应用程序的部署。
* 您可以发布到相同的 Web 应用程序容器的其他 Java 应用程序。

在任一情况下，是完全相同的过程，只需要几秒钟︰

1. 在 IntelliJ 项目资源管理器中，右键单击要更新或添加到现有的 Web 应用程序容器的 Java 应用程序。

1. 显示上下文菜单时，选择**Azure** ，然后**发布为 Azure Web 应用程序...**

1. 由于您已经登录以前，您将看到您现有的 Web 应用程序容器的列表。 选择要发布或重新发布到 Java 应用程序，然后单击**确定**。

几秒钟之后， **Azure 活动日志**视图将显示您**已发布**的更新的部署，您将无法验证您的 web 浏览器中更新的应用程序。

## <a name="starting-or-stopping-an-existing-web-app"></a>启动或停止对现有的 Web 应用程序

要启动或停止现有 Azure Web 应用程序容器，（包括所有已部署的 Java 应用程序中），可以使用**Azure 浏览器**视图。

如果**Azure 浏览器**视图尚未打开，您可以打开它然后单击 IntelliJ，在**视图**菜单上然后**工具窗口**，请单击，然后单击**服务资源管理器**。 如果您以前未记录，会提示您这样做。

**Azure 浏览器**视图显示时，使用请按照以下步骤启动或停止您的 Web 应用程序︰ 

1. 展开**Azure**节点。

1. 展开**Web 应用程序**节点。 

1. 用鼠标右键单击所需的 Web 应用程序。

1. 上下文菜单显示时，单击**启动**或**停止**。 注意菜单选项是上下文感知的因此只能停止一个正在运行的 web 应用程序，或启动 web 应用程序的当前未运行。

    ![][18]

## <a name="next-steps"></a>下一步行动

针对 Java Ide Azure 工具包的详细信息，请参阅以下链接︰

- [日蚀式的 azure Toolkit]
  - [日蚀式安装 Azure Toolkit]
  - [Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]
  - [什么是 Azure Toolkit 的 Eclipse 中的新功能]
- [对于 IntelliJ azure Toolkit]
  - [安装 IntelliJ Azure Toolkit]
  - *IntelliJ （本文） 在 azure 创建 Hello 世界 Web 应用程序*
  - [什么是 Azure Toolkit 的 IntelliJ 中的新增功能]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心]。

有关创建 Azure Web 应用程序的其他信息，请参阅[Web 应用程序概述]。

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[日蚀式的 azure Toolkit]: ../azure-toolkit-for-eclipse.md
[对于 IntelliJ azure Toolkit]: ../azure-toolkit-for-intellij.md
[Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[日蚀式安装 Azure Toolkit]: ../azure-toolkit-for-eclipse-installation.md
[安装 IntelliJ Azure Toolkit]: ../azure-toolkit-for-intellij-installation.md
[什么是 Azure Toolkit 的 Eclipse 中的新功能]: ../azure-toolkit-for-eclipse-whats-new.md
[什么是 Azure Toolkit 的 IntelliJ 中的新增功能]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Web 应用程序概述]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
