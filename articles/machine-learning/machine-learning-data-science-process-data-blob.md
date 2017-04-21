<properties 
    pageTitle="处理与高级分析功能的 Azure blob 数据 |Microsoft Azure" 
    description="在 Azure Blob 存储中的流程数据。" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>使用高级分析功能的流程 Azure blob 数据

本文档介绍探索数据和 Azure Blob 存储中存储的数据生成功能。 

## <a name="load-the-data-into-a-pandas-data-frame"></a>将数据加载到了 Pandas 数据帧
为了研究和操纵数据集，则必须下载 blob 从源到一个本地文件，则会加载 Pandas 数据帧中的。 下面是此过程的步骤︰

1. 下载从 Azure 数据 blob 与以下示例 Python 代码使用 blob 服务。 与特定的值替换以下代码中的变量︰ 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. 到了 Pandas 数据的帧从下载的文件中读取的数据。

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

现在您就考察数据，并生成该数据集上的功能。


##<a name="blob-dataexploration"></a>数据研究

下面是一些浏览使用 Pandas 数据的方法的示例︰

1. 检查行和列的数 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. 检查第一个或最后几行中的数据集如下所示︰

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. 检查每一列作为使用下面的代码示例导入数据类型
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. 检查数据集内的列的基本统计信息，如下所示
 
        dataframe_blobdata.describe()
    
5. 看一看的每个列的值的条目数，如下所示

        dataframe_blobdata['<column_name>'].value_counts()

6. 使用下面的代码示例中每一列中的实际项数与缺少的值进行计数

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  如果缺少某个特定列的值数据中，可以删除它们，如下所示︰

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    另一种方法来替换缺少的值是使用模式函数︰
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. 创建一个使用可变数量的储料箱绘制的变量分布的直方图平面图 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. 看一看使用 scatterplot 或使用内置的关联函数的变量之间的相关性

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>功能生成
    
我们可以生成使用 Python 如下功能︰

###<a name="blob-countfeature"></a>基于功能生成的指标值

分类功能可以创建，如下所示︰

1. 检查分类列的分布情况︰
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. 为每个列的值生成指示符值

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. 加入与原始数据帧的标记列 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. 删除原始变量本身︰

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Binning 功能生成

用于生成 binned 的功能，我们继续，如下所示︰

1. 添加要丢弃数字列的列的顺序
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. 将转换为布尔型的变量序列 binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. 最后，加入虚拟变量，返回到原始的数据帧

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>将数据写回到 Azure blob 和 Azure 机器学习中使用

研究了数据并创建所需的功能，您可以将数据上载后 (取样或 featurized) 到 Azure 斑点和 Azure 机器学习可以通过以下步骤中使用它︰ 请注意，可以在 Azure 机器学习 Studio 以及创建附加的功能。 
1. 数据帧写入本地文件

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. 将数据上载到 Azure blob，如下所示︰

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. 现在可以从使用 Azure 机器学习的[导入数据]的 blob 读取数据[import-data]模块，如下面的屏幕中所示︰
 
![读取器 blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
