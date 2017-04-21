<properties
    pageTitle="逻辑应用程序中添加 DB2 连接器 |Microsoft Azure"
    description="使用 REST API 的参数 DB2 连接器的概述"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>开始使用 DB2 连接器
DB2 的 Microsoft 连接器连接到 IBM DB2 数据库中存储的资源的逻辑应用程序。 此连接器包括 Microsoft 客户端与远程 DB2 服务器计算机通过 TCP/IP 网络进行通信。 这包括云数据库，IBM Bluemix dashDB 或 Windows Azure 的虚拟化，以运行的 IBM DB2 和本地数据库使用内部数据网关。 请参阅[受支持列表](connectors-create-api-db2.md#supported-db2-platforms-and-versions)的 IBM DB2 平台和版本 （在此主题）。

>[AZURE.NOTE] 此版本的文章将应用于逻辑应用程序一般可用性 (GA)。 

DB2 连接器支持以下数据库操作︰

- 列表的数据库表
- 阅读使用选择一行
- 阅读使用选择的所有行
- 添加一个使用插入的行
- 改变一个使用更新的行
- 删除使用删除一行

本主题演示如何在逻辑的应用程序到数据库操作过程中使用的连接器。

若要了解有关应用程序逻辑的详细信息，请参阅[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="available-actions"></a>可用的操作
DB2 连接器支持下列逻辑应用程序操作︰

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>列表中的表
通过 Microsoft Azure 门户执行许多步骤包括创建一个逻辑应用程序执行任何操作。

中逻辑的应用程序，可以添加列表在 DB2 数据库中的表操作。 操作指示 DB2 架构语句处理，如连接器`CALL SYSIBM.SQLTABLES`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  输入的**名称**，如`Db2getTables`，**订购**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器，选择**编辑**，选择**频率**下拉列表可以选择**一天**，然后设置的**时间间隔**类型**7**。  
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，键入`db2`中**搜索多个操作**编辑框，然后选择**DB2-获取表 （预览）**。

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  在**DB2 的获取表**配置窗格中，选择**复选框**以启用**通过内部数据网关的连接**。 请注意，从云中更改为内部部署的设置。
    - **服务器**，请在窗体的地址或别名冒号端口号，请键入值。 例如，键入`ibmserver01:50000`。
    - 为**数据库**键入值。 例如，键入`nwind`。
    - 选择用于**身份验证**的值。 例如，选择**基本**。
    - **用户名**，请键入值。 例如，键入`db2admin`。
    - 请键入**密码**的值。 例如，键入`Password1`。
    - **网关**，请选择值。 例如，选择**datagateway01**。
7. 选择**创建**，然后选择**保存**。 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  在**Db2getTables**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
9.  在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_tables**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;这应包括的表的列表。

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>创建连接
此连接器支持连接到承载的数据库部署和在云中使用了以下连接属性。 

属性 | 说明
--- | ---
服务器 | 必填。 接受一个字符串值，表示通过 TCP/IP 端口号的 TCP/IP 地址或别名，后面的 IPv4 或 IPv6 格式 （冒号分隔）。 
数据库 | 必填。 接受一个字符串值，表示 DRDA 关系数据库名称 (RDBNAM)。 针对 z/OS 的 DB2 接受一个 16 字节的字符串 （数据库被称为 z/OS 位置 IBM DB2）。 I5/OS 的 DB2 接受 18 个字节的字符串 (数据库就所谓的 IBM DB2 i 关系数据库)。 DB2 LUW 为接受 8 个字节的字符串。
身份验证 | 可选。 接受一个列表项值，基本或 Windows (kerberos)。 
用户名 | 必填。 接受一个字符串值。 针对 z/OS 的 DB2 接受 8 个字节的字符串。 DB2 为我接受一个 10 字节的字符串。 DB2 为 Linux 或 UNIX 接受 8 个字节的字符串。 DB2 为 Windows 接受 30 个字节的字符串。
密码 | 必填。 接受一个字符串值。
网关 | 必填。 接受一个列表项值，表示内部数据网关定义逻辑应用程序到存储组中。  

## <a name="create-the-on-premises-gateway-connection"></a>创建内部部署网关连接
此连接器可以访问使用内部网关内部 DB2 数据库。 将主题网关的详细信息，请参阅。 

1. 在**网关**配置窗格中，选择**复选框**以启用**通过网关的连接**。 请注意，从云中更改为内部部署的设置。
2. **服务器**，请在窗体的地址或别名冒号端口号，请键入值。 例如，键入`ibmserver01:50000`。
3. 为**数据库**键入值。 例如，键入`nwind`。
4. 选择用于**身份验证**的值。 例如，选择**基本**。
5. **用户名**，请键入值。 例如，键入`db2admin`。
6. 请键入**密码**的值。 例如，键入`Password1`。
7. **网关**，请选择值。 例如，选择**datagateway01**。
8. 选择**创建**以继续。 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>创建云连接
此连接器可以访问云 DB2 数据库。 

1. 在**网关**配置窗格中，保留**复选框**禁用 （没有）**通过网关的连接**。 
2. **连接的名称**，请键入值。 例如，键入`hisdemo2`。
3. **DB2 服务器的名称**、 地址或别名冒号端口号的形式，请键入值。 例如，键入`hisdemo2.cloudapp.net:50000`。
3. **DB2 数据库名称**，请键入值。 例如，键入`nwind`。
4. **用户名**，请键入值。 例如，键入`db2admin`。
5. 请键入**密码**的值。 例如，键入`Password1`。
6. 选择**创建**以继续。 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>获取使用选择的所有行
您可以定义一个逻辑应用程序操作，以便获取 DB2 表中的所有行。 这会指示连接器处理 DB2 SELECT 语句，如`SELECT * FROM AREA`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  输入的**名称**，如`Db2getRows`，**订购**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器中，选择**编辑**，选择**频率**下拉列表可以选择**一天**，，然后选择**间隔**类型**7**。 
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，键入`db2`在**搜索更多操作**编辑框，然后选择**DB2-获取行 （预览）**。
6. 在**获取行 （预览）**操作中，选择**更改连接**。
7. 在**连接**配置窗格中，选择**新建**。 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. 在**网关**配置窗格中，保留**复选框**禁用 （没有）**通过网关的连接**。
    - **连接的名称**，请键入值。 例如，键入`HISDEMO2`。
    - **DB2 服务器的名称**、 地址或别名冒号端口号的形式，请键入值。 例如，键入`HISDEMO2.cloudapp.net:50000`。
    - **DB2 数据库名称**，请键入值。 例如，键入`NWIND`。
    - **用户名**，请键入值。 例如，键入`db2admin`。
    - 请键入**密码**的值。 例如，键入`Password1`。
9. 选择**创建**以继续。

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. 在**表名称**列表中，选择**向下箭头**，然后选择**区域**。
11. 或者，选择**显示高级选项**指定查询选项。
12. 选择**保存**。 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. 在**Db2getRows**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
14. 在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_rows**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;其中应该包括的行的列表。

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>添加一个使用插入的行
您可以定义一个逻辑应用程序操作，以便在 DB2 表中添加一行。 此操作指示连接器处理 DB2 插入语句，如`INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  输入的**名称**，如`Db2insertRow`，**订购**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器中，选择**编辑**，选择**频率**下拉列表可以选择**一天**，，然后选择**间隔**类型**7**。 
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，在**搜索更多操作**编辑框中，键入**db2** ，然后选择**DB2 的插入行 （预览）**。
6. 在**获取行 （预览）**操作中，选择**更改连接**。 
7. 在**连接**配置窗格中，选择连接。 例如，选择**hisdemo2**。

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. 在**表名称**列表中，选择**向下箭头**，然后选择**区域**。
9. （请参见红星） 的所有必需列中输入值。 例如，键入`99999` **AREAID**，请键入`Area 99999`，然后键入`102`的**REGIONID**。 
10. 选择**保存**。

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. 在**Db2insertRow**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
12. 在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_rows**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;这应包括新行。

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>获取使用选择一行
您可以定义一个逻辑应用程序操作，以便读取 DB2 表中的一行。 此操作指示连接器来处理位置选择 DB2 语句，如`SELECT FROM AREA WHERE AREAID = '99999'`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  请输入**名称**(例如"**Db2getRow**")、**订阅**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。 
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器中，选择**编辑**，选择**频率**下拉列表可以选择**一天**，，然后选择**间隔**类型**7**。 
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，在**搜索更多操作**编辑框中，键入**db2** ，然后选择**DB2-获取行 （预览）**。
6. 在**获取行 （预览）**操作中，选择**更改连接**。 
7. 在**连接**配置窗格中，选择一个现有连接。 例如，选择**hisdemo2**。

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. 在**表名称**列表中，选择**向下箭头**，然后选择**区域**。
9. （请参见红星） 的所有必需列中输入值。 例如，键入`99999`的**AREAID**。 
10. 或者，选择**显示高级选项**指定查询选项。
11. 选择**保存**。 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. 在**Db2getRow**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
13. 在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_rows**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;这应包括行。

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>更改使用更新的一行
您可以定义一个逻辑应用程序操作，以便更改在 DB2 表中的一行。 此操作指示连接器处理 DB2 更新语句，如`UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  输入的**名称**，如`Db2updateRow`，**订购**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器中，选择**编辑**，选择**频率**下拉列表可以选择**一天**，，然后选择**间隔**类型**7**。 
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，在**搜索更多操作**编辑框中，键入**db2** ，然后选择**DB2 的更新的行 （预览）**。
6. 在**获取行 （预览）**操作中，选择**更改连接**。 
7. 在**连接**配置窗格中，选择要选择一个现有连接。 例如，选择**hisdemo2**。

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. 在**表名称**列表中，选择**向下箭头**，然后选择**区域**。
9. （请参见红星） 的所有必需列中输入值。 例如，键入`99999` **AREAID**，请键入`Updated 99999`，然后键入`102`的**REGIONID**。 
10. 选择**保存**。 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. 在**Db2updateRow**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
12. 在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_rows**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;这应包括新行。

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>删除使用删除一行
您可以定义一个逻辑应用程序操作，以便在 DB2 表中删除一行。 此操作指示连接器处理 DB2 删除语句，如`DELETE FROM AREA WHERE AREAID = '99999'`。

### <a name="create-a-logic-app"></a>创建一个逻辑应用程序
1.  在**Azure 开始板**，选择**+**（加号）， **Web + 移动**，，然后选择**应用程序逻辑**。
2.  输入的**名称**，如`Db2deleteRow`，**订购**、**资源组**、**位置**和**应用程序服务计划**。 选择**附到仪表板**，然后选择**创建**。

### <a name="add-a-trigger-and-action"></a>添加触发器和操作
1.  **逻辑应用程序设计器**中，在**模板**列表中选择**空白 LogicApp** 。 
2.  在**触发器**列表中，选择**重复执行**。 
3.  在**定期**触发器中，选择**编辑**，选择**频率**下拉列表可以选择**一天**，，然后选择**间隔**类型**7**。 
4.  **+ 新步骤**的框中，选择，然后选择**添加操作**。
5.  在**动作**列表中，在**搜索更多操作**编辑框中，选择**db2** ，然后选择**DB2-删除行 （预览）**。
6. 在**获取行 （预览）**操作中，选择**更改连接**。 
7. 在**连接**配置窗格中，选择一个现有连接。 例如，选择**hisdemo2**。

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. 在**表名称**列表中，选择**向下箭头**，然后选择**区域**。
9. （请参见红星） 的所有必需列中输入值。 例如，键入`99999`的**AREAID**。 
10. 选择**保存**。 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. 在**Db2deleteRow**刀片式服务器，**所有运行****摘要**下, 列表中选择第一个列出的项目 （最新运行）。
12. 在**逻辑应用程序运行**刀片式服务器，选择**运行详细信息**。 在**操作**列表中，选择**Get_rows**。 查看**状态**，应该**成功**的值。 选择**输入链接**可查看输入。 选择**输出链接**，然后查看输出;这应包括已删除的行。

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>技术详细信息

## <a name="actions"></a>操作
操作是由逻辑应用程序中定义的工作流执行的操作。 DB2 数据库连接器包括以下操作。 

|操作|说明|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|从 DB2 表中检索单个行|
|[GetRows](connectors-create-api-db2.md#get-rows)|从 DB2 表中检索行|
|[InsertRow](connectors-create-api-db2.md#insert-row)|DB2 表中插入新行|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|从 DB2 表中删除行|
|[GetTables](connectors-create-api-db2.md#get-tables)|从 DB2 数据库中检索表|
|[UpdateRow](connectors-create-api-db2.md#update-row)|更新现有 DB2 表中的行|

### <a name="action-details"></a>操作详细信息

在此部分，查看有关每个操作，包括任何必需的还是可选的输入的属性，以及任何相应的输出与连接器相关联的特定详细信息。

#### <a name="get-row"></a>获取行 
从 DB2 表中检索单个行。  

| 属性名称| 显示名称 |说明|
| ---|---|---|
|表 * | 表名 |DB2 表的名称|
|id * | 行 id |要检索的行的唯一标识符|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
项目

| 属性名称 | 数据类型 |
|---|---|
|ItemInternalId|字符串|


#### <a name="get-rows"></a>获取行 
从 DB2 表中检索行。  

|属性名称| 显示名称|说明|
| ---|---|---|
|表 *|表名|DB2 表的名称|
|$skip|跳过计数|若要跳过的条目数 (默认值 = 0)|
|$top|获取最大计数|要检索的项的最大数量 (默认值 = 256)|
|$filter|筛选器查询|ODATA 筛选查询以限制的条目数|
|$orderby|排序依据|指定项的顺序的 ODATA orderBy 查询|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
ItemsList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|


#### <a name="insert-row"></a>插入行 
DB2 表中插入一个新行。  

|属性名称| 显示名称|说明|
| ---|---|---|
|表 *|表名|DB2 表的名称|
|项 *|一行|要指定在 DB2 表中插入行|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
项目

| 属性名称 | 数据类型 |
|---|---|
|ItemInternalId|字符串|


#### <a name="delete-row"></a>删除行 
从 DB2 表中删除行。  

|属性名称| 显示名称|说明|
| ---|---|---|
|表 *|表名|DB2 表的名称|
|id *|行 id|要删除的行的唯一标识符|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。

#### <a name="get-tables"></a>获取表 
从 DB2 数据库中检索表。  

没有此调用的参数。 

##### <a name="output-details"></a>输出详细信息 
TablesList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|

#### <a name="update-row"></a>更新行 
更新 DB2 表中的现有行。  

|属性名称| 显示名称|说明|
| ---|---|---|
|表 *|表名|DB2 表的名称|
|id *|行 id|要更新的行的唯一标识符|
|项 *|一行|更新后的值的行|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息  
项目

| 属性名称 | 数据类型 |
|---|---|
|ItemInternalId|字符串|


### <a name="http-responses"></a>HTTP 响应

当调用不同的操作，您可能会收到的特定响应。 下表概述了响应和它们的说明︰  

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="supported-db2-platforms-and-versions"></a>支持 DB2 平台和版本
此连接器支持下列 IBM DB2 平台和版本，以及 IBM DB2 兼容产品 (如 IBM Bluemix dashDB) 的支持分布式关系数据库结构 (DRDA) SQL 访问管理器 (SQLAM) 版本 10 和 11:

- 针对 z/OS 11.1 IBM DB2
- 针对 z/OS 10.1 IBM DB2
- IBM DB2 的 i 7.3
- IBM DB2 的 i 7.2
- 对于 IBM DB2 i 7.1
- IBM DB2 LUW 11
- IBM DB2 LUW 10.5 个


## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 浏览其他可用的连接器，在我们的[Api 列表](apis-list.md)的逻辑应用程序中。

