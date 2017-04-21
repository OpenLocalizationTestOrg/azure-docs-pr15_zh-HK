# <a name="internet-of-things-security-from-the-ground-up"></a>从创建之初的互联网的内容安全

互联网的东西 (IoT) 会引起全球范围的公司的唯一安全、 隐私和法规遵从性挑战。 与不同的是，这些问题围绕软件和实现方式的传统网络技术、 IoT 涉及网络和物理世界融合时，会发生什么情况。 保护 IoT 解决方案需要确保安全设置的设备，这些设备和云，云中的处理和存储过程中的安全数据保护之间的安全连接。 针对此类功能正常工作，但是，是资源有限的设备、 地理分布，部署和大量的设备在一个解决方案中。

本文探讨了 Microsoft Azure IoT 套件如何提供安全和专用联网云解决方案。 Azure IoT 套件提供一个完整的端到端解决方案，带有内置组成的基本组件的每个阶段的安全。 在 Microsoft，开发安全软件是软件工程实践，植根在我们几十年的一部分长开发安全软件的经验。 要确保这一点，安全开发生命周期 (SDL) 为基础的开发方法，结合了大量如运营安全保障 (OSA) 和 Microsoft 数字犯罪单位、 Microsoft 安全响应中心，以及 Microsoft 恶意软件防护中心基础结构级别的安全服务。 

Azure IoT 套件提供独特的功能，哪些使资源调配、 连接和存储来自 IoT 设备变得简单而透明的数据，和大多数的好处是，安全。 在本文中我们将研究 Azure IoT 套件的安全功能并解决部署策略以确保安全性、 保密性和法规遵从性挑战。 

## <a name="introduction"></a>介绍

互联网的东西 (IoT) 是未来，提供即时的业务和实际的机会，降低成本、 增加收入，并转换他们的业务的浪潮。 但是，许多企业中，是由于担心安全、 隐私和法规遵从性组织中部署 IoT 时犹豫不决。 主要的关注点来自 IoT 基础结构，合并的网络和物理世界一起，复合个别风险这两个领域所固有的唯一性。 确保在设备上运行的代码的完整性与安全性 IoT 提供设备和用户身份验证、 定义明确的设备 （以及由这些设备生成的数据） 的所有权以及正在适应网络和物理攻击。 

然后，没有隐私的问题。 公司希望透明度有关的数据集合中正在收集什么和为什么，可以看到它，控制访问，等等。 最后，有一般安全问题的设备故障，操作人员和维护行业标准的法规遵从性的问题。

给出安全、 隐私、 透明和法规遵从性考虑，选择合适的 IoT 解决方案提供商仍是个难题。 缝合在一起 IoT 软件和由多个供应商提供的服务的各个部分介绍安全、 隐私、 透明和可能很难检测，更不用说解决的法规遵从性中的差距。 所选的右 IoT 软件和服务提供商基于查找提供程序具有运行跨跨行业和地区，但也是能够安全和透明的方式扩展服务的丰富经验。 同样，它可以帮助选定的提供程序有几十年的经验开发在数十亿美元的全球范围内的计算机上运行的安全软件，具有非常庆幸的事情互联网这一新领域所带来的威胁状况的能力。

## <a name="secure-infrastructure-from-the-ground-up"></a>从创建之初的安全基础结构 

