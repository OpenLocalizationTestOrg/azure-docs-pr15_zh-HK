<properties
    pageTitle="Azure 函数计时器触发 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用计时器触发器。"
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

# <a name="azure-functions-timer-trigger"></a>Azure 函数计时器触发

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍如何配置计时器触发器在 Azure 的函数中。 计时器触发基于计划、 一次或定期调用函数。  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>计时器触发的 function.json

*Function.json*文件提供的日程安排表达式。 例如，以下调度运行函数每分钟︰

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

计时器触发自动处理多实例扩张︰ 将所有实例运行单个实例的特定的计时器功能。

## <a name="format-of-schedule-expression"></a>计划表达式的格式

计划表达式是[CRON 表达式](http://en.wikipedia.org/wiki/Cron#CRON_expression)，其中包含 6 个字段︰ `{second} {minute} {hour} {day} {month} {day of the week}`。 

请注意您在网上找到的 cron 表达式的许多省略 {二} 的字段中，，所以如果要复制其中一个从您必须调整为额外的字段。 

下面是一些其他计划表达式示例︰

若要触发每隔 5 分钟记录一次︰

```json
"schedule": "0 */5 * * * *"
```

若要在顶部每隔一小时一次触发︰

```json
"schedule": "0 0 * * * *",
```

若要触发每两小时一次︰

```json
"schedule": "0 0 */2 * * *",
```

若要触发一次每小时从上午 9 点到下午 5 点︰

```json
"schedule": "0 0 9-17 * * *",
```

若要在每个工作日上午 9:30 触发︰

```json
"schedule": "0 30 9 * * *",
```

若要在每个工作日上午 9:30 触发︰

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>计时器的触发 C# 代码示例

此 C# 代码示例写入单个日志每次触发的函数。

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
