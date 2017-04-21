<properties
    pageTitle="在 Eclipse 中创建呼叫世界 Azure 的云服务"
    description="了解如何创建使用 Eclipse Azure Toolkit 简单 Hello World 应用程序。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>在 Eclipse 中创建呼叫世界 Azure 的云服务 #

以下步骤介绍如何创建和部署基本 JSP 应用程序到 Azure 使用 Azure Toolkit 的 Eclipse。 JSP 示例显示为简单起见，但非常相似的步骤就适用于 Java servlet，来说是 Azure 的部署。

应用程序将类似于以下︰

![][ic600360]

## <a name="prerequisites"></a>系统必备组件 ##

* Java 开发人员工具箱 (JDK)，v 1.7 或更高版本。
* 日蚀 IDE 式的 Java EE 开发，靛蓝色或更高版本。 这可以从<http://www.eclipse.org/downloads/>下载。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat、 GlassFish，JBoss 应用服务器、 Jetty 或 IBM® WebSphere® 应用程序服务器自由核的分布。
* 还可以从<http://azure.microsoft.com/pricing/purchase-options/>获得 Azure 订阅。
* 日蚀式 Azure Toolkit。 有关详细信息，请参阅[安装 Eclipse 的 Azure Toolkit][]。

## <a name="to-create-a-hello-world-application"></a>若要创建一个 Hello World 应用程序 ##

首先，我们将首先创建一个 Java 项目。

*  启动 Eclipse，并在菜单上单击**文件**，单击**新建**，然后单击**动态 Web 项目**。 (如果看不到**动态 Web 项目**列为可用的项目后，单击**文件****新建**，然后执行下列操作︰ 单击**文件**，单击**新建**，单击**项目...**，展开**Web**、**动态 Web 项目**，请单击，单击**下一步**。)
*  出于本教程的需要， **MyHelloWorld**为项目命名。 (确保您使用了此名称，在本教程中的后续步骤希望 WAR 文件命名为 MyHelloWorld)。 屏幕将类似于以下内容︰ ![][ic589576]
* 单击**完成**。
* 在 Eclipse 的项目资源管理器视图中，展开**MyHelloWorld**。 用鼠标右键单击**网站**，单击**新建**，然后单击**JSP 文件**。
* 在**新的 JSP 文件**对话框中，命名文件**index.jsp**。 保持与**MyHelloWorld/网站**，父文件夹如下所示︰  ![][ic659262]
* 在**选择 JSP 模板**对话框中，出于本教程的目的选择**新的 JSP 文件 (html)** ，然后单击**完成**。
* 当 index.jsp 文件打开在 Eclipse 中时，添加要动态显示的文本中**Hello World ！** 在现有的`<body>`元素。 您更新`<body>`的内容应显示如下︰
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* 保存 index.jsp。

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>您将应用部署到 Azure，快速而简单的方法 ##

一旦您有一个 Java web 应用程序进行测试，可以使用下列快捷方式若要直接在 Azure 的云上试一下。

1. 在 Eclipse 的项目资源管理器中，单击**MyHelloWorld**。
1. 在 Eclipse 工具栏中，单击**发布**下拉按钮，然后单击**发布为 Azure 云服务**
    ![][publishDropdownButton]
1. 如果您要发布到 Azure 此应用程序，第一次，您没有创建此应用程序之前 Azure 的部署项目，Azure 部署项目为您自动创建。 您应看到下面的提示，还列出了 JDK 软件包和应用程序服务器将自动部署运行您的应用程序。
    ![][ic789598]

    此快捷方式方法使快速方便地在 Azure 中测试应用程序，而无需配置一个特定的服务器或不同于默认设置的 JDK。 如果您认为用默认值，可以单击**确定**以继续执行以下步骤。
    但是，如果您想要更改 JDK 或应用程序服务器，以使用您的应用程序，通过编辑时自动为您创建，Azure 的部署项目，可以以后执行，也可以立即单击**取消**并阅读本教程的**有关 Azure 部署项目部分**。
