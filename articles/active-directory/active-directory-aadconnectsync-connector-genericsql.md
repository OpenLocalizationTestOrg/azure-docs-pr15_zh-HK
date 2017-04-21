<properties
   pageTitle="一般的 SQL 接口 |Microsoft Azure"
   description="本文介绍如何配置 Microsoft 的通用 SQL 接口。"
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

# <a name="generic-sql-connector-technical-reference"></a>一般的 SQL 接口技术参考

本文介绍了一般的 SQL 接口。 文章将应用于以下产品︰

- 2016 年 Microsoft 标识管理器 (MIM2016)
- Forefront 标识管理器 2010 R2 (FIM2010R2)
    -   必须使用修补程序 4.1.3671.0 或更高版本的[KB3092178](https://support.microsoft.com/kb/3092178)。

对于 MIM2016 和 FIM2010R2，连接器是可从[Microsoft 下载中心](http://go.microsoft.com/fwlink/?LinkId=717495)下载。

在操作此连接器，请参阅[通用 SQL 连接符分步](active-directory-aadconnectsync-connector-genericsql-step-by-step.md)文章。

## <a name="overview-of-the-generic-sql-connector"></a>一般的 SQL 接口的概述

一般的 SQL 接口，可提供 ODBC 连接数据库系统相结合的同步服务。  

从高层次的角度来看，通过当前版本的连接器支持以下功能︰

功能 | 支持
--- | ---
连接的数据源 | 连接器与所有 64 位 ODBC 驱动程序支持。 它经过如下︰ <li>Microsoft SQL Server 和 SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>10 和 11 的 oracle g</li><li>MySQL 5.x</li>
方案   | <li>对象生命周期管理</li><li>密码管理</li>
操作 | <li>完全导入和增量导入、 导出</li><li>导出︰ 添加，删除，更新和替换</li><li>设置密码，则更改密码</li>
架构 | <li>动态发现的对象和属性</li>

### <a name="prerequisites"></a>系统必备组件
使用连接器之前，请确保您具有同步服务器上的以下︰

- Microsoft.NET 4.5.2 框架或更高版本
- 64 位 ODBC 客户端驱动程序

### <a name="permissions-in-connected-data-source"></a>连接的数据源中的权限
若要创建或执行任何支持的任务在一般的 SQL 接口，您必须︰

- db_datareader
- 诸如

### <a name="ports-and-protocols"></a>端口和协议
ODBC 驱动程序正常工作所需的端口，请查阅数据库供应商的文档。

## <a name="create-a-new-connector"></a>创建一个新的连接器
若要创建通用 SQL 连接器，**同步服务**中选择**管理代理**和**创建**。 选择**通用 SQL (Microsoft)**连接器。

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>连接
连接器用于连接使用 ODBC DSN 文件。 创建 DSN 文件使用**ODBC 数据源****管理工具**下的 「 开始 」 菜单中找到。 在管理工具，因此它可以提供的接口创建**文件 DSN** 。

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

当您创建一个新的通用 SQL 连接器，连接屏幕的第一个。 首先，您需要提供以下信息︰

- DSN 文件路径
- 身份验证
    - 用户名称
    - 密码

数据库应支持这些身份验证方法之一︰

- **Windows 身份验证**︰ 验证数据库使用 Windows 凭据来验证用户。 用户名/密码指定用于与数据库进行身份验证。 此帐户必须对数据库的权限。
- **SQL 身份验证**︰ 身份验证的用户名/密码定义一个连接屏幕连接到数据库的数据库使用。 如果 DSN 文件中存储用户名称/密码，在连接屏幕上提供的凭据具有优先权。
- **SQL azure 数据库身份验证**︰ 有关详细信息，请参阅[连接到 SQL 数据库通过使用 Azure 活动目录的身份验证](..\sql-database\sql-database-aad-authentication.md)。

**DN 是定位点**︰ 如果您选择此选项，DN 也被用作的定位属性。 它可用于简单的实现，但存在以下局限性︰

-   连接器支持只有一个对象类型。 因此任何引用特性只能引用同一个对象类型。

**导出类型︰ 对象替换**︰ 导出，期间只有某些属性已更改时，整个对象的所有属性将导出并将取代现有的对象。

### <a name="schema-1-detect-object-types"></a>模式 1 （检测对象类型）
在此页上，您将配置连接器将如何在数据库中找到不同的对象类型。

每个对象类型显示为一个分区，并且配置进一步**配置分区**和层次结构。

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**对象类型检测方法**︰ 连接器支持这些对象类型检测方法。

- **固定值**︰ 提供的对象类型列表以逗号分隔的列表。 例如︰ `User,Group,Department`。  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **表/视图/存储过程**︰ 提供表/视图/存储过程的名称，然后提供的对象类型列表中的列名称。 如果您使用存储的过程，然后还参数为其提供的格式**[名称]: [方向]: [值]**。 在单独的行 （使用 Ctrl + Enter 才能获取新行） 上提供的每个参数。  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL 查询**︰ 此选项使您可以提供一个 SQL 查询，返回单个列的对象类型，例如`SELECT [Column Name] FROM TABLENAME`。 返回的列必须为字符串类型 (varchar)。

### <a name="schema-2-detect-attribute-types"></a>模式 2 （检测特性类型）
在此页上，您要配置的属性名称和类型将如何被检测到。 对于检测到的前一页上每个对象类型列出了配置选项。

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**属性类型检测方法**︰ 连接器支持架构 1 屏幕中的每个检测到的对象类型与这些属性类型检测方法。

- **表/视图/存储过程**︰ 提供用于查找的属性名称的表/视图/存储过程的名称。 如果您使用存储的过程，然后还参数为其提供的格式**[名称]: [方向]: [值]**。 在单独的行 （使用 Ctrl + Enter 才能获取新行） 上提供的每个参数。 若要检测中多值属性的属性名称，提供以逗号分隔的表或视图列表。 不支持多值的情况当父表和子表具有相同的列名称。
- **SQL 查询**︰ 此选项使您可以提供一个 SQL 查询，返回单个列属性的名称，例如`SELECT [Column Name] FROM TABLENAME`。 返回的列必须为字符串类型 (varchar)。

### <a name="schema-3-define-anchor-and-dn"></a>架构 （定义锚和 DN） 3
此页允许您配置定位和 DN 属性为每个检测到的对象类型。 您可以选择多个特性以使唯一的锚点。

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- 多值或布尔型属性没有列出。
- 相同的属性不能使用 DN 和锚定，除非**DN 定位点**选择连接页上。
- **DN 定位点**选择连接页上，如果此网页需要只有 DN 属性。 此属性还会用作定位特性。
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>模式 4 （定义的属性类型、 参考和方向）
此页允许您配置的属性类型，例如整数、 二进制文件中，或布尔值和方向的每个属性。 列出了包括多值的属性的所有属性页**架构 2** 。

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **数据类型**︰ 用于映射到同步引擎知道这些类型的属性类型。 默认情况下，SQL 架构中检测到使用相同的类型，但日期时间和引用都不是很容易地检测。 对于，您需要指定**日期时间**或**引用**。
- **方向**︰ 您可以将特性方向设置为导入、 导出或 ImportExport。 ImportExport 是默认设置。
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

注释︰

- 如果属性类型不是可检测到连接器，则使用字符串数据类型。
- **嵌套表**可以被视为一个列的数据库表。 Oracle 存储嵌套表中的行顺序不分先后。 但是，当到 PL/SQL 变量中检索嵌套的表，行给出连续下标从 1 开始的。 这样您的类似数组的访问单个行。
- 连接器不支持**VARRYS** 。

### <a name="schema-5-define-partition-for-reference-attributes"></a>架构 （定义分区的引用属性） 5
在此页上，您为配置 (对象类型) 的分区特性所指的所有参考属性。

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

如果您使用**DN 是定位点**，则必须使用相同的对象类型为您从引用的一个。 您不能引用另一个对象类型。

### <a name="global-parameters"></a>全局参数
全局参数页用于配置增量导入、 日期/时间格式和密码方法。

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

一般的 SQL 接口支持增量导入下列方法︰

- **触发器**︰ 请参阅[生成增量视图使用触发器](https://technet.microsoft.com/library/cc708665.aspx)。
- **水印**︰ 可以使用任何数据库的一般方法。 水印查询是预先填充基于数据库供应商联系。 水位线列必须存在于使用每个表/视图。 此列必须跟踪插入和更新对作为表和其相关 (多值或子) 表。 同步服务和数据库服务器之间的时钟必须同步。 如果没有，增量导入中的某些条目可能被忽略。  
限制︰
    - 水印策略不支持删除对象。
- **快照**: （适用于 Microsoft SQL Server 只）[使用快照生成增量视图](https://technet.microsoft.com/library/cc720640.aspx)
- **修订**: （适用于 Microsoft SQL Server 只）[有关修订](https://msdn.microsoft.com/library/bb933875.aspx)  
限制︰
    - 锚点和 DN 属性必须为表中所选对象的主键的一部分。
    - 在导入和导出与更改跟踪过程中不支持 SQL 查询。

**附加参数**︰ 指定数据库服务器所在的时区，指示数据库服务器所在的位置。 此值用于支持各种格式的日期和时间属性。

该连接器始终存储日期和日期时间以 UTC 格式。 能够正确转换日期和时间、 数据库服务器和所使用的格式的时区，必须指定。 应以.Net 格式表示格式。

在导出过程中每个日期时间属性，必须提供到 UTC 时间格式中的连接器。

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**密码配置**︰ 连接器提供了密码同步功能并支持设置和更改密码。

接口提供了两种方法来支持密码同步︰

- **存储过程**︰ 此方法要求两个存储的过程以支持设置和更改密码。 键入添加的所有参数，然后更改**设置密码 SP**和**更改密码 SP**参数分别为每个下面示例中的密码操作。
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **密码扩展**︰ 此方法要求密码扩展 DLL （您需要提供实现[IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx)接口的扩展 DLL 名称）。 密码扩展程序集必须放在扩展文件夹中，以便连接器可加载运行时 DLL。
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

您还必须启用**扩展配置**页上的密码管理。
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>配置分区和层次结构
在分区和层次结构页上选择所有的对象类型。 每个对象类型是它自己的分区。

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

您还可以重写在**连接**或**全局参数**页上定义的值。

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>配置标记
此页是只读的因为已定义了该锚点。 选定的定位属性始终附加对象类型，以确保在对象类型之间保持唯一。

![定位点](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>配置运行的步骤参数
在连接器上的运行配置文件上配置下列步骤。 这些配置执行导入和导出数据的实际的工作。

### <a name="full-and-delta-import"></a>完整和增量导入
一般的 SQL 接口支持完全和增量导入使用这些方法︰

- 表
- 视图
- 存储的过程
- SQL 查询

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**表/视图**  
要导入的对象的多值的属性，您必须提供与父表的**名称的 Multi-Valued 表/视图**中的以逗号分隔的表/视图名称和相应的联接条件**联接条件**中。

示例︰ 您想要导入的员工对象及其全部多值的属性。 有两个表，名为雇员 （主表） 和部门 （多值）。
请执行以下操作︰

- 类型**雇员****表/视图/SP**中。
- 键入部门**名称的 Multi-Valued 表/视图**中。
- 键入员工和部门之间的联接条件**联接条件**时，例如`Employee.DEPTID=Department.DepartmentID`。
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**存储的过程**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- 如果您有很多数据，建议实现分页存储过程使用。
- 有关支持分页您存储过程，您需要提供启动索引和结束索引。 请参见︰[有效地通过大量的数据进行分页](https://msdn.microsoft.com/library/bb445504.aspx)。
- @StartIndex和@EndIndex在执行时替换**配置步骤**页面上配置相应的页面大小值。 例如，连接器时检索第一页和页大小设置 500，在这种情况下@StartIndex将 1 和@EndIndex500。 这些值增加时接头检索后续页面和更改@StartIndex&@EndIndex值。
- 若要执行参数化存储过程，提供中的参数`[Name]:[Direction]:[Value]`格式。 在单独的行 （使用 Ctrl + enter 键来获取新行） 中输入每个参数。
- 一般的 SQL 接口在 Microsoft SQL Server 还支持从链接服务器的导入操作。 如果应从链接服务器中的表中检索信息，表格应提供格式︰`[ServerName].[Database].[Schema].[TableName]`
- 一般的 SQL 接口支持那些具有类似的结构 (别名名称和数据类型) 之间运行架构和信息检测的步骤的对象。 如果所选的对象从架构和在运行步骤提供的信息不同，SQL 连接器不能支持这种类型的方案。

**SQL 查询**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- 多个结果集不支持的查询。
- SQL 查询支持分页并提供起始索引和结束索引作为变量来支持分页。

### <a name="delta-import"></a>增量导入
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

增量导入配置需要一些更多的配置完全导入与比较。

- 如果您选择的触发器或快照的方法来跟踪增量更改，然后提供**历史记录表或快照数据库名称**框中的历史记录表或快照数据库。
- 您还需要提供历史记录表和父表之间的联接条件，例如`Employee.ID=History.EmployeeID`
- 要从历史记录表的父表跟踪交易记录，必须提供包含操作信息 （添加、 更新或删除） 的列名称。
- 如果您选择水印跟踪增量更改，然后提供包含**水标记列名称**中的操作信息的列名称。
- **更改类型属性**列是必需的更改类型。 此列将映射到增量视图中更改类型多值表的主表中发生的更改。 此列可以包含 Modify_Attribute 更改类型的属性级别更改或添加，修改，或者删除更改的对象级别的更改类型的类型。 如果这是一个不同于默认值添加、 修改或删除，则您可以定义这些值使用此选项。

### <a name="export"></a>导出
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

通用 SQL 连接器支持导出使用四个受支持的方法，如︰

- 表
- 视图
- 存储的过程
- SQL 查询

**表/视图**  
如果您选择表中的视图选项，该连接器将生成各自的查询，以执行导出。

**存储的过程**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

如果您选择存储过程选项，则导出要求三个不同的存储过程执行插入、 更新或删除操作。

- **添加 SP 名称**︰ 此 SP 运行如果任何对象进入各自的表中插入连接器。
- **更新 SP 名称**︰ 此 SP 运行如果任何对象进入更新各自的表中的连接器。
- **删除 SP 名称**︰ 如果任何对象进入各自的表中删除的连接器，将运行此 SP。
- 选择用作参数值的存储过程的架构中的属性。 例如， `EmployeeName: INPUT: @EmployeeName` （EmployeeName 选择连接器架构中和连接器执行导出时替换相应的值）
- 若要运行参数化存储的过程，提供了在参数`[Name]:[Direction]:[Value]`格式。 在单独的行 （使用 Ctrl + enter 键来获取新行） 中输入每个参数。

**SQL 查询**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

如果您选择 SQL 查询选项，则导出需要三个不同的查询执行插入、 更新或删除操作。

- **插入查询**︰ 如果任何对象进入各自的表中插入连接器运行此查询。
- **更新查询**︰ 该查询运行如果任何对象进入更新各自的表中的连接器。
- **删除查询**︰ 该查询运行如果任何对象进入各自的表中删除的连接器。
- 从架构使用作为参数值来查询，例如选择属性`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>故障排除

-   有关如何启用日志记录来解决连接器的信息，请参阅[如何启用 ETW 跟踪连接器](http://go.microsoft.com/fwlink/?LinkId=335731)。
