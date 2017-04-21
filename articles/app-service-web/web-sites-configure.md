<properties 
    pageTitle="在 Azure 应用程序服务中配置 web 应用程序" 
    description="如何在 Azure 应用程序服务中配置 web 应用程序" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>在 Azure 应用程序服务中配置 web 应用程序 #

本主题说明如何配置 web 应用程序使用[Azure 门户]。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>应用程序设置

1. 在[Azure 门户]，打开刀片式服务器的 web 应用程序。
2. 单击**所有设置**。
3. 单击**应用程序设置**。

![应用程序设置][configure01]

**应用程序设置**刀片式服务器已归在多种类别的设置。

### <a name="general-settings"></a>常规设置

**框架的版本**。 如果您的应用程序使用了任何这些框架，请设置下列选项︰ 

- **.NET Framework**︰ 设置.NET framework 版本。 
- **PHP**: PHP 版本或**OFF**来禁用 PHP 设置。 
- **Java**︰ 选择的 Java 版本或**OFF**来禁用 Java。 使用**Web 容器**选项 Tomcat 和 Jetty 版本之间进行选择。
- **Python**︰ 选择**OFF**来禁用 Python 的 Python 版本。

由于技术原因，为您的应用程序启用 Java 禁用.NET，PHP，Python 选项。

<a name="platform"></a>
**平台**。 选择是否在 32 位或 64 位环境中运行您的 web 应用程序。 在 64 位环境要求基本或标准的模式。 释放并共享模式始终运行在 32 位环境中。

**Web 套接字**。 设置为**ON** ，以便 WebSocket 协议;例如，如果您的 web 应用程序使用[ASP.NET 那么 SignalR]或[socket.io]。

<a name="alwayson"></a>
**始终打开**。 默认情况下，web 应用程序被卸载它们是否空闲的时间段。 这样可以节省资源的系统。 在基本或标准的模式下，您可以启用**始终在**保持应用程序加载的所有时间。 如果您的应用程序运行时 web 连续作业，您应该启用**一直开**，或 web 作业可能无法可靠地运行。

**管理管道版本**。 设置 IIS[管道模式]。 除非您有旧式应用程序要求使用 IIS 的旧版本，请将此设置保留为集成 （默认值）。

**自动交换**。 如果您启用部署插槽的自动交换，应用程序服务会自动交换使 web 应用程序投入生产时您将更新推送到该插槽。 有关详细信息，请参阅 [部署到临时在 Azure 应用程序服务 web 应用程序的插槽] (web 的站点-转移-publishing.md)。

### <a name="debugging"></a>调试

**远程调试**。 启用远程调试。 当启用时，可以使用在 Visual Studio 远程调试器将直接连接到您的 web 应用程序。 远程调试将始终启用 48 小时。 

### <a name="app-settings"></a>应用程序设置

本部分包含名称/值对的您的 web 应用程序将加载在开始。 

- 用于.NET 的应用程序，这些设置注入到您的.NET 配置`AppSettings`在运行时，重写现有的设置。 

- PHP、 Python、 Java 和节点的应用程序可以访问这些设置为在运行时的环境变量。 对于每个应用程序设置中，将创建两个环境变量;一个具有指定由应用程序设置项，和另一个前缀为 APPSETTING_ 的名称。 它们都包含相同的值。

### <a name="connection-strings"></a>连接字符串

链接的资源的连接字符串。 

用于.NET 的应用程序，这些连接字符串注入到您的.NET 配置`connectionStrings`在运行时，设置重写现有的条目，其中键等于所链接的数据库名称。 

对于 PHP，Python、 Java 和节点的应用程序中，这些设置都可作为环境变量在运行时，使用的连接类型作为前缀。 环境变量前缀如下所示︰ 

- SQL Server:`SQLCONNSTR_`
- MySQL 中︰`MYSQLCONNSTR_`
- SQL 数据库︰`SQLAZURECONNSTR_`
- 自定义︰`CUSTOMCONNSTR_`

例如，如果一个 MySql 连接字符串未命名为`connectionstring1`，通过环境变量即可访问到它`MYSQLCONNSTR_connectionString1`。

### <a name="default-documents"></a>默认文档

默认文档将显示在网站的根 URL 的网页。  使用列表中第一个匹配的文件。 

Web 应用程序可能会使用模块路由基于 URL，而不是有这种情况下提供静态内容，则这种情况下没有默认文档。    

### <a name="handler-mappings"></a>处理程序映射

使用此区域来添加自定义脚本的处理器来处理对特定文件扩展名的请求。 

