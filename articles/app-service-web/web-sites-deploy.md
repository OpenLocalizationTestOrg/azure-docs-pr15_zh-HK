<properties
    pageTitle="将您的应用程序部署到 Azure 应用程序服务"
    description="了解如何将您的应用程序部署到 Azure 应用程序服务。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>将您的应用程序部署到 Azure 应用程序服务

本文可帮助您确定最佳的选项将为您的 web 应用程序、 移动应用程序的后端或 API 的应用程序文件部署到[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)，，然后引导您到说明特定于您的首选项相应的资源。

## <a name="overview"></a>Azure 应用程序服务部署概述

Azure 应用程序服务维护为您 （ASP.NET、 PHP、 Node.js 等） 的应用程序框架。 一些框架启用 Java 和 Python 之类时，默认情况下，可能需要一个简单的复选标记配置来启用它。 此外，您还可以自定义您的应用程序框架，例如 PHP 版本或运行库位。 有关详细信息，请参阅[配置您的应用程序在 Azure 应用程序服务](web-sites-configure.md)。

由于您不必须担心 web 服务器或应用程序框架，将您的应用程序部署到应用程序服务是 Azure 中的[**/site/wwwroot**目录](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)中部署代码、 二进制文件、 内容文件和它们各自的目录结构，一种 (或**/网站/wwwroot/App_Data/作业/** WebJobs 目录)。 应用程序服务支持以下部署选项︰ 

- [FTP 或 FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol)︰ 使用您最喜爱的 FTP 或 FTPS 启用工具将文件移动到 Azure，，从[FileZilla](https://filezilla-project.org)到像[NetBeans](https://netbeans.org)齐全的 Ide。 这绝对是一个文件上载过程。 由应用程序服务，如版本控制、 文件结构管理等提供无附加服务。 

- [Kudu （Git/Mercurial 或 OneDrive/收存箱）](https://github.com/projectkudu/kudu/wiki/Deployment)︰ 在应用程序服务中使用[部署引擎](https://github.com/projectkudu/kudu/wiki)。 将您的代码直接从任何存储库推送到 Kudu。 Kudu 还提供增值的服务，只要代码推送，包括版本控制、 包还原，MSBuild 和[web 挂钩](https://github.com/projectkudu/kudu/wiki/Web-hooks)的连续部署和其他自动化任务。 Kudu 部署引擎支持 3 种不同类型的部署源︰   
    * OneDrive 和收存箱中的内容同步   
    * 从 GitHub、 Bitbucket，以及 Visual Studio 团队服务的自动同步的基于存储库的连续部署  
    * 与手动同步的本地 Git 存储库的基于部署  

- [Web 部署](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)︰ 直接从您最喜爱的 Microsoft 应用程序服务的部署代码工具，如使用同一工具，Visual Studio 自动部署到 IIS 服务器。 此工具支持仅比较部署、 创建数据库、 连接字符串等的转换。Web 部署 Kudu 的不同之处在于构建应用程序二进制文件之前，它们将部署到 Azure。 类似于 FTP，没有其他的服务提供应用程序服务。

流行的 web 开发工具支持一个或多个这些部署过程。 而您选择的工具确定可以利用的部署进程，随时可以实际 DevOps 功能取决于部署过程和您选择的特定工具的组合。 例如，如果您执行 Web 部署从[Azure sdk 的 Visual Studio](#vspros)中，即使不能从 Kudu 获得自动化，不要得到在 Visual Studio 中的包还原和 MSBuild 自动化。 

>[AZURE.NOTE] 这些部署过程不地[调配 Azure 的资源](../resource-group-template-deploy-portal.md)可能会需要您的应用程序。 但是，大部分链接的操作方法文章向您展示如何配置应用程序，将代码部署到它的端到端。 您还可以找到调配 Azure 资源的[使用命令行工具的自动化部署](#automate)中的其他选项。
     
## <a name="ftp"></a>通过将文件手动复制到 Azure 通过 FTP 部署
如果您习惯于手动复制到 web 服务器的 web 内容，可以使用[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)实用程序复制文件，例如 Windows 资源管理器或[FileZilla](https://filezilla-project.org/)。

手动复制文件的优点是︰

- 熟悉和 FTP 刀具的最少的复杂性。 
- 让我们了解完全在您的文件将。
- FTPS 增加的安全性。

手动复制文件的缺点是︰

- 无需了解如何将文件部署到应用程序服务中的正确目录。 
- 回滚时出现故障的任何版本控制。
- 没有内置部署历史记录部署问题进行故障排除。
- 潜在的长部署时间因为许多 FTP 工具不提供只比较复制，只需复制的所有文件。  

### <a name="howtoftp"></a>如何通过将文件手动复制到 Azure 部署
将文件复制到 Azure 涉及几个简单步骤︰

1. 假定您已建立部署凭据通过转到**设置**来获取 FTP 连接信息 > **属性**，然后将这些值复制**FTP/部署用户**， **FTP 主机名称**和**提取主机名**。 请复制 Azure 门户包括的应用程序名为的 FTP 服务器提供正确的上下文中显示**FTP/部署用户**用户值。
2. 从 FTP 客户端，使用收集连接到您的应用程序的连接信息。
3. 将您的文件和它们各自的目录结构复制到 Azure 中的[**/site/wwwroot**目录](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)(或**/网站/wwwroot/App_Data/作业/** WebJobs 目录)。
4. 浏览到您的应用程序的 URL，以验证应用程序正常运行。 

有关详细信息，请参阅以下资源︰

* [创建一个 PHP MySQL web 应用程序和部署使用 FTP](web-sites-php-mysql-deploy-use-ftp.md)。

## <a name="dropbox"></a>通过与云文件夹同步部署
[手动复制文件](#ftp)到一个好的替代方案同步文件和文件夹到应用程序服务从云存储服务 OneDrive 和收存箱等。 与云文件夹同步利用部署的 Kudu 过程 （请参见[部署流程的概览](#overview)）。

与云文件夹同步的优点是︰

- 部署简单。 正如 OneDrive 和收存箱提供桌面的同步客户端，因此您的本地工作目录也是部署目录服务。
- 一键式部署。
- 在 Kudu 部署引擎的所有功能都都可用 （例如包还原、 自动化）。

与云文件夹同步的缺点是︰

- 回滚时出现故障的任何版本控制。
- 没有自动化的部署，则需要手动同步。

### <a name="howtodropbox"></a>如何通过与云文件夹同步部署
在[Azure 门户网站](https://portal.azure.com)中，可以指定 OneDrive 或收存箱云存储中的内容同步的文件夹、 工作与您的应用程序代码和在该文件夹中的内容并通过单击一个按钮同步到应用程序服务。

* [同步到 Azure 应用程序服务的云文件夹的内容](app-service-deploy-content-sync.md)。 

## <a name="continuousdeployment"></a>从基于云的源控制服务连续部署
如果您的开发团队使用[Visual Studio 的团队服务](http://www.visualstudio.com/)， [GitHub](https://www.github.com)或[BitBucket](https://bitbucket.org/)等基于云的来源代码管理 (SCM) 服务，您可以配置应用程序服务集成与您的存储库和持续部署。 

从基于云的源控制服务部署的优点如下︰

- 若要启用回滚的版本控制。
- 可以配置为 Git （以及 （如果适用） Mercurial） 的连续部署资料库。 
- 特定于分支的部署，可以将不同分支部署到不同的[插槽](web-sites-staged-publishing.md)。
- 在 Kudu 部署引擎的所有功能都都可用 （例如部署版本控制、 回滚、 包还原、 自动化）。

从基于云的源控制服务部署的内容是︰

- 各自的 SCM 服务所需的一些知识。

###<a name="vsts"></a>如何从基于云的源控制服务连续部署
在[Azure 门户网站](https://portal.azure.com)中，您可以配置从 GitHub、 Bitbucket 和 Visual Studio 团队服务的连续部署。

* [连续部署到 Azure 应用程序服务](app-service-continuous-deployment.md)。 

## <a name="localgitdeployment"></a>从本地 Git 部署
如果您的开发团队使用基于 Git 内部本地源的代码管理 (SCM) 服务，您可以配置此作为部署源应用程序服务。 

从本地 Git 部署的优点如下︰

- 若要启用回滚的版本控制。
- 特定于分支的部署，可以将不同分支部署到不同的[插槽](web-sites-staged-publishing.md)。
- 在 Kudu 部署引擎的所有功能都都可用 （例如部署版本控制、 回滚、 包还原、 自动化）。

从本地 Git 部署的骗子是︰

- 各自的 SCM 系统所需的一些知识。
- 针对连续部署没有交钥匙解决方案。 

###<a name="vsts"></a>如何从本地 Git 部署
在[Azure 门户网站](https://portal.azure.com)中，您可以配置本地 Git 部署。

* [本地 Git 部署到 Azure 应用程序服务](app-service-deploy-local-git.md)。 
* [发布到任何 git/hg repo 从 Web 应用程序](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html)。  

## <a name="deploy-using-an-ide"></a>使用 IDE 部署
如果您已经使用[Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) [Azure SDK](https://azure.microsoft.com/downloads/)，或如[Xcode](https://developer.apple.com/xcode/)， [Eclipse](https://www.eclipse.org)和[IntelliJ 的想法](https://www.jetbrains.com/idea/)其他 IDE 套件，您可以将部署到 Azure 直接从在 IDE 内。 该选项非常适合于单个开发人员。

Visual Studio 会支持所有三种部署过程 （FTP，Git，而且 Web 部署），根据自己的喜好，而如果他们有 FTP 或 Git 集成其他 Ide 可以将部署到应用程序服务 （请参见[部署流程的概览](#overview)）。

部署使用 IDE 的优点是︰

- 可能最小化您的端到端应用程序生命周期的工具。 开发、 调试、 跟踪，并将您的应用程序部署到 Azure 的全部，而无需移动您的 IDE 之外。 

部署使用 IDE 的缺点是︰

- 在工具中增加了复杂性。
- 仍然需要为团队项目的源代码管理系统。

<a name="vspros"></a>部署 Visual Studio 中使用 Azure SDK 的其他优点是︰

- Azure SDK 使 Azure 资源在 Visual Studio 中一类公民。 创建、 删除、 编辑、 开始和停止应用程序、 后端 SQL 数据库中查询、 实时调试 Azure 应用程序，以及更多。 
- 生活在 Azure 上的代码文件的编辑。
- 在 Azure 上实时调试的应用程序。
- 集成的 Azure 浏览器。
- 只有比较部署。 

###<a name="vs"></a>如何直接从 Visual Studio 部署

* [开始使用 Azure 和 ASP.NET](web-sites-dotnet-get-started.md)。 如何创建和部署一个简单的 ASP.NET MVC web 项目使用 Visual Studio 和 Web 部署。
* [如何使用 Visual Studio 部署 Azure WebJobs](websites-dotnet-deploy-webjobs.md)。 如何配置控制台应用程序项目，以便它们将部署为 WebJobs。  
* [部署与成员资格、 OAuth 和 SQL 数据库与 Web 应用程序安全的 ASP.NET MVC 5 应用程序](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)。 如何创建和部署使用 Visual Studio、 Web 部署，和实体框架代码第一个迁移 SQL 数据库，ASP.NET MVC web 项目。
* [ASP.NET Web 部署使用 Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction)。 12 部分教程系列，包括比其他在此列表中更完整的部署任务范围。 由于编写本教程，但后来添加的注释解释了什么是缺少上添加了一些 Azure 的部署功能。
* [部署到 Azure Git 存储库直接从 Visual Studio 2012 中的 ASP.NET 网站](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881)。 说明如何部署 ASP.NET web 项目中使用插件 Git 提交代码到 Git，Git 存储库的连接 Azure 的 Visual Studio。 从 Visual Studio 2013年开始，Git 的支持是内置的不需要插件的安装。

###<a name="aztk"></a>如何部署使用 Azure 工具包用于 Eclipse 和 IntelliJ 的想法

Microsoft 使可以将 Web 应用程序部署到 Azure，直接从 Eclipse 和通过[Eclipse 的 Azure Toolkit](../azure-toolkit-for-eclipse.md)和[Azure Toolkit 为 IntelliJ](../azure-toolkit-for-intellij.md)IntelliJ。 下面的教程演示了将简单"Hello"世界 Web 应用程序部署到 Azure 使用任一 IDE 中涉及到的步骤︰

*  [创建在 Eclipse 中的 Azure 照会世界 Web 应用程序](./app-service-web-eclipse-create-hello-world-web-app.md)。 本教程展示如何使用 Eclipse 的 Azure Toolkit 创建和部署你好世界 Web 应用程序的 Azure。
*  [创建在 IntelliJ Azure 照会世界 Web 应用程序](./app-service-web-intellij-create-hello-world-web-app.md)。 本教程展示如何使用 Azure Toolkit IntelliJ 对于创建和部署你好世界 Web 应用程序的 Azure。


## <a name="automate"></a>通过使用命令行工具来自动化部署

* [使用 MSBuild 自动化部署](#msbuild)
* [将 FTP 工具和脚本文件复制](#ftp)
* [使用 Windows PowerShell 自动化部署](#powershell)
* [使用.NET 管理 API 自动化部署](#api)
* [从 Azure 部署命令行界面 (CLI Azure)](#cli)
* [从 Web 部署命令行部署](#webdeploy)
* [使用 FTP 批处理脚本](http://support.microsoft.com/kb/96269)。
 
另一个部署选项是使用基于云的服务如[Octopus 部署](http://en.wikipedia.org/wiki/Octopus_Deploy)。 有关详细信息，请参阅[部署 ASP.NET 应用程序到 Azure 网站](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites)。

###<a name="msbuild"></a>使用 MSBuild 自动化部署

如果您使用[Visual Studio IDE](#vs)的开发时，可以使用[MSBuild](http://msbuildbook.com/)自动化可以在 IDE 中执行的任何操作。 您可以配置 MSBuild 使用[Web 部署](#webdeploy)或[FTP/FTPS](#ftp)来复制文件。 Web 部署也可以自动执行许多其他与部署相关的任务，例如部署数据库。

有关使用 MSBuild 命令行部署的详细信息，请参阅以下资源︰

* [使用 Visual Studio 的 ASP.NET Web 部署︰ 命令行部署](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)。 有关部署到 Azure 使用 Visual Studio 的教程系列中的第十。 演示如何使用命令行来部署设置发布配置文件在 Visual Studio 中之后。
* [在 Microsoft 生成引擎︰ 使用 MSBuild 和基础的团队项目生成](http://msbuildbook.com/)。 包含有关如何部署使用 MSBuild 章节的硬拷贝的书籍。

###<a name="powershell"></a>使用 Windows PowerShell 自动化部署

您可以从[Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)执行 MSBuild 或 FTP 部署功能。 如果您这样做时，您还可以使用轻松地调用 Azure 其余管理 API 的 Windows PowerShell cmdlet 的集合。

有关详细信息，请参阅以下资源︰

* [部署 web 应用程序链接到 GitHub 存储库](app-service-web-arm-from-github-provision.md)
* [设置 web 应用程序与 SQL 数据库](app-service-web-arm-with-sql-database-provision.md)
* [配置和部署 microservices Azure 中预知](app-service-deploy-complex-application-predictably.md)
* [使用 Azure，构建真实的云应用程序自动化的一切](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)。 介绍了电子书中所示的示例应用程序如何使用 Windows PowerShell 的脚本来创建 Azure 的测试环境并将其部署到它的电子书章。 请参阅[资源](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources)部分的其他 Azure PowerShell 文档的链接。
* [使用 Windows PowerShell 脚本要发布到开发和测试环境](../vs-azure-tools-publishing-using-powershell-scripts.md)。 如何使用 Windows PowerShell 部署脚本的生成 Visual Studio。

###<a name="api"></a>使用.NET 管理 API 自动化部署

您可以编写代码 C# 部署执行 MSBuild 或 FTP 的功能。 如果您这样做时，您可以访问 Azure 管理 REST API 来执行网站管理功能。

有关详细信息，请参阅以下资源︰

* [自动化的 Azure 管理库和.NET 的所有内容](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。 介绍.NET 管理 API 和多个文档的链接。

###<a name="cli"></a>从 Azure 部署命令行界面 (CLI Azure)

可以在 Windows、 Mac 或 Linux 机器中使用命令行来使用 FTP 部署的。 如果这样做，您也可以访问 Azure 其余管理 API 使用 Azure CLI。

有关详细信息，请参阅以下资源︰

* [Azure 命令行工具](/downloads/#cmd-line-tools)。 在 Azure.com 命令行工具的信息的门户页。

###<a name="webdeploy"></a>从 Web 部署命令行部署

[Web 部署](http://www.iis.net/downloads/microsoft/web-deploy)是部署到 IIS，不仅提供了智能文件同步功能，但也可以执行或协调许多其他与部署相关的任务使用 FTP 时无法自动的 Microsoft 软件。 例如，Web 部署可以部署新数据库或数据库更新，以及您的 web 应用程序。 Web 部署可以还将更新现有的站点，因为它可以智能地将复制已更改的文件所需的时间降至最低。 Microsoft Visual Studio 和 Team Foundation Server 操作系统都支持 Web 部署内置，但您也可以使用 Web 部署直接从命令行以使部署自动化。 Web 部署命令的功能非常强大，但学习曲线很陡。

有关详细信息，请参阅以下资源︰

* [简单的 Web 应用程序︰ 部署](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/)。 通过关于他写来方便地使用 Web 部署工具的 David Ebbo 的博客。
* [Web 部署工具](http://technet.microsoft.com/library/dd568996)。 官方 Microsoft TechNet 站点上的文档。 但仍良好开端的发布日期。
* [使用 Web 部署](http://www.iis.net/learn/publish/using-web-deploy)。 访问 IIS.NET Microsoft 网站上的正式文档。 但良好开端还发布日期。
* [使用 Visual Studio 的 ASP.NET Web 部署︰ 命令行部署](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)。 MSBuild 将使用 Visual Studio，则生成引擎，它还可用于从命令行部署到 Web 应用程序的 web 应用程序。 本教程是关于 Visual Studio 部署主要是一系列的一部分。

##<a name="nextsteps"></a>下一步行动

在某些情况下可能需要能够轻松地转移到您的应用程序的产品版本之间来回切换。 有关详细信息，请参阅[在 Web 应用程序上分阶段部署](web-sites-staged-publishing.md)。

具有备份和还原计划到位是任何部署工作流的一个重要组成部分。 应用程序服务有关的信息备份和还原功能，请参见[Web 应用程序备份](web-sites-backup.md)。  

有关如何使用 Azure 的基于角色的访问控制来管理对部署的应用程序服务的访问信息，请参阅[RBAC 和 Web 应用程序发布](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/)。


 
