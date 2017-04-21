<properties
   pageTitle="故障模式分析 |Microsoft Azure"
   description="用于执行的基于 Azure 的云解决方案的故障模式分析的准则。"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure 复原指导︰ 故障模式分析

故障模式分析 (FMA) 是构建过程进行复原到系统中，通过在系统中标识可能的故障点。 FMA 应体系结构和设计阶段的一部分，以便您可以从头系统中生成故障恢复。

这里是开展 FMA 的一般过程︰ 

1. 标识系统中的所有组件。 包括外部从属项，如作为标识提供商、 第三方服务等。   

2. 对于每个组件，确定可能发生的潜在故障。 单个组件可能具有多个故障模式。 例如，您应考虑读取的故障和分开，写失败，因为影响和可能的缓解将是不同。

3. 根据其总体风险每种故障模式进行评价。 请考虑下列因素︰  

    - 什么是失败的可能性。 是比较常见？ Extrememly 很少？ 您不需要精确的数字;目的是帮助排列优先级。

    - 对应用程序的可用性、 数据丢失、 金钱成本和业务中断的影响是什么？ 

4. 对于每个故障模式，确定如何将响应应用程序并将其恢复。 考虑成本和应用程序的复杂性的折衷方案。   

FMA 过程的起点，为这篇文章包含目录中潜在的故障模式及他们缓解。 目录按技术或 Azure 服务，再加上应用程序级设计的一般类别。 该目录并不详尽，但涵盖了许多核心 Azure 服务。 


## <a name="app-service"></a>应用程序服务

### <a name="app-service-app-shuts-down"></a>应用程序服务的应用程序将关闭。

**检测**。 可能的原因︰

- 预期的关机
    
    - 运算符将关闭该应用程序。例如，使用 Azure 的门户。

    - 因为空闲，则该应用程序已卸载。 (仅当`Always On`设置被禁用。)

- 意外的关机

    - 应用程序崩溃。

    - 应用程序服务 VM 实例变为不可用。


Application_End 日志将捕获应用程序域关闭 （软进程崩溃），捕获应用程序域关闭的唯一办法。

**恢复**

- 当需要关闭时，使用应用程序的关闭事件正常关闭。 例如，在 ASP.NET 中，使用`Application_End`方法。

- 如果在空闲时，应用程序被卸载，则自动重新启动在下一个请求。 但是，将会"冷启动"成本。 

- 若要阻止此应用程序被卸载在空闲时，启用`Always On`在 web 应用程序中设置。 [在 Azure 应用程序服务的配置 web 应用程序]，请参阅[app-service-configure]。

- 若要防止操作员关闭应用程序，设置与资源锁`ReadOnly`级别。 [锁定资源使用 Azure 资源管理器]，请参阅[rm-locks]。

- 如果应用程序崩溃或应用程序服务虚拟机不可用时，应用程序服务将自动重新启动该应用程序。 


**诊断程序**。 应用程序日志和 web 服务器日志。 [启用 web 应用程序在 Azure 应用程序服务的诊断日志记录]，请参阅[app-service-logging]。


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>特定用户反复做出错误的请求，或重载系统。 

**检测**。 验证用户和应用程序日志中包括用户 ID。

**恢复**

- 使用[Azure API 管理][api-management]为限制来自用户的请求。 请参见[高级的申请使用 Azure API 管理限制][api-management-throttling]

- 阻止用户。

**诊断程序**。 记录所有的身份验证请求。


### <a name="a-bad-update-was-deployed"></a>坏的更新已部署。

**检测**。 监视应用程序运行状况，通过 Azure 门户 (请参阅[显示器 Azure web 应用程序性能][app-insights-web-apps]) 或实现[健康终结点监视模式][health-endpoint-monitoring-pattern]。

**故障恢复**。 使用多个[部署插槽][app-service-slots]和回滚到上次已知的正确部署。 有关详细信息，请参见[基本的 web 应用程序的参考体系结构][ra-web-apps-basic]。


## <a name="azure-active-directory"></a>Azure 的活动目录

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID 连接 (OIDC) 身份验证将失败。

**检测**。 可能的故障模式包括︰

1. Azure AD 不可用，或者由于网络问题而无法访问。 重定向到身份验证终结点失败，并 OIDC 中间件将引发异常。
2. Azure AD 租户不存在。 重定向到身份验证终结点返回的 HTTP 错误代码，并 OIDC 中间件将引发异常。
3. 用户不能进行身份验证。 没有检测的策略是必要的;Azure AD 处理登录失败。

