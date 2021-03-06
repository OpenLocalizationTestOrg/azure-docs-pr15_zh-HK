<properties
   pageTitle="重试的一般指南 |Microsoft Azure"
   description="重试的瞬时性故障处理指南。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
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

# <a name="retry-general-guidance"></a>重试的一般指南

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>概述

所有与远程服务和资源进行通信的应用程序必须对瞬时故障敏感。 这是应用程序运行在云中，其中环境和通过 Internet 连接的性质意味着这些类型的错误都可能更经常遇到的情况尤其如此。 瞬时故障包括瞬时丢失网络连接的组件和服务、 临时不可用的服务或超时发生时服务正忙。 这些错误通常会自我更正，并操作重复适用延迟后很有可能成功。

本文介绍了瞬态故障处理的一般准则。 有关使用 Microsoft Azure 服务时处理瞬时故障的信息，请参阅[Azure 服务特定重试准则](best-practices-retry-service-specific.md)。

## <a name="why-do-transient-faults-occur-in-the-cloud"></a>为什么瞬时故障发生在云中

瞬时故障可以在任何环境中，在任何平台或操作系统，和任何类型的应用程序中发生。 在解决方案上运行本地、 内部部署基础结构、 性能和应用程序及其组件的可用性通常通过维护成本高昂而且通常在使用硬件冗余和组件和资源都位于接近每另一个。 虽然这对出现故障的可能性较小，它仍然会被瞬时故障-和甚至中断时无法预见的事件，如外接电源或网络问题或其他灾难方案通过。

云托管，包括私有云系统，可以提供更高的总体可用性使用共享的资源、 冗余、 自动故障切换和动态资源分配，通过大量的商品计算节点。 不过，这些环境的性质可以意味着瞬时故障是更有可能发生。 有几个原因︰

* 在云环境中的许多资源共享的并对这些资源的访问受到限制来保护资源。 某些服务将拒绝连接，当负载上升到特定级别，或达到最大的吞吐率，以便处理现有请求和维护所有用户的服务的性能。 带宽限制有助于维护的邻居和其他承租人使用共享的资源的服务质量。
* 使用大量的商品硬件单元构建云环境。 他们通过动态分配负载，跨多个计算单位和基础结构组件，提供的性能，并提供自动回收或更换出现故障的设备的可靠性。 这种动态性质意味着可能偶尔发生瞬时故障和临时连接失败。
* 经常有多个硬件组件，包括网络基础设施，如路由器和负载平衡器，应用程序和资源和使用的服务之间。 此附加的基础结构有时会带来附加的连接延迟和瞬态连接故障。
* 客户端和服务器之间的网络条件可能变量，尤其是当通信交叉互联网。 即使在内部部署的位置，非常繁忙的流量负载可能降低通信并导致间歇性连接失败。

## <a name="challenges"></a>面临的挑战
瞬时故障会对非常大的影响感知的可用性的应用程序，即使已在所有可预见情况下全面测试。 为确保云托管应用程序可靠地工作，他们必须能够响应以下挑战︰

* 应用程序必须能够检测出故障，当它们发生，并确定是否这些故障很可能是暂时性的、 更长久，或者终端故障。 不同的资源很可能会返回不同的响应，当故障发生时，和这些响应可能还会因上下文的操作;例如，从存储中读取时出现错误的响应可能会有所不同从错误的响应写入存储时。 很多的资源和服务具有完备的瞬时性故障合同。 但是，此类信息不可用，则可能很难发现的故障，以及是否有可能是暂时性质。
* 应用程序必须能够确定故障很可能是暂时的并跟踪操作的重试次数重试该操作。
* 应用程序必须使用适当的战略以重试。 此策略指定的次数应重，尝试失败后采取的每次尝试中和操作之间的延迟。 相应数量的尝试和每个之间的延迟往往难以确定，并根据资源的类型，以及资源和应用程序本身的当前运行情况而有所变化。

## <a name="general-guidelines"></a>一般原则
以下指南将帮助您设计适合瞬时故障处理机制，用于您的应用程序︰

* **确定是否有内置的重试机制︰**
  * 许多服务提供 SDK 或客户端库，它包含瞬态错误处理机制。 它使用的重试策略通常被定制的性质和目标服务的要求。 另外，服务的 REST 接口可能会返回它可用于确定是否重试是适当的并在下一次重试尝试之前等待的时间长度的信息。
  * 使用内置的重试机制，其中一个是可用，除非您有具体和易于理解的要求，意味着不同的重试行为是更合适。
