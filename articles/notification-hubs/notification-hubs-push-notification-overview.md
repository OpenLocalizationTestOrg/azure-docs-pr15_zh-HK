<properties
    pageTitle="Azure 通知集线器"
    description="了解如何在 Azure 中使用推式通知。 用 C# 使用.NET API 编写的代码样本。"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure 通知集线器

##<a name="overview"></a>概述

Azure 通知集线器提供易于使用、 多平台、 向外扩展的推式基础架构，使您可以从任何后端 （在云或内部） 将移动推通知发送到任何移动平台。

通过通知集线器可以很容易地发送跨平台的、 个性化的推式通知，抽象的不同平台通知系统 （企业型） 的详细信息。 一次 API 调用，可以将各个用户或全体受众段包含数以百万计的用户，他们所有的设备的目标。

企业和消费者的情况下，可以使用通知集线器。 例如︰

- 将重大新闻通知发送给数以百万计和低的延迟时间 （通知集线器动力 Bing 在所有窗口和 Windows Phone 设备上预安装的应用程序）。
- 发送给用户段基于位置的优惠券。
- 将事件通知发送到的用户或组对体育/财务/游戏应用程序。
- 通知用户的企业如新邮件/电子邮件和潜在销售顾客的事件。
- 发送一-时间-密码所需的多因素身份验证。



##<a name="what-are-push-notifications"></a>什么是推送通知？

智能手机和平板电脑可以""时通知用户已发生事件。 这些通知可以有多种形式。

在 Windows 应用商店和 Windows Phone 应用程序通知可_祝酒_的形式︰ 一个无模式窗口出现，其中一种声音，信号的新通知。 其他受支持的通知类型包括_平铺_、_原始_的和_标记_通知。 通知 Windows 设备上受支持的类型的详细信息，请参阅[拼贴、 徽章和通知](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)。

苹果的 iOS 设备上推同样通知对话框，要求用户查看或关闭通知的用户。 单击**视图**打开的应用程序接收邮件。 IOS 通知的详细信息，请参阅[iOS 通知](http://go.microsoft.com/fwlink/?LinkId=615245)。

推式通知帮助移动设备显示同时高效节能的最新信息。 通知可以发送的后端系统向移动设备即使在设备上的相应应用程序处于非活动状态。 推式通知是至关重要的组件，用于使用者应用程序，它们用于提高应用程序服务和使用情况。 通知也是适用于企业中，增加了员工响应业务事件的最新信息。

移动合作方案的一些特定示例包括︰

1.  更新 Windows 8 或 Windows Phone 上的平铺当前的财务信息。
2.  警告使用 toast 某些工作项已指派给该用户，基于工作流的企业应用程序中的用户。
3.  显示与当前销售数徽章在 CRM 的应用程序 （如 Microsoft Dynamics CRM) 处于领先的地位。

##<a name="how-push-notifications-work"></a>如何将推送通知的工作

推式通知被通过称为_平台通知系统_（企业型） 的特定于平台的基础架构。 企业型提供最朴素的函数 （即，广播，个性化设置不支持），并且没有通用的界面。 例如，要将通知发送到 Windows 应用商店应用程序，开发人员必须联系 WNS （Windows 通知服务）。 若要将通知发送到 iOS 设备，该开发人员有联系 APNS （苹果推送通知服务），并再次发送该邮件。 通过提供一个通用的界面，以及其他的功能来支持每个平台间的推式通知帮助 azure 通知集线器。

从较高层次，但是，所有平台通知系统都遵循相同的模式︰

1.  客户端应用程序联系人企业型检索_处理_它。 句柄类型取决于系统。 它是一个 URI 的 WNS，或"通知通道"。 对于 APNS，它是一个令牌。
2.  客户端应用程序将此句柄存储在 app_后端_供以后使用。 对于 WNS 后, 端通常是云服务。 对于苹果，它被称为_提供程序_。
3.  若要发送推式通知后, 端应用程序联系，使用该句柄以面向特定的客户端应用程序实例企业型。
4.  企业型将转发到该句柄由指定的设备的通知。

![][0]

##<a name="the-challenges-of-push-notifications"></a>推式通知的所面临的挑战

而这些系统却非常强大，他们仍然保留工作量给应用程序开发人员为了实现即使是常见的推送通知方案，如广播或分段用户向其发送推式通知。

推式通知是一种在移动应用程序的云服务最常请求的功能。 这样做的原因是使它们工作时所需的基础结构是相当复杂和大部分主要业务逻辑的应用程序无关。 一些构建按需推送基础结构所面临的挑战包括︰

- **平台的依赖性。** 若要将通知发送到不同的平台上的设备，必须在后端编码多个接口。 不仅是不同的低级别的详细信息，但通知 （拼贴、 祝酒或标记） 的演示文稿也是取决于平台。 这些差异可能会导致复杂和难维护后端代码。

- **小数位数。** 扩展此基础结构包括两个方面︰
    + 企业型的指南，每次启动该应用程序，则必须刷新设备标记。 这将导致大量的通信 （以及后续的数据库访问） 只是为了保持设备标记为最新。 当设备的数量增长 （可能是向数以百万计） 时，创建和维护这一基础结构的成本是 nonnegligible。

    + 大多数的 PNSs 不支持广播到多个设备。 它遵循广播给数百万个设备导致数以百万计对 PNSs 的调用。 扩展这些请求是能够不常用，因为应用程序开发人员通常希望降低总滞后时间。 例如，用于接收消息的最后一个设备不应收到通知发送通知后 30 分钟，与很多情况下它可能会削弱有推式通知的目的。
