<properties
    pageTitle="在 Azure 应用程序服务的企业级的 WordPress |Microsoft Azure"
    description="了解如何在 Azure 应用程序服务企业级的 WordPress 网站宿主"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>企业级的 WordPress 在 Azure 应用程序服务

Azure 应用程序服务提供了一个可伸缩的、 安全的和易于使用的环境，以便使命关键的大比例[WordPress] [wordpress]站点。 Microsoft 本身运行[Office]企业级网站[officeblog]和[Bing] [bingblog]博客。 本文档演示如何使用 Azure 应用程序服务 Web 应用程序建立和维护一个企业级的、 基于云的 WordPress 站点可以处理大量的访问者。

## <a name="architecture-and-planning"></a>体系结构和规划

基本的 WordPress 安装有只有两个要求。

* **MySQL 数据库**-可通过[Azure 市场 ClearDB][cdbnstore]，或您可以管理自己的 MySQL 安装在 Azure 虚拟机使用[Windows] [mysqlwindows]或[Linux][mysqllinux]。

  > [AZURE.NOTE] ClearDB 提供几个 MySQL 配置，为每个配置不同的性能特点。 [Azure 存储区]，请参阅[cdbnstore]到 Azure 存储中或直接在[ClearDB 网站](http://www.cleardb.com/pricing.view)上看到的产品的信息。

* **为 5.2.4 PHP 或更高版本**-Azure 应用程序服务目前提供[PHP 版本 5.4 和 5.5 5.6][phpwebsite]。

    > [AZURE.NOTE] 我们建议始终在 PHP 以确保您具有最新的安全修补程序的最新版本上运行。

### <a name="basic-deployment"></a>基本的部署

使用的只是基本要求，您可以创建在 Azure 的区域内的基本解决方案。

![Azure 的 web 应用程序和 MySQL 数据库承载于单个 Azure 区域][basic-diagram]

这将允许您扩展您的应用程序通过创建多个站点的 Web 应用程序实例，而一切都承载在特定的地理区域中的数据中心内。 在此区域之外访问者可能会看到响应较慢问题时使用该网站，并在此区域中的数据中心会下降，如果如此您的应用程序。


### <a name="multi-region-deployment"></a>多区域部署

使用 Azure 的[流量管理器][trafficmanager]，就可以跨多个地理区域的 WordPress 站点扩展同时为访问者提供一个 URL。 所有访问者进入通过流量管理器，然后经过向基于负载平衡配置的区域。

![驻留在多个地区，使用 CDBR 高可用性路由器区域间路由到 MySQL 的 Azure 的 web 应用程序][multi-region-diagram]

每个地区，WordPress 站点将仍然放在多个 Web 应用程序实例，但此缩放区域特定的;高通讯量区域低流量的不同进行缩放。

可以复制和路由到多个 MySQL 数据库使用 ClearDB 的[CDBR 高可用性路由器][ cleardbscale] （如左边所示） 或[MySQL 群集 CGE][cge]。

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>多区域部署的介质存储和缓存
如果该站点接受上载，或者主机的媒体文件，则使用 Azure Blob 存储。 如果您需要缓存，请考虑[Redis 缓存][rediscache]，[还云](http://azure.microsoft.com/gallery/store/garantiadata/memcached/)、 [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)，或在[Azure 存储](http://azure.microsoft.com/gallery/store/)中其他缓存产品之一。

![Azure 的 web 应用程序中，驻留在多个地区，使用 MySQL，与托管缓存、 Blob 存储和 CDN CDBR 高可用性路由器][performance-diagram]

Blob 存储是地理分布区域在默认情况下，您不必担心会在所有站点之间复制文件。 您还可以启用 Azure[内容分发网络 (CDN)] [cdn]为 Blob 存储将分发文件结束节点靠近您的访问者。

### <a name="planning"></a>规划

#### <a name="additional-requirements"></a>其他要求

要这样做... | 使用此...
------------------------|-----------
**上载或将大型文件存储** | [用于使用 Blob 存储的 WordPress 插件。][storageplugin]
**发送电子邮件** | [SendGrid] [storesendgrid]和[SendGrid 使用的 WordPress 插件][sendgridplugin]
**自定义域名** | [在 Azure 应用程序服务中配置自定义域名][customdomain]
**HTTPS** | [在 Azure 应用程序服务 web 应用程序启用 HTTPS][httpscustomdomain]
**生产前验证** | [设置临时在 Azure 应用程序服务 web 应用程序的环境][staging] <p>切换 web 应用程序从临时生产还将 WordPress 配置。 您应该确保切换调试应用程序投入生产之前所有设置将都更新为您的生产应用程序的要求。</p>
**监视和故障排除** | [启用诊断日志记录在 Azure 应用程序服务 web 应用程序的][log]和[监视 Web 应用程序在 Azure 应用程序服务][monitor]
**部署网站** | [部署 web 应用程序在 Azure 应用程序服务][deploy]

#### <a name="availability-and-disaster-recovery"></a>可用性和灾难恢复

要这样做... | 使用此...
------------------------|-----------
**负载平衡站点**或**地区的分发网站** | [通信路由使用 Azure 通信管理器][trafficmanager]
**备份和还原** | [备份在 Azure 应用程序服务 web 应用程序][backup]和[还原在 Azure 应用程序服务 web 应用程序][restore]

#### <a name="performance"></a>性能

在云环境中的性能主要通过缓存和扩张;但是应考虑内存、 带宽和承载的 Web 应用程序的其他属性。

要这样做... | 使用此...
------------------------|-----------
**了解应用程序服务实例功能** | [定价详细信息，包括应用程序服务层中的功能][websitepricing]
**缓存的资源** | [缓存的 redis][rediscache]，[还云](/gallery/store/garantiadata/memcached/)、 [MemCachier](/gallery/store/memcachier/memcachier/)，或一个[Azure 存储区](/gallery/store/)中的其他缓存选项
**扩展您的应用程序** | [在 Azure 应用程序服务中扩展 web 应用程序][websitescale]和[ClearDB 高可用性路由][cleardbscale]。 如果您选择承载和管理您自己安装 MySQL，您应该考虑[MySQL 群集 CGE] [cge]为向外扩展

#### <a name="migration"></a>迁移

有两种方法将现有的 WordPress 站点迁移到 Azure 应用程序服务。

* ** [WordPress 导出][ export] ** -导出内容的博客，然后可导入到新的 WordPress 站点上使用[WordPress 的导入程序插件]的 Azure 应用程序服务[import]。

    > [AZURE.NOTE] 虽然此过程允许您迁移您的内容，它不会迁移任何插件、 主题或其他自定义设置。 这些必须手动重新安装。

* **手动迁移** - [备份您的网站][wordpressbackup]和[数据库][wordpressdbbackup]，然后手动将其还原到 Azure 应用程序服务 web 应用程序和关联 MySQL 数据库迁移高度自定义的站点并避免手动安装插件、 主题和其他自定义项的烦闷。

## <a name="step-by-step-instructions"></a>分步指导

### <a name="create-a-wordpress-site"></a>创建 WordPress 站点

1. 使用[Azure 市场][cdbnstore]创建一个 MySQL 数据库大小的确定在[体系结构和规划](#planning)部分中的本在将承载网站。

2. 按照[创建 Azure 应用程序服务中一个 WordPress web 应用程序]中的步骤[createwordpress]创建 WordPress web 应用程序。 在创建 web 应用程序时，选择**使用现有 MySQL 数据库**，并选择在步骤 1 中创建的数据库。

如果您正在迁移现有的 WordPress 站点，创建新的 web 应用程序之后，看到[迁移现有的 WordPress 站点到 Azure](#Migrate-an-existing-WordPress-site-to-Azure) 。

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>将现有的 WordPress 站点迁移到 Azure

如在[体系结构和规划](#planning)部分中所述，有两种方法的 WordPress 网站迁移。

* **导出和导入**的站点而不必多自定义，或者只是要将内容移走。

* **备份和还原**-为进行大量自定义的网站想要移动的所有内容。

使用以下各节之一迁移您的站点。

#### <a name="the-export-and-import-method"></a>导出和导入方法

1. 使用[WordPress 导出][export]导出您的现有网站。

2. 创建 web 应用程序使用[的 WordPress 站点创建](#Create-a-new-WordPress-site)部分中的步骤。

3. 登录到您在 Web 应用程序上的 WordPress 站点并单击**插件** -> **新添**。 搜索并安装， **WordPress 的导入程序**插件。

4. 导入程序插件程序安装后，单击**工具** -> **导入**，然后选择**WordPress**使用 WordPress 的导入程序插件。

5. 在**导入 WordPress**页上，单击**选择文件**。 浏览到您现有的 WordPress 站点，从导出的 WXR 文件，然后选择**上载文件和导入**。

6. 单击**提交**。 将提示您导入成功。

8. 在[Azure 门户]中完成所有这些步骤后，重新启动您的网站从其 web 应用程序刀片式服务器[mgmtportal]。

导入网站，完成后可能需要执行以下步骤来启用不包含在导入文件中的设置。

如果您在使用此命令。. | 执行此操作。.
------------------ | ----------
**固定链接** | 从新站点的 WordPress 的仪表板，请单击**设置** -> **固定链接**，然后更新该固定链接结构
**图像/媒体链接** | 若要更新到新的位置的链接，请使用[天鹅绒的蓝色更新 Url 插件][velvet]，搜索和替换工具，或在您的数据库中手动
**主题** | 转到**外观** -> **主题**和更新所需站点主题
**菜单** | 如果您的主题支持菜单，链接到您的主页上仍可能必须嵌入在它们该旧子目录。 转到**外观** -> **菜单**，并对其进行更新

#### <a name="the-backup-and-restore-method"></a>备份和恢复方法

1. 备份您现有的 WordPress 网站使用的信息，在[WordPress 备份][wordpressbackup]。

2. 备份现有数据库[备份您的数据库]在使用信息[wordpressdbbackup]。

3. 创建一个数据库，还原备份。

    1. 从[Azure 市场上]购买一个新的数据库[cdbnstore]，或设置 MySQL 数据库在[Windows] [mysqlwindows]或[Linux] [mysqllinux]虚拟机。

    2. 使用像[MySQL 工作台]MySQL 客户端[workbench]、 连接到新的数据库并导入 WordPress 数据库。

    3. 更新要更改到新 Azure 应用程序服务域的域条目的数据库。 例如，mywordpress.azurewebsites.net。 使用[搜索和替换的 WordPress 数据库脚本][searchandreplace]安全地更改所有实例。

4. 在 Azure 的门户网站中创建 web 应用程序和发布的 WordPress 备份。

    1. 在[Azure 的门户网站]中创建 web 应用程序[mgmtportal]与使用**新建**数据库 -> **Web + 移动** -> **Azure 市场** -> **Web 应用程序** -> **Web 应用程序 + SQL** （或**Web 应用程序 + MySQL**）->**创建**。 配置所有必需的设置来创建一个空的 web 应用程序。

    2. 在 WordPress 备份中，找到**wp config.php**文件，并在编辑器中打开它。 以下条目替换为新的 MySQL 数据库的信息。

        * **DB_NAME**的数据库的用户名

        * **DB_USER** -用于访问数据库的用户名称

        * **DB_PASSWORD** -用户密码

        之后更改这些条目，保存并关闭**wp config.php**文件。

    3. 使用[部署 web 应用程序在 Azure 应用程序服务][deploy]信息使部署方法想要使用，然后再将 WordPress 备份部署到您的 web 应用程序在 Azure 应用程序服务。

5. 一旦部署了 WordPress 站点，您应该能够访问新的网站 （作为应用程序服务 web 应用程序） 使用 *。 azurewebsite.net 的站点的 URL。

### <a name="configure-your-site"></a>配置您的网站

创建 WordPress 站点或将其迁移后，可以使用以下信息来提高性能或启用其他功能。

要这样做... | 使用此...
------------- | -----------
**设置应用程序服务计划模式、 大小和启用缩放** | [在 Azure 应用程序服务中扩展 web 应用程序][websitescale]
**启用持久性数据库连接** | 默认情况下，WordPress 不使用持久性数据库的连接，这可能会使您连接到数据库成为调节后的多个连接。 若要启用持续连接，安装 （持续连接适配器插件） [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]。 
**提高性能** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">禁用的 ARR cookie</a> -可以提高性能，当运行多个 Web 应用程序实例的 WordPress</p></li><li><p>启用缓存。 <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis 高速缓存</a>（预览） 可以使用<a href="https://wordpress.org/plugins/redis-object-cache/">Redis 对象缓存 WordPress 插件</a>，或使用<a href="/gallery/store/">Azure 存储</a>从其他缓存产品之一</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">如何更快地进行 WordPress Wincache</a> -Wincache 是默认启用的 Web 应用程序的</p></li><li><p><a href="../web-sites-scale/">规模在 Azure 应用程序服务 web 应用程序</a>和使用<a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB 高可用性路由</a>或<a href="http://www.mysql.com/products/cluster/">MySQL 群集 CGE</a></p></li></ul>
**用于存储 blob** | <ol><li><p><a href="../storage-create-storage-account/">创建一个 Azure 存储帐户</a></p></li><li><p>了解如何<a href="../cdn-how-to-use/">使用内容分发网络 (CDN)</a>对地理的分布存储 blob 中的数据。</p></li><li><p>安装和配置<a href="https://wordpress.org/plugins/windows-azure-storage/">的 WordPress 插件的 Azure 存储</a>。</p><p>有关详细的安装和配置插件的信息，请参阅<a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">用户指南</a>。</p> </li></ol>
**启用电子邮件** | 启用<a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a>使用 Azure 存储。 安装 WordPress <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid 插件</a>。
**配置自定义域名** | [在 Azure 应用程序服务中配置自定义域名][customdomain]
**自定义域名启用 HTTPS** | [在 Azure 应用程序服务 web 应用程序启用 HTTPS][httpscustomdomain]
**负载平衡或地理-分发您的站点** | [路由的通信使用 Azure 流量管理器][trafficmanager]。 如果您正在使用自定义的域，请参阅[配置 Azure 应用程序服务中的自定义域名][customdomain]有关使用具有自定义域名流量管理器信息
**启用自动的备份** | [备份在 Azure 应用程序服务 web 应用程序][backup]
**启用诊断日志记录** | [启用诊断日志记录的 Azure 应用程序服务中的 web 应用程序][log]

## <a name="next-steps"></a>下一步行动

* [WordPress 优化](http://codex.wordpress.org/WordPress_Optimization)

* [WordPress 转换在 Azure 应用程序服务的多站点](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB 升级 Azure 的向导](http://www.cleardb.com/store/azure/upgrade)

* [宿主的 WordPress 在 Azure 应用程序服务 web 应用程序的某个子文件夹中](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [分步︰ 创建使用 Azure 的 WordPress 站点](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [主机在 Azure 上的现有 WordPress 博客](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [启用在 WordPress 的漂亮固定链接](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [如何迁移和 Azure 应用程序服务上运行您的 WordPress 博客](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [如何运行 WordPress 免费 Azure 应用程序服务](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress 在 Azure 在 2 分钟或更短](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [WordPress 博客转向 Azure-第 1 部分︰ 在 Azure 上创建 WordPress 博客](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [WordPress 博客转向 Azure-第 2 部分︰ 传输您的内容](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [WordPress 博客转向 Azure-第 3 部分︰ 设置您自定义的域](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [移动到 Azure 的部分 4 的 WordPress 博客︰ 几乎固定链接和 URL 重写规则](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [WordPress 博客转向 Azure 的一部分 5︰ 将子文件夹移到根](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [如何设置 Azure 帐户中一个 WordPress web 应用程序](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping 在 Azure 上 WordPress 向上](http://www.johnpapa.net/wordpress-on-azure/)

* [在 Azure 的 WordPress 的技巧](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