[Microsoft 的云](https://www.microsoft.com/enterprise/microsoftcloud/default.aspx#fbid=WzBsRQi6aGk)基础架构支持在 127 个国家超过十亿客户。 绘制我们长几十年的经验，构建企业级软件和运行世界上一些最大的在线服务，我们提供更高级别的增强的安全性、 隐私、 法规遵从性和威胁缓解做法比大多数客户可以靠自己来实现。

我们[安全开发生命周期 (SDL)](https://www.microsoft.com/sdl/)提供必需的整个公司开发过程嵌入到整个软件生命周期的安全要求。 为了帮助确保操作活动按照相同级别的安全措施，我们使用我们运营安全保障 (OSA) 的过程中排放的严格的安全原则。 我们还使用第三方审核正在进行验证，我们满足我们的法规遵从性要求，并且我们寻求广泛安全通过中心的绩效，包括 Microsoft 数字犯罪单位、 Microsoft 安全响应中心，以及 Microsoft 恶意软件防护中心创建的公司。

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure 的安全 IoT 基础结构为您的业务

Microsoft Azure 提供了一个完整的云解决方案的组合集成的云服务不断增长的集合 — — 分析、 机器学习、 存储、 安全性、 网络和 web — — 与业界领先的承诺保护和个人数据的隐私。 我们[假定违反](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/)战略使用专用"红色"的团队软件安全专家来模拟攻击、 测试能力检测的 Azure，防范新出现的威胁，以及从破坏中恢复。 我们[全球的事件响应](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity)小组全天候工作，以缓解攻击和恶意活动的效果。 团队遵循建立的事件管理、 通信和恢复的过程，并与内部和外部合作伙伴使用可发现、 可预知的接口。

我们的系统提供了连续的入侵检测和预防、 服务攻击防护、 常规渗透测试，和取证工具，可帮助确定并减轻威胁。 [多因素身份验证](../articles/multi-factor-authentication/multi-factor-authentication.md)提供了额外的最终用户能够访问网络的安全性。 并且对于应用程序和主机提供商，我们提供访问控制、 监控、 反恶意软件、 安全漏洞扫描、 修补程序和配置管理。

Microsoft 的 Azure IoT 套件利用的安全性和保密性以及安全开发和操作的所有 Microsoft 软件我们 SDL 和 OSA 的流程的 Azure 平台内置了。 这些过程提供了基础架构保护、 网络保护和身份和管理功能的任何解决方案安全性的基础。 

[Azure IoT 集线器](../articles/iot-hub/iot-hub-what-is-iot-hub.md) [IoT 套件](../articles/iot-suite/iot-suite-what-is-azure-iot.md)中提供完全管理服务，通过使用每个设备的安全凭据和访问控制使 IoT 设备和[Azure 机器学习](../articles/machine-learning/machine-learning-what-is-machine-learning.md)和[Azure 流分析](../articles/stream-analytics/stream-analytics-introduction.md)等的 Azure 服务之间的可靠和安全的双向通信。  

最佳通信安全和隐私功能内置到 Azure IoT 套件，我们已经分解成三个主要安全领域套件。 

![Azure IoT 套件](media/iot-security-ground-up/securing-iot-ground-up-fig2.png)

### <a name="secure-device-provisioning-and-authentication"></a>安全设备置备和身份验证

Azure IoT 套件保护设备，都是出在字段中提供 IoT 基础架构可用于在操作时与该设备进行通信的每个设备的唯一的标识键。 过程是快速且易于安装。 生成的密钥与用户所选的设备 ID 构成的基础设备和 Azure IoT 集线器之间的所有通讯中使用的标记。

设备 Id （如硬件信任模块中闪存） 的制造过程可以是与设备相关联，或者可以使用现有的固定标识为代理 （例如 CPU 序列号）。 更改此设备的标识信息并不简单，因为很重要的基础设备硬件更改但逻辑设备保持不变的情况下引入逻辑设备 Id。 在某些情况下，设备标识的关联可以在设备部署时 （即经过身份验证的域工程物理配置新设备与 IoT 解决方案后端通信时） 会发生。 [Azure IoT 中心标识注册表](../articles/iot-hub/iot-hub-devguide.md)提供设备标识和解决方案的安全密钥的安全存储。 单个或成组的设备标识可以添加到允许列表中或阻止名单中，启用完全控制设备访问。
 
云中的 azure IoT 中心访问控制策略启用激活并禁用任何设备身份，提供一种方法来解除从 IoT 部署时所需的设备。 此关联和解除关联的设备基于每一个设备标识。

其他设备的安全功能包括︰

- 设备不接受未经请求的网络连接。 它们仅出站的方式建立了所有的连接和路由。 从后端接收命令的设备，该设备必须启动连接来检查有任何挂起的命令来处理。 一旦安全建立 IoT 集线器与设备之间的连接，从云中消息传送到设备并可以透明地发送到云的设备。  
- 设备仅连接到或建立路由到与他们都对，如 Azure IoT 中心的已知服务。
- 系统级授权和身份验证使用每设备标识，使访问凭据和权限附近-可立即撤销。

### <a name="secure-connectivity"></a>安全连接 

消息的耐久性是任何 IoT 解决方案的一个重要功能。 需要持久提供命令和/或从设备接收数据带有下划线 IoT 设备已连接到 Internet 或其他类似的网络可以是不可靠的事实。 Azure IoT 集线器提供的云和设备通过确认响应消息系统之间进行消息传送的耐用性。 其他持久性消息通过遥测和命令两天七天 IoT 中心消息存入高速缓存。
 
效率很重要，以确保节约资源和资源有限的环境中操作。 Azure IoT 集线器，使有效的通信支持 HTTPS （HTTP 安全），流行的 http 协议中，行业标准安全版本。 高级消息队列协议 (AMQP) 和消息队列遥测传输 (MQTT)，支持的 Azure IoT 集线器，不仅用于资源使用而且可靠的消息传送方面的效率。 

可伸缩性要求安全地与各种设备进行互操作的能力。 Azure IoT 中心启用安全连接到启用了 IP 的又非启用 IP 的设备。 启用了 IP 的设备可直接连接，并通过安全连接与 IoT 中心进行通信。 启用非 IP 的设备是资源有限，只能通过近距离的通信协议，如 Zwave、 ZigBee 和 Bluetooth 连接。 字段网关用来聚合这些设备，并执行协议转换，以便利用云安全的双向通信。

其他连接安全功能包括︰

- 行业标准传输层安全 (TLS) 中使用 Azure IoT 中心使用 X.509 协议身份验证保护设备和 Azure IoT 集线器，或者网关和 Azure IoT 集线器，之间的通信路径。
- 为了防止未经请求的入站连接的设备，Azure IoT 中心不会打开任何连接的设备。 设备启动所有连接。 
- Azure IoT 集线器持久存储的设备的通知，并等待要连接的设备。 启用设备连接由于电源或连接问题，以获得这些命令的个别情况下，两天内，存储这些命令。 Azure IoT 中心维护每台设备的每个设备队列。

### <a name="secure-processing-and-storage-in-the-cloud"></a>安全处理和存储在云中 

在云环境中处理数据的加密通信，从 Azure IoT 套件可帮助保护数据的安全。 它提供了实现额外的加密和安全密钥管理的灵活性。 使用 Azure 活动目录 (AAD) 进行用户身份验证和授权，Azure IoT 套件可以提供基于策略的授权模型数据在云中，允许方便地访问管理，进行审计和检查。 该模型还可以即时吊销访问云中的数据并连接到 Azure IoT 套件的设备。

云中的数据后，它可以处理和存储任何用户定义的工作流中。 对数据的每个部分的访问受控制 Azure Active Directory，具体取决于所使用的存储服务。
   
IoT 基础结构使用的所有密钥被都存储在云环境中安全地存储，翻转，以防密钥需要重新调配的能力。 数据可以存储在[DocumentDB](../articles/documentdb/documentdb-introduction.md)中或在[SQL 数据库](../articles/sql-database/sql-database-faq.md)，使定义的所需的安全级别。 此外，Azure 使您能够监控和审核所有访问数据来提醒您任何入侵或未经授权的访问。

## <a name="conclusion"></a>结论

互联网的东西开始与您的爱 — — 对企业最重要的事情。 IoT 可以为业务提供令人惊叹的价值，通过降低成本、 增加收入、 和转换业务。 此转换的成功与否很大程度上取决于选择右 IoT 软件和服务提供商。 这意味着查找提供程序 catalyzes 通过了解业务需要和要求，这种转换不仅还提供了服务和软件构建与安全、 隐私、 透明和法规遵从性成为主要的设计考虑事项。 Microsoft 已开发和部署安全的软件和服务的丰富经验，并继续成为在互联网的东西这个新时代的领导者。 

Microsoft Azure IoT 套件中安全措施通过建立设计，启用的资产，以提高效率，驱动器启用创新，并使用高级的数据分析，将企业的操作性能安全监视。 针对安全性、 多个安全功能和设计模式的分层方法，Azure IoT 套件有助于部署基础结构，这是值得信任的任何业务。 

## <a name="additional-information"></a>其他信息

每个 Azure IoT 套件预配置的解决方案创建实例的 Azure 服务，如下所示︰

- [**Azure IoT 中心**](https://azure.microsoft.com/services/iot-hub/)︰ 网关连接到"事物"云。 可以帮助您保护您的解决方案的每台设备身份验证支持缩放到数以百万计，每个中心和处理海量数据的连接。
- [**Azure DocumentDB**](https://azure.microsoft.com/services/documentdb/)︰ 半结构化数据管理设备的元数据的可伸缩的、 完全编入索引的数据库服务您配置，如属性、 配置和安全属性。 DocumentDB 提供了高性能、 高吞吐量的处理、 数据和丰富的 SQL 查询接口索引架构不可知。
- [**Azure 流分析**](https://azure.microsoft.com/services/stream-analytics/)︰ 实时流处理中云，使您可以快速开发和部署一个低成本分析解决方案，可以发现从设备、 传感器、 基础架构和应用程序的实时见解。 同时仍达到高吞吐量、 低延迟和可恢复性，这完全管理服务中的数据可以扩展到任何卷。
- [**Azure 应用程序服务**](https://azure.microsoft.com/services/app-service/)︰ 云平台来构建强大的 web 和移动应用程序连接到数据的任何地方;在云中还是内部。 构建合作为 iOS 和 Android，移动应用程序。 与您的软件即服务 (SaaS) 的框连接到云服务的很多企业级应用程序和企业级应用程序集成。 您喜欢的语言和 IDE 中的代码，.NET、 NodeJS、 PHP、 Python 或 Java — — 比以往任何时候都更快地生成 web 应用程序和 Api。
- [**逻辑应用程序**](https://azure.microsoft.com/services/app-service/logic/)︰ Azure 应用程序服务的逻辑应用程序功能有助于集成到您现有的业务系统线 IoT 解决方案和自动化工作流程。 逻辑应用程序，开发人员开始从触发器，然后执行一系列步骤的设计工作流 — — 规则和使用功能强大的连接器来与您的业务流程集成的操作。 逻辑应用程序提供的框连接到 SaaS，基于云的一个庞大的生态系统和内部部署的应用程序。
- [**Azure blob 存储**](https://azure.microsoft.com/services/storage/)︰ 为您的设备发送到云中的数据可靠、 经济的云存储。

