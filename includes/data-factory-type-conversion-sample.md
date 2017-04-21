### <a name="type-conversion-sample"></a>类型转换示例
下面的示例是将数据从一个 Blob 复制到 SQL Azure，使用类型转换。

假设该 Blob 数据集采用 CSV 格式和包含 3 列。 其中之一是使用日期是星期几的缩写法语名称的自定义 datetime 格式的日期时间列。

您将定义以及类型定义的列，如下所示的 Blob 源数据集。

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

在给定 SQL 类型设置为上面的.NET 类型映射表会定义 SQL Azure 表下面的架构。

| 列名称 | SQL 类型 |
| ----------- | -------- |
| 用户 id | bigint |
| 名称 | 文本 |
| lastlogindate | 日期时间 |

接下来将定义 SQL Azure 数据集，如下所示。 注意︰ 您不需要指定"结构"一节包含类型信息，因为在基础数据存储区中已指定的类型信息。

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

在这种情况下数据工厂会自动进行转换，包括与自定义日期时间日期时间字段的格式将数据从 Blob 移到 SQL Azure 时使用 fr-fr 区域性的类型。


