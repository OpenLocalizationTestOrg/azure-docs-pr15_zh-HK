<properties
   pageTitle="可恢复性核对表 |Microsoft Azure"
   description="提供在设计过程中的弹性问题指南的清单。"
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure 复原指导︰ 复原清单

设计您的应用程序恢复能力需要规划和缓解各种可能发生的故障模式。 查看应用程序的设计，使其更具弹性对此核对清单中的项目。

## <a name="requirements"></a>要求

- **定义您的可用性需求。** 您的客户将具有应用程序中的组件的可用性要求，这将影响应用程序的设计。 对于每个应用程序的可用性目标从您的客户获得协议，否则您的设计可能不满足客户的期望。 有关详细信息，请参阅[设计弹性 Azure 应用程序](guidance-resiliency-overview.md)文档的[恢复能力等需求定义](guidance-resiliency-overview.md#defining-your-resiliency-requirements)部分。

## <a name="failure-mode-analysis"></a>故障模式分析

- **您的应用程序进行故障模式分析 (FMA)。** FMA 是构建到应用程序在设计阶段的早期复原的过程。 FMA 的目标包括︰  

    - 标识应用程序可能会遇到何种类型的故障。 
    - 捕获的潜在效果和每种类型的应用程序故障的影响。
    - 确定故障恢复策略。 

    有关详细信息，请参阅[设计弹性 Azure 应用程序︰ 故障模式分析][fma]。  

## <a name="application"></a>应用程序

- **部署服务的多个实例。** 服务将不可避免地失败，并且如果您的应用程序依赖于一个服务实例将不可避免地失败也。 若要设置[Azure 应用程序](../app-service/app-service-value-prop-what-is.md)服务的多个实例，选择[应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)，提供了多个实例。 对于 Azure 的云服务，配置每个角色使用[多个实例](../cloud-services/cloud-services-choose-me.md#scaling-and-management)。 [Azure 虚拟机 (Vm)](../virtual-machines/virtual-machines-windows-about.md)，确保您的虚拟机体系结构包括多个虚拟机的[可用性设置]中包含每个虚拟机[availability-sets]。   

- **使用负载平衡器将请求分发。** 负载平衡器将对正常运行的服务实例的应用程序的请求分发的不健康的实例删除旋转。 如果您的服务使用 Azure 应用程序服务或 Azure 云服务，它已经负载平衡为您。 但是，如果您的应用程序使用 Azure 的虚拟机，您需要配置负载平衡器。 [Azure 负载平衡器](../load-balancer/load-balancer-overview.md)概述有关更多详细信息，请参阅。 

- **配置 Azure 应用程序网关使用多个实例。** 根据您的应用程序的要求， [Azure 应用程序网关](../application-gateway/application-gateway-introduction.md)可能会更好地适合于分发到应用程序的服务请求。 但是，应用程序网关服务的单个实例不能保证由 SLA 因此很可能您的应用程序可能会失败，如果应用程序网关实例失败。 设置多个中型或大型应用程序网关实例来保证[SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)条款到服务的可用性。

- **使用每个应用层的可用性设置**。 将您的实例放置在[可用性设置][availability-sets]内的[SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/)条款保证至少一个虚拟机实例的连接。 如果您的虚拟机不能在没有可用性组保证保护它们，可能他们所有可能会失败，或者同时进行更新。 

- **请考虑在多个区域部署您的应用程序。** 如果您的应用程序部署到单个区域，万一整个地区变得不可用，您的应用程序也将无法使用。 这可能是您的应用程序 SLA 条款不可接受。 如果是这样，请考虑在多个区域部署您的应用程序和服务。 多区域部署可以使用活动模式 （在多个活动实例之间分发请求） 或主动-被动模式 （保持"感兴趣"的实例中预留，在主实例出现故障的情况下）。 我们建议您在区域对部署您的应用程序服务的多个实例。 有关详细信息，请参阅[业务连续性和灾难恢复 (BCDR): Azure 配对地区](../best-practices-availability-paired-regions.md)。

- **在适当情况下实施远程操作的恢复模式。** 如果您的应用程序依赖于远程服务之间的通信，将不可避免地失败的通信路径。 如果有多个发生故障，剩余正常应用程序的服务实例无法请求不堪重负。 有几个模式用于包括超时图案，[重试模式]的常见故障处理[retry-pattern]，[断路器][circuit-breaker]模式，和其他人。 有关详细信息，请参见[设计弹性 Azure 应用程序](guidance-resiliency-overview.md#implementing-resiliency-strategies)。 

- **使用自动缩放来响应负载增加。** 如果您的应用程序未配置为自动随着负载的增加而扩展，则可能您的应用程序服务将失败，如果饱和与用户请求。 有关详细信息，请参阅以下文章︰

    - 常规︰[可伸缩性核对清单](../best-practices-scalability-checklist.md) 
    - Azure 应用程序服务︰[手动或自动缩放实例计数][app-service-autoscale]
    - 云服务︰[如何自动缩放云服务][cloud-service-autoscale]
    - 虚拟机︰[自动缩放功能以及虚拟机比例设置][vmss-autoscale]


- **实现异步操作，只要有可能。** 同步操作可以独占资源并阻止其他操作，而调用方等待进程完成。 设计您的应用程序，以便尽可能的异步操作的每个部分。 有关如何在 C# 中实现异步编程的详细信息，请参阅[使用异步的异步编程并等待][asynchronous-c-sharp]。

- **Azure 流量管理器用于将应用程序的通信路由到不同的地区。**  [Azure 的流量管理器][traffic-manager]执行 DNS 级负载平衡，可将通信路由到不同地区基于[通信路由][traffic-manager-routing]方法来指定和运行状况的应用程序的终结点。 

- **配置和测试为负载平衡器和通讯经理健康探测器。** 请确保您健康的逻辑检查系统的关键部分和对健康探测器做出相应的响应。

    - 运行状况寻找[Azure 流量管理器][ traffic-manager] [Azure 负载平衡器]和[load-balancer]提供特定功能。 为通讯管理器中，健康探测器可确定是否为故障切换到另一个区域。 对于负载平衡器，它确定是否从旋转删除虚拟机。      

    - 为通讯管理器探测器，健康端点应检查任何严重依赖项部署在相同的区域中，并且其失败将触发故障切换到另一个区域。  

    - 负载平衡器，对于健康终结点应报告虚拟机的运行状况。 不要包括其他层或外部服务。 否则，所外虚拟机发生故障将导致从旋转删除虚拟机的负载平衡器。

    - 实现应用程序中的运行状况监视的指导，请参阅[健康终结点监视模式](https://msdn.microsoft.com/library/dn589789.aspx)。

- **监视第三方服务。** 如果您的应用程序在第三方服务有依赖项，确定在何处如何这些第三方服务可能会失败，什么影响这些故障时会对您的应用程序。 第三方服务可能未包括监视和诊断，因此很重要，记录的这些您调用并将它们与您应用程序的运行状况相关联和诊断日志记录级别使用的唯一标识符。 有关监视和诊断的最佳做法的详细信息，请参阅[监视和诊断指导][monitoring-and-diagnostics-guidance]文档。

- **确保您使用的任何第三方服务提供 SLA。** 如果应用程序依赖于第三方服务，但第三方提供任何保证 SLA 的窗体中的可用性，您的应用程序可用性也无法得到保证。 SLA 是只相当于您的应用程序的最小可用组件。


## <a name="data-management"></a>数据管理

- **了解应用程序的数据源的复制方法。** 应用程序数据将存储在不同的数据源，并且有不同的可用性要求。 计算每种类型的数据存储在 Azure 中的复制方法，包括[Azure 存储复制](../storage/storage-redundancy.md)和[SQL 数据库活动地区的复制](../sql-database/sql-database-geo-replication-overview.md)，以确保满足您应用程序的数据需求。

- **确保没有单个的用户帐户有权访问生产和备份数据。** 如果一个单个的用户帐户有权限写入到生产和备份源也不怕您数据的备份。 恶意用户故意无法删除所有数据，而普通用户无法意外地都删除它。 限制每个用户帐户的权限，以便只需要写访问权限的用户具有写访问权限，它只会应用到生产或备份，但不是能同时将应用程序设计。

- **您的数据源进行故障转移和故障恢复的文档处理，并对其进行测试。** 在数据源失败灾难性的情况下，人工接线员将必须遵循一系列有案可稽的指令为故障切换到新的数据源。 如果有案可稽的步骤中存在错误，操作员不能成功地跟随他们和故障转移资源。 定期测试指令步骤验证运算符后面是能够成功故障转移和故障自动恢复的数据源。

- **验证您的数据的备份。** 定期验证备份数据是您预期通过运行脚本来验证数据的完整性、 架构和查询。 没有点拥有一个备份文件，如果它是没有用来恢复您的数据源。 记录和报告的任何不一致，因此可以修复备份服务。

- **请考虑使用地理冗余的存储帐户类型。** 在 Azure 存储帐户中存储的数据始终是本地复制。 但是，有多个复制战略配置存储帐户时进行选择。 选择要保护的罕见情况下对应用程序数据的整个区域不可用时的[Azure 读取访问地理冗余存储 (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) 。

    > [AZURE.NOTE] 为虚拟机，不要依赖 RA GRS 复制还原虚拟机磁盘 （VHD 文件）。 相反，使用[Azure 备份][azure-backup]。   

## <a name="operations"></a>操作

- **实施监视和警报在您的应用程序的最佳做法。** 没有适当监视、 诊断和报警，没有方法来检测应用程序中的故障，并提醒操作员来解决这些问题。 有关最佳做法的详细信息，请参阅[监视和诊断指导][monitoring-and-diagnostics-guidance]文档。

- **测量远程调用统计信息，并使信息可用于应用程序开发小组。**  如果不跟踪和报告实时远程调用统计信息，可以方便地查看此信息，操作团队不会即时查看到您的应用程序的运行状况。 如果您仅度量平均远程调用时，您将没有足够的信息来显示和服务中的问题。 总结了远程调用指标如延迟、 吞吐量和 99 和 95 百分点中的错误。 找出每个百分点中发生的错误的指标进行统计分析。

- **跟踪一段适当的时间范围内瞬时异常和重试次数。** 如果不跟踪和监视瞬时异常和重试次数随着时间的推移，就某个问题或故障无法隐藏的应用程序的重试逻辑。 也就是说，如果您监视和日志记录仅显示成功或失败的操作，操作不得不多次重试由于异常的事实将被隐藏。 随着时间的推移增加例外的趋势说明该服务时遇到问题，可能会失败。 有关详细信息，请参阅[重试服务具体指导][retry-service-guidance]。

- **实现早期的警告系统，向操作员发出警报。** 确定关键绩效指标的应用程序的运行状况，如瞬时异常和远程调用延迟时间，并设置适当的阈值为每个。 当达到临界值时，操作发送警报。 在发现问题之前它们变得重要，并且需要恢复响应的级别上设置这些阈值。

- **记录您的应用程序的发布过程。** 而详细的发布流程文档，无需操作员可能部署坏的更新或不正确地配置应用程序设置。 清楚地定义并记录发布过程，并确保可用于整个运营团队。 [有弹性的部署]中详细介绍了弹性部署您的应用程序的最佳实践[guidance-resilient-deployment]复原指导文档部分。

- **确保在团队中多人经过培训，能够监控应用程序并执行任何手动恢复步骤。** 如果在团队可以监视该应用程序并开始恢复步骤只有单个运算符，此人将成为单点故障。 检测和恢复的多个人员进行训练，并确保始终至少一个活动在任何时候。

- **自动执行应用程序的部署过程。** 如果需要手动部署您的应用程序操作人员，人为错误可能导致部署失败。 自动执行应用程序部署的最佳做法的详细信息，请参阅[弹性部署][guidance-resilient-deployment]复原指导文档部分。

- **设计您的释放过程，以最大化应用程序的可用性。** 如果您发布过程要求服务脱机部署期间，您的应用程序将无法使用，直到它们重新联机。 使用[蓝/绿](http://martinfowler.com/bliki/BlueGreenDeployment.html)或[加那利发布](http://martinfowler.com/bliki/CanaryRelease.html)部署技术部署到生产环境的应用程序。 这两种技术涉及部署发布代码一起成品代码以便发布代码的用户可以重定向到发生故障的生产代码。 有关详细信息，请参阅[弹性部署][guidance-resilient-deployment]复原指导文档部分。

- **记录和审核应用程序的部署。** 如果您使用分阶段部署技术，如蓝/绿或加那利版本中会有多个版本的应用程序在生产环境中运行。 如果发生问题，很重要的以确定哪个版本的应用程序导致了问题。 实施可靠的日志记录策略来捕获尽可能多尽可能的特定于版本的信息。 

- **请确保您的应用程序不会运行计算出来[Azure 订阅限制](../azure-subscription-service-limits.md)。** Azure 订阅特定资源类型，如资源组数、 核心，数量和存储帐户数有限制。  如果您的应用程序需求超过 Azure 的订阅数限制，创建另一个 Azure 订阅和提供足够的资源存在。

- **请确保您的应用程序不会运行计算出来[每个服务的限制](../azure-subscription-service-limits.md)。** 各个 Azure 服务具有消耗量限制&mdash;为例，在存储、 吞吐量、 连接、 每秒请求数和其他指标的数量限制。 如果它尝试使用资源超出了这些限制，您的应用程序将失败。 这将导致受影响的用户的服务限制和可能停机。 

    这取决于特定的服务和应用程序的要求，您常常可以避免这些限制通过提高个体功能 （例如，选择另一个定价层） 或横向扩展 （添加新实例）。  

- **设计应用程序的存储需求内 Azure 存储可扩展性和性能目标。** Azure 存储用于函数中预定义的可扩展性和性能目标，来设计您的应用程序可以利用这些目标中的存储。 如果超过这些目标应用程序会遇到存储限制。 若要解决此问题，提供额外的存储帐户。 如果您运行计算出来的存储帐户限制，调配其他 Azure 订阅，然后调配额外的存储帐户。 有关详细信息，请参阅[Azure 存储可扩展性和性能目标](../storage/storage-scalability-targets.md)。

- **选择您的应用程序虚拟机大小。** 衡量实际 CPU、 内存、 磁盘和 I/O 的您在生产环境中的虚拟机并验证所选的虚拟机大小足够。 如果不是，您的应用程序可能会遇到产能问题，当虚拟机接近极限。 在[Azure 中的虚拟机大小](../virtual-machines/virtual-machines-windows-sizes.md)文档详细描述 VM 大小。

- **确定您的应用程序工作负载是否稳定或波动随着时间的推移。** 如果您的负载量随着时间的推移，使用 Azure VM 比例自动缩放中设置虚拟机实例数。 否则，必须手动增加或减少虚拟机的数量。 有关详细信息，请参阅[虚拟机比例设置概述](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)。

- **为 SQL Azure 数据库中选择适当的服务层。** 如果您的应用程序使用 SQL Azure 数据库，请确保选择适当的服务层。 如果您选择不能处理您的应用程序数据库事务单位 (DTU) 要求某一层，将阻止您数据的使用。 选择正确的服务计划的详细信息，请参阅[SQL 数据库选项和性能︰ 了解什么是可用在每一个服务层中](../sql-database/sql-database-service-tiers.md)文档。 

- **已经部署回滚计划。** 很可能您的应用程序部署可能失败，并且会导致您的应用程序变得不可用。 设计出一个回滚过程返回到已知的最后良好版本并将停机时间降至最低。 请参阅[弹性部署][guidance-resilient-deployment]复原指导文档的详细信息一节。 

- **创建用于与 Azure 支持交互的流程。** 如果联系[Azure 支持](https://azure.microsoft.com/support/plans/)的过程未设置之前需要联系支持时，停机时间将需要较长时间为第一次导航支持流程。 包括联系支持人员和作为应用程序的恢复能力，在设计之初的一部分上报问题的过程。

- **请确保您的应用程序不能使用多个存储帐户，每个订阅的最大数量。** Azure 允许最多为每个订阅 200 存储帐户。 如果您的应用程序需要更多的存储帐户，而不只是在您的订阅当前可用，必须创建新的订阅，并创建那里额外的存储帐户。 有关详细信息，请参阅[Azure 订阅和服务限制、 配额和限制](../azure-subscription-service-limits.md#storage-limits)。

- **请确保您的应用程序不会超过虚拟机磁盘的可伸缩性目标。** Azure IaaS VM 支持附加大量的数据磁盘，具体取决于多种因素，包括虚拟机的大小和类型的存储帐户。 如果您的应用程序超过虚拟机磁盘的可伸缩性目标，提供附加的存储帐户，创建的虚拟机磁盘。 有关详细信息，请参阅 [Azure 存储可扩展性和性能目标] (.../ storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks）

## <a name="test"></a>测试

- **执行故障转移和故障自动恢复测试您的应用程序。** 如果完全没有测试故障转移和故障自动恢复，则您无法确定该应用程序中的相关服务时有时无以同步的方式在灾难恢复过程。 请确保您的应用程序依赖于服务故障转移和故障恢复按正确的顺序。

- **执行错误植入测试您的应用程序。** 您的应用程序可能会失败很多不同的原因，如证书过期，耗尽系统中的资源的虚拟机或存储故障。 在尽可能接近于生产，通过模拟或真实故障触发的环境中测试您的应用程序。 例如，删除证书、 人为消耗系统资源，或删除存储源。 请验证您的应用程序能够从所有类型的单独和组合中的故障恢复。 不传播或通过您的系统级联故障的检查。

- **使用这两种合成和真实的用户数据的生产环境中运行测试。** 测试和生产是很少完全相同，因此请务必使用蓝色/绿色或加那利部署和测试应用程序在生产环境中。 这允许您在实际负载下的生产环境中测试您的应用程序，并确保它将正常运行时完全部署。

## <a name="security"></a>安全

- **实现应用程序级别防范分布式拒绝服务 (DDoS) 攻击。** Azure 服务不会受到网络层的 DDos 攻击。 但是，因为很难区分真正的用户请求，恶意用户请求从 Azure 无法防止应用程序层攻击。 如何防止应用程序层 DDoS 攻击的详细信息，请参阅[Microsoft Azure 网络安全](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf)（PDF 下载） 的"根据 DDoS 保护"一节。

- **实现应用程序的资源的访问权的最小权限的原则。** 对应用程序的资源访问权限的默认设置应尽可能严格。 授予在审批的基础上的更高级别的权限。 默认情况下授予过度授权访问您的应用程序资源会被人有意或无意地删除资源。 Azure 提供[基于角色的访问控制](../active-directory/role-based-access-built-in-roles.md)来管理用户权限，但务必要验证具有其自己的权限系统，如 SQL Server 的其它资源的最少特权权限。 

## <a name="telemetry"></a>遥测

- **在生产环境中运行应用程序时，请记录遥测数据。** 捕获强健的遥测信息，而在生产环境中运行该应用程序，或者您不具有足够的信息来诊断问题的原因，尽管它主动为用户提供服务。 详细信息可用于日志记录的最佳做法可用于[监视和诊断指导][monitoring-and-diagnostics-guidance]文档。

- **实现使用异步模式的日志记录功能。** 如果日志记录的操作是同步的他们可能会阻止您的应用程序代码。 确保日志记录的操作为异步操作实现。 

- **跨服务的日志数据关联起来。** 在典型的 n 层应用程序中，用户请求，也能够遍历多个服务边界。 例如，用户请求 web 层通常源自是传递到业务层和最后保留在数据层中。 在更复杂的情况下，可能将用户请求分发给许多不同的服务和数据存储。 确保记录系统跨边界服务关联的调用以便您可以在整个应用程序跟踪请求。

##  <a name="azure-resources"></a>Azure 的资源 

- **使用 Azure 资源管理器模板来调配资源。** 资源管理器模板便于自动化部署通过 PowerShell 或 Azure CLI，更可靠的部署过程的潜在顾客。 有关详细信息，请参阅[Azure 资源管理器概述][resource-manager]。

- **为资源提供有意义的名称。** 为资源提供有意义的名称便于找到特定的资源并理解其角色。 有关详细信息，请参阅[推荐命名约定的 Azure 的资源](guidance-naming-conventions.md) 

- **使用基于角色的访问控制 (RBAC)**。 使用 RBAC 控制部署的 Azure 资源访问权限。 RBAC 可以将授权角色分配给您的 DevOps 团队成员，以防止意外删除或更改已部署的资源。 有关详细信息，请参阅[开始使用 Azure 门户中访问管理](../active-directory/role-based-access-control-what-is.md) 

- **用于关键的资源，如虚拟机的资源锁。** 资源锁可以防止意外删除资源运算符。 有关详细信息，请参阅[锁定资源使用 Azure 资源管理器](../resource-group-lock-resources.md) 

- **区域对。** 当部署到两个区域，从相同的区域对选择区域。 发生大停电，一个地区的恢复出每一对确定优先级。 某些服务，如地理冗余存储提供自动复制到成对的地区。 有关详细信息，请参阅[业务连续性和灾难恢复 (BCDR): Azure 配对地区](../best-practices-availability-paired-regions.md) 

- **组织的功能和生命周期的资源组**。  一般情况下，某一资源组应包含资源共享相同的生命周期。 这使得更容易地管理部署、 删除测试部署和分配访问权限，减少生产部署被意外地删除或修改的机会。 创建单独的资源组用于生产、 开发和测试环境。 在多区域部署中，将放入单独的资源组的每个地区的资源。 这使得更易于重新部署而不会影响其他区域的一个区域。 

## <a name="azure-services"></a>Azure 服务 

以下核对清单项适用于 Azure 中的特定服务。

###  <a name="app-service"></a>应用程序服务 

- **使用标准或特优层。** 这些层支持临时插槽和自动备份。 有关详细信息，请参阅[Azure 应用程序服务计划深入概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **避免向上或向下扩展。** 相反，选择层和满足您的性能要求，在正常负载，然后[向外的扩展](../app-service-web/web-sites-scale.md)的实例大小的实例来处理流量的变化。 向上和向下扩展可能会触发应用程序重启。  

- **用于存储配置为应用程序设置。** 使用应用程序设置来保存应用程序设置作为配置设置。 资源管理器模板，或使用 PowerShell，以便可以将它们作为自动部署的一部分应用 / 更新过程中，即更加可靠中定义的设置。 有关详细信息，请参阅[在 Azure 应用程序服务的配置 web 应用程序](../app-service-web/web-sites-configure.md)。 

- **创建单独的应用程序服务计划，以生产和测试。** 不要使用插槽上生产部署进行测试。  在同一应用程序服务计划内的所有应用程序共享同一个 VM 实例。 如果在同一计划将生产和测试部署，则会产生负面影响生产部署。 例如，负载测试可能会降低实际生产站点。 通过将测试部署到不同的计划，您的生产版本将它们隔离。  

- **单独的 web 应用程序从 web Api**。 如果您的解决方案具有一个 web 前端和 web API，可以考虑将其分解成单独的应用程序服务应用程序。 这种设计更容易分解解决方案按工作负载。 您可以运行 web 应用程序和 API 在单独的应用程序服务计划中，以便可以单独扩展。 如果不需要该级别的可伸缩性，在第一次，可以将应用程序部署到相同的计划，并将它们移到单独的计划之后，如果需要。

- **避免使用的应用程序服务备份功能来备份 SQL Azure 数据库。** 相反，请使用[SQL 数据库自动备份][sql-backup]。 应用程序服务备份将数据库导出到 SQL.bacpac 文件，即成本 DTUs。  

- **部署到临时的插槽。** 创建临时的部署细长。 将应用程序更新部署到临时的插槽中，并验证部署，然后换到生产环境。 这可以减少在生产中坏的更新的机会。 它还确保所有实例都取暖被交换到生产环境之前。 许多应用程序有大量的预热和冷启动时间。 有关详细信息，请参阅[设置临时在 Azure 应用程序服务 web 应用程序的环境](../app-service-web/web-sites-staged-publishing.md)。 

-  **创建用于保存上一次已知良好 (LKG) 部署的部署插槽。** 在将更新部署到生产环境中，进入 LKG 插槽以前生产部署。 这使得它容易回滚错误的部署。 如果以后发现了问题，您可以快速恢复到 LKG 版本。 有关详细信息，请参阅[Azure 的参考体系结构︰ 基本的 web 应用程序](guidance-web-apps-basic.md)。 

- **启用诊断日志记录**，包括日志记录应用程序和 web 服务器日志记录。 日志记录是很重要的监控和诊断。 请参阅[启用在 Azure 应用程序服务 web 应用程序的诊断日志记录](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Blob 存储日志**。 这便于收集和分析数据。 

- **创建日志的一个单独的存储帐户。** 不要使用相同的存储帐户日志和应用程序数据。 这有助于防止从降低应用程序性能日志记录。 

- **监视性能。** 使用的性能监视服务，如[新的 Relic](http://newrelic.com/)或[应用程序的见解](../application-insights/app-insights-overview.md)监视应用程序性能和负载下的行为。  性能监视可帮助您应用程序的实时深入。 它可以用来诊断问题并执行的失败的根本原因分析。 


###  <a name="application-gateway"></a>应用程序网关 

- **设置至少两个实例。** 部署应用程序网关，并至少两个实例。 一个实例是单点故障。 使用两个或多个实例的冗余和可扩展性。 为了符合[SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)，您必须配置两个或多个中等或更大的实例。 

### <a name="azure-search"></a>Azure 搜索

- **设置多个副本。** 用于读取的高可用性或三读写高可用性的至少两个副本。

- **配置索引对于多区域部署。** 如果您有多个区域部署，考虑您的选项的索引中的连续性。

    - 如果数据源是地区复制，指向其本地数据源复制副本每个区域的 Azure 搜索服务的每个索引器。  

    - 如果数据源不是地区复制，点对相同的数据源的多个索引器，以便在多个地区的 Azure 搜索服务不断地、 独立地进行索引从数据源。 有关详细信息，请参阅[Azure 搜索性能和优化考虑][search-optimization]。

###  <a name="azure-storage"></a>Azure 存储 

- **应用程序数据使用读取访问地理冗余存储 (RA GRS)。** RA GRS 存储将数据复制到辅助区域，并提供从次区域的只读访问权限。 如果在主区域中存储中断，应用程序可以从次区域读取数据。 有关详细信息，请参阅[Azure 存储复制](../storage/storage-redundancy.md)。

- **为虚拟机磁盘使用高级存储**有关详细信息，请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](../storage/storage-premium-storage.md)。

- **对于队列存储在另一个区域中创建备份队列。** 对于队列存储，只读副本有限使用，因为不能排队或取消排队项目。 相反，在另一个区域中的存储帐户创建备份队列。 如果没有存储中断，应用程序可以使用备份的队列中，直到再次变为可用的主要地区。 这样一来，应用程序仍可以处理新的请求。  

###  <a name="documentdb"></a>DocumentDB 

- **区域间复制数据库。** 多区域帐户，DocumentDB 数据库都有一个写地区和读的多个区域。 如果在写区内故障，可以从另一个副本中进行读取。 客户端 SDK 自动处理这。 您可以通过该写区域保存到另一个区域失败。 有关详细信息，请参阅[DocumentDB 全局分布数据](../documentdb/documentdb-distribute-data-globally.md)。


###  <a name="sql-database"></a>SQL 数据库 

- **使用标准或特优层。** 这些层提供再时间点还原期间 （35 天）。 有关详细信息，请参阅[SQL 数据库选项和性能](../sql-database/sql-database-service-tiers.md)。

- **启用 SQL 数据库的审核。** 审核可以用于诊断恶意攻击或人为错误。 有关详细信息，请参阅[开始使用 SQL 数据库的审核](../sql-database/sql-database-auditing-get-started.md)。 

- **使用活动的地理复制**活动的各地区复制用于创建不同区域中的可读的辅助。  如果主数据库出现故障，或者只需将离线，请执行手动故障切换到辅助数据库。  直到故障转移时，辅助数据库保持只读。  有关详细信息，请参阅[SQL 数据库活动地区的复制](../sql-database/sql-database-geo-replication-overview.md)。 

- **使用分片**。 请考虑使用分片对数据库进行水平分区。 分片可以提供故障隔离。 有关详细信息，请参阅[使用 SQL Azure 数据库扩展](../sql-database/sql-database-elastic-scale-introduction.md)。 

- **使用时间点还原从人为错误进行恢复。**  时间点还原的时间您数据库返回到早些时候。 有关详细信息，请参阅[使用自动的数据库备份 SQL Azure 数据库恢复][sql-restore]。

- **使用地区恢复来恢复从服务中断。** 地区恢复从地理冗余备份还原数据库。  有关详细信息，请参阅[使用自动的数据库备份 SQL Azure 数据库恢复][sql-restore]。


###  <a name="sql-server-running-in-a-vm"></a>SQL Server （在虚拟机中运行）

- **复制数据库。** 使用 SQL Server 总是在可用性组复制数据库。 如果一个 SQL Server 实例失败，可提供高可用性。 有关详细信息，请参阅[详细信息...](guidance-compute-n-tier-vm.md) 

- **备份数据库**。 如果您已经在使用[Azure 备份](https://azure.microsoft.com/documentation/services/backup/)来备份您的 Vm，请考虑使用[SQL Server 工作负载使用 DPM 的 Azure 备份](../backup/backup-azure-backup-sql.md)。 使用此方法时，没有组织的一个备份管理员角色和用于虚拟机和 SQL Server 的统一的恢复过程。 否则，请使用[SQL Server 管理备份到 Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)。 


###  <a name="traffic-manager"></a>通信管理器 

- **执行手动回切。** 通信管理器故障转移后，执行手动回切，而不是自动故障恢复。 回切之前, 验证所有的应用程序子系统的正常运行。  否则，您可以创建在其中应用翻转前后数据中心之间的情况。 有关详细信息，请参阅[在多个区域中运行虚拟机](guidance-compute-multiple-datacenters.md)。

- **创建健康探测器终结点**。 创建自定义的终结点的应用程序的整体健康状况报告。 这使得通信管理器故障转移，如果任何关键路径出现故障，而不仅仅是前端。 如果任何关键的相关性是不健全或无法到达，终结点应返回的 HTTP 错误代码。 不报告错误对于非关键的服务，但是。 否则，健康探测可能触发故障转移，它不需要时，产生误报。 有关详细信息，请参阅[通信量管理器终结点监控和故障转移](../traffic-manager/traffic-manager-monitoring.md)。


###  <a name="virtual-machines"></a>虚拟机 

- **避免在单个虚拟机上运行生产工作负载。** 一个 VM 部署不适应计划内或计划外的维护。 相反，置于多个虚拟机的可用性组或 VM 扩展集，使用负载平衡器在前。 为了符合 SLA 时，至少两个虚拟机必须部署在同一可用性组。 有关详细信息，请参阅[虚拟机比例设置概述](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)。 

- **指定设置当您配置虚拟机的可用性。** 目前，没有方法来将一个资源管理器的虚拟机添加到后 VM 配置设置的可用性。 当您添加新的虚拟机，对现有的可用性设置确保网卡创建虚拟机，而到负载平衡器上的后端地址池中添加网卡。 否则，负载平衡器不会将网络流量路由到该 VM。 

- **将每个应用程序层放入单独的可用性设置。** 在 N 层应用程序，不进入虚拟机从不同的层相同的可用性设置。 虚拟机的可用性组中故障以至 (FDs) 放置并更新域 (UD)。 但是，若要获取 FDs 和 UDs 的冗余好处，可用性组中的每个虚拟机必须能够处理相同客户端请求。 

- **选择适当的 VM 大小根据性能要求。** 当移动到 Azure 的现有的工作负荷，开头是最接近的匹配到您的内部服务器的虚拟机大小。 然后测量相对于 CPU、 内存和磁盘 IOPS，您实际工作负荷的性能，并根据需要调整大小。 这有助于确保应用程序的行为像预期的那样在云环境中。 此外，如果您需要多个 Nic，请注意每个尺寸的 NIC 限制。 

- **使用高级存储 Vhd。** Azure 的最优存储提供了高性能、 低延迟磁盘支持。 有关详细信息，请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](../storage/storage-premium-storage.md)选择支持高级存储的虚拟机大小。 

- **为每个虚拟机创建一个单独的存储帐户。** 对于一个 VM 将 Vhd，放在单独的存储帐户。 这有助于避免达到存储帐户的 IOPS 限制。 有关详细信息，请参阅[Azure 存储可扩展性和性能目标](../storage/storage-scalability-targets.md)。 但是，如果您正在部署大量虚拟机，请注意存储帐户的每个订阅限制。 请参阅[存储限制](../azure-subscription-service-limits.md#storage-limits)。

- **创建单独的存储帐户的诊断日志**。 将诊断日志写到 Vhd，以避免诊断日志记录的存储帐户并不影响虚拟机磁盘 IOPS。 标准的本地冗余存储 (LRS) 帐户是不足以诊断日志。 

- **数据在磁盘上，而操作系统磁盘安装应用程序。** 否则，可能会达到磁盘大小限制。 

- **使用 Azure 备份来备份虚拟机。** 备份是防止数据意外丢失。 有关详细信息，请参阅[保护恢复服务存储库的 Azure 虚拟机](../backup/backup-azure-vms-first-look-arm.md)。

- **启用诊断日志**，包括基本健康指标、 基础结构日志和[引导诊断][boot-diagnostics]。 启动诊断程序可帮助您诊断启动故障，如果您的虚拟机进入非可引导状态。 有关详细信息，请参阅[概述的 Azure 诊断日志][diagnostics-logs]。

- **使用 AzureLogCollector 扩展名**。 (仅在 Windows Vm 中。)此扩展聚合 Azure 平台日志并将它们上载到 Azure 存储中，而无需远程登录到虚拟机的操作员。 有关详细信息，请参阅[AzureLogCollector 扩展名](../virtual-machines/virtual-machines-windows-log-collector-extension.md)。


###  <a name="virtual-network"></a>虚拟网络 

- **白名单或块公用 IP 地址，添加到子网的 NSG。** 阻止来自恶意用户的访问权限或允许仅从具有权限才能访问该应用程序的用户访问。  

- **创建自定义运行状况探测器。** 负载平衡器运行状况探测器可以测试 HTTP 或 TCP。 虚拟机运行 HTTP 服务器，如果 HTTP 探测器将是比 TCP 探测器更好的健康状态指示器。 对于 HTTP 探测器，使用自定义的终结点所报告的应用程序，包括所有重要的依存关系整体的健康状况。 有关详细信息，请参阅[Azure 负载平衡器概述](../load-balancer/load-balancer-overview.md)。

- **不要阻止健康探测器。** 负载平衡器健康探测来自已知的 IP 地址 168.63.129.16。 不要阻止向或从任何防火墙策略或网络安全组 (NSG) 规则中此 IP 通信。 阻止健康探测将导致从旋转删除虚拟机的负载平衡器。 

- **启用负载平衡器日志记录。** 日志显示在后端上多少虚拟机未受到由于故障的探测器响应的网络流量。 有关详细信息，请参阅[Azure 负载平衡器的日志分析](../load-balancer/load-balancer-monitor-log.md)。


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
