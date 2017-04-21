## <a name="column-mapping-with-translator-rules"></a>翻译规则与列映射
列映射可用于指定如何在"结构"源表映射的列指定接收器表"结构"中指定的列。 复制活动的**typeProperties**部分中的**列映射**属性是可用。

列映射支持下列方案︰

- "结构"与源表中的所有列都映射到"结构"中的接收器表中的所有列。
- "结构"与源表中的列的子集映射到"结构"中的接收器表中的所有列。

以下错误情况，将会导致异常︰

- 较少的列或更多的"结构中"接收器表比映射中指定的列中。
- 重复的映射。
- SQL 查询的结果没有映射中指定的列名称。

## <a name="column-mapping-samples"></a>列映射示例
> [AZURE.NOTE] 下面的示例 SQL Azure 和 Azure Blob 但它们适用于任何数据存储区支持矩形的数据集。 您将需要调整数据集和链接的服务定义在下面的示例以指向相关的数据源中的数据。 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>示例 1 – 从 SQL Azure 映射到 Azure blob 列
在此示例中，输入的表有一个结构，它指向 SQL Azure 数据库中的 SQL 表。

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

在此示例中，输出表中包含一个结构，它指向在 Azure blob 存储 blob。

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

下面显示了 JSON 的活动。 使用**转换器**属性映射到接收器 (**columnMappings**) 中的列的源中的列。

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**列映射流程︰**

![列映射流](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>示例 2 – 映射与 SQL 查询从 Azure SQL Azure blob 列
在此示例中，一个 SQL 查询用于从 SQL Azure 中提取数据，而不是只需在"结构"部分中指定的表名和列名。 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

在这种情况下，查询结果首先映射到源的"结构"中指定的列中。 接下来，源"结构"中的列映射到接收器"结构"中的列与在 columnMappings 中指定的规则。  假设该查询将返回 5 列，两个附加列，然后指定源的"结构"中的那些。

**列映射流**

![列映射流-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







