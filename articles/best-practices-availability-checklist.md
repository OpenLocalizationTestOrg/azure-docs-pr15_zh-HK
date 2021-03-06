<properties
   pageTitle="可用性检查表 |Microsoft Azure"
   description="提供用于可用性考虑期间设计指南的清单。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="masimms"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="availability-checklist"></a>可用性检查表

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="application-design"></a>应用程序设计

- **避免任何单点故障。** 应为多个实例，以避免单点故障影响可用性部署所有的组件、 服务、 资源和计算实例。 这包括身份验证机制。 应用程序配置为使用多个实例，来自动检测故障，并请求重定向到未发生故障的实例，其中平台不满足该条件自动设计。
- **分解每个不同的服务级别协议工作负荷。** 如果服务组成的关键并不太重要的工作负载，以不同的方式对它们进行管理，并指定服务功能，以满足其可用性要求的实例数。
- **最小化，并了解服务依存关系。** 在可能的情况下，使用不同服务的数目减至最少并确保您了解所有的功能和服务的依赖关系，存在于系统中。 这包括这些依赖项的性质和故障的影响或在每个应用程序的总体上的性能下降。 Microsoft 在至少保证 99.9%的可用性对于大多数服务，但这意味着，每一个附加服务的应用程序可能依赖减少 SLA 为您的系统的整体可用性 0.1%。
- **设计任务和消息，以便在可能的情况是等幂 （安全可重复）**，因此，重复的请求不会造成问题。 例如，服务可以作为使用者处理由系统的其他部分作为生产者的作用，作为请求发送的消息。 如果使用者失败之后处理该邮件，但在确认它已被处理之前，制造者可能提交重复的请求未能处理使用者的另一个实例。 出于此原因，使用者和它们执行的操作应该是幂等的以便重复先前执行的操作不会呈现结果无效。 这可能意味着检测重复的邮件，或确保通过使用一种乐观的处理冲突方法的一致性。
- **使用实现高可用性的关键交易记录的消息代理。** 启动任务或访问远程服务的很多情况下使用消息传递指令之间的应用程序和目标服务。 为了获得最佳性能，应用程序应该能够发送该邮件，然后返回来处理更多请求，而无需等待答复。 要保证传递的邮件，邮件系统应提供高可用性。 Azure 服务总线消息队列实现了_至少一次_语义。 这意味着每个发送到队列的消息将不会丢失，虽然可能在某些情况下传递复制副本。 如果消息处理是幂等 （见前一项），重复传递应该不会有问题。
- **设计应用程序以妥善降级**时达到资源限制，并采取适当行动以用户的影响降到最低。 在某些情况下，应用程序的负载可能会超出容量的一个或多个部件，导致减少可用性和失败的连接。 缩放可以帮助来解决这问题，但它可能会达到资源可用性或成本等其他因素，限制。 设计应用程序，以便在此情况下，它可以自动性能平稳降低。 例如，在电子商务系统中，如果订单处理子系统是在压力下 （或甚至完全失败），可以暂时禁用它同时允许其他功能 （例如浏览产品目录） 继续。 它可能会延期到故障子系统，例如仍使客户能够提交订单，但订单子系统重新可用时进行后续处理，保存的请求。
- **妥善处理快速的突发事件。** 大多数应用程序需要处理不同的工作负荷，通过时间，例如上午在业务应用程序中或在电子商务网站发布了一个新的产品中的第一件工作的高峰期。 自动调整可以帮助处理的负载，但它可能需要一些时间的联机状态并处理请求的更多实例。 绝大多数的应用程序阻止突然和意外突发的活动︰ 向队列使用的服务请求对其进行设计和性能平稳降低队列时接近全产能。 确保有足够的性能和容量耗尽队列并处理未处理的请求的非突发情况下可用。 有关详细信息，请参阅[基于队列的负载调配操作模式](https://msdn.microsoft.com/library/dn589783.aspx)。

## <a name="deployment-and-maintenance"></a>部署和维护

- **部署的每个服务的角色的多个实例。** Microsoft 使可用性保证的服务创建和部署，但这些保证只有有效部署至少两个服务中的每个角色。 这样一个角色是不可用的而另将保持活动状态。 这一点尤其重要，如果您需要将更新部署到实际系统中，而不会中断客户端的活动;可以停机并同时其他联机继续分别升级实例。
- **在多个数据中心的主机应用程序。** 虽然极不可能的很可能整个数据中心的事件，如自然灾害或互联网故障通过脱机。 应该将重要的业务应用程序承载多个数据中心，以提供最大可用性。 这还可以减少延迟对于本地用户，并在更新应用程序时提供更多机会的灵活性。
- **自动化和测试部署和维护任务。** 分布式应用程序由多个部分组成必须一起工作。 部署应因此实现自动化，使用经测试和验证机制，例如脚本和部署应用程序。 这些可以更新和验证配置，并自动执行部署过程。 此外应使用自动化的技术执行的全部或部分应用程序更新。 关键是要测试所有这些进程，以确保错误不会导致额外的停机时间。 所有部署工具必须都有合适的安全限制，以保护已部署的应用程序;定义和认真实施部署策略并减少对人为干预的需要。
- **请考虑使用临时和生产平台的功能**在其中都有这些服务。 例如，使用 Azure 云服务临时和生产环境允许应用程序从一个到另一个虚拟的 IP 地址替换 （VIP 交换） 通过立即切换。 但是，如果您愿意转移场所，或并发部署不同版本的应用程序并逐渐迁移的用户，您可能无法使用 VIP 交换操作。
- **应用配置更改，而无需回收**实例在可能的情况。 在许多情况下，Azure 应用程序或服务的配置设置可以更改而无需重新启动的角色。 角色公开检测到配置更改并将其应用到应用程序中的组件可以处理的事件。 但是，对核心平台设置的某些更改需要重新启动的角色。 构建时组件和服务，最大化可用性，并减少宕机的设计他们接受而无需重新启动整个应用程序的配置设置的更改。
- **在更新过程中使用零停机时间升级域。** Azure 计算单位如 web 和辅助角色分配用于升级域。 升级域组角色实例组合在一起，以便升级域中的每个角色进行滚动更新时，将停止、 更新，并且又重新启动。 这最小化了对应用程序可用性的影响。 您可以指定部署服务时，应为服务创建多少升级域。

    > [AZURE.NOTE] 角色还分布故障域，是每个从其他故障域服务器机架、 电源和冷却资源调配，以便将影响所有角色实例出现故障的几率降至最低方面相当独立。 这种分布自动发生，并不能对其进行控制。

- **配置 Azure 的虚拟机的可用性设置。** 将两个或多个虚拟机放置在同一可用性组保证这些虚拟机不会部署到同一个容错域。 要最大化可用性，应创建的每个关键系统所用的虚拟机的多个实例，并将这些实例放在同一可用性组。 如果正在运行多个虚拟机的用途不同，创建设置为每个虚拟机的可用性。 将每个虚拟机的实例添加到每个可用性设置。 例如，如果您创建了单独的虚拟机作为 web 服务器和报告服务器，创建设置为 web 服务器的可用性和另一个设置为报表服务器的可用性。 添加 web 服务器可用性的 web 服务器虚拟机实例设置，并将报表服务器虚拟机的实例添加到报表服务器的可用性设置。

## <a name="data-management"></a>数据管理

- 通过本地和地理冗余的**利用数据复制**。 在 Azure 存储中的数据自动复制可以防止丢失基础设施故障时，可以配置此复制的某些方面。 例如，可能在多个地理区域 （称为读访问全局冗余存储或 RA GRS） 中复制数据的只读副本。 请注意，使用 RA GRS 招致额外的费用。 有关详细信息，请参阅[Azure 存储定价](https://azure.microsoft.com/pricing/details/storage/)。
- **使用开放式并发并最终一致性**在可能的情况。 交易记录块访问资源通过锁定 （保守式并发） 可以导致性能降低并显著降低可用性。 这些问题可能会变得非常棘手，在分布式系统。 在许多情况下，精心设计和技术，如分区可以尽量减少发生的更新冲突的可能性。 在进行复制时，或单独更新存储区中读取数据，数据才会最终一致。 但优点通常远远超过对使用交易记录以确保即时一致性的可用性的影响。
- **使用定期备份和时间点恢复**，并确保它满足恢复点目标 (RPO)。 定期自动备份了数据，则不会保留在其他地方，并验证您可以可靠地恢复数据和应用程序本身发生故障。 由于错误和不一致性引入故障、 错误或恶意操作通过将复制所有商店的数据复制不是备份的功能。 备份过程必须是安全的来保护数据在传输过程中和存储。 数据库或数据存储区的部分可以通常会恢复到以前的点时间通过使用事务日志。 Microsoft Azure 提供了一个备份工具，以便 SQL Azure 数据库中存储的数据。 数据导出到 Azure 的 blob 存储在备份包，并且可以下载到一个安全的内部部署位置进行存储。
- **启用高可用性选项来维护 Azure Redis 的缓存的辅助拷贝。** 当使用 Azure Redis 高速缓存时，选择标准维护内容的次要副本。 有关详细信息，请参阅[创建 Redis Azure 的高速缓存中的缓存](https://msdn.microsoft.com/library/dn690516.aspx)。

## <a name="errors-and-failures"></a>错误和失败

- **引入概念的超时。** 服务和资源可能不可用，导致请求失败。 确保您应用了超时都适合于每个服务或资源以及正在访问它们的客户端。 （在某些情况下，它可能适当允许较长超时的特定实例的客户端，具体取决于上下文和客户端执行其他操作。非常短的超时可能会导致过多的重试操作的服务和资源，它们具有相当长的滞后时间。 很长时间的超时会阻止如果大量请求进行排队，等待服务或资源来作出响应。
- **重试失败的操作导致的暂时性错误。** 设计用于访问所有的服务和资源，它们本身不支持自动连接重试重试策略。 使用一种战略，包括提高故障也随之增长，以防止重载的资源并使其平稳恢复，并处理排队的请求数次重试之间延迟。 不断重试，用很短的延迟都有可能加剧问题。
- **停止发送请求以避免级联故障**远程服务时不可用。 可能情况下的瞬时或其他故障，取值范围为从部分连接丢失服务，完成故障的严重性需要较长时间比预期的要返回到正常。 此外，如果服务是非常繁忙的系统的一个部分中的故障可能导致级联故障，和导致许多操作同时拿到关键的系统资源，如内存、 线程和数据库连接上阻塞。 而不是不断重试的操作，则不太可能成功，应用程序应迅速接受操作已失败，并妥善处理这种失败。 断路器模式可用于拒绝针对特定操作的定义的期间的请求。 有关详细信息，请参阅[断路器模式](https://msdn.microsoft.com/library/dn589784.aspx)。
- **撰写或回退到多个组件**以减少脱机或不可用的特定服务的影响。 设计应用程序，以尽可能充分利用现有连接而影响操作的多个实例。 使用多个实例和分发它们之间的请求和检测和避免将请求发送给失败的实例，以便最大限度。
- **回退到不同的服务或工作流**在可能的情况。 例如，如果写入 SQL 数据库出现故障，临时存储数据 blob 存储中。 提供设施重放在 blob 存储到 SQL 数据库中写入的当该服务可用时。 在某些情况下，操作失败可能使应用程序能够继续工作甚至组件或服务失败时的备选操作。 如果可能，请检测故障，重定向请求到可以提供合适的替代功能，其他服务或备份或缩减的功能主要服务处于脱机状态时可以维护核心操作的实例。

## <a name="monitoring-and-disaster-recovery"></a>监视和灾难恢复

- **提供丰富的检测可能失败和失败事件的**报告到操作人员的情况。 失败的可能但还未出现，提供足够的数据以使操作人员可以确定原因，缓解这种情况，并确保系统仍然可用。 已发生的故障，应用程序应该向用户返回相应的错误消息，但尝试继续运行，但功能受到限制。 监视系统应该在所有情况下，捕获全面的详细信息，以使操作人员可以有效的快速恢复，并且如有必要，为设计人员和开发人员可以修改系统可以防止再次因种情况。
- **通过实施检查函数监控系统运行状况。** 运行状况和性能的应用程序会降低随着时间的推移而直到它失败明显了。 实现探测器或检查执行定期从外部应用程序的功能。 这些检查可以很简单，作为测量响应时间作为一个整体的应用程序、 应用程序的各个部分、 各个服务的应用程序使用，或单个组件。 检查函数可执行流程，以确保产生有效的结果，测量延迟时间和检查可用性，并从系统中提取信息。
- **定期测试所有故障转移和后备系统**，以确保它们是可用的按预期方式运行。 对系统和操作的更改可能会影响故障转移或回退功能，但直到主系统出现故障或变得过载，造成的影响可能不会检测。 测试之前需要弥补它在运行时的实时问题。
- **测试监控系统。** 自动故障切换和后备系统，手动可视化的系统运行状况和性能，通过使用仪表板，都依赖于监视和检测正常。 如果这些元素失败，未命中关键信息或报告不准确的数据，操作员可能并未意识到系统的运行不正常或出现故障。
- **跟踪进度的长时间运行的工作流**和失败时重试。 长时间运行的工作流通常由多个步骤组成。 确保每个步骤是独立的并且可以重试要尽可能的整个工作流需要回滚，或者需要执行多个补偿事务。 监视和管理长时间运行的工作流的进度实现[计划代理主管模式](https://msdn.microsoft.com/library/dn589780.aspx)等模式。
- **制定灾难恢复计划。** 确保没有任何类型的故障，可能导致部分或全部的主系统恢复的有案可稽、 接受，并充分测试计划不可用。 定期测试过程，并确保所有的操作人员熟悉过程。
