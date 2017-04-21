<properties
   pageTitle="Lotus Domino 连接器 |Microsoft Azure"
   description="本文介绍如何配置 Microsoft 的 Lotus Domino 连接器。"
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

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino 连接器技术参考
本文介绍了 Lotus Domino 连接器。 文章将应用于以下产品︰

- 2016 年 Microsoft 标识管理器 (MIM2016)
- Forefront 标识管理器 2010 R2 (FIM2010R2)
    -   必须使用修补程序 4.1.3671.0 或更高版本的[KB3092178](https://support.microsoft.com/kb/3092178)。

对于 MIM2016 和 FIM2010R2，连接器是可从[Microsoft 下载中心](http://go.microsoft.com/fwlink/?LinkId=717495)下载。

## <a name="overview-of-the-lotus-domino-connector"></a>Lotus Domino 连接器的概述
Lotus Domino 连接器可以与 IBM 的 Lotus Domino 服务器集成的同步服务。

从高层次的角度来看，通过当前版本的连接器支持以下功能︰

功能 | 支持
--- | ---
连接的数据源 | 服务器︰ <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>客户端︰<li>Lotus Notes 9.x</li>
方案 | <li>对象生命周期管理</li><li>组管理</li><li>密码管理</li>
操作 | <li>完整和增量导入</li><li>导出</li><li>设置和更改 HTTP 密码的密码</li>
架构 | <li>人 （漫游用户，联系人 （没有证书的人员））</li><li>组</li><li>资源 （资源，房间，联机会议）</li><li>在邮件数据库中</li><li>动态发现的特性支持的对象。</li>

Lotus Domino 连接器使用 Lotus Notes 客户端与 Lotus Domino 服务器进行通信。 由于这种依赖性，必须同步服务器上安装受支持的 Lotus Notes 客户端。 在客户端和服务器之间的通信是通过 Lotus Notes.NET 互操作 (Interop.domino.dll) 界面实现的。 此接口便于 Microsoft.NET 平台和 Lotus Notes 客户端之间的通信，并支持对 Lotus Domino 文档和视图的访问。 增量导入，还有可能，（具体取决于选定的增量导入方法） 使用 c + + 本机接口。

### <a name="prerequisites"></a>系统必备组件
使用连接器之前，请确保您具有同步服务器上的以下︰

- Microsoft.NET 4.5.2 框架或更高版本
- 同步服务器上必须安装 Lotus Notes 客户端
- Lotus Domino 连接器需要默认 Lotus Domino LDAP 架构的数据库 (schema.nsf) 存在于 Domino 目录服务器。 如果不存在，您可以运行或重新启动在 Domino 服务器上的 LDAP 服务安装。

### <a name="connected-data-source-permissions"></a>连接的数据源的权限
若要执行任何支持的任务在 Lotus Domino 连接器，您必须是下列组的成员︰

- 完全访问权限的管理员
- 管理员
- 数据库管理员

下表列出了每个操作所需的权限︰

操作 | 访问权限
--- | ---
导入 | <li>阅读公共文档</li><li> 完全访问管理员 （完全访问权限的管理员组的成员时，会自动可以有效访问 ACL 中。）</li>
导出并设置密码 | 有效的访问︰ <li>创建文档</li><li>删除文档</li><li>阅读公共文档</li><li>编写公共文档</li><li>复制或复制文档</li>对于导出的操作，您还需要以下角色︰ <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>直接操作 AdminP
可以转到 Domino 目录直接或通过 AdminP 过程操作。 下面的表列出所有受支持的对象，操作和，如果适用，请相关的实现方法︰

**主地址簿**

对象 | 创建 | 更新 | 删除
--- | --- | --- | ---
人员 | AdminP | 直接 | AdminP
组 | AdminP | 直接 | AdminP
MailInDB | 直接 | 直接 | 直接
资源 | AdminP | 直接 | AdminP

**次要地址簿**

对象 | 创建 | 更新 | 删除
--- | --- | --- | ---
人员 | N/A | 直接 | 直接
组 | 直接 | 直接 | 直接
MailInDB | 直接 | 直接 | 直接
资源 | N/A | N/A | N/A

创建资源时，将创建 Notes 文档中。 同样，如果删除某个资源时，Notes 文档将被删除。

### <a name="ports-and-protocols"></a>端口和协议
使用说明远程过程调用 (NRPC) NRPC 使用 TCP/IP 的位置 IBM Lotus Notes 客户端和 Domino 服务器通信。 默认端口号为 1352，但通过 Domino 管理员可以更改。

### <a name="not-supported"></a>不受支持
当前版本的 Lotus Domino 连接器不支持以下操作︰

- 服务器之间移动邮箱。

## <a name="create-a-new-connector"></a>创建一个新的连接器

### <a name="client-software-installation-and-configuration"></a>客户端软件安装和配置
必须在服务器**之前**安装了接头上安装 lotus Notes。

在安装时，请确认执行**单个用户安装**。 默认**安装多用户**无法正常工作。  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

在功能页面上安装所需的 Lotus Notes 功能和**客户端单一登录**。 单一登录都必须要能够登录到 Domino 服务器上的连接器。  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**注意︰**从 Lotus Notes 开始一次用作连接器服务帐户的帐户所在的服务器上的用户。 此外请确保关闭 Lotus Notes 客户端在服务器上。 不能运行连接器连接到 Domino 服务器会尝试一次。

### <a name="create-connector"></a>创建连接器
若要创建 Lotus Domino 连接器，**同步服务**中选择**管理代理**和**创建**。 选择的**Lotus Domino (Microsoft)**连接器。  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

如果您的同步服务版本提供了配置**体系结构**的能力，确保连接器设置为其默认值，在**进程**中运行。

### <a name="connectivity"></a>连接
在连接页中，必须指定 Lotus Domino 服务器的名称，并输入登录凭据。  
![连接](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Domino 服务器属性的服务器名支持两种格式︰

- 服务器名
- 服务器名/目录名

**服务器名/目录名称**格式是此属性的首选的格式，因为连接器与 Domino 服务器联系时它可以提供更快的响应。

提供的用户 Id 文件存储在配置数据库中的同步服务。

**增量**导入可以使用这些选项︰

- **无**。 连接器不会执行任何增量导入。
- **添加/更新**。 连接器执行增量导入添加和更新操作。 为删除，**完全导入**操作是必需的。 此操作使用.Net 互操作。
- **添加、 更新或删除**。 连接器执行增量导入中添加、 更新和删除操作。 此操作使用本机 c + + 接口。

在**架构选项**您有下列选项︰

- **默认架构**。 接头检测从 Domino 服务器的架构。 这是默认选项。
- **DSML 架构**。 如果 Domino 服务器不公开架构，才使用。 然后可以创建包含架构的 DSML 文件并改为导入它。 DSML 的详细信息，请参阅[OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml)。

当您单击下一步时，进行验证的用户 Id 和密码的配置参数。

### <a name="global-parameters"></a>全局参数
在全局参数页上，您可以配置时区和导入和导出操作选项。  
![全局参数](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Domino 服务器的时区**参数定义的 Domino 服务器的位置。

此配置选项是必需以支持**增量导入**操作，因为它可以使同步服务确定最后两个导入之间的更改。

#### <a name="import-settings-method"></a>导入设置，方法
**执行完全导入的**具有下列选项︰

- 搜索
- 视图 （推荐）

**搜索**将使用 Domino 中的索引，但通常不会实时更新的索引，然后从服务器返回的数据并不总是正确。 对于具有多变化的系统，此选项通常也不起作用，并提供假删除在某些情况下。 但是，**搜索**是**视图**比速度更快。

**视图**是推荐的选项，因为它提供数据的正确状态。 它是比搜索速度稍慢。

#### <a name="creation-of-virtual-contact-objects"></a>虚拟联系人对象的创建
**可帮助创建\_联系人对象**具有下列选项︰

- 无
- 非参考值
- 引用和非引用的值

在 Domino，引用特性可以包含许多不同的格式，以引用其他对象。 为了能够表示不同的变体，连接器实现\_联系人对象，也称为**虚拟联系人**(VC)。 这些对象创建使他们可以加入到现有的 MV 对象或为新对象投影。 这种方式，可以保留属性的引用。

通过启用此设置，如果引用属性的内容没有 DN 格式\_创建联系人对象。 例如，组的 member 属性可以包含 SMTP 地址。 还有可能具有短名称和其他属性中引用的属性。 对于这种情况下，选择**非参考值**。 此配置是 Domino 实现最常用的设置。

如果 Lotus Domino 被配置为具有不同代表同一对象的可分辨名称具有单独的通讯簿，则可能还创建\_联系人找到通讯簿中的所有引用值的对象。 这种情况下，选择**引用，非引用的值**选项。

如果在 Domino **FullName**属性中有多个值，您也想要启用的虚拟联系人创建，因此可以解析引用。 例如，此属性可以在结婚或离婚后具有多个值。 选中此复选框启用**...FullName 具有多个值**为此方案。

通过加入正确的属性，在\_联系人对象将加入 MV 对象。

这些对象具有 VC =\_联系人添加到他们的 DN。

#### <a name="import-settings-conflict-object"></a>导入设置，发生冲突的对象
**排除冲突对象**

在大型的 Domino 实现中，就可能多个对象具有相同 DN 由于复制问题。 在这些情况下，连接器会看到两个对象具有不同的 UniversalIDs，但相同的 DN。 这种冲突会导致连接器空间中所创建临时对象。 连接器可以忽略复制受害者作为选择 Domino 中的对象。 建议是要保留选中此复选框。

#### <a name="export-settings"></a>导出设置
如果**更新引用使用 AdminP**选项未被选定，导出的引用属性，如成员，是一个直接调用，并且不使用 AdminP 过程。 AdminP 未配置为以保持参照完整性时，才使用此选项。

#### <a name="routing-information"></a>路由信息
在 Domino，很可能引用属性具有嵌入的 dn 后缀的路由信息。 例如，可以包含一组中的成员属性**CN=example/organization@ABC**。 后缀@ABC是路由选择信息。 Domino 使用路由信息将电子邮件发送到正确的 Domino 系统可能是在不同的组织系统。 在路由信息字段中，您可以指定路由使用连接器作用域中的组织结构中的后缀。 如果两个值之一作为后缀找到引用属性中，路由信息被从该引用。 如果对引用值的路由后缀不能匹配指定，这些值之一\_创建联系人对象。 这些\_联系人对象使用创建的**RO=@**插入 DN。 这些\_联系人对象也添加下列属性以允许加入真正的对象，如有必要︰ \_routingName，\_联系人姓名，\_显示名称，和 UniversalID。

#### <a name="additional-address-books"></a>附加通讯簿
如果您没有**目录协助**安装，它提供辅助的通讯簿的名称，则可以手动输入这些通讯簿。

#### <a name="multivalued-transformation"></a>多值的转换
在 Lotus Domino 中的很多属性是多值的。 对应的元节属性是通常单个值。 通过配置导入和导出操作选项，您可以启用连接器来帮助受影响的属性需要翻译。

**导出**  
导出操作选项支持两种模式︰

- 附加项目
- 替换的项目

**替换项目**– 当您选择此选项，则该连接器始终在 Domino 中移除的属性的当前值，并将它们替换为提供的值。 提供的值可以是单值或多值。

示例︰ 一个 person 对象的助手特性具有以下值︰

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

如果一个名为**David Alexander**的新助理分配给此用户对象时，其结果是︰

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**附加项**– 选择此选项，则该连接器时保留在 Domino 中的属性的现有值和顶部的数据列表中插入新值。

示例︰ 一个 person 对象的助手特性具有以下值︰

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

如果一个名为**David Alexander**的新助理分配给此用户对象时，其结果是︰

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**导入**  
导入操作选项支持两种模式︰

- 默认
- 为单个值的多值

**默认值**– 选择默认选项时，将导入的所有属性的所有值。

**为单个值的 Multivalued** – 选择此选项，多值属性时将转换为单值的属性。 如果存在多个值，则使用的值 （该值通常也是最新的） 上。

示例︰ 一个 person 对象的助手特性具有以下值︰

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

最新的更新到此特性是**David Alexander**。 由于导入操作选项被设置为单个值为 Multivalued，连接器只将**David Alexander**导入连接器空间。

若要将多值的属性转换为单值属性的逻辑不适用于对组成员属性和人 fullname 属性。

此外可以配置导入和导出转换规则的每个属性，多值属性为全局规则的例外。 若要配置此选项，请输入 [对象类型]。[属性名称] 中的**排除属性列表导入**和**导出排除属性列表**文本框。 例如，如果输入 Person.Assistant，并且设置全局标志要导入的所有值，仅第一个值为导入助手。

#### <a name="certifiers"></a>Certifiers
所有的公司/组织单位列出连接器。 若要能够将 person 对象导出到主地址簿，用其密码验证者是必需的。

如果所有的 certifiers 有相同的密码，可以使用**所有 Certifers 的密码**。 那么您可以输入密码并只有指定的验证者文件。

如果您只导入，然后您不必指定任何 certifiers。

### <a name="configure-provisioning-hierarchy"></a>配置配置层次结构
Lotus Domino 连接器配置时，请跳过此对话框页面。 Lotus Domino 连接器不支持资源调配的层次结构。  
![配置层次结构](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>配置分区和层次结构
在配置分区和层次结构时，您必须选择名为 NAB=names.nsf 的主地址簿。 除了主要的通讯簿中，您可以选择辅助的通讯簿，如果它们存在。  
![分区](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>选择属性
当您配置您的属性时，您必须选择带有前缀的所有特性**\_MMS\_**。 这些属性是必需的当您配置新对象到 Lotus Domino

![属性](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>对象生命周期管理
本节概述了 Domino 中的不同对象。

### <a name="person-objects"></a>Person 对象
Person 对象代表组织和组织单位中的用户。 除了默认的属性，Domino 管理员可以添加一个 Person 对象的自定义属性。 至少，一个 Person 对象必须包含所有必选属性。 必填字段的属性的完整列表，请参阅[Lotus Notes 属性](#lotus-notes-properties)。 若要注册一个 person 对象，必须满足以下先决条件︰

- 通讯簿 (names.nsf) 必须具有已定义，它应该是主要的通讯簿。
- 必须具有 O/组织单位验证者 Id 和密码注册在该组织中的特定用户 / 组织单位。
- 您必须设置一组特定的 Lotus Notes 一个 person 对象的属性。 这些属性用于设置 person 对象。 有关更多详细信息，请参阅本文档下文中称为[Lotus Notes 属性](#lotus-notes-properties)的部分。
- 一个人的初始 HTTP 密码是在资源调配过程中属性和设置。
- Person 对象必须是下列三个受支持的类型之一︰
    1. 普通用户的邮件文件和用户 id 文件
    2. 漫游用户 （包括漫游的所有数据库文件的普通用户）
    3. （有没有 id 文件的用户） 的联系人

（联系人除外） 的人员可以进一步分为美国用户和国际用户定义的值的\_MMS\_IDRegType 属性。 这些人使用 Notes 客户机访问 Lotus Domino 服务器有一个注释 Id 和用户文档。 如果他们使用 Notes 邮件，则它们也具有邮件文件。 用户必须注册才能生效。 有关详细信息，请参阅︰

- [Notes 用户设置](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [用户注册](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [管理用户](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [重命名用户](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

所有这些操作都在 Lotus Domino 中执行，然后导入到的同步服务。

### <a name="resources-and-rooms"></a>资源和文件室
资源是在 Lotus Domino 数据库的另一种类型。 资源可以是设备的与各种类型，例如投影机的会议室。 有的 Lotus Domino 连接器所支持的资源，由资源类型属性定义的子类型︰

资源类型 | 资源类型属性
--- | ---
文件室 | 1
资源 （其他） | 2
联机会议 | 3

有关资源的对象类型工作，必须满足下列条件︰

- 资源保留数据库应该存在连接的 Domino 服务器中
- 该站点已定义资源

资源预订数据库包含三种类型的文档︰

- 站点配置文件
- 资源
- 预留

资源预订数据库的设置的详细信息，请参阅[设置资源预留数据库](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html)。

**创建、 更新和删除资源**  
由 Lotus Domino 连接器资源保留数据库中执行创建、 更新和删除操作。 作为 Names.nsf （就是主地址簿） 中的文档创建的资源。 关于编辑和删除资源的详细信息，请参阅[编辑和删除资源的文档](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html)。

**导入和导出操作的资源**  
可以导入和导出从同步服务，像任何其他对象类型资源。 在配置过程中为资源选择对象类型。 为了成功导出操作，您应该有资源类型、 会议数据库和网站名称的详细信息。

### <a name="mail-in-databases"></a>邮件中的数据库
邮寄数据库是一个数据库，用来接收邮件。 它是不是与任何特定的 Lotus Domino 用户帐户相关联的 Lotus Domino 邮箱 （也就是说，它没有自己的 ID 文件和密码）。 邮件数据库具有唯一用户标识 （"短名称"） 与它和自己的电子邮件地址相关联。

如果没有与自己的电子邮件地址，可以在不同用户间共享的单独邮箱需要 (例如，group@contoso.com),创建邮件数据库。 对此邮箱的访问权限控制通过其访问控制列表 (ACL)，其中包含允许打开邮箱 Notes 用户的名称。

所需属性的列表，请参阅本文内下文中称为[必选属性](#mandatory-attributes)的部分。

当一个数据库被设计为接收邮件时，在邮件数据库中的文档在 Lotus Domino 创建。 此文档必须位于 Domino 目录的每个服务器的存储数据库的一个副本。 关于创建邮件在数据库中的文档的详细说明，请参阅[创建邮件在数据库中的文档](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)。

之前创建邮件数据库，该数据库应已存在 （应该是由 Lotus 管理） 在 Domino 服务器上。

### <a name="group-management"></a>组管理
您可以从以下资源获得 Lotus Domino 组管理的详细的概述︰

- [使用组](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [创建组](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [创建和修改组](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [管理组](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [重命名组](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>密码管理
对于已注册的 Lotus Domino 用户，有两种类型的密码︰

1. 用户密码 （存储在 User.id 文件中）
2. 互联网 / HTTP 密码

Lotus Domino 连接器支持只使用 HTTP 密码的操作。

若要执行密码管理，您应该启用管理代理程序设计器中的连接器的密码管理。 若要启用密码管理，**配置扩展**对话框页面上选择**启用密码管理**。  
![配置扩展](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Lotus Domino 连接器支持以下互联网密码的操作︰

- 设置密码︰ 设置密码在 Domino 用户设置一个新的 HTTP/互联网密码。 默认情况下未还锁定帐户。 同步引擎的 WMI 接口上公开解锁标志。
- 更改密码︰ 在这种情况下，用户可能想要更改密码或提示指定的时间后更改密码。 要执行此操作，是必需的 （旧名称和新的密码）。 一旦更改，新密码已更新 Lotus Domino 中。

有关详细信息，请参阅︰

- [使用 Internet 锁定功能](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [管理互联网密码](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>参考信息
本部分列出了属性说明和 Lotus Domino 接头的特性要求等。

### <a name="lotus-notes-properties"></a>Lotus Notes 属性
当您配置到您的 Lotus Domino 目录的 Person 对象时，对象必须具有一组特定的属性与特定的值填充。 这些值只需的创建操作。

下表列出了这些属性，并描述了它们。

属性 | 说明
--- | ---
\_MMS_AltFullName | 替代用户的全名。
\_MMS_AltFullNameLanguage | 要用于指定用户的完整名称的备用语言。
\_MMS_CertDaysToExpire | 过期的证书之前的当前日期的天数。 如果未指定，则默认日期是当前日期两年。
\_MMS_Certifier | 包含验证者的组织层次结构名称的属性。 例如︰ OU = OrganizationUnit，O = Org，C = 国家。
\_MMS_IDPath | 如果该属性为空，则同步服务器上本地创建没有用户标识文件。 如果属性包含文件的名称，用户 ID 文件创建的 madata 文件夹中。 该属性还可以包含完整路径。
\_MMS_IDRegType | 人可以被归类为联系人、 美国用户和国际用户。 下表列出可能的值︰ <li>0-联系人</li><li>1-美国用户</li><li>2-国际用户</li>
\_MMS_IDStoreType | 美国和国际用户所需的属性。 该属性包含一个整数值，指定是否作为附件在 Notes 通讯簿或此人的邮件文件中存储的用户标识。 如果该用户 ID 文件的地址簿中的附件，它可以 （可选） 创建为一个文件\_MMS_IDPath。 <li>文件为空的存储库 ID ID 存储库，（用于联系人） 没有标识文件中。</li><li> 1-附件说明通讯簿中。 \_MMS_Password 属性必须设置为附加的用户标识文件</li><li>2-存储在用户的邮件文件中的 ID。 \_MMS_UseAdminP 必须设置为 false，以便在用户注册过程中创建的邮件文件。 \_MMS_Password 属性必须设置为用户标识文件。</li>
\_MMS_MailQuotaSizeLimit | 允许电子邮件文件数据库的兆字节数。
\_MMS_MailQuotaWarningThreshold | 允许电子邮件文件数据库之前会发出一个警告的兆字节数。
\_MMS_MailTemplateName | 用于创建该用户的电子邮件文件的电子邮件模板文件。 如果指定一个模板，则使用指定的模板创建邮件文件。 如果不指定任何模板，则使用默认模板文件来创建文件。
\_MMS_OU | 已验证者下的 OU 名称的可选属性。 该属性应为空的联系人。
\_MMS_Password | 用户的所需的属性。 属性包含对象的标识文件的密码。
\_MMS_UseAdminP | 属性应设置为 true，如果邮件文件应创建 AdminP 过程对 Domino 服务器上 （异步的导出过程）。 如果属性设置为 false 时，邮件文件创建与 Domino 用户 （在导出过程中同步）。

对于具有关联的标识文件的用户\_MMS_Password 属性必须包含一个值。 通过 Lotus Notes 客户端的电子邮件访问，用户的邮件服务器和 MailFile 属性必须包含一个值。

通过 Web 浏览器访问的电子邮件，以下属性必须包含值︰

- MailFile-必需的属性包含在 Lotus Domino 服务器的邮件文件的存储位置的路径。
- 邮件服务器的必需的属性包含 Lotus Domino 服务器的名称。 此值是创建在 Domino 服务器上的 Lotus 邮件文件时要使用的名称。
- HTTPPassword 的可选属性，其中包含对象的 Web 访问密码。

若要访问 Domino 服务器没有邮件功能，HTTPPassword 属性必须包含一个值。 MailFile 属性的邮件服务器属性可以为空。

与\_MMS_ IDStoreType = 2 (邮件文件中的存储 id)，NotesRegistrationclass 的 MailSystem 属性设置为 REG_MAILSYSTEM_INOTES (3)。

### <a name="mandatory-attributes"></a>强制属性
Lotus Domino 连接器主要支持这些类型的对象 （文档类型）︰

- 组
- 在邮件数据库中
- 人员
- 联系人 （人与任何验证者）
- 资源

本节列出对于每个受支持的对象导出到 Domino 服务器是必需的属性。

对象类型 | 强制属性
--- | ---
组 | <li>ListName</li>
在主数据库中 | <li>全名</li><li>MailFile</li><li>邮件服务器</li><li>内部</li>
人员 | <li>姓氏</li><li>MailFile</li><li>短名称</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
联系人 （人与任何验证者） | <li>\_MMS_IDRegType</li>
资源 | <li>全名</li><li>资源类型</li><li>ConfDB</li><li>资源能力合计</li><li>网站</li><li>显示名称</li><li>MailFile</li><li>邮件服务器</li><li>内部</li>

## <a name="common-issues-and-questions"></a>常见问题

### <a name="schema-detection-does-not-work"></a>模式检测不起作用
要将能够检测到架构，则有必要 schema.nsf 文件是在 Domino 服务器上存在。 如果在服务器上安装了 LDAP，才会显示此文件。 如果该架构不是可检测到，请验证以下各项︰

- 文件 schema.nsf 在 Domino 服务器的根文件夹存在
- 用户有权查看 schema.nsf 文件。
- 强制重新启动 LDAP 服务器。 打开**Lotus Domino 控制台**并使用**告诉 LDAP ReloadSchema**命令以重新加载架构。

### <a name="not-all-secondary-address-books-are-visible"></a>不是所有的辅地址簿是可见的
Domino 连接器依赖**查号台**能找到辅地址簿的功能。 如果缺少辅助的通讯簿，请验证如果已启用并在 Domino 服务器上配置[目录的帮助](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html)。

### <a name="custom-attributes-in-domino"></a>在 Domino 中的自定义属性
Domino 扩展架构，使其出现连接器可耗用的自定义特性中有几种方法。

**Lotus Domino 架构扩展方法 1:**

1. 创建一份 Domino 目录模板 {PUBNAMES。NTF} 按以下[这些步骤](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html)（应未自定义默认 IBM Lotus Domino 目录模板）︰
2. 打开副本的 Domino 目录模板 {CONTOSO。NTF} 模板在 Domino 设计器中创建的请按照下列步骤操作︰
    - 单击共享的元素，然后展开子窗体
    - 双击 ${对象名称} InheritableSchema 子窗体 (其中 {对象名称} 是默认结构对象类的名称，例如︰ 人)。
    - 要向该属性添加到架构 {MyPersonAtrribute} 和相应的属性的名称。 通过选择**创建**菜单创建一个字段，然后从菜单中选择**字段**。
    - 在添加字段中，选择它的类型、 样式、 大小、 字体和其他相关的参数在字段属性窗口中设置其属性。
    - 保留默认值相同作为该属性的给定名称的属性 （例如，如果属性名称为 MyPersonAttribute，保留具有相同名称的默认值）。
    - 更新后的值保存 ${对象名称} InheritableSchema 子窗体。
3. 替换的 Domino 目录模板 {PUBNAMES。NTF} 与新的自定义模板 {CONTOSO。NTF} 按以下[这些步骤](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)。
4. Domino 管理员关闭和打开 Domino 控制台重新启动 LDAP 服务和重新加载 LDAP 架构︰
    - 在 Domino 控制台中，将插入归档，以便重新启动 LDAP 服务-[重新启动任务 LDAP]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)的**Domino 命令**文本下的命令。
    - 若要重新加载 LDAP 架构使用告诉 LDAP 命令-告诉 LDAP ReloadSchema
5. 打开 Domino 管理员和选择人员和组选项卡以查看添加的属性会反映到 domino 中添加人员。
6. 从**文件**选项卡中打开 Schema.nsf，请参阅添加的属性反映到 dominoPerson 的 LDAP 对象类。

**方法 2︰ 使用自定义属性创建 auxClass 和相关联的对象类**

1. 创建一份 Domino 目录模板 {PUBNAMES。NTF} 按以下[这些步骤](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html)（永远不会自定义模板的默认值 IBM Lotus Domino 目录）︰
2. 打开副本的 Domino 目录模板 {CONTOSO。NTF} 在 Domino 设计器所创建的模板。
3. 在左窗格中，选择代码共享，然后选择子窗体。
4. 单击新的子窗体
5. 请执行下列步骤以指定新的子窗体的属性︰
    - 新的子窗体打开时，选择设计-子窗体属性
    - 旁边的名称属性中，输入辅助对象类，例如，TestSubform 的名称。
    - 保持"包括中插入子窗体...对话框"所选的选项属性
    - 取消选择选项属性"通过在注释中的 HTML 呈现传递"。
    - 保持其他属性不变，并关闭子窗体属性框。
    - 保存并关闭新子窗体。
6. 执行下列操作来添加一个字段，以定义辅助对象类︰
    - 打开您创建的子窗体。
    - 选择创建-字段。
    - 旁边域对话框中的基础知识选项卡上的名称，指定任何名称，例如: {MyPersonTestAttribute}。
    - 在添加字段中，选择其类型、 样式、 大小、 字体和相关的属性中设置其属性。
    - 保留默认值相同作为该属性的给定名称的属性 （例如，如果属性名称为 MyPersonTestAttribute，保留具有相同名称的默认值）。
    - 保存更新后的值与子窗体，请执行下列操作︰
        - 在左窗格中，选择共享代码，然后选择子窗体
        - 选择新的子窗体，然后选择设计-设计属性。
        - 单击左侧，从第三个选项卡，然后选择**传播设计的禁止更改**。
7. 打开 ${对象名称} ExtensibleSchema 子窗体 （其中 {对象名称} 是默认结构对象类，例如--人的名称）。
8. 插入资源并选择子窗体 （即您创建，例如 – TestSubform） 并保存 ${对象名称} ExtensibleSchema 子窗体。
9. 替换的 Domino 目录模板 {PUBNAMES。NTF} 与新的自定义模板 {CONTOSO。NTF} 按以下[这些步骤](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)。
10. Domino 管理员关闭和打开 Domino 控制台重新启动 LDAP 服务和重新加载 LDAP 架构︰
    - 在 Domino 控制台中，将插入归档，以便重新启动 LDAP 服务-[重新启动任务 LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)的**Domino 命令**文本下的命令。
    - 若要重新加载 LDAP 架构使用告诉 LDAP 命令**告诉 LDAP ReloadSchema**。
11. 打开 Domino 管理员并选择用户和组选项卡以查看添加的属性将反映在 domino 添加用户 （在其他选项卡）。
12. 从**文件**选项卡中打开 Schema.nsf，请参阅添加的属性反映下 TestSubform LDAP 辅助对象类。

**方法 3︰ 将自定义属性添加到 ExtensibleObject 类**

1. 打开 {Schema.nsf} 文件放置在根目录
2. 从下**所有架构文档**的左侧菜单中选择 LDAP 对象类并单击**添加对象类别**按钮︰
3. 提供 LDAP {zzzExtensibleSchema} 的形式 （其中 zzz 是默认结构对象类，例如人的名称） 的名称。 例如，用户对象类扩展架构，提供 LDAP 名 {PersonExtensibleSchema}。
4. 提供优异的对象的类名，所需的架构扩展。 例如，若要扩展 Person 对象类的架构，提供优异的对象类名称 {dominoPerson}:
5. 提供了一个有效的 OID，对应的对象类。
6. 选择下按照要求的必需或可选属性类型字段的扩展/自定义特性︰
7. 后将所需的属性添加到 ExtensibleObjectClass 中，单击**保存并关闭**。
8. ExtensibleObjectClass 将为各自的默认对象带有扩展属性的类创建。

## <a name="troubleshooting"></a>故障排除

-   有关如何启用日志记录来解决连接器的信息，请参阅[如何启用 ETW 跟踪连接器](http://go.microsoft.com/fwlink/?LinkId=335731)。