* **确定操作是否适用于重试**︰
  * 其中错误是瞬态的 （通常由错误的性质），并且至少一些操作时重新尝试成功的可能性仅重试操作。 在重试操作指示无效的操作，例如数据库中没有点对不存在的项目的更新或请求到服务或遭到致命错误的资源
  * 一般情况下，您应该实现重试次数只有其中可以确定此的全面影响，和条件都很好地理解和可验证。 如果没有，则将其保留为调用代码来实现重试次数。 请记住，从资源返回的错误和控件之外的服务可能会随着时间，您可能需要重新考虑您的瞬时性故障检测逻辑。
  * 在创建服务或组件时，请考虑实现错误代码和消息，以帮助客户确定他们是否应该重试失败的操作。 特别是，表明是否客户端应重试操作，（可能是通过返回一个**isTransient**值） 和建议下一次重试尝试之前适当延迟。 如果生成的 web 服务时，请考虑返回在服务合同中定义的自定义错误。 即使通用客户端可能无法读取这些，他们也会有用构建自定义客户端时。
* **确定适当的重试次数和间隔︰**
  * 关键是要优化的重试次数和间隔为用例的类型。 如果不进行重试的次数足够多，应用程序将无法完成此操作，可能会出现故障。 如果重试次数过多，或尝试之间的间隔太短，应用程序可能会产生负面影响的应用程序运行状况的长期容纳线程、 连接和内存等资源。
  * 适当的时间间隔和重试次数值取决于所尝试的操作类型。 例如，如果该操作是用户交互的一部分，间隔应较短且仅几次以免让用户等待的响应 （它保存打开的连接，并且可以降低对其他用户的可用性）。 如果操作是长时间运行或关键工作流的一部分，其中取消操作并重新启动该进程是昂贵或费时，最好两次尝试之间等待长时间，然后重试多次。
  * 确定适当的间隔重试之间是设计成功战略的最困难部分。 典型的策略使用以下类型的重试间隔︰
      * **指数后关闭**。 应用程序在等待片刻之前首次重试，然后再以指数级增加每个后续重试之间的时间。 例如，它可能会重试该操作后 3 秒，12 秒，30 秒，等等。
      * **增量间隔**。 应用程序在等待片刻之前首次重试，然后再逐渐递增的每个后续重试之间的时间。 例如，它可能会重试该操作后 3 秒、 7 秒、 13 秒，等等。
      * **规律的时间间隔**。 应用程序将等待相同的每次尝试之间的时间段。 例如，它可能会重试此操作每隔 3 秒。
      * **即时重试**。 有时瞬时性故障是很短，可能是由事件例如网络数据包冲突或硬件组件中的出现高峰。 在这种情况下，立即重试该操作是适当的因为它可能会成功，如果故障已清除应用程序装配并发送下一个请求所花费的时间。 但是，应该永远不会有多个立即重试尝试，并且您应该切换到备用策略，如指数后关闭或回退操作，如果立即重试失败。
      * **随机选择**。 重试策略上面列出的任何可能包括随机选择以防止发送后续重试尝试一次的客户端的多个实例。 例如，一个实例可以重试后 3 秒、 11 秒，28 秒，等另一个实例可以重试后 4 秒，12 秒，26 秒，操作时操作，等等。 随机选择是有用的技术，可能结合其他策略。  
  * 一般原则是，使用后台操作和交互式操作的即时或常规时间间隔重试策略指数后关闭策略。 在这两种情况下，您应选择延迟和重试次数，以便为所有的最大延迟重试次数是内所需的端到端延迟要求。
  * 考虑到所有因素构成的最大总超时重试操作的组合。 这些因素包括失败连接起来 （通常由客户端的超时值设置） 的响应所需的时间以及重试次数和重试最大次数之间的延迟。 所有这些时间的总和可能会导致非常大的整个操作时间，尤其是当使用指数延迟策略之间的时间间隔重试次数迅速增长在每次失败后。 如果进程必须满足特定的服务级别协议 (SLA)，总体的操作时间，包括所有超时和延迟的影响，必须在该定义 SLA 中
  * Over-aggressive 重试策略，它有间隔太短或太可能重试次数，可以有负面影响的目标资源或服务。 这可能会阻止资源或服务恢复从超负荷状态，并将继续为阻止或拒绝请求。 这导致恶性循环，越来越多的请求被发送到资源或服务，因而它能够恢复也会进一步降低。
  * 选择以避免启动立即 （例如，如果在超时期间相似的重试间隔） 的后续尝试的重试时间间隔时，考虑到操作的超时。 此外应考虑是否需要保留总可能段 （超时和重试间隔） 到以下特定的总时间。 有非常短或很长超时的操作会影响多少时间来等待，以及频率重试该操作。
  * 使用异常和它所包含的任何数据类型或服务，以优化时间间隔和次数返回的错误代码和消息。 例如，某些异常或错误代码 （例如，HTTP 代码重试后标头在响应中的 503 服务不可用） 可能表明该错误可能会持续多长时间，或该服务失败，将不响应任何后续的尝试。
