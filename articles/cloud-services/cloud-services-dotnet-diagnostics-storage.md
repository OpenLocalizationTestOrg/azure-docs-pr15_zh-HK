<properties
    pageTitle="在 Azure 存储中的存储和查看诊断数据 |Microsoft Azure"
    description="到 Azure 存储中获取 Azure 诊断数据并查看它"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>在 Azure 存储中的存储和查看诊断数据

不是永久存储诊断数据，除非将它传输到 Microsoft Azure 存储仿真器或 Azure 存储。 一次在存储中，则可以查看具有多个可用的工具之一。

## <a name="specify-a-storage-account"></a>指定存储帐户

指定您想要使用 ServiceConfiguration.cscfg 文件中的存储帐户。 帐户信息被指某一配置设置中的连接字符串。 下面的示例显示了为新的云服务项目在 Visual Studio 中创建的默认连接字符串︰


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

您可以更改此连接字符串提供的 Azure 存储帐户的帐户信息。

根据所收集的诊断数据的类型，Azure 诊断使用 Blob 服务或表服务。 下表显示了将保留数据源和格式。

|数据源|存储格式|
|---|---|
|Azure 的日志|表|
|IIS 7.0 日志|Blob|
|Azure 诊断基础结构日志|表|
|失败请求跟踪日志|Blob|
|Windows 事件日志|表|
|性能计数器|表|
|故障转储|Blob|
|自定义错误日志|Blob|

## <a name="transfer-diagnostic-data"></a>将诊断数据传输

SDK 2.5 及更高版本中，诊断数据传输的请求会通过配置文件。 您可以在配置中指定的定期转移诊断数据。

SDK 2.4 和以前可以请求为以编程方式通过配置文件和转移的诊断数据。 程序化的方法还允许您执行按需传输。


>[AZURE.IMPORTANT] 当您将诊断数据转移到 Azure 存储帐户时，则会导致您的诊断数据使用存储资源的成本。

## <a name="store-diagnostic-data"></a>将诊断数据存储

具有以下名称的 Blob 或表存储在存储日志数据︰

**表**

- **WadLogsTable** -在代码中使用跟踪侦听器写入的日志。

- **WADDiagnosticInfrastructureLogsTable** -诊断监视和配置更改。

- **WADDirectoriesTable** – 诊断监视器监视的目录。  这包括 IIS 日志、 IIS 失败请求日志和自定义目录。  容器字段中指定位置的 blob 日志文件和 blob 的名称是在 RelativePath 字段中。  AbsolutePath 字段中指示的位置和文件的名称在 Azure 的虚拟机上的一样。

- **WADPerformanceCountersTable** – 性能计数器。

- **WADWindowsEventLogsTable** – Windows 事件日志。

**Blob**

- **wad 控件容器**— （仅对 SDK 2.4 和以前） 包含控制 Azure 的诊断程序的 XML 配置文件。

- **wad-iis failedreqlogfiles** – 包含 IIS 失败请求的信息记录。

- **日志文件 iis wad** -包含关于 IIS 的信息记录。

- **"自定义"** – 基于配置诊断监视器监视的目录的自定义容器。  将在 WADDirectoriesTable 中指定此 blob 容器的名称。

## <a name="tools-to-view-diagnostic-data"></a>若要查看诊断数据的工具
几个工具可以查看数据，则传输到存储区后。 例如︰

- 服务器资源管理器在 Visual Studio-如果您的 Microsoft Visual Studio 安装了 Azure 工具可用于 Azure 存储节点服务器资源管理器中从 Azure 存储帐户查看只读的斑点和表数据。 您可以从您的本地存储仿真程序帐户显示数据和同样来自存储帐户您已经创建了 azure。 有关详细信息，请参阅[浏览和使用服务器资源管理器中管理存储资源](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md)。

- [Microsoft Azure 存储资源管理器](../vs-azure-tools-storage-manage-with-storage-explorer.md)是一个独立应用程序，使您可以轻松地使用 Azure 存储数据窗口、 OSX 和 Linux 上。

- [Azure 管理 Studio](http://www.cerebrata.com/products/azure-management-studio/introduction)包括 Azure 诊断程序管理器，以便您可以查看、 下载和管理对 Azure 上运行的应用程序收集诊断数据。


## <a name="next-steps"></a>下一步行动

[跟踪在云服务应用程序中使用 Azure 诊断程序流](cloud-services-dotnet-diagnostics-trace-flow.md)
