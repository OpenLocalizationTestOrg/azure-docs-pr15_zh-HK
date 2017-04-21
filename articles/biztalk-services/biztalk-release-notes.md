<properties
    pageTitle="发行说明 Azure BizTalk 服务 |Microsoft Azure BizTalk 服务"
    description="列出了 Azure BizTalk 服务的已知的问题" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>发行说明，了解 Azure BizTalk 服务

发行说明，了解 Microsoft Azure BizTalk 服务包含此版本中的已知的问题。

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>在 11 月的 BizTalk 服务更新的新增功能
* 在 BizTalk 服务门户可以启用加密存放。 请参阅[启用加密存放在 BizTalk 服务门户](https://msdn.microsoft.com/library/azure/dn874052.aspx)。

## <a name="update-history"></a>更新历史记录

### <a name="october-update"></a>10 月更新

* 支持组织的帐户︰  
 * **方案**︰ 注册 BizTalk 服务部署使用 Microsoft 帐户 (如user@live.com)。 在这种情况下，只有 Microsoft 帐户的用户可以管理 BizTalk 服务使用 BizTalk 服务门户。 不能使用组织的帐户。  
 * **方案**︰ 注册 BizTalk 服务部署在 Azure Active Directory 中使用组织的帐户 (如user@fabrikam.com或user@contoso.com)。 在这种情况下，只有 Azure Active Directory 同一组织中的用户可以管理 BizTalk 服务使用 BizTalk 服务门户。 不能使用 Microsoft 帐户。  
* 在 Azure 的传统门户网站中创建 BizTalk 服务时，您自动注册在 BizTalk 服务门户中。
 * **方案**︰ 到 Azure 的传统门户网站，签名创建的 BizTalk 服务，然后选择**管理**第一次。 当打开 BizTalk 服务门户时，BizTalk 服务将自动注册，并已准备好进行部署。  
 请参见[注册和更新 BizTalk 服务门户上的 BizTalk 服务部署](https://msdn.microsoft.com/library/azure/hh689837.aspx)。  

### <a name="august-14-update"></a>8 月 14 日的更新
* 协议和分离 — 贸易伙伴协议和桥桥现在分离的 BizTalk 服务门户中。 您现在创建协议和桥分别和桥协议基于 EDI 消息中的值在运行时解析。 请参阅[创建 Azure BizTalk 服务协议](https://msdn.microsoft.com/library/azure/hh689908.aspx)，[创建 EDI 桥使用 BizTalk 服务门户](https://msdn.microsoft.com/library/azure/dn793986.aspx)，[创建使用 BizTalk 服务门户 AS2 桥](https://msdn.microsoft.com/library/azure/dn793993.aspx)、 和[桥如何解决在运行时的协议？](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* 创建协议模板的选项，则停止。  
* 发送端协议，您现在可以指定不同的分隔符设置为每个架构。 在发送端协议的协议设置下，指定此配置。 有关详细信息，请参阅[创建一个 X12 协议在 Azure BizTalk 服务](https://msdn.microsoft.com/library/azure/hh689847.aspx)和[创建在 Azure BizTalk 服务 EDIFACT 协议](https://msdn.microsoft.com/library/azure/dn606267.aspx)。 两个新的实体也将添加到 TPM OM API，用于相同的目的。 请参阅[X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx)和[EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx)。  
* 现在支持标准的 XSD 构造，包括派生的类型。 请参阅[使用标准 XSD 构造在您映射](https://msdn.microsoft.com/library/azure/dn793987.aspx)和[使用映射方案和示例中的派生类型](https://msdn.microsoft.com/library/azure/dn793997.aspx)。  
* AS2 支持新消息签名的 MIC 算法和新的加密算法。 请参阅[创建在 Azure BizTalk 服务了 AS2 协议](https://msdn.microsoft.com/library/azure/hh689890.aspx)。  
## <a name="know-issues"></a>知道问题

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>BizTalk 服务门户更新后出现的连接问题

  如果您有 BizTalk 服务门户打开 BizTalk 服务升级以滚滚的服务更改时，可能面临与 BizTalk 服务门户的连接问题。  
  作为一种替代方法，可以重新启动浏览器，删除浏览器的缓存，或在专用模式下启动门户。  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>如果您单击错误或警告的 BizTalk 服务项目中，Visual Studio IDE 找不到项目
安装 Visual Studio 2012 更新 3 RC 1 若要解决此问题。  

### <a name="custom-binding-project-reference"></a>自定义绑定的项目引用
请考虑以下情况下使用 Visual Studio 解决方案中的 BizTalk 服务项目︰  
* 在相同的 Visual Studio 解决方案中，没有一个 BizTalk 服务项目和自定义绑定项目。 该 BizTalk 服务项目都有对此自定义绑定项目文件的引用。
* BizTalk 服务项目都有一个自定义绑定/行为 DLL 的引用。

在生成解决方案在 Visual Studio 中的成功。 然后，您重建或者清除解决方案。 在此之后，当重新生成或清理，则出现下列错误︰  
  无法将文件复制<Path to DLL>为"bin\Debug\FileName.dll"。 该进程无法访问文件 bin\Debug\FileName.dll，因为另一个进程正在使用它。  

#### <a name="workaround"></a>解决方法
* 如果安装了[Visual Studio 2012 更新 3](https://www.microsoft.com/download/details.aspx?id=39305) ，您有以下两个选项︰

  * 重新启动 Visual Studio，或

  * 重新启动解决方案。 然后，执行只生成解决方案。  

* 如果未安装[Visual Studio 2012 更新 3](https://www.microsoft.com/download/details.aspx?id=39305) ，打开任务管理器的进程选项卡上单击 MSBuild.exe 的进程中，和，然后单击结束进程按钮。  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>对 BasicHttpRelay 的终结点的路由不支持从桥接器和 BizTalk 服务门户如果不可打印的字符被提升为 HTTP 标头

如果使用不可打印的字符作为升级的属性的一部分的消息，这些消息无法路由到中继使用 BasicHttpRelay 绑定的目标位置。 此外，升级的属性的程序可用，因为跟踪的一部分是为 blob 的 URL 编码和非编码为目标。  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>即使发送异步 MDN 选项处于未选中状态异步发送 MDN  
考虑这种情况下 – 如果您选中**发送异步 MDN**复选框，指定要发送 MDN 异步的 URL，然后取消选中**发送异步 MDN**复选框，并再次，MDN 仍会将发送到指定的 URL 即使未选中发送异步 MDNs 选项。  
作为一种变通方法，您必须先取消选中**发送异步 MDN**复选框清除指定的 URL，然后部署 AS2 协议。  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>空白字符之外有效交换会导致空消息要发送到挂起的终结点  
如果有超出 IEA 段空白，拆装器将这视为当前交换结束和下一组空格作为下一条消息来看待。 由于这是一种无效的交换，可能会注意到一个成功的消息发送到路由的目标，和一个空消息发送挂起终结点。  
### <a name="tracking-in-biztalk-services-portal"></a>跟踪在 BizTalk 服务门户  
EDI 消息处理和任何相关捕获跟踪事件。 如果邮件以外的协议阶段失败，跟踪将显示为成功。 在此情况下，请参阅日志区域中**跟踪**错误的详细信息的**详细信息**列下。
X12 接收和发送设置 ([创建一个 X12 在 Azure BizTalk 服务协议](https://msdn.microsoft.com/library/azure/hh689847.aspx)) 协议舞台上提供的信息。  

### <a name="update-agreement"></a>更新协议  
BizTalk 服务门户允许您配置协议时修改标识限定符。 这可能会导致 inconsistence 属性。 例如，没有使用 ZZ:1234567 和 ZZ:7654321 限定符的协议。 在 BizTalk 服务门户配置文件设置，您可以更改为 01:ChangedValue ZZ:1234567。 打开该协议和 01:ChangedValue 显示而不是 ZZ:1234567。
要修改标识限定符，删除该协议，合作伙伴配置文件中更新**标识**然后重新创建该协议。  
> AZURE。警告这种行为会影响 X12 和 AS2。  

### <a name="as2-attachments"></a>AS2 附件  
AS2 中不支持邮件的附件发送或接收。 具体来说，悄悄忽略附件和邮件正文作为常规 AS2 消息进行处理。  
### <a name="resources-remembering-path"></a>资源︰ 记住路径  
在添加**资源**时，该对话框窗口可能不记得以前用来添加资源的路径。 要记得以前使用的路径，请尝试将 BizTalk 服务门户网站添加到**受信任的站点**在 Internet Explorer 中。  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>如果重命名的一座桥梁的实体名称和关闭而不保存更改的项目，重新打开实体导致错误
请考虑一个方案以下列顺序︰  
* 将桥接器 （例如 XML 单向桥） 添加到 BizTalk 服务项目  

* 通过指定实体名称属性的值来重命名桥。 这将重命名具有您指定的名称相关联的.bridgeconfig 文件。  

* （通过关闭 Visual Studio 中的选项卡） 的.bcs 文件关闭而不保存所做的更改。  

* 从解决方案资源管理器中再次打开.bcs 文件。  
您会注意到，尽管相关联的.bridgeconfig 文件具有您指定的新名称，在设计图面上的实体名称仍是旧名称。 如果您尝试通过双击桥接组件打开桥配置，您会收到以下错误︰  
  <old name>实体的关联文件<old name>.bridgeconfig' 不存在  
要避免遇到这种情况下的，请确保重命名实体 BizTalk 服务项目中的，您将更改保存。  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>即使已从 Visual Studio 项目中排除某个项目成功生成 BizTalk 服务项目
请您在其中添加项目 （例如，XSD 文件） 的方案考虑到 BizTalk 服务项目 （例如，通过将其指定为一个请求消息类型），将该项目纳入桥配置，然后从 Visual Studio 项目中排除它。 在这种情况下，生成项目不会给任何错误，只要是从它已包括在 Visual Studio 项目中的相同位置的磁盘上的可用的已删除的项目。
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>该 BizTalk 服务项目不会检查架构可用性配置网桥时
在 BizTalk 服务项目中，如果添加到项目中的架构导入另一个架构，BizTalk 服务项目不会检查是否将导入的架构添加到项目。 如果您试图建立这样一个项目，没有得到任何生成错误。
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>响应消息的 XML 请求-回复桥始终为 utf-8 字符集
对于此版本，从现有的 XML 请求-回复桥响应消息的字符集始终设置为 utf-8。
### <a name="user-defined-datatypes"></a>用户定义的数据类型
在 BizTalk 适配器服务功能中的 BizTalk 适配器诊断适配器可以利用用户定义数据类型适配器的操作。
在使用用户定义数据类型时，将驱动器︰ \Program Files\Microsoft BizTalk 适配器 Service\BAServiceRuntime\bin\ 或到全局程序集缓存 (GAC) 承载 BizTalk 适配器服务服务的服务器上复制文件 (.dll)。 否则，客户端上可能会出现以下错误︰  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] 建议使用 GACUtil.exe 将文件安装到全局程序集缓存中。 GACUtil.exe 介绍如何使用此工具，将 Visual Studio 的命令行选项。  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>重新启动 BizTalk 适配器服务网站
安装**BizTalk 适配器服务运行时***创建* *BizTalk 适配器服务**中包含 IIS 网站* *BAService* *应用程序。**BAService** 应用程序内部使用中继绑定扩展到云的后端服务终结点的范围。 对于承载服务内部，对应的中继终结点将注册在服务总线上只能在内部服务启动时。  

停止并启动应用程序时，如果不被采用自动启动应用程序的配置。 因此**BAService**停止时，您必须始终重新启动**BizTalk 适配器服务**网站相反。 无法启动或停止的**BAService**应用程序。
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>对于 LOB 组件的地址和实体名称不应使用特殊字符
不应该为地址和实体名称的 LOB 组件中使用特殊字符。 如果您这样做时，将部署 BizTalk 服务项目时可能会出错。 对于某些如 %字符，BizTalk 适配器服务网站可能进入停止状态，则必须手动启动它。
### <a name="test-map-with-get-context-property"></a>获取上下文属性的测试映射
如果转换包含**获取上下文属性**的映射操作，**测试映射**将会失败。 作为临时的替代方法，替换字符串连接映射操作包含虚拟数据**获取上下文属性**映射操作。 这将填充目标架构，并且允许您测试其他转换功能。
### <a name="test-map-property-does-not-display"></a>测试映射属性不显示
**测试映射**属性不显示在 Visual Studio 中。 如果**属性**窗口和**解决方案资源管理器**窗口不同时停靠，这会发生。 若要解决此问题，停靠**属性**和**解决方案资源管理器**窗口。  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>重新格式化 DateTime 下拉列表将显示为灰色
日期时间重新映射操作是添加到设计图面和配置时，格式下拉列表可能会灰显。 如果计算机显示设置**中-125%**或**更大 – 150%**，可发生此错误。 若要解决，请将显示设置为**较小 – 100%（默认值）** ，按照下面的步骤︰  
1. 打开**控制面板**并单击**外观和个性化**。
2. 单击**显示**。
3. 单击**较小 – 100%（默认值）** ，然后单击**应用**。

如预期的那样，现在应处理**格式**下拉列表。
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>在 BizTalk 服务门户中的重复协议
请考虑以下情形︰
1. 创建使用贸易合作伙伴管理 OM API 协议。
2. 在 BizTalk 服务门户中两个不同的选项卡中打开该协议。
3. 将部署从这两个选项卡的协议。
4. 因此，这两个协议获取部署导致 BizTalk 服务门户中的重复项

**解决方法**。 在 BizTalk 服务门户中，打开任何一种重复协议和取消部署。  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>即使证书已更新的项目存储在桥接器不使用更新的证书
请考虑以下方案︰  

**方案 1︰ 使用基于指纹的证书来保护邮件传输从一座桥的服务终结点**  
考虑的一下基于指纹的证书使用 BizTalk 服务项目中的位置。 更新在 BizTalk 服务门户中的证书具有相同的名称但不同的指纹，但做相应地更新该 BizTalk 服务项目。 在这种情况下，该桥接器可能会继续处理消息，因为较旧的证书数据仍然可能是通道高速缓存中。 在此之后，邮件处理失败。  

**解决方法**︰ 更新 BizTalk 服务项目中的证书，并重新部署项目。  

**方案 2︰ 使用基于名称的行为来标识证书用于保护邮件传输从一座桥的服务终结点**

请考虑在使用基于名称的行为来 BizTalk 服务项目中标识的证书方案。 更新 BizTalk 服务门户中的证书，但不是相应地更新该 BizTalk 服务项目。 在这种情况下，该桥接器可能会继续处理消息，因为较旧的证书数据仍然可能是通道高速缓存中。 在此之后，邮件处理失败。  

**解决方法**︰ 更新 BizTalk 服务项目中的证书，并重新部署项目。  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>桥接器继续处理消息，即使在 SQL 数据库处于脱机状态时
BizTalk 服务桥梁继续处理消息一段时间，即使 Microsoft Azure SQL 数据库 （用于存储正在运行的信息，如部署的工件和管道），处于脱机状态。 这是因为 BizTalk 服务使用的缓存的项目和桥接配置。
如果不希望使用桥接器处理任何消息时 SQL 数据库处于脱机状态，您可以使用 BizTalk 服务 PowerShell cmdlet 停止或暂停该 BizTalk 服务。 请参阅[Azure BizTalk 服务管理示例](http://go.microsoft.com/fwlink/p/?LinkID=329019)Windows PowerShell cmdlet 来管理操作。  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>正在读取的 XML 消息中的桥接器的自定义代码组件包括一个额外的物料清单字符
请考虑想要读取 XML 消息的桥接器的自定义代码中的方案。 如果您使用.NET API System.Text.Encoding.UTF8.GetString(bytes) 开头的消息输出中包括一个额外的物料清单字符。 因此，如果您不希望包括额外物料清单字符输出，则必须使用```System.IO.StreamReader().ReadToEnd()```。
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>不能将消息发送到使用 WCF 的桥梁
消息发送到使用 WCF 的桥梁不会缩放。 如果所需的可扩展的客户端，而是应使用 HttpWebRequest。
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>之后从 BizTalk 服务预览升级到常规可用性 (GA) 的升级︰ 令牌提供程序错误
没有 EDI 或 AS2 协议与活动的批次。 在 BizTalk 服务是从预览升级到正式上市时，可能会出现以下︰
* 错误︰ 不能提供安全令牌令牌提供程序。 令牌提供程序返回消息︰ 无法解析此远程名称。

* 批处理任务将被取消。

**解决方法**︰ 在 BizTalk 服务更新到常规可用性 (GA)、 重新部署的协议。  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>升级︰ 工具箱 BizTalk 服务 SDK 升级后显示旧的桥接器图标
升级早期版本的 BizTalk 服务 SDK，有旧图标表示桥接器后, 工具箱中将继续显示桥接器的旧图标。 但是，如果向 BizTalk 服务项目设计器图面添加一个桥，表面显示新建图标。  

**解决方法**。 您可以通过删除下的.tbd 文件来变通解决此问题<system drive>: \Users\<用户 > \AppData\Local\Microsoft\VisualStudio\11.0。  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>升级︰ 从预览到正式上市的 BizTalk 门户更新可能会显示一个错误，指示 EDI 功能不可用
如果登录到 BizTalk 服务门户 BizTalk 服务从预览升级到正式发行时，门户上可能会出现以下错误︰  

此功能不可用作为此版本的 Microsoft Azure BizTalk 服务的一部分。 要使用这些功能，切换到相应的版本。  

**解决方法**︰ 从门户网站，关闭和打开浏览器，然后再登录到门户注销。  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>升级︰ 新跟踪数据未显示 BizTalk 服务升级到正式上市后
假设您拥有在 BizTalk 服务预览订阅上部署 XML 桥方案。 将消息发送到桥中并且在 BizTalk 服务门户可用相应的跟踪数据。 现在，如果 BizTalk 服务门户和 BizTalk 服务运行时位都升级到正式上市，而到前面部署桥方终结点发送一条消息，跟踪数据不显示升级后发送的消息。  

### <a name="pipelines-vs-bridges"></a>管线 v/s 桥
在本文中，术语管道和桥互换使用。 两者本质上意味着同样的工作，即，在 BizTalk 服务上部署的消息处理单元。  

### <a name="concepts"></a>概念  

[BizTalk 服务](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
