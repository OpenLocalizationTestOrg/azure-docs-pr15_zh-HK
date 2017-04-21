<properties
   pageTitle="一般的 LDAP 接口 |Microsoft Azure"
   description="本文介绍如何配置 Microsoft 的通用 LDAP 连接符。"
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

# <a name="generic-ldap-connector-technical-reference"></a>一般的 LDAP 接口技术参考
本文介绍了通用 LDAP 连接符。 文章将应用于以下产品︰

- 2016 年 Microsoft 标识管理器 (MIM2016)
- Forefront 标识管理器 2010 R2 (FIM2010R2)
    -   必须使用修补程序 4.1.3671.0 或更高版本的[KB3092178](https://support.microsoft.com/kb/3092178)。

对于 MIM2016 和 FIM2010R2，连接器是可从[Microsoft 下载中心](http://go.microsoft.com/fwlink/?LinkId=717495)下载。

当提及 IETF Rfc，此文档使用格式 (RFC [RFC 编号] / [RFC 文档中节])，例如 (RFC 4512/4.3)。
您可以在 http://tools.ietf.org/html/rfc4500 （您需要替换为正确的 RFC 编号 4500） 找到详细信息。

## <a name="overview-of-the-generic-ldap-connector"></a>通用 LDAP 接口概述
一般的 LDAP 接口使您可以与 LDAP v3 服务器集成的同步服务。

在 IETF Rfc 中未指定某些操作，如执行增量导入所需的架构元素。 对于这些操作，支持显式指定唯一的 LDAP 目录。

从高层次的角度来看，通过当前版本的连接器支持以下功能︰

功能 | 支持
--- | --- |
连接的数据源 | 连接器支持所有 LDAP v3 服务器 (RFC 4510 兼容)。 它经过如下︰ <li>Microsoft 活动目录轻量级目录服务 (AD LDS)</li><li>Microsoft 活动目录全局编录 (AD GC)</li><li>389 目录服务器</li><li>Apache 目录服务器</li><li>IBM Tivoli DS</li><li>Isode 目录</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>打开 DJ</li><li>打开 DS</li><li>打开 LDAP (openldap.org)</li><li>Oracle （以前周日） 目录服务器的企业版</li><li>RadiantOne 虚拟目录服务器 (VDS)</li><li>周日一台目录服务器</li>**值得注意的目录不受支持︰** <li>Microsoft Active Directory 域服务 (AD DS) [改为使用内置的活动目录连接器]</li><li>Oracle Internet 目录 (OID)</li>
方案   | <li>对象生命周期管理</li><li>组管理</li><li>密码管理</li>
操作 |在所有的 LDAP 目录上支持以下操作︰ <li>完全导入</li><li>导出</li>在指定的目录上只支持下列操作︰<li>增量导入</li><li>设置密码，则更改密码</li>
架构 | <li>从 LDAP 架构 （RFC3673 和 RFC4512/4.2） 检测到架构</li><li>支持结构类、 辅助类和 extensibleObject 对象类 (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>增量导入和密码管理支持
支持增量导入和密码管理的目录︰

- Microsoft 活动目录轻量级目录服务 (AD LDS)
    - 支持增量导入的所有操作
    - 支持设置密码
- Microsoft 活动目录全局编录 (AD GC)
    - 支持增量导入的所有操作
    - 支持设置密码
- 389 目录服务器
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- Apache 目录服务器
    - 由于此目录没有永久性更改日志不支持增量导入
    - 支持设置密码
- IBM Tivoli DS
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- Isode 目录
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- Novell eDirectory 和 NetIQ eDirectory
    - 支持增量导入进行添加、 更新和重命名操作
    - 不支持删除操作进行增量导入
    - 支持设置密码和更改密码
- 打开 DJ
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- 打开 DS
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- 打开 LDAP (openldap.org)
    - 支持增量导入的所有操作
    - 支持设置密码
    - 不支持更改密码
- Oracle （以前周日） 目录服务器的企业版
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
- RadiantOne 虚拟目录服务器 (VDS)
    - 必须使用 7.1.1 版本或更高版本
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码
-  周日一台目录服务器
    - 支持增量导入的所有操作
    - 支持设置密码和更改密码

### <a name="prerequisites"></a>系统必备组件
使用连接器之前，请确保您具有同步服务器上的以下︰

- Microsoft.NET 4.5.2 框架或更高版本

### <a name="detecting-the-ldap-server"></a>检测该 LDAP 服务器
该连接器依赖于各种技术来检测和识别的 LDAP 服务器。 路由组连接器使用根 DSE、 供应商名称或版本，它会检查要查找唯一的对象和属性已知存在于特定的 LDAP 服务器的架构。 这些数据，如果找到，则使用预填充在连接器的配置选项。

### <a name="connected-data-source-permissions"></a>连接的数据源的权限
执行导入和导出操作在连接目录中的对象上，连接器帐户必须具有足够的权限。 连接器需要写权限以便能够导出，以及读取权限以便能够导入。 执行权限配置中的目标目录本身的管理经验。

### <a name="ports-and-protocols"></a>端口和协议
路由组连接器使用的配置，即默认情况下用于 LDAP 的 389 和 LDAPS 的 636 中指定的端口号。

对于 LDAPS，您必须使用 SSL 3.0 或 TLS。 SSL 2.0 不受支持，无法激活。

### <a name="required-controls-and-features"></a>必选的控件和功能
下面的 LDAP 控件/功能必须在连接器正常工作的 LDAP 服务器上可用︰  
`1.3.6.1.4.1.4203.1.5.3`真/假筛选器

真/假筛选器经常未报告为支持的 LDAP 目录，可能会显示在**全局页**下**未找到强制的功能**。 它用来在 LDAP 查询中，例如，当导入多个对象类型创建**或**筛选器。 如果您可以导入多个对象类型，则 LDAP 服务器支持此功能。

如果您使用一个目录位置的唯一标识符是锚也必须以下可用 （请参阅下文中的详细信息的[配置标记](#configure-anchors)部分）︰  
`1.3.6.1.4.1.4203.1.5.1`所有操作属性

如果目录中有更多比什么可以容纳一个调用到目录中的对象，然后建议使用分页。 用于分页工作，您需要下列选项之一︰

**选项 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**选项 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

如果在连接器配置中启用了这两个选项，则使用 pagedResultsControl。

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl 仅使用 USNChanged 增量导入方法能够查看已删除的对象。

连接器将尝试检测服务器上存在的选项。 如果不能检测到所选项，警告是在连接器属性出现在共用网页上。 他们支持并不是所有 LDAP 服务器存在所有的控制功能，即使此警告，则该连接器可能工作没有问题。

### <a name="delta-import"></a>增量导入
在检测到支持目录时，增量导入才可用。 当前使用以下方法︰

- LDAP Accesslog。 请参阅[http://www.openldap.org/doc/admin24/overlays.html#Access 日志记录](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP 的更改日志。 请参见[http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- 时间戳。 为 Novell/NetIQ eDirectory 连接器使用最后一个日期/时间能够创建和更新过的对象。 Novell/NetIQ eDirectory 不提供等效意味着检索已删除的对象。 如果没有其他增量导入方法在 LDAP 服务器上处于活动状态，也可以使用此选项。 此选项不能删除导入的对象。
- USNChanged。 请参见︰ [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>不受支持
不支持下面的 LDAP 功能︰

- LDAP 服务器 (RFC 4511/4.1.10) 之间的引用

## <a name="create-a-new-connector"></a>创建一个新的连接器
若要创建通用 LDAP 连接器，**同步服务**中选择**管理代理**和**创建**。 选择**通用 LDAP (Microsoft)**连接器。

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>连接
在连接页中，必须指定的主机、 端口和绑定信息。 根据绑定是选定，其他可能在以下各节中提供信息。

![连接](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- 连接超时设置仅用于第一次连接到服务器时检测架构。
- 如果绑定是匿名，然后两个用户名 / 密码和证书使用。
- 其他绑定，请输入信息或者在用户名 / 密码或选择一个证书。
- 如果您使用 Kerberos 进行身份验证，还提供用户的领域/域。

**属性别名**文本框用于 RFC4522 语法的架构中定义的属性。 这些属性不能检测到架构检测期间，连接器需要帮助来标识这些特性。 例如以下需要在属性别名中，为了正确地标识作为二进制属性的用户证书属性中输入︰

`userCertificate;binary`

下面是一个用于此配置可以象示例︰

![连接](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

选择**包含操作属性架构中的**复选框，还包括由服务器创建的属性。 这些包括属性，例如当对象创建和上次更新时间。

如果使用可扩展对象 (RFC4512/4.3) 并启用该选项，在所有的对象上使用的每个属性，请选择**架构中的包含可扩展特性**。 选择此选项会架构非常大，这样除非连接的目录使用此功能的推荐方法是保留选项处于未选中状态。

### <a name="global-parameters"></a>全局参数
在全局参数页上，您可以配置的增量更改日志和其他 LDAP 功能的 DN。 该页面也提供由 LDAP 服务器的信息预填充。

![连接](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

上半部分显示信息由服务器本身，例如，服务器的名称。 连接器还验证强制控件存在于根 DSE。 如果未列出这些控件，则显示一条警告。 一些 LDAP 目录待办事项列表中根 DSE 的所有功能，还可能连接器工作没有问题，即使警告。

**支持控件**的复选框来控制某些操作的行为︰

- 与所选的目录树删除，删除层次结构使用一个 LDAP 调用。 与目录树删除未选定该属性，连接器会递归删除，如果需要。
- 选择分页的结果，与连接器大小指定的运行的步骤在执行分页的导入。
- VLVControl 和 SortControl 是 pagedResultsControl 从 LDAP 目录读取数据的替代方法。
- 如果未选中所有三个选项 (pagedResultsControl，VLVControl 和 SortControl) 连接器导入所有对象在一次操作，如果它是一个大的目录可能会失败。
- USNChanged 的增量导入方法时仅使用 ShowDeletedControl。

更改日志 DN 是命名上下文使用增量更改日志，例如**cn = 更改日志**。 必须指定此值，以便能够执行增量导入。

以下是默认 DNs 的更改日志的列表︰

目录 | 增量更改日志
--- | ---
Microsoft AD LDS 和 AD GC | 自动检测。 USNChanged。
Apache 目录服务器 | 不可用。
目录 389 | 更改日志。 默认值使用︰ **cn = 更改日志**
IBM Tivoli DS | 更改日志。 默认值使用︰ **cn = 更改日志**
Isode 目录 | 更改日志。 默认值使用︰ **cn = 更改日志**
Novell/NetIQ eDirectory | 不可用。 时间戳。 连接器使用上次更新的日期/时间来添加和更新记录。
打开的 DJ DS | 更改日志。  默认值使用︰ **cn = 更改日志**
打开 LDAP | 访问日志。 默认值使用︰ **cn = accesslog**
Oracle DSEE | 更改日志。 默认值使用︰ **cn = 更改日志**
RadiantOne VDS | 虚拟目录。 取决于连接到 VDS 的目录。
周日一台目录服务器 | 更改日志。 默认值使用︰ **cn = 更改日志**

口令属性是连接器应使用设置密码，密码更改的属性的名称和密码设置操作。
此值是默认设置为**用户密码**，但需要为特定的 LDAP 系统时可以更改。

在其他的分区列表中，则可以添加其他命名空间不会自动检测到。 例如，如果多个服务器组成一个逻辑群集，应该全部被导入一次，则可以使用此设置。 正如活动目录可以在一个目录林中有多个域，但所有域都共享一个架构，可通过在此框中输入其他命名空间模拟相同。 每个命名空间可以从不同的服务器导入，并进一步配置配置分区和层次结构页上。 使用 Ctrl + enter 键来获取一个新行。

### <a name="configure-provisioning-hierarchy"></a>配置配置层次结构
此页用于将该 DN 组件，例如 OU 中，映射到对象类型应调配，例如 organizationalUnit。

![配置层次结构](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

通过配置设置层次结构，您可以将连接器配置为自动创建结构时需要。 例如，如果名称空间 dc = contoso，dc com 和新对象的 cn = = Joe，ou = 西雅图，c = 美国，dc = contoso，dc = 配置 com，则连接器可以创建对象类型对于美国的国家/地区和西雅图的 organizationalUnit，如果那些尚不存在的目录中。

### <a name="configure-partitions-and-hierarchies"></a>配置分区和层次结构
在分区和层次结构页上，选择与您要导入和导出的对象的所有命名空间。

![分区](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

每个命名空间，则还可以配置连接设置将重写在连接屏幕中指定的值。 如果这些值保留为其默认为空值，则使用连接屏幕中的信息。

也是可以选择的容器和 Ou 连接器应从导入和导出。

### <a name="configure-anchors"></a>配置标记
此页始终具有一个预配置的值，不能更改。 如果服务器供应商已发现，锚可能填入不可变属性，例如对象的 GUID。 如果未检测到或已知不具有不可变属性，然后连接器使用 dn （可分辨名称） 作为定位点。

![定位点](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

下面是一个列表的 LDAP 服务器和正在使用的定位点︰

目录 | 定位属性
--- | ---
Microsoft AD LDS 和 AD GC | objectGUID
389 目录服务器 | dn
Apache 目录 | dn
IBM Tivoli DS | dn
Isode 目录 | dn
Novell/NetIQ eDirectory | GUID
打开的 DJ DS | dn
打开 LDAP | dn
Oracle ODSEE | dn
RadiantOne VDS | dn
周日一台目录服务器 | dn

## <a name="other-notes"></a>其他说明
此部分提供了特定于此连接器或出于其他原因都需要了解的方面的信息。

### <a name="delta-import"></a>增量导入
在打开 LDAP 的增量水印是 UTC 日期/时间。 鉴于此，FIM 同步服务之间打开 LDAP 时钟必须同步。 如果没有，增量更改日志中的某些条目可能被忽略。

Novell eDirectory 的增量导入无法检测到任何对象的删除。 有鉴于此，有必要进行完全导入定期以查找所有已删除的对象。

对于与根据日期/时间的增量更改日志目录，强烈建议在定期时间运行完整的导入。 此过程允许同步引擎来查找和 LDAP 服务器和连接器空间中当前是之间的 dissimilarities。

## <a name="troubleshooting"></a>故障排除

-   有关如何启用日志记录来解决连接器的信息，请参阅[如何启用 ETW 跟踪连接器](http://go.microsoft.com/fwlink/?LinkId=335731)。
