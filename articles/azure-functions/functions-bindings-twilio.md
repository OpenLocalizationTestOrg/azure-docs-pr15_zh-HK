<properties
    pageTitle="Azure 函数 Twilio 绑定 |Microsoft Azure"
    description="了解如何使用 Azure 函数 Twilio 绑定。"
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure 函数 Twilio 输出绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍如何配置和使用 Azure 函数 Twilio 绑定。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure 函数支持 Twilio 输出绑定启用您的函数来发送 SMS 短信只用几行代码和[Twilio](https://www.twilio.com/)帐户。 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Azure 通知中心的 function.json 输出绑定

Function.json 文件提供了以下属性︰

- `name`︰ 在函数代码中使用的 Twilio 短信变量的名称。
- `type`︰ 必须设置为*"twilioSms"*。
- `accountSid`︰ 此值必须设置为应用程序设置，保存您的 Twilio 帐户 Sid 的名称。
- `authToken`︰ 此值必须设置为包含您的 Twilio 身份验证令牌的应用程序设置的名称。
- `to`︰ 此值设置为 SMS 文本发送到的电话号码。
- `from`︰ 此值设置为 SMS 文本发送的电话号码。
- `direction`︰ 必须设置为*"出"*。
- `body`︰ 此值可用于硬编码 SMS 短信，如果不需要在您的函数的代码中动态地设置它。 

 
示例 function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>使用 Twilio 触发器示例 C# 队列输出绑定

#### <a name="synchronous"></a>同步

Azure 存储队列触发器此同步的示例代码使用 out 参数将文本消息发送到下订单的客户。

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>异步

Azure 存储队列触发器此异步示例代码将文本消息发送到下订单的客户。

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>使用 Twilio 的示例 Node.js 队列触发器输出绑定

本例中 Node.js 发送文本消息到下订单的客户。

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
