<properties
   pageTitle="在 Microsoft Azure 应用程序服务使用 Informix 连接器 |Microsoft Azure"
   description="如何使用 Informix 连接器与逻辑应用触发器和操作"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="informix-connector"></a>Informix 连接器
>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。

Informix 的 Microsoft 连接器是 API 的应用程序通过 Azure 应用程序服务的应用程序连接到 IBM Informix 数据库中存储的资源。 连接器包括 Microsoft 客户端通过 TCP/IP 网络连接，包括 Azure 混合连接到内部 Informix 服务器使用 Azure 服务总线中继到 Informix 服务器的远程计算机连接。 接口支持以下数据库操作︰

- 使用读取行，请选择
- 轮询读取行使用跟选择的选择计数
- 添加包含一行或多个 （大容量） 行使用插入
- 更改包含一行或多个 （大容量） 行使用更新
- 删除一行或多个 （大容量） 行使用删除
- 读来改变使用选择光标跟更新位置当前的游标行
- 读来移除使用选择光标跟更新位置当前的游标行
- 运行过程的输入和输出参数、 返回值，使用调用的结果集
- 自定义命令和复合使用选择，插入、 更新和删除的操作

## <a name="triggers-and-actions"></a>触发器和操作
接口支持以下逻辑应用触发器和操作︰

触发器 | 操作
--- | ---
<ul><li>轮询数据</li></ul> | <ul><li>大容量插入</li><li>插入</li><li>批量更新</li><li>更新</li><li>调用</li><li>批量删除</li><li>删除</li><li>选择</li><li>条件的更新</li><li>EntitySet 的帖子</li><li>删除条件</li><li>选取一个图元</li><li>删除</li><li>对 EntitySet Upsert</li><li>自定义命令</li><li>复合操作</li></ul>


## <a name="create-the-informix-connector"></a>创建 Informix 连接器
您可以在下面的示例类似定义连接器内的逻辑应用程序或者从 Azure 的市场上:  

1. 在 Azure 的 startboard 中，选择**市场**。
2. **所有**刀片式服务器中，在**搜索内容**框中，键入**informix** ，然后单击 enter 键。
3. 一切都在搜索结果窗格中，选择**Informix 连接器**。
4. 在 Informix 连接器说明刀片式服务器，选择**创建**。
5. 在 Informix 连接器包刀片式服务器，请输入名称 (例如"InformixConnectorNewOrders")、 应用程序服务计划，以及其他属性。
6. 选择**包设置**，然后输入以下程序包设置。

    名称 | 必填 |  说明
--- | --- | ---
连接字符串 | 是的 | Informix 客户端连接字符串 (如"网络地址 = 服务器名称;网络端口 = 9089;用户 ID = 用户名;密码 = 密码; Initial Catalog = nwind; 默认架构 = informix")。
表 | 是的 | 以逗号分隔的表、 视图和别名的名称和所需的 OData 操作生成示例 (例如"NEWORDERS") 的 swagger 文档的列表。
过程 | 是的 | 过程和函数名称 (如"SPORDERID") 的逗号分隔列表。
OnPremise | 不 | 部署内部使用 Azure 服务总线中继。
ServiceBusConnectionString | 不 | Azure 服务总线中继连接字符串。
PollToCheckData | 不 | 选择计数语句用于触发器逻辑应用程序 (如"选择计数 (\*) 从 NEWORDERS 位置发货日期 IS NULL")。
PollToReadData | 不 | 使用逻辑应用程序触发的 SELECT 语句 (如"选择\*NEWORDERS 其中发货日期是 NULL FOR 更新从")。
PollToAlterData | 不 | 更新或删除语句，用于逻辑应用程序触发 (例如"更新 NEWORDERS 设置发货日期 = 当前日期在当前的&lt;光标&gt;")。

7. 选择**确定**，然后选择**创建**。
8. 完成后，包设置与以下类似︰  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>使用 Informix 连接器操作若要添加数据的逻辑应用程序 ##
您可以定义一个逻辑应用程序操作，以便将数据添加到实体 OData 操作使用 API 插入或开机自检 Informix 表。 例如，可以插入新的客户订单记录，处理对表定义为标识列，插入 SQL 语句返回的标识值或逻辑应用程序向受影响的行 (从最终表选择不到，或者 (插入到 NEWORDERS （CUSTID、 货主名称、 SHIPADDR、 货主国家、 SHIPREG、 SHIPZIP） 值 (？，？，？，？，？，？)))。

