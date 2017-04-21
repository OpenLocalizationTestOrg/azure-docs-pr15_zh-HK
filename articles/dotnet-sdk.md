<properties
    pageTitle="Azure.NET SDK 是什么"
    description="了解在 Azure.NET SDK 中包含的内容。"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>为使.NET Azure SDK 是什么？

## <a name="overview"></a>概述

为使.NET Azure SDK 是 Visual Studio 工具一套、 命令行工具、 运行时二进制文件，以及客户端库，可帮助您开发、 测试和部署在 Azure 上运行的应用程序。 本文详细介绍了安装 SDK 时所获取的内容。 您可以从[Azure 下载页面](https://azure.microsoft.com/downloads/)下载 SDK。

为使.NET Azure SDK 也构成[使用 Azure 服务的客户端库](http://go.microsoft.com/fwlink/?LinkId=510472)。 使用 NuGet 单独安装这些库。

##<a id="included"></a>什么是 Azure SDK 中包含.net

为使.NET Azure SDK 安装以下产品︰

- [Visual Studio 社区版 2015](#vwd)
- [Microsoft Azure 存储仿真器](#stgemulator)
- [Microsoft Azure 存储工具](#stgtools)
- [Microsoft Azure 创作工具](#auth)
- [Microsoft Azure 仿真程序](#emulator)
- [Visual Studio 和微软的 HDInsight 工具配置单元的 ODBC 驱动程序](#hdinsight)
- [.NET 的 Microsoft Azure 库](#libraries)
- [Microsoft Azure 移动应用程序 SDK](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure Microsoft Visual Studio 工具](#tools)
- [Microsoft ASP.NET 和 Visual Studio 的 Web 工具](#wte)
- [Visual Studio 的 Microsoft Azure 数据湖工具](#datalake)

###<a id="vwd"></a>Visual Studio 社区版 2015

如果您的计算机上没有 Visual Studio，SDK 将安装[Visual Studio 社区版 2015年](https://www.visualstudio.com/products/visual-studio-community-vs)。

###<a id="stgemulator"></a>Microsoft Azure 存储仿真器

[Azure 存储仿真器](http://msdn.microsoft.com/library/hh403989.aspx)使用的 SQL Server 实例和本地文件系统模拟 Azure 存储队列、 表 （blob），以便您可以在本地测试。

###<a id="stgtools"></a>Microsoft Azure 存储工具

这将安装[AzCopy](http://aka.ms/AzCopy)，一个命令行工具，可用于传输数据传入和传出的 Azure 存储帐户。

###<a id="auth"></a>Microsoft Azure 创作工具

这包括以下内容︰

* 用于创建部署包[CSPack 命令行工具](http://msdn.microsoft.com/library/gg432988.aspx)。
* 用于加密密码用来访问远程桌面连接到云服务角色实例的[CSEncrypt 命令行工具](http://msdn.microsoft.com/library/hh404001.aspx)。
* 云服务项目的运行时二进制文件要求为其运行时环境与通讯和诊断程序。 这些二进制文件不可用 NuGet 程序包中。

###<a id="emulator"></a>Microsoft Azure 仿真程序

[Azure 仿真程序](http://msdn.microsoft.com/library/dn339018.aspx)模拟，以便您可以测试云服务项目本地计算机上之前将其部署到 Azure 的云服务环境。

###<a id="hdinsight"></a>Visual Studio 和微软的 HDInsight 工具配置单元的 ODBC 驱动程序

在服务器资源管理器中的 HDInsight 工具使您能够定位配置单元数据库和 HDInsight 群集的链接的存储帐户、 创建表、 创建和提交配置单元查询。 有关详细信息，请参阅[开始使用 Visual Studio 的 HDInsight Hadoop 工具](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md)。

###<a id="libraries"></a>.NET 的 Microsoft Azure 库

这包括以下内容︰

* NuGet 程序包的 Azure 存储、 服务总线和缓存以便 Visual Studio 可以创建新的云服务项目时存储在计算机上离线。
* Visual Studio 插件，使[角色内缓存](http://msdn.microsoft.com/library/dn386103.aspx)项目若要在 Visual Studio 中本地运行。

###<a id="mobile"></a>Microsoft Azure 移动应用程序 SDK

使用[Azure 应用程序服务移动应用程序](app-service-mobile/app-service-mobile-value-prop.md)的工具。

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell 使您能够[自动化 Azure 环境创建和部署](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)。

###<a id="tools"></a>Microsoft Azure Microsoft Visual Studio 工具

这使您能够使用 Azure 的资源，主要是云服务和虚拟机︰

* [创建、 打开和发布云服务项目](cloud-services/cloud-services-dotnet-get-started.md)。
* [对于云服务项目创建部署包](http://msdn.microsoft.com/library/ff683672.aspx)。
* [创建新的 web 项目时创建 Azure 的虚拟机](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md)。
* [在创建新的虚拟机创建 PowerShell 脚本](http://msdn.microsoft.com/library/dn642480.aspx)。
* [视图和管理在 Visual Studio 项目属性窗口中的云服务项目设置](http://msdn.microsoft.com/library/ee405486.aspx)。
* 查看和管理[云服务](http://msdn.microsoft.com/library/ff683675.aspx)、[虚拟机](http://msdn.microsoft.com/library/jj131259.aspx)和[服务总线](http://msdn.microsoft.com/library/jj149828.aspx)在服务器资源管理器中。
* [在调试模式下运行远程云服务和虚拟机](http://msdn.microsoft.com/library/ff683670.aspx)。
* [自动化资源调配使用 Azure 资源组部署项目](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Visual Studio 的 Microsoft 应用程序服务工具

这使您能够使用 Azure 网站︰

* [发布到 Azure 网站的 web 项目](app-service-web/web-sites-dotnet-get-started.md)。
* [发布到 Azure WebJobs 的控制台应用程序项目](app-service-web/websites-dotnet-deploy-webjobs.md)。
* [创建 Azure 网站和 SQL 数据库资源或发布 web 项目时创建新的 web 项目](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)。
* [创建新网站时 PowerShell 创建部署脚本](http://msdn.microsoft.com/library/dn642480.aspx)。
* [管理并解决服务器资源管理器中的 Azure 网站](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement)。
* [在调试模式下运行远程网站和 WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)。

>[AZURE.NOTE] 您不必安装 Azure SDK.net 才能使用这些功能;它们也包括在 Visual Studio 的更新。

##<a id="datalake"></a>Visual Studio 的 Microsoft Azure 数据湖工具

有关详细信息，请参阅[教程︰ 开发 U SQL 脚本使用 Visual Studio 数据湖工具](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md)。

##<a id="notincluded"></a>不包括 Azure SDK 安装.NET 时

还有少数的 Azure 开发可能需要安装 SDK 时不包括在内的措施。 其中最重要的如下所示︰

* [客户端库](http://go.microsoft.com/fwlink/?LinkId=510472)。

    Azure SDK 中包含客户端库使用 Azure 服务，但是不是所有在安装时安装 SDK 时。 如果您的应用程序需要客户端库不会安装 SDK，可以从[NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)获得。 如果您的应用程序使用的客户端库，安装 SDK，最好用来更新它在 NuGet.org 的当前版本。

    **客户端库的本地副本。** .Net Azure SDK 将某些 Azure 的客户端库，如存储、 服务总线和缓存的 NuGet 程序包复制到计算机中。 这些客户端库将自动包括在新的云服务项目，因此本地的 NuGet 程序包启用 Visual Studio 创建的项目，即使您未连接到 Internet。 客户端库会通常更新比新 SDK 版本发布，更频繁，因此在 NuGet.org 的客户端库通常是更多比您所获得的 sdk。

    **项目模板，包括客户端库。** 只有[Azure 云服务](cloud-services/cloud-services-dotnet-get-started.md)和 Azure 应用程序服务[移动应用程序](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)项目模板自动包含某些客户端库。 对于其他库或其他模板，安装所需[客户端库 NuGet 程序包](http://go.microsoft.com/fwlink/?LinkId=510472)。

* [移动应用程序项目模板](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

    移动应用程序模板是仅可用于 Visual Studio 2013年更新 2 和更高版本。 他们不是可在 Visual Studio 2012 或早期版本，而不是在 Visual Studio 2013年更新 1 或更早版本，即使您安装适用于.NET Azure SDK。

##<a id="faq"></a>常见问题

- [Azure 的许多功能都已在 Visual Studio 中。是否需要安装.NET Azure SDK？](#azinvs)
- [我想要客户端库。是否需要安装.net 就 Azure SDK？](#clientlib)
- [在哪里可以找到.NET 的 Azure SDK 的较早版本？](#olderversions)
- [对于.net 的 Azure sdk 版本生命周期策略是什么？](#lifecycle)
- [来宾操作系统版本是 Azure SDK.net 兼容？](#guestos)
- [如何卸载.net 的 Azure SDK？](#uninstall)

###<a id="azinvs"></a>Azure 的许多功能都已在 Visual Studio 中。 是否需要安装.NET Azure SDK？

它是很好的做法，如果想要使用最新工具的 azure 开发安装 SDK。 如果将而不安装 SDK，如果满足以下条件时才可以进行操作︰

* 您已经安装了最新的[Visual Studio 的更新](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)。
* 在开发仅对 Azure 网站或移动服务，不是为云服务或虚拟机。
* 您的应用程序不使用存储，或使用存储但您不需要存储仿真器或 AzCopy 工具。

###<a id="clientlib"></a>我想要客户端库。 是否需要安装.net 就 Azure SDK？

SDK 安装客户端库仅使您能创建云服务项目即使您未连接到 Internet。 当前客户端库都提供了在[NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)的 NuGet 程序包。 有关详细信息，请参阅本文档前面的[内容未包括在安装.net Azure SDK 时](#notincluded)。

###<a id="olderversions"></a>在哪里可以找到.NET 的 Azure SDK 的较早版本？

对于较早版本，请参阅[.NET 的 Azure SDK](https://azure.microsoft.com/downloads/archive-net-downloads/)下载页。

###<a id="lifecycle"></a>对于.net 的 Azure sdk 版本生命周期策略是什么？

请参阅[Microsoft Azure 云服务支持生命周期策略](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq)。

###<a id="guestos"></a>来宾操作系统版本是 Azure SDK.net 兼容？

请参阅[Azure 的来宾 OS 发行版和 SDK 兼容性矩阵](http://msdn.microsoft.com/library/ee924680.aspx)。

###<a id="uninstall"></a>如何卸载.net 的 Azure SDK？

[.Net Azure SDK 中包含的内容](#included)在本文中所示的每一项都是一个单独的程序在 Windows 控制面版**程序和功能**的已安装程序的列表中。  无法卸载它们作为一个组;您必须单独卸载的每个程序。

当必须已经安装，.NET Azure SDK 并安装新版本时，则通常无需卸载旧。 在大多数情况下，安装 SDK 更新现有程序，而不是添加一个新和保留原。

但是，如果您想要删除的早期版本不长-需要残余，卸载指定的旧版本号的程序，仅卸载它们是否存在较新版本的相同程序。 例如，从 2.5 到 2.6 更新后，您可能会看到 2.5 和 2.6 版本的"Microsoft Visual Studio 2013 Microsoft Azure 的工具，"，您可以卸载 2.5 版本。 但您可能只看到 2.5 版本的"Microsoft Azure 创作工具"，并将无法安全地卸载它。

请注意显示在**程序和功能**中的程序标题中的版本号可能会引起误解。  例如，SDK 版本 2.6 包含"Microsoft Azure 移动应用程序 SDK V1.0"如果 SDK Visual Studio 2013，和"Microsoft Azure 移动应用程序 SDK V2.0"安装的 Visual Studio 2015;SDK 版本但 Visual Studio 版本程序适用于其中的指标，在这种情况下不是版本。

这篇文章不会列出每个程序，每个早期版本的 Azure SDK 包含;因为早期 SDK 版本有时也包括被省略从更高版本的程序，有早期 SDK 版本中，可以通过卸载其他程序。 例如，2.5 版安装"Azure 资源管理器工具为 Visual Studio"但不在本文中的列表中因为它不再显示为一个单独的程序，在**程序和功能**中。  这篇文章只列出.net 版本 2.6 Azure SDK 中包含的程序。

> **注意︰**一些 SDK 中包含的程序可能也分别安装在其他上下文中，即使不需要完整的 SDK 可能需要。 这可能是程序的早期 SDK 版本已安装，但从更高的 SDK 版本被省略，则返回 true。 当卸载程序时，请小心地避免删除仍需要在您的计算机上的内容。



##<a id="versions"></a>版本

要查看哪个版本是最新或下载早期版本，请参阅[Azure SDK 的.NET 版本历史记录](https://azure.microsoft.com/downloads/archive-net-downloads/)页。

##<a id="resources"></a>资源

若要下载当前的 Azure SDK 的.NET 客户端库，请参阅[Azure 下载页面](https://azure.microsoft.com/downloads/)。

Azure SDK.NET 的源代码，包括客户端库，请参阅[GitHub.com/Azure](https://github.com/azure/)。

Azure 的客户端库的参考文档，请参阅[Azure.NET 参考](https://azure.microsoft.com/documentation/api/)。
