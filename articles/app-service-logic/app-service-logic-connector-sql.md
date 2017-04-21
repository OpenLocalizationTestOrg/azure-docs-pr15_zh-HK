<properties
   pageTitle="在应用程序逻辑中使用 SQL 接口 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 SQL 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>开始使用 Microsoft SQL 连接器并将其添加到您的逻辑应用程序
>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。 Azure SQL 2015-08-01-预览架构版本，请单击[SQL Azure API](../connectors/connectors-create-api-sqlazure.md)。

连接到内部部署 SQL Server 或 Azure 的 SQL 数据库，创建和更改您的信息或数据。 连接器可以使用逻辑应用程序在检索过程中，或将数据推为"工作流"的一部分。 在工作流中使用 SQL 连接器时，就可以获得各种方案。 例如，您可以︰

- 公开驻留在您使用 web 或移动应用程序的 SQL 数据库中的数据一节。
- 存储在 SQL 数据库表中插入数据。 例如，您可以输入员工记录，更新销售订单，等等。
- 从 SQL 中获取数据，并在业务流程中使用它。 例如，可以获取客户记录和这些客户记录置于队伍。

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 SQL 接口。 

## <a name="triggers-and-actions"></a>触发器和操作
*触发器*是发生的事件。 例如，在更新订单时或当新客户添加。 *操作*是触发器的结果。 例如，在更新订单时，将通知发送给销售人员。 或者，当添加一个新客户，新客户发送欢迎电子邮件。

SQL 连接器可以用作触发器或 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 为您的程序包中包含的每个表设置 （详细介绍本主题中后面），还有一套 JSON 动作和一套 XML 操作。

SQL 接口具有以下触发器和操作可用︰

触发器 | 操作
--- | ---
轮询数据 | <ul><li>插入到表中</li><li>更新表</li><li>从表中选择</li><li>从表中删除</li><li>调用存储的过程</li></ul>

## <a name="create-the-sql-connector"></a>创建 SQL 连接器

连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "SQL 接口"中搜索，请选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性。
4. 输入以下程序包设置︰

    名称 | 必填 |  说明
