<properties
    pageTitle="数据工厂教程︰ 第一个数据管道 |Microsoft Azure"
    description="本 Azure 数据工厂教程演示如何创建和安排处理数据使用 Hadoop 群集配置单元脚本的数据工厂。"
    services="data-factory"
    keywords="azure 数据工厂教程，hadoop 集群，hadoop 配置单元"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>教程︰ 创建用于处理数据使用 Hadoop 群集您第一个渠道 
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-build-your-first-pipeline.md)
- [Azure 门户](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [资源管理器模板](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API，](data-factory-build-your-first-pipeline-using-rest-api.md)

在本教程中，您可以生成数据管道通过 Azure HDInsight (Hadoop) 群集上运行配置单元脚本处理数据与第一个 Azure 数据工厂。 

> [AZURE.NOTE] 这篇文章不提供 Azure 数据工厂的概念性概述。 该服务的概念性概述，请参阅[Azure 数据工厂简介](data-factory-introduction.md)。 请参阅[数据工厂学习路径](https://azure.microsoft.com/documentation/learning-paths/data-factory/)浏览我们的内容建议的方法来了解数据工厂。

## <a name="whats-covered-in-this-tutorial"></a>在本教程中包含的内容？ 
**Azure 数据工厂**可以为数据驱动的工作流 （也称为数据管道） 撰写数据**移动**和数据**处理**任务。 您将学习如何构建您的第一个数据管道与数据处理 （或数据转换） 活动。 此活动使用 HDInsight Hadoop 群集转换和分析示例 web 日志。  

在本教程中，您将执行以下步骤︰

1.  创建一个**数据工厂**。 数据工厂可以包含一个或多个数据管道移动和处理数据。 
2.  创建**链接的服务**。 若要链接数据存储的链接的服务或计算服务于数据工厂创建。 如 Azure 存储数据存储区保存在管道中的输入/输出数据的活动。 如 HDInsight Hadoop 群集的计算服务流程/转换数据。    
3.  创建输入和输出**数据集**。 输入数据集表示为管道中活动的输入和输出数据集表示活动的输出。
3.  创建**管线**。 管道可以具有一个或多个活动 (示例︰ 复制活动、 HDInsight 配置单元活动)。 此示例使用 HDInsight Hadoop 群集运行的配置单元脚本 HDInsight 配置单元活动。 该脚本首先创建引用 Azure blob 存储中存储的原始 web 日志数据的表，然后按年和月的原始数据进行分区。

    有两种类型的活动支持的 Azure 数据工厂。 它们是︰[数据移动活动](data-factory-data-movement-activities.md)和[数据转换活动](data-factory-data-transformation-activities.md)。 没有一种数据移动活动，即复制活动。 在本教程中，您不要使用复制活动。 使用复制活动的教程，请参见[教程︰ Azure 的数据复制到 SQL Azure 的 blob](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 您在本教程中使用的 HDInsight 配置单元活动是一种数据工厂支持的数据转换活动。  
 
这里是您在本教程中生成示例数据工厂的**关系图视图**。 

![数据工厂教程中的关系图视图](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

在本教程中， **adfgetstarted** Azure blob 容器的**inputdata**文件夹中包含一个名为 input.log 的文件。 该日志文件有三个月的项目︰ 一月、 二月和三月的 2016年。 下面是示例行每个月在输入文件中。 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

当与 HDInsight 配置单元活动管道处理文件时，活动运行配置单元脚本 HDInsight 群集上分区，按年和月输入数据。 该脚本创建三个输出文件夹，其中包含每个月中的项的文件。  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

从上面所示的示例行，第一个 （与 2016年-01-01) 被写入到 000000_0 文件在月中 = 1 的文件夹。 同样，第二个被写入到文件在一个月中 = 2 文件夹和第三个月一个被写入到文件 = 3 的文件夹。  


## <a name="pre-requisites"></a>必备组件
在开始本教程之前，您必须满足以下先决条件︰

1.  **Azure 订阅**-如果没有 Azure 的订阅，可以在几分钟创建一个免费试用帐户。 有关如何获得免费的试用帐户，请参阅[免费试用版](https://azure.microsoft.com/pricing/free-trial/)的文章。

2.  **Azure 存储**--将数据存储在本教程中使用 Azure 存储帐户。 如果没有 Azure 存储帐户，请参阅文章[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)。 在创建存储帐户之后，请注意下的**帐户名称**和**访问键**。 请参阅[查看、 复制和重新生成存储访问键](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys)。 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>将文件上载到 Azure 存储，用于本教程
开始本教程之前，您需要准备教程 Azure 存储帐户的示例文件。

1. 将配置单元查询文件 (HQL) 上载到**adfgetstarted** blob 容器的**脚本**文件夹中。
2. 将输入的文件上载到**adfgetstarted** blob 容器的**inputdata**文件夹中。 

#### <a name="create-hql-script-file"></a>创建 HQL 脚本文件 

1. 启动**记事本**，然后粘贴以下 HQL 脚本。 该配置单元脚本创建两个表︰ **WebLogsRaw**和**WebLogsPartitioned**。 单击**文件**菜单上，选择**另存为**。 切换到您的硬盘上的**C:\adfgetstarted**文件夹。 选择**的所有文件 (*。*)**的**另存为类型**字段。输入**partitionweblogs.hql**的****的文件名。确认， ****底部的对话框中的字段设置为**ANSI**编码。如果不是，则将其设置为**ANSI * *。  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

在运行时，数据工厂管道中的配置单元活动传递的值**inputtable**和**partitionedtable**参数，如下面的代码段所示︰  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname**是 Azure 存储帐户的名称。
 
#### <a name="create-a-sample-input-file"></a>创建一个示例输入的文件
使用记事本创建一个名为**c:\adfgetstarted**使用以下内容中的**input.log**文件︰ 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>输入的文件和 HQL 文件上传到 Azure Blob 存储

本部分使用**AzCopy**工具可以将 input.log 和 partitionweblogs.hql 文件复制到 Azure Blob 存储提供指导。 您可以使用您选择的任何工具 (例如︰ [Microsoft Azure 存储浏览器](http://storageexplorer.com/)，[通过 ClumsyLeaf 软件 CloudXPlorer](http://clumsyleaf.com/products/cloudxplorer)来执行该任务。   
     
1. 下载[最新版本**AzCopy**](http://aka.ms/downloadazcopy)或[最新的预览版本](http://aka.ms/downloadazcopypr)。 请参阅有关使用该实用程序说明[如何使用 AzCopy](../storage/storage-use-azcopy.md)文章。
2. 导航至 c:\adfgetstarted 文件夹中，然后运行以下命令︰ 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    此命令将**input.log**文件上载到的存储帐户 （**adfgetstarted**容器和**inputdata**文件夹）。 替换为**storageaccountname**您的存储帐户和**storageaccesskey**的名称和存储的访问键。

    > [AZURE.NOTE] 此命令创建一个名为**adfgetstarted**的 Azure Blob 存储容器，并将**input.log**文件从本地硬盘复制到容器中的**inputdata**文件夹。 
    
3. 文件已成功上载后，您会看到类似于下面的 AzCopy 的输出。
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. 运行下面的命令以将**partitionweblogs.hql**文件上载到**adfgetstarted**容器的**脚本**文件夹。 以下是该命令︰ 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

您已经完成了系统必备组件。 您可以创建使用以下方式之一的数据工厂。 单击某个选项卡顶部或下面的链接以执行本教程。 

- [Azure 门户](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [资源管理器模板](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API，](data-factory-build-your-first-pipeline-using-rest-api.md)