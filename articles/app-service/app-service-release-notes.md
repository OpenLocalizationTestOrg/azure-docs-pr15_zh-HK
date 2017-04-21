<properties 
   pageTitle="用于.NET 2.5.1 azure SDK 发行说明" 
   description="用于.NET 2.5.1 azure SDK 发行说明" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>用于.NET 2.5.1 azure SDK 发行说明

本文档包含的发行说明的 Azure sdk.NET 2.5.1 发布。 

##<a name="azure-sdk-for-net-251-release-notes"></a>用于.NET 2.5.1 azure SDK 发行说明

以下是新功能和更新.net 2.5.1 Azure SDK 中。

- 新的 features\scenarios 相关的**Web 工具扩展**。 

    - Azure 网站已重命名为 Azure 应用程序服务。 有关详细信息，请参阅[Azure 应用程序服务和现有的 Azure 服务](app-service-changes-existing-services.md)。
    - Azure 应用程序 API （预览） 支持已添加，以便客户可以将发布 API 的应用程序，ASP.NET 项目，然后使用添加 > Azure API 应用程序客户端 C# 中的笔势项目生成的代码基于部署 API 应用程序的结构。 
    - 在服务器资源管理器中的网站节点已被否决而不是 Azure 应用程序服务节点，其中包含支持基于组的分组的 Azure API 的应用程序、 移动应用程序和 Web 应用程序的资源。
    - 添加移动应用程序控制器，以便客户可以创建新的移动应用程序项目中，已添加了 azure 的移动应用程序 （预览） 支持、 发布项目，和远程调试应用程序。
    - 添加 > 笔势 Azure API 应用程序客户端现在支持本地 Swagger JSON 文件，因此 Web API 的开发人员可以使用第三方 NuGets 与 Swashbuckle 一样来生成 Swagger 或手动创作。 这样，客户端开发人员可以使用代码生成功能来使用在 C# 项目中的任何 Swagger 端点。 
    - Web 应用程序和 API 的应用程序发布对话框得到了增强，支持资源分组，Azure 门户概念，并表示在新 Web 应用程序和 API 的应用程序资源调配对话框中选择/Azure 资源组创建和应用程序服务计划。 
    - Azure API 的应用程序服务器资源管理器节点提供到 Azure 门户网站，以及其他功能，如日志流和远程调试 API 应用程序深层链接的链接。

    有关已知的问题和[Azure SDK.NET 2.5.1 本节下面](app-service-release-notes.md#known_issues_2_5_1)的当前限制。


- 在本版本中启用了新的 features\scenarios 与**HDInsight 工具**在 Visual Studio 中。 
    - 本地验证的配置单元的脚本中。 单击验证脚本按钮在工具栏中查看您的脚本中是否有任何错误。 
    - 改进的配置单元作业的调试功能。 现在，您可以通过访问 Yarn 日志在 Visual Studio 中的调试配置单元作业。 如果您的应用程序存在性能问题，研究 YARN 日志将提供有用的信息.
    - （公共预览）关键字自动完成功能和 IntelliSense 支持配置单元。 为了帮助您创建的脚本配置单元、 HDInsight Visual Studio 工具添加关键字自动完成功能和 IntelliSense 支持配置单元。
    - 风暴的支持。 现在可以使用 HDInsight 工具 Visual Studio 开发风暴拓扑/Spouts/螺栓在 C#。 然后可以提交开发到风暴群集拓扑和管口拓扑/螺栓状态，请参阅。 可以使用系统日志和客户日志来解决您的暴风雨拓扑/螺栓/Spouts。 您还可以在 HDInsight 上的风暴中使用现有 JAVA 资产。
    
    有关详细信息，请参阅[开始使用 Visual Studio 的 HDInsight Hadoop 工具](hdinsight-hadoop-visual-studio-tools-get-started.md)。



##<a id="known_issues_2_5_1"></a>用于.NET 2.5.1 azure SDK 已知问题和限制

- Azure API 应用程序将显示为移动应用程序的部署目标。 Web 应用程序的后续版本之前应为移动应用程序的唯一目标。 
- Azure API 的应用程序资源调配可以奏效，但间歇性地无法更新 Azure 应用程序服务活动窗口中的进度。 解决方法是检查在 Azure 门户新 Azure API 应用程序的状态。 
- 文件 > 新建项目 > API 应用程序 > F5 体验会因为没有 default/index.html 导致 HTTP 错误。 解决方法是手动浏览到 /api/values 的 URL。 
- 间歇，平展显示服务器资源管理器的图标。 重新启动 VS 可以解决此问题。 
- 在 Web 应用程序或 API 的应用程序 （如超出的配额错误或重复 Azure API 应用程序网关名称） 资源调配过程中引发异常，则错误将显示一些原始 JSON 文本。 
- 间歇性的项目创建问题时在创建项目时检查应用程序的见解。
- 有时，生成的 Azure API 应用程序客户端代码没有命名空间，它们需要手动包含 （或自动导入通过 Visual Studio 提示） 编译的代码。 
- 移动应用程序项目应该发布到 web 应用程序，但您必须选取由于移动应用程序项目都需要一个数据库创建为一个移动应用程序在 Azure 门户的网站。 
- 移动应用程序的起始页使用术语"移动服务"而不是"移动应用程序" 
- 移动应用程序项目创建可能需要一分钟的时间来创建。 
- 在 API 的应用程序 （在某些情况下） 资源调配错误来自回 Azure API 以反映的权限无法设置正确，虽然 API 的应用程序已正确配置并可供使用。 您可以手动设置使用 Azure 门户的权限。
- 在 API 的应用程序和移动应用程序模板不支持应用程序的见解。
- 不能配合云服务项目使用 API 的应用程序项目。
- API 的应用程序项目模板才可用在 C#。
- 在 C# 中只支持 API 的应用程序通过"添加 Azure API 的应用程序客户端"上下文菜单的消耗。

 
