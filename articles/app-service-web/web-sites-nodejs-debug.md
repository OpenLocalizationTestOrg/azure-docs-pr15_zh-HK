<properties
    pageTitle="如何调试 Node.js web 应用程序在 Azure 应用程序服务"
    description="了解如何调试 Node.js web 应用程序在 Azure 应用程序服务。"
    tags="azure-portal"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>如何调试 Node.js web 应用程序在 Azure 应用程序服务

Azure 提供内置诊断程序，以帮助进行调试 Node.js [Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序中承载的应用程序。 在本文中，您将学习如何启用日志记录标准输出和 stderr，在浏览器中显示错误的信息以及如何下载并查看日志文件。

由[IISNode]提供，Node.js 承载的应用程序在 Azure 上诊断。 本文讨论了用于收集诊断信息的最常用的设置，而它不使用 IISNode 提供的完整参考资料。 使用 IISNode 的详细信息，请参阅在 GitHub 上的[IISNode 自述文件]。

<a id="enablelogging"></a>
## <a name="enable-logging"></a>启用日志记录

默认情况下，应用程序服务 web 应用程序仅捕获部署，例如在部署 web 应用程序使用 Git 的诊断信息。 如果您在部署期间，如故障模块引用**package.json**，在安装时有问题，或如果您要使用自定义部署脚本，则此信息很有用。

若要启用日志记录的标准输出和 stderr 流，必须 Node.js 应用程序的根目录下创建一个**IISNode.yml**文件并添加下列代码︰

    loggingEnabled: true

这使 stderr 和从 Node.js 应用程序的标准输出的记录。

**IISNode.yml**文件可以还能用于控制是否友好错误或开发人员错误返回给浏览器发生故障时。 若要使开发人员错误，请将下行添加到**IISNode.yml**文件︰

    devErrorsEnabled: true

启用此选项后，IISNode 将返回最后一个 64k 发送到 stderr 而不是一个友好的错误，如"发生内部服务器错误"的信息。

> [AZURE.NOTE] 虽然 devErrorsEnabled 是有用的在开发过程中诊断问题时，使其在生产环境中可能会导致开发错误发送给最终用户。

如果**IISNode.yml**文件尚不存在在您的应用程序中，在发布更新的应用程序后必须重新启动您的 web 应用程序。 如果只更改现有的**IISNode.yml**文件中以前发布的设置，则不重新启动是必需的。

> [AZURE.NOTE] 如果您的 web 应用程序使用 Azure PowerShell Cmdlet 的 Azure 命令行工具创建的默认的**IISNode.yml**文件是自动创建的。

若要重新启动 web 应用程序，在[Azure 门户](https://portal.azure.com)中，选择 web 应用程序，然后单击**重新启动**按钮︰

![重新启动按钮][restart-button]

如果在开发环境中安装了 Azure 命令行工具，可以使用以下命令重新启动该 web 应用程序︰

    azure site restart [sitename]

> [AZURE.NOTE] 尽管 loggingEnabled 和 devErrorsEnabled 是最常使用 IISNode.yml 配置选项用于捕获的诊断信息，IISNode.yml 可用于配置各种选项为宿主环境。 有关配置选项的完整列表，请参阅[iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml)文件。

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>访问日志

诊断日志可通过三种方式;使用文件传输协议 (FTP) 下载 Zip 存档，或作为实时更新日志 （也称为尾巴） 的流。 下载日志文件的 Zip 存档文件或查看实时流需要 Azure 命令行工具。 这些可以通过使用下面的命令来安装︰

    npm install azure-cli -g

安装后，可以使用 azure 命令访问这些工具。 必须首先配置命令行工具使用 Azure 订购。 有关如何完成此任务的信息，请参阅**如何下载并导入发布设置**[如何使用 Azure 命令行工具](../xplat-cli-connect.md)文章部分。

###<a name="ftp"></a>FTP

要通过 FTP 访问的诊断信息，请访问[Azure 门户](https://portal.azure.com)，选择 web 应用程序，，然后选择**仪表板**。 在**快速链接**部分中， **FTP 诊断日志**和**提取诊断日志**链接可以使用 FTP 协议的日志。

> [AZURE.NOTE] 如果您还没有以前配置的用户名和密码 FTP 或部署，您可以通过选择**设置部署凭据**从**快速入门**管理页面做到这样。

返回在仪表板中的 FTP URL 是**日志文件**目录，请将包含以下子目录︰

* [部署方法](web-sites-deploy.md)-如果使用 Git 的部署方法，具有相同名称的目录将被创建，并且将包含与部署相关的信息。

* nodejs 的标准输出和 stderr 信息捕获从您的应用程序的所有实例 （如果 loggingEnabled 也是如此。）

###<a name="zip-archive"></a>Zip 存档文件

要下载 Zip 存档的诊断日志，请使用下面的命令从 Azure 命令行工具︰

    azure site log download [sitename]

这将在当前目录中的**diagnostics.zip**下载。 该存档文件包含以下目录结构︰

* 部署的部署您的应用程序有关的信息日志

* 日志文件

    * [部署方法](web-sites-deploy.md)-如果使用 Git 的部署方法，具有相同名称的目录将被创建，并且将包含与部署相关的信息。

    * nodejs 的标准输出和 stderr 信息捕获从您的应用程序的所有实例 （如果 loggingEnabled 也是如此。）

###<a name="live-stream-tail"></a>实时流 （尾）

要查看诊断日志信息的实时记录，请使用下面的命令从 Azure 命令行工具︰

    azure site log tail [sitename]

这将返回的流，会在服务器上发生更新的日志事件。 该流将返回部署信息，以及标准输出和 stderr 信息 （当 loggingEnabled 也是如此。）

<a id="nextsteps"></a>
## <a name="next-steps"></a>下一步行动

在这篇文章中您学习了如何启用和访问 Azure 的诊断信息。 有助于了解问题的应用程序出现此信息时，可能会指向一个问题与您正在使用或 Node.js 应用程序服务 Web 应用程序所使用的版本是不同于在部署环境中使用的模块。

在 Azure 上使用模块中的信息，请参阅[使用 Azure 应用程序使用的 Node.js 模块](../nodejs-use-node-modules-azure-apps.md)。

指定应用程序的 Node.js 版本信息，请参阅[指定 Node.js 版本 Azure 应用程序中]。

有关详细信息，请参见[Node.js 开发中心](/develop/nodejs/)。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode 自述]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[在 Azure 应用程序指定 Node.js 版本]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
