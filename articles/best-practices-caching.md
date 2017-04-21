<properties
   pageTitle="缓存的指导 |Microsoft Azure"
   description="有关缓存以提高性能和可扩展性指南。"
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
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>缓存的指南

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

缓存是一种旨在提高性能和可扩展性的系统的常用技术。 之所以如此是因为暂时将频繁访问的数据复制到位于接近的快速存储应用程序。 如果此快速数据存储位置靠近比原始的源应用程序，然后缓存可以显著提高客户端应用程序的响应时间，通过更快地处理数据。

缓存是最有效时客户端实例重复读取相同的数据，尤其是在下列所有条件都应用于原始数据存储区︰
- 保持相对静止。
- 与缓存的速度很慢。
- 它是争夺的容易高级别。
- 它是远时网络延迟会导致较慢的访问权限。

## <a name="caching-in-distributed-applications"></a>分布式应用程序中缓存

缓存数据时，分布式应用程序通常会实现一个或两个以下策略︰

- 使用专用的高速缓存，其中的数据本地保存正在运行的应用程序或服务实例的计算机。
- 使用共享的缓存，充当一个通用源，可以通过多个进程和/或计算机访问。

在这两种情况下，缓存可以执行客户端和/或服务器端。 客户端高速缓存是通过提供一种系统，如 web 浏览器或桌面应用程序的用户界面的过程。
服务器端缓存是通过提供业务服务，远程运行的过程。

### <a name="private-caching"></a>专用的高速缓存

最基本的一种高速缓存是内存中的存储。 它有保存在一个单独的进程的地址空间，在该进程中运行的代码的直接访问。 这种类型是缓存的非常快速的访问。 它还可以提供一种非常有效的方法，用于存储静态数据，量不大，因为缓存的大小通常会受到承载进程的计算机上可用的内存量。

如果您需要缓存多于实际可能会在内存中的信息，可以将缓存的数据写入本地文件系统。 这将是比保存在内存中的数据访问速度慢，但仍应更快、 更可靠比通过网络检索数据。

如果您有使用这种模型同时运行的应用程序的多个实例，每个应用程序实例都有自己独立的缓存保存自己的数据副本。

看作一个缓存原始数据在某个时间点的快照在过去。 如果此数据不是静态的则很可能不同的应用程序实例，在其高速缓存中保存的数据的不同版本。 因此，通过这些实例执行的相同查询可能返回不同的结果，如图 1 所示。

![使用内存中缓存的应用程序的不同实例中](media/best-practices-caching/Figure1.png)

_图 1︰ 使用应用程序的不同实例中内存中高速缓存_

### <a name="shared-caching"></a>共享缓存

使用共享的缓存有助于缓解问题在每个缓存，可能会发生内存中缓存可能会与不同的数据。 共享缓存可确保在不同的应用程序实例请参见缓存数据的视图。 这是服务的通过在单独的位置，找到缓存通常承载作为单独的一部分，如图 2 所示。

![使用共享的缓存](media/best-practices-caching/Figure2.png)

_图 2︰ 使用共享的缓存_

一个重要的共享缓存方法的优点是它提供了可扩展性。 许多共享的缓存服务通过使用服务器，群集中的实现，并利用以透明的方式在群集上分发数据的软件。 应用程序实例只是发送一个请求到缓存服务。
底层的基础架构负责确定群集中的缓存数据的位置。 通过添加更多服务器，您可以轻松调整高速缓存。

有两个主要缺点的共享缓存的方法︰
- 高速缓存较慢，因此它不再本地持有的每个应用程序实例访问。
- 要实现一个单独的缓存服务的要求可能会向解决方案中添加复杂性。

## <a name="considerations-for-using-caching"></a>使用缓存的注意事项

以下各节详细描述了设计和使用缓存时的注意事项。

### <a name="decide-when-to-cache-data"></a>决定何时缓存数据

缓存可以显著提高性能、 可扩展性和可用性。 您有更多的数据，需要访问这些数据，较大的缓存变得的优点的用户数越大。 这是因为缓存可以减少延迟和与处理大量并发请求原始数据存储区中的争用。

例如，一个数据库可能支持有限的数量的并发连接。 从共享缓存中检索数据，但是，而不是基础数据库中，使客户端应用程序访问这些数据，即使当前耗尽可用连接数。 此外，如果数据库变为不可用时，客户端应用程序可能能够继续使用的数据保留在缓存中。

请考虑缓存数据被频繁地读取但修改很少 （例如，读取操作的较高比例，比写入操作的数据）。 但是，我们不建议作为关键信息的授权存储，使用缓存。 相反，确保您的应用程序无法承受失去的所有更改始终都保存到持久数据存储区。 这意味着，如果缓存不可用，您的应用程序仍可以继续操作使用的数据存储，并且您不会丢失重要信息。

### <a name="determine-how-to-cache-data-effectively"></a>确定如何有效地缓存数据

有效地使用缓存的关键在于确定最适当的数据缓存，并在适当的时间将其缓存。 数据可以添加到按需在首次检索应用程序缓存中。 这意味着该应用程序需要从数据存储中提取数据一次，并可以通过使用缓存来满足后续访问。

另外，缓存可以部分或完全用数据填充提前，通常当应用程序启动 （称为播种方法）。 但是，它可能不是最好实现播种为大的高速缓存，因为这种方法可以提出原始数据存储区上的突然、 高负载应用程序启动运行时。

往往使用模式的分析可以帮助您决定是否完全或部分地预填充缓存中，并可以选择要缓存的数据。 例如，它可用于种子用静态的用户配置文件数据缓存的用户使用该应用程序定期 （也许是每一天），而不是使用应用程序每周只有一次的客户。

高速缓存通常适用于是不可变的或不经常更改的数据。 示例包括在电子商务应用程序或共享的构造成本较高的静态资源引用信息，比如产品和定价信息。 部分或所有这些数据可以载入缓存在应用程序启动时尽量减少对资源的需求，从而提高性能。 也可能相应有一个后台进程，定期更新引用数据中缓存以确保它是最新的或，刷新缓存时引用数据更改。

缓存是不太有用的动态数据，尽管有此考虑一些例外情况 （请参阅下文中的详细信息的缓存高度动态数据一节）。 当原始数据经常更改时，或者很快成为过时缓存的信息或者与原始数据存储区同步缓存的开销会降低缓存的效果。

请注意，缓存不需要包括实体的完整数据。 例如，如果数据项表示一个多值的对象，例如名称、 地址和帐户余额的银行客户，其中一些元素可能保持静态内容 （如姓名和地址），而其他人 （例如，帐户余额） 可能会更加动态化。 在这些情况下，它可用于缓存数据的静态部分和检索 （或计算） 剩余的信息时它是必需的。

我们建议您执行性能测试和使用情况分析，以确定是否适合预先填充或按需加载缓存或二者兼而有之。 决定应基于的波动性和使用模式的数据。 缓存利用率和性能分析就遇到繁重的负载和必须是高度可扩展的应用程序尤其重要。 例如，在具有高可扩展性的方案中可能会有用植入高速缓存的高峰时期减少数据存储区上的负载。

缓存还可用来避免重复计算，在运行应用程序时。 如果操作数据转换或执行复杂的计算，它可以在缓存中保存操作的结果。 如果以后需要时同样的计算，应用程序只需可以从缓存中检索结果。

