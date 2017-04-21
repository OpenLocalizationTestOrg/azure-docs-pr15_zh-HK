<properties
   pageTitle="PowerShell 连接器 |Microsoft Azure"
   description="本文介绍如何配置微软 Windows PowerShell 连接器。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Windows PowerShell 连接器技术参考
本文介绍了 Windows PowerShell 连接器。 文章将应用于以下产品︰

- 2016 年 Microsoft 标识管理器 (MIM2016)
- Forefront 标识管理器 2010 R2 (FIM2010R2)
    -   必须使用修补程序 4.1.3671.0 或更高版本的[KB3092178](https://support.microsoft.com/kb/3092178)。

对于 MIM2016 和 FIM2010R2，连接器是可从[Microsoft 下载中心](http://go.microsoft.com/fwlink/?LinkId=717495)下载。

## <a name="overview-of-the-powershell-connector"></a>PowerShell 连接器的概述
PowerShell 连接器，您可以使用外部系统提供 Windows PowerShell 基于 Api 集成同步服务。 连接器提供了调用基于可扩展连接管理代理的能力之间的桥梁，2 (ECMA2) 框架和 Windows PowerShell。 ECMA 框架有关的详细信息，请参阅[可扩展连接 2.2 管理代理参考](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx)。

### <a name="prerequisites"></a>系统必备组件
使用连接器之前，请确保您具有同步服务器上的以下︰

- Microsoft.NET 4.5.2 框架或更高版本
- Windows PowerShell 2.0、 3.0 或 4.0

必须配置同步服务服务器上的执行策略以允许运行 Windows PowerShell 脚本的连接器。 除非该连接器运行进行了数字签名，这些脚本将执行策略配置运行此命令︰  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>创建一个新的连接器
若要创建 Windows PowerShell 连接器中的同步服务，必须提供一系列 Windows PowerShell 脚本执行请求的同步服务的步骤。 根据您连接到的数据源和所需的功能，您必须实现的脚本会有所不同。 本节概述了每个可实现的和他们所需的脚本。

Windows PowerShell 连接器用于存储每个同步服务数据库中的脚本。 虽然可能会运行脚本存储在文件系统上，可以很方便地插入的每个脚本直接在连接器的配置主体。

若要创建 PowerShell 连接器，**同步服务**中选择**管理代理**和**创建**。 选择**PowerShell (Microsoft)**连接器。

![创建连接器](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>连接
提供用于连接到远程系统的配置参数。 这些值安全地存储在同步服务并供 Windows PowerShell 脚本运行连接器时。

![连接](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

您可以配置以下连接参数︰

**连接**

参数 | 默认值 | 目的
--- | --- | ---
服务器 | <Blank> | 该连接器应连接到的服务器名称。
域 | <Blank> | 要运行连接器时，使用存储的凭据的域。
用户 | <Blank> | 运行连接器时用于存储凭据的用户名。
密码 | <Blank> | 要运行连接器时，使用存储的凭据的密码。
模拟接口帐户 | 假 | 如果为 true，则同步服务提供的凭据上下文中运行 Windows PowerShell 脚本。 在可能的情况下，建议的**$Credentials**参数传递给每个而不是模拟使用脚本。 使用此选项所需的附加权限的详细信息，请参阅[附加配置为模拟](#additional-configuration-for-impersonation)。
当模拟加载用户配置文件 | 假 | 指定 Windows 在模拟过程中加载的连接器的凭据的用户配置文件。 如果被模拟的用户有漫游配置文件，则连接器不会加载漫游配置文件。 使用此参数所需的附加权限的详细信息，请参阅[附加配置为模拟](#additional-configuration-for-impersonation)。
当模拟登录类型 | 无 | 在模拟过程中的登录类型。 有关详细信息，请参阅[dwLogonType] [dw]文档。
仅签名的脚本 | 假 | 如果为 true，则 Windows PowerShell 连接器验证每个脚本具有有效的数字签名。 如果为 false，则确保同步服务服务器的 Windows PowerShell 执行策略是 RemoteSigned 或不受限制。

**公共模块**  
连接器可以在配置中存储的共享的 Windows PowerShell 模块。 当该连接器运行一个脚本时，以便它可以由每个脚本导入到文件系统提取 Windows PowerShell 模块。

导入、 导出和密码同步脚本的通用模块被解压到连接器的 MAData 文件夹中。 对于架构、 验证、 层次结构和分区发现脚本，至 %TEMP%文件夹中提取通用模块。 在这两种情况下，根据公共模块脚本名称设置名为提取公共模块脚本。

要加载 MAData 文件夹中名为 FIMPowerShellConnectorModule.psm1 的模块，请使用下面的语句︰`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

要加载 %TEMP%文件夹中名为 FIMPowerShellConnectorModule.psm1 的模块，请使用下面的语句︰`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**参数验证**  
验证脚本是用于确保由管理员提供的连接器配置参数都是有效的可选的 Windows PowerShell 脚本。 正在验证的服务器、 连接凭据和连接参数是验证脚本的常见用法。 验证脚本调用后的以下选项卡和对话框被修改︰

- 连接
- 全局参数
- 分区配置

验证脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | 配置选项卡或对话框触发验证请求。
ConfigParameters | [KeyedCollection] [ keyk] [字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。

验证脚本应与管道返回一个 ParameterValidationResult 对象。

**通过架构发现**  
通过架构发现脚本是必需的。 此脚本会返回对象类型、 属性和配置属性流规则时，将使用同步服务的属性约束。 通过架构发现脚本运行期间创建连接器并填充连接器的架构。 它还可通过刷新架构操作同步服务管理器中。

模式发现脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。

该脚本必须返回单个[架构][schema]对象到管道。 架构对象组成[SchemaType] [schemaT]表示对象类型的对象 (例如︰ 用户和组)。 SchemaType 对象包含一套[SchemaAttribute] [schemaA]表示的属性的对象 (例如︰ 名字、 姓氏和邮寄地址) 的类型。

**其他参数**  
除了标准的配置设置，您可以定义特定于接口的实例的其他自定义配置设置。 这些参数可以指定在接头，分区，或运行的步骤和相关的 Windows PowerShell 脚本从访问级别。 自定义配置设置可以存储在纯文本格式的同步服务数据库，或可能对它们进行加密。 同步服务会自动加密和解密安全时所需的配置设置。

若要指定自定义配置设置，请用逗号 （，） 分隔每个参数的名称。

从脚本访问自定义配置设置，您必须后缀的名称以下划线 ( \_ ) 和范围 （全局、 分区或 RunStep） 的参数。 例如，若要访问全局 FileName 参数，使用此代码段︰`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>功能
管理代理设计器的功能选项卡上定义的行为和功能连接器。 在创建连接器时，不能修改此选项卡上所做的选择。 下表列出的功能设置。

![功能](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

功能 | 说明 |
--- | --- |
[可分辨的名称的样式][dnstyle] | 指示如果连接器支持的可分辨的名称，如果如此，什么样式。
[导出类型][exportT] | 确定提供给导出脚本的对象的类型。 <li>AttributeReplace – 包括全套的多值属性值的属性发生更改时。</li><li>AttributeUpdate – 包括仅增量与多值属性在属性更改时。</li><li>MultivaluedReferenceAttributeUpdate-包括一套完整的非引用多值属性值和多值的引用属性的唯一增量。</li><li>ObjectReplace – 包括对象的所有属性在任何属性更改时</li>
[数据规范化][DataNorm] | 指示同步服务规范化 anchor 属性，它们只提供给脚本之前。
[对象确认][oconf] | 配置同步服务挂起导入行为。 <li>普通 – 需要所有导出的更改以便确认通过导入的默认行为</li><li>NoDeleteConfirmation – 当对象被删除，没有生成任何挂起导入。</li><li>NoAddAndDeleteConfirmation – 创建或删除对象时没有生成任何挂起导入。</li>
DN 用作锚点 | 如果可分辨名称样式设置为 LDAP，连接器空间的定位特性也是可分辨的名称。
多个连接器的并发操作 | 当选中时，可以同时运行多个 Windows PowerShell 连接器。
分区 | 当选中时，该连接器支持多个分区和分区发现。
层次结构 | 当选中时，该连接器支持 LDAP 样式层次结构。
启用导入 | 当选中时，连接器导入数据通过导入脚本。
启用增量导入 | 当选中时，连接器可以请求增量导入脚本。
启用导出 | 当选中时，连接器将通过导出脚本的数据导出。
启用全部导出 | 在选中后，导出脚本支持导出整个连接器空间。 若要使用此选项，启用导出还必须检查。
第一个导出过程中没有引用值 | 选中后，在第二个导出传递中导出引用特性。
启用对象重命名 | 当选中时，可以修改可分辨的名称。
删除添加的替换 | 在选中后，删除添加操作会被导出为单个替换。
启用密码操作 | 当选中时，密码同步脚本支持。
启用在第一次导出密码 | 选中后，在资源调配过程中设置的密码导出创建对象时。

### <a name="global-parameters"></a>全局参数
管理代理程序设计器中的全局参数选项卡可以配置 Windows PowerShell 脚本运行的连接器。 您还可以配置为在连接选项卡上定义的自定义配置设置的全局值。

**分区的发现**  
分区是在一个共享的架构内一个单独的命名空间。 例如，在 Active Directory 中的每个域是林中一个分区。 分区是逻辑分组进行导入和导出操作。 导入和导出有如分区在此上下文中的上下文和所有操作发生。 分区被应该表示在 LDAP 中的层次结构。 一个分区的可分辨的名称导入过程中使用，以验证所有返回的对象在一个分区的范围内。 分区的可分辨的名称还调配到连接器空间中元节期间用来确定对象应在导出过程中与相关联的分区。

分区发现脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。

该脚本必须返回单个[分区][part]对象或到管道的分区对象的列表 [T]。

**层次结构发现**  
LDAP 可分辨名称样式功能时，才使用层次结构发现脚本。 该脚本用于使您可以浏览并选择被认为是一套容器或缩小范围导入和导出操作。 该脚本只应该提供向脚本提供的根节点的直接子级的节点的列表。

层次结构发现脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
ParentNode | [HierarchyNode][hn] | 该脚本应在其下返回直接子级层次结构的根节点。

脚本必须返回单个子 HierarchyNode 对象或子 HierarchyNode 对象列表 [T] 到管线。

#### <a name="import"></a>导入
支持导入操作的连接器必须实现三个脚本。

**开始导入**  
开始导入脚本在一个运行的导入步骤开始运行。 在此步骤中，可以建立到源系统的连接，并从已连接的系统导入数据之前执行准备步骤。

开始导入脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | 该脚本会告知导运行 （增量或完全），分区、 层次结构、 水印，以及预期的页面大小的类型。
类型 | [架构][schema] | 连接器空间是导入的架构。

该脚本必须返回单个[OpenImportConnectionResults] [oicres]对象到管道，例如︰`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**导入数据**  
直到该脚本指示没有详细的数据导入，导入数据脚本称为连接器。 Windows PowerShell 连接器有 9999 对象的页面大小。 如果您的脚本返回超过 9999 对象的导入，您必须支持分页。 自定义数据的属性可用于向存储水印，以便每次导入数据脚本称为连接器公开，脚本继续导入的对象离开的位置。

导入数据脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
GetImportEntriesRunStep | [ImportRunStep][irs] | 保存过程中可以使用的水印 (CustomData) 分页的导入和增量导入。
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | 该脚本会告知导运行 （增量或完全），分区、 层次结构、 水印，以及预期的页面大小的类型。
类型 | [架构][schema] | 连接器空间是导入的架构。

导入数据脚本必须写入列表 [[CSEntryChange][csec]] 对象到管道。 此集合由表示要导入的每个对象的 CSEntryChange 特性。 完全导入运行，期间此集合应有一套完整的 CSEntryChange 对象，必须为每个对象的所有属性。 增量导入，在 CSEntryChange 对象应包含要导入的每个对象或已更改对象的 （替换模式） 的完整表示的属性级别变化量。

**最终导入**  
在导入运行结束时，将运行最终导入脚本。 此脚本应该执行任何清理任务有必要 （例如，关闭系统连接），并对故障的响应。

最终导入脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | 该脚本会告知导运行 （增量或完全），分区、 层次结构、 水印，以及预期的页面大小的类型。
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | 导入已结束的原因通知该脚本。

该脚本必须返回单个[CloseImportConnectionResults] [cicres]对象到管道，例如︰`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>导出
与连接器的导入体系结构，支持出口的连接器必须实现三个脚本。

**开始导出**  
开始导出脚本运行导出运行步骤的开始处。 在此步骤中，可以建立到源系统的连接并将数据导出到已连接的系统之前进行任何准备步骤。

开始导出脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | 该脚本会告知导出运行 （增量或完全），分区、 层次结构和预期的页面大小的类型。
类型 | [架构][schema] | 连接器空间是导出架构。

该脚本不应与管道返回任何输出。

**导出数据**  
同步服务调用数据导出脚本根据需要处理所有挂起的导出多次。 如果连接器空间有多个挂起输出连接器的页面尺寸比，导出数据脚本可能被调用多次，并有可能多次对同一对象。

导出数据脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
CSEntries | IList[CSEntryChange][csec] | 所有的连接器空间对象与挂起的处理在此阶段中的导出列表。
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | 该脚本会告知导出运行 （增量或完全），分区、 层次结构和预期的页面大小的类型。
类型 | [架构][schema] | 连接器空间是导出架构。

导出数据脚本必须返回[PutExportEntriesResults] [peeres]对象到管道。 此对象不需要包括导出的每个连接器的结果信息，除非出现错误或对定位属性更改时发生。 例如，若要返回一个 PutExportEntriesResults 对象到管道︰`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**最终导出**  
在运行导出的结论，最终导出脚本运行。 此脚本应该执行任何清理任务有必要 （例如，关闭系统连接），并对故障的响应。

最终导出脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | 该脚本会告知导出运行 （增量或完全），分区、 层次结构和预期的页面大小的类型。
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | 该脚本会告知出口已结束的原因。

该脚本不应与管道返回任何输出。

#### <a name="password-synchronization"></a>密码同步
Windows PowerShell 连接器可以用作密码更改/重置目标。

该密码脚本从连接器接收以下参数︰

名称 | 数据类型 | 说明
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][字符串、 [ConfigParameter][cp]] | 连接器的配置参数表。
凭据 | [PSCredential][pscred] | 包含由管理员在连接选项卡上输入的任何凭据。
分区 | [分区][part] | CSEntry 位于的目录分区。
CSEntry | [CSEntry][cse] | 连接器空间项是该对象接收到更改密码或重置。
︰ 键入 | 字符串 | 指示操作是否重置 (**SetPassword**) 或更改 (**更改密码**)。
PasswordOptions | [PasswordOptions][pwdopt] | 标志，它们指定预期的密码重置问题。 此参数才可用︰ 键入是**SetPassword**。
旧密码 | 字符串 | 填充对象的密码更改的旧密码。 此参数才可用︰ 键入时**更改密码**。
新密码 | 字符串 | 填充对象的脚本应该设置的新密码。

该密码脚本不需要 Windows PowerShell 管道中返回任何结果。 如果密码脚本中出现错误，脚本应引发下列异常通知同步服务相关的问题之一︰

- [PasswordPolicyViolationException] [ pwdex1] – 如果密码不符合密码策略连接的系统中，则引发。
- [PasswordIllFormedException] [ pwdex2] – 如果密码不是可接受的连接的系统。
- [PasswordExtension] [ pwdex3] – 引发对于密码脚本中的所有其他错误。

## <a name="sample-connectors"></a>示例连接器
可用的示例连接器的完整概述，请参阅[Windows PowerShell 连接器示例连接器集合][samp]。

## <a name="other-notes"></a>其他说明

### <a name="additional-configuration-for-impersonation"></a>模拟其他配置
授予用户模拟同步服务服务器上的以下权限︰

读取访问以下注册表项︰

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

要确定同步服务的服务帐户的安全标识符 (SID)，请运行以下 PowerShell 命令︰

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

对以下文件系统文件夹的读取权限︰

- %ProgramFiles%\Microsoft 最前沿标识 Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft 最前沿标识 Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft 最前沿标识 Manager\2010\Synchronization Service\MaData\\{ConnectorName}

{ConnectorName} 占位符替换 Windows PowerShell 连接器的名称。

## <a name="troubleshooting"></a>故障排除

-   有关如何启用日志记录来解决连接器的信息，请参阅[如何启用 ETW 跟踪连接器](http://go.microsoft.com/fwlink/?LinkId=335731)。

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
