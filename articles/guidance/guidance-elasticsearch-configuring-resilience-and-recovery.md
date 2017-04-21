<properties
   pageTitle="在 Azure 上 Elasticsearch 上配置的复原和恢复"
   description="与 Elasticsearch 有关的复原和恢复注意事项。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>在 Azure 上 Elasticsearch 上配置的复原和恢复

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

本文是[一系列的一部分](guidance-elasticsearch.md)。 

Elasticsearch 的主要功能是它提供了用于在节点故障和/或网络分区事件发生时的自我恢复能力的支持。 复制是最明显的方法，在其中可以提高恢复能力的任何群集，启用 Elasticsearch，以确保在一个节点应该变得无法访问的情况下，任何数据项目的多个副本是位于不同节点上可用。 如果某个节点暂时不可用，可以服务于其他节点包含丢失的节点中的数据的副本丢失的数据，直到问题解决。 在出现更长的时间期限问题时，丢失的节点可以被替换为一个新和 Elasticsearch 可以将数据还原到新的节点，从复制副本。

这里我们总结了可用于 Elasticsearch 时寄宿在 Azure，并说明应该考虑尽量减少数据丢失和扩展的数据恢复时间的几率 Elasticsearch 群集的某些重要方面的复原和恢复选项。

本文还演示了一些执行以显示 Elasticsearch 群集中，对不同类型的故障的影响和恢复系统的响应方式的示例测试。

Elasticsearch 群集使用副本以维护可用性并提高读取的性能。 复制副本应该存储在不同的虚拟机从它们复制主 shards。 目的是，如果虚拟机承载的某个数据节点发生故障或变为不可用，系统可以继续正常使用保存副本的虚拟机。

## <a name="using-dedicated-master-nodes"></a>使用专用的主节点

在 Elasticsearch 群集的一个节点选择作为主节点上。 此节点的目的是执行群集管理操作，如︰

- 出现故障的节点的检测并切换到副本。

- 重新定位 shards 平衡节点负载。

- 当一个节点已联机，则恢复 shards。

应该考虑在重要群集，使用专用的主节点，并确保有 3 个专用的节点的唯一职责就是为主。 此配置可减少这些节点必须执行资源密集型工作 （它们不存储数据，或者处理查询），并帮助提高群集的稳定性。 只会选择这些节点中的一个，但其他人将包含系统状态的副本，可以接管应该选举的主会失败。

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>控制与 Azure – 更新域和故障域的高可用性 

不同的虚拟机可以共享相同的物理硬件。 在 Azure 数据中心，一个机架上可以驻留多个虚拟机，并且所有这些虚拟机共享通用电源源和网络交换机。 单个机架级故障可能因此会影响多个虚拟机。 Azure 使用故障域的概念，尝试并传播这种风险。 容错域大致对应于一组共享同一个机架上的虚拟机。 确保机架级故障使节点不崩溃并同时按其复制副本的所有节点，您应该确保虚拟机遍布故障域。

同样，虚拟机可以服用[Azure 结构控制器](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/)执行计划内的维护和操作系统升级。 Azure 分配虚拟机来更新域。 计划内的维护事件发生时，一次单个更新域中唯一的虚拟机都受影响。 其他更新域中的虚拟机保持运行，直到被更新的更新域中的虚拟机联机。 因此，您还需要确保尽可能 Vm 托管节点和其副本属于不同的更新域。

> [AZURE.NOTE] 有关故障域和更新域的详细信息，请参阅[管理虚拟机的可用性][]。

明确不能分配到特定更新域和故障域的 VM。 这种分配受 Azure 时创建的虚拟机。 但是，您可以指定应将虚拟机创建为可用性组的一部分。 在同一可用性组中的虚拟机将分布在更新域和故障域中。 如果您手动创建的虚拟机，Azure 创建具有两个故障域和五个更新域设置每个可用性。 虚拟机分配给这些故障域和更新域，循环往返，随着更多虚拟机调配，如下︰ 

