<properties
    pageTitle="最佳做法和节点应用程序对 Azure Web 应用程序的故障排除指南"
    description="了解最佳实践和节点应用程序对 Azure Web 应用程序的故障排除步骤。"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>最佳做法和节点应用程序对 Azure Web 应用程序的故障排除指南

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

在本文中，您将学习到的最佳做法和 （使用[iisnode](https://github.com/azure/iisnode)) 在 Azure Webapps 上运行的[节点的应用程序](app-service-web-nodejs-get-started.md)的故障排除步骤。

>[AZURE.WARNING] 在您的生产站点上使用的故障排除步骤时，请格外小心。 建议是解决您在非生产系统上的应用程序，例如您暂存槽和修复问题后，换您临时生产插槽的插槽。

## <a name="iisnode-configuration"></a>IISNODE 配置

此[架构文件](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml)显示可以为 iisnode 配置的所有设置。 下面是一些适用于您的应用程序的设置︰

* nodeProcessCountPerApplication

    此设置控制节点启动每个 IIS 应用程序的进程的数量。 默认值为 1。 您可以启动多个 node.exe 作为您的 VM 核心计数将此值设置为 0。 建议的值为 0，对于大多数应用程序以便在您的计算机上，可以利用所有的内核。 Node.exe 是单线程的这样一个 node.exe 将消耗最多 1 核心并获得最大性能要利用所有的核心应用程序节点。

* nodeProcessCommandLine

    此设置控制 node.exe 的路径。 您可以设置此值以指向您的 node.exe 版本。

* maxConcurrentRequestsPerProcess

    此设置控制的最大并发请求发送到每个 node.exe iisnode 数。 在 azure webapps，默认值是无限。 您不需要担心此设置。 外部 azure webapps，默认值为 1024年。 您可能需要配置了此设置取决于您的应用程序获取多少请求和应用程序处理每个请求的速度。

* maxNamedPipeConnectionRetry

    此设置控制 iisnode 将重试上将通过请求发送到 node.exe 的命名管道进行连接的最大次数。 与 namedPipeConnectionRetryDelay 结合使用此设置可确定 iisnode 中的所有请求的总超时。 默认值是 Azure Webapps 200。 以秒为单位的总超时 = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000年

* namedPipeConnectionRetryDelay

    此设置控制的时间 （以毫秒为单位） iisnode 量将等待通过命名管道将请求发送至 node.exe 每次重试之间。 默认值为 250ms年。
    以秒为单位的总超时 = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000年

    默认情况下在 iisnode azure webapps 上总超时为 200 \* 250ms = 50 秒。

* logDirectory

    此设置控制 iisnode 日志 stdout/stderr 的位置的目录。 默认值是 iisnode，它是相对于主脚本目录 （主 server.js 所在的目录）

* debuggerExtensionDll

    此设置控制调试节点应用程序时使用的节点检查器 iisnode 版本。 目前 iisnode-检查-0.7.3.dll，iisnode inspector.dll 是有效的只有 2 此设置的值。 默认值为 0.7.3.dll-iisnode-检查器。 iisnode-检查-0.7.3.dll 版本使用节点-检查-0.7.3，并使用 websocket，因此您将需要启用在您使用此版本的 azure webapp websocket。 请参阅如何配置以使用新节点检查的 iisnode <http://www.ranjithr.com/?p=98>了解详情。

* flushResponse

    IIS 的默认行为是它缓冲响应数据向上 4 MB 之前刷新，或直到结尾的响应，为准。 iisnode 提供了一个配置设置来重写此行为︰ 若要刷新响应实体正文的片段，只要从 node.exe 的 iisnode 接收它，您需要设置iisnode/@flushResponse为 true 的 web.config 中的属性︰
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    启用刷新响应实体正文的每个片段的增加 ~ 5%（截至 v0.1.13)，降低了系统吞吐量的性能开销因此最好确定此设置仅对需要响应流的终结点的范围 (如，使用<location>在 web.config 中的元素)

    除此之外，为流媒体应用程序，需要将 responseBufferLimit iisnode 处理程序设置为 0。
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    这是将更改监视的文件的以分号分隔列表。 对文件的更改将导致应用程序回收。 每个条目都包括可选的目录名称以及所需的文件名称的相对于主应用程序入口点所在的目录。 文件名称部分中允许使用通配符。 默认值是"\*。 js;web.config"