1. 在**发布到 Azure**对话框中︰
    1. 如果尚未选择**订阅**中没有订阅，请按照下列步骤操作以导入您的订阅信息︰
        1. 单击**发布设置文件中的导入**。
        1. 在**导入订阅信息**对话框中，单击**下载发布设置文件**。 如果您还没有登录到 Azure 帐户，系统将提示您登录。 然后系统将提示您保存 Azure 发布设置文件。 将其保存到本地计算机。
        1. 仍在**导入订阅信息**对话框中，单击**浏览**按钮，选择您在前面步骤中本地保存的发布设置文件，然后单击**打开**。 您的屏幕应该类似于以下︰ ![][ic644267]
        1. 单击**确定**。
    1. 对于**订阅**，选择您想为您的部署使用的订阅。
    1. 对于**存储帐户**，请选择所需的存储帐户或单击**新建**以创建新的存储帐户。
    1. 选择所需的云服务**服务名称**，或单击**新建**以创建新的云服务。
    1. **目标操作系统**中，选择您想要为您的部署使用的操作系统的版本。
    1. 为**目标环境**，用于本教程中，选择**转移**。 （当您准备好部署到您的生产站点时，您将更改此到**生产**。）
    1. 可选︰ 确保**覆盖以前的部署**检查，如果您希望新部署自动覆盖以前的部署。 启用此选项时，您将体验"409 冲突"的问题，发布到相同的位置时。
        注意**到 Azure 发布**对话框包含一个部分进行**远程访问**。 默认情况下未启用远程访问，我们不会为此示例启用它。 要启用远程访问，您将输入用户名和密码才能使用远程登录时。 有关远程访问的详细信息，请参阅[在 Eclipse 中的 Azure 部署启用远程访问][]。
        **发布到 Azure**对话框将显示与以下类似︰ ![][ic719488]
1. 单击**发布**来发布到暂存环境。
    当系统提示您执行完全创建，请单击**是**。 这可能需要几分钟的第一个生成的时间。
    **Azure 活动日志**将显示在选项卡式的 Eclipse 的视图部分中。
    ![][ic719489]
   可以使用此日志，以及**控制台**视图来查看您的部署的进度。 另一种方法是登录到[Azure 管理门户网站][]，并使用**云服务**部分中监视的状态。
1. 当成功部署部署时， **Azure 活动日志**将显示状态为**已发布**。 单击**发布**，下面的图像所示，您的浏览器将打开您的部署的实例。
    ![][ic719490]

这是部署到临时环境，因为 DNS 名称将窗体 http://&lt;*guid*&gt;。 cloudapp.net 和 URL 将包含 DNS 名称加上后缀为您的应用程序。 例如，http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld。 （ **MyHelloWorld**部分是区分大小写）。如果您单击 Azure 平台管理门户 （在管理门户的云服务部分中） 中的部署名称，您还可以查看 DNS 名称。

尽管此预排已部署到临时环境，部署到生产**发布到 Azure**对话框内除遵循相同的步骤，选择而不是**转移****生产**的**目标环境**。 部署到生产将导致基于您的选择，而不是 GUID 为临时使用的 DNS 名称的 URL。

>[AZURE.WARNING] 目前已经部署到云 Azure 应用程序。 但是，在继续之前，意识到，一个已部署的应用程序，即使它没有运行，将继续累算计费时间为您的订阅。 因此，是极为重要的删除不需要的部署，从 Azure 订阅。

## <a name="about-azure-deployment-projects"></a>关于 Azure 部署项目 ##

为了部署到 Azure 的一个或多个 Java 应用程序，需要一个 Azure 部署项目。 播放您的应用程序需要包装到 Azure 上发布以"包"的角色。

有关您的应用程序的信息，除了 Azure 部署项目还包含您的部署的其他关键组件的信息最重要的是︰ 若要运行您的 web 应用程序中，应用程序服务器容器和 Java 运行时上运行它。 Azure 支持大量的 Java 运行时，您可以从 Java 应用程序服务器。

尽管这里使用的示例得到了大大简化为教学目的，Azure 部署项目还可以包含其他重要的配置信息，使您可以创建与您的应用程序几乎任意复杂、 可扩展、 高可用、 多层云服务。 您可以启用**会话关联 （"粘滞会话"）**、**快速缓存**，**远程调试**、 **SSL 卸载**、**防火墙/端口路由**、**远程访问**和大量的其他强大的功能。

如果您已完成本教程的 （"将应用程序部署到 Azure，快速而简单的方法"） 上一节，您将看到新的 Azure 的部署项目在项目资源管理器为您自动生成和名为"**MyHelloWorld_onAzure**"。

您可能还纷纷本教程通过自己首先创建一个空白的 Azure 部署项目，然后添加到您应用程序。 它是更长的时间进程，而且使您可以更好地控制从一开始的初始配置。

要从头开始创建新的 Azure 的部署项目，请单击**新的 Azure 部署项目**按钮![][ic710876]。

无论您是使用现有的 Azure 的部署项目中，还是创建一个从零开始，您就可以更改其配置设置和组件，例如 JDK 或应用程序服务器上，同样很容易地在任何时候。

若要更改 JDK，或应用程序服务器上或现有的 Azure 的部署项目中的应用程序列表︰

1. 展开项目资源管理器中的项目节点 (例如**MyHelloWorld_onAzure**)
2. 用鼠标右键单击**WorkerRole1**
3. 展开**Azure**子菜单中的上下文菜单
4. 单击**服务器配置**

