<properties
   pageTitle="将 Excel 连接到 Hadoop 使用配置单元的 ODBC 驱动程序 |Microsoft Azure"
   description="了解如何设置和 HDInsight 群集中查询数据，excel 使用 Microsoft 配置单元的 ODBC 驱动程序。"
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>使用 Microsoft 配置单元的 ODBC 驱动程序连接到 Hadoop 的 Excel

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

微软大数据解决方案集成了 Apache Hadoop 群集通过 Azure HDInsight 已部署 Microsoft 商业智能 (BI) 组件。 这种集成的一个示例是将 Excel 连接到使用 Microsoft 配置单元开放式数据库连接(ODBC) (ODBC) 驱动程序的 HDInsight 在 Hadoop 群集配置单元数据仓库的能力。

还有可能与 HDInsight 群集和其他数据源，包括其他 (非 HDInsight) Hadoop 群集，Excel 使用 Query 电源外接 Excel 中的数据连接。 有关安装和使用电源查询的信息，请参阅[将 Excel 连接到电源查询 HDInsight][hdinsight-power-query]。

> [AZURE.NOTE] 虽然这篇文章中的步骤可以用的 Linux 或基于 Windows HDInsight 群集，Windows 是所必需的客户端工作站。

**系统必备组件**︰

在开始这篇文章之前，您必须具有以下︰

- **HDInsight 群集**。 若要创建文档库，请参阅[开始使用 Azure HDInsight][hdinsight-get-started]。
- 与 Office 2013 专业加、 Office 365 专业人员加上、 Excel 2013 独立或 Office 2010 专业版加**一个工作站**。


##<a name="install-microsoft-hive-odbc-driver"></a>安装 Microsoft 配置单元的 ODBC 驱动程序

下载并安装从[下载中心获取]Microsoft 配置单元 ODBC 驱动程序[hive-odbc-driver-download]。

此驱动程序可以安装在 32 位或 64 位版本的 Windows 7、 Windows 8、 Windows 10，Windows Server 2008 R2 和 Windows Server 2012 和将允许连接至 Azure HDInsight （1.6 版及更高版本） 和 Azure HDInsight 仿真程序 (v.1.0.0.0 或更高版本)。 您应该安装应用程序可在其中使用的 ODBC 驱动程序的版本相匹配的版本。 本教程中，将从 Office Excel 使用驱动程序。

##<a name="create-hive-odbc-data-source"></a>创建配置单元的 ODBC 数据源

以下步骤显示如何创建一个配置单元的 ODBC 数据源。

1. 从 Windows 8 或 Windows 10，按 Windows 键打开开始屏幕，然后键入**数据源**。
2. 这取决于您的 Office 版本，请单击**设置 ODBC 数据源 （32 位）**或**设置 ODBC 数据源 （64 位）** 。 如果您使用的 Windows 7，从**管理工具**选择**（32 位） 的 ODBC 数据源**或**ODBC 数据源 （64 位）** 。 这将启动**ODBC 数据源管理器**对话框。

    ![OBDC 数据源管理器][img-hdi-simbahiveodbc-datasource-admin]

3. 从用户的 DNS，请单击**添加**以打开**创建新数据源**向导。
4. 选择**Microsoft 配置单元 ODBC 驱动程序**，然后再单击**完成**。 这将启动**Microsoft 配置单元 ODBC 驱动程序 DNS 设置**对话框。

5. 键入或选择以下值︰

    属性|说明
    ---|---
    数据源名称|为您的数据源名称
    主机|输入&lt;HDInsightClusterName >。 azurehdinsight.net。 例如，myHDICluster.azurehdinsight.net
    端口|使用<strong>443</strong>。 （此端口已从 563 443。）
    数据库|使用<strong>默认值</strong>。
    配置单元服务器类型|选择<strong>配置单元服务器 2</strong>
    机制|选择<strong>Azure HDInsight 服务</strong>
    HTTP 路径|保留为空。
    用户名称|输入 HDInsight 群集用户的用户名。 这是群集配置过程中创建的用户名。 如果您使用快速创建选项，则默认的用户名是<strong>管理员</strong>。
    密码|输入 HDInsight 群集用户密码。
    </table>

    有一些重要的参数，需要注意的当您单击**高级选项**︰

    参数|说明
    ---|---
    使用本机查询|选中此选项，ODBC 驱动程序不会尝试将 TSQL 转换为 HiveQL。 仅当您确认您要提交纯 HiveQL 语句的 100%，您应使用它。 连接到 SQL Server 或 SQL Azure 数据库时，您应保留未选中状态。
    每个块读取的行|当取大量的记录，调整此参数可能需要确保最佳性能。
    默认字符串列的长度、 二进制列的长度、 小数列的小数位|数据类型的长度和精度会影响如何返回数据。 它们将导致不正确的信息，要返回因精度和/或截断。


    ![高级的选项][img-HiveOdbc-DataSource-AdvancedOptions]

6. 单击**测试**以测试数据源。 当数据源配置正确时，则显示*成功完成测试 ！*。
7. 单击**确定**以关闭测试对话框。 现在应在**ODBC 数据源管理器**列出新的数据源。
8. 单击**确定**退出该向导。

##<a name="import-data-into-excel-from-hdinsight"></a>将数据导入 Excel 从 HDInsight

下面的步骤描述配置单元表中的数据导入 Excel 工作簿使用上述步骤中创建的 ODBC 数据源的方法。

1. 在 Excel 中打开新的或现有的工作簿。
2. 从**数据**选项卡，单击**从其他数据源**，然后单击**来自数据连接向导**，以启动**数据连接向导**。

    ![打开数据连接向导][img-hdi-simbahiveodbc.excel.dataconnection]

3. 作为数据源，选择**ODBC DSN** ，然后单击**下一步**。
4. 从 ODBC 数据源，选择您在前面步骤中创建的数据源名称，然后单击**下一步**。
5. 重新输入在该向导中，群集的密码，然后单击**测试**再一次验证的配置，如果需要。
6. 单击**确定**以关闭测试对话框。
7. 单击**确定**。 等待**选择数据库和表**对话框中打开。 这可能需要几秒钟。
8. 选择要导入的表，然后单击**下一步**。 *Hivesampletable*是附带了 HDInsight 群集示例配置单元表。  如果尚未创建，您可以选择它。 了解更多有关运行配置单元查询和创建配置单元表，请参阅[使用配置单元与 HDInsight][hdinsight-use-hive]。
8. 单击**完成**。
9. 在**导入数据**对话框中，您可以更改或指定的查询。 要执行此操作，请单击**属性**。 这可能需要几秒钟。
10. 单击**定义**选项卡，然后将**限制 200**附加到**命令文本**文本框中的配置单元 select 语句。 修改将限制为 200 返回的记录集。

    ![连接属性][img-hdi-simbahiveodbc-excel-connectionproperties]

11. 单击**确定**以关闭连接属性对话框。
12. 单击**确定**以关闭**导入数据**对话框。  
13. 重新输入密码，然后单击**确定**。 需要数据获取导入到 Excel 之前的几秒钟。

##<a name="next-steps"></a>下一步行动

在这篇文章中，您将了解如何使用 Microsoft 配置单元的 ODBC 驱动程序来导入 Excel HDInsight 服务中检索数据。 同样，可以从 HDInsight 服务到 SQL 数据库检索数据。 就还可以将数据上传到 HDInsight 服务。 若要了解详细信息，请参阅︰

- [使用 HDInsight 的航班延迟数据分析][hdinsight-analyze-flight-data]
- [将数据上传到 HDInsight][hdinsight-upload-data]
- [使用 HDInsight Sqoop] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