| 虚拟机 | 容错域 | 更新域 |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] 如果您创建使用 Azure 资源管理器的虚拟机，每个可用性组分配的可达 3 故障域和 20 更新域。 这是使用资源管理器一个令人信服的理由。

一般情况下，放置在相同的可用性组中，可以实现同一目的的所有 Vm，但为执行不同功能的虚拟机创建不同的可用性设置。 与 Elasticsearch，这意味着您应该考虑创建至少单独可用性设置为︰

- 虚拟机承载数据节点。
- Vm 托管客户端节点 （如果您使用它们）。
- 主节点承载的虚拟机。

此外，应确保群集中的每个节点都注意更新域和它所属的故障域。 此信息可以帮助确保 Elasticsearch 不会创建 shards 和其副本中的相同的错误，并不更新域，将 shard 和其复制副本从一次停机的可能性降到最低。 您可以配置要通过配置[shard 分配意识](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness)镜像群集的硬件分布的 Elasticsearch 节点。 例如，您可以定义一对自定义节点属性名为*faultDomain*和*updateDomain*在 elasticsearch.yml 文件中，如下︰

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

在这种情况下，属性设置使用的值保存在*\${FAULTDOMAIN}*和*\${UPDATEDOMAIN}*环境变量在启动 Elasticsearch 时。 您还需要将以下项添加到 Elasticsearch.yml 文件，以指示*faultDomain*和*updateDomain*是分配意识的特性，并指定这些属性的可接受值的集︰

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

可用于 shard 分配意识[shard 分配筛选](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering)结合显式指定哪些节点可以承载任何给定索引的 shards。

如果需要扩展到故障域和可用性集中更新域的数量，可以更好的可用性设置中创建虚拟机。 但是，您需要了解的不同可用性组中的节点可以停机维护同时。 请确保每个 shard，至少它的某个副本都包含在同一可用性组。

> [AZURE.NOTE] 目前每个可用性集 100 Vm 的限制。 有关详细信息，请参阅[Azure 订阅和服务限制、 配额和限制](../azure-subscription-service-limits.md)。

### <a name="backup-and-restore"></a>备份和恢复

使用副本不提供完整的保护，从灾难性故障 （如意外删除整个群集）。 您应该确保定期备份群集中的数据，并且能够从这些备份中还原系统的经过验证和测试策略。

使用 Elasticsearch[快照还原 Api](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) ︰ 弹性不封闭这些。 >> 来备份和还原索引。 可以将快照保存到共享文件系统。 此外，插件不可用时，可以写入快照，Hadoop 分布式文件系统 (HDFS) （ [HDFS 插件](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)） 或到 Azure 存储 （ [Azure 的插件](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)）。

选择快照存储机制时，请考虑以下几点︰

