<properties
   pageTitle="自动缩放指南 |Microsoft Azure"
   description="有关如何自动缩放比例来动态分配资源所需的应用程序的指南。"
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

# <a name="autoscaling-guidance"></a>自动缩放指南

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>概述
自动缩放时运行成本降至最低是动态分配的应用程序，以满足性能要求并满足服务级别协议 (Sla) 所需的资源的过程。 随着工作的量的增长，应用程序可能需要额外的资源，以使它能够及时地执行其任务。 如需 slackens，资源可以是解除分配，尽量减少成本，同时仍保持足够的性能和满足 sla 要求。
自动缩放采用弹性云托管环境的同时减少管理开销。 它是通过减少操作员能够不断地监视系统的性能，并决定有关添加或删除资源的需要。
>[AZURE.NOTE] 自动缩放适用于所有的应用程序，而不仅仅是计算资源使用的资源。 例如，如果您的系统使用消息队列来发送和接收信息，它可以创建其他队列进行缩放。

## <a name="types-of-scaling"></a>扩展类型
缩放比例通常采用以下两种形式之一︰

- **垂直**（通常称为_水平扩展向上和向下_）。 该表单要求您修改硬件 （扩大或缩小它的容量和性能），或重新部署解决方案使用具有适当的容量和性能的备用硬件。 在云环境中，硬件平台通常是虚拟化的环境。 除非原始硬件已明显过，与后续的前期资本支出，垂直向上扩展在此环境中涉及资源调配功能更强大的资源，并移动到这些新的资源系统。 垂直缩放比例通常是需要使系统暂时不可用，而重新部署它的破坏性进程。 它可能保留原系统运行，而新的硬件配置和在线，但有可能会到新旧环境中处理过渡时一些中断。 它是正常现象，使用自动缩放来实现垂直扩展战略。
- **水平**（通常称为_水平扩展出和中_）。 此窗体需要部署解决方案上更多或更少的资源，通常是商品资源，而不是几个高容量系统。 该解决方案可以继续运行而不中断调配这些资源。 资源调配过程完成后，可在以下这些附加资源上部署构成解决方案的元素的副本，并供。 如果需求下降，更多的资源可以回收后使用它们的元素具有已完全关闭。 许多基于云的系统，包括 Microsoft Azure 支持扩展这种形式的自动化。

## <a name="implement-an-autoscaling-strategy"></a>实施自动缩放战略
实施自动缩放战略通常包括以下组件和流程︰

- 检测并监视应用程序、 服务和基础结构级别的系统。 这些系统捕获关键指标，如响应时间、 队列长度、 CPU 利用率和内存使用情况。
- 决策的逻辑，可以评估被监视的比例因子对预定义的系统阈值或计划，并决定是否或不缩放。
- 负责执行任务与扩展的系统，如资源调配或消除资源调配的资源相关联的组件。
- 测试、 监控和调节的自动缩放战略，以确保其工作正常。

大多数基于云的环境，如 Azure，提供了内置的自动缩放机制该地址的常见方案。 如果您使用的服务或环境没有提供所需的自动缩放功能，或者您有超出其能力的极端自动缩放要求，可能需要一个自定义实现。 使用此自定义的实现收集操作和系统指标，分析他们找出相关数据，然后相应地调节资源。


## <a name="configure-autoscaling-for-an-azure-solution"></a>配置自动缩放 Azure 的解决方案
有几个选项用于配置自动缩放 Azure 的解决方案︰

