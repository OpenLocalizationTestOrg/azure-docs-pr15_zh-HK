有一些限制的指标和每个应用程序事件的数量 (即，每个检测密钥)。 

限制取决于[定价层](https://azure.microsoft.com/pricing/details/application-insights/)供您选择。

**资源** | **默认限制** | **最大限制**
-------- | ------------- | -------------
<sup>1、 2</sup>每月的会话数据点 | 无限制 | 
总数据点，每月的请求、 事件、 依赖项、 跟踪、 异常和页视图 | 5 万 | 50 万<sup>3</sup>
[跟踪和日志](../articles/application-insights/app-insights-search-diagnostic-logs.md)数据速率 | 200 的 dp/s | 500 dp/s
[异常](../articles/application-insights/app-insights-asp-net-exceptions.md)数据速率 | 50 dp/s | 50 dp/s
总数据速率请求、 事件、 相关性及页面视图遥测 | 200 的 dp/s | 500 dp/s
[搜索](../articles/application-insights/app-insights-diagnostic-search.md)和[分析](../articles/application-insights/app-insights-analytics.md)的原始数据保留 | 7 天
[测量数据资源管理器中](../articles/application-insights/app-insights-metrics-explorer.md)的聚合的数据保留 | 90 天
[属性](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties)名称计数 | 100 |
属性名称的长度 | 150 | 
属性值长度 | 8192 | 
跟踪和异常消息长度 | 10000 |
[指标](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties)名称计数 | 100 |
指标名称长度 |  150 | 
[可用性测试](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup>数据点是单个度量值或事件时，附加的属性和度量值。

<sup>2</sup> A 会话数据点记录的开始或结束一个会话，并记录用户的身份。

<sup>3</sup>您可以购买更多容量超过 50 万。
 
[有关定价和应用程序的见解中的配额](../articles/application-insights/app-insights-pricing.md)