* recycleSignalEnabled

    默认值为 false。 如果启用，应用程序节点可以连接到命名管道 (环境变量 IISNODE\_控件\_管道) 和发送"回收"。 这将导致 w3wp 妥善回收。

* idlePageOutTimePeriod

    默认值为 0 表示禁用此功能。 如果设置为某个值大于 0，iisnode 将页出它的所有子进程每一个 idlePageOutTimePeriod 毫秒。 若要了解哪些页面输出方法，请参考本[文档](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx)。 此设置将非常有用的应用程序占用大量的内存和 pageout 内存到磁盘的偶尔想要释放一些内存。

>[AZURE.WARNING] 启用对生产应用程序的以下配置设置时要格外小心。 推荐方法是不实际生产应用程序上启用它们。

* debugHeaderEnabled

    默认值为 false。 如果设置为 true，iisnode 将添加到每个 HTTP 响应的 HTTP 响应标头 iisnode 调试它发送 iisnode 调试标头值是一个 URL。 可以通过查看 URL 片段中，收集的诊断信息的各个组成部分，但更好地可视化项通过在浏览器中打开 URL。

* loggingEnabled

    此设置控制 iisnode 的标准输出和 stderr 日志记录。 Iisnode 将捕获从其启动的节点进程的标准输出/stderr 并写到 logDirectory 设置中指定的目录。 一旦启用，应用程序将被写入日志文件系统和通过该应用程序的日志记录的金额，可能会影响性能。

* devErrorsEnabled

    默认值为 false。 当设置为 true，iisnode 将显示 HTTP 状态代码和 Win32 错误代码在您的浏览器。 Win32 代码在调试中某些类型的问题会有所帮助。
    
* debuggingEnabled （不能在实际生产站点上）

    此设置控制的调试功能。 Iisnode 是与节点检查器集成在一起。 通过启用此设置，则启用调试的应用程序节点。 启用此设置后，iisnode 将布局节点应用程序第一次调试请求 debuggerVirtualDir 目录中的所需节点检查器文件。 您可以通过将请求发送到 http://yoursite/server.js/debug 加载节点检查器。 可以使用 debuggerPathSegment 设置来控制调试 URL 段。 按默认值 debuggerPathSegment 显示为调试。 以使它更难被人发现，可以设置为 GUID，例如这。

    请检查此有关调试的更多详细信息的[链接](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html)。

## <a name="scenarios-and-recommendationstroubleshooting"></a>方案和建议/故障排除

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>我节点的应用程序发出太多的出站调用。

许多应用程序可能需要作为其常规操作的一部分进行出站连接。 例如，一个请求到达时，您的节点应用程序想要联系 REST API，在其他地方，并获得一些信息，来处理该请求。 您想要使用 http 或 https 打电话保持活动代理。 例如，可以为您保持活动状态代理使用 agentkeepalive 模块制作这些出站调用时。 这将确保您 azure webapp 虚拟机上使用，使用套接字并减少创建新的套接字，对于每个出站请求的系统开销。 此外，这将确保使用较少的套接字的数目使许多出站请求，并且因此不超过每个虚拟机分配最大套接字数。 在 Azure Webapps 建议应将 agentKeepAlive 最大套接字数的值设置为每个虚拟机的 160 套接字总数。 这意味着，如果您有运行在虚拟机上的 4 node.exe，希望将 agentKeepAlive 最大套接字数设置为 40 / node.exe 为每个虚拟机总 160。

AgentKeepALive 配置的示例︰

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

此示例假定您有 4 node.exe 在 VM 上运行。 如果您有不同数量的虚拟机上运行的 node.exe，您将必须修改相应地设置最大套接字数。

### <a name="my-node-application-is-consuming-too-much-cpu"></a>我节点应用程序消耗过多的 CPU。

