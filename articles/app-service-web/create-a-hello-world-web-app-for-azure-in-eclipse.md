<properties 
    pageTitle="Azure 在 Eclipse 中创建一个照会世界 Web 应用程序" 
    description="本教程展示如何使用 Eclipse 的 Azure Toolkit 创建你好世界 Web 应用程序的 Azure。" 
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

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Azure 在 Eclipse 中创建一个照会世界 Web 应用程序

本教程展示如何创建和部署基本 Hello World 应用到 Azure 作为 Web 应用程序通过使用[Eclipse 的 Azure Toolkit]。 一个基本的 JSP 示例显示为简单起见，但非常相似的步骤就适用于 Java servlet，来说是 Azure 的部署。

完成本教程后，您的应用程序将类似于下面的插图在 web 浏览器中查看时︰

![][01]
 
## <a name="prerequisites"></a>系统必备组件

* Java 开发人员工具箱 (JDK)，v 1.7 或更高版本。
* 日蚀 IDE 式的 Java EE 开发，靛蓝色或更高版本。 这可以从<http://www.eclipse.org/downloads/>下载。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat 或 Jetty 的分布。
* 还可以从<https://azure.microsoft.com/en-us/free/>或<http://azure.microsoft.com/pricing/purchase-options/>获得 Azure 订阅。
* 日蚀式 Azure Toolkit。 有关详细信息，请参阅[安装 Eclipse 的 Azure Toolkit]。

## <a name="to-create-a-hello-world-application"></a>若要创建一个 Hello World 应用程序

首先，我们将首先创建一个 Java 项目。

1. 启动 Eclipse，并在菜单上单击**文件**，单击**新建**，然后单击**动态 Web 项目**。 (如果看不到**动态 Web 项目**列为可用的项目，单击**文件**和**新建**后，再执行下列操作︰ 单击**文件**，单击**新建**，单击**项目...**，展开**Web**、**动态 Web 项目**，请单击，单击**下一步**。)

1. 出于本教程的需要， **MyHelloWorld**为项目命名。 屏幕将类似于以下内容︰

    ![][02]

1. 单击**完成**。

1. 在 Eclipse 的项目资源管理器视图中，展开**MyHelloWorld**。 用鼠标右键单击**网站**，单击**新建**，然后单击**JSP 文件**。

1. 在**新的 JSP 文件**对话框中，命名文件**index.jsp**。 将父文件夹保留为**MyHelloWorld/网站**。

1. 在**选择 JSP 模板**对话框中，出于本教程的目的选择**新的 JSP 文件 (html)**，，然后单击**完成**。

1. 要动态显示的文本中添加的 index.jsp 文件打开时在 Eclipse 中， **Hello World ！** 在现有的`<body>`元素。 您更新`<body>`内容应类似于下面的示例︰

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. 保存 index.jsp。

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>若要部署到 Azure 的 Web 应用程序容器应用程序

有几种方法，通过它您可以部署到 Azure Java web 应用程序。 本教程介绍一个最简单的︰ 应用程序将部署到 Azure Web 应用程序容器-所需任何特殊项目类型或其他工具。 JDK 和 web 容器软件将为您提供通过 Azure，因此无需上载您自己;您只需要为您的 Java Web 应用程序。 因此，您的应用程序的发布过程将秒，不分钟。

1. 在 Eclipse 的项目资源管理器中，右键单击**MyHelloWorld**。

1. 在上下文菜单中，选择**Azure**，然后单击**发布为 Azure Web 应用程序...**

    ![][03]
   
    或者，在项目资源管理器中选择了 web 应用程序项目中，可以单击工具栏上的**发布**下拉按钮，并从那里选择**发布 Azure Web 应用程序为**︰
   
    ![][14]
   
1. 如果没有已注册到 Azure 从 Eclipse，将提示您登录到您的 Azure 帐户︰

    ![][04]
   
    注意︰ 如果您有多个 Azure 帐户，某些期间注册过程的提示可能会显示一次以上，即使他们看起来是相同的。 这种情况下，继续按照中说明的符号。
1. 您已经成功地注册到 Azure 帐户后，**管理订阅**对话框将显示与您的凭据的预订列表。 如果列出多个订阅，并且想要使用它们的一个特定子集，可能还可以取消选中要使用的。 选择了您的订阅，请单击**关闭**。

    ![][05]
   
1. 当出现**部署到 Azure Web 应用程序容器**对话框时，它会显示任何 Web 应用程序容器，您已经创建。如果尚未创建任何容器，则该列表将为空。

    ![][06]
   
