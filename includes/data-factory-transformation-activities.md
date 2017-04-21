Azure 数据工厂支持以下转换活动可添加到管线或者单独或链与另一个活动。

数据转换活动 |  计算环境 
:----------------------- | :--------------------
[配置单元](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[小猪](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop 流](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[机器学习活动︰ 批处理执行和更新资源](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure 的 VM 
[存储的过程](../articles/data-factory/data-factory-stored-proc-activity.md) | SQL azure、 SQL Azure 数据仓库或 SQL Server |
[数据分析湖 U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure 数据湖分析 
[最低](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] 或 Azure 的批处理
   
> [AZURE.NOTE] 
> 您可以使用 MapReduce 活动 HDInsight 触发群集上运行触发程序。 详细信息，请参阅[从 Azure 数据工厂调用触发程序](../articles/data-factory/data-factory-spark.md)。
> 您可以创建自定义活动与 R 安装 HDInsight 群集上运行 R 脚本。 请参阅[运行 R 脚本使用 Azure 数据工厂](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)。