--- | --- | ---
服务器名称 | 是的 | 输入 SQL Server 名称。 例如，输入*为 sqlexpress* SQLserver/或*SQLserver.mydomain.com*。
端口 | 不 | 默认值为 1433年。
用户名称 | 是的 | 输入 SQL Server 可以登录的用户名。 如果连接到内部部署 SQL Server，请输入 SQL 身份验证凭据。
密码 | 是的 | 输入的用户名和密码。
数据库名称 | 是的 | 输入要连接的数据库。 例如，您可以输入*客户*或*dbo 中的订单*。
内部部署 | 是的 | 默认值为 False。 如果连接到 SQL Azure 数据库，输入 False。 如果连接到内部部署 SQL Server，则输入 True。
服务总线连接字符串 | 不 | 如果您要连接到内部，请输入服务总线中继连接字符串。<br/><br/>[使用混合连接管理器](app-service-logic-hybrid-connection-manager.md)<br/>[服务总线定价](https://azure.microsoft.com/pricing/details/service-bus/)
合作伙伴服务器的名称 | 不 | 如果主服务器不可用，您可以输入伙伴服务器作为备用或备份服务器。
表 | 不 | 列出可以通过连接器进行更新的数据库表。 例如，输入*OrdersTable*或*EmployeeTable*。 如果输入没有表，可以使用的所有表。 使用此连接器为一个操作需要有效的表和/或存储过程。
存储的过程 | 不 | 请输入现有的存储的过程可以调用的连接器。 例如，输入*sp_IsEmployeeEligible*或*sp_CalculateOrderDiscount*。 使用此连接器为一个操作需要有效的表和/或存储过程。
可用数据的查询 | 有关触发器支持 | 若要确定数据是否可用于轮询一个 SQL Server 数据库表的 SQL 语句。 这应返回一个数字值，表示可用的数据的行数。 示例︰ 从 table_name 选择 COUNT(*)。
轮询数据的查询 | 有关触发器支持 | 要轮询的 SQL Server 数据库表的 SQL 语句。 您可以输入任意数量的 SQL 语句以分号分隔。 此语句是以事务方式执行，才提交时的数据安全地存储在您的逻辑的应用程序。 示例︰ 选择*从 table_name;删除从 table_name。<br/>**Note**<br/>您必须提供一个轮询语句，可通过删除、 移动或更新所选的数据，以确保相同的数据不会再次轮询避免无限循环。

5. 完成后，包设置与以下类似︰  
![][1]  

6. 选择**创建**。 


## <a name="use-the-connector-as-a-trigger"></a>为触发器中使用的连接器
让我们看一个简单的逻辑应用程序轮询 SQL 表中的数据，在另一个表中添加数据并更新数据。

要用作触发器的 SQL 接口，输入**数据使用查询**和**轮询数据查询**的值。 **数据可用的查询**执行的计划输入并确定是否有任何数据。 由于此查询只返回一个标量数字，也可以调整和优化为频繁执行。

当数据可用查询显示数据可用时，只执行**轮询数据查询**。 此语句在一个事务内执行，并且只能提交时提取的数据持久存储在您的工作流。 请务必避免无限重新提取相同的数据。 这种执行的事务性质可用于删除或更新的数据，以确保它不会收集下一次查询数据。

> [AZURE.NOTE] 此语句所返回的架构标识您的连接器中可用的属性。 所有列必须具有都名称。

#### <a name="data-available-query-example"></a>数据可用的查询示例

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>轮询数据查询示例

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>添加触发器
1. 在创建或编辑逻辑应用程序，选择作为触发器创建 SQL 接口。 这将列出可用触发器︰**轮询数据 (JSON)**和**轮询数据 (XML)**:  
![][5]

2. 选择**轮询数据 (JSON)**触发器，输入频率，然后单击 ✓︰  
![][6]

3. 触发器现在显示在应用程序中的逻辑配置。 触发器的输出显示，并且可以用作任何后续操作中输入︰  
![][7]

## <a name="use-the-connector-as-an-action"></a>为操作中使用的连接器
使用轮询数据从一个 SQL 表中，我们简单的逻辑应用程序方案中另一个表中添加数据并更新数据。

若要为一个操作使用 SQL 接口，输入表和/或存储过程创建 SQL 连接器时输入的名称︰

1. 之后您的触发器 （或选择手动运行此逻辑），添加库中创建的 SQL 接口。 选择插入操作，如*插入到 TempEmployeeDetails (JSON)*之一︰  
![][8]

2. 输入记录被插入，并单击 ✓ 的输入的值︰  
![][9]

3. 从库，选择您创建的同一个 SQL 连接器。 作为一项操作，选择更新操作在同一个表，如*更新 EmployeeDetails*:  
![][11]

4. 请输入的值输入的更新操作，然后单击 ✓ 上︰  
![][12]

通过轮询表中添加新的记录，您可以测试逻辑应用程序。

## <a name="what-you-can-and-cannot-do"></a>您可以做什么和不能做

SQL 查询 | 支持 | 不受支持
--- | --- | ---
其中子句 | <ul><li>运算符︰ 和，或者，=，<>、 <、 < =、 >，> =，如下所示</li><li>可以通过组合多个的子条件 '（' 和'）'</li><li>字符串文本、 日期时间 （括在单引号），数字 （应仅包含数字字符）</li><li>应该严格是以二进制表达式的格式，如 ((operand operator operand) 和/或 （操作数运算符的操作数）) *</li></ul> | <ul><li>运算符︰ 之间在</li><li>所有内置功能，如 add （）、 max （） now （）、 POWER()，等等</li><li>喜欢数学运算符 *、-，+ 等</li><li>使用字符串串联 +。</li><li>所有联接</li><li>为空且不为空</li><li>任何数字与非数字的字符，例如十六进制数字</li></ul>
字段 （表中选择的查询） | <ul><li>以逗号分隔的有效的列名称。 无表名称前缀允许 （一次一个表上的连接器有效）。</li><li>可以使用转义名称 [和]</li></ul> | <ul><li>顶部、 互异，等等类似的关键字</li><li>如街道 + 城市 + Zip AS 地址的别名，</li><li>所有内置的函数，如 add （）、 max （） now （）、 POWER()，等等</li><li>数学运算符，如下所示 *，-，+ 等</li><li>使用字符串串联 +</li></ul>

#### <a name="tips"></a>提示

- 对于高级的查询，我们建议您创建一个存储的过程并使用 API 执行存储过程的执行。
- 当使用内部的查询，请在存储过程中使用它们。
- 对于加入多个条件，您可以使用 AND 和 OR 运算符。

## <a name="hybrid-configuration-optional"></a>混合配置 （可选）

> [AZURE.NOTE] 此步骤是必需的只有当您使用的 SQL Server 部署在防火墙后面。

应用程序服务使用混合配置管理器安全地连接到您的内部系统。 如果连接器使用内部部署 SQL Server，混合连接管理器是必需的。

> [AZURE.NOTE] 如果您想要开始使用 Azure 逻辑应用程序在 Azure 帐户注册之前，转到[重试逻辑应用程序](https://tryappservice.azure.com/?appservice=logic)，立即可以在此创建短期的初学者的逻辑应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

请参阅[使用混合连接管理器](app-service-logic-hybrid-connection-manager.md)。


## <a name="do-more-with-your-connector"></a>用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