**恢复**

1. 捕捉从中间件未经处理的异常。
2. 处理`AuthenticationFailed`事件。
3. 将用户重定向到错误页。
4. 用户重试次数。


## <a name="azure-search"></a>Azure 搜索

### <a name="writing-data-to-azure-search-fails"></a>将数据写入 Azure 搜索将失败。

**检测**。 捕捉`Microsoft.Rest.Azure.CloudException`错误。

**恢复**

[搜索.NET SDK] [search-sdk]瞬时性故障后自动重试。 由客户端 SDK 所引发的任何异常都应视为非瞬态错误。

默认重试策略使用指数后关闭。 若要使用不同的重试策略，调用`SetRetryPolicy`在`SearchIndexClient`或`SearchServiceClient`类。 有关详细信息，请参阅[自动重试][auto-rest-client-retry]。

**诊断程序**。 使用[搜索流量分析][search-analytics]。


### <a name="reading-data-from-azure-search-fails"></a>读取数据从 Azure 搜索将失败。

**检测**。 捕捉`Microsoft.Rest.Azure.CloudException`错误。

**恢复**

[搜索.NET SDK] [search-sdk]瞬时性故障后自动重试。 由客户端 SDK 所引发的任何异常都应视为非瞬态错误。

默认重试策略使用指数后关闭。 若要使用不同的重试策略，调用`SetRetryPolicy`在`SearchIndexClient`或`SearchServiceClient`类。 有关详细信息，请参阅[自动重试][auto-rest-client-retry]。

**诊断程序**。 使用[搜索流量分析][search-analytics]。



## <a name="cassandra"></a>卡桑德拉


### <a name="reading-or-writing-to-a-node-fails"></a>读取或写入到某个节点出现故障。

**检测**。 捕获异常。 对于.NET 客户端，这通常会是`System.Web.HttpException`。 其他客户端可能有其他的异常类型。  有关详细信息，请参阅[卡桑德拉的错误处理得当](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right)。

**恢复**

