<properties
    pageTitle="到 Azure 应用程序服务的连续部署 |Microsoft Azure"
    description="了解如何启用连续部署到 Azure 应用程序服务。"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>连续部署到 Azure 应用程序服务

本教程展示了如何配置您的[Azure 应用程序服务]应用程序的连续部署工作流。 与 BitBucket、 GitHub，以及 Visual Studio 团队服务 (VSTS) 的应用程序服务集成使其中 Azure 拉入最新的更新从发布到这些服务项目的连续部署工作流。 连续部署选项很有用的项目，多和频繁的贡献已被集成。

## <a name="overview"></a>启用连续部署

若要启用连续部署， 

1. 将您的应用程序的内容发布到存储库，它可用于连续部署。  
    将项目发布到这些服务的详细信息，请参阅[创建 repo (GitHub)]、[创建 repo (BitBucket)]，并[开始使用 VSTS]。

2. 在[Azure 的门户]应用程序的菜单刀片式服务器，请单击**应用程序部署 > 部署选项**。 单击**选择源**，然后选择部署源。  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] VSTS 配置部署的应用程序服务帐户，请参阅本[教程](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)。
    
3. 完成授权工作流。 

4. 在**部署源**刀片式服务器，选择项目和分支从部署。 当您完成时，单击**确定**。
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] 当启用 GitHub 或 BitBucket 的连续部署，公共和私人项目将会显示。

    应用程序服务创建与选定存储库的关联，指定的分支，从文件中提取并维护您为您的应用程序服务的应用程序的存储库的克隆。 集成 VSTS 从 Azure 门户的连续部署配置时，使用应用程序服务[Kudu 部署引擎](https://github.com/projectkudu/kudu/wiki)，后者已经可以自动生成和部署任务的每个`git push`。 您不需要分别设置 VSTS 中的连续部署。 此过程完成后，**部署选项**的应用程序刀片式服务器将显示指示部署活动部署已成功完成。

5. 要验证已成功部署应用程序，请单击顶部的 Azure 的门户应用程序的刀片式服务器的**URL** 。 

6. 若要验证您选择的存储库中，出现了连续部署，推送到存储库中更改。 您的应用程序应该更新，以反映所做的更改推送到存储库中完成后不久。 您可以验证它拉开在您的应用程序的**部署选项**刀片式服务器中的更新。

## <a name="VSsolution"></a>Visual Studio 解决方案的连续部署 

Visual Studio 解决方案推向 Azure 应用程序服务是按简单 index.html 文件一样简单。 应用程序服务部署过程简化了所有细节，包括还原 NuGet 依存关系以及构建应用程序二进制文件。 可以按照源控件维护代码仅在 Git 存储库中的最佳做法，并让负责其余的应用程序服务部署。

Visual Studio 解决方案推向应用程序服务的步骤是相同与[上一节](#overview)中，前提是您配置解决方案和存储库，如下所示︰

-   使用 Visual Studio 源代码管理选项来生成`.gitignore`文件，如图像下面或手动添加`.gitignore`在您的存储库根内容类似于此的[.gitignore 示例](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore)中的文件。 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   存储库根中的.sln 文件到您的存储库，添加整个解决方案的目录树。

一旦您设置您的存储库，如所述，并从一个在线的 Git 存储库连续发布 Azure 中配置您的应用程序，可以在开发 ASP.NET 应用程序在 Visual Studio 中的本地和连续部署您的代码，只需通过将更改推送到您联机的 Git 存储库。

## <a name="disableCD"></a>禁用连续部署

若要禁用连续部署， 

1. 在[Azure 的门户]应用程序的菜单刀片式服务器，请单击**应用程序部署 > 部署选项**。 在**部署选项**刀片式服务器，然后单击**断开连接**。

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. 在出现确认消息，回答****之后, 返回到应用程序的刀片式服务器，单击**应用程序部署 > 部署选项**如果您想要将来自另一个源的发布设置。

## <a name="additional-resources"></a>其他资源

* [如何调查与连续部署的常见问题](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [如何使用 Azure PowerShell]
* [如何使用 Mac 和 Linux 的 Azure 命令行工具]
* [Git 文档]
* [项目 Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

[Azure 应用程序服务]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure 门户]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[如何使用 Azure PowerShell]: ../articles/powershell-install-configure.md
[如何使用 Mac 和 Linux 的 Azure 命令行工具]: ../articles/xplat-cli-install.md
[Git 文档]: http://git-scm.com/documentation

[创建 repo (GitHub)]: https://help.github.com/articles/create-a-repo
[创建 repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS 入门]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
