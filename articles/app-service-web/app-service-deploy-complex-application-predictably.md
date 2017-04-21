<properties
    pageTitle="配置和部署 microservices Azure 中预知"
    description="了解如何部署应用程序组成 microservices Azure 应用程序作为一个整体的服务中，并以可预测的方式使用 JSON 资源组模板和模板，PowerShell。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>配置和部署 microservices Azure 中预知 #

本教程展示如何配置和部署应用程序组成， [microservices](https://en.wikipedia.org/wiki/Microservices)在[Azure 应用程序服务](/services/app-service/)作为一个整体，以可预测的方式使用 JSON 资源组模板和模板，PowerShell。 

在资源调配和部署高级别的应用程序构成的高度分离时 microservices，可再现和可预测性是成功关键。 [Azure 应用程序服务](/services/app-service/)使您能够创建包含 web 应用程序、 移动应用程序、 API 的应用程序和应用程序逻辑的 microservices。 [Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)使您能够管理所有 microservices 作为一个单元，如数据库的资源依赖项和源控制设置。 现在，您还可以部署使用 JSON 模板和简单 PowerShell 脚本这样的应用程序。 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>您将做什么 ##

在本教程中，您将部署的应用程序包括︰

-   两个 web 应用程序 (即两个 microservices)
-   后端 SQL 数据库
-   应用程序设置、 连接字符串和源控制
-   应用程序的见解、 警报、 自动缩放设置

## <a name="tools-you-will-use"></a>您将使用的工具 ##

在本教程中，您将使用下列工具。 由于它不是工具的全面讨论，我要坚持的端到端方案和向各位简要介绍每个，并在哪里可以找到详细信息。 

### <a name="azure-resource-manager-templates-json"></a>Azure 的资源管理器模板 (JSON) ###
 
每次在 Azure 应用程序服务创建一个 web 应用程序，例如，Azure 资源管理器使用 JSON 模板创建整个资源组组件资源。 从[Azure 市场](/marketplace)像 MySQL 数据库，存储帐户，应用程序服务计划，使 web 应用程序本身，包括[WordPress 可伸缩](/marketplace/partners/wordpress/scalablewordpress/)应用程序的复杂模板预警规则、 应用程序设置、 自动缩放设置和更多，和所有这些模板都可通过 PowerShell。 有关如何下载和使用这些模板的信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。

Azure 资源管理器模板的详细信息，请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Visual Studio 的 azure SDK 2.6 ###

最新 SDK 包含 JSON 编辑器中的资源管理器模板支持的改进。 可以使用此快速从头开始创建资源组模板或打开现有的 JSON 模板 （如下载的库模板） 进行修改，填充参数文件，和甚至部署直接从 Azure 资源组解决方案的资源组。

有关详细信息，请参阅[有关 Visual Studio Azure SDK 2.6](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/)。

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 或更高版本 ###

在 0.8.0 版本开始，Azure PowerShell 安装包括 Azure 模块除了 Azure 资源管理器模块。 此新模块使您可以编写脚本部署的资源组。

有关详细信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure 资源浏览器 ###

该[预览工具](https://resources.azure.com)使您能够浏览在您的订购和各个资源的所有资源组的 JSON 定义。 在工具中，您可以编辑资源的 JSON 定义、 删除的资源，整个层次结构和创建新的资源。  在此工具中可用的信息是非常有用的模板创作，因为它显示哪些属性，您需要设置为特定类型的资源，正确的值，等等。甚至可以在[Azure 门户](https://portal.azure.com/)，创建资源组，然后检查其 JSON 定义在资源管理器工具可帮助您将模板化的资源组。

### <a name="deploy-to-azure-button"></a>将部署到 Azure 的按钮 ###

如果 GitHub 用作源代码管理后，可以将[部署到 Azure 按钮](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/)置于您的自述文件。MD，使交钥匙部署到 Azure 的 UI。 虽然可以为任何简单的 web 应用程序执行此操作，您可以扩展这可以通过将 azuredeploy.json 文件放置在存储库根部署整个资源组。 部署到 Azure 按钮将用此 JSON 文件，其中包含资源的组模板，来创建资源组。 有关示例，请参见[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)示例中，您将使用在本教程中。

## <a name="get-the-sample-resource-group-template"></a>获取示例资源组模板 ##

现在，让我们恢复正常。

1.  定位到[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)的应用程序服务的示例。

2.   在 readme.md 中，**部署到 Azure**。
 
3.  您要转到[部署到 azure](https://deploy.azure.com)站点，要求输入部署参数。 注意到为您存储库名称和一些随机字符串填充大部分字段。 如果您希望，但必须输入唯一的事情是 SQL Server 管理登录名和密码，然后单击**下一步**，您可以更改所有字段。
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  接下来，单击**部署**开始部署过程。 一旦进程一直运行到完成，请单击 http://todoapp*XXXX*。 azurewebsites.net 链接可以浏览已部署的应用程序。 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    用户界面将时第一次浏览到该应用程序刚开始向上，因为会有点慢，但说服自己它是一个功能齐全的应用程序。

5.  在部署页单击**管理**链接以查看新的应用程序在 Azure 门户。

6.  在**精要**下拉列表中，单击资源组链接。 此外请注意 web 应用程序已连接到 GitHub 存储库**外部的项目**下。 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  在资源组刀片式服务器，请注意，已有两个 web 应用程序和资源组中的一个 SQL 数据库。

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
您刚才看到在几个短的几分钟内完全部署两个 microservice 应用程序中的，所有组件、 依存关系、 设置、 数据库和连续发布，与由设置 Azure 资源管理器中的自动业务流程的全部内容。 所有这些都是通过以下两种情况︰

-   部署到 Azure 的按钮
-   在 repo 根 azuredeploy.json

您可以部署此相同的应用程序有几十、 上百或上千次，并且具有完全相同的配置每次。 重复性和可预测性，这种方法使您能够部署高缩放应用程序的易用性和可靠性。

## <a name="examine-or-edit-azuredeployjson"></a>检查 （或编辑） AZUREDEPLOY。JSON ##

现在让我们看一下 GitHub 存储库的设置方式。 您可以使用 JSON 编辑器在 Azure.NET SDK，所以如果您还没有安装[Azure.NET SDK 2.6](/downloads/)，做到现在。

1.  复制使用您最喜爱的 git 工具[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)资源库。 在下面的屏幕截图，我正在做这在 Visual Studio 2013年团队资源管理器中。

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  从存储库根，在 Visual Studio 中打开 azuredeploy.json。 如果您看不到 JSON 大纲窗格中，您需要安装 Azure.NET SDK。

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

我不会描述 JSON 格式中，每个细节，但[其他资源](#resources)部分的学习资源组模板语言链接。 在这里，我只要向您展示的有趣功能，可帮助您开始制作您自己的应用程序部署的自定义模板。

### <a name="parameters"></a>参数 ###

看一看参数部分以查看这些参数中的大多数是什么**部署到 Azure**按钮会提示您输入。 站点**部署到 Azure**按钮背后填充输入 UI 使用 azuredeploy.json 中定义的参数。 这些参数用于整个资源定义、 资源名称、 属性值等。

### <a name="resources"></a>资源 ###

在资源节点中，您可以看到 4 顶级资源已被定义，这将包括一个 SQL Server 实例、 应用程序服务计划，以及两个 web 应用程序。 

#### <a name="app-service-plan"></a>应用程序服务计划 ####

让我们开始使用 json 格式简单根级别的资源。 在 JSON 大纲中，单击突出显示相应的 JSON 代码命名为**[hostingPlanName]**的应用程序服务计划。 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

请注意，`type`元素指定的字符串 （它被称为服务器场前很长，长时间） 的应用程序服务计划，并填写其他元素和属性使用 JSON 文件中定义的参数和该资源没有任何嵌套的资源。

>[AZURE.NOTE] 另请注意的值`apiVersion`告诉的 Azure 哪个版本的 REST API，使用 JSON 资源定义，并且它可能会影响应如何在内部设置格式资源`{}`。 

#### <a name="sql-server"></a>SQL Server ####

接下来，单击名为**SQLServer** JSON 大纲中的 SQL Server 资源。

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
注意以下关于突出显示的 JSON 代码︰

-   使用参数可确保创建的资源进行命名和使它们彼此一致的方式配置。
-   SQLServer 资源具有两个嵌套的资源，各有不同的值`type`。
-   内的嵌套的资源`“resources”: […]`，定义了数据库和防火墙规则，有`dependsOn`指定的根级别 SQLServer 资源的资源 ID 的元素。 这会告诉 Azure 资源管理器中，"之前创建此资源时，其他资源必须已经存在;如果在模板中定义的其他资源，然后创建一个第一次"。

    >[AZURE.NOTE] 有关如何使用的详细信息`resourceId()`函数，请参见[Azure 资源管理器模板函数](../resource-group-template-functions.md)。

-   影响`dependsOn`元素是 Azure 资源管理器，可以知道哪些资源可以创建并行，并且必须按顺序创建的资源。 

#### <a name="web-app"></a>Web 应用程序 ####

现在，让我们转到实际的 web 应用程序本身，更复杂的。 单击 [variables('apiSiteName')] web app JSON 大纲以突出显示它 JSON 的代码中。 您会注意到事情正变得更有趣。 为此目的，我将谈到的功能一个一个地︰

##### <a name="root-resource"></a>根目录资源 #####

使 web 应用程序取决于两个不同的资源。 这意味着只有在创建应用程序服务计划和 SQL Server 实例后，Azure 资源管理器将创建 web 应用程序。

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>应用程序设置 #####

此外作为嵌套资源定义应用程序设置。

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

在`properties`元素的`config/appsettings`，您有两个应用程序设置的格式`“<name>” : “<value>”`。

-   `PROJECT`是告诉 Azure 的部署使用多项目 Visual Studio 解决方案中的项目的[KUDU 设置](https://github.com/projectkudu/kudu/wiki/Customizing-deployments)。 我将显示您以后如何配置源代码管理，但由于 ToDoApp 代码是多项目 Visual Studio 解决方案中，我们需要此设置。
-   `clientUrl`是只设置，应用程序代码的应用程序使用。

##### <a name="connection-strings"></a>连接字符串 #####

此外作为嵌套资源定义的连接字符串。

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

在`properties`元素的`config/connectionstrings`，每个连接字符串还定义名称︰ 值对，具有特定的格式为`“<name>” : {“value”: “…”, “type”: “…”}`。 对于`type`元素，可能的值为`MySql`， `SQLServer`， `SQLAzure`，和`Custom`。

>[AZURE.TIP] 有关连接字符串类型的明确列表，在 Azure PowerShell 运行下面的命令︰ \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>源代码管理 #####

源代码管理设置也被定义为嵌套资源。 Azure 的资源管理器使用该资源配置连续发布 (请参阅警告`IsManualIntegration`以后) 并且还可以甩掉的 JSON 文件处理过程的自动部署的应用程序代码。

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`和`branch`应该是非常直观和应指向的 Git 存储库，并要从发布的分支的名称。 再次，这些由输入参数定义。 

请注意，在`dependsOn`元素，该 web 应用程序资源本身，除了`sourcecontrols/web`还取决于`config/appsettings`和`config/connectionstrings`。 这是因为一旦`sourcecontrols/web`是配置，Azure 部署过程将自动尝试部署、 构建和启动应用程序代码。 因此，插入此依赖项可帮助您确保应用程序之前运行的应用程序代码具有访问所需的应用程序设置和连接字符串。 

>[AZURE.NOTE] 另请注意，`IsManualIntegration`设置为`true`。 此属性必须在本教程中进行，因为实际上不拥有 GitHub 存储库中，并因此无法实际授予的权限 Azure （即配置从[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)连续发布 自动存储库将更新推送到 Azure）。 您可以使用默认值`false`指定的存储库的所有者 GitHub 凭据配置在[Azure 门户](https://portal.azure.com/)之前，才进行。 换句话说，如果您先前已设置到 GitHub 或 BitBucket 的[Azure 门户网站](https://portal.azure.com/)中的任何应用程序的源代码管理，使用您的用户凭据，然后 Azure 将记住凭据并在将来部署从 GitHub 或 BitBucket 的任何应用程序时使用它们。 但是，如果尚未完成此操作，JSON 模板的部署将失败当 Azure 资源管理器尝试配置 web 应用程序的源代码管理设置，因为它不能以存储库所有者的身份登录到 GitHub 或 BitBucket。

## <a name="compare-the-json-template-with-deployed-resource-group"></a>比较与已部署的资源组的 JSON 模板 ##

在这里，您可以通过 web app 刀片在[Azure 门户](https://portal.azure.com/)中，但没有一样有用，如果不是更多的另一个工具。 转到[Azure 资源浏览器中](https://resources.azure.com)预览工具，为您提供的所有资源组的 JSON 表示在您的订阅，因为它们实际上存在于 Azure 的后端。 您还可以查看该资源组的 JSON 在 Azure 中的层次结构如何与用于创建该模板文件中的层次结构相对应。

例如，当我转到[Azure 资源管理器](https://resources.azure.com)工具并展开资源管理器中的节点时，我可以看到资源组，并在他们各自的资源类型收集的根级资源。

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

如果您深入了解 web 应用程序，您应该能够查看 web 应用程序配置详细信息类似于下面的屏幕快照︰

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

同样，嵌套的资源应具有层次结构非常类似于那些在 JSON 模板文件中，并且应该看到的应用程序设置、 连接字符串、 JSON 窗格中正确反映等。 此处的设置缺乏可能表明存在问题与 JSON 文件，可帮助您解决您 JSON 的模板文件。

## <a name="deploy-the-resource-group-template-yourself"></a>将部署用户的资源组模板 ##

**部署到 Azure**按钮很不错，但是它允许您仅当您已将 azuredeploy.json 推送到 GitHub 部署在 azuredeploy.json 中的资源组模板。 Azure.NET SDK 还提供工具，您可以部署任何 JSON 模板文件直接从您的本地计算机。 若要执行此操作，请按照下面的步骤操作︰

1.  在 Visual Studio 中，单击**文件** > **New** > **项目**。

2.  单击**C#** > **云** > **Azure 资源组**，然后单击**确定**。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  在**选择 Azure 模板**中，选择**空白模板**，然后单击**确定**。

4.  将 azuredeploy.json 拖到您的新项目**模板**文件夹中。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  从解决方案资源管理器，打开复制的 azuredeploy.json。

6.  为了方便演示，让我们添加一些标准的应用程序分析资源我们的 JSON 文件，通过单击**添加资源**。 如果你只关注部署的 JSON 文件，请跳至的部署步骤。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  选择**Web 应用程序的应用程序理解**，然后确保计划现有的应用程序服务和 web 应用程序被选中，然后单击**添加**。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    现在，您将能看到几个新的资源，具体取决于资源和它的作用，必须依赖的应用程序服务计划或使 web 应用程序。 这些资源不启用通过其现有的定义，要改变这种情况。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  在 JSON 的大纲，请单击**appInsights 自动缩放**以突出显示它 JSON 的代码。 这是您的应用程序服务计划的缩放设置。

9.  在突出显示的 JSON 代码，找到`location`和`enabled`属性，如下所示设置它们。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. 在 JSON 的大纲，请单击**CPUHigh appInsights**以突出显示它 JSON 的代码。 这是一个警报。

11. 查找`location`和`isEnabled`属性，如下所示设置它们。 做相同的其他三个警报 （紫色鳞茎植物）。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. 您现在可以进行部署。 用鼠标右键单击该项目，然后选择**部署** > **新的部署**。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. 如果还没有这么做，则登录到 Azure 帐户。

14. 选择您的订阅中的现有资源组或创建一个新**azuredeploy.json**，选择，然后单击**编辑参数**。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    现在，您将能够编辑好的表中的模板文件中定义的所有参数。 定义默认值的参数将已经有默认值，并且参数，以便定义的允许值列表将显示为下拉列表。

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. 填写所有空的参数，并在**repoUrl**中使用[GitHub repo ToDoApp 地址](https://github.com/azure-appservice-samples/ToDoApp.git)。 然后，单击**保存**。
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] 自动缩放是提供在**标准**层或更高，功能和规划级警报是功能提供在**基本**层或更高版本，您将需要设置为**标准**或**特优** **sku**参数，以便查看所有您的新应用程序见解资源亮起。
    
16. 单击**部署**。 如果您选择**保存密码**，该密码将保存在参数文件**以纯文本格式**。 否则，系统将要求您在部署过程中输入数据库密码。

就是这样！ 现在您只需转到[Azure 门户](https://portal.azure.com/)和[Azure 资源管理器](https://resources.azure.com)工具以查看新的警报，并自动缩放设置添加到您的 JSON 部署应用程序。

您在本部分中的步骤主要完成以下任务︰

1.  准备好模板文件
2.  创建与模板文件一起使用的参数文件
3.  部署模板文件中使用的参数文件

最后一步是很容易通过 PowerShell cmdlet。 要查看 Visual Studio 未部署该应用程序时，请打开 Scripts\Deploy AzureResourceGroup.ps1。 有大量的代码，但我只要突出显示部署参数文件的模板文件所需的所有相关代码。

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

最后一个 cmdlet， `New-AzureResourceGroup`，是实际执行的操作。 所有这一切应该向您展示，刀具的帮助下，它是相对简单，高的可预见性部署云应用程序。 每次使用相同的参数文件在同一模板上运行该 cmdlet，您将得到相同的结果。

## <a name="summary"></a>摘要 ##

在 DevOps，可再现和可预测性是由 microservices 组成一个高级别的应用程序任何成功部署的关键。 在本教程中，您已经部署到 Azure 的两个 microservice 应用程序作为一个资源组使用 Azure 资源管理器模板。 但愿它还赋予为开始将 Azure 应用程序转换为模板并可以调配和预见部署所需的知识。 

## <a name="next-steps"></a>下一步行动 ##

了解如何[应用敏捷方法和连续发布 microservices 应用程序提供易用性](app-service-agile-software-development.md)和高级的部署像[flighting 的部署](app-service-web-test-in-production-controlled-test-flight.md)技术很容易。

<a name="resources"></a>
## <a name="more-resources"></a>更多的资源 ##

-   [Azure 的资源管理器模板语言](../resource-group-authoring-templates.md)
-   [Azure 的资源管理器模板创作](../resource-group-authoring-templates.md)
-   [Azure 的资源管理器模板函数](../resource-group-template-functions.md)
-   [将与 Azure 资源管理器模板的应用程序部署](../resource-group-template-deploy.md)
-   [使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)
-   [在 Azure 中的故障排除资源组部署](../resource-manager-troubleshoot-deployments-portal.md)




 
