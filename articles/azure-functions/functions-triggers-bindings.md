<properties
    pageTitle="Azure 功能触发器和绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用触发器和绑定。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure 的函数和触发器绑定开发人员参考


本主题提供一般参考触发器和绑定。 它包括一些高级的绑定功能和支持的所有绑定类型的语法。  

如果您正在寻找解决配置和编码特定类型的触发器或绑定的详细信息，您可能需要单击其中一个或多个下面列出的绑定︰

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

这些文章假定您已经阅读[Azure 功能开发人员参考](functions-reference.md)，以及[C#](functions-reference-csharp.md)、 [F#](functions-reference-fsharp.md)或[Node.js](functions-reference-node.md)开发人员参考文章。



## <a name="overview"></a>概述

触发器是用来触发自定义的代码的事件响应。 它们允许您在 Azure 平台之间或内部部署对事件做出响应。 绑定表示必要的元数据，用来连接您的代码所需的触发器相关联的输入或输出数据。

要更好地了解不同的绑定可与您 Azure 函数的应用程序集成的请参阅下表。

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

为了更好地理解触发并绑定通常情况下，假设您想要执行到新项放到 Azure 存储队列的进程的一些代码。 Azure 的函数提供了 Azure 队列触发器来支持此功能。 您将需要，监视队列的以下信息︰
 
- 该队列存在并在其中存储帐户。
- 队列名。
- 变量名称，您的代码将用于引用该被放到队列的新项目。  
 
队列触发器绑定包含此信息的 Azure 的函数。 每个函数的*function.json*文件包含所有相关的绑定。  下面是一个示例包含队列*function.json*触发绑定。 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

您的代码可能会发送不同类型的输出，具体取决于如何处理新的队列项。 例如，可能要到 Azure 存储表编写一个新的记录。  若要完成此任务，您可以设置输出绑定到 Azure 存储表。 下面是包含队列触发器可以用于存储表输出绑定示例*function.json* 。 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


下面的 C# 函数响应被放到队列的新项，并将新用户条目到 Azure 存储表。

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

有关代码示例和 Azure 存储所支持的类型有关的更具体信息，请参阅[Azure 功能触发器和 Azure 存储的绑定](functions-bindings-storage.md)。


在 Azure 门户使用的更高级的绑定功能，请单击函数**集成**选项卡上的**高级的编辑器**选项。 高级的编辑器使您可以直接在门户网站中的*function.json* 。

## <a name="dynamic-parameter-binding"></a>动态参数绑定 

而不是静态配置输出绑定属性的设置，您可以配置的设置要动态绑定到触发器的输入绑定的一部分的数据。 考虑一个方案，其中新订单处理使用 Azure 存储队列。 每个新的队列项是 JSON 字符串至少包含以下属性︰

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

您可能想要向客户发送短信使用 Twilio 帐户作为更新订单已收到。  可以配置`body`和`to`字段您 Twilio 输出绑定动态绑定到`name`， `mobileNumber` ，属于输入如下所示。

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
现在函数代码只具有初始化输出参数，如下所示。 在执行过程中的输出属性将被绑定到所需的输入数据。

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>随机 Guid

Azure 函数提供语法来与您的绑定生成随机 Guid。 下面的绑定语法将输出写入新 BLOB Azure 存储容器中的唯一名称︰ 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>返回单个输出

在函数代码中返回单个输出的情况下，您可以使用名为输出绑定`$return`保留在您的代码更自然的函数签名。 这只能与支持返回值 C#，Node.js （F#） 的语言。 该绑定应类似于下面用队列触发器的 blob 输出绑定。

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


下面的 C# 代码返回输出更加自然而无需使用`out`的函数签名中的参数。


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


这种方法与 Node.js 下面演示。

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F# 下面提供的示例。

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

这也可具有多个输出参数通过指定与单个输出`$return`。


## <a name="route-support"></a>工艺路线的支持

默认情况下创建 HTTP 触发器或 WebHook，一个函数时函数为窗体的路由寻址︰

    http://<yourapp>.azurewebsites.net/api/<funcname> 

您可以自定义此路由使用可选的`route`上的 HTTP 触发器属性的输入绑定。 例如，下面的*function.json*文件定义`route`HTTP 触发器的属性︰

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

使用此配置，该函数现与下列路由而不是原来的路由寻址。

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

这样，函数代码以支持两个参数的地址， `category` ， `id`。 与您的参数，您可以使用任何[Web API 工艺约束](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints)。 下面的 C# 函数代码使用两个参数。

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

这里是 Node.js 函数代码使用相同的工艺参数。

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

默认情况下，函数中的所有路由都前缀与*api*。 您还可以自定义或删除前缀使用`http.routePrefix` *host.json*文件中的属性。 下面的示例使用*host.json*文件中的前缀为一个空字符串移除*api*路由前缀。

    {
      "http": {
        "routePrefix": ""
      }
    }

有关如何更新您的函数，请参阅[如何更新函数的应用程序文件](functions-reference.md#fileupdate)的*host.json*文件的详细信息。 

通过添加此配置，功能现在是可寻址与以下路由︰

    http://<yourapp>.azurewebsites.net/products/electronics/357

您可以在*host.json*文件中配置的其他属性的信息，请参阅[host.json 参考](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json)。





## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅以下资源︰

* [测试函数](functions-test-a-function.md)
* [扩展函数](functions-scale.md)
