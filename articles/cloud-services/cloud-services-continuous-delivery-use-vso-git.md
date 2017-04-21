<properties
    pageTitle="Git 和 Azure 中的 Visual Studio 团队服务不断提供 |Microsoft Azure"
    description="了解如何配置 Visual Studio 团队服务团队项目使用 Git 来自动生成和部署对 Azure 应用程序服务或云服务中的 Web 应用程序功能。"
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>连续传递到 Azure 使用 Visual Studio 的团队服务和 Git

可以使用 Visual Studio 的团队服务团队项目承载的 Git 存储库的源代码，自动生成并部署到 Azure 的 web 应用程序或云服务，只要您将提交推送到存储库中。

您将需要 Visual Studio 2013年和 Azure SDK 安装。 如果您尚没有 Visual Studio 2013年，通过选择的**入门免费**链接可以在[www.visualstudio.com](http://www.visualstudio.com)下载。 从[此处](http://go.microsoft.com/fwlink/?LinkId=239540)安装 Azure SDK。


> [AZURE.NOTE]您需要一个 Visual Studio 团队服务帐户来完成本教程︰ 您可以[打开 Visual Studio 团队服务的客户免费](http://go.microsoft.com/fwlink/p/?LinkId=512979)。

若要设置自动生成并使用 Visual Studio 的团队服务部署到 Azure 的云服务，请按照下列步骤。

## <a name="1-create-a-git-repository"></a>1︰ 创建一个 Git 存储库

1. 如果您尚没有一个 Visual Studio 团队服务帐户，您可以获取一个[此处](http://go.microsoft.com/fwlink/?LinkId=397665)。 创建团队项目时，选择 Git 作为源代码管理系统。 按照说明将 Visual Studio 的连接到您的团队项目。

2. 在**团队资源管理器**，选择**复制此资源库**的链接。

    ![][3]

3. 指定的本地副本的位置，然后选择**克隆**按钮。

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2︰ 创建项目并将其提交到存储库

1. 在**团队资源管理器**，在**解决方案**部分中，选择**新建**链接以在本地存储库中创建新项目。

    ![][4]

2. 可以在本演练中的步骤来部署 web 应用程序或云服务 （Azure 应用程序）。 创建新的 Azure 云服务项目或一个新的 ASP.NET MVC 项目。 请确保项目面向.NET Framework 4 或更高版本。 如果您正在创建一个云服务项目，添加 ASP.NET MVC web 角色和辅助角色。
如果您想要创建一个 web 应用程序， **ASP.NET Web 应用程序**项目模板中，选择，然后选择**MVC**。 有关详细信息，请参阅[创建在 Azure 应用程序服务的 ASP.NET web 应用程序](../app-service-web/web-sites-dotnet-get-started.md)。

3. 打开的解决方案中，快捷菜单，然后选择**提交**。

    ![][7]

4. 如果您已在 Visual Studio 团队服务中使用 Git 的第一次，您需要提供一些信息，来确定自己的 Git。 在**团队资源管理器****挂起的更改**区域中，输入您的用户名和电子邮件地址。 为提交输入注释，然后选择**提交**按钮。

    ![][8]

5. 请注意包含或排除特定的更改，当您签入选项。 如果排除所需的更改，请选择**包括所有**。

6. 您现在已经在存储库中的本地副本提交所做的更改。 下一步，通过选择**同步**链接同步这些更改与服务器。

## <a name="3-connect-the-project-to-azure"></a>3: 项目连接到 Azure

1. 既然您已经有中 Visual Studio 团队服务的 Git 存储库中某些源代码，您就可以连接到 Azure 的 git 存储库。  在[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)中，选择您的云服务或 web 应用程序，或创建一个新的选择 + 在左下角和选择**云服务**或**Web 应用程序**，然后**快速创建**图标。

    ![][9]

3. 对于云服务，选择**设置使用 Visual Studio 的团队服务发布**链接。 Web 应用程序，请选择**设置部署从源代码管理**链接。

    ![][10]

2. 在向导中，在文本框中键入您的 Visual Studio 团队服务帐户的名称，并选择**立即授权**链接。 您可能需要在每次登录。

    ![][11]

3. 在**连接请求**弹出对话框中，选择**接受**授权 Azure 若要在 Visual Studio 的团队服务配置团队项目。

    ![][12]

4. 授权成功后，您将看到包含 Visual Studio 团队服务团队项目的下拉列表。  选择在前面的步骤中创建的团队项目的名称，然后选择该向导复选标记按钮。

    ![][13]

    下一次您将提交推送到您的存储库，Visual Studio 团队服务将生成并将项目部署到 Azure。

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4︰ 触发重新构建和重新部署您的项目

1. 在 Visual Studio，打开的文件并对其进行更改。 例如，更改文件`_Layout.cshtml`在视图\\MVC web 角色中的共享文件夹。

    ![][17]

2. 编辑站点的页脚文本并保存该文件。

    ![][18]

3. 在**解决方案资源管理器**中打开解决方案节点、 项目节点或更改时，该文件的快捷菜单，然后选择**提交**。

4. 键入注释，然后选择**提交**。

    ![][20]

5. 选择**同步**链接。

    ![][38]

6. 选择**推**链接到存储库中 Visual Studio 团队服务推您承约。 （您可以也使用**同步**按钮将您提交复制到存储库中。 区别是**同步**还拉从存储库中最新的更改。

    ![][39]

7. 选择**主页**按钮以返回到**团队资源管理器**主页上。

    ![][21]

8. 选择**生成**以查看正在进行的生成。

    ![][22]

    **团队资源管理器**显示已为您签入触发的生成。

    ![][23]

9. 若要查看详细的日志进行生成时，双击正在进行的生成的名称。

10. 正在进行生成时，看一下当向导用于链接到 Azure 创建的生成定义。  打开该生成定义的快捷菜单，然后选择**编辑生成定义**。

    ![][25]

11. 在**触发器**选项卡中，您将看到，生成定义设置为在每次签入生成默认。 （对于云服务，Visual Studio 团队服务生成并自动将主分支部署到临时环境。 您仍然需要做一个手动步骤来部署到网站。 对于 web 应用程序中未使用暂存环境的它直接到活动站点部署主分支。

    ![][26]

1. 在**进程**选项卡，您可以看到部署环境设置为您的云服务或 web 应用程序的名称。

    ![][27]

1. 如果您希望与默认值不同的值，请指定属性的值。 Azure 发布的属性是在**部署**部分中，您可能还需要设置 MSBuild 参数。 例如，在云服务项目，若要指定除"云"服务配置 MSbuild 参数设置为`/p:TargetProfile=[YourProfile]` *[YourProfile]*与服务配置文件服务配置类似的名称相匹配。*YourProfile*.cscfg。

    下表显示在**部署**部分中的可用属性︰

  	|属性|默认值|
  	|---|---|
  	|允许不受信任的证书|如果为 false，则必须由根证书颁发机构签名的 SSL 证书。|
  	|允许升级|允许更新现有部署而不是创建一个新的部署。 保留的 IP 地址。|
  	|请不要删除|如果为 true，则不会覆盖现有的相关的部署 （允许升级）。|
  	|部署设置路径|用于 web 应用程序中，相对于根目录文件夹的 repo.pubxml 文件的路径。 对于云服务忽略。|
  	|Sharepoint 部署环境|服务名称相同。|
  	|Azure 的部署环境|Web 应用程序或云服务名称。|

1. 这个时候，应成功完成您的生成。

    ![][28]

1. 如果您双击生成名称，Visual Studio 将显示**生成摘要**，包括关联的单元测试项目中的所有测试结果。

    ![][29]

1. 在[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)中，您可以在**部署**选项卡上查看关联的部署当选临时环境。

    ![][30]

1.  浏览到您的站点的 URL。 对于 web 应用程序，只需在门户中选择**浏览**按钮。 对于云服务，**仪表板**页，其中显示暂存环境的**快速概览**部分中选择 URL。

    默认情况下，从云服务的持续集成的部署发布到临时环境。 您可以更改此**备用云服务环境**属性设置到**生产环境**。 下面是其中的网站 URL 是云服务的仪表板页面上。

    ![][31]

    一个新的浏览器选项卡会打开以显示您正在运行的站点。

    ![][32]

1.  如果您对您的项目中进行其他更改，则触发器更生成，而且将累计多个部署。 最新标记为活动。

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5︰ 重新部署较早的版本

此步骤是可选的。 在 Azure 的传统门户网站，选择早期部署，然后选择**重新部署**退到早期签入您的网站。 注意，这将触发新生成在 TFS，您部署的历史记录中创建新条目。

![][34]

## <a name="6-change-the-production-deployment"></a>6︰ 更改生产部署

准备就绪后，您可以通过在 Azure 经典门户中选择**换**提升暂存环境与生产环境。 新部署的临时环境被提升为生产，和以前的生产环境中，如果有的话，将成为临时环境。 活动部署可能是不同的生产环境和临时环境，但最新版本的部署历史记录是相同的无论环境如何。

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7︰ 从工作分支部署。

当您使用 Git 时，您通常工作分支中进行的更改和集成到主分支，当您开发达到完成的状态。 在项目的开发阶段，您需要生成和部署对 Azure 的工作分支。

1. 在**团队资源管理器**中选择**主页**按钮，然后选择**分支**按钮。

    ![][40]

2. 选择**新的分支**链接。

    ![][41]

3. 输入的名称的分支，例如"正在工作"，然后选择**创建分支**。 这将创建新的本地分支。

    ![][42]

4. 发布该分支。 在**未发布的分支**中选择分支名称，然后选择**发布**。

    ![][44]

6. 默认情况下，仅对主分支的更改触发一次持续生成。 若要设置连续工作分支的生成，在**团队资源管理器**，选择**生成**页上，然后选择**编辑生成定义**。

7. 打开**源设置**选项卡。 **受监视以及持续集成生成进行分支**，在选择**添加新行，请单击此处**。

    ![][47]

8. 指定创建，例如，引用/领导/工作的分支。

    ![][48]

9. 在对代码进行更改，打开快捷菜单，用于更改的文件，然后选择**提交**。

    ![][43]

10. 选择**同步提交**链接，然后选择**同步**按钮或**推**链接将更改复制到 Visual Studio 团队服务中的工作分支的副本。

    ![][45]

11. 导航到**生成**视图，并查找工作分支的生成就有了触发的。

## <a name="next-steps"></a>下一步行动

在 Git 中使用 Visual Studio 的团队服务的更多提示，请参阅[开发并共享您的代码中使用 Visual Studio 的 Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx)并使用不受 Visual Studio 团队服务发布到 Azure 的 Git 存储库的信息，请参阅[连续部署到 Azure 应用程序服务](../app-service-web/app-service-continuous-deployment.md)。 Visual Studio 团队服务的详细信息，请参阅[Visual Studio 团队服务](http://go.microsoft.com/fwlink/?LinkId=253861)。

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