应用程序将修改保存在高速缓存中的数据。 但是，我们建议为随时可能消失的瞬态数据存储缓存的思维。 不将有价值的数据存储在缓存请确保您保留原始的数据存储中的信息。 这意味着缓存不可用，如果您减少丢失数据的可能性。

### <a name="cache-highly-dynamic-data"></a>高动态缓存的数据

快速变化的信息存储在持久性数据存储区中时，它可以强加在系统开销。 例如，请考虑不断报告状态或某些其他测量设备。 如果应用程序选择不缓存这些数据几乎总是会过期缓存的信息的基础上，相同的考虑可能是当存储和从数据存储区中检索此信息，则返回 true。 在保存和读取此数据所需的时间，它可能已更改。

在此情况下，应考虑直接在永久性数据存储中而不是缓存中存储的动态信息的好处。 如果数据不重要，并且不需要审核，然后无论如果偶尔的更改都将丢失。

### <a name="manage-data-expiration-in-a-cache"></a>管理在缓存中的数据过期

在大多数情况下，保存在高速缓存中的数据是原始数据存储区中保存的数据的副本。 原始数据存储区中的数据可能会更改后缓存，从而导致缓存的数据变得陈旧。 许多缓存系统使您能够配置缓存过期数据并减少数据可能已过期的时间。

缓存的数据过期时，它会从缓存中删除，应用程序必须从原始数据存储区 （它会给新提取信息返回到缓存） 中检索数据。 配置缓存时，您可以设置默认过期策略。 在很多高速缓存服务，您可以规定过期期限的各个对象存储时它们以编程方式缓存中。
某些高速缓存使您能够指定过期期限，作为一个绝对的值，或滑动，导致如果不能在指定时间内访问从缓存中移除的项值。 此设置将覆盖任何范围内缓存过期策略，但只针对指定的对象。

> [AZURE.NOTE] 请考虑缓存和对象的仔细包含过期期限。 如果您做太短，太快过期的对象，同时还可以降低使用缓存的好处。 如果您的期间太长，则风险正逐渐变旧的数据。

还有可能如果一直驻留很长时间，允许将数据缓存可能会填满。 在这种情况下，将新项添加到缓存的任何请求可能会导致强行逐出的过程中删除一些项目。 缓存服务通常退出数据最早使用 (LRU) 进行，但通常可以重写此策略并防止项目退出。 但是，如果您采用这种方法，则可能超出缓存中可用的内存。 应用程序尝试将某项添加到缓存失败，出现异常。

某些缓存实现可能会提供其他逐出策略。 有几种类型的逐出策略。 其中包括︰
- 最近使用过的策略 （在这些数据将不需要再次的期望）。
- 先进先出策略 （第一次驱逐最早的数据）。
- 根据触发事件 （如被修改的数据） 显式删除策略。

### <a name="invalidate-data-in-a-client-side-cache"></a>使客户端的缓存中的数据无效

在客户端缓存中保存的数据通常被认为可以以外的服务提供给客户端的数据的 auspices。 服务不能直接强制客户端，若要添加或删除客户端高速缓存的信息。

这意味着，很可能会使用的不当配置的缓存继续使用过时的信息的客户端。 例如，如果未正确实现的缓存过期策略，客户端可以使用原始数据源中的信息发生更改时在本地缓存的过时的信息。

如果您正在生成的 web 应用程序提供数据通过 HTTP 连接，可以隐式强制 web 客户端 （如浏览器或 web 代理） 来获取最新信息。 如果资源更新由该资源的 URI 中的更改，您可以这样做。 Web 客户端通常使用的资源的 URI 作为客户端的缓存中的项，因此如果更改 URI，web 客户端忽略任何以前缓存版本的资源改为获取最新版本。

## <a name="managing-concurrency-in-a-cache"></a>在缓存管理并发

高速缓存通常用于由多个应用程序实例的共享。 每个应用程序实例可以读取和修改缓存中的数据。 因此，同样程序出现的任何共享的数据存储的并发问题也适用于高速缓存。 在应用程序需要修改保存在缓存中的数据的情况下，您可能需要确保由一个实例的应用程序所做的更新不会覆盖另一个实例所做的更改。

根据数据的性质和发生冲突的可能性，您可以采用与并发的两种方法之一︰

- __乐观。__ 更新的数据，在应用程序检查以查看是否已更改缓存中的数据，因为它被检索。 如果数据仍然是相同的可以进行更改。 否则，应用程序必须决定是否要对其进行更新。 （这一决定的业务逻辑将是特定于应用程序）。此方法是合适的情况下，位置更新不太频繁，或在冲突是不可能发生。
- __悲观。__ 当它检索的数据时，应用程序就锁定以防止另一个实例更改它的缓存中。 此过程可确保不会发生冲突，但他们还可以阻止其他需要处理的同一数据的实例。 保守式并发可能影响解决方案的可伸缩性和建议只用于短期操作。 这种方法可能适合情况下冲突更有可能的尤其是在应用程序更新缓存中的多个项目，必须确保以一致的方式应用这些更改。

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>实现高可用性和可扩展性，并提高性能

避免使用缓存作为主存储库的数据;这是填充缓存，原始数据存储区的作用。 原始数据存储区是负责确保数据的持久性。

小心不要将严重依赖的共享的缓存服务可用性引入到您的解决方案。 应用程序应该能够继续工作，如果提供共享的缓存的服务不可用。 应用程序不应挂起或等待缓存服务恢复时失败。

