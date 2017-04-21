<properties
   pageTitle="数据源连接 |Microsoft Azure"
   description="描述用于在 Azure Analysis Services 的数据模型的数据源连接。"
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>数据源连接

在 Azure Analysis Services 的数据模型可能需要不同的数据提供程序连接到特定的数据源时。 在某些情况下，连接到使用本机的提供程序，例如 SQL Server 本机客户端 (SQLNCLI11) 的数据源的表格模型可能会返回一个错误。

例如;如果您有内存中或数据模型的直接查询连接到云数据源，例如 SQL Azure 的数据库，如果您使用本机 SQLOLEDB 以外的提供程序，您可能会看到错误消息: **"未注册 SQLNCLI11.1 提供程序"**。

或者，如果您有 DirectQuery 模型连接到内部数据源，如果使用本机的提供程序，您可能会看到错误消息︰ **"创建 OLE DB 行集时出错。限制' 附近有语法错误"**。

## <a name="data-source-providers"></a>数据源提供程序

下列数据源提供程序支持的内存中或连接到内部部署时直接查询数据模型或云数据源︰

|               | **数据源**                     | **内存中**                            |  **直接查询**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **云**                     | SQL azure 数据仓库      | SQL Server 的.NET Framework 数据提供程序 | SQL Server 的.NET Framework 数据提供程序 |
|                           | SQL azure 数据库            | SQL Server 的.NET Framework 数据提供程序 | SQL Server 的.NET Framework 数据提供程序 |
| **内部部署**（通过网关） | SQL Server                    | SQL Server 本机客户端 11.0               | SQL Server 的.NET Framework 数据提供程序 |
|                           |  SQL Server                             | 对于 SQL Server Microsoft OLE DB 提供程序    |   SQL Server 的.NET Framework 数据提供程序                                          |
|                           |  SQL Server                             | SQL Server 的.NET Framework 数据提供程序 |  SQL Server 的.NET Framework 数据提供程序                                           |
|                           | Oracle                        | 适用于 Oracle 的 Microsoft OLE DB 提供程序        | Oracle.NET 数据提供程序               |
|                           |  Oracle                             | Oracle.NET 数据提供程序               | Oracle.NET 数据提供程序                                            |
|                           | Teradata                      | OLE DB 提供程序 Teradata                | 用于.NET Teradata 数据提供程序             |
|                           |  Teradata                             | 用于.NET Teradata 数据提供程序             |  用于.NET Teradata 数据提供程序                                            |
|                           | 分析平台系统 | SQL Server 的.NET Framework 数据提供程序 | SQL Server 的.NET Framework 数据提供程序 |


> [AZURE.NOTE] 确保安装 64 位提供了使用内部网关时。

当迁移到 Azure Analysis Services 的本地 SQL Server Analysis Services 表格模型，可能需要更改提供程序。

**若要指定数据源提供程序**

1. 在 SSDT >**表格模型资源管理器** > **数据源**，数据源连接，用鼠标右键单击，然后单击**编辑数据源**。

2. 在**编辑连接**中，单击**高级**以打开高级属性窗口。

3. 在**设置高级属性** > **提供程序**，然后选择相应的提供程序。

## <a name="impersonation"></a>模拟
在某些情况下，可能需要指定不同的模拟帐户。 可以在 SSDT 或 SSMS 中指定模拟帐户。

对于内部数据源︰

- 如果使用 SQL 身份验证，模拟应为服务帐户。
- 如果使用 Windows 身份验证，请设置 Windows 的用户密码。 对于 SQL Server 支持 Windows 身份验证使用具体的模拟帐户仅对内存中的数据模型。

对于云数据源︰

- 如果使用 SQL 身份验证，模拟应为服务帐户。


## <a name="next-steps"></a>下一步行动

如果您有内部数据源，请务必安装[内部网关](analysis-services-gateway.md)。 若要了解有关管理您的服务器在 SSDT 或 SSMS 中的详细信息，请参阅[管理您的服务器](analysis-services-manage.md)。