有关高 cpu 消耗您的门户网站上，可能会从 Azure Webapps 获得建议。 此外可以设置监视器来监视某些[指标](web-sites-monitor.md)。 在检查[Azure 门户仪表板](../application-insights/app-insights-web-monitor-performance.md)上的 CPU 使用率，请检查最大值为 CPU 以便您可不要错过高峰值。
您认为您的应用程序会消耗太多 CPU 并不能解释原因的情况下，您需要分析节点的应用程序。

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>分析节点在 azure webapps V8 探查器与应用程序

例如，可以说有你好世界应用程序所需的配置文件如下所示︰

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

转到您的 scm 网站 https://yoursite.scm.azurewebsites.net/DebugConsole

如下所示，您将看到命令提示符。 转到您的网站/wwwroot 目录

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

运行"npm 安装 v8 探查器"命令

这应安装节点下的 v8 探查器\_模块目录及其所有依赖项。
现在，编辑您的 server.js 来分析您的应用程序。

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

在进行上述更改将分析 WriteConsoleLog 函数，然后将配置文件输出写入站点 wwwroot 下 profile.cpuprofile 文件。 向应用程序发送的请求。 您会看到在您的站点 wwwroot 下创建的 profile.cpuprofile 文件。

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

下载此文件，您将需要使用 Chrome F12 工具打开此文件。 镶边，在按 f12 键，然后单击"配置文件选项卡"。 单击"加载"按钮。 选择您刚刚下载的 profile.cpuprofile 文件。 单击刚加载的配置文件。

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

您将看到如下所示，95%的时间已使用 WriteConsoleLog 函数。 这也显示您的确切的行号和源文件产生问题的。

### <a name="my-node-application-is-consuming-too-much-memory"></a>我节点的应用程序占用太多内存。

高内存占用有关门户网站上，可能会从 Azure Webapps 获得一些建议。 此外可以设置监视器来监视某些[指标](web-sites-monitor.md)。 在检查在[Azure 门户仪表板](../application-insights/app-insights-web-monitor-performance.md)上的内存使用情况时，请检查内存的最大值，以便您可不要错过高峰值。

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>泄漏检测和 node.js 的堆比较 

