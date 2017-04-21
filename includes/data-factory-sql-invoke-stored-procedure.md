## <a name="invoking-stored-procedure-for-sql-sink"></a>调用存储的过程的 SQL 水池

当将数据复制到 SQL Server 或 Azure SQL/SQL Server 数据库，则无法配置和使用附加参数调用存储的过程指定用户。 

可以利用存储的过程，当目的时并不提供内置的复制机制。 这通常被利用额外处理 （合并列，查找其他值，插入到多个表...） 需要在目标表中的源数据的最终插入之前完成时。 

您可以调用存储的过程选择。 下面的示例演示如何使用存储的过程来执行简单插入到数据库中的表。 

**输出数据集**

在此示例中，类型设置为︰ SqlServerTable。 将其设置为 AzureSqlTable，使用 SQL Azure 数据库。 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
复制活动 JSON 中定义的 SqlSink 部分，如下所示。 若要调用存储的过程插入数据时，需要 SqlWriterStoredProcedureName 和 SqlWriterTableType 属性。

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

在数据库中，作为 SqlWriterStoredProcedureName 中定义具有相同名称的存储的过程。 它可以处理输入的数据从指定的源和插入到输出表。 注意到该存储过程的参数名称应与表 JSON 文件中定义的表名相同。

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

在数据库中，作为 SqlWriterTableType 中定义具有相同名称的表类型。 请注意，表类型的架构应该与您的输入数据所返回的架构相同。

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

存储的过程功能利用[依次参数](https://msdn.microsoft.com/library/bb675163.aspx)。