<properties
   pageTitle="在 Azure 诊断中使用性能计数器 |Microsoft Azure"
   description="在 Azure 的云服务或虚拟机的性能计数器用于找出瓶颈和优化性能。"
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
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>创建和使用 Azure 应用程序中的性能计数器

本文介绍了所带来的好处以及如何放入 Azure 应用程序的性能计数器。 可以使用它们来收集数据，找出瓶颈，调整系统和应用程序的性能。

对于 Windows 服务器，IIS 和 ASP.NET 可用的性能计数器也可以收集并用于确定您的 Azure web 角色、 工作人员角色和虚拟机的运行状况。 您还可以创建并使用自定义性能计数器。  

您可以检查性能计数器数据
1. 使用性能监视器工具访问应用程序主机上直接使用远程桌面
2. 系统中心操作管理器使用 Azure 管理包
3. 与其他监视工具访问诊断数据转移到 Azure 存储中。 有关详细信息，请参阅[存储和查看 Azure 存储中的诊断数据](https://msdn.microsoft.com/library/azure/hh411534.aspx)。  

在[Azure 经典门户](http://manage.azure.com/)应用程序的性能监视的详细信息，请参阅[如何监视云服务](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/)。

有关创建日志记录和跟踪策略和使用诊断程序和其他技术来解决问题和优化 Azure 应用程序的其他深入指导，请参阅[故障排除开发 Azure 应用程序的最佳做法](https://msdn.microsoft.com/library/azure/hh771389.aspx)。


## <a name="enable-performance-counter-monitoring"></a>启用性能计数器监视

默认情况下不启用性能计数器。 您的应用程序或启动任务必须修改缺省诊断代理配置，包含您想要为每个角色实例监视特定的性能计数器。

### <a name="performance-counters-available-for-microsoft-azure"></a>可用于 Microsoft Azure 的性能计数器

Azure 的 Windows 服务器，IIS 和 ASP.NET 堆栈提供可用的性能计数器的一个子集。 下表列出了一些感兴趣的 Azure 应用程序的性能计数器。

|计数器类别︰ 对象 （实例）|计数器名称      |引用|
|---|---|---|
|.NET CLR 异常 （_全球_）|# Exceps 引发 / 秒   |异常性能计数器|
|.NET CLR 内存 （_全球_）    |%Time in GC            |内存性能计数器|
|ASP.NET                      |应用程序重新启动    |ASP.NET 的性能计数器|
|ASP.NET                      |请求执行时间  |ASP.NET 的性能计数器|
|ASP.NET                      |断开连接的请求   |ASP.NET 的性能计数器|
|ASP.NET                      |辅助进程重新启动 |ASP.NET 的性能计数器|
|ASP.NET 应用程序 （__总数__）|请求总数        |ASP.NET 的性能计数器|
|ASP.NET 应用程序 （__总数__）|每秒请求数          |ASP.NET 的性能计数器|
|ASP.NET v4.0.30319           |请求执行时间  |ASP.NET 的性能计数器|
|ASP.NET v4.0.30319           |请求的等待时间       |ASP.NET 的性能计数器|
|ASP.NET v4.0.30319           |请求当前        |ASP.NET 的性能计数器|
|ASP.NET v4.0.30319           |排队的请求         |ASP.NET 的性能计数器|
|ASP.NET v4.0.30319           |被拒绝的请求       |ASP.NET 的性能计数器|
|内存                       |可用的兆字节数        |内存性能计数器|
|内存                       |提交的字节数         |内存性能计数器|
|Processor(_Total)            |处理器时间百分比        |ASP.NET 的性能计数器|
|TCPv4                        |连接失败     |TCP 对象|
|TCPv4                        |建立连接 |TCP 对象|
|TCPv4                        |连接重置       |TCP 对象|
|TCPv4                        |每秒已发送段       |TCP 对象|
|网络 Interface(*)         |接收到的字节数/秒      |网络接口对象|
|网络 Interface(*)         |发送的字节数/秒          |网络接口对象|
|网络接口 （Microsoft 虚拟机总线网络适配器 _2）|接收到的字节数/秒|网络接口对象|
|网络接口 （Microsoft 虚拟机总线网络适配器 _2）|发送的字节数/秒|网络接口对象|
|网络接口 （Microsoft 虚拟机总线网络适配器 _2）|字节总数/秒|网络接口对象|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>创建并添加到您的应用程序的自定义性能计数器

Azure 具有支持创建和修改 web 角色和辅助角色的自定义性能计数器。 这些计数器可用于跟踪和监视特定应用程序的行为。 您可以创建并启动任务、 web 角色或辅助角色，以提升的权限从中删除自定义性能计数器类别和说明符。

>[AZURE.NOTE] 更改了自定义性能计数器的代码必须具有提升的权限运行。 如果代码在 web 角色或辅助角色，该角色必须包括的标记<Runtime executionContext="elevated" />在 ServiceDefinition.csdef 文件中为要正确初始化的角色。

您可以将自定义性能计数器数据发送到 Azure 存储使用诊断代理。

标准的性能计数器数据生成 Azure 的进程。 必须通过 web 角色或辅助角色应用程序创建自定义的性能计数器数据。 可以自定义性能计数器中存储的数据类型的信息，请参阅[性能计数器类型](https://msdn.microsoft.com/library/z573042h.aspx)。 [改示例](http://code.msdn.microsoft.com/azure/)创建并设置 web 角色中的自定义性能计数器数据的示例，请参见

## <a name="store-and-view-performance-counter-data"></a>存储和查看性能计数器数据

Azure 缓存性能计数器数据与其他诊断信息。 此数据是可用于远程监视角色实例正在运行远程桌面访问用于查看性能监视器这样的工具时。 若要保留外部角色实例数据，诊断代理程序必须将数据传输到 Azure 存储。 缓存的性能计数器数据的大小限制可以配置在诊断代理，或它可能配置为将所有诊断数据的共享限制的一部分。 有关设置缓冲区大小的详细信息，请参阅[OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx)和[DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx)。 设置诊断代理将数据传输到存储帐户的概述，请参阅[存储和查看 Azure 存储中的诊断数据](https://msdn.microsoft.com/library/azure/hh411534.aspx)。

每个已配置的性能计数器实例记录指定的采样速率和采样的数据转移到由计划的传送请求或按需传输请求的存储帐户。 自动传输可能会被安排为通常为每分钟一次。 转移诊断代理的性能计数器数据存储在表中，WADPerformanceCountersTable，在存储帐户。 可能访问此表，并使用标准的 Azure 存储 API 方法进行查询。 请参阅[Microsoft Azure 改示例](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9)查询和显示性能计数器 WADPerformanceCountersTable 表中的数据的示例。

>[AZURE.NOTE] 根据诊断代理传输频率和队列延迟时间，最新的性能计数器数据的存储帐户中可能几分钟过期。

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>启用性能计数器使用诊断程序配置文件

使用以下过程来启用 Azure 应用程序中的性能计数器。

## <a name="prerequisites"></a>系统必备组件

本节假定您已经导入您的应用程序的诊断显示器并诊断配置文件添加到 Visual Studio 解决方案 （在 SDK 2.4 和更低的 diagnostics.wadcfg 或 diagnostics.wadcfgx 在 SDK 2.5 及以上）。 请参阅步骤 1 和 2 在[Azure 云服务和虚拟机中启用诊断](./cloud-services-dotnet-diagnostics.md)中的） 的详细信息。

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>步骤 1︰ 收集和存储数据的性能计数器

诊断文件添加到 Visual Studio 解决方案后，您可以配置的集合和的性能计数器数据存储 Azure 应用程序中。 这是通过将性能计数器添加到诊断程序文件。 在实例上首先收集诊断数据，包括性能计数器。 数据然后保存到 WADPerformanceCountersTable 表在 Azure 表服务，因此您需要在您的应用程序中指定的存储帐户。 如果您正在本地计算仿真程序测试您的应用程序，您还可以存储诊断数据本地存储仿真程序中。 存储诊断数据之前必须首先转到[Azure 的经典门户](http://manage.windowsazure.com/)并创建存储帐户。 最佳做法是 Azure 应用程序的同一地理位置中找到您的存储帐户，以避免支付外部带宽成本和减少延迟。

### <a name="add-performance-counters-to-the-diagnostics-file"></a>将性能计数器添加到诊断文件

有许多可用的计数器。 下面的示例演示了几个建议针对 web 和工作角色监视的性能计数器。

诊断程序或打开文件 （在 SDK 2.4 和更低的 diagnostics.wadcfg diagnostics.wadcfgx 在 SDK 2.5 及以上），将以下内容添加到 DiagnosticMonitorConfiguration 元素︰

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

BufferQuotaInMB 属性，它指定用于数据集合类型 （Azure 日志、 IIS 日志等） 的文件系统存储的最大。 默认值为 0。 当达到配额时，添加新数据时将删除最旧的数据。 所有的 bufferQuotaInMB 属性的总和必须大于 OverallQuotaInMB 属性的值。 确定诊断数据收集将需要多少存储空间的更详细讨论，请参阅[疑难解答最佳做法开发 Azure 应用程序](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)的安装 WAD 部分。

ScheduledTransferPeriod 属性，指定计划的数据传输之间的时间间隔，四舍五入到最接近的分钟数。 在下面的示例设置为 PT30M （30 分钟）。 将转移周期设置为一个较小的值，如 1 分钟，将对生产中的应用程序的性能产生负面影响，但对于查看诊断工作快速测试时非常有用。 在计划的转移期间应该足够小，以确保诊断数据不会被覆盖上的实例，但足够大，它不会影响您的应用程序的性能。

CounterSpecifier 属性指定性能计数器，来收集。SampleRate 属性指定的性能计数器应进行抽样，在这种情况下 30 秒的速率。

添加您想要收集的性能计数器后，保存所做的更改对诊断文件。 接下来，您需要指定诊断程序数据将保存到的存储帐户。

### <a name="specify-the-storage-account"></a>指定的存储帐户

若要保留到 Azure 存储帐户您的诊断信息，必须服务 (ServiceConfiguration.cscfg) 配置文件中指定的连接字符串。

对于 Azure SDK 2.5 可以在 diagnostics.wadcfgx 文件中指定存储帐户。

>[AZURE.NOTE] 以下说明仅适用于 Azure SDK 2.4 和下方。 对于 Azure SDK 2.5 可以在 diagnostics.wadcfgx 文件中指定存储帐户。

若要设置连接字符串︰

1. 打开 ServiceConfiguration.Cloud.cscfg 文件使用您喜爱的文本编辑器并设置存储的连接字符串。 *帐户名*和*AccountKey*值中存储帐户仪表板管理项下的 Azure 传统门户网站中找到。

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. 将 ServiceConfiguration.Cloud.cscfg 文件保存。

3. 打开 ServiceConfiguration.Local.cscfg 文件，并验证 UseDevelopmentStorage 被设置为 true。

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
既然设置连接字符串，应用程序在部署应用程序时将会保留诊断数据到您的存储帐户。
4. 保存并生成项目，然后将应用程序部署。

## <a name="step-2-optional-create-custom-performance-counters"></a>步骤 2: （可选） 创建自定义性能计数器

除了预定义的性能计数器，您可以添加您自己的自定义性能计数器来监视 web 或辅助角色。 自定义性能计数器可用于跟踪和监视特定应用程序的行为和可以创建或删除启动任务、 web 角色或辅助角色，以提升的权限。

Azure 诊断代理刷新的性能计数器配置从.wadcfg 文件在启动后 1 分钟。  如果您创建的 OnStart 方法中的自定义性能计数器和启动任务执行时间超过一分钟来执行，自定义性能计数器不会被创建在 Azure 诊断代理尝试加载它们。  在此方案中，您将看到 Azure 诊断正确捕获所有诊断数据，除非您自定义性能计数器。  要解决此问题，请启动任务中创建的性能计数器或移动创建的性能计数器后 OnStart 方法启动任务的一些工作。

执行以下步骤来创建简单的自定义性能计数器，名为"\MyCustomCounterCategory\MyButton1Counter":

1. 打开您的应用程序的服务定义文件 (CSDEF)。
2. 添加运行时元素到 WebRole 或 WorkerRole 元素，以便允许使用提升的权限执行︰

    ```
    <runtime executioncontext="elevated"/>
    ```
3. 保存该文件。
4. 诊断程序或打开文件 （在 SDK 2.4 和更低的 diagnostics.wadcfg diagnostics.wadcfgx 在 SDK 2.5 及以上），并将以下内容添加到 DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. 保存该文件。
6. 您的角色的 OnStart 方法中的自定义性能计数器类别创建之前调用基。OnStart。 如果不存在，下面的 C# 示例创建一个自定义类别︰

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. 更新您的应用程序内的计数器。 下面的示例更新自定义性能计数器 Button1_Click 事件︰

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. 保存该文件。  

现在通过 Azure 诊断监控器收集自定义性能计数器数据。

## <a name="step-3-query-performance-counter-data"></a>第 3 步︰ 查询性能计数器数据

一旦应用程序部署并运行诊断程序监视器将开始收集性能计数器和持久化到 Azure 存储该数据。 使用服务器资源管理器在 Visual Studio， [Azure 存储浏览器](http://azurestorageexplorer.codeplex.com/)或通过 Cerebrata [Azure 诊断程序管理器](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx)的工具 WADPerformanceCountersTable 表中查看性能计数器数据。 以编程方式还可以查询使用[C#](../storage/storage-dotnet-how-to-use-tables.d)、 [Java](../storage/storage-java-how-to-use-table-storage.md)、 [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md)， [Python](../storage/storage-python-how-to-use-table-storage.md)， [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)或[PHP](../storage/storage-php-how-to-use-table-storage.md)的表服务。

下面的 C# 示例演示对 WADPerformanceCountersTable 表的简单查询，并将诊断数据保存到 CSV 文件。 性能计数器保存到 CSV 文件后可以使用 Microsoft Excel 或某些其它工具中的绘图功能若要可视化的数据。 请务必将引用添加到 Microsoft.WindowsAzure.Storage.dll，它是在.net 2012 年 10 月 Azure SDK 中包含及更高版本。 将程序集安装到 %程序 Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version num\ref\ 目录中。

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

实体映射到 C# 使用**TableEntity**从派生的自定义类的对象。 下面的代码定义一个实体类，表示性能计数器**WADPerformanceCountersTable**表中。


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>下一步行动
[查看其他文章，Azure 诊断程序](../ azure diagnostics.md）
