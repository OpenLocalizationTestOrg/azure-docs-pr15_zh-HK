<properties
   pageTitle="逻辑应用程序循环、 范围和 Debatching |Microsoft Azure"
   description="逻辑应用程序循环、 范围和 debatching 概念"
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
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>逻辑应用程序循环、 范围和 Debatching
  
>[AZURE.NOTE] 本版文章应用到逻辑应用程序 2016年-04-01-预览架构及更高版本。  概念是类似对于较旧的架构，但范围仅供此架构及更高版本。
  
## <a name="foreach-loop-and-arrays"></a>ForEach 循环和阵列
  
逻辑应用程序允许您循环访问一组数据并为每个项执行的操作。  这一点通过`foreach`操作。  在设计器中，您可以指定要添加的每个循环。  在选择您想要循环访问的数组后，就可以开始添加操作。  目前仅限于 foreach 循环中，每一个操作，但将在未来几周中解除这种限制。  一次循环中您可以开始指定数组的每个值在发生什么。

如果使用代码视图，您可以指定为类似下面的每个循环。  这是一种用于发送包含 microsoft.com' 的每个电子邮件地址电子邮件每个循环︰

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A`foreach`操作可循环转移阵列最多 5000 行。  每次迭代可以并行执行，因此可能需要将消息添加到一个队列，如果需要流控制。
  
## <a name="until-loop"></a>直到循环
  
  直到满足某个条件时，可以执行一个操作或一系列操作。  最常见的情况，为此正在调用终结点，直到获得您正在寻找的响应。  在设计器中，您可以指定要添加到循环。  添加后内部循环的操作，您可以设置退出条件，以及循环限制。  循环周期之间还有 1 分钟的延迟。
  
  如果使用代码视图，您可以指定直到下面的循环等。  这是一种调用一个 HTTP 端点，直到响应正文具有值已完成。  它将完成时或者 
  
  * HTTP 响应的状态为已完成
  * 试图 1 小时
  * 它已经循环 100 次
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn 和 Debatching

有时一个触发器可能会收到一个 debatch 和启动每个项目工作流所需的项的数组。  这可以通过实现`spliton`命令。  默认情况下，如果触发器 swagger 指定为数组、 负载`spliton`将被添加和启动每个项目的运行。  SplitOn 只能添加到一个触发器。  这可以手动配置或重写定义代码视图中。  Debatch SplitOn 可以当前阵列多达 5000 项。  不能有`spliton`，还实现了 syncronous 响应模式。  调用的任何工作流具有`response`于操作`spliton`发送即时并运行 asyncronously`202 Accepted`响应。  

可以在代码视图中指定 SplitOn，如下面的示例。  此接收一个项的数组并在每一行上 debatches。

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>作用域

很可能要进行分组的一系列操作一起使用范围。  这是用于实现的异常处理非常有用。  可以在设计器中添加一个新作用域，并开始添加内的任何操作。  您可以在如下所示的代码视图中定义作用域︰


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
