<properties
    pageTitle="在 Eclipse 中的调试 Azure 应用程序"
    description="了解有关使用 Eclipse Azure Toolkit 调试 Azure 应用程序。"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>在 Eclipse 中的调试 Azure 应用程序 #

使用 Eclipse Azure Toolkit，无论它们如果您使用的 Windows 为您的操作系统运行在 Azure，或计算仿真程序可以调试应用程序。 下图显示对话框用来启用远程调试的**调试**属性︰

![][ic719504]

本教程假设您已将应用程序已成功创建，并且熟悉如何计算仿真程序并部署到 Azure。

我们将使用应用程序不能[使用 Azure 服务运行库在 JSP 中][]的教程作为起始点本主题。 在继续之前，如果您没有创建该应用程序。

## <a name="to-debug-your-application-while-running-in-azure"></a>调试时在 Azure 中运行应用程序 ##

>[AZURE.WARNING] 该工具包的当前支持的远程 Java 调试主要用于部署在 Azure 计算仿真程序中运行。 因为调试连接不安全，不应启用远程调试生产部署中。 如果您需要调试在 Azure 的云环境中运行的应用程序，使用临时部署，但认识到未经授权的访问您的调试会话是可能的如果其他人知道您的云部署的 IP 地址，即使临时部署。

1. 生成您在仿真器中进行测试的项目︰ 在 Eclipse 的项目资源管理器中，用鼠标右键单击**MyAzureProject**，单击**属性**，单击**Azure**，，为设置**生成**到**部署至云**。
1. 重新生成项目︰ 从 Eclipse 菜单中，单击**项目**，然后单击**生成所有**。
1. 部署您的应用程序到 Azure 中的*转移*。
    >[AZURE.IMPORTANT] 如上所述，强烈建议您在大多数情况下，计算仿真调试，然后才需要额外调试在暂存环境中进行调试。 建议在生产环境中进行调试。
1. 一旦部署在 Azure 就绪，获得[Azure 管理门户][]部署的 DNS 名称。 临时部署具有 DNS 名称形式的 http://*&lt;guid&gt;*。 cloudapp.net，其中*&lt;guid&gt;*是由 Azure 分配一个 GUID 值。
1. Eclipse 的项目资源管理器中右键单击**WorkerRole1**， **Azure**，请单击，然后单击**调试**。
1. 在**WorkerRole1 调试属性**对话框中︰
    1. 检查**启用远程调试此角色。**
    1. 对于**输入终结点使用**，请使用**调试 （私钥︰ 8090 公众︰ 8090）**。
    1. 确保**在挂起模式下，等待调试器连接启动 JVM**处于未选中状态。
        >[AZURE.IMPORTANT] **在挂起模式下，等待调试器连接启动 JVM**选项适用于高级调试方案中计算仿真程序只 （不用于云部署）。 如果使用**在挂起模式下，等待调试器连接启动 JVM**选项，则它将 Eclipse 调试器连接到其 JVM 之前暂停服务器的启动过程。 使用计算仿真程序的调试会话，可以使用此选项，而不要使用它调试会话中的云部署。 服务器初始化发生在 Azure 启动的任务，和 Azure 的云不会使公共端点可用之前启动任务已完成。 因此，启动过程将成功完成如果启用了此选项，在云中部署中，因为它将不能接收来自外部的 Eclipse 客户端的连接。
1。 单击**创建调试配置**。
1. 在**Azure 调试配置**对话框中︰
    1. 对于**Java 项目进行调试**，请选择**MyHelloWorld**项目。
    1. 对于**调试配置**，检查**Azure 云 （转储）**。
    1. 确保**Azure 计算仿真程序**处于未选中状态。
    1. 对于**主机**，输入分阶段部署，但没有前面的**http://**的 DNS 名称。 例如 （使用此处所示的 GUID 代替 GUID）︰ **4e616d65-6f6e-6 d 65 6973 526f62657274.cloudapp.net**
1. 单击**确定**以关闭**Azure 调试配置**对话框。
1. 单击**确定**以关闭**WorkerRole1 调试属性**对话框。
1. 如果您没有 index.jsp 中已经设置了断点，请将其设置︰
    1. Eclipse 的项目资源管理器中展开**MyHelloWorld**，展开**网站**，然后双击**index.jsp**。
    1. Index.jsp 内, 蓝色栏左侧的 Java 代码中右键单击，单击**切换断点**，如下所示︰ ![][ic551537]
1. 日蚀式的菜单中单击**运行**，然后单击**调试配置**。
1. 在**调试配置**对话框中，在左侧窗格中展开**远程 Java 应用程序**，选择**Azure 云 (WorkerRole1)**，并单击**调试**。
1. 在您的浏览器中运行调试应用程序， **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**，替换 DNS 名称的 GUID * &lt;guid&gt;*。 如果提示您**确认视角切换**对话框中，单击**是**。 设置了断点的代码行到现在执行您的调试会话。

>[AZURE.NOTE] 如果您试图启动远程调试的部署具有多个角色实例中运行，无法当前控制哪个实例连接调试器将最初连接到，随着 Azure 负载平衡器将随机选择一个实例。 一旦您可以使用该实例，但将继续调试同一个实例。 注意还，如果处于非活动状态 （例如，当您太长时间在断点处停止时） 的时间超过 4 分钟的一段，Azure 可能会关闭连接。

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>调试多实例部署中的特定角色实例 ##

当在云中运行您的部署时，您将很可能运行它在多个计算，或角色，实例。 这使您能够利用 Azure 99.95%可用性保证，以及扩展您的应用程序。

