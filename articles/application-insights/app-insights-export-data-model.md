<properties 
    pageTitle="应用程序数据模型的见解" 
    description="介绍了从 json 格式，连续导出导出，用作筛选器的属性。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>应用程序的见解导出数据模型

此表列出了遥测从[应用程序的见解](app-insights-overview.md)Sdk 发送到门户网站的属性。 您将看到数据[连续导出](app-insights-export-telemetry.md)输出中的这些属性。
它们还显示在[诊断搜索](app-insights-diagnostic-search.md)[度量资源管理器](app-insights-metrics-explorer.md)中的属性筛选器。

需要注意的事项︰

* `[0]`这些表中表示必须要插入索引; 路径中的点但它并不总是 0。
* 持续时间是以十分之一微秒，因此 10000000 = = 1 秒。
* 日期和时间是 UTC，并给出了 ISO 格式`yyyy-MM-DDThh:mm:ss.sssZ`

有几个[示例](app-insights-export-telemetry.md#code-samples)说明如何使用它们。



## <a name="example"></a>示例

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>上下文

所有类型的遥测都附有上下文部分。 不是所有这些字段传输与每个数据点。



|路径|类型|备注|
|---|---|---|
| context.custom.dimensions [0]  | [对象]  | 通过自定义属性参数设置键 / 值字符串对。 密钥的最大长度 100，数值最大长度为 1024年。 100 多个唯一值，该属性可以搜索，但不能用于分段。 每个 ikey 最大 200 项。  |
| context.custom.metrics [0]  | [对象]  | 通过自定义的测量参数和 TrackMetrics 设置的键 / 值对。 密钥的最大长度 100，值可能是数字。 |
| context.data.eventTime | 字符串 | UTC |
| context.data.isSynthetic | 布尔值 | 请求似乎来自于 bot 或 web 测试。 |
| context.data.samplingRate | 编号 | 由发送到门户的 SDK 生成的遥测的百分比。 0.0 100.0 的范围。|
| context.device | 对象 | 客户端设备 |
| context.device.browser | 字符串 | IE，镶边... |
| context.device.browserVersion | 字符串 | Chrome 48.0... |
| context.device.deviceModel | 字符串 | |
| context.device.deviceName | 字符串 | |
| context.device.id | 字符串 | |
| context.device.locale | 字符串 | en GB，DE-DE，...... |
| context.device.network | 字符串 | |
| context.device.oemName | 字符串 | |
| context.device.osVersion | 字符串 | 主机操作系统 |
| context.device.roleInstance | 字符串 | 服务器主机的 ID |
| context.device.roleName | 字符串 | |
| context.device.type | 字符串 | PC 上，浏览器中... |
| context.location | 对象 | 从 clientip 派生。 |
| context.location.city | 字符串 | 如果已知，来自 clientip，  |
| context.location.clientip | 字符串 | 最后一个八边形被 anonymized 为 0。 |
| context.location.continent | 字符串 | |
| context.location.country | 字符串 | |
| context.location.province | 字符串 | 省 / 自治区 |
| context.operation.id | 字符串 | 具有相同的操作 id 的项在门户中显示为相关项目。 通常请求 id。 |
| context.operation.name | 字符串 | url 或请求 |
| context.operation.parentId | 字符串 | 允许嵌套的相关的项目。 |
| context.session.id | 字符串 | 一组来自相同来源的操作 id。 一段 30 分钟无操作信号会话结束。 |
| context.session.isFirst | 布尔值 | |
| context.user.accountAcquisitionDate | 字符串 | |
| context.user.anonAcquisitionDate | 字符串 | |
| context.user.anonId | 字符串 | |
| context.user.authAcquisitionDate | 字符串 | [身份验证的用户](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | 布尔值 | |
| internal.data.documentVersion | 字符串 | |
| internal.data.id | 字符串 | |



## <a name="events"></a>事件

由[TrackEvent()](app-insights-api-custom-events-metrics.md#track-event)生成的自定义事件。 


|路径|类型|备注|
|---|---|---|
| [0] 事件计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如 4 =&gt; 25%。 |
| [0] 的事件名称 | 字符串 | 事件的名称。  最大长度为 250。 |
| 事件 [0] url | 字符串 | |
| 事件 [0] urlData.base | 字符串 | |
| 事件 [0] urlData.host | 字符串 | |

## <a name="exceptions"></a>例外情况

报告[异常](app-insights-asp-net-exceptions.md)在服务器和浏览器。 


|路径|类型|备注|
|---|---|---|
| basicException [0] 程序集 | 字符串 | |
| basicException [0] 计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如 4 =&gt; 25%。 |
| basicException [0] exceptionGroup | 字符串 | |
| basicException [0] exceptionType | 字符串 | |字符串 | |
| basicException [0] failedUserCodeMethod | 字符串 | |
| basicException [0] failedUserCodeAssembly | 字符串 | |
| basicException [0] handledAt | 字符串 | |
| basicException [0] hasFullStack | 布尔值 | |
| basicException [0] id | 字符串 | |
| basicException [0] 方法 | 字符串 | |
| basicException [0] 消息 | 字符串 | 异常消息。 10 k 的最大长度。|
| basicException [0] outerExceptionMessage | 字符串 | |
| basicException [0] outerExceptionThrownAtAssembly | 字符串 | |
| basicException [0] outerExceptionThrownAtMethod | 字符串 | |
| basicException [0] outerExceptionType | 字符串 | |
| basicException [0] outerId | 字符串 | |
| basicException [0] [0] parsedStack 程序集 | 字符串 | |
| basicException [0] parsedStack [0] 文件名 | 字符串 | |
| basicException [0] parsedStack [0] 级别 | 整数 | |
| basicException [0] parsedStack [0] 行 | 整数 | |
| basicException [0] parsedStack [0] 方法 | 字符串 | |
| basicException [0] 堆栈 | 字符串 | 10 k 的最大长度|
| basicException [0] 类型名称 | 字符串 | |



## <a name="trace-messages"></a>跟踪消息

通过[TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)，并[记录适配器](app-insights-asp-net-trace-logs.md)发送。


|路径|类型|备注|
|---|---|---|
| 消息 [0] loggerName | 字符串 ||
| [0] 的消息参数 | 字符串 ||
| 消息 [0] 的原始 | 字符串 | 日志消息，10k 的最大长度。 |
| 消息 [0] severityLevel | 字符串 | |



## <a name="remote-dependency"></a>远程相关性

由 TrackDependency 发送。 用于报表的性能和用法的服务器中的[调用依赖项](app-insights-asp-net-dependencies.md)和 AJAX 调用在浏览器中。

|路径|类型|备注|
|---|---|---|
| remoteDependency [0] 异步 | 布尔值 | |
| remoteDependency [0] 基名称 | 字符串 |  |
| remoteDependency [0] 按钮 | 字符串 | 例如"家庭/索引" |
| remoteDependency [0] 计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如 4 =&gt; 25%。 |
| remoteDependency [0] dependencyTypeName | 字符串 | HTTP，SQL... |
| remoteDependency [0] durationMetric.value | 编号 | 从调用完成由依赖项的响应时间 |
| remoteDependency [0] id | 字符串 | |
| remoteDependency [0] 名称 | 字符串 | Url。 最大长度为 250。|
| remoteDependency [0] resultCode | 字符串 | 从 HTTP 依赖项 |
| remoteDependency [0] 成功 | 布尔值 | |
| remoteDependency [0] 类型 | 字符串 | Http，Sql... |
| remoteDependency [0] url | 字符串 |  最大长度 2000 |
| remoteDependency [0] urlData.base | 字符串 | 最大长度 2000 |
| remoteDependency [0] urlData.hashTag | 字符串 | |
| remoteDependency [0] urlData.host | 字符串 | 最大长度 200 |


## <a name="requests"></a>请求

由[TrackRequest](app-insights-api-custom-events-metrics.md#track-request)发送。 标准模块使用这到报表服务器的响应时间，在服务器上测量。 


|路径|类型|备注|
|---|---|---|
| [0] 的请求计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如︰ 4 =&gt; 25%。 |
| 请求 [0] durationMetric.value | 编号 | 为响应传入的请求的时间。 1e7 = = 1 |
| [0] 请求 id | 字符串 | 操作 id |
| [0] 请求名称 | 字符串 | 获取/开机自检 + 基 url。  最大长度为 250 |
| 请求 [0] responseCode | 整数 | 发送到客户端的 HTTP 响应 |
| [0] 请求成功 | 布尔值 | 默认值 = = (responseCode &lt; 400) |
| [0] 请求 url | 字符串 | 不包括主机 |
| 请求 [0] urlData.base | 字符串 | |
| 请求 [0] urlData.hashTag | 字符串 |  |
| 请求 [0] urlData.host | 字符串 | |


## <a name="page-view-performance"></a>页面视图模式下的性能

由浏览器发送。 测量的时间来处理用户启动请求显示完整 （不包括异步 AJAX 调用） 中的一页。

客户端操作系统和浏览器版本，将显示上下文的值。 


|路径|类型|备注|
|---|---|---|
| 客户端性能 [0] clientProcess.value | 整数 | 从接收到显示页的 HTML 的结束时间。 |
| 客户端性能 [0] 名称 | 字符串 | |
| 客户端性能 [0] networkConnection.value | 整数 | 所建立的网络连接的时间。 |
| 客户端性能 [0] receiveRequest.value | 整数 | 从将请求发送到在答复接收 HTML 结束的时间。 |
| 客户端性能 [0] sendRequest.value | 整数 | 从时间所发送的 HTTP 请求。 |
| 客户端性能 [0] total.value | 整数 | 从开始将请求发送到显示页面的时间。 |
| 客户端性能 [0] url | 字符串 | 此请求的 URL |
| 客户端性能 [0] urlData.base | 字符串 | |
| 客户端性能 [0] urlData.hashTag | 字符串 | |
| 客户端性能 [0] urlData.host | 字符串 | |
| 客户端性能 [0] urlData.protocol | 字符串 | |

## <a name="page-views"></a>网页视图

发送的 trackPageView() 或[stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|路径|类型|备注|
|---|---|---|
| 查看 [0] 的计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如 4 =&gt; 25%。 |
| 查看 [0] durationMetric.value | 整数 | 值 （可选） 设置在 trackPageView() 或 startTrackPage()-stopTrackPage()。 没有客户端性能值相同。 |
| [0] 视图名称 | 字符串 | 页标题。  最大长度为 250 |
| 查看 [0] 的 url | 字符串 | |
| 查看 [0] urlData.base | 字符串 | |
| 查看 [0] urlData.hashTag | 字符串 | |
| 查看 [0] urlData.host | 字符串 | |



## <a name="availability"></a>可用性

报告[可用性 web 测试](app-insights-monitor-web-app-availability.md)。

|路径|类型|备注|
|---|---|---|
| 可用性 [0] availabilityMetric.name | 字符串 | 可用性 |
| 可用性 [0] availabilityMetric.value | 编号 |1.0 或 0.0 |
| [0] 可用性计数 | 整数 | 100 / （[采样](app-insights-sampling.md)率）。 例如 4 =&gt; 25%。 |
| 可用性 [0] dataSizeMetric.name | 字符串 | |
| 可用性 [0] dataSizeMetric.value | 整数 | |
| 可用性 [0] durationMetric.name | 字符串 | |
| 可用性 [0] durationMetric.value | 编号 | 测试的持续时间。 1e7 = = 1 |
| [0] 可用性消息 | 字符串 | 故障诊断 |
| 可用性 [0] 结果 | 字符串 | 通过/失败 |
| 可用性 [0] runLocation | 字符串 | 地理来源的 http 要求 |
| [0] 可用性测试名称 | 字符串 | |
| 可用性 [0] testRunId | 字符串 | |
| 可用性 [0] testTimestamp | 字符串 | |




## <a name="metrics"></a>指标

由 TrackMetric() 生成。

在 context.custom.metrics[0 中找到跃点值]

例如︰

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>关于度量值

度量值，在指标的报告和其他地方，两个报告使用标准对象结构。 例如︰

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

目前虽然这可能会改变将来-所有值中报告从 SDK 的标准模块中， `count==1` ，仅`name`和`value`字段很有用。 他们将是不同的唯一情况是如果您编写您自己的 TrackMetric 调用中您设置其他参数。 

其他字段的目的是允许 SDK 来减少通信到门户中聚合的标准。 例如，可以发送每个跃点的报告之前平均几个连续的读数。 然后将计算的最小值、 最大值、 标准偏差和聚合值 （总和或平均值） 并设置为报告所表示的读数次数的计数。 

在上面的表中，我们忽略了很少使用字段计数、 最小值、 最大值、 标准偏差和 sampledValue。

而不是预聚合的度量标准，您可以使用[采样](app-insights-sampling.md)，如果需要减少量的遥测。


### <a name="durations"></a>持续时间

除非另行说明，否则工期将表示以十分之一一微秒，以便 10000000.0 意味着 1 秒。



## <a name="see-also"></a>请参见

* [应用程序的见解](app-insights-overview.md) 
* [连续的导出](app-insights-export-telemetry.md)
* [代码示例](app-insights-export-telemetry.md#code-samples)


