### <a name="compression-support"></a>支持压缩  
处理大型数据集可能会导致 I/O 和网络瓶颈。 因此，压缩的数据存储区中可以不仅加快跨网络传输数据和节省磁盘空间，但也会带来显著的性能改进，在处理大数据。 目前，对于如 Azure Blob 或本地文件系统的基于文件的数据存储区支持压缩。  

> [AZURE.NOTE] **AvroFormat**、 **OrcFormat**或**ParquetFormat**中的数据不支持压缩设置。 

指定压缩的数据集，如下面的示例中所示的 JSON 数据集中使用**压缩**属性︰   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
**压缩**部分有两个属性︰  
  
- **类型︰**可以是**GZIP**、 **Deflate**或**BZIP2**压缩编。  
- **级别︰**的压缩比，它可以是**最佳**或**最快**。 
    - **最快︰**即使结果文件不以最佳方式压缩，压缩操作应尽可能快地完成。 
    - **最佳**︰ 压缩操作应采用最佳压缩，即使操作花费更长的时间才能完成。 
    
    有关详细信息，请参阅[压缩级别](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx)主题。 

假设上述示例数据集用作复制活动的输出时，复制活动用 GZIP 编码解码器使用最佳的比率压缩输出数据，然后将压缩的数据写入到名为 pagecounts.csv.gz 的 Azure Blob 存储区中的文件。   

当您指定压缩属性中输入的数据集 JSON 时，管道可以读取压缩的数据源和输出数据集中 JSON，指定该属性时复制活动可以压缩的数据写入目标。 下面是一些示例方案︰ 

- 从 Azure 的斑点，读 GZIP 压缩数据解压缩，并写入 SQL Azure 数据库结果数据。 在这种情况下定义了压缩 JSON 属性输入的 Azure Blob 数据集。 
- 从纯文本格式的文件从本地文件系统读取数据、 压缩使用 GZip 格式和压缩的数据写入 Azure 的 blob。 在这种情况下定义了压缩 JSON 属性输出 Azure Blob 数据集。  
- GZIP 压缩的数据读出 Azure 的 blob、 解压缩、 压缩使用 BZIP2，和结果数据写入 Azure 的 blob。 在这种情况下设置为 BZIP2 压缩类型设置为 GZIP 和输出数据集的压缩类型定义输入的 Azure Blob 数据集。   
