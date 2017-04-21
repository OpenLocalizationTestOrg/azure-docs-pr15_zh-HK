## <a name="fileshare-dataset-type-properties"></a>文件共享数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](../articles/data-factory/data-factory-create-datasets.md)。 如结构、 可用性和 JSON 数据集策略的部分是为所有数据集的类型相似。 

**TypeProperties**部分是不同的每种类型的数据集。 它提供了特定于类型的数据集的信息。 TypeProperties 部分类型**文件共享**数据集的数据集具有以下属性︰

属性 | 说明 | 必填
-------- | ----------- | --------
采用文件夹路径 | 子文件夹路径。 使用转义字符 '\' 特殊字符在字符串中。 有关示例，请参见[示例链接服务和数据集的定义](#sample-linked-service-and-dataset-definitions)。<br/><br/>可将此属性与**partitionBy**具有基于片上的文件夹路径合并的开始/结束日期-时间。 | 是的
文件名 | 在**采用文件夹路径**指定的文件的名称，如果您想要特定文件夹中的文件引用的表。 如果未指定此属性的任何值，表指向该文件夹中的所有文件。<br/><br/>当输出数据集未指定文件名时，所生成的文件的名称将在下面的格式︰ <br/><br/>数据。<Guid>.txt (示例︰ Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | 不
partitionedBy | partitionedBy 可用于指定动态采用文件夹路径，用于时间序列数据的文件名。 例如，采用文件夹路径参数的数据每隔一小时。 | 不
格式 | 支持以下格式类型︰**绑定**、 **AvroFormat**、 **JsonFormat**和**OrcFormat**。 将格式下的**类型**属性设置为下列值之一。 [指定格式](#specifying-textformat)，[指定 AvroFormat](#specifying-avroformat)，[指定 JsonFormat](#specifying-jsonformat)，并[指定 OrcFormat](#specifying-orcformat)部分的详细信息，请参阅。 如果您想要复制的文件-是之间的基于文件的存储 （二进制副本），则可以跳过这两个输入和输出数据集定义中的格式部分。 | 不
过滤 | 指定的筛选器可用于选择采用文件夹路径中的文件，而不是所有文件的子集。<br/><br/>允许值为︰ `*` （多个字符） 和`?`（单个字符）。<br/><br/>示例 1:`"fileFilter": "*.log"`<br/>示例 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> 过滤是适用于输入文件共享数据集。 HDFS 不支持此属性。  | 不
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括︰ **GZip**， **Deflate**， **BZip2**并受支持的级别是︰**最佳**和**最快**。 目前， **AvroFormat**或**OrcFormat**中的数据不支持压缩设置。 有关详细信息，请参阅[压缩支持](#compression-support)部分。  | 不 |
| useBinaryTransfer | 指定是否使用二进制传输模式。 二进制模式和假 ASCII，则为 true。 默认值︰ True。 相关链接的服务类型为类型时，才可以使用此属性︰ FtpServer。 | 不 | 
 

> [AZURE.NOTE] 不能同时使用文件名和过滤。

### <a name="using-partionedby-property"></a>使用 partionedBy 属性

如在上一节中所述，您可以指定动态采用文件夹路径，文件名与 partitionedBy 的时间系列数据。 您可以使用数据工厂宏和系统变量 SliceStart，SliceEnd，用于指示给定的数据切片的逻辑时间段这样做。 

若要了解关于时间系列数据集、 调度和切片，请参阅[创建数据集](../articles/data-factory/data-factory-create-datasets.md)、[计划与执行](../articles/data-factory/data-factory-scheduling-and-execution.md)、 以及[创建管线](../articles/data-factory/data-factory-create-pipelines.md)文章。 

#### <a name="sample-1"></a>示例 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

在此示例中 {切片} 替换指定的格式 (YYYYMMDDHH) 中的数据工厂系统变量 SliceStart 的值。 SliceStart 是指切片的开始时间。 采用文件夹路径是不同的每个扇区。 例如︰ wikidatagateway/wikisampledataout/2014100103 或 wikidatagateway/wikisampledataout/2014100104。

#### <a name="sample-2"></a>示例 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

在此示例中，年、 月、 日和时间 SliceStart 提取到不同采用文件夹路径和文件名属性所使用的变量。