- **Azure 自动缩放**支持最常见的扩展方案根据日程安排，并 （可选） 触发缩放操作基于运行时 （例如，处理器利用率、 队列长度、 或内置和自定义计数器） 的标准。 使用 Azure 的门户，可以快速而轻松地配置解决方案的简单自动缩放策略。 有关更多详细的控制，您可以使用[Azure 服务管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)或[资源管理器 REST API，Azure](https://msdn.microsoft.com//library/azure/dn790568.aspx)。 [Azure 监视服务管理库](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring)和[Microsoft 见解库](https://www.nuget.org/packages/Microsoft.Azure.Insights/)（在预览） 是允许从不同的资源收集度量标准的 Sdk 通过使用 REST Api 来执行自动缩放。 对于资源 Azure 资源管理器支持不可用，或者如果您正在使用 Azure 的云服务，服务管理 REST API，可以用于自动缩放。 在所有其他情况下，使用 Azure 资源管理器。
- **自定义解决方案**，基于您检测上的应用程序和 Azure，管理功能会很有用。 例如，可以在您的应用程序和自定义代码来持续监视和度量应用程序的导出中使用 Azure 诊断或其他的检测方法。 您可以让工作依据这些指标，并使用服务管理的自定义规则或 REST API，资源管理器的触发自动缩放。 触发的缩放操作的指标可以是任何内置或自定义计数器或您的应用程序中实现其他检测。 但是，自定义解决方案并不容易实现，并应将其视为仅是否任何以前的方法可以满足您的需求。 [自动缩放应用程序块](http://msdn.microsoft.com/library/hh680892%28v=pandp.50%29.aspx)使用这种方法。
- **第三方服务**，例如， [Paraleap AzureWatch](http://www.paraleap.com/AzureWatch)，使您能够扩展基于计划、 服务负载和系统性能指标、 自定义的规则和组合不同类型的规则的解决方案。

在选择采用哪种自动缩放解决方案时，请考虑以下几点︰

- 使用内置的自动缩放功能的平台，它们可满足您的需求。 如果没有，请仔细考虑是否您真正需要更复杂的扩展功能。 一些其他要求的示例可能包括更多粒度的控制，不同的方法来检测触发事件的缩放比例、 缩放整个订阅，和扩展其他类型的资源。
- 请考虑是否可以预测应用程序的负载足够精度仅取决于计划自动缩放 （添加和删除实例，以满足预期需求的高峰期）。 这不可能，请使用基于标准收集在运行时，允许应用程序处理请求中出现的突然变化的反应自动缩放。 通常情况下，您可以组合使用这些方法。 例如，创建添加资源，如计算、 存储和队列，根据当您知道应用程序是最忙碌的时间的计划战略。 这有助于确保容量需要，而不会启动新的实例时遇到延迟时可用。 此外，对于每个计划的规则，定义允许在该期间内，以确保应用程序可以处理的需求量也将持续，但无法预测峰值反应的自动缩放的指标。
- 很难理解指标和容量需求，尤其是在最初部署应用程序之间的关系。 愿意提供一些额外的容量，在开始时，然后监视和调整容量近置于实际负载的自动缩放规则。

### <a name="use-azure-autoscale"></a>使用 Azure 自动缩放比例
自动缩放比例使您能够配置向外的扩展和扩展解决方案的选项。 自动缩放比例可以自动添加和删除在 Azure 应用程序服务的 Azure 云服务 web 和辅助角色、 Azure 移动服务和 Web 应用程序功能的实例。 它还可自动按比例缩放通过启动和停止实例的 Azure 的虚拟机。 Azure 自动缩放的战略包括两个因素集︰

- 计划基于自动缩放，可确保更多实例可用来在用法中，预期峰值与重合，一旦过了高峰时间可以扩展中。 这使您可以确保您有足够的实例已在运行，而无需等待系统响应负载。
- 在过去一小时或订货的解决方案处理在 Azure 存储或 Azure 服务总线队列中的消息的平均 CPU 利用率等因素对反应的基于标准的缩放。 这使应用程序分别从定时自动缩放规则以适应计划外或不可预见的变化需求作出反应。

使用自动缩放时，请考虑以下几点︰

- 自动缩放战略结合了计划和基于标准的扩展。 您可以指定两种类型的服务的规则。
- 应该配置自动缩放的规则，然后监视应用程序性能的一段时间。 使用这种监视的结果来调整系统可在必要时扩展在其中的方式。 但是，请记住该自动缩放不是瞬时完成的过程。 它需要时间对平均 CPU 利用率超过 （或低于） 等指标做出反应指定的阈值。
- 自动缩放规则使用一个基于测量的触发器属性 （例如，CPU 使用情况或队列长度） 的检测机制的使用时间，而不是瞬时值，通过聚合的值触发自动缩放操作。 默认情况下，聚合是值的平均值。 这防止了系统反应太快，或导致快速振动。 它还允许将自动启动要结算到运行模式，防止发生启动新实例时的附加自动缩放操作的新实例的时间。 Azure 云服务和 Azure 的虚拟机，聚合的默认时间是 45 分钟，因此，可能要花费到这段时间度量来触发响应峰值需求的自动缩放。 您可以更改聚合期间通过使用 SDK，但请注意不超过 25 分钟的期间，可能会导致不可预知的结果 （有关详细信息，请参阅[自动缩放云服务上使用 Azure 监视服务管理库的 CPU 百分比](http://rickrainey.com/2013/12/15/auto-scaling-cloud-services-on-cpu-percentage-with-the-windows-azure-monitoring-services-management-library/)）。 对于 Web 应用程序，则平均周期是短得多，并允许新的实例，可以在大约 5 分钟后到平均触发度量值的更改。
- 如果您配置自动缩放使用 SDK，而不是 web 门户，您可以指定更详细的日程安排，其间规则处于活动状态。 您还可以创建您自己的度量标准并带有或不带任何现有的自动缩放规则中使用它们。 例如，您可能希望使用可选的计数器，例如每秒或平均内存可用性请求数或使用自定义计数器，用来测量特定的业务流程。
- 自动缩放 Azure 的虚拟机，您必须部署大量实例的虚拟机，它的最大数目等于将允许自动缩放以启动。 这种情况必须属于相同的可用性设置。 虚拟机自动缩放机制不会创建或删除实例的虚拟机;相反，您配置的自动缩放规则将启动和停止这些实例的适当数量。 有关详细信息，请参阅[自动缩放 Web 角色、 工作人员角色或虚拟机运行的应用程序](./cloud-services/cloud-services-how-to-scale.md)。
- 如果无法启动新的实例，可能是由于已达到的最大订阅或启动过程中，发生错误门户可能显示自动缩放操作已成功。 但是，随后显示在门户网站中的**ChangeDeploymentConfiguration**事件将仅显示该服务启动请求，但会有任何事件，它指示已成功完成。
- Web 门户用户界面可用于链接到计算服务实例的资源，例如 SQL 数据库实例和队列。 这使您可以更轻松地访问单独的手动和自动缩放每个链接的资源的配置选项。 有关详细信息，请参阅[方法︰ 将资源链接到云服务](cloud-services-how-to-manage.md#linkresources)，以及[如何扩展应用程序](./cloud-services/cloud-services-how-to-scale.md)。
- 在配置多个策略和规则时，它们会相互冲突。 自动缩放比例使用下面的冲突解决规则来确保是总是足够数量正在运行的实例︰
  - 缩放操作始终优先于在操作中的比例。
  - 当扩展操作冲突，启动的实例数的最大增加的规则优先。
  - 当缩放操作冲突，启动的实例数最小下降的规则优先。

<a name="the-azure-monitoring-services-management-library"></a>

## <a name="application-design-considerations-for-implementing-autoscaling"></a>用于实现自动缩放的应用程序设计注意事项
自动缩放不是即时的解决方案。 只是将资源添加到系统或运行流程的多个实例并不能保证系统的性能将提高。 设计一种自动缩放策略时，请考虑以下几点︰

- 系统必须能够进行水平扩展。 避免做出假设实例相似性;不设计需要始终进程的特定实例中运行这段代码的解决方案。 当水平缩放的云服务或网站，不要假设总是将被一系列来自相同来源的请求路由到相同的实例。 出于同样的原因，设计为了避免要求一系列请求从应用程序总是被路由到服务的同一个实例，无状态的服务。 在设计时从队列中读取消息并处理它们的服务，不作任何假设的服务句柄实例特定的消息。 随着队列长度自动缩放无法启动服务的其他实例。 [竞争使用者模式](http://msdn.microsoft.com/library/dn568101.aspx)描述如何处理这种情况。
- 如果该解决方案实现长时间运行的任务，设计此任务以支持横向扩展和扩展中。 而到期不小心，此任务可能会阻止流程实例正在关闭干净系统调整，或如果强行终止该进程，它也许会丢失数据。 理想情况下，重构一个长时间运行任务，并分解成更小的、 离散的块执行处理。 [管道和筛选器模式](http://msdn.microsoft.com/library/dn568100.aspx)提供了如何来做到这一点的示例。
- 或者，您可以实现记录状态定期任务有关的信息和可以访问的任何实例运行任务的进程的持久存储中保存此状态的检查点机制。 这种方式，如果进程将关闭，它正在执行的工作可以继续从最后一个检查点通过使用另一个实例。
- 后台任务运行时在单独的计算实例，如云服务的角色在工作人员中承载应用程序，您可能需要扩展使用不同刻度的策略的应用程序的不同部分。 例如，您可能需要部署额外的用户界面 (UI) 计算情况下增加了背景还不计算实例或相反的情况。 如果您提供不同级别的服务 （如基本和特优服务包），您可能需要向外扩展特优服务包的计算资源比基本服务包更严格地以满足服务级别协议。
- 请考虑使用的用户界面和后台计算实例进行通信作为自动缩放策略条件的队列长度。 这是最能反应不平衡或电流负载和后台任务的处理能力之间的差异。
- 如果您基于测量业务流程，如每个小时或一个复杂事务的平均执行时间的订单数目的计数器的自动缩放战略确保您完全理解这种计数器的结果和实际计算容量需求之间的关系。 它可能需要扩展多个组件或计算单元响应业务流程计数器中的更改。  
- 若要防止系统尝试过，向外扩展并避免与运行数以千计的实例相关联的成本，考虑限制可以自动添加的最大次数。 大多数自动缩放机制允许您指定规则实例的最小值和最大数目。 此外，应考虑适当地降低系统提供如果已部署的最大实例数，然后系统仍过载的功能。
- 请记住该自动缩放可能不是最适当的机制来处理负载的突然爆发。 花时间来设置和启动新服务实例或将资源添加到系统中，并且峰值需求可能通过以下这些附加资源已使用的时间。 在这种情况下，可能更好一些，调解服务。 有关详细信息，请参阅[调节模式](http://msdn.microsoft.com/library/dn589798.aspx)。
- 相反，如果您需要的容量来处理所有请求后，该卷波动迅速，成本不是主要的推动因素，考虑使用更快地启动其他实例积极自动缩放策略。 您还可以使用启动了足够数量的实例，以满足预计该负载的最大负载的计划的策略。
- 自动缩放机制应监控自动缩放过程中，并记录每个自动缩放事件的详细信息 (什么触发了它，添加或移除了，哪些资源和时间)。 如果您创建自定义的缩放机制，确保它包含此功能。 分析信息来衡量有效性的自动缩放战略中，并在必要时进行调整。 您可以调整在短期内，这两种使用模式变得更加明显，并且在长远来看，随着业务扩展，或应用程序需求的发展而。 如果在应用程序达到上限为自动缩放定义，机制可能还提醒操作员，他在必要时可以手动启动更多的资源。 注意，在这些情况下，操作员也可能负责减轻工作负荷后，手动删除这些资源。

## <a name="related-patterns-and-guidance"></a>相关的模式和指南
下面的模式和指南也可能涉及到您的方案实现自动缩放时︰

- [限制模式](http://msdn.microsoft.com/library/dn589798.aspx)。 此模式描述了应用程序可以继续和运行时的需求增长将极端负载放置在资源满足 sla 的要求。 限制可使用自动缩放以防止系统被击败时系统已得到扩展。
- [竞争使用者模式](http://msdn.microsoft.com/library/dn568101.aspx)。 此模式描述了如何实现一个可以处理来自任何应用程序实例消息的服务实例的池。 自动缩放可以用于启动和停止服务实例匹配预期的工作负荷。 这种方法，使系统可以处理多个邮件，同时以优化吞吐量、 改进的可扩展性和可用性，并平衡工作负荷。
- [检测和遥测指南](http://msdn.microsoft.com/library/dn589775.aspx)。 检测和遥测是对收集的信息可以驱动自动缩放过程至关重要。

## <a name="more-information"></a>详细信息
- [如何扩展应用程序](./cloud-services/cloud-services-how-to-scale.md)
- [自动缩放 Web 角色、 工作人员角色或虚拟机运行的应用程序](cloud-services-how-to-manage.md#linkresources)
- [如何︰ 将资源链接到云服务](cloud-services-how-to-manage.md#linkresources)
- [扩展链接的资源](./cloud-services/cloud-services-how-to-scale.md#scale-link-resources)
- [Azure 监视服务管理库](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring)
- [Azure 服务管理 REST API，](http://msdn.microsoft.com/library/azure/ee460799.aspx)
- [Azure REST API，资源管理器](https://msdn.microsoft.com/library/azure/dn790568.aspx)
- [Microsoft 的见解库](https://www.nuget.org/packages/Microsoft.Azure.Insights/)
- [自动缩放操作](http://msdn.microsoft.com/library/azure/dn510374.aspx)
- [Microsoft.WindowsAzure.Management.Monitoring.Autoscale Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.management.monitoring.autoscale.aspx)