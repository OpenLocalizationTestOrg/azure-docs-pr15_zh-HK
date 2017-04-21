<properties
    pageTitle="Azure 功能开发人员参考 |Microsoft Azure"
    description="了解如何开发使用 C# Azure 函数。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure 函数 C# 开发人员参考

> [AZURE.SELECTOR]
- [C# 脚本](../articles/azure-functions/functions-reference-csharp.md)
- [F# 脚本](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Azure 函数的 C# 体验基于 Azure WebJobs SDK。 数据流入 C# 函数通过方法参数。 在指定参数名称`function.json`，并且没有预定义的名称，用于访问函数记录器和取消标记等内容。

本文假定您已经已经了解了[Azure 功能开发人员参考](functions-reference.md)。

## <a name="how-csx-works"></a>.Csx 的工作原理

`.csx`格式允许编写更少"套用"并集中精力编写只有 C# 函数。 Azure 的函数，您只需包含任何引用的程序集和命名空间的需要同往常一样，最顶部，和而不是环绕的所有内容的命名空间和类，您可以只是定义您`Run`方法。 如果您需要任何类中，例如包括定义 POCO 对象，可以包括在同一文件内的类。

## <a name="binding-to-arguments"></a>绑定到参数

不同的绑定绑定到 C# 函数通过`name` *function.json*配置中的属性。 每个绑定都有自己支持的类型，其中介绍了每个绑定;例如，字符串、 POCO 或几种其他类型，可以支持 blob 触发器。 您可以使用最适合您的需要的类型。 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>日志记录

若要登录到您在 C# 的流式处理日志输出，可以包括`TraceWriter`输入参数。 我们建议您命名`log`。 我们建议您避免`Console.Write`Azure 的函数中。

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>异步

要异步函数，使用`async`关键字并返回`Task`对象。

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>取消标记

在某些情况下，您可能必须对正在关闭敏感操作。 虽然总是最适合用来编写代码可以处理崩溃，在希望的位置的情况下处理正常关机请求，定义[`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx)输入参数。  A`CancellationToken`将获得如果主机关闭时触发。 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>导入命名空间

如果您需要导入的命名空间，则可以执行因此照例，与`using`子句。

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

下列命名空间自动导入的因此是可选︰

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>引用外部程序集

框架的程序集添加引用通过使用`#r "AssemblyName"`指令。

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

宿主环境的 Azure 功能，将自动添加以下程序集︰

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

此外，下列程序集是特殊大小写混合格式并通过 simplename 可以引用 (例如`#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

如果您需要引用一个私有程序集，则可以上载到该程序集文件`bin`相对于您 （例如，它通过使用文件命名的函数和引用的文件夹 `#r "MyAssembly.dll"`). 有关如何将文件上载到您的函数文件夹的信息，请参阅包管理上的下一节。

## <a name="package-management"></a>包管理

若要使用 NuGet 程序包中的 C# 函数， *project.json*文件上载到该函数的函数的应用程序的文件系统中的文件夹。 下面是添加到 Microsoft.ProjectOxford.Face 版本 1.1.0 引用示例*project.json*文件︰

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

仅.NET Framework 4.6 都支持，所以请确保*project.json*文件指定`net46`如下所示。

在上载一个*project.json*文件时，运行时获取包和包程序集的引用将自动添加。 您不需要添加`#r "AssemblyName"`指令。 只需添加所需`using` *run.csx*文件使用 NuGet 程序包中定义的类型的语句。


### <a name="how-to-upload-a-projectjson-file"></a>如何将 project.json 文件上载

1. 通过确保您函数的应用程序开始运行，可以通过在 Azure 的门户打开您的函数执行此操作。 

    这还使访问流日志包安装输出的显示位置。 

2. 若要上载的 project.json 文件，使用[Azure 功能开发人员参考主题](functions-reference.md#fileupdate)**如何更新函数的应用程序文件**一节中所述的方法之一。 

3. *Project.json*文件上载后，您会看到输出类似于下面的示例中您的函数的流式传输日志︰

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261 
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>环境变量

若要获取环境变量或应用程序设置值，请使用`System.Environment.GetEnvironmentVariable`，如下面的代码示例所示︰

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>重用.csx 代码

您可以使用类和其他在*run.csx*文件中的*.csx*文件中定义的方法。 为此，请使用`#load`在*run.csx*文件中，如下面的示例中所示的指令。

示例*run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

示例*mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

您可以使用相对路径与`#load`指令︰

* `#load "mylogger.csx"`将加载函数文件夹中的文件。

* `#load "loadedfiles\mylogger.csx"`将加载位于函数文件夹中的文件夹中的文件。

* `#load "..\shared\mylogger.csx"`加载在*wwwroot*下直接位于函数文件夹，即，相同级别的文件夹中的文件。
 
`#load`指令工作只与*.csx* （C# 脚本） 的文件，不具有*.cs*文件。 

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅以下资源︰

* [Azure 功能开发人员参考](functions-reference.md)
* [Azure 函数 F# 开发人员参考](functions-reference-fsharp.md)
* [Azure 函数 NodeJS 开发人员参考](functions-reference-node.md)
* [Azure 功能触发器和绑定](functions-triggers-bindings.md)