- **路由选择。** PNSs 提供一种方法来向设备发送一条消息。 但是，在大多数应用程序通知了面向用户和/或利益集团 （例如，所有员工分配给特定客户帐户）。 在这种情况下，为了将通知路由到正确的设备中后, 端应用程序必须维护将利益集团与设备标记相关联的注册表。 这种开销将添加到应用程序的市场和维护成本的总时间。

##<a name="why-use-notification-hubs"></a>为什么要使用通知集线器？

集线器消除复杂性的通知︰ 您不需要管理的难题推式通知。 相反，您可以使用通知中心。 通知集线器使用完整多平台，向外扩展的推式通知的基础结构，并显著减少推特定于的代码运行在应用程序的后端。 通知集线器实施推入基础结构的所有功能。 设备仅负责注册企业型处理和后端负责将独立于平台的消息发送到用户或感兴趣的组，如下图所示︰

![][1]


通知集线器提供现成的推式通知基础结构具有以下优点︰

- **多个平台。**
    +  支持所有主要的移动平台。 通知集线器可以向 Windows 应用商店、 iOS、 Android 和 Windows Phone 应用程序发送推式通知。

    +  通知集线器提供一个通用的界面，若要将通知发送到所有支持的平台。 特定于平台的协议不是必需的。 后端应用程序可以以特定于平台的或独立于平台的格式来发送通知。 应用程序只与通知集线器。

    +  设备句柄管理。 通知集线器维护注册表句柄和 PNSs 的反馈。

- **与任何后端的工作原理**︰ 云或内部，.NET、 PHP、 Java、 节点，等等。

- **小数位数。** 通知集线器扩展到数以百万计的而不需要重新设计或 shard 的设备。


- **一组丰富的传送模式**︰

    - *广播*︰ 允许几乎同时广播给数百万个设备与一个 API 调用。

    - *单址广播/多址广播*︰ 推送到标记表示单个用户，包括所有他们的设备;或更多;例如，单独的窗体的因素 (tablet 和电话)。

    - *分段*︰ 推到复杂段定义的标记表达式 （例如，在纽约杨基棒球队后面的设备）。

    每个设备，那么当将其句柄发送到通知集线器，可以指定一个或多个_标记_。 有关[标记]的详细信息。 标记没有被预先设置或释放。 标记提供了一种将通知发送给用户或利益集团的简单方法。 由于标签可以包含任何应用程序特定的标识符 （如用户或组 Id)，其使用释放应用程序后端的存储和管理设备句柄的工作负担。

- **个性化**︰ 每个设备可以有一个或多个模板，而不会影响后端代码实现每台设备本地化和个性化设置。

- **安全**︰ 共享访问机密 (SAS) 或联合身份验证。

- **丰富的遥测**︰ 可用在门户和编程方式。


##<a name="integration-with-app-service-mobile-apps"></a>与应用程序服务移动应用程序的集成

为了便于跨 Azure 服务无缝且统一的经验，[应用程序服务移动应用程序]具有对推式通知使用通知集线器的内置支持。 [应用程序服务移动应用程序]提供一个高度可扩展、 全局可用的移动应用程序开发平台，为企业开发人员和系统集成商为移动开发人员带来了丰富的功能。

移动应用程序开发人员可以利用通知集线器与以下工作流程︰

1. 检索设备企业型句柄
2. 通过方便的移动应用程序客户端 SDK 注册 API 通知集线器中注册设备和[模板]
    + 请注意，移动应用程序对出于安全目的的登记中的去除，离开了所有的标记。 处理通知集线器从您的后端，直接以将标签与设备相关联。
3. 从您的应用程序后端用通知集线器发送通知

下面是一些为开发人员提供这种集成带来的便利︰

- **移动应用程序客户端 Sdk。** 这些多平台 Sdk 登记提供简单的 Api，并谈到自动链接移动应用程序与该通知中心。 开发人员不需要通知集线器凭据通过更深入地介绍和使用附加的服务。
    + Sdk 自动标记移动应用程序已经过身份验证的用户 ID 可以推送到用户方案与给定的设备。
    + 自动，Sdk 将使用 GUID 作为移动应用程序的安装 ID 注册通知集线器，开发人员节省维护多个服务 Guid 的麻烦。
    
- **安装模型。** 移动应用程序可用于通知集线器最新推模型来表示在 JSON 安装推送通知服务与对齐且易于使用的设备与相关联的所有强制属性。

- **灵活性。** 开发人员总是可以选择用直接通知集线器工作甚至就地加盟。

- **在[Azure 门户]的综合的体验。** 一种功能的可视表示形式在移动应用程序和开发人员可以轻松地使用通过移动应用程序关联的通知中心推移。



##<a name="next-steps"></a>下一步行动

在这些主题中，可以找到有关通知集线器的更多信息︰

+ **[客户怎样使用通知集线器]**

+ **[通知集线器教程和参考线]**

+ **通知集线器入门教程**([iOS]， [Android]、 [Windows 通用]、 [Windows Phone]、 [Kindle]、 [Xamarin.iOS]、 [Xamarin.Android])

有关推式通知位于以下的相关.NET 托管 API 引用︰

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [客户怎样使用通知集线器]: http://azure.microsoft.com/services/notification-hubs
  [通知集线器教程和参考线]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Windows 世界]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [应用程序服务移动应用程序]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [模板]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure 门户]: https://portal.azure.com
  [标记]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
