<properties 
    pageTitle="如何使用 WebJobs SDK Azure blob 存储" 
    description="了解如何使用 WebJobs SDK Azure blob 存储。 新斑点出现在容器中时触发流程，处理有害 blob。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>如何使用 WebJobs SDK Azure blob 存储

## <a name="overview"></a>概述

本指南提供了 C# 的代码示例，显示如何 Azure 的斑点是创建或更新时触发流程。 代码示例使用[WebJobs SDK](websites-dotnet-webjobs-sdk.md)版本 1.x。

有关代码示例，演示如何创建 blob，请参阅[如何使用 WebJobs sdk 的 Azure 队列存储](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)。 
        
该指南假定您知道[如何创建一个在 Visual Studio 中指向您的存储帐户连接字符串的 WebJob 项目](websites-dotnet-webjobs-sdk-get-started.md)或[多个存储帐户](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs)。

## <a id="trigger"></a>如何创建或更新 blob 时触发一个函数

本节说明如何使用`BlobTrigger`属性。 

> [AZURE.NOTE] WebJobs SDK 扫描日志文件来监视的新的或更改的 blob。 这一过程不是实时的。函数可能不触发几分钟或更长时间之后创建 blob。 此外，[存储日志创建在"尽最大努力"](https://msdn.microsoft.com/library/azure/hh343262.aspx)的基础;就不能保证，将捕获所有事件。 某些情况下，日志可能会丢失。 如果 blob 触发器的速度和可靠性限制无法接受您的应用程序，推荐的方法是创建队列的邮件，当您创建 blob，并使用[QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger)属性而不是`BlobTrigger`有关处理 blob 函数的特性。

### <a name="single-placeholder-for-blob-name-with-extension"></a>Blob 名称带有扩展名的单个占位符  

下面的代码示例会将*输入*与*输出*容器容器中显示的文本 blob 复制︰

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

属性构造函数采用一个字符串参数，它指定容器名称和 blob 名称的占位符。 在此示例中，如果在*输入*容器，创建名为*Blob1.txt*的 blob 函数创建名为*Blob1.txt*的*输出*容器中的 blob。 

下面的代码示例中所示，您可以使用 blob 名称的占位符，指定名称模式︰

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

此代码将复制唯一 blob 包含与"原始-"开头的名称。 例如，*输入*的容器中的*原始 Blob1.txt*复制到*复制 Blob1.txt* *输出*容器中。

如果您需要指定名称中带有括号的 blob 名称的名称模式，双大括号。 例如，如果您想要查找具有类似名称的*图像*容器中的 blob:

        {20140101}-soundfile.mp3

使用为您的模式︰

        images/{{20140101}}-{name}

在示例中，*名称*的占位符值为*soundfile.mp3*。 

### <a name="separate-blob-name-and-extension-placeholders"></a>单独的 blob 名称和扩展名占位符

下面的代码示例更改文件扩展名，并在复制出现在*输入**输出*容器到容器中的 blob。 该代码记录*输入*的 blob 的扩展和设置*输出*blob 的扩展名为*.txt*。

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>您可以绑定到 blob 类型

您可以使用`BlobTrigger`在以下类型的属性︰

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* 由[ICloudBlobStreamBinder](#icbsb)反序列化的其他类型 

如果要直接使用 Azure 存储帐户，也可以添加`CloudStorageAccount`的方法签名的参数。

有关示例，请参阅[上 GitHub.com azure-webjobs-sdk 存储库中的 blob 绑定代码](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs)。

## <a id="string"></a>获取绑定到字符串的文本 blob 内容

如果预计文本 blob，`BlobTrigger`可以应用于`string`参数。 下面的代码示例将绑定到的文本 blob`string`参数命名为`logMessage`。 该函数使用该参数将 blob 的内容写到 WebJobs SDK 的仪表板。 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>获取序列化使用 ICloudBlobStreamBinder 的 blob 内容

下面的代码示例使用类实现`ICloudBlobStreamBinder`以启用`BlobTrigger`属性绑定到一个 blob`WebImage`类型。

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

`WebImage`绑定代码中提供`WebImageBinder`从派生类`ICloudBlobStreamBinder`。

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>获取触发 blob 的 blob 路径

要获取的容器名称和已触发函数的 blob blob 名称，包括`blobTrigger`字符串的函数签名中的参数。

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>如何处理有害的 blob

当`BlobTrigger`函数失败，SDK 调用它，以防故障由暂时的错误。 如果失败由内容的 blob，函数将失败，每当它试图处理 blob。 默认情况下，SDK 调用一个函数最多 5 次为一个给定的 blob。 如果第五重试失败，SDK 中添加到名为*webjobs-blobtrigger-毒性*队列消息。

最大重试次数进行配置。 相同的[MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue)设置用于有害 blob 处理和毒信队列消息处理。 

有害的 blob 的队列消息是一个 JSON 对象，该对象包含以下属性︰

* FunctionId （采用*{WebJob 名称}*。功能。*{函数名称}*，例如︰ WebJob1.Functions.CopyBlob)
* BlobType （"BlockBlob"或"PageBlob"）
* 容器名称
* BlobName
* ETag (blob 版本标识符，例如:"0x8D1DC6E70A277EF")

在下面的代码示例中，`CopyBlob`函数具有代码，使它每次调用时失败。 后 SDK 调用它的最大重试次数，在有害的 blob 队列中，创建一封邮件，该邮件通过`LogPoisonBlob`函数。 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK 自动反序列化 JSON 消息。 以下是`PoisonBlobMessage`类︰ 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Blob 轮询算法

WebJobs SDK 扫描所有容器由`BlobTrigger`应用程序开始处的特性。 在一个大的存储客户，该扫描可能需要一些时间，因此可能是一段时间才能找到新的 blob 和`BlobTrigger`的功能执行。

若要在应用程序启动后检测新的或更改的 blob，SDK 定期读取 blob 存储日志。 Blob 日志缓冲和仅获得物理写入每隔 10 分钟或因此，因此可能会显著延迟之后创建或更新相应的前一个 blob`BlobTrigger`函数执行。 

没有为您使用创建的 blob 异常`Blob`属性。 当 WebJobs SDK 创建新的斑点时，它将新的 blob 立即交给任何匹配`BlobTrigger`函数。 因此如果您有斑点的输入和输出的链，SDK 可以处理它们有效。 但如果您想创建的或通过其他方式更新 blob 的处理函数运行您 blob 的低延迟，我们建议使用`QueueTrigger`而不是`BlobTrigger`。

### <a id="receipts"></a>Blob 收据

WebJobs SDK 可确保没有`BlobTrigger`函数获取多个调用一次相同的新的或更新 blob。 这是通过维护*blob 回执*，以便确定已处理给定的 blob 版本。

在名为*webjobs 的 azure 主机*在 Azure 存储帐户 AzureWebJobsStorage 连接字符串所指定的容器中存储 blob 收据。 Blob 收据具有以下信息︰

* 函数调用为 blob （"*{WebJob 名称}*。功能。*{函数名称}*"，例如:"WebJob1.Functions.CopyBlob")
* 容器名称
* Blob 类型 （"BlockBlob"或"PageBlob"）
* Blob 名称
* ETag (blob 版本标识符，例如:"0x8D1DC6E70A277EF")

如果您想要强制 blob 的重新处理，您可以从*webjobs 的 azure 主机*容器来手动删除该 blob 的 blob 收据。

## <a id="queues"></a>队列项目所涉及的相关的主题

有关如何处理由队列消息触发的 blob 处理信息或 WebJobs SDK 以斑点不明确的情况下处理，请参见[如何使用 WebJobs sdk 的 Azure 队列存储](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)。 

在这篇文章中介绍的相关的主题如下所示︰

* 异步函数
* 多个实例
* 正常关机
* 使用 WebJobs SDK 的函数体中的属性
* 在代码中设置 SDK 连接字符串。
* 值为 WebJobs SDK 构造函数参数在代码中设置
* 配置`MaxDequeueCount`病毒 blob 处理。
* 手动触发函数
* 写入日志

## <a id="nextsteps"></a>下一步行动

本指南提供了代码示例来演示如何处理使用 Azure blob 的常见方案。 有关如何使用 Azure WebJobs 和 WebJobs SDK 的详细信息，请参阅[Azure WebJobs 推荐资源](http://go.microsoft.com/fwlink/?linkid=390226)。
 