因此，应用程序必须准备检测缓存服务的可用性和回退到原始数据存储区访问缓存时。 [断路器模式](http://msdn.microsoft.com/library/dn589784.aspx)可用于处理这种情况。 可以将恢复提供缓存的服务，并且形成原始数据存储区，遵循策略[缓存模式](http://msdn.microsoft.com/library/dn589799.aspx)如读取数据时，一旦它变为可用时，可以重新填充缓存。

但是，可能存在可扩展性影响系统上如果该应用程序会回到原始数据存储区时高速缓存是暂时不可用。
正在恢复数据存储区，而原始数据存储区无法淹没请求数据，从而导致超时并失败的连接。

考虑实现本地、 专用缓存中的应用，以及应用程序的所有实例都会都访问共享缓存的每个实例。 当应用程序检索一个项目时，它可以首先检查其本地缓存，然后在共享缓存，并最后在原始数据存储。 可以使用的数据共享的缓存中或数据库中不可用的共享的缓存是否填充本地缓存。

此方法需要细致的配置，以防止变得太陈旧方面共享缓存本地缓存。 但是，本地缓存充当缓冲区如果共享的缓存不可访问。 图 3 显示了此结构。

![本地、 专用缓存中使用共享的缓存](media/best-practices-caching/Caching3.png)
_图 3︰ 使用共享缓存本地、 专用缓存_

为了支持大保存相对较长的数据的缓存，某些缓存服务提供缓存不可用时实现自动故障转移功能的高可用性选项。 这种方法通常涉及复制缓存到二级高速缓存服务器，主高速缓存服务器存储的数据，如果主服务器发生故障时切换到辅助服务器或连接丢失。

为了减少与写入到多个目标相关联的延迟，复制到辅助服务器时可能会出现以异步方式在主服务器上的缓存写入数据。 这种方法可能导致某些缓存的信息可能会丢失发生故障时，，但此数据的比例应该是小的与缓存的总大小。

如果共享的缓存较大时，它可能有好处，对数据进行分区缓存跨节点以减少争用的机会并提高可伸缩性。 许多共享的缓存支持动态添加 （和删除） 节点的能力，并重新平衡跨分区的数据。 这种方法可能涉及群集中节点的集合显示到客户端应用程序为无缝、 单个缓存。 在内部，但是，数据被分散后均匀平衡负载的预定义的分发策略的节点之间。 在 Microsoft 网站上的[数据分区指导文档](http://msdn.microsoft.com/library/dn589795.aspx)提供有关可能的分区策略的详细信息。

群集还可以增加高速缓存的可用性。 如果某个节点发生故障，高速缓存的其余部分是仍然可以访问。
经常使用结合复制和故障切换群集。 每个节点都可以复制，并且复制副本可以快速联机如果节点出现故障。

很多读和写操作都有可能涉及到单个数据值或对象。 但是，有时它可能需要存储或检索大量数据的快速。
例如，播种缓存可能涉及数百或数千项写入缓存。 应用程序可能还需要从缓存中检索大量的相关项目，作为同一个请求的一部分。

许多大规模的缓存提供了用于这些目的的批处理操作。 这使客户端应用程序进行打包到单个请求的项的量很大，并降低与执行大量小请求相关联的开销。

## <a name="caching-and-eventual-consistency"></a>缓存和最终的一致性

缓存模式工作，填充缓存的应用程序实例必须有权访问最新且一致的数据版本。 在系统中，实现最终的一致性 （例如，复制的数据存储区），这可能不是这种情况。

应用程序的一个实例可以修改数据项并使缓存的项的版本。 另一个实例的应用程序可能会尝试从缓存中，从而导致缓存未命中数，因此，它从数据存储区中读取数据并将其添加到缓存中读取此项目。 但是，如果数据存储区尚未完全同步与其他副本，应用程序实例无法读取和填充缓存的旧值。

有关处理数据的一致性的详细信息，请参阅 Microsoft 网站上的[数据一致性初级课程](http://msdn.microsoft.com/library/dn589800.aspx)页面。

### <a name="protect-cached-data"></a>保护缓存的数据

而不考虑缓存服务使用，请考虑如何保护存放在未经授权的访问缓存中的数据。 有两个主要问题︰

- 缓存中数据的保密性
- 缓存和使用缓存的应用程序之间流动的数据作为它的隐私

为了保护缓存中的数据，缓存服务可能会实现要求的应用程序指定以下身份验证机制︰
- 标识哪些可以访问缓存中的数据。
- 这些标识允许执行哪些操作 （读取和写入）。

为了减少开销与读取和写入数据后写和/或对标识可以使用缓存中的所有数据缓存读取访问权限授予某个标识。

如果您需要限制访问的缓存数据的子集，可以执行以下任一操作︰

- 缓存分为分区 （通过使用不同的缓存服务器） 并仅授予标识应允许它们使用的分区的访问。
- 通过使用不同的密钥加密中每个子集的数据并提供仅对应该有权访问每个子集的标识的加密密钥。 客户端应用程序可能仍然能够检索中的所有数据缓存，但它将只能够有这些密钥将数据解密。

流动和缓存，还必须保护的数据。 若要执行此操作，则取决于客户端应用程序使用连接缓存到网络基础结构提供的安全功能。 如果缓存实现使用同一个组织内的现场服务器托管的客户端应用程序，然后网络本身的隔离可能不需要采取额外的步骤。 如果高速缓存位于远程，并需要通过公用网络 （如 Internet) 的 TCP 或 HTTP 连接，请考虑实现 SSL。

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>实施缓存与 Microsoft Azure 时的注意事项

Azure 提供 Azure Redis 高速缓存。 这是开源作为服务 Azure 数据中心中运行的 Redis 高速缓存实现。 是否作为一种云服务，网站，或在 Azure 的虚拟机中实现该应用程序，它提供了从任何 Azure 应用程序，可以访问缓存服务。 具有适当的访问密钥的客户端应用程序可以共享缓存。

Azure Redis 高速缓存是高性能缓存解决方案提供了可用性、 可扩展性和安全性。 它通常作为分布在一个或多个专用机服务运行。 它尝试将存储在内存，以确保快速访问任意数量的信息。 这种体系结构旨在通过减少需要执行速度慢的 I/O 操作来提供低延迟和高吞吐量。

 Azure Redis 高速缓存是与许多客户端应用程序使用的各种 Api 兼容。 如果您有现有的应用程序已经使用 Redis 运行内部缓存的 Azure，Azure Redis 缓存提供缓存在云中的快速迁移路径。

> [AZURE.NOTE] Azure 还提供了托管缓存服务。 此服务基于 Azure 服务结构缓存引擎。 它使您能够创建可由松散耦合的应用程序共享一个分布式的缓存。 高速缓存位于 Azure 数据中心中运行的高性能服务器上。
但是，此选项不再建议使用，并且仅可用于支持已生成使用它的现有应用程序。 对于所有新的开发，改用 Azure Redis 高速缓存。
>
> 此外，Azure 支持中角色缓存。 此功能使您可以创建特定于云服务的缓存。
高速缓存承载的 web 或辅助角色实例并仅可由属于同一个云服务部署单元操作的角色。 （部署单元是部署为云服务的特定区域的角色实例的集合）。高速缓存聚集索引，并且在同一个部署单元中承载缓存角色的所有实例都成为同一缓存群集的一部分。 但是，此选项不再建议使用，并且仅可用于支持已生成使用它的现有应用程序。 对于所有新的开发，改用 Azure Redis 高速缓存。
>
> Azure 托管缓存服务和 Azure 角色内缓存当前预定 2016 11 月 16 日，在退休。
建议您在此退休准备迁移到 Azure Redis 缓存。 有关详细信息，请访问页  [Azure Redis 缓存产品，并应使用何种大小是什么？](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) Microsoft 网站上。


### <a name="features-of-redis"></a>Redis 的功能

 Redis 是更多而不是简单的缓存服务器。 它具有广泛的命令集，支持许多常见方案提供一个分布式的数据库，内存中。 这些部分中使用 Redis 缓存在下文中介绍。 本节概述了一些 Redis 提供的主要功能。

### <a name="redis-as-an-in-memory-database"></a>Redis 作为内存中的数据库

Redis 同时支持读取和写入操作。 在 Redis，写操作可以抵御通过定期在本地快照文件或仅限附加的日志文件存储系统故障。 这不是这种情况在许多缓存 （这应该被视为暂时的数据存储区）。

 所有写操作是异步的不会阻止客户端读取和写入数据。 当 Redis 开始运行，从快照或日志文件中读取数据并使用其构造内存中缓存。 有关详细信息，请参阅在 Redis 网站上的[Redis 持久性](http://redis.io/topics/persistence)。

> [AZURE.NOTE] Redis 并不保证发生灾难性故障时，将保存所有的写操作，但最坏的情况下可能会在几秒钟之内值得的数据丢失。 请记住，缓存不应作为一个权威数据源，它是使用缓存来确保关键数据已成功保存到适当的数据存储的应用程序的责任。 有关详细信息，请参阅[缓存模式](http://msdn.microsoft.com/library/dn589799.aspx)。

#### <a name="redis-data-types"></a>Redis 数据类型

Redis 是简单类型或复杂的数据结构，如哈希列表，其中包含值的键 / 值存储和设置。 它支持一组原子操作这些数据类型上。 关键字可以是永久性地或有限的生存时间，此时的键和其对应的值自动从缓存中移除的标记。 有关 Redis 键和值的详细信息，请访问 Redis 网站上的页[Redis 数据类型和抽象的简介](http://redis.io/topics/data-types-intro)。

#### <a name="redis-replication-and-clustering"></a>Redis 复制和群集

Redis 支持主/从复制，以帮助确保可用性和维护的吞吐量。 写入到 Redis 主节点的操作会被复制到一个或多个从属节点。 读取操作可以提供到主机或任何下属。

网络分区，在下属可以继续提供数据，然后以透明方式重新同步主服务器时重新建立连接。 有关更多详细信息，请访问 Redis 网站上的[复制](http://redis.io/topics/replication)页。

Redis 还提供了群集技术，它使您能够以透明方式分开到 shards 在服务器上的数据和分布负载。 此功能可以提高可伸缩性，因为可以添加新的 Redis 服务器和缓存的大小为重新分区的数据增加。

此外，在群集中的每个服务器可以使用主/从复制复制。 这确保在群集中的每个节点的可用性。 有关群集和分片的详细信息，请访问 Redis 网站上的[Redis 群集教程页面](http://redis.io/topics/cluster-tutorial)。

### <a name="redis-memory-use"></a>Redis 内存使用

Redis 高速缓存具有有限大小取决于主机计算机上的可用资源。 Redis 服务器配置时，您可以指定最大可用的内存量。 您还可以在 Redis 高速缓存具有的有效期，在此之后它会自动从缓存中删除配置项。 此功能可帮助阻止用旧的或过时的数据填充的内存中缓存。

内存已满，Redis 可以自动退出键以及它们的值按照策略数。 默认值为 LRU （最近最少使用），但您也可以选择和随机退出键 （在将项添加到缓存失败，如果它是完全的案例企图） 完全关闭逐出其他策略。 [使用 Redis 作为一个 LRU 缓存](http://redis.io/topics/lru-cache)的页面提供了详细信息。

### <a name="redis-transactions-and-batches"></a>Redis 事务处理和批处理

Redis 可以将客户端应用程序提交一系列操作的读取和写入数据缓存为原子事务中。 交易记录中的所有命令都都按顺序运行，没有其他并发客户端发出的命令将交织其间。

但是，这些不是真正的交易记录关系数据库会执行它们。 事务处理包括两个阶段，第一种是当命令进行排队，并命令运行时，第二个。 在命令队列阶段，客户端提交构成交易记录的命令。 如果某种类型的错误发生在此时 （如语法错误或参数的个数错误） 然后 Redis 会拒绝处理整个事务和丢弃它。

在运行阶段，Redis 按顺序执行每个队列中的命令。 如果命令失败，在此阶段中，Redis 将继续执行队列中的下一个命令并不会回滚已运行的任何命令的效果。 这种简化的形式的交易记录可帮助维护性能，并避免由争用导致的性能问题。

Redis 实现开放式锁定，以帮助保持一致性的一种形式。 事务和锁定与 Redis 有关的详细信息，请访问 Redis 网站上的[交易记录页面](http://redis.io/topics/transactions)。

Redis 还支持非事务处理批处理的请求。 客户端用于将命令发送到 Redis 服务器的 Redis 协议使客户端可以为同一请求的一部分发送一系列操作。 这可以帮助减少在网络上的数据包碎片。 当处理批处理时，执行每个命令。 如果任何这些命令格式不正确，它们将被拒绝 （这不会发生这种情况与事务），但其余的命令将被执行。 此外，还有关于将处理批处理中的命令的顺序不能保证。

### <a name="redis-security"></a>Redis 安全

Redis 专门在提供对数据的快速访问，旨在在只能由受信任的客户端可以访问的受信任环境中运行。 Redis 支持一个有限的安全模型，该模型基于密码的身份验证。 （它是可以完全移除身份验证尽管不建议这样做。）

所有经过身份验证的客户端共享相同的全局密码并访问相同的资源。 如果您需要更全面的登录安全，您必须实现 Redis 服务器上，您自己的安全层，所有客户端请求应通过此附加层。 Redis 应不直接公开给不受信任或未经身份验证的客户端。

通过禁用它们，或重命名这些 （并提供只有特权客户端使用新的名称），可以限制访问的命令。

Redis 直接不支持任何形式的数据加密，因此必须由客户端应用程序执行所有编码。 此外，Redis 不提供任何形式的传输安全。 如果您需要保护的数据通过网络传输，我们建议实施 SSL 代理。

有关详细信息，请访问 Redis 网站上的[Redis 安全](http://redis.io/topics/security)页。

> [AZURE.NOTE] Azure Redis 缓存提供了它自己通过该客户端连接的安全层。 不公开到公用网络的基础的 Redis 服务器。

### <a name="using-the-azure-redis-cache"></a>使用 Azure Redis 高速缓存

Azure Redis 缓存提供了对 Redis Azure 数据中心; 在承载的服务器上运行的服务器访问它充当外观提供了访问控制和安全。 可以通过使用 Azure 管理门户设置高速缓存。 门户网站提供了多种预定义的配置，范围从 53 GB 缓存作为专用服务支持 SSL 通信 （保密） 和主/从复制与 SLA 的 99.9%的可用性，至 250 MB 缓存不共享的硬件上运行的复制 （不可用性保证） 的情况下运行。

使用 Azure 管理门户，还可以配置逐出策略的缓存中，然后通过将用户添加到角色提供; 控制缓存的访问所有者，参与者，读取器。 这些角色定义的成员可以执行的操作。 例如，所有者角色的成员具有完全控制权 （包括安全） 的缓存及其内容、 讨论参与者角色的成员可以读取和写入信息在缓存中，和读者角色的成员只能从缓存检索的数据。

大多数管理任务都执行通过 Azure 管理门户网站，并关闭 Redis 服务器，因此许多 Redis 的标准版本中可用的管理命令都不可用，包括能够以编程方式修改配置配置其他从属服务器，或强行将数据保存到磁盘。

Azure 管理门户包括方便的图形化显示，可用于监视缓存的性能。 例如，您可以查看正在建立的连接数、 执行的请求数，读取和写入的卷和缓存的命中与高速缓存未命中数。 使用此信息，您可以确定缓存的效果，如果需要切换到一个不同的配置或更改逐出策略。 此外，您可以创建警报，向管理员发送电子邮件，如果一个或多个关键指标超出预期的范围。 例如，如果在最后一个小时内，高速缓存未命中数超过指定的值，管理员无法发出警报，缓存可能太小或数据可能会被逐出太快。

您还可以监视 CPU、 内存和网络缓存的使用情况。

有关进一步的信息和示例来演示如何创建和配置一个 Azure Redis 缓存，在 Azure 博客上访问页[Azure Redis 缓存绕膝](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/)。

## <a name="caching-session-state-and-html-output"></a>缓存的会话状态和 HTML 输出

如果您创建 ASP.NET web 应用程序，使用 Azure web 角色运行，您可以保存会话状态信息和 HTML 输出 Azure Redis 高速缓存中。 Azure Redis 缓存会话状态提供程序使您可以在会话之间共享信息的 ASP.NET web 应用程序的不同实例，并在客户端-服务器关系不可用，并不会相应会话数据的内存中缓存的 web 场情况下非常有用。

使用会话状态提供程序使用 Azure Redis 缓存提供了多种好处，包括︰

- 它可以共享大量的实例提供改进的可扩展性，ASP.NET web 应用程序的会话状态
- 它支持受控、 并发访问同一个会话状态数据用于多个读取器和单个编写器，并
- 它可以使用压缩来节省内存并提高网络性能。

详细信息请访问 Microsoft 网站上的[Redis Azure 的高速缓存的 ASP.NET 会话状态提供程序](redis-cache/cache-aspnet-session-state-provider.md)页。

> [AZURE.NOTE] 不要使用会话状态提供程序为 Azure Redis 缓存 Azure 的环境之外运行的 ASP.NET 应用程序。 访问缓存在 Azure 之外的延迟可以消除数据缓存的性能优势。

与此类似，Redis Azure 的高速缓存的输出缓存提供程序使您可以保存由 ASP.NET web 应用程序生成的 HTTP 响应。 输出缓存提供程序中使用 Azure Redis 缓存可以提高呈现复杂 HTML 输出，则应用程序的响应时间可以使生成相似响应的应用程序实例的共享的输出片段缓存，而不是生成此 HTML 输出重新使用。  详细信息请访问 Microsoft 网站上的[Redis Azure 的高速缓存的 ASP.NET 输出缓存提供程序](redis-cache/cache-aspnet-output-cache-provider.md)页。

### <a name="azure-redis-cache"></a>Azure 的 Redis 高速缓存

Azure Redis 缓存提供了对 Azure 数据中心承载的 Redis 服务器的访问。 它充当外观提供了访问控制和安全。 可以通过使用 Azure 门户设置高速缓存。

门户网站提供了许多预定义的配置。 这些涵盖了从 53 GB 缓存作为专用服务运行的支持 （对于隐私性） 的 SSL 通信并与 SLA 的 99.9%的可用性，而无需共享的硬件上运行的复制 （不可用性保证） 25 0 MB 缓存到的主/从复制。

使用 Azure 的门户，您可以配置的缓存中逐出策略并通过将用户添加到角色提供控制缓存的访问。  定义的成员可以执行的操作，这些角色包括所有者、 参与者和读者。 例如，所有者角色的成员具有完全控制权 （包括安全） 的缓存及其内容、 讨论参与者角色的成员可以读取和写入信息在缓存中，和读者角色的成员只能从缓存检索的数据。

通过 Azure 的门户执行大多数的管理任务。 由于这个原因，许多管理 Redis 标准版中提供的命令将不可用，包括能够以编程方式修改配置、 关闭 Redis 服务器、 配置其他下属，或强行将数据保存到磁盘。

Azure 门户包括方便的图形化显示，可用于监视缓存的性能。 例如，您可以查看正在进行，正在执行的请求数、 读取和写入的卷的连接数和高速缓存命中与缓存未命中数。 使用此信息，您可以确定缓存的有效性，如有必要，切换到不同的配置或更改逐出策略。

此外，您可以创建警报，向管理员发送电子邮件，如果一个或多个关键指标超出预期的范围。 例如，可能要向管理员发出警报如果高速缓存未命中数超过指定的值在过去一小时，因为这意味着缓存可能太小或数据可能会被逐出太快。

您还可以监视 CPU、 内存和网络缓存的使用情况。

有关进一步的信息和示例来演示如何创建和配置一个 Azure Redis 缓存，在 Azure 博客上访问页[Azure Redis 缓存绕膝](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/)。

## <a name="caching-session-state-and-html-output"></a>缓存的会话状态和 HTML 输出

如果您正在构建 ASP.NET web 应用程序，使用 Azure web 角色运行，您可以保存会话状态信息和 Azure Redis 高速缓存中的 HTML 输出。 Azure Redis 缓存的会话状态提供程序使您可以在会话之间共享信息的 ASP.NET web 应用程序的不同实例，并在客户端-服务器关系不可用，并不会相应会话数据的内存中缓存的 web 场情况下非常有用。

使用会话状态提供程序使用 Azure Redis 缓存提供了多种好处，包括︰

- 与大量的 ASP.NET web 应用程序实例的共享会话状态。
- 提供改进的可扩展性。
- 用于多个读取器和单个编写器支持受控、 并发访问同一个会话状态数据。
- 使用压缩以节省内存并提高网络性能。

有关详细信息，请访问 Microsoft 网站上的[Azure Redis 缓存的 ASP.NET 会话状态提供程序](redis-cache/cache-aspnet-session-state-provider.md)页。

> [AZURE.NOTE] 请不要使用会话状态提供程序为 Azure Redis 缓存在 Azure 环境之外运行的 ASP.NET 应用程序。 访问缓存在 Azure 之外的延迟可以消除数据缓存的性能优势。

与此类似，Redis Azure 的高速缓存的输出缓存提供程序使您可以保存由 ASP.NET web 应用程序生成的 HTTP 响应。 输出缓存提供程序中使用 Azure Redis 缓存可以提高呈现复杂的 HTML 输出的应用程序的响应时间。 可以使生成类似响应的应用程序实例的共享的输出片段缓存，而不是生成此 HTML 输出重新使用。 有关详细信息，请访问 Microsoft 网站上的[Azure Redis 缓存的 ASP.NET 输出缓存提供程序](redis-cache/cache-aspnet-output-cache-provider.md)页。

## <a name="building-a-custom-redis-cache"></a>生成一个自定义的 Redis 高速缓存

Azure Redis 缓存作为基础的 Redis 服务器的外观。 目前它支持一套固定的配置，但不提供对 Redis 群集。 如果您需要不受 Azure Redis 缓存 （如高速缓存大于 53 GB） 高级的配置可以生成并使用 Azure 虚拟机主机 Redis 服务器。

这是潜在的复杂的过程，因为您可能需要创建多个虚拟机作为主和次级节点，如果您想要执行复制。 此外，如果您想要创建一个群集，则需要多个主服务器和从属服务器。 提供更高程度的可用性和可伸缩性小群集的复制拓扑包括至少六个组织为三对主/次级服务器 （群集必须包含至少三个主节点） 的虚拟机。

应紧靠在一起位于每个主/次级对尽量减少延迟。 但是，对每个组可以运行在位于不同地区，不同的 Azure 数据中心如果您想要找到附近最有可能使用它的应用程序的缓存的数据。 [在 CentOS Linux 虚拟机在 Azure 上运行 Redis](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) Microsoft 网站上的页引导完成一个示例，演示如何生成并将其配置为 Azure 虚拟机运行的 Redis 节点。

[AZURE.NOTE] 请注意，如果您以这种方式实现了 Redis 高速缓存，则负责监视、 管理和服务的安全。

## <a name="partitioning-a-redis-cache"></a>Redis 高速缓存分区

缓存分区涉及跨多台计算机拆分缓存。 这种结构具有若干优点使用单一缓存服务器，包括︰

- 创建一个缓存来是不是可以存储在单个服务器上大得多。
- 通过在提高可用性的服务器之间分布数据。 如果一台服务器发生故障或变为不可访问，数据，它包含不可用，但其余的服务器上的数据仍可以访问。 对于缓存中，这是不重要因为缓存的数据是瞬时副本保存在数据库中的数据。 变得无法访问服务器上的缓存的数据改为可在另一台服务器上缓存。
- 将负载分散到服务器，从而提高了性能和可扩展性。
- Geolocating 数据接近用户的访问它，从而减少了等待时间。

对于缓存分区的最常见的形式是分片。 在此策略中，每个分区 （或 shard） 是本身就 Redis 高速缓存。 数据是通过分片逻辑，可以使用多种方法来分发的数据定向到特定的分区。 [分片模式](http://msdn.microsoft.com/library/dn589797.aspx)提供了有关实施分片的详细信息。

若要实现在 Redis 高速缓存分区，可以采用下列方法之一︰

- _服务器端查询路由。_ 这种技术，在客户端应用程序对任何包含缓存 （可能是最接近的服务器） 的 Redis 服务器发送一个请求。 每个 Redis 服务器存储元数据，描述了它持有，并且还包含在其他服务器上的信息位于哪个分区的分区。 Redis 服务器会检查客户端请求。 如果本地可以得到解决，它将执行所请求的操作。 否则，它将转发到相应的服务器请求。 此模型由 Redis 群集，并在 Redis 网站上[Redis 群集教程](http://redis.io/topics/cluster-tutorial)页面上更详细地介绍。 Redis 聚集功能对客户端应用程序透明，可以无需重新配置客户端到群集 （和重新分区的数据） 添加附加的 Redis 服务器。

- _客户端的分区。_ 在此模型中，客户端应用程序中包含逻辑 （可能是在某个库的形式），将请求路由到相应的 Redis 服务器。 这种方法可以使用 Azure Redis 高速缓存。 创建多个 Azure Redis 高速缓存 （每个数据分区的一个），并实现将请求路由到正确的缓存的客户端的逻辑。 如果分区方案发生更改 （如果其他 Azure Redis 缓存创建，例如），则客户端应用程序可能需要重新配置。

- _代理协助分区。_ 在此方案中，客户端应用程序发送到理解数据分区中间代理服务请求，然后将请求路由到相应的 Redis 服务器。 这种方法也可使用 Azure Redis 高速缓存;作为 Azure 的云服务，可以实现代理服务。 这种方法需要更高层次的复杂性，以实现该服务，并请求可能需要更长的时间要比使用客户端的分区。

页面[分区︰ 如何拆分多个 Redis 实例间的数据](http://redis.io/topics/partitioning)在 Redis 网站提供了有关实现的 Redis 分区的详细信息。

### <a name="implement-redis-cache-client-applications"></a>实现 Redis 缓存客户端应用程序

Redis 支持多种编程语言编写的客户端应用程序。 如果您在构建新的应用程序使用.NET Framework，推荐的方法是使用 StackExchange.Redis 客户端库。 此库提供了.NET Framework 对象模型抽象化为连接到 Redis 服务器、 发送命令，并接收响应的详细信息。 它是在 Visual Studio 中可用作为 NuGet 程序包。 您可以使用此相同库连接到 Azure Redis 高速缓存或自定义 Redis 缓存驻留在虚拟机上。

若要连接到 Redis 服务器使用静态`Connect`方法的`ConnectionMultiplexer`类。 此方法创建连接设计用于整个生命周期的客户端应用程序，并可以由多个并发线程使用相同的连接。 不要重新连接和断开连接每次您执行 Redis 操作，因为这样做会降低性能。

您可以指定连接参数，如 Redis 主机地址和密码。 如果您使用的 Redis Azure 的高速缓存，密码不是主要或辅助生成的密钥是 Azure Redis 缓存通过使用 Azure 管理门户。

您已连接到 Redis 服务器后，您可以获得充当缓存对 Redis 数据库的句柄。 Redis 连接提供的`GetDatabase`方法完成此操作。 然后可以从缓存中检索的项，并在缓存中存储数据，使用`StringGet`和`StringSet`的方法。 这些方法期望一个密钥作为参数，并且返回的项在缓存中具有匹配值 (`StringGet`) 或此项添加到缓存的项 (`StringSet`)。

Redis 服务器的位置，根据许多操作可能会带来一定程度的延迟而请求传输到服务器并向客户端返回的响应。 StackExchange 库提供了许多帮助保持响应状态的客户端应用程序公开的方法的异步版本。 这些方法支持.NET Framework 中的[基于任务的异步模式](http://msdn.microsoft.com/library/hh873175.aspx)。

下面的代码段显示了名为`RetrieveItem`。 它说明了基于 Redis 和 StackExchange 库缓存模式实现。 该方法将字符串键值并尝试从 Redis 缓存中检索相应的项目，通过调用`StringGetAsync`方法 (异步版本的`StringGet`)。

如果没有找到该项，则提取从基础数据源使用`GetItemFromDataSourceAsync`（这是本地的方法并不是 StackExchange 库的一部分） 的方法。 它然后将添加到缓存通过使用`StringSetAsync`方法，以便它可以检索更快下一次。

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

`StringGet` ，`StringSet`方法并不仅限于检索或存储字符串值。 它们可以被序列化为一个字节数组中的任何项目。 如果您需要保存一个.NET 对象时，可以将其序列化为一个字节流，并使用`StringSet`方法将写入缓存。

同样，通过使用，从缓存读取对象`StringGet`方法并作为.NET 对象进行反序列化。 下面的代码演示了一套 IDatabase 接口的扩展方法 ( `GetDatabase` Redis 连接方法将返回`IDatabase`对象)，并使用这些方法来读取和写入一些示例代码`BlogPost`对缓存的对象︰

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

下面的代码阐释了名为的方法`RetrieveBlogPost`，它使用这些扩展方法来读取和写入序列化`BlogPost`后面的缓存模式缓存的对象︰

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis 支持命令管道如果客户端应用程序发送多个异步请求。 Redis 可以多路传输请求使用相同的连接，而不是接收和响应命令中严格的顺序。

这种方法有助于减少延迟，从而更有效地使用网络。 下面的代码段演示同时检索两个客户的详细信息。 代码提交两个请求，然后执行其他处理 （未显示） 之前等待接收结果。 `Wait`的高速缓存对象的方法类似于.NET Framework 是`Task.Wait`方法︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

在 Microsoft 网站上的页[Azure Redis 缓存文档](https://azure.microsoft.com/documentation/services/cache/)提供了有关如何编写客户端应用程序可以使用 Azure Redis 高速缓存的详细信息。 在 StackExchange.Redis 网站上[基本使用页面](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md)上提供了其他信息。

在同一网站上的页[管线和复用器](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md)提供了有关异步操作和管道与 Redis 和 StackExchange 库的详细信息。  在本文中，使用 Redis 缓存下, 一节提供了一些更高级的技术，可应用于 Redis 缓存中保留的数据的示例。

## <a name="using-redis-caching"></a>使用 Redis 缓存

Redis 用于缓存问题最简单的用法是键 / 值对的 value 是一个未解释可以包含任何二进制数据的任意长度的字符串。 （它是本质上的可以被视为一个字符串的字节数组）。 这种情况下所示在本文前面部分实现 Redis 缓存客户端应用程序。

请注意，键还包含未解释的数据，因此您可以使用任何二进制信息作为键。 越长的关键是，但是，需要存储，更多的空间和执行查找操作需要的时间越长。 易用性和易维护性，小心地设计您的密钥空间和使用意义 （但不是详细） 键。

例如，使用结构化的键，如"客户︰ 100"来表示客户 ID 为 100，而不只是"100"的关键。 这种方案使您能够轻松区分存储不同数据类型的值。 例如，您可以使用键"订单︰ 100"来表示的键具有 ID 为 100 的订单。

与一维的二进制字符串中 Redis 键 / 值对的值也可以容纳更结构化的信息，包括列表设置 （排序和未排序），和进行哈希处理。 Redis 提供了一个全面的命令集，可以操纵这些类型，和其中的许多命令都如 StackExchange 的客户端库通过.NET Framework 应用程序可用。 在 Redis 网站上的[Redis 数据类型和抽象的简介](http://redis.io/topics/data-types-intro)页提供了这些类型，您可以使用对其进行操作的命令的详细的概述。

本节概述了一些常见的使用案例，这些数据类型和命令。

### <a name="perform-atomic-and-batch-operations"></a>执行原子和批处理操作

Redis 支持一系列原子获取设置操作的字符串值。 这些操作删除单独使用时可能会出现可能的争用风险`GET`和`SET`的命令。 可用操作包括︰

- `INCR``INCRBY`， `DECR`，和`DECRBY`，其执行整数数字数据值上的原子增量和减量运算。 StackExchange 库提供重载的版本的`IDatabase.StringIncrementAsync`，`IDatabase.StringDecrementAsync`方法来执行这些操作并返回结果值存储在缓存中。 下面的代码段说明了如何使用这些方法︰

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`即检索与某个键相关联并且更改为新值的值。 StackExchange 库使得此操作可通过`IDatabase.StringGetSetAsync`方法。 以下代码片段显示了此方法的示例。 此代码将返回与"数据︰ 计数器"前面的示例中的键相关联的当前值。 然后它该关键字的值重置为零，所有属于相同的操作︰

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`和`MSET`，它可以返回或更改一组字符串值作为单个操作。 `IDatabase.StringGetAsync` ，`IDatabase.StringSetAsync`方法重载以支持此功能，如下面的示例所示︰

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Redis 事务处理和批处理部分本文前面所述，还可以组合成一个 Redis 事务的多个操作。 StackExchange 库提供支持的交易记录，通过`ITransaction`接口。

创建`ITransaction`对象通过使用`IDatabase.CreateTransaction`方法。 使用提供的方法调用到交易记录的命令`ITransaction`对象。

`ITransaction`接口提供对一组方法类似于那些通过访问`IDatabase`接口，不同之处在于，所有方法都是异步。 这意味着，它们只是执行时`ITransaction.Execute`调用方法。 返回的值`ITransaction.Execute`方法指示交易记录是否已成功创建 (true) 或失败 (false)。

下面的代码段显示一个示例的递增和递减的两个计数器作为同一事务的一部分︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

请记住，Redis 的交易记录都与关系数据库中的事务。 `Execute`方法只是队列的所有命令，包括事务来运行，并且如果其中的任何格式不正确，则停止交易记录。 如果已成功排队的所有命令，每个命令将以异步方式运行。

如果任何命令失败，其他人仍会继续处理。 如果您需要验证命令已成功完成，您必须使用的相应任务的**Result**属性获取命令的结果，如上面的示例中所示。 读取**结果**属性会阻止调用线程，直到完成任务为止。

有关详细信息，请参阅在 StackExchange.Redis 网站上的[Redis 中的交易记录](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md)页面。

执行批处理操作时，您可以使用`IBatch`StackExchange 库的接口。 此接口提供对一组方法类似于那些通过访问`IDatabase`接口，不同之处在于，所有方法都是异步。

创建`IBatch`对象通过使用`IDatabase.CreateBatch`方法，并通过使用然后运行批处理`IBatch.Execute`方法，如下面的示例中所示。 此代码只需设置一个字符串值，递增和递减在前面的示例中，使用的同一个计数器，并显示结果︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

请务必了解与不同的事务中，是否在批处理中的命令将失败，因为它的格式不正确，其他命令可能仍然运行。 `IBatch.Execute`方法不返回任何指示成功或失败。

### <a name="perform-fire-and-forget-cache-operations"></a>执行火灾并忘记了高速缓存操作

Redis 支持火灾和忘记操作通过使用命令标志。 在此情况下，客户只需启动一个操作但已不感兴趣的结果并不会等待要完成的命令。 下面的示例显示了如何执行该增量命令作为火灾和忘记操作︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>指定自动过期的键

当您在 Redis 高速缓存中存储项时，您可以指定的超时后，项目将自动从缓存中移除。 您也可以查询如何更多的时间一个密钥已到期之前将它使用`TTL`命令。 此命令是 StackExchange 应用程序可通过`IDatabase.KeyTimeToLive`方法。

下面的代码段显示了如何在项上设置过期时间为 20 秒以及查询的键的剩余寿命︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

您还可以将过期时间使用到期命令，可在 StackExchange 库设置为一个特定的日期和时间`KeyExpireAsync`方法︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _提示︰_您可以手动删除项从缓存通过使用 DEL 命令，可通过 StackExchange 库作为`IDatabase.KeyDeleteAsync`方法。

### <a name="use-tags-to-cross-correlate-cached-items"></a>使用标记来交叉关联的缓存的项

Redis 集是共享一个密钥的多个项的集合。 通过 SADD 命令，可以创建一组。 可以通过使用 SMEMBERS 命令来检索集合中的项。 StackExchange 库实现 SADD 命令及`IDatabase.SetAddAsync`方法和 SMEMBERS 命令与`IDatabase.SetMembersAsync`方法。

您还可以结合现有的设置，使用 SDIFF （差集）、 SINTER （交集） 和 SUNION （集） 命令创建新集。 StackExchange 库统一了这些操作`IDatabase.SetCombineAsync`方法。 对此方法的第一个参数指定要执行的一组操作。

下面的代码段显示了如何设置可用于快速存储和检索相关的项的集合。 此代码使用`BlogPost`已在本文前面的实现 Redis 缓存的客户端应用程序一节中描述的类型。

A`BlogPost`对象包含四个字段 — — 一个 ID、 一个标题、 排名分数和标记的集合。 第一个代码段演示的示例数据来填充 C# 列表中的使用`BlogPost`对象︰

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

您可以为每个存储标记`BlogPost`对象作为 Redis 缓存中一组并将每个组相关联的 ID 为`BlogPost`。 这使应用程序能够快速查找属于特定博客张贴内容的所有标记。 要启用方向相反的搜索并查找所有共享特定标记的博客文章，您可以创建包含博客文章引用键中的标记 ID 的另一组︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

这些结构可以非常高效地执行许多常见的查询。 例如，可以查找并显示所有标记的博客文章 1 如下︰

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

您可以发现共有的博客的所有标记，如下所示执行对集合进行交集运算，都张贴 1 和博客都帖子 2:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

然后，您可以找到包含特定标记的所有博客帖子︰

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>查找最近访问的项目

许多应用程序所需的常见任务是查找最近访问过的项目。 例如，博客网站可能想要显示有关最近读的博客张贴内容的信息。

通过使用 Redis 列表，可以实现此功能。 Redis 列表包含多个项目共享相同的密钥。 该列表作为双向队列。 通过使用 LPUSH （左推） 和 RPUSH （右推） 命令，可以将项目推到该列表的任意一端。 通过使用 LPOP 和 RPOP 命令，可以从列表中的任一端检索项目。 您还可以通过使用 LRANGE 和 RRANGE 命令返回一的组元素。

下面的代码段显示了如何使用 StackExchange 库可以执行这些操作。 此代码使用`BlogPost`前面示例中的类型。 博客张贴内容读取由用户`IDatabase.ListLeftPushAsync`方法将拖到列表与"blog:recent_posts"Redis 高速缓存中的项相关联的博客张贴内容的标题。

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

当读取更多的博客张贴内容时，其标题被推相同的列表。 在其中添加标题的次序排列。 最近读的博客文章是靠左边列表的末尾。 （如果多次读取同一个博客张贴内容时，它将具有多个条目在列表中。）

您可以通过使用显示最近读过的文章的标题`IDatabase.ListRange`方法。 此方法将包含列表、 起始点和结束点的键。 下面的代码检索标题的 10 个博客张贴内容 （从 0 到 9 项） 最左边的列表的末尾︰

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

请注意，`ListRangeAsync`方法不从列表中移除项。 若要执行此操作，您可以使用`IDatabase.ListLeftPopAsync`和`IDatabase.ListRightPopAsync`的方法。

若要防止无限增长列表，可以定期修剪列表挑选项目。 以下代码片段显示如何删除所有但五最左边的项从列表︰

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>实现领导委员会

默认情况下，一组中的项不包含在任何特定的顺序。 您可以使用 ZADD 命令来创建一组有序 ( `IDatabase.SortedSetAdd` StackExchange 库中的方法)。 通过使用称作分数，作为命令的参数提供一个数字值，各项的排序。

下面的代码段将博客文章的标题添加到排序列表。 在此示例中，每个博客文章还有一个分数字段包含的博客张贴内容的等级。

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

可以检索的博客帖子标题和分数分数按升序使用`IDatabase.SortedSetRangeByRankWithScores`方法︰

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] StackExchange 库还提供了`IDatabase.SortedSetRangeByRankAsync`方法，它以得分顺序返回数据但不返回分数。

此外可以检索项目按降序分数，并限制通过提供额外的参数返回的项的数目`IDatabase.SortedSetRangeByRankWithScoresAsync`方法。 下一个示例显示标题和得分前 10 的排名的博客文章︰

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

下一个示例使用`IDatabase.SortedSetRangeByScoreWithScoresAsync`方法，可用于限制返回到那些范围内给定分数的项目范围︰

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>通过使用渠道的消息

除了作为一个数据缓存，Redis 服务器提供消息传递通过高性能发布服务器/订阅机制。 客户端应用程序可以订阅频道，和其他应用程序或服务可以将消息发布到通道。 订阅应用程序然后将接收这些消息，并且可以处理它们。

Redis 提供了客户端应用程序使用为订阅频道订阅命令。 此命令要求一个或多个通道的应用程序将接受消息的名称。 StackExchange 库包括`ISubscription`接口，可以将.NET Framework 应用程序订阅和发布渠道。

创建`ISubscription`对象通过使用`GetSubscriber`到 Redis 服务器的连接的方法。 使用侦听信道上的邮件，然后`SubscribeAsync`的此对象的方法。 下面的代码示例演示如何订阅到名为"邮件︰ blogPosts"的通道︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

第一个参数为`Subscribe`方法是频道的名称。 此名称应遵循相同的约定所使用的高速缓存中的项。 名称可以包含任意二进制数据，但最好使用相对较短的、 有意义的字符串来帮助确保良好的性能和可维护性。

此外请注意渠道所使用的命名空间是分开使用的键。 这意味着可以有渠道和键具有相同的名称，虽然这可能会使您的应用程序代码更难以维护。

第二个参数是操作委托。 只要新邮件通道上出现，此委托将以异步方式运行。 此示例只是 （消息将包含的博客文章标题） 在控制台上显示消息。

若要发布到的频道，应用程序可以使用 Redis 发布命令。 StackExchange 库提供了`IServer.PublishAsync`方法来执行此操作。 下一步的代码段显示了如何向"邮件︰ blogPosts"频道发布一条消息︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

有几个问题，您应了解有关发布/订阅机制︰

- 多个订阅服务器可以订阅同一信道，并且他们将收到发布到该渠道的消息。
- 订阅服务器只接收后他们已订阅已发布的消息。 频道未进行缓冲处理，以及一旦发布一条消息，Redis 基础结构将消息推送到每个订阅服务器，并将其移除。
- 默认情况下，消息接收由订阅服务器所发送的顺序中。 在与大量的邮件和许多订阅服务器和发布服务器系统中非常活跃，有保证的顺序传递的邮件会降低系统的性能。 如果每条消息都是独立的顺序并不重要，则可以并发处理由 Redis 系统，可以帮助提高响应能力。 来做到这一点在 StackExchange 客户端通过设置 PreserveAsyncOrder 假到订阅服务器上所使用的连接︰

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>相关的模式和指南

实现您的应用程序中的缓存时，下面的模式也可能是相关的︰

- [缓存模式](http://msdn.microsoft.com/library/dn589799.aspx)︰ 此模式描述了如何将数据按需加载到数据存储区中缓存。 这种模式还有助于保持保留在缓存中的数据与原始数据存储区中的数据之间的一致性。
- [分片模式](http://msdn.microsoft.com/library/dn589797.aspx)提供了有关实现水平分区以帮助改善可伸缩性时存储和访问大量数据的信息。

## <a name="more-information"></a>详细信息

- 在 Microsoft 网站上的[MemoryCache 类](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx)页面
- 在 Microsoft 网站上[Azure Redis 缓存文档](https://azure.microsoft.com/documentation/services/cache/)页
- 在 Microsoft 网站上[Azure Redis 缓存 FAQ](redis-cache/cache-faq.md)页
- 在 Microsoft 网站上[配置模型](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx)页
- 在 Microsoft 网站上[的基于任务的异步模式](http://msdn.microsoft.com/library/hh873175.aspx)页
- 在 StackExchange.Redis GitHub repo[管线和复用器](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md)页
- 在 Redis 网站上的[Redis 持久性](http://redis.io/topics/persistence)页
- 在 Redis 网站上[复制页](http://redis.io/topics/replication)
- 在 Redis 网站上[Redis 群集教程](http://redis.io/topics/cluster-tutorial)页
- [分区︰ 如何拆分多个 Redis 实例间的数据](http://redis.io/topics/partitioning)Redis 网站上的页面
- 在 Redis 网站上[使用 Redis 作为一个 LRU 缓存](http://redis.io/topics/lru-cache)页
- 在 Redis 网站上[的交易记录](http://redis.io/topics/transactions)页
- 在 Redis 网站上的[Redis 安全](http://redis.io/topics/security)页
- Azure 博客上[膝周围 Redis Azure 的高速缓存](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/)页
- 在 Microsoft 网站上[一个 CentOS Azure 中的 Linux 虚拟机上运行 Redis](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx)页
- 在 Microsoft 网站上[Azure Redis 缓存的 ASP.NET 会话状态提供程序](redis-cache/cache-aspnet-session-state-provider.md)页
- 在 Microsoft 网站上[Azure Redis 缓存的 ASP.NET 输出缓存提供程序](redis-cache/cache-aspnet-output-cache-provider.md)页
- 在 Redis 网站上[的介绍，以 Redis 数据类型和抽象](http://redis.io/topics/data-types-intro)页
- 在 StackExchange.Redis 网站上[的基本用法](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md)页
- 在 StackExchange.Redis repo [Redis 中的交易记录](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md)页
- 在 Microsoft 网站上的[数据分区指南](http://msdn.microsoft.com/library/dn589795.aspx)