无论是否启动这些服务器配置步骤编辑现有的 Azure 的部署项目，如上所示，或创建一个新从零开始，您将看到对话框使您可以配置您的 JDK、 服务器和应用程序的组件的类型相同。 若要了解如何更改在这些对话框中，更改了 JDK，应用程序服务器并添加或删除应用程序在部署中，例如设置的详细信息请参阅[服务器配置属性][]文章。

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>仅限 Windows︰ 部署计算仿真程序对应用程序 ##

>[AZURE.NOTE] 在 Windows Azure 仿真程序才可用。 如果您使用的操作系统不是 Windows，请跳过此部分。

如果创建了新的 Azure 部署项目之前，即隐式地描述的步骤通过发布 Azure，JDK 和应用程序到应用程序服务器已配置的云，而不是本地的仿真。 准备您的项目在本地仿真器中进行测试，请执行以下步骤︰

1. 在 Eclipse 的项目资源管理器中，单击**MyHelloWorld_onAzure**。
1. 右键单击**WorkerRole1**。
1. 展开**Azure**子菜单中的上下文菜单。
1. 单击**服务器配置**。
1. **JDK**选项卡上，检查该工具包已预配置了默认为您的本地 JDK。 如果没有，或者如果您想要更改的假定的默认值，请确保**使用的 JDK 进行本地测试此文件路径中的**复选框，并指定要使用的 JDK 安装位置。 如果您想要对其进行更改，请单击**浏览**按钮并使用浏览控件，选择要使用的 jdk 的目录位置。
1. 单击**服务器**选项卡。
1. 在底部的对话框中**本地服务器路径**文本框中，输入本地安装相匹配的类型和主版本号下**部署此类型的服务器**复选框的对话框顶部选择的服务器的服务器的路径。 如果您想要使用不同的类型或主要应用程序服务器的版本，请首先更改下该复选框的选定内容。
1. 单击**确定**。
1. 在 Eclipse 工具栏中，单击**在 Azure 仿真程序中运行**按钮， ![][ic710879]。 如果未启用**在 Azure 仿真程序中运行**按钮，确保在 Eclipse 的项目资源管理器中选择**MyHelloWorld_onAzure** ，并确保 Eclipse 的项目资源管理器，与当前窗口具有焦点。 这首先将启动完整生成您的项目的，然后启动 Java web 应用程序中的计算仿真程序。 （请注意，这取决于您的计算机的性能特性，第一个版本可能需要几秒钟到几分钟，但随后生成会更快。）第一个生成步骤完成后，将会提示通过 Windows 用户帐户控制 (UAC) 以允许对计算机进行更改此命令。 单击**是**。

>[AZURE.IMPORTANT] 如果您看不到 UAC 提示，请检查 Windows 任务栏上的 UAC 图标并单击它第一次。 有时 UAC 提示不显示为最上面的窗口，但仅作为任务栏图标可见。

1. 检查输出的计算仿真程序用户界面以确定是否有任何与您的项目的问题。 这取决于您的部署的内容，可能需要几分钟时间，您的应用程序必须完全计算仿真程序内启动。
1. 启动您的浏览器，并使用 URL`http://localhost:8080/MyHelloWorld`的地址 ( `MyHelloWorld` URL 的任何部分是区分大小写)。 您应该会看到您的 MyHelloWorld 应用程序 （index.jsp 的输出），类似于下图︰ ![][ic589579]

当准备停止应用程序运行在计算仿真器中，Eclipse 在工具栏中，单击**重置 Azure 仿真程序**按钮， ![][ic710880]。

## <a name="to-delete-your-deployment"></a>若要删除您的部署 ##

若要删除在 Eclipse 的 Azure Toolkit 部署，确保在 Eclipse 的项目资源管理器中选择**MyHelloWorld_onAzure** ，请确保 Eclipse 项目资源管理器具有当前窗口焦点，，然后单击**取消发布**按钮， ![][ic710883]，Eclipse 工具栏中。 （您可以完成相同的操作由**MyHelloWorld_onAzure**在 Eclipse 的项目资源管理器中用鼠标右键单击，单击**Azure** ，然后单击**取消部署后的再次从 Azure 云**。）这将显示对话框中**取消发布 Azure 项目**。

![][ic719491]

选择包含部署的订阅和云服务，选择您想要删除的部署，然后单击**取消发布**。

(使用该工具包删除部署的替代方法是使用 Azure 管理门户的**云服务**部分︰ 导航到您的部署，选择它，然后单击**删除**按钮。 这将停止，并删除部署。 如果您只是想要停止部署并不将其删除，单击**停止**按钮而不是**删除**按钮，但如上所述，如果您不删除部署，收费费用将继续部署累，即使它已停止）。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[日蚀式安装 Azure Toolkit][] 

[什么是 Azure Toolkit 的 Eclipse 中的新功能][]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure 的管理门户]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[在 Eclipse 中的 Azure 部署中启用远程访问]: http://go.microsoft.com/fwlink/?LinkID=699538
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546
[服务器配置属性]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[什么是 Azure Toolkit 的 Eclipse 中的新功能]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