- [卡桑德拉客户端](https://wiki.apache.org/cassandra/ClientOptions)每个都有自己重试策略和功能。 有关详细信息，请参阅[卡桑德拉的错误处理得当][cassandra-error-handling]。
- 注意机架的部署中，使用数据节点故障域分布。
- 本地仲裁的一致性与部署到多个区域。 如果出现非瞬态故障，故障切换到另一个区域。

**诊断程序**。 应用程序日志


## <a name="cloud-service"></a>云服务

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web 或辅助角色意外都被关闭。

**检测**。 [RoleEnvironment.Stopping] [RoleEnvironment.Stopping]激发事件。

**故障恢复**。 重写[RoleEntryPoint.OnStop] [RoleEntryPoint.OnStop]方法以妥善清理。 有关详细信息，请参阅[右侧往处理 Azure OnStop 事件][ onstop-events] （网络日志）。


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>DocumentDB 从读取数据失败。

**检测**。 捕捉`System.Net.Http.HttpRequestException`或`Microsoft.Azure.Documents.DocumentClientException`。 

**恢复**

- SDK 自动重试失败的尝试。 若要设置重试次数和最长等待时间的数量，配置`ConnectionPolicy.RetryOptions`。 客户端引发的异常以外的重试策略或者不是暂时性错误。 

- 如果 DocumentDB 限制客户端，它将返回 HTTP 429 错误。 签入的状态代码`DocumentClientException`。 如果要以一致的方式获取错误 429，可考虑增加 DocumentDB 集合的吞吐量值。

- 跨两个或多个区域复制的 DocumentDB 数据库。 所有副本都是可读的。 使用客户端 Sdk，指定`PreferredLocations`参数。 这是 Azure 区域的排序的列表。 所有读取将被都发送到列表中的第一个可用区域。 如果请求失败，则客户端将尝试在列表中，按顺序的其他地区。 有关详细信息，请参见[开发与多区域 DocumentDB 帐户][docdb-multi-region]。

**诊断程序**。 日志客户端上的所有错误。


### <a name="writing-data-to-documentdb-fails"></a>将数据写入到 DocumentDB 失败。

**检测**。 捕捉`System.Net.Http.HttpRequestException`或`Microsoft.Azure.Documents.DocumentClientException`。 

**恢复**

- SDK 自动重试失败的尝试。 若要设置重试次数和最长等待时间的数量，配置`ConnectionPolicy.RetryOptions`。 客户端引发的异常以外的重试策略或者不是暂时性错误。 

- 如果 DocumentDB 限制客户端，它将返回 HTTP 429 错误。 签入的状态代码`DocumentClientException`。 如果要以一致的方式获取错误 429，可考虑增加 DocumentDB 集合的吞吐量值。

- 跨两个或多个区域复制的 DocumentDB 数据库。 如果出现故障的主要区域，将提升另一个区域来编写。 您也可以手动触发故障转移。 SDK 进行自动发现和路由，因此应用程序代码将继续运行在故障转移之后。 在故障转移期间 （通常是分钟），写操作必须等待时间较长，SDK 找到新写入的区域。 有关详细信息，请参见[开发与多区域 DocumentDB 帐户][docdb-multi-region]。

- 作为后备，保留文档的备份队列和稍后处理该队列。

**诊断程序**。 日志客户端上的所有错误。


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Elasticsearch 从读取数据失败。

**检测**。 捕获特定的[Elasticsearch 客户端]的相应异常[elasticsearch-client]正在使用。 

**恢复**

- 使用重试机制。 每个客户端都有自己的重试策略。 

- 部署多个 Elasticsearch 节点并使用复制以实现高可用性。

有关详细信息，请参阅[在 Azure 上运行 Elasticsearch][elasticsearch-azure]。

**诊断程序**。 可以为 Elasticsearch，使用监视工具，也可以与负载记录在客户端上的所有错误。 请参阅监视部分[在 Azure 上运行 Elasticsearch][elasticsearch-azure]。


### <a name="writing-data-to-elasticsearch-fails"></a>将数据写入到 Elasticsearch 失败。

**检测**。 捕获特定的[Elasticsearch 客户端]的相应异常[elasticsearch-client]正在使用。  

**恢复**

- 使用重试机制。 每个客户端都有自己的重试策略。 
 
- 如果应用程序能够容忍降低的一致性级别，请考虑编写`write_consistency`设置为`quorum`。

有关详细信息，请参阅[在 Azure 上运行 Elasticsearch][elasticsearch-azure]。

**诊断程序**。 可以为 Elasticsearch，使用监视工具，也可以与负载记录在客户端上的所有错误。 请参阅监视部分[在 Azure 上运行 Elasticsearch][elasticsearch-azure]。


## <a name="queue-storage"></a>队列存储

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>以一致的方式，将消息写入队列 Azure 存储失败。

**检测**。 *N*重试次数后，写操作仍将失败。

**恢复**

- 将数据存储在本地缓存中，并将转发到存储的写入操作以后，当该服务可用时。
- 创建辅助队列，并写入到该队列，如果主队列不可用。

**诊断程序**。 使用[存储标准][storage-metrics]。


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>应用程序无法处理队列中的特定消息。 

**检测**。 特定于应用程序。 例如，该消息包含无效数据，或由于某种原因失败的业务逻辑。 

**恢复**

将邮件移动到单独的队列。 运行独立的进程以检查该队列中的邮件。

请考虑使用 Azure 服务总线消息队列，它提供了一个[死信队列][sb-dead-letter-queue]为此目的的功能。

注意︰ 如果您对 WebJobs 使用存储队列，WebJobs SDK 提供了内置的有害消息处理。 了解[如何使用 WebJobs sdk 的 Azure 队列存储][sb-poison-message]。

**诊断程序**。 使用应用程序的日志记录。


## <a name="redis-cache"></a>Redis 高速缓存

### <a name="reading-from-the-cache-fails"></a>从缓存中的读取失败。

**检测**。 捕捉`StackExchange.Redis.RedisConnectionException`。

**恢复**

1. 瞬间失败重试。 Azure Redis 缓存支持内置重试，请参阅[Redis 缓存重试准则]通过[redis-retry]。
2. 缓存未命中，被视为非瞬态失败并回退到原始数据源。

**诊断程序**。 使用[Redis 缓存诊断][redis-monitor]。


### <a name="writing-to-the-cache-fails"></a>写入缓存失败。

**检测**。 捕捉`StackExchange.Redis.RedisConnectionException`。

**恢复**

1. 瞬间失败重试。 Azure Redis 缓存支持内置重试，请参阅[Redis 缓存重试准则]通过[redis-retry]。
2. 如果非瞬态错误，忽略它，并让后面写入缓存的其他事务。

**诊断程序**。 使用[Redis 缓存诊断][redis-monitor]。


## <a name="sql-database"></a>SQL 数据库

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>无法连接到主区域中的数据库。

**检测**。 连接失败。

**恢复**

前提条件︰ 必须将数据库配置为活动的各地区复制。 请参阅[SQL 数据库活动地区-复制][sql-db-replication]。

- 对于查询，从一个辅助副本中进行读取。 
- 对插入和更新，手动故障切换到辅助副本。 请参阅[启动 SQL Azure 数据库为计划内或计划外故障切换][sql-db-failover]。

复制副本使用不同的连接字符串，因此您需要更新您的应用程序中的连接字符串。


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>客户端运行完连接池中的连接。

**检测**。 捕捉`System.InvalidOperationException`错误。 

**恢复**

- 重试该操作。
- 为缓解这一计划，这样一个用例不能占据市场的所有连接隔离每个用例，用于连接池。
- 提高最大连接池。

**诊断程序**。 应用程序日志。


### <a name="database-connection-limit-is-reached"></a>已达到数据库连接限制。 

**检测**。 Azure 的 SQL 数据库限制并发工作人员、 登录和会话的数量。 限制取决于服务层。 有关详细信息，请参阅[SQL Azure 数据库资源限制][sql-db-limits]。

要检测这些错误，请捕捉`System.Data.SqlClient.SqlException`和检查的值`SqlException.Number`的 SQL 错误代码。 相关的错误代码的列表，请参阅[SQL 数据库客户端应用程序的 SQL 错误代码︰ 数据库连接错误和其他问题][sql-db-errors]。

**故障恢复**。 这些错误视为暂时的以便重试可能解决该问题。 如果持续遇到这些错误，请考虑扩展数据库。

**诊断程序**。 [Sys.event_log] -[sys.event_log]查询返回成功的数据库连接，连接故障和死锁。

- 创建[预警规则][azure-alerts]的失败的连接。

- 启用[SQL 数据库的审核][sql-db-audit]并检查失败的登录。


## <a name="service-bus-messaging"></a>服务总线消息

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>从服务总线队列中读取一条消息将会失败。

**检测**。 捕获客户端 SDK 中的异常。 服务总线的异常的基类是[MessagingException][sb-messagingexception-class]。 如果错误是瞬态的`IsTransient`属性为 true。 

有关详细信息，请参阅[服务总线消息传递异常][sb-messaging-exceptions]。

**恢复**

1. 瞬间失败重试。 请参阅[服务总线重试准则][sb-retry]。

2. 无法发送给所有接收者的邮件放在*死信队列*中。 使用此队列无法接收的邮件，请参阅。 没有任何自动清理的死信队列。 邮件那里保留，直到您显式检索它们。 [服务总线概述死信队列]，请参阅[sb-dead-letter-queue]。


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>服务总线中写入消息队列失败。

**检测**。 捕获客户端 SDK 中的异常。 服务总线的异常的基类是[MessagingException][sb-messagingexception-class]。 如果错误是瞬态的`IsTransient`属性为 true。 

有关详细信息，请参阅[服务总线消息传递异常][sb-messaging-exceptions]。

**恢复**

1. 服务总线客户端自动重试后瞬态错误。 默认情况下，它使用指数后关闭。 之后的最大重试计数或最大超时时间，客户端将引发异常。 有关详细信息，请参阅[服务总线重试准则][sb-retry]。

2. 如果超出队列配额时，客户端将引发[QuotaExceededException][QuotaExceededException]。 异常消息提供了详细信息。 重试之前, 释放某些消息队列中的，考虑使用断路器模式为避免继续重试次数超出了配额。 另外，请确保[BrokeredMessage.TimeToLive]属性未设置得太高。 

3. 区域内恢复能力可以提高使用[分区的队列或主题][sb-partition]。 非分区队列或主题分配给一个邮件存储区。 如果此邮件存储区不可用时，该队列或主题的所有操作将都失败。 在多个邮件存储分区分区的队列或主题。 

4. 额外的恢复能力，在不同地区，创建两个服务总线命名空间和复制邮件。 您可以使用活动的复制或被动复制。

    - 活动的复制︰ 客户端向这两个队列发送每一封邮件。 在两个队列上侦听接收器。 标记邮件的唯一标识符，以便客户端可以放弃重复的邮件。

    - 被动复制︰ 客户机将消息发送到一个队列。 如果没有出现错误，客户端将回退到另一个队列。 在两个队列上侦听接收器。 这种方法可以减少重复发送的邮件数。 但是，接收端仍必须处理重复的邮件。

    有关详细信息，请参见[GeoReplication 示例][ sb-georeplication-sample] [针对服务总线中断和灾难的绝缘应用程序的最佳做法]和[sb-outages]。


### <a name="duplicate-message"></a>重复的消息。

**检测**。 检查`MessageId`和`DeliveryCount`消息的属性。

**恢复**

- 如有可能，设计邮件处理操作是幂等的。 否则，存储已处理和检查处理消息之前的 ID 的消息的消息 Id。

- 通过创建队列启用重复检测功能，`RequiresDuplicateDetection`设置为 true。 使用此设置，服务总线自动删除任何邮件发送具有相同的`MessageId`作为上一封邮件。  请注意以下事项︰

    -  此设置可防止重复的邮件被放入队列中。 它不能防止多次处理相同的消息接收器。

    - 重复检测具有时间窗口。 如果超出了此窗口发送重复，将不会被检测到。

**诊断程序**。 记录重复的消息。


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>应用程序无法处理队列中的特定消息。 

**检测**。 特定于应用程序。 例如，该消息包含无效数据，或由于某种原因失败的业务逻辑。 

**恢复**

有两种故障模式，要考虑。 

- 接收器检测到故障。 在这种情况下，将邮件移动到死信队列中。 之后，运行一个单独的进程来检查死信队列中的邮件。

- 接收方在处理邮件失败&mdash;为例，由于未经处理的异常。 若要处理这种情况下，使用`PeekLock`模式。 在此模式下锁定到期后，如果有可用消息到其他接收器。 如果邮件超过最大传递计数或生存时间，邮件被自动移到死信队列中。

有关详细信息，请参阅[服务总线概述死信队列][sb-dead-letter-queue]。

**诊断程序**。 每当应用程序将消息移动到死信队列，将事件写入应用程序日志。


## <a name="service-fabric"></a>服务结构

### <a name="a-request-to-a-service-fails"></a>对服务的请求将失败。

**检测**。 该服务返回一个错误。

**恢复**

- 再次查找代理 (`ServiceProxy`或`ActorProxy`)，然后调用服务/演员再次。 

- **有状态的服务**。 在事务中包装上可靠的集合操作。 如果没有错误，事务将回滚。 处理该请求，如果从队列中提取将再次。 
 
- **无状态的服务**。 如果服务仍然存在到外部存储的数据，所有的操作必须是幂等的。


**诊断程序**。 应用程序日志

### <a name="service-fabric-node-is-shut-down"></a>服务结构节点已被关闭。

**检测**。 取消标记传递到该服务的`RunAsync`方法。 在关闭该节点之前，服务结构取消任务。

**故障恢复**。 使用取消标记来检测关机。 当服务结构请求取消时，完成所有的工作并退出 `RunAsync`尽可能快地。

**诊断程序**。 应用程序日志


## <a name="storage"></a>存储

### <a name="writing-data-to-azure-storage-fails"></a>写入数据到 Azure 存储失败

**检测**。 在编写时，客户端会收到错误。

**恢复**

1. 重试操作，从瞬时性故障中进行恢复。 [重试策略][Storage.RetryPolicies]客户端 SDK 这自动处理。
2. 实现了断路器模式，以避免巨大的存储。
3. 如果 N 重试失败，则执行温和回退。 例如︰

    - 将数据存储在本地缓存中，并将转发到存储的写入操作以后，当该服务可用时。
    - 如果写操作在事务范围内，补偿事务。

**诊断程序**。 使用[存储标准][storage-metrics]。


### <a name="reading-data-from-azure-storage-fails"></a>从 Azure 存储中读取数据失败。

**检测**。 读取数据时，客户端会收到错误。

**恢复**

1. 重试操作，从瞬时性故障中进行恢复。 [重试策略][Storage.RetryPolicies]客户端 SDK 这自动处理。
2. RA GRS 存储如果读取主终结点失败，尝试读取辅助的终结点。 客户端 SDK 可以此自动处理。 请参阅[Azure 存储复制][storage-replication]。
3. 如果*N*重试失败，执行回退操作性能平稳降低。 例如，如果不能从存储中检索产品图像，显示泛型占位符图像。

**诊断程序**。 使用[存储标准][storage-metrics]。


## <a name="virtual-machine"></a>虚拟机

### <a name="connection-to-a-backend-vm-fails"></a>连接到后端虚拟机失败。

**检测**。 网络连接错误。

**恢复**

- 部署在可用性组中，负载平衡器后面至少两个后端虚拟机。

- 如果连接错误是瞬态的有时 TCP 将成功重试发送邮件。 

- 在应用程序中实现重试策略。 

- 对于永久或非瞬态错误，实现[断路器][circuit-breaker]模式。

- 如果调用 VM 超过极限网络出口，出站队列将填满。 如果出站队列已满一致时，应考虑横向扩展。 

**诊断程序**。 记录在服务边界的事件。

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM 实例变为不可用或不正常。

**检测**。 配置负载平衡器[运行状况探测器][lb-probe]的信号是否正常运行的 VM 实例。 该探测器应该检查是否都正确响应的关键功能。 

**故障恢复**。 对于每个应用程序层，将多个 VM 实例放入同一可用性组，并将负载平衡器在虚拟机中。 如果健康探测失败，负载平衡器将停止对不健康的实例发送新的连接。

**诊断程序**。 -使用负载平衡器[日志分析][lb-monitor]。
- 配置监视系统，监视所有运行状况监视的终结点。


### <a name="operator-accidentally-shuts-down-a-vm"></a>运算符不小心关闭虚拟机。

**检测**。 N/A

**故障恢复**。 设置资源锁与`ReadOnly`级别。 [锁定资源使用 Azure 资源管理器]，请参阅[rm-locks]。

**诊断程序**。 使用[Azure 活动日志][azure-activity-logs]。


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>连续作业停止 SCM 主机处于空闲状态时运行。

**检测**。 将取消标记传递到 WebJob 函数。 有关详细信息，请参阅[正常关机][web-jobs-shutdown]。 

**故障恢复**。 启用`Always On`在 web 应用程序中设置。 有关详细信息，请参阅[运行后台任务与 WebJobs][web-jobs]。


## <a name="application-design"></a>应用程序设计

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>应用程序不能处理传入的请求中出现高峰。

**检测**。 取决于应用程序。 典型症状︰

- 该网站开始返回 HTTP 5xx 错误代码。
- 依赖的服务，例如数据库或存储，开始限制的请求。 查找 HTTP 错误，例如 HTTP 429 （太多个请求），具体取决于该服务。
- HTTP 队列长度增长。

**恢复**

- 向外扩展以处理增加的负载。 

- 减少故障，为避免破坏整个应用程序的级联故障。 缓解策略包括︰

    - 实现[调节模式][throttling-pattern]以避免巨大的后端系统。
    - 使用[基于队列的负载调配][queue-based-load-leveling]缓冲请求并处理它们以适当的速度。
    - 优先考虑某些客户端。 例如，如果应用程序有免费和付费的层，限制客户在自由层上，但不是支付的客户。 请参阅[优先级队列模式][priority-queue-pattern]。

**诊断程序**。 使用[应用程序服务的诊断日志记录][app-service-logging]。 使用[Azure 日志分析]服务[azure-log-analytics]，[应用程序的见解][app-insights]，或[新的 Relic] [new-relic]可以帮助理解诊断日志。


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>其中一个工作流或分布式的事务中的操作失败。

**检测**。 *N*重试次数后，它仍然无法正常工作。

**恢复**

- 为缓解这一计划，实施[计划代理主管][scheduler-agent-supervisor]要管理整个工作流模式。 
- 不重试超时。 没有针对此错误的低成功率。 
- 队列的工作，以便稍后重试。

**诊断程序**。 记录所有操作 （成功的和失败的），包括补偿操作。 使用相关 Id，以便可以跟踪同一事务中的所有操作。


### <a name="a-call-to-a-remote-service-fails"></a>对远程服务的调用将失败。

**检测**。 HTTP 错误代码。

**恢复**

1. 瞬间失败重试。 
2. *N*如果呼叫失败之后尝试，执行回退操作。 （应用程序特定。）
3. 实现[断路器模式][circuit-breaker]以避免级联故障。 

**诊断程序**。 记录所有远程调用失败。


## <a name="next-steps"></a>下一步行动

FMA 过程有关的详细信息，请参阅[特意设计的云服务的复原能力][ resilience-by-design-pdf] （PDF 下载）。

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

