<properties
    pageTitle="Azure 函数 DocumentDB 绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 Azure DocumentDB 绑定。"
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
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure 函数 DocumentDB 绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍了如何配置和 Azure 函数中的代码 Azure DocumentDB 绑定。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB 输入绑定

输入的绑定可以将一个文档加载 DocumentDB 集合中并将其传递到绑定直接。 可以基于触发器调用该函数来确定文档 id。 C# 函数中对记录所做的任何更改将自动发送回集合函数成功退出时。

#### <a name="functionjson-for-documentdb-input-binding"></a>function.json 的 DocumentDB 输入绑定

*Function.json*文件提供了以下属性︰

- `name`︰ 在函数代码中使用的文档变量的名称。
- `type`︰ 必须设置为"documentdb"。
- `databaseName`︰ 包含文档数据库。
- `collectionName`︰ 包含文档集合。
- `id`︰ 若要检索文档 Id。 此属性支持绑定类似于"{queueTrigger}"，它将使用队列消息的字符串值作为文档 id。
- `connection`︰ 此字符串必须是应用程序设置为您的 DocumentDB 帐户的终结点。 如果您从集成选项卡中选择您的帐户，将采用以下的形式中，yourAccount_DOCUMENTDB 的名称为您创建新的应用程序设置。 如果您需要手动创建应用程序设置，实际连接字符串必须采用以下格式，AccountEndpoint =<Endpoint for your account>;AccountKey =<Your primary access key>;。
- 方向︰ 必须设置为*"中"*。

示例*function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>C# 队列触发器的 azure DocumentDB 输入的代码示例
 
使用上面的示例 function.json，DocumentDB 输入的绑定将检索匹配的队列消息字符串的 id 与该文档并将其传递到文档参数。 如果未找到该文档，文档参数将为 null。 函数退出时，文档然后将更新与新的文本值。
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>F# 队列触发器的 azure DocumentDB 输入的代码示例

使用上面的示例 function.json，DocumentDB 输入的绑定将检索匹配的队列消息字符串的 id 与该文档并将其传递到文档参数。 如果未找到该文档，文档参数将为 null。 函数退出时，文档然后将更新与新的文本值。

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

您将需要`project.json`使用 NuGet 指定的文件`FSharp.Interop.Dynamic`和`Dynamitey`包作为包的依赖项，如下所示︰

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

这将使用 NuGet 提取您的依赖性，并将在您的脚本中引用它们。

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure Node.js 队列触发器的 DocumentDB 输入的代码示例
 
使用上面的示例 function.json，DocumentDB 输入的绑定将检索该文档 id 相匹配的队列消息字符串，并将其传递给`documentIn`绑定属性。 在 Node.js 函数中，更新的文档不会发送回集合。 但是，您可以输入的绑定将直接传递给 DocumentDB 输出绑定命名`documentOut`支持更新。 此代码示例更新输入文档的文本属性，并将其设置为输出文档。
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB 输出绑定

这些函数可以编写 JSON 文档到 Azure DocumentDB 数据库使用**Azure DocumentDB 文档**输出绑定。 了解更多有关 Azure DocumentDB 查看[简介 DocumentDB](../documentdb/documentdb-introduction.md)和[快速入门教程](../documentdb/documentdb-get-started.md)。

#### <a name="functionjson-for-documentdb-output-binding"></a>DocumentDB 的 function.json 输出绑定

Function.json 文件提供了以下属性︰

- `name`︰ 在函数代码中使用新文档变量的名称。
- `type`︰ 必须设置为*"documentdb"*。
- `databaseName`: 数据库，其中包含将在其中创建新的文档集合。
- `collectionName`︰ 创建新文档的位置的集合。
- `createIfNotExists`︰ 这是一个布尔值，指示是否将创建集合，如果它不存在。 默认值为*false*。 原因这是新的集合创建与保留的吞吐量，其定价的影响。 有关详细信息，请访问[定价页](https://azure.microsoft.com/pricing/details/documentdb/)。
- `connection`︰ 此字符串必须是**应用程序设置**设置为您的 DocumentDB 帐户的终结点。 如果您从**集成**选项卡中选择您的帐户，新建应用程序设置将为您创建名称采用以下形式， `yourAccount_DOCUMENTDB`。 如果您需要手动创建应用程序设置，实际连接字符串必须采取以下形式， `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`。 
- `direction`︰ 必须设置为*"出"*。 
 
示例 function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Node.js 队列触发器的 azure DocumentDB 输出代码示例

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

输出文档中︰

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>F# 队列触发器的 azure DocumentDB 输出代码示例

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>C# 队列触发器的 azure DocumentDB 输出代码示例


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB 输出的代码示例设置文件的名称

如果您想要在该函数中设置的文档的名称，只需设置`id`值。  例如，如果 JSON 内容为某一员工已被放到队列类似于以下内容︰

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

在队列触发器函数中，可以使用下面的 C# 代码︰ 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

或等效的 F# 代码︰

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

输出示例︰

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
