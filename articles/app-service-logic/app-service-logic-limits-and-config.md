<properties
    pageTitle="逻辑应用程序限制和配置 |Microsoft Azure"
    description="服务限制和配置值可用的逻辑应用程序概述。"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>逻辑应用程序限制和配置

以下是 Azure 逻辑应用程序的当前限制和配置的详细信息的信息。

## <a name="limits"></a>限制

### <a name="http-request-limits"></a>HTTP 请求限制

这些都是限制单个 HTTP 请求和/或连接器调用

#### <a name="timeout"></a>超时

|名称|限制|备注|
|----|----|----|
|请求超时|1 分钟|根据需要可以补偿[异步模式](app-service-logic-create-api-app.md)，或者[直到循环](app-service-logic-loops-and-scopes.md)|

#### <a name="message-size"></a>邮件大小

|名称|限制|备注|
|----|----|----|
|邮件大小|50 MB|有些连接线和 Api 可能不支持 50 MB。  请求触发器支持最多 25MB|
|表达式求值限制|131072 个字符|`@concat()``@base64()`，`string`不能比这更长的时间|

#### <a name="retry-policy"></a>重试策略

|名称|限制|备注|
|----|----|----|
|重试次数|4|可以使用[重试策略参数](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)配置|
|重试连接最大延迟|1 小时|可以使用[重试策略参数](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)配置|
|重试延迟时间最小值|20 分钟|可以使用[重试策略参数](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)配置|

### <a name="run-duration-and-retention"></a>运行持续时间和保留

这些都是对单个的逻辑应用程序运行的限制。

|名称|限制|备注|
|----|----|----|
|运行持续时间|90 天||
|存储保留|90 天|它是从运行的开始时间|
|最小值重复出现的间隔|15 秒||
|最大重复间隔|500 天||


### <a name="looping-and-debatching-limits"></a>循环和 debatching 限制

这些都是限制为单个逻辑应用程序运行。

|名称|限制|备注|
|----|----|----|
|ForEach 项目|5000|您可以使用[查询操作](../connectors/connectors-native-query.md)筛选器所需的更大的阵列|
|直到迭代|10000||
|SplitOn 项目|5000||
|ForEach 并行度|20|您可以通过添加设置为连续 foreach`"operationOptions": "Sequential"`到`foreach`操作|


### <a name="throughput-limits"></a>吞吐量限制

这些都是限制为单个逻辑应用程序实例。 

|名称|限制|备注|
|----|----|----|
|每秒的触发器|100|可以根据需要在多个应用程序分配工作流|

### <a name="definition-limits"></a>定义限制

这些都是一个逻辑的应用程序定义的限制。

|名称|限制|备注|
|----|----|----|
|在 ForEach 操作|1|您可以根据需要扩展该嵌套工作流|
|每个工作流操作|60|您可以根据需要扩展该嵌套工作流|
|允许操作嵌套深度|5|您可以根据需要扩展该嵌套工作流|
|每个地区每个订阅的流程|1000||
|每个工作流触发器|10||
|每个表达式的最大字符数|8192||
|最大值`trackedProperties`大小，以字符为单位|16000|
|`action`/`trigger`名称限制|80||
|`description`长度限制|256||
|`parameters`限制|50||
|`outputs`限制|10||

## <a name="configuration"></a>配置

### <a name="ip-address"></a>IP 地址

从[连接器](../connectors/apis-list.md)进行的调用将来自下面指定的 IP 地址。

从直接 （即通过[HTTP](../connectors/connectors-native-http.md)或[HTTP + Swagger](../connectors/connectors-native-http-swagger.md)） 的逻辑应用程序发出的调用可能来自任何的[Azure 数据中心 IP 范围](https://www.microsoft.com/en-us/download/details.aspx?id=41653)。

|逻辑应用程序区域|出站 IP|
|-----|----|
|澳大利亚东部|40.126.251.213|
|澳大利亚东南部|40.127.80.34|
|巴西南部|191.232.38.129|
|中央印度|104.211.98.164|
|美国中部|40.122.49.51|
|东亚|23.99.116.181|
|东亚的美国|191.237.41.52|
|东亚美国 2|104.208.233.100|
|日本东|40.115.186.96|
|日本西部|40.74.130.77|
|美国中北部|65.52.218.230|
|北欧|104.45.93.9|
|美国中南部|104.214.70.191|
|东南亚|13.76.231.68|
|印度南部|104.211.227.225|
|西欧|40.115.50.13|
|西部的印度|104.211.161.203|
|美国西部|104.40.51.248|


## <a name="next-steps"></a>下一步行动  

- 入门的逻辑应用程序，请按照[创建逻辑应用程序](app-service-logic-create-a-logic-app.md)的教程。  
- [查看常见示例和应用场景](app-service-logic-examples-and-scenarios.md)
- [可以自动执行业务流程逻辑的应用程序](http://channel9.msdn.com/Events/Build/2016/T694) 
- [了解如何将您的系统集成与应用程序逻辑](http://channel9.msdn.com/Events/Build/2016/P462)