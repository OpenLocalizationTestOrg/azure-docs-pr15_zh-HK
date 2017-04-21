<properties 
    pageTitle="如何在 Linux 上的应用程序服务创建一个 Web 应用程序 |Microsoft Azure" 
    description="Web 应用程序创建工作流应用程序在 Linux 上的服务。" 
    keywords="azure 应用程序服务、 web 应用程序、 linux、 oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>创建 Web 应用程序与在 Linux 上的应用程序服务

## <a name="using-the-management-portal-to-create-your-web-app"></a>使用管理门户创建 web 应用程序
您可以开始创建您的 Web 应用程序在 Linux 上从[管理门户](https://portal.azure.com)中下面的图像所示。

![][1]

一旦您选择下面的选项时，将显示帮助您创建刀片式服务器下面的图像所示。 

![][2]

-   为您的 web 应用程序指定一个名称。
-   现有资源组中选择或创建一个新。 （请参见区域[限制一节](./app-service-linux-intro.md)中提供）。
-   选择一个现有的应用程序服务计划或创建一个新一 （请参见应用程序服务计划中说明[限制一节](./app-service-linux-intro.md)）。 
-   选择您要使用的应用程序堆栈。 您将得到多个 Node.js 版本和 PHP 之间进行选择。 

创建应用程序之后，您可以更改应用程序设置中的应用程序堆栈，如下面的图像所示。

![][3]

## <a name="deploying-your-web-app"></a>部署 web 应用程序

从管理门户中选择"部署选项"使您可以使用本地 Git 存储库或 GitHub 资料库部署应用程序的选项。 此后的说明同样为非 Linux web 应用程序，可以按照 GitHub[本地 Git 部署](./app-service-deploy-local-git.md)我们或我们的[连续部署](./app-service-continuous-deployment.md)文章中的说明。

您还可以使用 FTP 上载到您的站点的应用程序。 从诊断日志部分下面的图像所示，可以为您的 web 应用程序获取 FTP 端点。

![][4]


## <a name="next-steps"></a>下一步行动 ##

* [在 Linux 上的应用程序服务是什么？](./app-service-linux-intro.md)
* [用于在 Web 应用程序在 Linux 上的 Node.js PM2 配置](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
