<properties 
    pageTitle="事件集线器身份验证和安全模型概述 |Microsoft Azure"
    description="事件集线器身份验证和安全模型概述。"
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>事件集线器身份验证和安全模型概述

事件集线器安全模型满足以下要求︰

- 唯一设备提供有效的凭据，则可以将数据发送到事件中心。
- 设备无法模拟另一个设备。
- 可以阻止欺诈设备将数据发送到事件中心。

## <a name="device-authentication"></a>设备验证

事件集线器安全模型基于[共享访问签名 (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md)标记和*事件发行者*的组合。 事件发布者定义事件中心的虚拟的终结点。 发布服务器仅用于将消息发送到事件中心。 不能从发布服务器接收邮件。

通常情况下，事件中心采用了一个出版商出版的每个设备。 发送到任何事件中心出版的所有消息都是排在该事件的中心。 细粒度的访问控制和调节，使出版商。

每台设备分配一个唯一的标记，上载到该设备。 标记会产生的每个唯一的标记为不同的唯一发布服务器授予访问权限。 有标记的设备只能为一家出版社，但没有其他发布者发送。 如果多个设备共享同一个标记，则这些设备的每个共享发布服务器。

尽管不建议这样做，很可能使以直接访问权限授予事件中心的标记的设备。 持有此标记的任何设备可以直接向该事件中心发送消息。 这种设备不受限制。 此外，该设备无法将发送到该事件中心列入黑名单。

所有标记都使用 SA 密钥都签名。 通常情况下，所有标记都用相同的密钥都签名。 设备没有察觉到的键;这样可以防止设备制造的标记。

### <a name="create-the-sas-key"></a>创建 SA 密钥

当创建事件集线器的命名空间，Azure 事件集线器生成名为**RootManageSharedAccessKey**的 256 位 SA 密钥。 此密钥的授权发送、 欣赏，并管理到命名空间的权限。 您可以创建其他的密钥。 建议您生成授予，发送到该特定事件中心的权限项。 有关本主题的其余部分，则假定，此项名为`EventHubSendKey`。

下面的示例创建一个仅发送键创建事件中心时︰

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>生成标记

您可以生成使用 SA 密钥令牌。 您必须生成每个设备只有一个令牌。 然后可以使用以下方法生成标记。 使用**EventHubSendKey**密钥生成所有标记。 每个标记分配唯一的 URI。

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

调用此方法时，应将 URI 指定为`//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`。 URI 的所有标记，是相同的除了`PUBLISHER_NAME`，这应该是每个标记的不同。 理想情况下，`PUBLISHER_NAME`表示接收此标记的设备的 ID。

此方法生成一个令牌具有以下结构︰

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

在 1970 年 1 月 1 日之后的秒数指定令牌过期时间。 下面是一个标记的一个示例︰

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

通常情况下，标记具有类似于或超过该设备的生命周期内的使用期限。 如果设备具有的功能，来获取新的令牌，可以使用标记与较短的生命周期。

### <a name="devices-sending-data"></a>设备发送数据

一旦创建标记，每个设备提供它自己的唯一标记。

当设备将数据发送到事件中心时，设备标记与发送请求的标记。 为了防止攻击者从窃听和窃取该标记，设备和事件中心之间的通信必须通过加密通道。

### <a name="blacklisting-devices"></a>黑名单的设备

如果一个令牌被盗的攻击，攻击者可以模拟的设备被盗的标记。 黑名单设备将导致设备不可用直到设备提供使用不同的出版商一个新标记。

## <a name="authentication-of-back-end-applications"></a>后端应用程序的身份验证

验证后端应用程序使用设备生成的数据，事件集线器，采用了类似于用于服务总线主题模型的安全模型。 事件集线器使用者组等同于服务总线主题的订阅。 客户端可以创建使用者组，如果创建使用者组请求都伴有一个令牌授予管理权限，对于事件中心，或对事件中心所属的命名空间。 允许客户端使用的使用者组中的数据如果接收请求都伴有一个标记，用于接收该使用者组、 事件中心，或对事件中心所属的命名空间的权限授予。

单个订阅服务总线的当前版本不支持 SAS 规则。 事件集线器使用者组也是如此。 将这两个功能在将来添加 SAS 支持。

在没有个别使用者组 SAS 身份验证的情况下，您可以使用 SAS 键使用公共密钥保护所有使用者组。 此方法使应用程序能够使用从事件中心的使用者组中的任何数据。

## <a name="next-steps"></a>下一步行动

若要了解有关事件集线器的详细信息，请访问下面的主题︰

- [事件集线器概述]
- [排队的消息传送解决方案]使用服务总线队列。
- 完整的[示例应用程序，它使用事件集线器]。

[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[排队的消息传送解决方案]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