- **扩展名**。 若要处理，例如 *.php 或 handler.fcgi 文件扩展名。 
- **脚本处理器路径**。 脚本处理器的绝对路径。 对文件扩展名匹配的文件的请求将由脚本处理器处理。 使用路径`D:\home\site\wwwroot`来引用您的应用程序的根目录下。
- **额外的参数**。 可选的命令行参数的脚本处理器 


### <a name="virtual-applications-and-directories"></a>虚拟应用程序和目录 
 
要配置虚拟应用程序和目录，可以指定每个虚拟目录和它对应相对于网站根目录的物理路径。 或者，您可以选择**应用程序**复选框以标记为应用程序的虚拟目录。


## <a name="enabling-diagnostic-logs"></a>启用诊断日志

要启用诊断日志︰

1. 在刀片式服务器为您的 web 应用程序，请单击**所有设置**。
2. 单击**诊断日志**。 

从 web 应用程序支持日志记录写入诊断日志的选项︰ 

- **应用程序日志**。 将应用程序日志写入到文件系统。 日志记录持续 12 小时。 

**级别**。 启用应用程序日志记录时，此选项指定的金额的信息将会记录 （的错误、 警告、 信息或详细）。

**Web 服务器日志记录**。 W3C 扩展的日志文件格式保存日志。 

**详细的错误消息**。 保存详细的错误消息的.htm 文件。 这些文件将保存在 /LogFiles/DetailedErrors 之下。 

**失败请求跟踪**。 日志失败的 XML 文件的请求。 这些文件将保存在 /LogFiles/W3SVC*xxx*，其中 xxx 是一个唯一的标识符。 此文件夹包含一个 XSL 文件和一个或多个 XML 文件。 请确保下载 XSL 文件，因为它提供了功能的格式和筛选 XML 文件的内容。

若要查看日志文件，必须创建下列 FTP 凭据︰

1. 在刀片式服务器为您的 web 应用程序，请单击**所有设置**。
2. 单击**凭据部署**。
3. 输入用户名和密码。
4. 单击**保存**。

![集中部署凭据][configure03]

FTP 用户全名是"app\username"，其中*应用程序*是 web 应用程序的名称。 用户名已列入 web app 刀片式服务器，在**精要**下。  

![FTP 部署凭据][configure02]

## <a name="other-configuration-tasks"></a>其他配置任务

### <a name="ssl"></a>SSL 

在基本或标准的模式下，您可以上载一个自定义的域的 SSL 证书。 有关详细信息，请参阅 [启用 HTTPS 的 web 应用程序]。 

若要查看您已上载的证书，请单击**所有设置** > **自定义的域和 SSL**。

### <a name="domain-names"></a>域名称

添加您的 web 应用程序的自定义域名。 有关详细信息，请参阅 [配置在 Azure 应用程序服务 web 应用程序的自定义域名称]。

若要查看您的域名，请单击**所有设置** > **自定义的域和 SSL**。

### <a name="deployments"></a>部署

- 设置连续部署。 请参阅[使用 Git 来部署 Web 应用程序在 Azure 应用程序服务](./web-sites-deploy.md)。
- 部署的插槽。 请参见[部署到临时环境在 Azure 应用程序服务 Web 应用程序]。


若要查看您部署的段，请单击**所有设置** > **部署插槽**。

### <a name="monitoring"></a>监视

在基本或标准的模式下，您可以测试 HTTP 或 HTTPS 的终结点，来自三个地理分布位置的可用性。 如果 HTTP 响应代码是错误 （4xx 或 5xx） 或响应时间超过 30 秒监视测试失败。 终结点被视为只有一个监视测试成功从所有指定的位置。 

有关详细信息，请参阅[如何︰ 监视 web 端点状态]。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务]，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="next-steps"></a>下一步行动

- [在 Azure 应用程序服务中配置自定义域名]
- [在 Azure 应用程序服务的应用程序启用 HTTPS]
- [在 Azure 应用程序服务中扩展 web 应用程序]
- [在 Azure 应用程序服务 Web 应用程序的监视基础知识]

<!-- URL List -->

[ASP.NET 那么 SignalR]: http://www.asp.net/signalr
[Azure 门户]: https://portal.azure.com/
[在 Azure 应用程序服务中配置自定义域名]: ./web-sites-custom-domain-name.md
[对于在 Azure 应用程序服务 Web 应用程序部署到临时环境]: ./web-sites-staged-publishing.md
[在 Azure 应用程序服务的应用程序启用 HTTPS]: ./web-sites-configure-ssl-certificate.md
[如何︰ 监视 web 端点状态]: http://go.microsoft.com/fwLink/?LinkID=279906
[在 Azure 应用程序服务 Web 应用程序的监视基础知识]: ./web-sites-monitor.md
[管线模式]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[在 Azure 应用程序服务中扩展 web 应用程序]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[试用应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