在这种情况下，您可能需要远程调试 Java 应用程序中的特定角色实例。 但是，如果启用仅常规输入终结点以进行调试时，Azure 负载平衡器将为您将调试器连接到特定角色实例实际上已不可能。 而是它将您连接到它将随机选择一个角色实例。

这是一种方案，利用实例输入终结点将更便于调试特定角色实例。

让我们假设您计划要运行的部署的最多 5 个角色实例。 使用角色属性对话框中的**终结点**属性页，创建实例输入终结点并为其分配一系列公共端口，而不是一个端口号。 例如，在**公共端口**的输入框中，指定**81-85**。

部署您的应用程序与此实例终结点后，Azure 将为每个角色实例从该区域分配唯一的端口号。 然后，以找出哪个实例被分配的端口号，可以使用*InstanceEndpointName***_PUBLICPORT**环境变量 （其中*InstanceEndpointName*是分配时创建实例终结点的名称） 自动配置中 （例如，通过在网页的页脚中返回其值，因此，当您浏览到它可以阅读它） 部署工具包。

一旦知道哪个公用端口号分配实例，可以引用它在 Eclipse，调试配置中将它附加到您的服务的主机名。 这将使 Eclipse 调试器连接到该特定情况下，并不是任何其他实例。

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>仅限 Windows︰ 调试时计算在模拟器中运行应用程序 ##

>[AZURE.NOTE] 在 Windows Azure 仿真程序才可用。 如果您使用的操作系统不是 Windows，请跳过此部分。 

1. 生成您在仿真器中进行测试的项目︰ 在 Eclipse 的项目资源管理器中，右键单击**MyAzureProject**，单击**属性**，单击**Azure**，，到**仿真程序中的测试**设置**为生成**。
1. 重新生成项目︰ 从 Eclipse 菜单中，单击**项目**，然后单击**生成所有**。
1. Eclipse 的项目资源管理器中右键单击**WorkerRole1**， **Azure**，请单击，然后单击**调试**。
1. 在**WorkerRole1 调试属性**对话框中︰
    1. 检查**启用远程调试此角色。**
    1. **输入终结点使用**，为使用该工具包，列为**调试 （私钥︰ 8090 公众︰ 8090）**由自动生成的默认终结点。
    1. 确保**在挂起模式下，等待调试器连接启动 JVM**选项处于未选中状态。
        >[AZURE.IMPORTANT] **在挂起模式下，等待调试器连接启动 JVM**选项适用于高级调试方案中计算仿真程序只 （不用于云部署）。 如果使用**在挂起模式下，等待调试器连接启动 JVM**选项，则它将 Eclipse 调试器连接到其 JVM 之前暂停服务器的启动过程。 使用计算仿真程序的调试会话，可以使用此选项，而不要使用它调试会话中的云部署。 服务器初始化发生在 Azure 启动的任务，和 Azure 的云不会使公共端点可用之前启动任务已完成。 因此，启动过程将成功完成如果启用了此选项，在云中部署中，因为它将不能接收来自外部的 Eclipse 客户端的连接。
1。 单击**创建调试配置**。
1. 在**Azure 调试配置**对话框中︰
    1. 对于**Java 项目进行调试**，请选择**MyHelloWorld**项目。
    1. 对于**调试配置**，请检查**Azure 计算仿真程序**。
1. 单击**确定**以关闭**Azure 调试配置**对话框。
1. 单击**确定**以关闭**WorkerRole1 调试属性**对话框。
1. 在 index.jsp 设置断点︰
    1. Eclipse 的项目资源管理器中展开**MyHelloWorld**，展开**网站**，然后双击**index.jsp**。
    1. Index.jsp 内, 蓝色栏左侧的 Java 代码中右键单击，单击**切换断点**，如下所示︰ ![][ic551537]

       如果您看到一个蓝色栏内的断点图标左侧的 Java 代码，设置了断点。
1. 通过单击**在 Azure 仿真程序中运行**工具栏上的按钮 Azure 计算仿真程序中启动应用程序。
1. 日蚀式的菜单中单击**运行**，然后单击**调试配置**。
1. 在**调试配置**对话框中，在左侧窗格中展开**远程 Java 应用程序**，选择**Azure 仿真程序 (WorkerRole1)**，并单击**调试**。
1. 计算仿真表明运行您的应用程序，在您的浏览器之后, 运行**http://localhost:8080/MyHelloWorld**。
    如果提示您**确认视角切换**对话框中，单击**是**。
    设置了断点的代码行到现在执行您的调试会话。

这会向您展示如何调试中计算仿真;下一节演示如何调试应用程序部署到 Azure。

## <a name="debugging-notes"></a>调试的说明 ##

* 调试之后, 您可以通过单击 Eclipse 的菜单上，单击**窗口**，**打开角度来看**，并选择要使用的角度来看切换从**调试**到**Java**的角度。
* 若要启用远程调试在 GlassFish，不要为 Eclipse 使用 Azure Toolkit 的远程调试配置功能。 改为手动配置 GlassFish。 由于 GlassFish 将预定义的环境变量中的 Java 选项的方式，该工具包远程调试配置功能与 GlassFish 不无法正常工作。 如果启用了该工具包远程调试配置功能，它可能会阻止 GlassFish 启动。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

[日蚀式安装 Azure Toolkit][] 

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure 的管理门户]: http://go.microsoft.com/fwlink/?LinkID=512959
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546
[使用 JSP 中的 Azure 服务运行时库]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
