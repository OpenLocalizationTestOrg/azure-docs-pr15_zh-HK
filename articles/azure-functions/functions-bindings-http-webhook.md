<properties
    pageTitle="Azure 的函数 HTTP 和 webhook 绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 HTTP 和 webhook 触发器和绑定。"
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure 的函数 HTTP 和 webhook 绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍如何配置和 HTTP 和 webhook 触发器和绑定在 Azure 的函数中的代码。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>function.json 为 HTTP 和 webhook 绑定的

*Function.json*文件提供有关请求和响应的属性。

HTTP 请求的属性︰

- `name`︰ 在函数代码中使用请求对象 （或在 Node.js 函数的情况下请求正文） 变量的名称。
- `type`︰ 必须设置为*httpTrigger*。
- `direction`︰ 必须设置为*中*。 
- `webHookType`︰ 对于 WebHook 触发器，有效值*github*、*可宽延时间*和*genericJson*。 WebHook 不是 HTTP 触发器，将此属性设为空字符串。 WebHooks 的详细信息，请参阅下一节[WebHook 触发器](#webhook-triggers)。
- `authLevel`︰ 不能应用于 WebHook 触发器。 设置为"正常"，要求"匿名"放 API 的关键要求，或"admin"需要主的 API 密钥的 API 密钥。 有关更多信息，请参见下面的[API 密钥](#apikeys)。

HTTP 响应的属性︰

- `name`︰ 在函数代码中用于响应对象变量的名称。
- `type`︰ 必须设置为*http*。
- `direction`︰ 必须给*出*设置。 
 
示例*function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook 触发器

WebHook 触发器是具有以下功能为 WebHooks 设计的一个 HTTP 触发器︰

* 供应商特定 WebHook （目前 GitHub 和时差都支持），函数运行时验证提供者的签名。
* 对于 Node.js 函数，函数运行时提供了请求正文，而不是请求对象。 没有任何特殊处理的 C# 函数，因为您可以控制所提供的指定参数类型。 如果指定`HttpRequestMessage`获得请求对象。 如果您指定 POCO 类型，函数运行时将尝试分析填充对象属性的请求的正文中的 JSON 对象。
* 要触发 WebHook 函数的 HTTP 请求必须包含一个 API 键。 对于 WebHook HTTP 触发器，这一要求是可选的。

有关如何设置 GitHub WebHook 的信息，请参阅[GitHub 开发-创建 WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409)。

## <a name="url-to-trigger-the-function"></a>若要触发该函数的 URL

要触发某个函数，请到函数应用程序 URL 和函数名称的组合的 URL 发送一个 HTTP 请求︰

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API 键

默认情况下，API 密钥必须包含 HTTP 请求以触发一个 HTTP 或 WebHook 函数。 项可以包含在一个名为的查询字符串变量`code`，或者它可以包含在`x-functions-key`HTTP 标头。 对于非 WebHook 函数，您可以指示一个 API 键不必需的设置`authLevel` *function.json*文件中的属性为"匿名"。

可以在函数的应用程序的文件系统的*D:\home\data\Functions\secrets*文件夹中找到 API 密钥值。  主机键和功能键设置在*host.json*文件中，如本示例所示。 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

从*host.json*的功能键可以用于触发任何函数，但不会触发禁用的函数。 为主密钥可用于触发任何函数，将触发一个函数，即使它被禁用。 您可以配置需要通过将设置为主要密钥函数`authLevel`为"管理员"属性。 

如果*机密*文件夹包含一个 JSON 文件具有相同的名称作为函数，`key`在该文件中的属性也可以用于触发函数，并且此参数仅适用于引用的函数。 例如，一个名为函数的 API 键`HttpTrigger` *HttpTrigger.json*在*机密*文件夹中指定。 下面是一个示例︰

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] 当设置 WebHook 触发器的过程中时，不使用 WebHook 提供程序共享主密钥。 使用仅适用于处理 WebHook 函数的密钥。  为主密钥可用于触发任何函数，即使禁用的函数。

## <a name="example-c-code-for-an-http-trigger-function"></a>对于 HTTP 触发器函数 C# 的示例代码 

此代码示例查找`name`参数在查询字符串或 HTTP 请求的正文中。

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>对于 HTTP 触发器函数 F# 的示例代码

此代码示例查找`name`参数在查询字符串或 HTTP 请求的正文中。

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

您将需要`project.json`使用 NuGet 引用的文件`FSharp.Interop.Dynamic`和`Dynamitey`程序集，如下︰

```json
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
```

这将使用 NuGet 提取您的依赖性，并将在您的脚本中引用它们。

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>对于 HTTP 触发器函数的 Node.js 代码示例 

此代码示例查找`name`参数在查询字符串或 HTTP 请求的正文中。

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>GitHub WebHook 函数的 C# 的示例代码 

此代码示例记录 GitHub 问题注释。

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>GitHub WebHook 函数的 F# 的示例代码

此代码示例记录 GitHub 问题注释。

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>GitHub WebHook 函数的 Node.js 代码示例 

此代码示例记录 GitHub 问题注释。

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
