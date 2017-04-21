<properties
    pageTitle="敏捷软件开发与 Azure 应用程序服务"
    description="了解如何使用 Azure 应用程序服务创建高比例的复杂应用程序，支持敏捷软件开发的方式。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>敏捷软件开发与 Azure 应用程序服务 #

在本教程中，您将学习如何使用[Azure 应用程序服务](/services/app-service/)创建高比例的复杂应用程序，支持[敏捷软件开发](https://en.wikipedia.org/wiki/Agile_software_development)的方式。 该示例假定您已经知道如何[将 Azure 中预见的复杂应用程序部署](app-service-deploy-complex-application-predictably.md)。

技术过程中的限制通常可以阻碍成功实施敏捷方法。 Azure 应用程序服务功能，例如[连续发布](app-service-continuous-deployment.md)、[临时环境](web-sites-staged-publishing.md)（插槽） 和[监视](web-sites-monitor.md)、 时明智地结合的业务流程和管理部署在[Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)，可以很好的开发人员采用敏捷软件开发解决方案的一部分。

下表是一个与敏捷开发，和如何 Azure 服务启用每个要求的简短列表。

| 要求 | 如何使 Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -建立与每个提交<br>-自动生成和快速 | 用连续的部署配置，Azure 应用程序服务可以作为基于 dev 分支的实时运行生成。 每次代码被推送到分支，它是自动生成和运行实时 Azure 中。|
| -使自我测试的生成 | 负载测试、 web 测试等，可与 Azure 资源管理器模板一起部署。|
| -克隆生产环境中执行测试 | Azure 的资源管理器模板可用于创建用于测试快速、 可预知的 Azure 生产环境 （包括应用程序设置、 连接字符串的模板、 缩放等） 的克隆。|
| -方便地查看最新生成的结果 | 从存储库到 Azure 的连续部署意味着可以在实时应用程序中都测试新代码提交所做的更改后立即。 |
| -提交对主分支每一天<br>-自动化部署 | 生产应用程序与存储库的主要分支的持续集成自动部署到主分支到生产的每个提交/合并。 |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>您将做什么 ##

若要将新的更改发布到[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)示例应用程序，包含两个[web 应用程序](/services/app-service/web/)，一种是前端 (FE) 和另一个执行 Web API 后端 (BE) 和一个[SQL 数据库](/services/sql-database/)，将指导典型的开发测试阶段生产工作流程。 您将使用如下所示的部署体系结构︰

![](./media/app-service-agile-software-development/what-1-architecture.png)

若要将图片放入单词︰

-   部署体系结构被分成三个不同环境 （或[资源组](../azure-resource-manager/resource-group-overview.md)在 Azure），每个都有其自己[的应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)、[缩放](web-sites-scale.md)设置和 SQL 数据库。 
-   可以单独管理每个环境。 他们甚至可以存在于不同的预订。
-   临时和生产实现为两个插槽中的相同的应用程序服务应用程序。 主分支是暂存槽与持续集成的安装程序。
-   当提交到主分支上临时插槽 （与生产数据） 验证时，验证临时应用程序转换到生产插槽[而不需要停机](web-sites-staged-publishing.md)。

由模板中定义的生产和临时环境[*&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json)。

由模板中定义的开发和测试环境[*&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json)。

也将使用典型的分支策略，转向从 dev 分支到 test 分支中，然后 （在中向上移动质量，这样说） 的主分支的代码。

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>您将需要 ##

-   Azure 帐户
-   [GitHub](https://github.com/)帐户
-   Git 外壳 （安装使用[GitHub 的 Windows](https://windows.github.com/)）-这使您可以在同一个会话中运行的 Git 和 PowerShell 命令 
-   最新的[Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi)位
-   下列情况的基本了解︰
    -   [Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)模板部署 （也请参见[部署预知在 Azure 中复杂的应用程序](app-service-deploy-complex-application-predictably.md)）
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Azure 帐户来完成本教程，您需要︰
> + 您可以[免费开设 Azure 帐户](/pricing/free-trial/)-获取积分可用于试验 Azure 服务付费，和甚至用完后可以保留该帐户并使用自由 Azure 服务，例如 Web 应用程序。
> + 您可以[激活 Visual Studio 订户权益](/pricing/member-offers/msdn-benefits-details/)-您的 Visual Studio 预订为您提供了积分可用于付费的 Azure 服务每月。
>
> 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="set-up-your-production-environment"></a>设置您的生产环境 ##

>[AZURE.NOTE] 在本教程中使用的脚本将自动配置连续发布从 GitHub 资料库。 这就要求，已在 Azure 存储 GitHub 凭据，否则脚本的部署将失败时尝试配置 web 应用程序的源代码管理设置。 
>
>在 Azure 存储您 GitHub 的凭据，请[Azure 门户](https://portal.azure.com/)和[GitHub 部署配置](app-service-continuous-deployment.md)中创建 web 应用程序。 您只需执行一次。 

在 DevOps 典型方案中，您必须运行在 Azure，实时的应用程序和您要对其进行更改，通过连续发布。 在这种情况下，您可以开发、 测试，和部署在生产环境使用的模板。 您将其设置在此部分中。

1.  创建您自己分叉的[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)存储库。 创建您分叉的信息，请参阅[Repo 分叉](https://help.github.com/articles/fork-a-repo/)。 您分叉创建后，您可以在浏览器中看到它。
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  打开 Git Shell 会话。 如果您没有 Git 外壳，请立即安装[GitHub 的 Windows](https://windows.github.com/) 。

3.  通过执行以下命令来创建您分叉的本地克隆︰

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  一旦您本地克隆，定位到*&lt;repository_root >*\ARMTemplates，然后运行 deploy.ps1 脚本，如下所示︰

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  出现提示时，键入所需的用户名和密码访问数据库。

    您应该看到 Azure 的各种资源的调配进度。 当部署完成时，该脚本将启动该应用程序在浏览器中并为您提供友好的提示音。

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] 看一看*&lt;repository_root >*\ARMTemplates\Deploy.ps1，要看它如何生成具有唯一 Id 的资源。 可以使用相同的方法来创建相同的部署的克隆，而无需担心资源名称发生冲突。
 
6.  返回在 Git Shell 会话，运行︰

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  当脚本完成后时，要返回到浏览前端的地址 (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) 若要查看在生产环境中运行的应用程序。
 
5.  登录到[Azure 门户](https://portal.azure.com/)并看一看创建的内容。

    您应该能够看到在同一个资源组中，具有两个 web 应用程序`Api`的名称后缀。 如果您看一下资源组视图中，您将看到 SQL 数据库和服务器、 应用程序服务计划中和用于 web 应用程序的临时插槽。 浏览不同的资源，并比较它们与*&lt;repository_root >*\ARMTemplates\ProdAndStage.json 来查看他们在模板中的配置方式。

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

现在设置生产环境。 接下来，您将启动新给应用程序的更新。

## <a name="create-dev-and-test-branches"></a>创建开发和测试分支 ##

现在，您已经在 Azure 中的生产运行复杂的应用程序，将使对敏捷方法根据您的应用程序更新。 在本节中，将创建开发和测试将需要进行必要的更新的分支。

1.  首先创建测试环境。 在 Git Shell 会话中，运行下列命令以创建一个新的分支，称为**NewUpdate**的环境。 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  出现提示时，键入所需的用户名和密码访问数据库。 

    当部署完成时，该脚本将启动该应用程序在浏览器中并为您提供友好的提示音。 然后就像这样，现在有一个新的分支，有它自己的测试环境。 花点时间查看有关此测试环境中的几种方法︰

    -   您可以在任何 Azure 订阅创建它。 这意味着可以从您的测试环境分别管理生产环境。
    -   Azure 中实时运行您的测试环境。
    -   您的测试环境等同于生产环境中，除了临时插槽和缩放设置。 因为这是 ProdandStage.json 和 Dev.json 之间的唯一差异，您可以知道这。
    -   在其自己的应用程序服务计划，与不同价格层 （如**可用**） 中，您可以管理您的测试环境。
    -   删除此测试环境将是简单，只需删除该资源组。 您将了解如何执行此[更新](#delete)。

2.  继续创建 dev 分支通过运行以下命令︰

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  出现提示时，键入所需的用户名和密码访问数据库。 

    花点时间查看有关此开发环境中的几种方法︰ 

    -   您的开发环境具有相同的测试环境的配置，因为部署使用相同的模板。
    -   开发人员自己 Azure 预订，留下进行单独管理的测试环境中，可以创建每个开发环境。
    -   Azure 中实时运行开发环境。
    -   删除开发环境是简单，只需删除该资源组。 您将了解如何执行此[更新](#delete)。

>[AZURE.NOTE] 如果您有多个开发人员从事新的更新，每个可以轻松地创建分支和专用的开发环境通过执行以下操作︰
>
>1. 在 GitHub 创建自己分叉的存储库 （请参阅[分叉 Repo](https://help.github.com/articles/fork-a-repo/)）。
>2. 克隆在其本地计算机上的分叉
>3. 运行相同的命令来创建自己的 dev 分支和环境。

当您完成时，GitHub 分叉应有三个分支︰

![](./media/app-service-agile-software-development/test-1-github-view.png)

并在三个单独的资源组中应有六个 web 应用程序 （三套两个）︰

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] 请注意，ProdandStage.json 指定生产环境，使用**标准**的定价层，它是适合的生产应用程序的可伸缩性。

## <a name="build-and-test-every-commit"></a>生成并测试每个提交 ##

ProdAndStage.json 和 Dev.json 的模板文件已经指定源控制参数，其默认设置连续发布 web 应用程序。 因此，每次提交到 GitHub 分支会触发自动部署到 Azure 从该分支。 让我们看您的安装程序现在工作的方式。

1.  请确保您处于 Dev 分支的本地存储库。 若要执行此操作，请在 Git 外壳程序中运行以下命令︰

        git checkout Dev

2.  通过更改代码以使用[引导](http://getbootstrap.com/components/)列表到应用程序的 UI 层进行简单的更改。 打开*&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml 和使下面突出显示的更改︰

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] 如果您无法看清上面的图像︰ 
    >
    >- 在第 18 行，更改`check-list`到`list-group`。
    >- 在行 19，更改`class="check-list-item"`到`class="list-group-item"`。

3.  保存更改。 返回在 Git 外壳程序中，运行以下命令︰

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    这些 git 命令是类似于"检查您的代码在"像 TFS 的另一个源代码管理系统中。 当您运行`git push`，新提交触发自动代码推送到 Azure，然后重新生成应用程序，以反映在开发环境中的变化。

4.  若要验证此代码推送到您的开发环境已经出现，转到您的开发环境的 web 应用程序刀片式服务器并查看**部署**部分。 您应该能够看到您最新提交消息。

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  据此，请单击**浏览**以查看新的更改在 Azure 中的实时应用程序中。

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    这是非常次要更改的应用程序。 但是，对复杂的 web 应用程序的新更改进行多次有意外和令人不快的副作用。 能够方便地在动态生成中测试每个提交使您能够在您的客户看到它们之前捕捉这些问题。

到目前为止，您应该熟悉，作为**NewUpdate**项目的开发人员，您将能够轻松地为自己创建一个开发环境，然后生成每个提交并测试每次生成实现。

## <a name="merge-code-into-test-environment"></a>将代码合并到测试环境 ##

准备从 Dev 分支到分支 NewUpdate 推您的代码时，它就是标准的 git 过程︰

1.  合并的任何新承诺到 Dev 分支在 GitHub，如提交由其他开发人员创建的 NewUpdate。 在 GitHub 上的任何新提交将触发代码推，并在开发环境中生成。 然后可以确保 Dev 分支中的代码与 NewUpdate 分支中的最新位仍然起作用。

2.  将所有从 Dev 分支的新提交合并到 GitHub 上的 NewUpdate 分支。 本对策将触发代码推送并在测试环境中的生成。 

再次注意，连续部署已经具有这些 git 分支机构的设置，因为您不需要采取任何其他措施如运行集成构建。 您只需执行标准源控制实践使用 git，和 Azure 将为您执行所有生成过程。

现在，让我们将代码推送到**NewUpdate**分支。 Git Shell 中运行以下命令︰

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

就是这样！ 

转至 web 应用程序刀片式服务器为您的测试环境，若要查看您新的承约 （合并到 NewUpdate 分支） 现在推入到测试环境。 然后，单击**浏览**以查看样式更改现在正在运行实时 Azure 中。

## <a name="deploy-update-to-production"></a>将更新部署到生产环境 ##

推到临时和生产环境的代码应该感觉没有什么两样了已经做的工作时您将代码推送到测试环境。 它真的很简单。 

Git Shell 中运行以下命令︰

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

请记住，基于临时和生产环境是安装在 ProdandStage.json 的方式，您的新代码推送到**暂存**槽，那里运行。 所以如果您导航到暂存槽的 URL，您会发现运行出现的新代码。 若要执行此操作，请运行`Show-AzureWebsite`在 Git 外壳中的 cmdlet。

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
而现在，验证了临时的插槽中的更新后，唯一剩下所要做的事情就是将其切换到生产。 在 Git 外壳程序中，只需运行以下命令︰

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

祝贺您 ！ 生产 web 应用程序，已成功发行了新的更新。 更重要的是您所做它通过轻松地创建开发和测试环境，并构建和测试每个提交。 这些是敏捷软件开发的重要组成部分。

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>删除适用于开发人员和测试利用率 ##

因为故意具有设计开发和测试环境要独立的资源组，它是很容易将其删除。 删除您在本教程中，GitHub 分支和 Azure 项目，只需在 Git Shell 中运行下面的命令︰

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>摘要 ##

敏捷软件开发是必须对许多公司来说，那些想要采用 Azure 作为其应用程序平台。 在本教程中，您学习了如何创建和使用缓动，即使对于复杂应用程序拉下精确副本或接近生产环境的副本。 您还学习了如何利用这种能力来创建一个开发过程，它可以生成并测试在 Azure 中的每个单个的提交。 本教程中希望显示您如何最佳使用 Azure 应用程序服务和 Azure 资源管理器在一起以创建一个 DevOps 解决方案，它解决了敏捷方法。 接下来，您可以在这种情况下生成通过执行高级的 DevOps 的技术，如[在生产环境中测试](app-service-web-test-in-production-get-start.md)。 常见的测试在生产方案中，请参见[Flighting （beta 测试） 在 Azure 应用程序服务的部署](app-service-web-test-in-production-controlled-test-flight.md)。

## <a name="more-resources"></a>更多的资源 ##

-   [预见在 Azure 中复杂的应用程序部署](app-service-deploy-complex-application-predictably.md)
-   [在实践中的敏捷开发︰ 提示和技巧现代化的开发周期](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Azure 使用资源管理器模板的 Web 应用程序的高级的部署策略](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure 的资源管理器模板创作](../resource-group-authoring-templates.md)
-   [JSONLint-JSON 验证程序](http://jsonlint.com/)
-   [ARMClient – 设置 GitHub 发布到网站](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Git 分支--基本的分支和合并](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo 的博客](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [跨平台 azure 的命令行工具](../xplat-cli-install.md)
-   [创建或编辑在 Azure AD 用户](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [项目 Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
