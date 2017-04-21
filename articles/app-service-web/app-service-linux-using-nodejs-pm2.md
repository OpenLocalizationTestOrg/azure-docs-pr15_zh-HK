<properties 
    pageTitle="PM2 配置用于在 Web 应用程序在 Linux 上的 NodeJS |Microsoft Azure" 
    description="对 NodeJS 在 Linux 上的 Web 应用程序中使用 PM2 配置" 
    keywords="azure 应用程序服务、 web 应用程序、 nodejs、 pm2、 linux、 oss"
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

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>用于在 Web 应用程序在 Linux 上的 Node.js PM2 配置

如果 Web 应用程序在 Linux 上对 Node.js 设置应用程序堆栈，则会设置 Node.js 启动文件，如下面的图像所示的选项。

![][1]

您可以使用它为

-   指定启动脚本的 Node.js 应用程序 (例如︰ /bin/server.js)
-   指定要用于 Node.js app PM2 配置文件 (例如︰ /foo/process.json)

 >[AZURE.NOTE] 如果您希望您的节点过程修改某些文件时自动重新启动，您将需要使用 PM2 配置。 否则您的应用程序不会重新启动您的应用程序代码更改时从诸如连续部署接收更改通知时。

您可以检查 Node.js[处理文件文档](http://pm2.keymetrics.io/docs/usage/application-declaration/)的所有选项，但下面是您将使用作为您的 process.json 文件的示例

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

在此配置中注意重要的事情是 

-   "脚本"属性指定应用程序的启动脚本。
-   使用"实例"属性可以指定节点进程启动的多少个实例。 如果 VM 大小具有多个内核上运行您的应用程序，您希望通过在此处设置的值最大化您的资源。
-   "监视"数组指定您要为其更改重新启动节点进程的所有文件。
-   "Watch_options"，目前需要用户安装应用程序内容的方式为 true，则指定"usePolling"。


## <a name="next-steps"></a>下一步行动 ##

* [在 Linux 上的应用程序服务是什么？](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png