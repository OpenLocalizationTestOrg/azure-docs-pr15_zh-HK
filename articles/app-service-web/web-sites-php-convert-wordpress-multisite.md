<properties 
    pageTitle="WordPress 转换在 Azure 应用程序服务的多站点" 
    description="了解如何通过在 Azure 库创建对现有 WordPress web 应用程序并将其转换到 WordPress 多站点" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>WordPress 转换在 Azure 应用程序服务的多站点

## <a name="overview"></a>概述

*由[Ben Lobaugh][ben-lobaugh]， [Microsoft 打开技术公司][ms-open-tech]*

在本教程中，您将学习如何通过 Azure 中的库创建对现有 WordPress web 应用程序并将其转换为一个 WordPress 多站点的安装。 此外，您将学习如何在您的安装创建子网站的每个分配自定义的域。

假定您具有现有安装的 WordPress。 如果不这样做，请按照[创建 WordPress 网站从 Azure 中的库]中提供的指导[website-from-gallery]。

转换现有的 WordPress 单个站点安装到多站点通常是相当地简单，和许多的最初的步骤是直接来自[创建网络][wordpress-codex-create-a-network]上[WordPress](http://codex.wordpress.org)页。

我们开始吧。

## <a name="allow-multisite"></a>允许多站点

首先需要启用多站点通过`wp-config.php`文件中**WP\_允许\_多站点**常量。 若要编辑您的 web 应用程序文件的两种方法︰ 第一种是通过 FTP 和通过 git 中获取第二个。 如果您熟悉如何设置以下任一方法，请参考下面的教程︰

* [与 MySQL 和 FTP 的 PHP 网站][website-w-mysql-and-ftp-ftp-setup]

* [与 MySQL 和 Git 的 PHP 网站][website-w-mysql-and-git-git-setup]

打开`wp-config.php`文件使用您选择的编辑器并添加以下上面`/* That's all, stop editing! Happy blogging. */`线。

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

一定要保存该文件并将其上载到服务器 ！

## <a name="network-setup"></a>网络安装程序

记录在到您的站点的*wp 管理*区域的应用程序，您应该看到称为**网络设置****工具**菜单下的新项目。 单击**网络设置**，然后填写您的网络的详细信息。

![网络安装屏幕][wordpress-network-setup]

本教程使用*子目录*网站架构，因为它应该总是有效的而且我们将设置自定义的域为每个子网站在本教程后面部分。 但是，应该是可能的如果您正确地映射到[Azure 门户](https://portal.azure.com)和安装通配符 DNS 域的子域安装的安装程序。

了解更多有关子域 vs 子目录设置请参见[类型多站点网络的][wordpress-codex-types-of-networks]在 WordPress 文章。

## <a name="enable-the-network"></a>启用网络

网络现在已配置数据库中，但没有一个剩余步骤以启用网络功能。 定版`wp-config.php`设置，并确保`web.config`正确地将每个站点的路由。


单击*网络设置*页面上的**安装**按钮后，WordPress 会尝试更新`wp-config.php`和`web.config`的文件。 但是，您应始终检查文件，以确保更新成功。 如果没有，此屏幕将向您提供必要的更新。 编辑并保存文件。


后使这些更新程序，您需要注销然后登录回 wp 管理仪表板。

现在上应该有更多菜单栏标记**我的网站**管理员。 该菜单允许您控制您的新网络，通过**网络管理**仪表板。

## <a name="adding-custom-domains"></a>添加自定义的域

[WordPress MU 域映射][wordpress-plugin-wordpress-mu-domain-mapping]插件使它轻松将自定义的域添加到您的网络中的任何站点。 为了使该插件才能正常运行，您需要执行一些附加的安装在门户网站中，以及在您的域注册。

## <a name="enable-domain-mapping-to-the-web-app"></a>启用域映射到 web 应用程序

**免费**[的应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)计划模式不支持 Web 应用程序中添加自定义的域。 您需要切换到**共享**或**标准**模式。 若要此操作︰

* 登录到 Azure 门户中，找到您的 web 应用程序。 
* 在**设置**中的**扩展**选项卡上单击。
* 在**常规**下选择*共享*或*标准*
* 单击**保存**

您可能会收到一条消息，要求您确认更改并确认您的 web 应用程序可能会立即产生成本，取决于使用情况和其他配置设置。

花几秒钟的时间来处理新的设置，因此，现在是开始设置您的域的好时机。

## <a name="verify-your-domain"></a>请验证您的域

Azure Web 应用程序将允许您将域映射到站点之前，您首先需要请确认您有权将域映射。 若要执行此操作，必须向 DNS 条目添加的新 CNAME 记录。

* 登录到您的域的 DNS 管理器
* 创建新的 CNAME *awverify*
* 指向*awverify *awverify* 。YOUR_DOMAIN.azurewebsites.net*

它可能需要一些时间才能使 DNS 更改完全生效，因此如果以下步骤不起作用，请做一杯咖啡，然后回来并再试一次。

## <a name="add-the-domain-to-the-web-app"></a>将域添加到 web 应用程序

返回到您通过 Azure 门户的 web 应用程序，单击**设置**，然后单击**自定义的域和 SSL**。

*SSL 设置*显示时，您将看到字段中将输入所有您想要将分配给您的 web 应用程序的域。 如果此处未列出的域，它不会用于映射在 WordPress，无论域 DNS 的安装程序的方式。

![管理自定义的域对话框][wordpress-manage-domains]

在文本框中键入您的域后, Azure 将验证您先前创建的 CNAME 记录。 如果 DNS 没有充分传播，将显示红色标记。 如果它成功，您将看到一个绿色的复选标记。 

记下在对话框的底部列出的 IP 地址。 您将需要此选项设置为您的域的 A 记录。

## <a name="setup-the-domain-a-record"></a>设置记录的域

如果其他步骤是否成功，您现在可能将域分配给 Azure 的 web 应用程序通过 DNS A 记录。 

请务必注意这里 Azure 的 web 应用程序接受 CNAME 和记录，但是，您*必须*使用 A 记录启用正确的域映射。 一个 CNAME 无法转发到另一个 CNAME，哪是什么 Azure 创建为您的 YOUR_DOMAIN.azurewebsites.net。

使用上一步中的 IP 地址，返回到您的 DNS 管理器，并设置为指向该 IP 的 A 记录。


## <a name="install-and-setup-the-plugin"></a>安装和设置插件

WordPress 多站点当前没有内置的方法以将自定义的域映射。 但是，没有插件调用[WordPress MU 域映射][ wordpress-plugin-wordpress-mu-domain-mapping] ，为您添加功能。 登录到您的站点的网络管理部分中并安装**WordPress MU 域映射**插件。

安装和激活插件，请访问**设置**后 > **域映射**配置插件。 在第一个文本框中，*服务器的 IP 地址*，输入用于安装的域 A 记录的 IP 地址。 设置所需的任何*域选项*（默认值通常是正常的），并单击**保存**。

## <a name="map-the-domain"></a>映射域

请访问**仪表板**站点要映射到的域。 单击**工具** > **域映射**并在文本框中键入新域并单击**添加**。

默认情况下，会将新域重新写入自动生成站点域。 如果您想要为新的域发送的所有通信，请检查之前保存*此博客的主要域*框。 可以将不限的数量的域添加到网站，但只有一个可以是主。

## <a name="do-it-again"></a>再次执行该

Azure 的 Web 应用程序允许您将任意的数量的域添加到 web 应用程序。 要添加另一个域，将需要执行的每个域的**验证您的域**并**设置域记录**部分。  

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