> [AZURE.TIP] Informix 连接"*EntitySet 的帖子*"返回的标识列值，"*API 插入*"返回受影响的行

1. 在 Azure startboard，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2. 输入的名称 (例如"NewOrdersInformix")、 应用程序服务计划、 其他属性，然后选择**创建**。
3. 在 Azure startboard，选择逻辑刚刚创建的应用程序、**设置**，然后**触发器和操作**。
4. 在触发器和操作刀片式服务器，逻辑应用程序模板中选择**从头开始创建**。
5. 在 API 的应用程序面板中，选择**重复周期**、 频率和时间间隔，然后设置**复选标记**。
6. 在 API 应用程序面板中，选择**Informix 连接器**，展开操作列表中选择**插入 NEWORDER**。
7. 展开参数列表，可以输入下面的值︰  

    名称 | 值
--- | --- 
CUSTID | 10042
输入 | 10000
货主名称 | 惰性的 K Kountry 存储 
SHIPADDR | 12 乐团 Terrace
货主国家 | 沃拉沃拉 
SHIPREG | 华盛顿州
SHIPZIP | 99362 

8. 选择保存的操作设置，然后**保存****复选标记**。
9. 这些设置应如下所示︰  
![][3]  
10. 在**所有运行**的列表，在**操作**下，选择第一列项目 （最新运行）。 
11. 在**逻辑应用程序运行**刀片式服务器，请选择**操作**项**informixconnectorneworders**。
12. 在**逻辑应用程序操作**刀片式服务器，选择**输入链接**。 Informix 连接器使用输入的参数化的 INSERT 语句处理。
13. 在**逻辑应用程序操作**刀片式服务器，选择**输出链接**。 输入应如下所示︰  
![][4]

#### <a name="what-you-need-to-know"></a>您需要知道

- 连接器截断形成逻辑应用程序操作名称时，Informix 的表名。 例如，**将 NEWORDERS 插入**操作将被截断为**NEWORDER 中插入**。
- 保存后**触发器和操作**的逻辑应用程序，应用程序逻辑处理操作。 可能有一个延迟之前的秒数 （例如 3-5 秒） 逻辑的应用程序处理操作。 或者，您可以单击**立即运行**处理操作。
- Informix 连接器定义 EntitySet 成员属性，包括是否该成员对应于一个带有默认值的 Informix 列或生成的列 （如标识）。 逻辑应用程序显示 EntitySet 成员 ID 名称，来表示需要值的 Informix 列旁边的红色星号。 不应为不到，或者成员，它对应于 Informix 标识列中输入一个值。 可能在其他可选成员项目、 ORDDATE、 REQDATE、 输入、 运费 （SHIPCTRY），分别对应于 Informix 具有默认值的列中输入值。 
- Informix 连接器返回到逻辑的应用程序响应上发布到 EntitySet 派生 DRDA SQLDARD （SQL 数据区域答复数据） 上准备好的 SQL 插入语句包含标识列的值。 Informix 服务器不会返回列的插入的值使用默认值。  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>使用 Informix 连接器操作将数据批量添加逻辑应用程序 ##
您可以定义一个逻辑应用程序操作，以便将数据添加到使用 API 大容量插入操作 Informix 表。 例如，您可以插入两个新的客户订单记录，通过使用数组的行值对表定义为标识列，插入 SQL 语句处理返回逻辑应用程序向受影响的行 (从最终表选择不到，或者 (插入到 NEWORDERS （CUSTID、 货主名称、 SHIPADDR、 货主国家、 SHIPREG、 SHIPZIP） 值 (？，？，？，？，？，？)))。

