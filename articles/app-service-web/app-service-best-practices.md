<properties
    pageTitle="Azure 应用程序服务的最佳做法"
    description="了解最佳实践和对 Azure 应用程序服务进行故障诊断。"
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
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Azure 应用程序服务的最佳做法

这篇文章总结了使用[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)的最佳做法。 

## <a name="colocation"></a>但是
当编写一个 web 应用程序和数据库之类的解决方案的 Azure 资源位于不同地区的效果可以如下所示︰

*  增加的资源之间的通信的延迟
*  货币费用用于出站数据传输跨地区在[Azure 定价页](https://azure.microsoft.com/pricing/details/data-transfers)所述。

在同一个地区也存在最好 Azure 的资源编写 web 应用程序，用来存放内容或数据的数据库或存储帐户之类的解决方案。 当创建的资源，则应确保它们是 Azure 的同一个区域中，除非您有特定的业务或设计原因他们不能。 通过利用[应用程序服务克隆功能](app-service-web-app-cloning-portal.md)当前可用于计划的高级应用程序服务的应用程序，您可以移动到同一区域，为您的数据库应用程序服务的应用程序。   

## <a name="memoryresources"></a>当应用程序占用的内存比预期
如果您注意到某个应用程序消耗更多内存比预期通过监视所述或服务建议考虑[应用程序服务自动修复功能](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites)。 自动修复功能的选项之一采取基于内存阈值的自定义操作。 操作通过回收辅助进程范围光谱与内存转储到点上缓解通过调查电子邮件通知。 可以通过友好的用户界面中所述在该博客文章为[应用程序服务支持站点扩展](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)和通过 web.config 配置自动修复。   

## <a name="CPUresources"></a>当应用程序消耗了比预期更多的 CPU
通过监视所述，当您发现应用程序会消耗更多 CPU 多于预期，或者遇到重复 CPU 峰值或服务建议考虑扩大或横向扩展的应用程序服务计划。 如果您应用程序 statefull，向上扩展是唯一的选择，而如果您的应用程序是无状态的缩放出将为您提供更多灵活性和更高比例的可能性。 

有关"statefull"vs"无状态"应用程序，您可以观看这段视频︰[规划 Microsoft Azure Web 应用程序中的可扩展的端到端多层应用程序](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid)。 阅读有关应用程序服务缩放和自动缩放选项的详细信息︰[比例在 Azure 应用程序服务 Web 应用程序](web-sites-scale.md)。  

## <a name="socketresources"></a>当套接字资源将会耗尽
耗尽的出站 TCP 连接的一个常见原因是使用的客户端库，其中未实现重用的 TCP 连接，或者就如 HTTP-保持不利用较高级别的协议。 请检查每个库在您的应用程序服务的规划以确保它们是配置或者访问代码中的出站连接的有效重用的应用程序所引用的文档。 此外遵循正确创建和发布或清理若要避免泄漏连接库文档指南。 而通过横向扩展到多个实例，可以缓解进度影响进行调查，此类客户端库。  

## <a name="appbackup"></a>当您的应用程序备份启动失败
为什么应用程序备份失败的两个最常见的原因︰ 无效存储设置和无效的数据库配置。 这些失败通常发生这种情况发生时如何访问这些资源 （如备份设置中选定的数据库更新凭据） 或者更改存储或数据库资源。 备份通常按计划运行，并且需要访问 （对于输出已备份文件） 存储和数据库 （用于复制和读取要包含在备份中的内容）。 无法访问这些资源的结果是一致的备份失败。 

当发生这种情况引起的备份失败时，请查看最新的结果，以弄清发生了哪种类型的故障。 存储访问失败，请检查并更新存储设置备份配置中使用。 数据库访问失败，请检查和更新您的连接字符串作为应用程序的设置;然后继续更新备份配置正确包含所需的数据库。 有关应用程序备份的详细信息，请参阅[备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)的文档。

## <a name="nodejs"></a>当新的 Node.js 应用程序会部署到 Azure 应用程序服务
Azure 应用程序服务默认配置为 Node.js 应用程序是最适合的最常见的应用程序的需要。 如果 Node.js 应用程序的配置将受益于个性化的优化以提高性能、 优化 CPU/内存/网络资源的资源使用率，无法查看我们的最佳做法和排除故障的步骤。 此文档介绍了您可能需要配置，Node.js app 的、 描述各种方案或发出您的应用程序可能面临并说明如何解决这些问题的 iisnode 设置︰[最佳做法和节点上 Azure 应用程序服务的应用程序的故障排除指南](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)。   


