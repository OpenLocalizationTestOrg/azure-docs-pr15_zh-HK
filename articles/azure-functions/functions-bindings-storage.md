<properties
    pageTitle="Azure 功能触发器和 Azure 存储的绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 Azure 存储触发器和绑定。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数，函数、 事件处理、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure 功能触发器和 Azure 存储的绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍了如何配置和代码 Azure 存储触发器和 Azure 函数中的绑定。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure 存储队列触发器

#### <a name="functionjson-for-storage-queue-trigger"></a>function.json 存储队列触发器

*Function.json*文件指定下列属性。

- `name`︰ 在函数代码中使用队列或队列消息变量名。 
- `queueName`︰ 要轮询的队列的名称。 队列的命名规则，请参见[命名的队列和元数据](https://msdn.microsoft.com/library/dd179349.aspx)。
- `connection`︰ 包含存储连接字符串的应用程序设置的名称。 如果将`connection`为空，该触发器将使用函数的应用程序，由 AzureWebJobsStorage 应用程序设置指定的默认存储连接字符串。
- `type`︰ 必须设置为*queueTrigger*。
- `direction`︰ 必须设置为*中*。 

示例*function.json*存储队列触发器︰

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>支持队列触发器的类型

队列消息可以反序列化到任何以下类型︰

* 对象 （从 JSON)
* 字符串
* 字节数组 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>队列触发器元数据

通过使用这些变量的名称，可以在函数中获取队列的元数据︰

* expirationTime
* insertionTime
* nextVisibleTime
* 标识
* popReceipt
* dequeueCount
* queueTrigger （另一种方法来检索队列消息文字设置为一个字符串）

此 C# 的代码示例检索和日志队列的元数据︰

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>毒信队列的消息进行处理

其内容会导致一个函数失败的消息称为*有害消息*。 当函数失败时，队列消息不会被删除并最终拾取，从而导致重复的周期。 您可以手动执行或 SDK 可以有限数目的迭代后, 自动中断周期。

SDK 将调用函数达 5 倍来处理队列消息。 如果第五重试失败，邮件将被移动到毒信队列。

毒信队列名为*{originalqueuename}*-有害。 您可以编写需要从毒信队列日志记录它们或发送通知该手动注意函数来处理消息。 

如果您希望手动处理有害消息，可以通过检查来获取次数，拾取消息处理`dequeueCount`。

## <a id="storagequeueoutput"></a>Azure 存储队列输出绑定

#### <a name="functionjson-for-storage-queue-output-binding"></a>function.json 存储队列输出绑定

*Function.json*文件指定下列属性。

