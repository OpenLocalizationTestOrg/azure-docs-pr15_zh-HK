<properties
   pageTitle="逻辑应用程序内容键入处理 |Microsoft Azure"
   description="了解逻辑应用程序如何处理用在设计和运行库的内容类型"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>处理的逻辑应用程序内容类型

有许多不同类型的内容可以通过逻辑应用程序 — 包括 JSON、 XML、 平面文件和二进制数据的流动。  支持所有内容类型，一些本身理解的逻辑应用程序引擎，而其他人可能需要强制转换或转换为所需。  下文将说明引擎如何处理不同的内容类型和方式可以正确处理这些需要。

## <a name="content-type-header"></a>内容类型标头

以简单的开始，让我们看一下两个`Content-Types`，不需要任何转换或强制转换在逻辑应用程序 — 使用`application/json`和`text/plain`。

### <a name="applicationjson"></a>应用程序/json

依赖于工作流引擎`Content-Type`来自 HTTP 标头调用来确定适当的处理。  任何请求的内容类型与`application/json`将存储并作为一个 JSON 对象处理。  此外，JSON 内容可以分析默认情况下中，而不需要任何转换。  如此有内容类型标头的请求`application/json `如下所示︰

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

无法分析工作流具有类似的表达式中`@body('myAction')['foo'][0]`以获取值 (在此情况下， `bar`)。  没有其他的强制转换，则需要。  如果您正在使用 JSON 但没有指定标头的数据，您可以手动将其强制转换为 JSON 使用`@json()`函数 (例如︰ `@json(triggerBody())['foo']`)。

### <a name="textplain"></a>纯文本

类似于`application/json`，与接收到的 HTTP 消息`Content-Type`头`text/plain`将被存储在原始窗体。  此外，如果包括在后续操作而不需要任何转换请求将转与`Content-Type`:`text/plain`头。  例如，如果使用平面文件，您可能会收到以下 HTTP 内容︰

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  如果下一步操作中发送另一个请求的正文 (`@body('flatfile')`)，请求者必须`text/plain`内容类型标头。  如果您正在使用的是纯文本，但没有指定标头的数据，您可以手动将其转换为文本使用`@string()`函数 (例如︰ `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>应用程序/xml 和应用程序/八位字节流和转换器功能

逻辑应用程序引擎将始终保持`Content-Type`，接收 HTTP 请求或响应。  这意味着如果将内容接收与`Content-Type`的`application/octet-stream`，包括后续行动与不强制转换将导致的传出请求`Content-Type`: `application/octet-stream`。  在这样的引擎可以 guaruntee 数据不会丢失整个工作流中移动。  但是，动作状态 （输入和输出） 存储在一个 JSON 对象通过工作流流动。  这意味着为了保留某些数据类型，该引擎会将内容转换为相应的元数据保留两者的二进制 base64 编码字符串`$content`和`$content-type`-它将自动转换。  您可以手动转换内容类型之间使用内置的转换器︰

* `@json()`-将强制转换为的数据`application/json`
* `@xml()`-将强制转换为的数据`application/xml`
* `@binary()`-将强制转换为的数据`application/octet-stream`
* `@string()`-将强制转换为的数据`text/plain`
* `@base64()`-将内容转换为 base64 字符串
* `@base64toString()`-base64 编码将字符串转换为`text/plain`
* `@base64toBinary()`-base64 编码将字符串转换为`application/octet-stream`
* `@encodeDataUri()`-dataUri 字节数组的形式对字符串进行编码
* `@decodeDataUri()`-对 dataUri 进行解码转换为字节数组

例如，如果您接收到的 HTTP 请求与`Content-Type`:`application/xml`的︰

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

我无法强制转换和以后使用类似`@xml(triggerBody())`，或内所示的函数`@xpath(xml(triggerBody()), '/CustomerName')`。

### <a name="other-content-types"></a>其他内容类型

其他内容类型受支持，将使用逻辑的应用程序，但可能需要手动进行解码中检索邮件正文`$content`。  例如，如果我被关闭的触发`application/x-www-url-formencoded`看起来类似于下面的请求︰

```
CustomerName=Frank&Address=123+Avenue
```

因为这不是纯文本或 JSON 存入为该操作︰

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

其中`$content`被编码为 base64 字符串保留所有数据的有效负载。  由于目前尚无用于窗体数据的本机函数，因此仍然无法通过手动访问的数据所示的函数一起使用此工作流中的数据`@string(body('formdataAction'))`。  如果我愿意我传出的请求也有`application/x-www-url-formencoded`内容类型标头，我无法只是将其添加到操作正文而不需要任何转换类似`@body('formdataAction')`。  然而，这只会工作如果正文中的唯一参数`body`输入。  如果您尝试执行`@body('formdataAction')`内的`application/json`请求将获得运行时错误，因为它将发送编码的正文。
