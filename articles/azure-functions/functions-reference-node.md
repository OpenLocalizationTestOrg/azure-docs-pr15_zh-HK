<properties
    pageTitle="Azure 函数 NodeJS 开发人员参考 |Microsoft Azure"
    description="了解如何开发使用 NodeJS Azure 函数。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure 函数 NodeJS 开发人员参考

> [AZURE.SELECTOR]
- [C# 脚本](../articles/azure-functions/functions-reference-csharp.md)
- [F# 脚本](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Azure 函数的节点/JavaScript 体验更加方便地导出传递函数`context`通信与运行时，以及接收和发送通过绑定的数据的对象。

本文假定您已经已经了解了[Azure 功能开发人员参考](functions-reference.md)。

## <a name="exporting-a-function"></a>导出函数

所有 JavaScript 函数必须都导出单个`function`通过`module.exports`运行时查找函数，并运行它。 此函数必须始终包括`context`对象。

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

绑定的`direction === "in"`作为函数参数，意味着您可以使用传递[`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx)新输入的动态处理 (例如，通过使用`arguments.length`循环访问所有的输入)。 此功能是非常方便，如果只有具有任何附加的输入，触发器可以预知触发器数据访问而不引用您`context`对象。

传递的参数是始终沿着*function.json*，在它们发生的顺序中的函数即使不导出语句中指定它们。 例如，如果您有`function(context, a, b)`将其更改为`function(context, a)`，您仍然可以获取值的`b`在函数代码中通过引用`arguments[3]`。

所有绑定，而不考虑方向，也在都传递沿`context`对象 （见下文）。 

## <a name="context-object"></a>上下文对象

运行时使用`context`对象传递函数与数据并使您可以与运行库进行通信。

上下文对象始终是第一个参数的函数，始终应该是包括的因为它具有方法如`context.done`和`context.log`的需要正确使用运行时。 您可以命名对象您希望的任何内容 (即`ctx`或`c`)。

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.bindings

`context.bindings`对象收集所有输入和输出数据。 将数据添加到`context.bindings`对象通过`name`绑定的属性。 例如，给定下面的绑定定义在*function.json*中，您可以访问通过队列的内容`context.bindings.myInput`。 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

`context.done`函数指示运行时完成后运行。 这一点很重要，要调用完成后使用功能;如果不这样做，运行时仍然永远不会知道您的函数完成。 

`context.done`功能，您可以到运行时，以及属性的一个属性包，此操作将覆盖属性在传递回用户定义的错误`context.bindings`对象。

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(message)

`context.log`方法可用于记录相关联的日志语句输出。 如果您使用`console.log`，您的消息将仅显示为进程级别的日志记录，这并不那么有用。

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

`context.log`方法支持节点[util.format 方法](https://nodejs.org/api/util.html#util_util_format_format)支持的参数相同的格式。 因此，例如，代码如下所示︰

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

可以编写如下︰

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>触发器 HTTP: context.req 和 context.res

对于 HTTP 触发器，因为它是这样一种常见模式使用`req`和`res`的 HTTP 请求和响应对象，我们决定以方便地访问这些上下文对象，而不是强迫您使用完整的`context.bindings.name`模式。

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>节点版本和包管理

节点的版本当前被锁定在`5.9.1`。 我们研究的多个版本添加支持，使其可配置。

通过函数的应用程序的文件系统中*package.json*文件上载到您的函数文件夹，您可以在函数中包括包。 文件上载的说明，请参阅[Azure 功能开发人员参考主题](functions-reference.md#fileupdate)的**函数的应用程序文件的更新**部分。 

您还可以使用`npm install`函数的应用程序的 SCM (Kudu) 的命令行界面中︰

1. 导航到︰ `https://<function_app_name>.scm.azurewebsites.net`。

2. 单击**调试控制台 > CMD**。

3. 定位到`D:\home\site\wwwroot\<function_name>`。

4. 运行`npm install`。

一旦安装所需的程序包，则将其导入到您的函数以通常的方式 (例如通过`require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>环境变量

若要获取环境变量或应用程序设置值，请使用`process.env`，如下面的代码示例所示︰

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>TypeScript/CoffeeScript 的支持

没有，还，自动编译 TypeScript/CoffeeScript 通过运行时，因此，将所有任何直接支持需要在部署时在运行时，外部处理。 

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅以下资源︰

* [Azure 功能开发人员参考](functions-reference.md)
* [Azure 函数 C# 开发人员参考](functions-reference-csharp.md)
* [Azure 函数 F# 开发人员参考](functions-reference-fsharp.md)
* [Azure 功能触发器和绑定](functions-triggers-bindings.md)
