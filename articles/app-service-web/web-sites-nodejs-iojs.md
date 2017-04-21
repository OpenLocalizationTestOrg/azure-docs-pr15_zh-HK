<properties 
    pageTitle="如何使用 io.js Azure 应用程序服务 Web 应用程序" 
    description="了解如何在 Azure 应用程序服务 web 应用程序使用 io.js。" 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>如何使用 io.js Azure 应用程序服务 Web 应用程序

常用的节点分叉[io.js]功能到 Joyent 的 Node.js 项目，其中包括更加开放的治理模型、 速度更快的发布周期和更快地采用新的和实验性的 JavaScript 功能的各种差异。

虽然[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序有许多预装的 Node.js 版本，它还允许用户提供的 Node.js 二进制文件。 本文讨论了实现的 io.js 应用程序服务 Web 应用程序上使用的两种方法︰ 使用的扩展的部署脚本，这会自动将配置 Azure 使用最新的可用的 io.js 版本，以及二进制 io.js 手动上载。 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>使用部署脚本

在 Node.js 应用程序的部署之后，应用程序服务 Web 应用程序运行许多小的命令，以确保正确配置了环境。 使用部署脚本，这一过程可以自定义要包含的下载和配置的 io.js。

[Io.js 部署脚本](https://github.com/felixrieseberg/iojs-azure)出现在 GitHub 上。 若要启用您的 web 应用程序上的 io.js，只需将**.deployment**、 **deploy.cmd**和**IISNode.yml**复制到应用程序文件夹的根目录，将部署到 Web 应用程序。  

第一个文件， **.deployment**，指示 Web 应用程序运行在部署时的**deploy.cmd** 。 此脚本运行所有的通常步骤 Node.js 应用程序，但还可以下载最新版本的 io.js。 最后， **IISNode.yml**配置 Web 应用程序使用仅下载的 io.js 二进制而不是预先安装的 Node.js 二进制文件。

> [AZURE.NOTE] 要更新使用的 io.js 二进制文件，就可以重新部署您的应用程序的脚本将下载新版本的 io.js 每次部署应用程序。

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>使用手动安装

手动安装自定义 io.js 版本包括两个步骤。 首先，直接从[io.js 分发]下载二进制**win x64** 。 需要有两个文件的**iojs.exe**和**iojs.lib**。 将这两个文件保存到您的 web 应用程序，例如在**bin/iojs**内部的文件夹。

要配置 Web 应用程序，而不是预先安装的节点版本使用**iojs.exe** ，您的应用程序的根目录下创建一个**IISNode.yml**文件，添加以下行。

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>下一步行动

在本文中，您学习了如何使用与应用程序服务 Web 应用程序，同时使用 io.js 提供的部署脚本手动安装。 

> [AZURE.NOTE] io.js 在大开发中，Node.js 比经常更新。 多个 Node.js 模块可能不适用于 io.js-请咨询[io.js 在 GitHub 上的]进行故障排除。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

[io.js]: https://iojs.org
[io.js 通讯组]: https://iojs.org/dist/
[在 GitHub 上的 io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 