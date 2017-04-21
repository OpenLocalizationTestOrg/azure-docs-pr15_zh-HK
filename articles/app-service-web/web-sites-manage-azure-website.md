<properties 
    pageTitle="管理 web 应用程序在 Azure 应用程序服务" 
    description="用于管理 web 应用程序在 Azure 应用程序服务的资源的链接。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>管理 web 应用程序在 Azure 应用程序服务

本主题包含有关管理在[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)web 应用程序的资源的链接。 管理包括的所有任务，使您的 web 应用程序平稳运行。 

Web 应用程序的生命周期，将从初始部署移动到正常操作、 维护和更新执行不同的管理任务。

许多 web 应用程序管理任务可以在 Azure 门户中执行。

## <a name="before-you-deploy-your-web-app-to-production"></a>将您的 web 应用程序部署到生产环境之前

### <a name="choose-a-tier"></a>选择一个层

Azure 应用程序服务将提供 5 个层次︰ 释放，共享、 基本、 标准和特优。 有关功能和定价每一层的信息，请参阅[定价详细信息](/pricing/details/app-service/)。 

- [应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)使您可以组合多个 web 应用程序在同一层。
- 始终可以[切换层](web-sites-scale.md)后创建 web 应用程序。

### <a name="configuration"></a>配置

使用[Azure 门户](https://portal.azure.com/)设置不同的配置选项。 有关详细信息，请参阅[在 Azure 应用程序服务的配置 web 应用程序](web-sites-configure.md)。 下面是一个快速的检查表︰

- 如果需要请选择**运行时版本**的.NET、 PHP、 Java 或 Python。
- 如果您的 web 应用程序使用 WebSocket 协议，允许**Websocket** 。 （这包括应用程序使用[ASP.NET 那么 SignalR](http://www.asp.net/signalr)或[socket.io](web-sites-nodejs-chat-app-socketio.md)）。
- 您正在运行 web 连续作业吗？ 如果是这样，**总是在上**启用。
- 设置**默认文档**，如 index.html。

除了这些基本配置设置，您可以进行以下配置︰

- **安全套接字层 (SSL)**加密。 若要自定义域名使用 SSL，必须获取 SSL 证书并配置您的 web 应用程序要使用它。 请参阅[在 Azure 应用程序服务 web 应用程序启用 HTTPS](web-sites-configure-ssl-certificate.md)。
- **自定义的域的名称。** 您的 web 应用程序都自动获得在 azurewebsites.net 下的子域。 您可以将一个自定义的域名，如 contoso.com 相关联。 请参阅[配置自定义域名在 Azure 应用程序服务](web-sites-custom-domain-name.md)。

语言特定的配置︰

- **PHP**︰[配置 PHP 在 Azure 应用程序服务 Web 应用程序中的](web-sites-php-configure.md)。
- **Python**: [Python Azure 应用程序服务 Web 应用程序配置](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>运行您的 web 应用程序时

在您的 web 应用程序运行时，要确保它不可用，并且它可以扩展以满足用户的通信。 您可能还需要解决错误。

### <a name="monitoring"></a>监视

- 通过 Azure 门户，您可以如 CPU 使用率和数量的客户端请求[添加的性能指标](web-sites-monitor.md)。
- [扩展您的 web 应用程序](web-sites-scale.md)以响应通信。 根据您的层，您可以扩展虚拟机的数量和/或虚拟机实例的大小。 在标准和特优层中，您还可以设置自动缩放，以便固定的时间表，或在加载您的 web 应用程序，自动缩放。  
 
### <a name="backups"></a>备份

- 设置 web 应用程序的[自动备份](web-sites-backup.md)。 了解更多有关在[此视频中](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/)的备份。
- 了解如何在 SQL Azure 数据库中的[数据库恢复](../sql-database/sql-database-business-continuity.md)的选项。

### <a name="troubleshooting"></a>故障排除

- 如果出现错误，您可以[在 Visual Studio 中的疑难解答](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)，请使用诊断日志并在云中的实时调试。 
- 在 Visual Studio 中外, 有多种方式来收集诊断日志。 请参阅[启用在 Azure 应用程序服务 web 应用程序的诊断日志记录](web-sites-enable-diagnostic-log.md)。
- 对于 Node.js 应用程序，请参阅[如何调试 Node.js web 应用程序在 Azure 应用程序服务](web-sites-nodejs-debug.md)。

### <a name="restoring-data"></a>还原数据

- [还原](web-sites-restore.md)以前备份 web 应用程序。


## <a name="when-you-update-your-web-app"></a>当您更新您的 web 应用程序

如果您没有启用自动备份，您可以创建[手动备份](web-sites-backup.md)。

请考虑使用[分阶段的部署](web-sites-staged-publishing.md)。 此选项允许您将更新发布到运行并排放置临时部署到生产部署。 

如果您使用 Visual Studio 的团队服务，您可以设置从源代码管理连续部署︰

- [使用 Team Foundation 版本控制 (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [使用 Git](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