- 您可以使用[Azure 文件存储](https://azure.microsoft.com/services/storage/files/)实现从所有节点可以访问的共享文件系统。

- 如果您正在运行 Elasticsearch 结合 Hadoop 只能使用 HDFS 插件。

- HDFS 插件需要禁用 Java 安全管理器的 Java 虚拟机 (JVM) 的 Elasticsearch 实例中运行。

- HDFS 插件支持兼容 HDFS 的任何文件系统，提供正确的 Hadoop 配置可与 Elasticsearch。

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>处理节点之间的连接中断

间歇性网络故障虚拟机重新启动之后在数据中心的日常维护和其他类似事件可能会导致节点暂时不可。 在这些情况下，其中事件很可能是短期的重新平衡 shards 的系统开销将发生两次在快速连续 （一次当检测到故障并再次当节点变得可见到主服务器） 可以成为巨大的系统开销，对性能的影响。 可以防止临时节点不可访问引起的主机来重新平衡群集，通过设置*延迟\_超时*的索引，或者为所有索引的属性。 下面的示例设置为 5 分钟的延迟︰

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

有关详细信息，请参见[延迟分配一个节点离开时](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html)。

在网络中，很容易导致中断，您还可以修改检测时另一个节点将不再可访问主机配置的参数。 这些参数是提供 Elasticsearch， [zen 发现](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen)模块的一部分，您可以在 Elasticsearch.yml 文件中设置它们。 例如， *discovery.zen.fd.ping.retries*参数指定一个主节点将尝试 ping 另一个节点决定之前群集中的它已失败的次数。 此参数的默认值为 3，但您可以对其进行修改，如下所示︰

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>控制恢复

在出现故障后恢复到节点的连接后，在该节点上的所有 shards 将都需要进行恢复，以使其最新。 默认情况下，Elasticsearch 将恢复 shards 顺序如下︰

- 通过反向索引的创建日期。 更新索引之前旧索引恢复。

- 通过反向索引名称。 将第一个还原具有字母大于其他人的名称的索引。

如果某些索引是比其他人更重要，但不是符合这些条件，可以通过设置*index.priority*属性重写优先级的索引。 具有较高的值，该属性的索引将会恢复之前的值较低的索引︰

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

有关详细信息，请参阅[索引恢复优先顺序](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization)。

您可以监视使用的一个或多个索引的恢复进程*\_恢复*API:

```http
GET /high_priority_index/_recovery?pretty=true
```

有关详细信息，请参阅[索引恢复](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery)。

> [AZURE.NOTE] 有需要恢复的 shards 的群集将有状态为*黄色*，表示并不是所有的 shards 是目前。 所有 shards 可用时，群集状态应恢复为*绿色*。 群集的状态为*红色*指示一个或多个 shards 物理丢失，则可能需要从备份中还原数据。

## <a name="preventing-split-brain"></a>防止大脑分裂 

如果节点间的连接失败，可能会出现裂脑。 主节点无法连接到群集的一部分时，选举会发生在保持访问的网络段，另一个节点将成为主节点。 在错误配置群集中，就可能具有不同的主控形状，从而导致数据不一致或损坏群集的每个部分。 这种现象称为*大脑分裂*。

可以通过配置来减少裂脑的几率*最小\_主\_节点*发现模块，elasticsearch.yml 文件中的属性。 此属性指定多少个节点必须可用于启用主的选举。 下面的示例将此属性的值设置为 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

此值应设置为最低大部分能够满足主机角色的节点数。 例如，如果您的群集具有 3 个主节点，*最小\_主\_节点*应设置为 2。 如果您有 5 个主节点，*最小\_主\_节点*应设置为 3。 理想情况下，您应具有主节点数为奇数。

> [AZURE.NOTE] 它有可能大脑分裂出现同一群集中的多个主节点会同时启动。 虽然这种情况非常少见，您可以阻止它从节点开始顺序进行一个短暂的延迟 （5 秒） 每个之间。 

## <a name="handling-rolling-updates"></a>处理滚动更新

如果您正在执行软件升级到节点自己 （如迁移到较新版本或执行修补程序），您可能需要需要同时保持可用群集的其余部分将其脱机的各个节点上执行工作。 在此情况下，请考虑实施下列过程。 

1. 确保足够延迟 shard 重新分配以防止选举的主重新从丢失的节点的 shards 平衡整个群集的其余部分。 默认情况下 shard 重新分配被延迟 1 分钟，但如果一个节点时，可能需要更长一段不可用，您可以增加持续时间。 下面的示例将增加到 5 分钟的延迟︰

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] 您还可以通过将群集的*cluster.routing.allocation.enable*设置为*无*完全禁用 shard 重新分配。 但是，应避免使用这种方法是否可能会因为这可能会导致索引分配失败导致了红色状态的群集节点处于脱机状态时创建新的索引。

