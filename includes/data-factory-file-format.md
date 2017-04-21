## <a name="specifying-formats"></a>指定格式

Azure 数据工厂支持以下格式类型︰ 

- [文本格式](#specifying-textformat)
- [JSON 格式](#specifying-jsonformat)
- [Avro 格式](#specifying-avroformat)
- [ORC 格式](#specifying-orcformat)
- [Parquet 格式](#specifying-parquetformat)

### <a name="specifying-textformat"></a>指定格式

如果格式设置为**格式**，可以在**格式**部分中指定以下**可选**属性。

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | 用来分隔文件中的列的字符。 | 只有一个字符被允许。 默认值是逗号 （，）。 <br/><br/>要使用 Unicode 字符，请参阅[Unicode 字符](https://en.wikipedia.org/wiki/List_of_Unicode_characters)可获得相应的代码。 例如，您可以考虑使用极少的不可打印字符数据中不可能存在的︰ 指定代表开始的标题 (SOH) 的"\u0001"。 | 不 |
| rowDelimiter | 用在一个文件中的行分隔符的字符。 | 只有一个字符被允许。 默认值可以是下列值的任意上读: ["\r\n"、"\r"、"\n"]，并写上的"\r\n"。 | 不 |
| escapeChar | 用于列分隔符输入文件的内容中进行转义特殊字符。 <br/><br/>您不能指定 escapeChar 和 quoteChar 的表。 | 只有一个字符被允许。 没有默认值。 <br/><br/>示例︰ 如果有逗号 （，） 列分隔符，但您想要在文本中有逗号字符 (示例:"Hello，世界")，可以将 $ 定义为转义字符，并使用字符串"Hello$，世界"源中。 | 不 | 
| quoteChar | 用于报价单的字符串值的字符。 在引号字符的列和行分隔符将视为字符串值的一部分。 此属性是适用于输入和输出数据集。<br/><br/>您不能指定 escapeChar 和 quoteChar 的表。 | 只有一个字符被允许。 没有默认值。 <br/><br/>例如，如果您有逗号 （，） 列分隔符，但您想要在文本中有逗号字符 (示例︰ < Hello、 世界 >)，您可以定义"（双引号） 作为引号字符并使用字符串"Hello，世界"源中。 | 不 |
| nullValue | 一个或多个字符，用来表示 null 值。 | 一个或多个字符。 默认值为"\N"和"NULL"读取和"\N"上写上。 | 不 |
| encodingName | 指定编码的名称。 | 有效的编码名称。 请参阅[Encoding.EncodingName 属性](https://msdn.microsoft.com/library/system.text.encoding.aspx)。 示例︰ windows 1250 或 shift_jis。 默认值为 utf-8。 | 不 | 
| firstRowAsHeader | 指定是否要考虑作为一个标头的第一行。 对于输入的数据集，数据工厂作为标头读取第一行。 为输出数据集，数据工厂作为标题写入第一行。 <br/><br/>示例方案，请参阅[使用**firstRowAsHeader**和**skipLineCount**的方案](#scenarios-for-using-firstrowasheader-and-skiplinecount)。 | 真<br/>False （默认值） | 不 |
| skipLineCount | 指示要从输入文件中读取数据时跳过行的数。 如果指定了 skipLineCount 和 firstRowAsHeader，先跳过这些行，然后从输入文件读取此标头信息。 <br/><br/>示例方案，请参阅[使用 firstRowAsHeader 和 skipLineCount 的方案](#scenarios-for-using-firstrowasheader-and-skiplinecount)。 | 整数 | 不 | 
| treatEmptyAsNull | 指定是否从一个输入文件中读取数据时为 null 或空字符串视为 null 值。 | True （默认值）<br/>假 | 不 |  

#### <a name="textformat-example"></a>格式示例
下面的示例演示一些格式属性的格式。

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

若要使用 escapeChar 而不是 quoteChar，替换下面的 escapeChar 与 quoteChar 行︰

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>使用 firstRowAsHeader 和 skipLineCount 的方案

- 非文件从源复制到文本的文件并希望添加一个标题行包含架构元数据 (例如︰ SQL 架构)。 指定为 true，则在这种情况下的输出数据集的**firstRowAsHeader** 。 
- 您从包含非文件接收到一个标题行的文本文件复制，并且想要删除该行。 指定为 true，则在输入数据集**firstRowAsHeader** 。
- 从文本文件中复制，想要跳过开头的几行包含任何数据或标头信息。 指定**skipLineCount**来指示要跳过的行数。 如果该文件的其余部分包含一个标题行，您还可以指定**firstRowAsHeader**。 如果指定了**skipLineCount**和**firstRowAsHeader** ，先跳过这些行，然后从输入文件读取此标头信息

### <a name="specifying-avroformat"></a>指定 AvroFormat
如果格式设置为 AvroFormat，您不需要在 typeProperties 部分中的格式部分中指定的任何属性。 示例︰

    "format":
    {
        "type": "AvroFormat",
    }

若要配置单元表中使用 Avro 格式，您可以参考[Apache 配置单元的教程](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)。

请注意以下几点︰  

- 不支持[复杂数据类型](http://avro.apache.org/docs/current/spec.html#schema_complex)（记录枚举、 数组、 地图、 联合和固定）

### <a name="specifying-jsonformat"></a>指定 JsonFormat

如果将格式设置为**JsonFormat**，可以在**格式**部分中指定以下**可选**属性。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| filePattern | 指示每个 JSON 文件中存储的数据的模式。 允许值为︰ **setOfObjects**和**arrayOfObjects**。 **默认**值是︰ **setOfObjects**。 以下各节有关这些模式的详细信息，请参阅。| 不 |
| encodingName | 指定编码的名称。 有效的编码名称的列表，请参见︰ [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx)属性。 例如︰ windows 1250 或 shift_jis。 **默认**值是︰ **utf-8**。 | 不 | 
| nestingSeparator | 用于分隔的嵌套级别的字符。 默认值是 '。（圆点）。 | 不 | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects 文件模式

每个文件包含单个对象或带有行分隔/串联多个对象。 在输出数据集选择此选项，则当复制活动产生一个 JSON 文件与每行 （行分隔） 的每个对象。

**单个对象** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**分隔线的 JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**串联的 JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects 模式。 

每个文件包含的对象的数组。 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>JsonFormat 示例

如果您有一个 JSON 文件包含以下内容︰  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

并且想要将其复制到 SQL Azure 表以下面的格式︰ 

标识  | Name.First | Name.Middle | Name.Last | 标记
--- | ---------- | ----------- | --------- | ----
1 | 约翰 | 为空 | Doe | ["数据工厂"，"Azure"]

与 JsonFormat 类型的输入数据集定义，如下所示: （分部定义与相关的部分）

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

如果未定义该结构，复制活动默认情况下，会平面化结构，并将复制每一件事。 

#### <a name="supported-json-structure"></a>支持的 JSON 结构
请注意以下几点︰ 

- 每个对象的名称/值对集合映射到一个表格式的数据行。 可以嵌套对象，您可以定义如何在默认情况下嵌套分隔符 （.） 与拼合中数据集的结构。 请参阅前面的示例部分的[JsonFormat 示例](#jsonformat-example)。  
- 如果数据工厂数据集中未定义结构，则复制活动检测中的第一个对象的架构和平面化的整个对象。 
- 如果 JSON 输入数组，复制活动将整个数组的值转换为字符串。 您可以选择通过使用[列映射或过滤](#column-mapping-with-translator-rules)跳过它。
- 如果在同一级别中有重复的名称，复制活动将选取的最后一个。
- 属性名称不区分大小写。 使用名称相同但大小写不同的两个属性都将被视为两个单独的属性。 

### <a name="specifying-orcformat"></a>指定 OrcFormat
如果格式设置为 OrcFormat，您不需要在 typeProperties 部分中的格式部分中指定的任何属性。 示例︰

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] 如果不复制 ORC 文件**作为-是**内部和云之间数据存储区，您需要在您的网关计算机上安装的 JRE 8 （Java 运行时环境）。 64 位网关需要 64 位 JRE，32 位网关需要 32 位的 JRE。 您可以找到从[以下](http://go.microsoft.com/fwlink/?LinkId=808605)两个版本。 选择合适的一个。

请注意以下几点︰

-   复杂数据类型是不受支持 （结构、 地图、 列表、 联合）
-   ORC 文件有三个[与压缩相关的选项](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/)︰ 无 ZLIB，SNAPPY。 数据工厂支持 ORC 任何这些压缩格式的文件中读取数据。 它使用压缩编解码器是元数据中读取的数据。 但是，当写入 ORC 文件，数据工厂选择 ZLIB，ORC 的默认值。 目前，没有任何选项可重写此行为。 

### <a name="specifying-parquetformat"></a>指定 ParquetFormat
如果格式设置为 ParquetFormat，您不需要在 typeProperties 部分中的格式部分中指定的任何属性。 示例︰

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] 如果不将 Parquet 文件复制**作为-是**内部和云之间数据存储区，您需要在您的网关计算机上安装的 JRE 8 （Java 运行时环境）。 64 位网关需要 64 位 JRE，32 位网关需要 32 位的 JRE。 您可以找到从[以下](http://go.microsoft.com/fwlink/?LinkId=808605)两个版本。 选择合适的一个。

请注意以下几点︰

-   复杂数据类型是不受支持 （图，列表）
-   Parquet 文件已压缩相关的以下选项︰ 无，SNAPPY、 GZIP 和 LZO。 数据工厂支持 ORC 任何这些压缩格式的文件中读取数据。 它可以使用元数据中的压缩编解码器读取数据。 但是时写入 Parquet 文件，数据工厂中选择 SNAPPY，这是 Parquet 格式的默认设置。 目前，没有任何选项可重写此行为。 
