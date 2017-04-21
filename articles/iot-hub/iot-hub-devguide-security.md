<properties
 pageTitle="开发人员指南-IoT 中枢控制访问 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-如何控制对 IoT 中心访问和管理安全性"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>控制访问 IoT 集线器

## <a name="overview"></a>概述

本文介绍了用于保护 IoT 集线器的选项。 IoT 中心使用*权限*来授予访问每个 IoT 中心端点。 权限限制到 IoT 集线器根据功能的访问。

本文介绍了︰

- 可以到设备或后端应用程序访问 IoT 中心授予不同的权限。
- 在身份验证过程，它用于验证权限的标记。
- 如何确定范围来限制对特定资源的访问权限的凭据。
- IoT 集线器支持的 X.509 证书。
- 自定义设备使用现有的设备标识注册表或身份验证方案的身份验证机制。

### <a name="when-to-use"></a>何时使用

您必须具有适当的权限，可以访问任何 IoT 集线器终结点。 例如，设备必须包含一个标记，该标记包含安全凭据，以及它将发送到 IoT 集线器的每条消息。

## <a name="access-control-and-permissions"></a>访问控制和权限

您可以通过以下方式来授予[权限](#iot-hub-permissions)︰

* **中心级共享访问策略**。 共享的访问策略可以授予[权限](#iot-hub-permissions)的任意组合。 可以定义策略在[Azure 的门户网站][lnk-management-portal]，或以编程方式使用[IoT 中心资源提供程序 REST Api][lnk-resource-provider-apis]。 新创建的 IoT 中心具有下面的默认策略︰

    - **iothubowner**︰ 具有所有权限策略。
    - **服务**︰ 与 ServiceConnect 权限策略。
    - **设备**︰ DeviceConnect 权限策略。
    - **registryRead**: RegistryRead 权限策略。
    - **registryReadWrite**: RegistryRead 和 RegistryWrite 的权限与策略。


* **每个设备的安全凭据**。 每个 IoT 集线器包含[设备标识注册表][lnk-identity-registry]。 为此注册表中的每个设备，您可以配置将作用于相应设备端点的**DeviceConnect**权限授予的安全凭据。

例如，在典型的 IoT 解决方案︰

- 设备管理组件使用的*registryReadWrite*策略。
- 事件处理器组件使用的*服务*策略。
- 运行库设备业务逻辑组件使用的*服务*策略。
- 单个设备使用 IoT 中枢的标识注册表中存储的凭据进行连接。

## <a name="authentication"></a>身份验证

Azure IoT 集线器通过验证的标记对共享的访问策略和设备标识注册表安全凭据授予访问权限的终结点。

安全凭据，如对称密钥，就不会发送线路上。

> [AZURE.NOTE] Azure IoT 中心资源提供程序安全通过 Azure 的订阅，因为在[Azure 资源管理器]中的所有提供程序[lnk-azure-resource-manager]。

有关如何构建并使用安全令牌的详细信息，请参阅[IoT 中心安全令牌][lnk-sas-tokens]。

### <a name="protocol-specifics"></a>协议具体内容

每个支持的协议，如 MQTT、 AMQP 和 HTTP 传输标记以不同的方式。

连接数据包时使用 MQTT，与客户机 Id，{iothubhostname} 具有 deviceId / {deviceId} 在用户名字段和密码字段中的 SAS 标记。 {iothubhostname} 应 IoT 中心 (例如，contoso.azure-devices.net) 的完整 CName。

当使用[AMQP][lnk-amqp]，IoT 集线器支持[SASL 纯][lnk-sasl-plain]和[AMQP 索赔基于的安全][lnk-cbs]。

如果您使用 AMQP 索赔基于的安全性，标准将规定如何传输这些标记。

SASL 纯为**用户名**可以是︰

* `{policyName}@sas.root.{iothubName}`如果使用集线器级别的标记。
* `{deviceId}@sas.{iothubname}`如果使用设备范围标记。

在这两种情况下，密码字段包含的标记， [IoT 中心安全令牌]所述[lnk-sas-tokens]。

HTTP 实现通过**授权**请求标头中包含有效的令牌身份验证。

#### <a name="example"></a>示例

用户名 （DeviceId 为区分大小写）︰`iothubname.azure-devices.net/DeviceId`

密码 （生成 SAS 与设备资源管理器）︰`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT 集线器 Sdk] [lnk-sdks]连接到服务时自动生成的标记。 在某些情况下，Sdk 并不支持所有协议或所有身份验证方法。

### <a name="special-considerations-for-sasl-plain"></a>SASL 纯的特殊注意事项

AMQP 使用 SASL 纯时, IoT 集线器连接的客户端可以使用同一个标记为每个 TCP 连接。 令牌到期时，TCP 连接与服务断开连接，并触发重新连接。 这种行为，而不出现问题的应用程序后端组件中，是设备端应用程序非常具有破坏性出于以下原因︰

*  网关通常代表许多设备连接。 当使用 SASL 纯，他们必须创建不同的 TCP 连接 IoT 集线器连接每个设备。 这种情况下有很大提高了电源和网络资源的消耗，并提高了设备的每个连接的反应。
* 资源有限的设备造成不良影响增加了资源使用的每个令牌到期后重新连接。

## <a name="scope-hub-level-credentials"></a>作用域中心级凭据

可以通过创建具有受限制的资源的 URI 的标记范围中心级别的安全策略。 例如，要从设备发送设备到云消息的终结点是**/devices/ {deviceId} / 消息/事件**。 您也可以用于中心级共享的访问策略**DeviceConnect**权限来签署其 resourceURI 为**/devices/ {deviceId}**的标记。 此方法不创建只能是可用设备**deviceId**代表发送邮件的标记。

这种机制是类似于[集线器事件发行者策略][lnk-event-hubs-publisher-policy]，并使您能够实现自定义身份验证方法。

## <a name="security-tokens"></a>安全令牌

IoT 中心使用安全令牌进行身份验证的设备和服务，以避免在网络上发送键。 此外，安全令牌的时间有效性和范围受到限制。 [Azure IoT 集线器 Sdk] [lnk-sdks]自动生成的标记，而无需任何特殊的配置。 某些情况下，但是，要求用户生成并直接使用安全令牌。 这些包括 MQTT、 AMQP 或 HTTP 的表面，在直接使用或标记服务模式，实现[自定义设备身份验证]中所述[lnk-custom-auth]。

IoT 中心还可以与 IoT 中心进行身份验证的设备使用[X.509 证书][lnk-x509]。 

### <a name="security-token-structure"></a>安全令牌结构
安全令牌用于授予时间界定对设备的访问权限和 IoT 集线器中的特定功能的服务。 若要确保只有经授权的设备和服务可以连接，必须使用共享的访问策略项或对称密钥标识注册表中的设备标识与存储在签名安全令牌。

令牌签名与共享的访问策略权限相关联的所有功能的共享的访问策略密钥的授权访问。 另一方面，使用设备标识的对称密钥签名的标记只授予**DeviceConnect**权限相关的设备标识。

安全令牌具有以下格式︰

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

以下是所需要的值︰

| 值 | 说明 |
| ----- | ----------- |
| {签名} | 窗体的 HMAC SHA256 签名字符串︰ `{URL-encoded-resourceURI} + "\n" + expiry`。 **重要提示**︰ 从 base64 解码密钥并将其用作键执行 HMAC SHA256 计算。 |
| {resourceURI} | 可以访问与此标记，以 IoT 集线器 （没有协议） 的主机名开头的端点的 URI 前缀 （由段）。 例如，`myHub.azure-devices.net/devices/device1` |
| {到期} | 1970 年 1 1 月的时段 00:00:00 UTC 以来的秒数的 UTF8 字符串。 |
| {URL-编码-resourceURI} | 小写的小写资源 URI 的 URL 编码 |
| {policyName} | 此标记所引用的共享访问权限策略的名称。 在设备注册表凭据引用标记的情况下不存在。 |

**在前缀的注意**︰ 段而不是字符计算的 URI 前缀。 例如`/a/b`是前缀`/a/b/c`而不是为`/a/bc`。

下面的 Node.js 段显示函数调用计算中输入标记的**generateSasToken** `resourceUri, signingKey, policyName, expiresInMins`。 以下各节详细说明了如何初始化令牌的不同用例不同的输入。

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

作为比较，是相当的 Python 代码，生成的安全令牌︰

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] 由于 IoT 中心机上验证令牌的有效期，它非常重要生成标记机的时钟偏差很小。

### <a name="use-sas-tokens-in-a-device-client"></a>在设备客户机使用 SAS 令牌

有两种方法来获取 IoT 中枢与安全令牌的**DeviceConnect**权限︰ 使用[对称设备设备标识注册表项](#use-a-symmetric-key-in-the-identity-registry)，或者使用一个[共享的访问策略注册表项](#use-a-shared-access-policy)。

记住从设备可访问的所有功能都由设计对终结点使用前缀`/devices/{deviceId}`。

> [AZURE.IMPORTANT] IoT 中心对特定设备进行身份验证的唯一方法使用设备标识的对称密钥。 在情况下当共享的访问策略用于访问设备的功能，解决方案必须考虑颁发安全令牌作为受信任的子组件的组件。

面向设备的终结点 （不考虑协议） 是︰

| 终结点 | 功能 |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | 发送设备到云的消息。 |
| `{iot hub host name}/devices/{deviceId}/devicebound` | 接收云至设备信息。 |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>在注册表中标识使用对称密钥

当使用设备标识的对称密钥来生成一个令牌 policyName (`skn`)，则忽略该标记的元素。

例如，创建用于访问所有设备功能的标记应具有以下参数︰

* 资源 URI: `{IoT hub name}.azure-devices.net/devices/{device id}`，
* 签名密钥︰ 任何对称密钥的`{device id}`标识，
* 没有策略名称，
* 任何到期时间。

使用上面的节点函数的一个例子是︰

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

结果，将访问权授予所有功能的 device1 中，就是︰

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] 可以生成使用.NET 工具[设备资源管理器中]的安全令牌[lnk-device-explorer]。

### <a name="use-a-shared-access-policy"></a>使用共享的访问策略

当创建一个令牌从一个共享的访问策略，该策略命名字段`skn`必须设置为使用该策略的名称。 它也是必需的策略授予**DeviceConnect**权限。

使用共享的访问策略用于访问设备功能的两种主要情况是︰

* [云的协议网关][lnk-endpoints]，
* [标记服务][lnk-custom-auth]用于实现自定义身份验证方案。

共享的访问策略可能可以授予访问权限与其他设备连接，因为务必要创建安全令牌时使用正确的资源的 URI。 这是尤其重要的令牌服务，已确定到特定设备使用的资源的 URI 的标记的范围。 这一点是不太相关的协议网关因为他们已经被考虑的所有设备的通信。

举一个例子，使用预先创建的共享的访问策略称为**设备**标记服务将使用以下参数创建一个令牌︰

* 资源 URI: `{IoT hub name}.azure-devices.net/devices/{device id}`，
* 签名密钥︰ 一个键的`device`策略，
* 策略名称︰ `device`，
* 任何到期时间。

使用上面的节点函数的一个例子是︰

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

结果，将访问权授予所有功能的 device1 中，就是︰

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

协议网关可以仅仅设置资源 URI 的所有设备使用相同的标记为`myhub.azure-devices.net/devices`。

### <a name="use-security-tokens-from-service-components"></a>使用安全令牌服务组件

服务组件可以只生成使用共享的访问策略授予适当的权限，如先前所述的安全令牌。

这些都是公开在终结点上的服务功能︰

| 终结点 | 功能 |
| ----- | ----------- |
| `{iot hub host name}/devices` | 创建、 更新、 检索和删除设备的标识。 |
| `{iot hub host name}/messages/events` | 接收设备到云的消息。 |
| `{iot hub host name}/servicebound/feedback` | 接收反馈的云至设备信息。 |
| `{iot hub host name}/devicebound` | 发送云至设备信息。 |

举一个例子，服务生成使用名为**registryRead**的预先创建的共享的访问策略会创建一个令牌使用以下参数︰

* 资源 URI: `{IoT hub name}.azure-devices.net/devices`，
* 签名密钥︰ 一个键的`registryRead`策略，
* 策略名称︰ `registryRead`，
* 任何到期时间。

    var 终结点 ="myhub.azure-devices.net/devices";  var policyName = 设备;  var policyKey =...;

    var 令牌 = generateSasToken （终结点、 policyKey、 policyName，60）;

结果，这会授予访问权限以读取所有设备标识，就是︰

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>支持的 X.509 证书

可以使用任何 X.509 证书以验证 IoT 集线器的设备。 这包括︰

-   **现有的 X.509 证书**。 设备可能已有与之关联的 X.509 证书。 该设备可以使用此证书与 IoT 中心进行身份验证。

-   **X-509 的证书自行生成和自签名证书**。 设备制造商或内部部署者可以生成这些证书，并在该设备上存储的相应的私钥和证书）。 您可以使用例如[OpenSSL]工具[lnk-openssl]和[Windows SelfSignedCertificate] [lnk-selfsigned]实用程序用于此目的。

-   **CA 签名的 X.509 证书**。 您还可以使用 X.509 证书生成和签名的证书颁发机构 (CA) 来识别设备和验证 IoT 集线器的设备。

设备可能用于身份验证，但不是能同时使用 X.509 证书或安全令牌。

### <a name="register-an-x509-client-certificate-for-a-device"></a>注册设备的 X.509 客户端证书

[Azure IoT 服务 SDK C#] [ lnk-service-sdk] (版本 1.0.8+) 支持注册使用 X.509 客户端证书进行身份验证的设备。 导入/导出的设备如其他 Api 还支持客户端的 X.509 证书。

### <a name="c-support"></a>C\#支持

**RegistryManager**类提供注册设备的编程方法。 特别是，在**AddDeviceAsync**和**UpdateDeviceAsync**方法使用户能够注册并更新 Iot 集线器设备标识注册表中的设备。 这两种方法接受作为输入**设备**实例。 **设备**类包括**身份验证**属性，它允许用户指定主要和次要 X.509 证书指纹。 指纹表示 （存储使用二进制 DER 编码） 的 X.509 证书的 sha-1 哈希值。 用户可以选择指定主指纹或辅助指纹。 为处理证书翻转方案支持的主要和次要的作用。

> [AZURE.NOTE] IoT 集线器不需要或存储整个 X.509 客户端证书，只有指纹。

以下是示例 C\#代码段来注册使用 X.509 客户端证书的设备︰

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>在运行时操作过程中使用 X.509 客户端证书

[Azure IoT 设备 SDK.net] [ lnk-client-sdk] (版本 1.0.11+) 支持使用 X.509 客户端证书。

### <a name="c-support"></a>C\#支持

**DeviceAuthenticationWithX509Certificate**的类支持使用 X.509 客户端证书的 **DeviceClient**实例的创建。

下面是示例代码段︰

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>自定义设备的身份验证

您可以使用 IoT 集线器[设备标识注册表][lnk-identity-registry]配置每台设备的安全凭据和访问控制使用[标记][lnk-sas-tokens]。 但是，如果 IoT 解决方案已有自定义设备标识注册表和/或身份验证方案中的一项重大的投资，可此现有的基础结构与集成 IoT 集线器通过创建*令牌服务*。 这种方式，可以使用其他 IoT 功能，您的解决方案中。

标记的服务是一种自定义云服务。 它使用**DeviceConnect**权限 IoT 中心*共享的访问策略*来创建*设备范围*标记。 这些标记启用 IoT 集线器连接的设备。

  ![该令牌服务模式的步骤][img-tokenservice]

这些是标记服务模式的主要步骤︰

1. 创建 IoT 中心的**DeviceConnect**权限策略 IoT 中心共享访问。 您可以在[Azure 门户]创建此策略[lnk-management-portal]或以编程方式。 标记的服务使用此策略进行签名创建的标记。
2. 当设备需要访问 IoT 集线器时，它从令牌服务请求签名的标记。 该设备可以使用您的自定义设备标识注册表/身份验证方案，以确定设备标识标记服务用来创建令牌进行身份验证。
3. 令牌服务返回一个标记。 使用创建该标记`/devices/{deviceId}`为`resourceURI`，与`deviceId`作为经过身份验证的设备。 令牌服务使用的共享的访问策略来构造该标记。
4. 该设备直接与 IoT 中心使用令牌。

> [AZURE.NOTE] 您可以使用.NET 类[SharedAccessSignatureBuilder] [lnk-dotnet-sas]或 Java 类[IotHubServiceSasToken] [lnk-java-sas]令牌服务中创建一个标记。

标记的服务可以根据需要设置令牌过期。 当令牌到期时，IoT 集线器服务器的设备连接。 然后，设备必须申请新的令牌令牌服务。 如果您使用短的过期时间，这会增加设备和令牌服务上的负载。

连接到网络集线器的设备，还必须添加它到 IoT 集线器设备标识注册表 — — 即使此设备正在使用一个标记并不是设备键连接。 因此，您可以继续使用通过启用或禁用设备标识[IoT 中心标识注册表]中的每个设备的访问控制[lnk-identity-registry]设备时使用令牌进行身份验证。 这缓解了长到期时间使用令牌的风险。

### <a name="comparison-with-a-custom-gateway"></a>与自定义网关的比较

该令牌服务模式是 IoT 集线器来实现自定义标识注册表/身份验证方案的推荐的方式。 因为 IoT 中心继续处理解决方案通信的大多数建议。 但是，一些位置的自定义身份验证方案这样缠结在一起具有处理所有通信 （*自定义网关*） 服务是必需的协议的情况。 此示例是[传输层安全 (TLS) 和预共享的密钥 (PSKs)][lnk-tls-psk]。 有关详细信息，请参阅[协议网关][lnk-protocols]主题。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关控制访问 IoT 中心的详细信息。

## <a name="iot-hub-permissions"></a>IoT 中心的权限

下表列出可用于控制对 IoT 中心访问的权限。

| 权限            | 备注 |
| --------------------- | ----- |
| **RegistryRead**      | 授权读取设备标识注册表访问权限。 有关详细信息，请参阅[设备标识注册表][lnk-identity-registry]。 |
| **RegistryReadWrite** | 授予对读取和写入访问设备标识注册表。 有关详细信息，请参阅[设备标识注册表][lnk-identity-registry]。 |
| **ServiceConnect**    | 授予访问云面向服务的通信和监控的终结点。 例如，它授予权后端云服务来接收设备到云邮件、 发送云至设备信息和检索相应的送达回执。 |
| **DeviceConnect**     | 授予对面向设备的通信端点的访问权限。 例如，它授予权限设备到云的邮件收发云至设备信息。 设备使用此权限。 |

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在，您已学习了如何控制访问 IoT 中心，您可以关注在下列主题中开发人员指南 》:

- [使用设备 twins 同步状态和配置][lnk-devguide-device-twins]
- [在设备上的直接方法调用][lnk-devguide-directmethods]
- [多台设备上的计划作业][lnk-devguide-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣下面 IoT 集线器教程︰

- [开始使用 Azure IoT 集线器][lnk-getstarted-tutorial]
- [如何发送 IoT 中枢云至设备信息][lnk-c2d-tutorial]
- [如何处理 IoT 集线器设备到云的消息][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