2. 停止节点维护上的 Elasticsearch。 如果 Elasticsearch 作为服务运行，您可能能够通过使用操作系统命令控制的方式停止进程。 下面的示例演示如何停止在 Ubuntu 上运行的单个节点上的 Elasticsearch 服务︰

    ```bash
    service elasticsearch stop
    ```

    或者，您可以直接在节点上使用关闭 API:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. 在节点上执行必要的维护

4. 重新启动该节点，并等待其加入群集。

5. 重新启用 shard 分配︰

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] 如果您需要维护多个节点，请重复步骤 2&ndash;4 之前重新启用 shard 分配的每个节点上。

如果可能，请停止索引在此过程中的新数据。 这将有助于最小化恢复时间节点重新联机重新加入群集。

当心自动更新的 JVM （理想情况下，禁用自动更新这些项目） 等项目，特别在 Elasticsearch 下运行 Windows。 Java 更新代理可以自动下载最新版本的 Java，但可能需要 Elasticsearch 重新启动更新才能生效。 这可以导致不协调临时丢失的节点，具体取决于如何配置 Java 更新代理。 这也可能在不同情况下的 Elasticsearch 在同一群集中运行不同版本的 JVM，这可能会导致兼容性问题导致。

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>测试和分析 Elasticsearch 的复原和恢复

本部分介绍一系列为适应性评估所执行的测试和包含三个数据节点和主节点，三个 Elasticsearch 群集故障恢复。

以下方案进行了测试︰ 

- 节点出现故障并且不丢失数据的重新启动。 数据节点停止并重新启动后 5 分钟。 Elasticsearch 已配置为不重新分配缺少 shards 在此时间间隔，因此 shards 移动而导致没有额外的 I/O。 节点重新启动后，恢复过程将 shards 带回最新的节点上。

- 灾难性的数据丢失的节点故障。 停止数据节点和它包含的数据被删除，以模拟灾难性磁盘故障。 该节点然后后重新启动 （5 分钟），有效地作为代替原来的节点。 恢复过程需要重建丢失的数据节点，并且可能涉及 shards 持有其他节点上重新定位。

- 节点出现故障，重启没有数据丢失，但 shard 重新分配。 数据节点已停止，它持有的 shards 被重新分配给其他节点。 然后重新启动该节点并将更多的再分配来重新平衡群集。

- 滚动更新。 停止群集中的每个节点并在较短的间隔，以模拟在软件更新后重新启动计算机后重新启动。 在任何时刻，只有一个节点已停止。 当一个节点出现故障时，shards 是不重新分配。

每个方案是根据同一负载时节点脱机和恢复拍摄包括数据接收任务、 聚合和筛选器查询的组合。 大容量插入操作中每个存储 1000年文档并对聚合时的一个索引执行的工作负载和筛选器查询使用单独的索引包含几种数以百万计的文档。 这就是能够从批量插入单独评估的查询的性能。 每个索引包含五个 shards 和一个副本。

以下各节概括的这些测试结果，一个节点处于脱机状态时，请注意性能降级或正在恢复，并报告任何错误。 突出显示一个或多个节点的缺少和估计系统完全恢复并达到类似水平的节点脱机之前存在的性能所需的时间点，结果都以图形化的方式，呈现。

> [AZURE.NOTE] 用来执行这些测试，这些测试工具在联机时可用。 您可以调整和使用这些工具来验证复原和您自己的群集配置的可恢复性。 有关详细信息，请参阅[运行自动的 Elasticsearch 弹性测试][]。

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>节点出现故障并且不丢失数据的重新启动︰ 结果

<!-- TODO; reformat this pdf for display inline --> 

此测试结果文件[ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf)所示。 图形中将显示性能特征的工作负载和物理资源的每个节点的群集中。 图形的初始部分显示正常运行约 20 分钟，此时节点 0 关闭 5 分钟后重新启动系统。 阐释了进一步 20 分钟的统计信息;系统需要大约 10 分钟恢复和稳定。 说明了这一点的事务处理速率和不同的工作负载的响应时间。

