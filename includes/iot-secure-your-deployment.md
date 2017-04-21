# <a name="securing-your-iot-deployment"></a>保证 IoT 部署

本文针对保护基于 Azure IoT Internet 内容 (IoT) 基础结构提供下一级别的详细信息。 它链接到配置和部署每个组件的实现级别的详细信息。 它还提供了比较和竞争的各种方法之间选择。

保护 Azure IoT 部署可以划分为以下三个安全区域中︰

- **设备安全**︰ 固定 IoT 设备同时部署处于放任状态。
- **连接安全**︰ 确保 IoT 集线器 IoT 设备之间传输的所有数据是机密和篡改的。
- **云安全**︰ 提供一种方法来保护数据中, 移动并存储在云中。

![三个安全区域][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>安全设备置备和身份验证

Azure IoT 套件可以通过以下两种方法保护 IoT 设备︰

- 通过为每个设备，该设备可用于与 IoT 中心通信提供一个唯一的标识键 （安全令牌）。

- 通过使用设备上的[X.509 证书][lnk-x509]和作为一种手段，以验证到 IoT 集线器设备的专用密钥。 此身份验证方法可确保设备上的专用密钥不知道外部设备在任何时间，提供了更高级别的安全性。

安全令牌方法提供了身份验证的每一个调用都通过设备 IoT 集线器通过将每个调用的对称密钥相关联。 基于 X.509 身份验证允许 IoT 设备的身份验证在物理层作为建立 TLS 连接的一部分。 基于安全令牌的方法可以 X.509 身份验证，这是一种不太安全模式不能使用。 这两种方法之间的选择主要被受如何安全设备的身份验证需要和安全上的存储设备 （若要安全地存储私有密钥） 的可用性。

## <a name="iot-hub-security-tokens"></a>IoT 中心安全令牌

IoT 中心使用安全令牌进行身份验证的设备和服务，以避免在网络上发送的键。 此外，安全令牌的时间有效性和范围受到限制。 Azure IoT 集线器 Sdk 自动生成标记，而无需任何特殊的配置。 某些情况下，但是，要求用户生成并直接使用安全令牌。 其中包括直接使用 MQTT、 AMQP 或 HTTP 的表面或标记服务模式的实现。

在下列文章中找不到结构的安全令牌和其用法的更多详细信息︰

-   [安全令牌结构][lnk-security-tokens]
-   [为设备使用 SAS 标记][lnk-sas-tokens]

每个 IoT 中枢有[设备标识注册表][ lnk-identity-registry] ，可以用来创建每个设备资源，在服务中，如队列，其中包含正在进行云至设备信息，并允许访问面向设备的端点。 IoT 中心标识注册表提供设备标识和解决方案的安全密钥的安全存储。 单个或成组的设备标识可以添加到允许列表中或阻止名单中，启用完全控制设备访问。 下列文章结构的设备标识注册表中提供更多详细信息和支持的操作。

[IoT 集线器支持 MQTT、 AMQP 和 HTTP 等协议][lnk-protocols]。 两种协议各自以不同的方式使用安全令牌从 IoT 设备到 IoT 中心︰

- AMQP: SASL 普通和基于 AMQP 声明的安全({policyName}@sas.root.{iothubName}在集线器级别的标记;{deviceId} 如果设备范围标记)。

- MQTT︰ 连接数据包使用 {deviceId} {客户机 Id}，{IoThubhostname} / {deviceId} 在**用户名**字段和**密码**字段中的 SAS 标记。

- HTTP︰ 授权请求标头中是有效的标记。

IoT 集线器设备标识注册表可用来配置每台设备的安全凭据和访问控制。 但是，如果 IoT 解决方案已[自定义设备标识注册表和/或身份验证方案]中有大量的投资，[lnk-custom-auth]，它可以集成到 IoT 集线器与现有基础结构通过创建令牌服务。

### <a name="x509-certificate-based-device-authentication"></a>X.509 证书基于设备的身份验证

[基于设备的 X.509 证书]的使用[lnk-use-x509]和它相关联的私钥和公钥的密钥对在物理层允许额外的身份验证。 私钥安全存储在该设备中并不是外部设备可检测到。 X.509 证书包含有关该设备，如设备 ID，以及其他组织的详细信息。 通过使用私钥生成的证书签名。

高级设备资源调配流程︰

- 将关联到一个物理设备 – 设备标识和/或 X.509 证书在设备制造或调试期间与设备相关联的标识符。

- 创建相应的标识项 IoT 中心 – 设备标识和 IoT 集线器设备注册表中的关联的设备信息。

- 在 IoT 集线器设备注册表中安全地存储 X.509 证书指纹。

### <a name="root-certificate-on-device"></a>在设备上的根证书

同时建立安全的 TLS 连接 IoT 中枢，IoT 设备验证 IoT 中心使用根证书的设备 SDK 的一部分。 对于 C 客户端 SDK 证书位于文件夹下的"\\c\\证书"repo 的根目录下。 这些根证书寿命较长，但它们仍然可能过期或被吊销。 如果无法更新设备上的证书，该设备可能不能随后连接到 IoT 集线器 （或任何其他云服务）。 用一种办法一旦部署 IoT 设备更新根证书将有效地降低这种风险。

## <a name="securing-the-connection"></a>保护连接

使用传输层安全性 (TLS) 标准保护 IoT 集线器 IoT 设备之间的互联网连接。 Azure IoT 支持[TLS 1.2][lnk-tls12]，TLS 1.1 和 TLS 1.0，按此顺序。 为向后兼容性提供支持 TLS 1.0 的。 建议使用 TLS 1.2，因为它提供了最好的安全。

Azure IoT 套件支持下面的密码套件，按此顺序。

| 编码器套件 | 长度 |
|--------------|--------|
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 （均衡 7680 位 RSA） FS | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 ECDH secp256r1 (0xc027) （均衡 FS 3072 位 RSA） | 128    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 （均衡 7680 位 RSA） FS           | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 （均衡 FS 3072 位 RSA）           | 128    |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>保护云

Azure IoT 中心允许的[访问控制策略]的定义[lnk-protocols]为每个安全参数。 它使用的以下权限集授予 IoT 中枢的终结点的每个访问。 权限限制到 IoT 集线器根据功能的访问。

- **RegistryRead**。 设备标识注册表中授予读取访问权限。 有关详细信息，请参阅[设备标识注册表][lnk-identity-registry]。

- **RegistryReadWrite**。 授予对读取和写入访问设备标识注册表。 有关详细信息，请参阅[设备标识注册表][lnk-identity-registry]。

- **ServiceConnect**。 授予访问云面向服务的通信和监控的终结点。 例如，它授予权后端云服务来接收设备到云邮件、 发送云至设备信息和检索相应的送达回执。

- **DeviceConnect**。 授予对面向设备的通信端点的访问权限。 例如，它授予权限设备到云的邮件收发云至设备信息。 设备使用此权限。

有两种方法来获取**DeviceConnect**权限 IoT 中枢与[安全令牌][lnk-sas-tokens]︰ 使用设备标识密钥或共享的访问策略项。 此外，就一定要注意从设备可访问的所有功能都由设计对终结点使用前缀`/devices/{deviceId}`。

[服务组件可以只生成安全令牌][lnk-service-tokens]使用共享访问策略授予适当的权限。

Azure IoT 中心和其他服务可能是解决方案的一部分，允许用户使用 Azure Active Directory 管理。

数据 ingested Azure IoT 集线器可消耗的各种服务，例如 Azure 流分析和 Azure blob 存储。 这些服务允许管理访问权限。 阅读有关这些服务和下面的可用选项的详细信息︰

- [Azure DocumentDB][lnk-docdb]︰ 半结构化数据管理设备的元数据的可伸缩的、 完全编入索引的数据库服务您配置，如属性、 配置和安全属性。 DocumentDB 提供了高性能、 高吞吐量的处理、 数据和丰富的 SQL 查询接口索引架构不可知。

- [Azure 流分析][lnk-asa]︰ 实时流处理中云，使您可以快速开发和部署一个低成本分析解决方案，可以发现从设备、 传感器、 基础架构和应用程序的实时见解。 同时仍达到高吞吐量、 低延迟和可恢复性，这完全管理服务中的数据可以扩展到任何卷。

- [Azure 应用程序服务][lnk-appservices]︰ 云平台来构建强大的 web 和移动应用程序连接到数据的任何地方;在云中还是内部。 构建合作为 iOS 和 Android，移动应用程序。 与您的软件即服务 (SaaS) 的框连接到云服务的很多企业级应用程序和企业级应用程序集成。 在您喜欢的语言和 IDE （.NET、 NodeJS、 PHP、 Python 或 Java） 比以往任何时候都更快地生成 web 应用程序和 Api 的代码。

- [逻辑应用程序][lnk-logicapps]: Azure 应用程序服务的逻辑应用程序功能有助于集成到您现有的业务线系统 IoT 解决方案和自动化工作流程。 逻辑应用程序，开发人员开始从触发器，然后执行一系列步骤的设计工作流 — — 规则和使用功能强大的连接器来与您的业务流程集成的操作。 逻辑应用程序提供的框连接到 SaaS，基于云的一个庞大的生态系统和内部部署的应用程序。

- [Azure blob 存储][lnk-blob]︰ 您的设备发送到云中的数据可靠、 经济的云存储。

## <a name="conclusion"></a>结论

本文概述的实现设计和部署使用 Azure IoT IoT 基础结构级别的详细信息。 配置每个组件进行保护是保护整体 IoT 基础结构中的关键。 在 Azure IoT 提供的设计选项提供一定程度的灵活性和可选择性;但是，每种选择可能带来安全隐患。 建议的每一个这些选择进行评估通过风险/成本评估。

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