1. 如果您没有创建 Azure Web 应用程序容器之前，或者如果您想要发布到新的容器应用程序，使用以下步骤。 否则为选择现有的 Web 应用程序容器并跳至下面的步骤 7。

    1. 单击**新...**

    1. 将显示**新建 Web 应用程序容器**对话框中︰

        ![][07]

    1. 为您的 Web 应用程序容器; 输入一个**DNS 标签**这将构成 Azure 中的 web 应用程序的主机 URL 的叶 DNS 标签。 注意︰ 名称必须可用并且符合 Azure 命名要求的 Web 应用程序。

    1. 在**Web 容器**下拉列表菜单中，选择适当的软件应用程序。

        目前，您可以选择从 Tomcat 8、 Tomcat 7 或 Jetty 9。 将通过 Azure，提供所选软件的最新分布，它将运行 JDK 8 由 Oracle 和 Azure 所提供的新分布。

    1. 在**订阅**下拉列表菜单中，选择要用于此部署的订阅。

    1. 在**资源组**下拉列表菜单中，选择您想要将您的 Web 应用程序相关联的资源组。

        注意︰ Azure 资源组允许您这样，例如，可以删除它们协同相关的资源组合在一起。

        您可以选择现有资源组 （如果有） 和跳到步骤 g 下，或使用的以下这些步骤来创建新的资源组︰

        * 单击**新...**

        * 将显示**新建资源组**对话框中︰

            ![][08]

        * 在**名称**文本框中，指定新资源组的名称。

        * 在**地区**下拉菜单中，选择适当的 Azure 数据中心资源组的位置。

        * 单击**确定**。

    1. **应用程序服务计划**下拉菜单中列出与所选资源组相关联的应用程序服务计划。

        注意︰ 应用程序服务计划指定的 Web 应用程序、 定价层和计算实例的大小的信息，如位置。 单个应用程序服务规划可以用于多个 Web 应用程序，这是从特定的 Web 应用程序部署单独维护的原因。

        可以选择现有应用程序服务计划 （如果有） 和跳到步骤 h，或使用的下列这些步骤来创建新的应用程序服务计划︰

        * 单击**新...**

        * 将显示**新建应用程序服务计划**对话框中︰

            ![][09]

        * 在**名称**文本框中，指定您新的应用程序服务计划的名称。

        * 在中，选择**位置**下拉菜单中适当的 Azure 数据中心计划的位置。

        * 在**定价层**下拉菜单中，选择适当的定价计划。 出于测试目的，您可以选择**自由**。

        * 在**实例大小**下拉菜单中，选择相应的实例大小的计划。 出于测试目的，您可以选择**小**。

    1. 完成所有上述步骤操作后，新的 Web 应用程序容器对话框应类似于下图︰

        ![][10]

    1. 单击**确定**完成创建新的 Web 应用程序容器。

        等待几秒钟的 Web 应用程序容器的列表，刷新，和现在应从列表中选择您新创建的 web 应用程序容器。

1. 现在您可以完成您的 Web 应用程序到 Azure 的初始部署︰

    ![][11]

    单击**确定**以部署到所选的 Web 应用程序容器的 Java 应用程序。

    注意︰ 默认情况下，您的应用程序将部署为应用程序服务器的子目录。 如果您希望其作为根应用程序进行部署，然后单击**确定**检查**到根部署**复选框。

1. 接下来，您应该看到**Azure 活动日志**视图中，将指示您的 Web 应用程序的部署状态。

    ![][12]

    将您的 Web 应用程序部署到 Azure 的进程应在几秒钟之内完成。 当您应用程序已准备好，您会看到一个名为**状态**列中的**已发布**链接。 当您单击该链接时，您将部署的 Web 应用程序的主页上。

## <a name="updating-your-web-app"></a>更新您的 Web 应用程序

更新现有的运行 Azure Web 应用程序是一个快速而简单的过程，并且有两个选项可用于更新︰

* 您可以更新现有的 Java Web 应用程序的部署。
* 您可以发布到相同的 Web 应用程序容器的其他 Java 应用程序。

在任一情况下，是完全相同的过程，只需要几秒钟︰

1. 在 Eclipse 项目资源管理器中，右键单击要更新或添加到现有的 Web 应用程序容器的 Java 应用程序。

2. 显示上下文菜单时，选择**Azure** ，然后**发布为 Azure Web 应用程序...**

3. 由于您已经登录以前，您将看到您现有的 Web 应用程序容器的列表。 选择要发布或重新发布到 Java 应用程序，然后单击**确定**。

几秒钟之后， **Azure 活动日志**视图将显示您**已发布**的更新的部署，您将无法验证您的 web 浏览器中更新的应用程序。

## <a name="stopping-an-existing-web-app"></a>停止对现有的 Web 应用程序

若要停止现有 Azure Web 应用程序容器，（包括所有已部署的 Java 应用程序中），您可以使用**Azure 浏览器**视图。

如果**Azure 浏览器**视图尚未打开，您可以打开它，然后单击**窗口**菜单在 Eclipse 中，然后单击**显示视图**中，然后**另...**，然后**Azure**，，然后单击**Azure 资源管理器**。 如果您以前未记录，会提示您这样做。

**Azure 浏览器**视图显示时，使用请按照以下步骤停止您的 Web 应用程序︰ 

1. 展开**Azure**节点。

1. 展开**Web 应用程序**节点。 

1. 用鼠标右键单击所需的 Web 应用程序。

1. 上下文菜单显示时，单击**停止**。

    ![][13]

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅以下链接︰

* [Java 开发人员中心](/develop/java/)。
* [Web 应用程序概述](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png