请注意以下几点︰

- 在测试期间，未不报告任何错误。 没有数据已丢失，并且所有操作成功都完成。

- 所有的三种类型的断开操作 （大容量插入、 聚合查询和筛选查询） 和平均响应时间的事务率增加脱机节点 0 时。

- 在恢复期、 事务处理速率和聚合的查询和筛选查询的响应时间逐渐恢复操作。 虽然之前降低，短恢复大容量插入的性能。 但是，这可能是由于导致大容量插入用于增长时，索引的数据量，并且可以看到此操作事务处理速率减缓，甚至在节点 0 脱机之前。

- 0 节点的 CPU 利用率图显示在恢复阶段，减少了的活动，这是由于增加的磁盘和网络活动引起的故障恢复机制，该节点已赶上与离线时，它就已经丢失了所有数据并更新包含 shards。

- 索引 shards 不完全分布在所有节点间的平等。 有两个包含 5 shards 和 1 副本，每个，使总数 20 shards 的索引。 两个节点因此将包含 6 shards，而其他两个容纳 7 每个。 这是在 CPU 使用率图表中明显在最初 20 分钟内，节点 0 比其他两个不太忙。 完成恢复后，一些切换似乎出现节点 2 出现变得更轻的节点。

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>灾难性的数据丢失的节点故障︰ 结果

<!-- TODO; reformat this pdf for display inline -->

此测试的结果将显示在[ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf)文件中。 与第一次测试中，关系图的开始部分所示系统运行通常约为 20 分钟，在哪个点节点 0 已被关闭了 5 分钟。 在此期间，Elasticsearch 在这个节点上会删除数据之前正在重新启动, 模拟灾难性的数据丢失。 完整恢复似乎需要 12-15 分钟之前还原性能测试之前看到的级别。

请注意以下几点︰

- 在测试期间，未不报告任何错误。 没有数据已丢失，并且所有操作成功都完成。

- 所有的三种类型的断开操作 （大容量插入、 聚合查询和筛选查询） 和平均响应时间的事务率增加脱机节点 0 时。 在这种情况下，测试的性能配置文件是类似于第一种方案。 这并不奇怪，到目前为止，情况相同。

- 在恢复期间，事务率和响应时间被还原，尽管在此期间有了更多波动性图中。 这最可能是由于额外的工作，执行群集中的节点，提供数据来还原丢失的 shards。 这一附加工作是很明显的 CPU 利用率、 磁盘活动和网络活动图。

- 0 和 1 的节点的 CPU 利用率图显示减少了的活动的恢复阶段，这是由于增加的磁盘和网络活动恢复过程所致。 在第一个方案中，不仅要恢复的节点出现这种现象，并且这种情况下似乎有可能丢失的数据节点 0，大部分正在还原从节点 1。

- 为节点 0 的 I/O 活动实际减少了与第一种情形相比。 这可能是由于 I/O 效率只是复制整个 shard，而不是必需，使最新现有 shard 的较小 I/O 请求的一系列数据。

- 所有三个节点的网络活动指明发送和接收节点之间数据突发的活动。 在方案 1 中，只需节点 0 多网络活动，但此活动表现出似乎会持续更长一段。 再次，这种差异可能是由于效率作为单个请求，而不是一连串小 shard 中恢复时，接收到的请求传输整个数据的 shard 中。

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>节点故障和重新启动与 shard 重新分配︰ 结果

<!-- TODO; reformat this pdf for display inline -->

[ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf)的文件说明了此测试的结果。 按照与第一个测试中，初始部分的图形显示系统运行通常约为 20 分钟，在哪个点节点 0 已被关闭了 5 分钟。 在这种情况下，Elasticsearch 群集尝试重新创建缺少的 shards 并重新平衡剩余节点上的 shards。 后 5 分钟节点 0 已联机，并且再一次群集重新平衡 shards。 12-15 分钟后恢复性能。