1. 在 Azure startboard，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2. 输入的名称 (例如"NewOrdersBulkInformix")、 应用程序服务计划、 其他属性，然后选择**创建**。
3. 在 Azure startboard，选择逻辑刚刚创建的应用程序、**设置**，然后**触发器和操作**。
4. 在触发器和操作刀片式服务器，逻辑应用程序模板中选择**从头开始创建**。
5. 在 API 的应用程序面板中，选择**重复周期**、 频率和时间间隔，然后设置**复选标记**。
6. 在 API 的应用程序面板中，选择**Informix 连接器**，展开操作列表选择**批量插入新建**。
7. 以数组形式输入的**行**值。 例如，复制并粘贴以下项︰  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
    ```
        
8. 选择保存的操作设置，然后**保存****复选标记**。 这些设置应如下所示︰  
![][6]

9. 在**所有运行**的列表，在**操作**下，单击第一个列出的项目 （最新运行）。
10. 在**逻辑应用程序运行**刀片式服务器，单击**待办**事项。
11. 在**逻辑应用程序操作**刀片式服务器，请单击**输入链接**。 输出应如下所示︰  
[][7]
12. 在**逻辑应用程序操作**刀片式服务器，请单击**输出链接**。 输出应如下所示︰  
![][8]

#### <a name="what-you-need-to-know"></a>您需要知道

- 连接器截断形成逻辑应用程序操作名称时，Informix 的表名。 例如，**批量插入 NEWORDERS**的操作将被截断为**批量插入新建**。
- Informix 数据库可以是区分大小写的表名和列名。 例如，大容量插入操作数组列名称可能需要用小写字母 ("custid")，而不是大小写 ("CUSTID")。
- 通过省略标识列 （例如不到，或者）、 为空的列 （如发货日期），并使用默认值 （如 ORDDATE、 REQDATE、 输入、 运费、 SHIPCTRY） 的列，Informix 数据库生成的值。
- 通过指定"今天"和"明天"，Informix 连接器生成"当前日期"和"当前日期 + 1 天"的功能 (例如 REQDATE)。 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>逻辑应用程序使用 Informix 连接器触发器来读取、 更改或删除数据 ##
您可以定义逻辑应用触发器可以轮询并使用 API 轮询数据复合运算 Informix 表中读取的数据。 例如，您可以读取一个或多个新的客户订单记录，逻辑应用程序返回的记录。 Informix 连接包/应用程序设置应如下所示︰

    应用程序设置 | 值
--- | --- | ---
PollToCheckData | 选择计数 (\*) 从 NEWORDERS 发货日期为
PollToReadData | 选择\*NEWORDERS 其中发货日期更新为从
PollToAlterData | <no value specified>


此外，您可以定义逻辑应用触发器轮询、 阅读和修改使用 API 轮询数据复合运算 Informix 表中的数据。 例如，您可以读取一个或多个新客户订单记录更新到逻辑应用程序返回 （更新） 之前所选的记录的行值。 Informix 连接包/应用程序设置应如下所示︰

    应用程序设置 | 值
--- | --- | ---
PollToCheckData | 选择计数 (\*) 从 NEWORDERS 发货日期为
PollToReadData | 选择\*NEWORDERS 其中发货日期更新为从
PollToAlterData | 更新 NEWORDERS 集发货日期 = 当前日期在当前的&lt;光标&gt;


此外，您可以定义逻辑应用触发器轮询、 阅读和使用 API 轮询数据复合运算 Informix 表中删除数据。 例如，您可以读取一个或更多的新客户订单记录，删除行，返回到逻辑应用程序 （前删除） 选定的记录。 Informix 连接包/应用程序设置应如下所示︰

    应用程序设置 | 值
--- | --- | ---
PollToCheckData | 选择计数 (\*) 从 NEWORDERS 发货日期为
PollToReadData | 选择\*NEWORDERS 其中发货日期更新为从
PollToAlterData | 删除 NEWORDERS 地方当前的&lt;光标&gt;

在此示例中，逻辑的应用程序将轮询、 读取、 更新，然后重新读取 Informix 表中的数据。

1. 在 Azure startboard，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2. 输入的名称 (例如"ShipOrdersInformix")、 应用程序服务计划、 其他属性，然后选择**创建**。
3. 在 Azure startboard，选择逻辑刚刚创建的应用程序、**设置**，然后**触发器和操作**。
4. 在触发器和操作刀片式服务器，逻辑应用程序模板中选择**从头开始创建**。
5. 在 API 的应用程序面板中，选择**Informix 连接器**，频率和时间间隔，然后设置**复选标记**。
6. 在 API 的应用程序面板中，选择**Informix 连接器**，展开操作列表中选择**从 NEWORDERS 中选择**。
7. 选择保存的操作设置，然后**保存****复选标记**。 这些设置应如下所示︰  
![][10]
8. 单击以关闭**触发器和操作**刀片式服务器，然后单击以关闭刀片式服务器**的设置**。
9. 在**所有运行**的列表，在**操作**下，单击第一个列出的项目 （最新运行）。
10. 在**逻辑应用程序运行**刀片式服务器，单击**待办**事项。
11. 在**逻辑应用程序操作**刀片式服务器，请单击**输出链接**。 输出应如下所示︰  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>逻辑应用程序使用 Informix 连接器操作来删除数据 ##
您可以定义一个逻辑应用程序操作，以便从实体 OData 操作中使用的 API 删除或开机自检 Informix 表中删除数据。 例如，可以插入新的客户订单记录，处理对表定义为标识列，插入 SQL 语句返回的标识值或逻辑应用程序向受影响的行 (从最终表选择不到，或者 (插入到 NEWORDERS （CUSTID、 货主名称、 SHIPADDR、 货主国家、 SHIPREG、 SHIPZIP） 值 (？，？，？，？，？，？)))。

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>创建逻辑应用程序使用 Informix 连接器来删除数据 ##
您可以创建新的逻辑应用程序从内 Azure 的市场上，并再为操作使用 Informix 连接器删除客户订单。 例如，可以使用 Informix 连接器条件删除操作的删除 SQL 语句处理 (删除从 NEWORDERS，不到，或者 > = 10000)。

1. 在 Azure**开始**板集线器菜单中，单击**+**（加号），单击**Web + 移动**，然后单击**逻辑的应用程序**。 
2. 在**创建逻辑应用程序**刀片式服务器，键入**名称**，例如**RemoveOrdersInformix**。
3. 选择或定义其他设置 （如服务计划、 资源组） 的值。
4. 这些设置应如下所示。 单击**创建**︰  
![][12]
5. 在**设置**刀片式服务器，请单击**触发器和操作**。
6. 在**触发器和操作**刀片式服务器**逻辑应用程序模板**列表中，单击**创建从零开始**。
7. 在**触发器和操作**刀片式服务器**API 的应用程序**面板中的资源组中，单击**重复周期**。
8. 逻辑应用程序设计图面上, 单击**重复**项，设置**频率**和**时间间隔**，例如**天**和**1**，，然后单击保存重复项设置**复选标记**。
9. **触发器和操作**刀片式服务器中的资源组中， **API 应用程序**面板中单击**Informix 连接器**。
10. 逻辑应用程序设计图面上单击**Informix 连接器**操作项，单击省略号 （**...**） 展开操作列表中，，然后单击**条件删除 N**。
11. 在 Informix 连接器操作项，键入**不到，或者 ge 10000** **表达式，用于标识条目的子集**。
12. **复选标记**保存操作设置，请单击，然后单击**保存**。 这些设置应如下所示︰  
![][13]
13. 单击以关闭**触发器和操作**刀片式服务器，然后单击以关闭刀片式服务器**的设置**。
14. 在**所有运行**的列表，在**操作**下，单击第一个列出的项目 （最新运行）。
15. 在**逻辑应用程序运行**刀片式服务器，单击**待办**事项。
16. 在**逻辑应用程序操作**刀片式服务器，请单击**输出链接**。 输出应如下所示︰  
![][14]

**注意︰**逻辑应用程序设计器将截断表名。 例如**从 NEWORDERS 中删除条件**的操作被截断为**从 N 删除条件**。


> [AZURE.TIP] 使用以下 SQL 语句创建示例表和存储的过程。 

您可以创建使用以下 Informix SQL DDL 语句的示例 NEWORDERS 表︰
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


您可以创建使用以下 Informix DDL 语句 SPORDERID 存储过程示例︰
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>混合配置 （可选）

> [AZURE.NOTE] 此步骤是必需的只有当您使用的 DB2 连接器内部防火墙的后面。

应用程序服务使用混合配置管理器安全地连接到您的内部系统。 如果路由组连接器使用内部 IBM DB2 服务器的 Windows，混合连接管理器是必需的。

请参阅[使用混合连接管理器](app-service-logic-hybrid-connection-manager.md)。


## <a name="do-more-with-your-connector"></a>用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

创建 API 应用程序使用 REST Api。 请参阅[连接器和应用程序的 API 参考](http://go.microsoft.com/fwlink/p/?LinkId=529766)。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