- `name`︰ 在函数代码中使用队列或队列消息变量名。 
- `queueName`︰ 队列的名称。 队列的命名规则，请参见[命名的队列和元数据](https://msdn.microsoft.com/library/dd179349.aspx)。
- `connection`︰ 包含存储连接字符串的应用程序设置的名称。 如果将`connection`为空，该触发器将使用函数的应用程序，由 AzureWebJobsStorage 应用程序设置指定的默认存储连接字符串。
- `type`︰ 必须设置到*队列*。
- `direction`︰ 必须给*出*设置。 

示例中的*function.json*存储队列输出绑定使用队列触发器和写入队列消息︰

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>队列支持输出绑定类型

`queue`绑定可以序列化以下为队列的消息类型︰

* 对象 (`out T`在 C# 中创建一条消息带有 null 对象如果此参数为空，函数结束时)
* 字符串 (`out string`在 C# 中创建队列消息如果参数值在函数结束时非空)
* 字节数组 (`out byte[]`在 C# 的工作原理，如字符串) 
* `out CloudQueueMessage`(C#，运作字符串) 

在 C# 中还可以绑定到`ICollector<T>`或`IAsyncCollector<T>`，`T`是受支持的类型之一。

#### <a name="queue-output-binding-code-examples"></a>队列输出绑定代码示例

此 C# 代码示例将每个输入的队列的消息的单个输出队列消息。

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

此 C# 代码示例通过将多个消息`ICollector<T>`(使用`IAsyncCollector<T>`在异步函数):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure 存储 blob 触发器

#### <a name="functionjson-for-storage-blob-trigger"></a>存储 blob 触发器 function.json

*Function.json*文件指定下列属性。

- `name`: 在函数代码中用于该 blob 变量名。 
- `path`︰ 指定要监视的容器路径和 （可选） blob 名称模式。
- `connection`︰ 包含存储连接字符串的应用程序设置的名称。 如果将`connection`为空，该触发器将使用函数的应用程序，由 AzureWebJobsStorage 应用程序设置指定的默认存储连接字符串。
- `type`︰ 必须设置为*blobTrigger*。
- `direction`︰ 必须设置为*中*。

示例*function.json*监视添加到示例工作项容器的 blob 存储 blob 触发器︰

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Blob 触发器支持的类型

该 blob 可以反序列化到任何节点或 C# 函数中的以下类型︰

* 对象 （从 JSON)
* 字符串

在 C# 函数还可以绑定到任何以下类型︰

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* 由[ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb)反序列化的其他类型 

#### <a name="blob-trigger-c-code-example"></a>Blob 触发器 C# 代码示例

该 C# 代码示例将记录添加到被监视的容器每个 blob 中的内容。

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Blob 触发器名称模式

您可以指定中的 blob 名称模式`path`属性。 例如︰

```json
"path": "input/original-{name}",
```

此路径将查找名为*原始 Blob1.txt* *输入*容器的值中的 blob`name`变量在函数代码中的将`Blob1`。

另一个示例︰

```json
"path": "input/{blobname}.{blobextension}",
```

此路径还将查找名为*原始 Blob1.txt*和一个 blob`blobname`和`blobextension`*原 Blob1*和*txt*，都是在函数代码中的变量。

您可以限制通过指定模式的文件扩展名为固定值的触发功能的 blob 类型。 如果您设置`path` */采样 / {名称}.png*、 为*.png* blob 仅在*样本*容器将触发函数。

如果您需要指定名称中带有括号的 blob 名称的名称模式，双大括号。 例如，如果您想要查找具有类似名称的*图像*容器中的 blob:

        {20140101}-soundfile.mp3

使用此窗体`path`属性︰

        images/{{20140101}}-{name}

在示例中，`name`变量的值为*soundfile.mp3*。 

#### <a name="blob-receipts"></a>Blob 收据

Azure 函数运行时确保相同的新的或更新 blob 的多次调用获取无斑点触发器函数。 这是通过维护*blob 回执*，以便确定已处理给定的 blob 版本。

在名为*webjobs 的 azure 主机*在 Azure 存储帐户 AzureWebJobsStorage 连接字符串所指定的容器中存储 blob 收据。 Blob 收据具有以下信息︰

* 函数调用为 blob （"*{函数的应用程序名称}*。功能。*{函数名称}*"，例如:"functionsf74b96f7。Functions.CopyBlob")
* 容器名称
* Blob 类型 （"BlockBlob"或"PageBlob"）
* Blob 名称
* ETag (blob 版本标识符，例如:"0x8D1DC6E70A277EF")

如果您想要强制 blob 的重新处理，您可以从*webjobs 的 azure 主机*容器来手动删除该 blob 的 blob 收据。

#### <a name="handling-poison-blobs"></a>处理病毒的 blob

当 blob 触发器函数失败时，SDK 调用它再次，以防故障由暂时的错误。 如果失败由内容的 blob，函数将失败，每当它试图处理 blob。 默认情况下，SDK 调用一个函数最多 5 次为一个给定的 blob。 如果第五重试失败，SDK 中添加到名为*webjobs-blobtrigger-毒性*队列消息。

有害的 blob 的队列消息是一个 JSON 对象，该对象包含以下属性︰

* FunctionId （在格式*{函数的应用程序名称}*。功能。*{函数名称}*）
* BlobType （"BlockBlob"或"PageBlob"）
* 容器名称
* BlobName
* ETag (blob 版本标识符，例如:"0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>大型容器的轮询的斑点

监视该触发器的 blob 容器包含 blob 超过 10000 个，如果函数运行时扫描日志文件来监视的新的或更改的 blob。 这一过程不是实时的。函数可能不触发几分钟或更长时间之后创建 blob。 此外，[存储日志创建在"尽最大努力"](https://msdn.microsoft.com/library/azure/hh343262.aspx)的基础;就不能保证，将捕获所有事件。 某些情况下，日志可能会丢失。 如果大型容器的 blob 触发器的速度和可靠性限制无法接受您的应用程序，建议的方法是创建队列邮件，当您创建 blob，并使用队列触发器来代替 blob 触发器来处理 blob。
 
## <a id="storageblobbindings"></a>Azure 存储 blob 输入和输出绑定

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>存储 blob 的 function.json 输入或输出绑定

*Function.json*文件指定下列属性。

- `name`: 在函数代码中用于该 blob 变量名。 
- `path`︰ 指定读取从 blob 或写入到 blob 容器路径和 （可选） 的 blob 名称模式。
- `connection`︰ 包含存储连接字符串的应用程序设置的名称。 如果将`connection`为空，该绑定将使用函数的应用程序，由 AzureWebJobsStorage 应用程序设置指定的默认存储连接字符串。
- `type`︰ 必须设置为*blob*。
- `direction`︰ 设置为*或*缩小**。 

示例中的*function.json*存储 blob 输入或输出使用队列触发器复制一个 blob 的绑定︰

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>斑点输入和输出受支持的类型

`blob`绑定可以序列化或反序列化 Node.js 或 C# 函数中的以下类型︰

* 对象 (`out T` C# 的输出 blob 中︰ 如果参数值为 null，在函数结束后为空的对象创建一个 blob)
* 字符串 (`out string` C# 的输出 blob 中︰ 创建 blob 的字符串参数为非空，当函数返回时，才)

在 C# 函数，也可以绑定到以下类型︰

* `TextReader`（仅输入）
* `TextWriter`（仅适用于输出）
* `Stream`
* `CloudBlobStream`（仅适用于输出）
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Blob 输出 C# 代码示例

本 C# 代码示例复制其名称接收队列消息中的 blob。

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure 存储表输入和输出绑定

#### <a name="functionjson-for-storage-tables"></a>用于存储表 function.json

*Function.json*指定以下属性。

- `name`: 在函数代码中使用的表绑定的变量名称。 
- `tableName`: 表的名称。
- `partitionKey`和`rowKey`︰ 阅读 C# 或节点函数中的单个实体或在节点函数编写单个实体一起使用。
- `take`: 最大读取的输入节点函数中的表的行数。
- `filter`︰ 节点函数中输入表 OData 筛选器表达式。
- `connection`︰ 包含存储连接字符串的应用程序设置的名称。 如果将`connection`为空，该绑定将使用函数的应用程序，由 AzureWebJobsStorage 应用程序设置指定的默认存储连接字符串。
- `type`︰ 必须设置为*表格*。
- `direction`︰ 设置为*或*缩小**。 

下面的示例*function.json*使用队列触发器来读取一个表行。 JSON 提供硬性分区密钥值，并指定行键来自队列的消息。

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>支持的存储表格输入和输出类型

`table`绑定可以序列化或反序列化 Node.js 或 C# 函数中的对象。 这些对象将具有 RowKey 和 PartitionKey 属性。 

在 C# 函数，也可以绑定到以下类型︰

* `T`其中`T`实现`ITableEntity`
* `IQueryable<T>`（仅输入）
* `ICollector<T>`（仅适用于输出）
* `IAsyncCollector<T>`（仅适用于输出）

#### <a name="storage-tables-binding-scenarios"></a>存储表绑定方案

表绑定支持下列方案︰

* 阅读 C# 或节点函数中的一行。

    设置`partitionKey`， `rowKey`。 `filter` ，`take`在这种情况下不使用属性。

* 阅读的 C# 函数中的多个行。

    函数运行时提供了`IQueryable<T>`对象绑定到的表。 类型`T`必须从`TableEntity`或实现`ITableEntity`。 `partitionKey`， `rowKey`， `filter`，和`take`属性不用在这种情况下;您可以使用`IQueryable`对象来完成任何必需的筛选。 

* 读取的节点函数中的多个行。

    设置`filter`和`take`属性。 不能设置`partitionKey`或`rowKey`。

* C# 函数中写入一行或多行。

    函数运行时提供了`ICollector<T>`或`IAsyncCollector<T>`绑定到表，其中`T`指定您想要添加的实体的架构。 通常情况下，键入`T`从`TableEntity`或实现`ITableEntity`，但是它没有。 `partitionKey`， `rowKey`， `filter`，和`take`在这种情况下不使用属性。

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>存储表格示例︰ 读取 C# 或节点中的单个表实体

下面的 C# 代码示例与前面的*function.json*文件前面所示为单个表实体的工作。 队列消息具有行的键值并表实体读取到*run.csx*文件中定义的类型。 类型包括`PartitionKey`，`RowKey`属性并不是派生自`TableEntity`。 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

与前面的*function.json*文件，读取单个表实体也适用下面的 F# 代码示例。

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

下面的代码示例节点也适用与前面的*function.json*文件，读取单个表实体。

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>存储表格示例︰ 阅读 C 中的多个表实体# 

以下*function.json*和 C# 的代码示例读取队列消息中指定的分区键的实体。

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# 代码将添加对 Azure 存储 SDK 的引用，以便可以从派生的实体类型`TableEntity`。

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>存储表格示例︰ 在 C 中创建表的实体# 

*Function.json*和*run.csx*下面的示例演示如何编写在 C# 表的实体。

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>存储表格示例︰ 在 F 中创建表的实体#

*Function.json*和*run.fsx*下面的示例演示如何编写在 F# 表的实体。

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>存储表格示例︰ 在节点中创建的表的实体

*Function.json*和*run.csx*下面的示例演示如何编写表实体节点中。

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