请注意以下几点︰

- 在测试期间，未不报告任何错误。 没有数据已丢失，并且所有操作成功都完成。

- 删除操作 （大容量插入、 聚合查询和筛选查询） 的所有三种类型的事务率和平均响应时间显著增加节点 0 脱机时与前面的两个测试。 这是由于提高了的群集活动重新创建缺少的 shards 和重新平衡群集，因为在此期间由节点 1 和 2 的磁盘和网络活动的凸起数字证据。

- 期间内恢复正常，节点 0 后的事务率和响应时间保持易失性。

- 在恢复阶段的节点 0 显示非常减少初始操作 CPU 利用率和磁盘活动图表。 这是因为在这种情况下，节点 0 不提供任何数据。 一段约 5 分钟之后, 节点的突发行动 < RBC︰ 这使我 snort 大声读出。 我并没有提出尽管说这更好的方法。  >> 网络、 磁盘和 CPU 活动的突然增加所示。 这很可能被由于重新 shards 分发跨节点的群集。 然后将节点 0 显示正常活动。
  
## <a name="rolling-updates-results"></a>滚动更新︰ 结果

<!-- TODO; reformat this pdf for display inline -->

此测试，请在文件[ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf)，结果显示每个节点脱机并且再调回再次连续方式。 5 分钟后重新启动在哪个点停止序列中的下一个节点的每个节点已被关闭。

请注意以下几点︰

- 在循环的每个节点，则会相当甚至保持在吞吐量和响应时间方面的性能。

- 磁盘活动的每个节点在短时间提高，因为它恢复正常。 这很可能是由于恢复过程滚下节点时出现的任何变化。

- 当一个节点脱机时，出现剩余的节点网络活动中的峰值。 重新启动节点时，也会出现峰值。

- 回收的最后一个节点后，系统将进入一段明显的波动性。 这是最有可能导致恢复过程无需在每个节点之间同步更改，并确保所有复制副本和其相应的 shards 是一致的。 在一个点上，此投入原因连续批量插入操作超时并失败。 每个用例都时报告错误︰

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

后续的实验结果表明，引入几分钟循环的每个节点之间的延迟消除此错误，因此，很可能是由恢复过程尝试同时还原多个节点之间的争用和大容量插入操作尝试存储数千个新文档。


## <a name="summary"></a>摘要

执行的测试指出︰

- Elasticsearch 是高弹性给可能在群集中出现故障的最常见模式。

- 如果精心设计的群集会发生灾难性的数据丢失的节点上，Elasticsearch 可以快速恢复。 如果您配置 Elasticsearch 将数据保存到临时存储，但随后该节点以重新启动后重新设置，则可发生此错误。 这些结果显示，即使在这种情况下，使用临时存储的风险是最有可能被这类存储提供的性能收益所抵消。

- 在前三个方案中，没有时发生错误并发的大容量插入、 聚合和筛选器的查询工作负载中某个节点脱机，并恢复拍摄。

- 只有最后的方案指示潜在的数据丢失，并且这种损失只会影响正在添加新数据。 它是在应用程序中执行数据摄取来降低这种可能性，通过重试已失败，因为报告错误的类型是很有可能会是暂时的插入操作的好习惯。

- 最后的测试结果也显示是否您执行计划内的维护的节点的群集中，性能将获益是否允许之间重启一个节点以及接下来的几分钟时间。 在意外情况下 （例如回收执行操作系统更新后的节点数据中心），有更少的控制方式和时间节点都关闭并重新启动。 Elasticsearch 尝试连续节点停机后恢复群集的状态时发生的争用可能会导致超时和错误。 

[管理虚拟机的可用性]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[运行自动化的 Elasticsearch 弹性测试]: guidance-elasticsearch-running-automated-resilience-tests.md