* **避免反模式**︰
  * 在绝大多数情况下，应避免包括的代码重试复制图层的实现。 这避免的设计，包括级联的重试机制，或除非您有特定的需求要求实现重试的操作的请求，层次结构的每个阶段。 这些例外情况中，使用的策略，防止过多的重试次数和延迟时间，请确保您了解其后果。 例如，如果一个组件发出请求到另一个时，针对服务的总目标服务，并实现上都有两个调用三个计数重试哪然后访问九重试尝试。 许多服务和资源实现内置的重试机制，应研究如何禁用或修改此，如果您需要实现更高的级别的重试次数。
  *  永远不会实现无限重试机制。 这可能会阻止从重载情况下，恢复服务的资源，从而使带宽限制和拒绝连接继续更长一段。 使用有限或重试，或者实现如[断路器](http://msdn.microsoft.com/library/dn589784.aspx)，以允许服务恢复模式。
  * 永远不会多次执行立即重试。
  * 避免使用普通的重试间隔，尤其是当您有大量的重试次数时访问服务和 Azure 中的资源。 最佳做法是这种情况下是指数后关闭策略与电路换行功能。
  * 防止多个实例的相同的客户端或从在同一时间发送重试次数的不同客户端的多个实例。 如果这是有可能发生，引入随机入重试间隔。
* **测试您重试策略和实施︰**
  * 请确保您完全测试下您重试战略实施为宽一组的情况下，尤其是当同时应用程序和目标资源或服务使用是极端负载的情况下。 要检查在测试过程中的行为，您可以︰
      * 瞬态和非瞬态故障注入服务。 例如，发送无效请求或添加检测测试请求，并与不同类型的错误做出响应的代码。 示例使用 TestApi，请参阅[故障注入测试与 TestApi](http://msdn.microsoft.com/magazine/ff898404.aspx)和[简介 TestApi-5 部分︰ 托管代码故障注入 Api](http://blogs.msdn.com/b/ivo_manolov/archive/2009/11/25/9928447.aspx)。
      * 创建资源或返回实际服务可能返回的错误范围服务的模型。 确保涵盖所有类型的错误，重试策略旨在检测。
      * 强制通过暂时禁用或重载该服务创建和部署自定义服务时出现暂时性错误 （您应该不是这样，当然，尝试重载任何共享的资源，或共享服务在 Azure 中的）。
      * 基于 HTTP 的 Api，请考虑使用自动测试中的 FiddlerCore 库更改 HTTP 请求的结果，通过添加额外的往返时间，或通过更改 （如 HTTP 状态代码、 页眉、 正文或其他因素） 的响应。 这使确定性测试子集的故障条件，是否暂时故障或其他类型的故障。 有关详细信息，请参阅[FiddlerCore](http://www.telerik.com/fiddler/fiddlercore)。 有关如何使用库，特别是**HttpMangler**类的示例检查[Azure 存储 SDK 的源代码](https://github.com/Azure/azure-storage-net/tree/master/Test)。
      * 执行高负载系数和并发测试，以确保重试机制，以及在这些情况下正确的战略工作并没有对客户端的操作造成不利影响或导致请求之间的交叉污染。
* **管理重试策略配置︰**
  * _重试策略_是所有的重试策略元素的组合。 它定义了确定是否可能是暂时的故障检测机制的使用 （如常规、 指数后休息、 和随机选择），实际的时间间隔值，并数次重试的时间间隔类型。
  * 即使最简单的应用中的很多地方和更复杂的应用程序的每一层中，必须实现重试次数。 而不是硬编码在多个位置的每个策略元素，请考虑使用存储的所有策略的中心点。 例如，存储等时间间隔的值和重试计数应用程序配置文件中的，阅读这些内容在运行时，并以编程方式生成重试策略。 这使得更容易管理的设置，并进行修改和微调值以响应不断变化的要求和方案。 但是，如果不能从配置中获取值，则使用设计系统中存储的值而不是重新读取配置文件中每个时间，并确保适当的默认值。
  * 在 Azure 云服务应用程序，请考虑用于生成在运行时的服务配置文件中的重试策略，以便以后能够进行改动而无需重新启动应用程序的值。
  * 利用内置的或默认可用客户端 Api 使用，但只在它们都适合您的方案中的重试策略。 这些战略是通常一般用途。 在某些情况下它们可能只是必需的但在其他情况下它们不能提供全部的选项以满足您的特定要求。 您必须了解如何设置将影响您的应用程序通过测试来确定最合适的值。
* **记录和跟踪瞬态和非瞬态错误︰**
  * 重试策略的一部分，包括异常处理和日志时重试尝试其他检测。 尽管偶尔的瞬时性故障和重试都是意料之中，并不表示问题、 定期和不断增加的重试次数数通常是指示器，可能导致失败，也当前影响应用程序性能和可用性的问题。
  * 瞬时故障记录为警告的条目，而不是错误条目，以便监控系统不能发现其作为应用程序错误，可能会触发误报。
  * 考虑在您表明，是否重试次数已导致通过调节中的服务，或其他类型的故障如连接失败，以便可以在数据的分析过程中区分它们的日志条目中存储一个值。 限制错误数的增加通常是设计存在缺陷，需要切换到一种高级服务提供专用的硬件或应用程序中的指示器。  
  * 请考虑测量和记录包括重试机制的操作所需的总体时间。 这对用户的响应时间、 流程滞后和应用程序用例的效率是瞬时故障的整体效果的良好指标。 此外日志的重试次数出现为了了解参与了响应时间的因素。
  * 请考虑实施遥测和监控系统，可以引发警报时的数字和失败的速率、 平均次数，或操作成功，所需的总体时间越来越多。
* **管理不断失败的操作︰**
  * 将其中操作将继续在每次尝试，失败，关键是要考虑将如何处理这种情况下的情况下︰
      * 虽然重试策略将定义操作应重试最大次数，但并不妨碍具有相同数量的重试次数重复该操作，该应用程序。 例如，如果订单处理服务失败，永久放置操作一个致命错误，重试策略可能检测连接超时，并将其视为瞬时性故障。 代码将重试此操作指定的次数，然后放弃。 但是，当另一个客户下订单，在操作时将再次进行-即使它是确保每一次会失败。
      * 为防止持续不断失败的操作重试次数，请考虑实现[断路器模式](http://msdn.microsoft.com/library/dn589784.aspx)。 在此模式中，如果在指定的时间窗口内失败的次数超过了阈值，请求将返回到调用方立即为错误，而不尝试访问发生故障的资源或服务。
      * 应用程序可以定期测试间歇性地并具有很长间隔请求，来检测变得可用时的服务。 一个适当的时间间隔将取决于该方案，如操作的重要程度和性质的服务，并且可能任何几分钟到几个小时之间。 时刻在其中测试成功，应用程序可以恢复正常运行，并将请求传递给新复原服务。
      * 同时，它可能会回退到另一个实例 （可能位于不同的数据中心或应用程序中） 的服务，使用的类似服务，提供兼容的 （也许更简单） 功能，或希望能很快变为可用服务执行一些其他操作。 例如，它可能适合存储服务的请求队列中或数据存储和稍后重播。 否则您可能无法将用户重定向到应用程序的其他实例，会降低应用程序的性能，但仍然提供了可接受的功能，或只是返回到指示的用户应用程序没有在一条消息显示。

* **其他考虑事项**
  * 在决定上的重试次数值和重试间隔的策略时，考虑如果服务或资源上的操作是在长时间运行或多步操作的一部分。 它可能会很困难或成本补偿的所有其他操作步骤已成功一种失败。 在这种情况下，在很长的时间间隔和大量的重试次数是可以接受，只要它不会阻止其他操作通过持有或锁定稀缺资源。
  * 请考虑是否重试此操作可能会导致不一致的数据。 如果有重复的多步进程的某些部分，并且操作不是幂等，可能会导致不一致的情况。 例如，值递增操作如果重复，会产生无效的结果。 重复将消息发送到队列的操作可能会导致不一致消息使用者如果不能检测到重复的邮件。 要防止出现这种情况，请确保您设计作为一个幂等操作的每一步。 关于幂等性的详细信息，请参阅[幂等性模式](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/)。
  * 请考虑将重试的操作范围。 例如，它可能容易实现重试代码级别包含多个操作，并且如果其中一个出现故障重试所有这些。 但是，这样做可能会导致幂等性问题或不必要的回滚操作。
  * 如果您选择包含多个操作的重试范围，考虑到所有这些在确定重试的时间间隔，在监视的时间，以及引发故障的警报前的总延迟时间。
  * 考虑您重试策略可能对影响邻居和其他承租人在共享的应用程序，或当使用共享的资源和服务。 雄心勃勃的重试策略可能会导致越来越多的瞬时故障发生这些其他用户和应用程序共享的资源和服务。 同样，实现资源和服务的其他用户重试策略可能会影响您的应用程序。 对于任务关键型应用程序，您可能决定使用它们不能共享的优质服务。 这提供了更多控制的负载和这些资源和服务，这有助于证明额外成本随之减速。

## <a name="more-information"></a>详细信息

* [Azure 服务特定重试准则](best-practices-retry-service-specific.md)
* [瞬时故障处理应用程序块](http://msdn.microsoft.com/library/hh680934.aspx)
* [断路器模式](http://msdn.microsoft.com/library/dn589784.aspx)
* [补偿事务模式](http://msdn.microsoft.com/library/dn589804.aspx)
* [幂等性模式](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/)