您可以使用[节点 memwatch](https://github.com/lloyd/node-memwatch)来帮助确定内存泄漏。
可以安装 memwatch 就像 v8 探查器和编辑您的代码捕获和比较与堆来标识内存泄漏应用程序中。

### <a name="my-nodeexes-are-getting-killed-randomly"></a>我 node.exe 是随机获取终止 

有几个原因为什么这怎么回事︰

1.  您的应用程序会引发未捕获的异常--请检查 d:\\家庭\\日志文件\\应用程序\\上引发的异常详细信息的日志记录 errors.txt 文件。 此文件的堆栈跟踪，这样可以解决应用程序基于此。

2.  您的应用程序占用太多内存，这会影响其他进程从入门。 如果 VM 内存总量接近 100%，则 node.exe 的无法终止通过进程管理器允许其他进程有机会做一些工作。 若要解决此问题，请确保您的应用程序不泄漏内存或如果您应用程序确实需要使用大量的内存，请扩展到更大更多的 ram VM。

### <a name="my-node-application-does-not-start"></a>我节点的应用程序不会启动

如果您的应用程序返回 500 错误在启动时，可能有几个原因︰

1.  Node.exe 在正确的位置不存在。 检查 nodeProcessCommandLine 设置。

2.  主脚本文件的正确位置不存在。 检查 web.config 并确保在处理程序部分中的主脚本文件的名称匹配的主脚本文件。

3.  Web.config 配置不正确，请检查设置的名称/值。

4.  冷启动 – 您的应用程序太长而启动。 如果您的应用程序时间长比 (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 秒，iisnode 将返回 500 错误。 增加这些设置以匹配您应用程序的开始时间以防止 iisnode 从超时并返回 500 错误的值。

### <a name="my-node-application-crashed"></a>我崩溃的节点应用程序

您的应用程序会引发未捕获的异常--请检查 d:\\家庭\\日志文件\\应用程序\\上引发的异常详细信息的日志记录 errors.txt 文件。 此文件的堆栈跟踪，这样可以解决应用程序基于此。

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>我节点的应用程序 （冷启动） 的启动需要很长时间

最常见的原因是应用程序的节点中的文件很多\_模块和应用程序尝试在启动时加载其中的大部分文件。 默认情况下，由于您的文件驻留在网络共享上 Azure Webapps 加载太多的文件可能需要一些时间。
为了更快地实现这一些解决方法如下︰

1.  确保通过使用 npm3 来安装模块具有平面相关性结构并没有重复的依赖项。

2.  尝试延迟加载您的节点\_模块加载的所有模块启动时。 这意味着对 require('module') 的调用应在实际需要时在您尝试使用该模块函数中。

3.  Azure Webapps 提供了一种称为本地缓存功能。 此功能将从网络共享您的内容复制到本地磁盘上的虚拟机中。 由于这些文件是本地节点的负载时间\_模块是快得多。 -此[文档](../app-service/app-service-local-cache.md)说明了如何使用本地缓存的更多详细信息。

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http 状态和子状态

此[源文件](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h)列出所有可能的状态/子状态组合 iisnode 可以在错误出现时返回。

启用应用程序，请参阅 win32 错误代码 FREB （请确保启用 FREB 仅出于性能原因的非生产网站上）。

| Http 状态 | Http 子状态 | 可能原因如下︰                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | 没有调度到 IISNODE 请求某些问题 – 检查 node.exe 已启动。 Node.exe 无法在启动时已崩溃。 检查 web.config 配置错误。                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2-应用程序没有响应 URL。 请检查 URL 重写规则，或者明确应用程序有定义的正确路由。 -Win32Error 0x6d-命名管道正忙--Node.exe 不会接受请求，因为管道正忙。 查看 cpu 使用率过高。 -其他的错误--检查 node.exe 是否崩溃。
| 500         | 1002           | Node.exe 崩溃--检查 d:\\家庭\\日志文件\\日志记录-errors.txt 的堆栈跟踪。                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | 管道配置问题 – 应该永远不会看到这，但如果那样的话，命名的管道配置不正确。                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | 在发送请求或处理对 node.exe 的响应的同时发生一些错误。 检查 node.exe 是否崩溃。 检查 d:\\家庭\\日志文件\\日志记录-errors.txt 的堆栈跟踪。                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | 内存不足，无法分配更多的命名的管道连接。 检查您的应用程序会占用过多内存的原因。 检查 maxConcurrentRequestsPerProcess 的设置值。 如果它不限，并有大量的请求，增大此值以防止出现此错误。                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | 无法对 node.exe 调度请求，因为应用程序回收。 回收应用程序之后，应正常提供服务请求。                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | 无法调度实际原因 – 请求检查 win32 错误代码为 node.exe。                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | 命名的管道是太忙--检查节点是否占用了大量的 CPU                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
NODE.exe 调用节点中的设置\_待决\_管道\_实例。 默认情况下，外部 azure webapps 此值为 4。 这意味着该 node.exe 命名管道上的一次只能接受 4 次请求。 在 Azure Webapps，此值设置为 5000，这个值应足够好，对于大多数在 azure webapps 上运行的节点应用程序。 应不会看到 503.1003 在 azure webapps 因为我们有较高的值节点的\_待决\_管道\_实例。  |

## <a name="more-resources"></a>更多的资源

按照这些链接以了解有关 node.js Azure 应用程序服务的应用程序。

* [在 Azure 应用程序服务的 Node.js web 应用程序入门](app-service-web-nodejs-get-started.md)
* [如何调试 Node.js web 应用程序在 Azure 应用程序服务](web-sites-nodejs-debug.md)
* [Node.js 模块使用 Azure 应用程序](../nodejs-use-node-modules-azure-apps.md)
* [Azure 应用程序服务 Web 应用程序︰ Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js 开发人员中心](../nodejs-use-node-modules-azure-apps.md)
* [了解超级机密 Kudu 调试控制台](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)