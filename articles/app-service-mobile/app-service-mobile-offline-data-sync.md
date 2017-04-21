<properties
    pageTitle="在 Azure 的移动应用程序的脱机数据同步 |Microsoft Azure"
    description="概念的引用和 Azure 移动应用程序的脱机数据同步功能概述"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>在 Azure 的移动应用程序的脱机数据同步

## <a name="what-is-offline-data-sync"></a>什么是脱机数据同步？

脱机数据同步是在客户端和服务器的 Azure 的移动应用程序使开发人员能够创建不使用网络连接功能的应用程序轻松地 SDK 功能。

当您的应用程序处于脱机模式时，用户仍然可以创建和修改将被保存到一个本地存储的数据。 当应用程序返回联机状态时，它可以与您 Azure 的移动应用程序的后端同步本地更改。 该功能还包括用于检测冲突时在客户端和后端上更改相同的记录的支持。 服务器或客户端上，然后可以处理冲突。

脱机同步具有许多优点︰

* 通过缓存服务器本地设备上的数据来提高应用程序响应能力
* 创建可靠的应用程序保持可用的网络问题时
* 允许最终用户可以创建和修改数据，即使没有网络访问，与很少或没有连接支持方案
* 在多个设备之间同步数据和两台设备修改同一个记录时检测冲突
* 高延迟或计量网络限制网络使用

下面的教程演示了如何添加到您使用 Azure 移动应用程序的移动客户端脱机同步︰

* [Android︰ 启用脱机同步]
* [iOS︰ 启用脱机同步]
* [Xamarin iOS︰ 启用脱机同步]
* [Xamarin Android︰ 启用脱机同步]
* [Xamarin.Forms︰ 启用脱机同步](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [通用的 Windows 平台︰ 启用脱机同步]

## <a name="what-is-a-sync-table"></a>同步表是什么？

若要访问"/ 表"端点，Azure 移动客户端 Sdk 提供接口如`IMobileServiceTable`（.NET 客户端 SDK） 或`MSTable`（iOS 客户端）。 这些 Api 直接连接到 Azure 的移动应用程序的后端和客户端设备不具有网络连接将失败。

若要支持脱机使用，您的应用程序应改用*同步表*的 Api，如`IMobileServiceSyncTable`（.NET 客户端 SDK） 或`MSSyncTable`（iOS 客户端）。 统一的 CRUD 操作 （创建、 读取、 更新和删除） 起作用同步表 Api，但现在它们将读取或写入到*本地存储区*。 在执行任何同步表操作之前，必须初始化本地存储区。

## <a name="what-is-a-local-store"></a>什么是本地存储？

本地存储是在客户端设备上的数据持久性层。 Azure 的移动应用程序客户端 Sdk 提供了一个默认的本地存储库实现。 在窗口、 Xamarin 和 Android，它基于 SQLite;在 iOS 中，它基于核心数据。

若要使用 Windows Phone 或 Windows 商店 8.1 基于 SQLite 的实现，您需要安装一个 SQLite 扩展。 有关详细信息，请参阅[通用 Windows 平台︰ 启用脱机同步]。 Android 和 iOS 附带 SQLite 的版本中设备操作系统本身，因此不需要引用 SQLite 的版本。

开发人员还可以实施其自己的本地存储区。 例如，如果您想要在移动客户端以加密格式存储数据，您可以定义 SQLCipher 用于加密的本地存储。

## <a name="what-is-a-sync-context"></a>什么是同步上下文？

*同步上下文*是与移动客户端对象相关联 (如`IMobileServiceClient`或`MSClient`) 并跟踪与同步表所做的更改。 同步上下文将保持*操作队列*保留 CUD 操作 （创建、 更新、 删除） 的排序的列表，以后将发送到服务器。

本地存储之前与同步上下文使用一个初始化方法，如`IMobileServicesSyncContext.InitializeAsync(localstore)`在[.NET 客户端 SDK]。

## <a name="how-sync-works"></a>同步脱机工作

使用同步的表，当客户端代码控制时将与 Azure 的移动应用程序的后端同步本地更改。 任何发送到后端*推入*本地更改调用之前。 同样，本地存储填充新数据只有在没有对*提取*数据的调用时。

* **推式**︰ 推送操作的同步上下文，并将所有 CUD 更改都发送自上次推后。 请注意，不能发送只有单个表的更改，因为否则操作无法被发送顺序。 强制执行一系列的其余部分调用您 Azure 的移动应用程序端，这反过来又会修改服务器数据库。

* **拉**︰ 在每个表的基础上执行的请求订阅和可自定义的查询来检索服务器数据的子集。 Azure 移动客户端 Sdk 然后将得到的数据插入到本地存储区。

* **隐式将**︰ 如果拉对表具有本地更新挂起执行，拉第一次将同步上下文上执行推。 这将有助于最小化更改的已排队和来自服务器的新数据之间的冲突。

* **增量同步**︰ 抽取操作的第一个参数是仅在客户端使用的*查询名称*。 如果您使用非空查询名称，Azure 移动 SDK 将执行*增量同步*。
  每次抽取操作返回的结果中，最新一组`updatedAt`，结果集从时间戳存储在 SDK 本地系统表。 后续的请求操作仅将在该时间戳后检索记录。

  若要使用增量同步，您的服务器必须返回有意义`updatedAt`值，而且还必须支持按该字段排序。 但是，由于 SDK updatedAt 字段中添加自己的排序，因此不能使用有自己拉查询`$orderBy$`子句。

  查询名称可以是任何字符串选择，但它必须是唯一的应用程序中每个逻辑查询。
  否则为不同请求的操作可能会覆盖相同的增量同步时间戳，您的查询可以返回不正确结果。

  如果查询中有一个参数，以创建一个唯一的查询名称的一种方法是将参数值。
  例如，如果将筛选的用户 id，您查询的名称可能是，如下所示 （在 C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  如果您想要抛弃增量同步，通过`null`作为查询 id。 在这种情况下，将检索所有记录在每次调用`PullAsync`，这是有可能效率低下。

* **Purging**︰ 您可以清除本地存储区使用的内容`IMobileServiceSyncTable.PurgeAsync`。
  如果您有陈旧数据在客户端数据库中，或者如果您希望放弃所有挂起的更改，这可能是有必要。

  清除将清除本地存储区中的表。 如果有挂起的同步与服务器数据库的操作，请清除将引发异常，除非*强制清除*参数设置。

  为客户端上的陈旧数据的示例，假设在"todo 列表"的示例中，device1 中只提取未完成的项目。 然后，todoitem"购买牛奶"被标记为在服务器上完成由另一个设备。 但是，device1 中仍然可以"购买牛奶"todoitem 本地存储区中因为它只提取未标记为完成的项目。 清除将清除此过时的项目。

## <a name="next-steps"></a>下一步行动

* [iOS︰ 启用脱机同步]
* [Xamarin iOS︰ 启用脱机同步]
* [Xamarin Android︰ 启用脱机同步]
* [通用的 Windows 平台︰ 启用脱机同步]

<!-- Links -->
[.NET 客户端 SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android︰ 启用脱机同步]: app-service-mobile-android-get-started-offline-data.md
[iOS︰ 启用脱机同步]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS︰ 启用脱机同步]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android︰ 启用脱机同步]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[通用的 Windows 平台︰ 启用脱机同步]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
