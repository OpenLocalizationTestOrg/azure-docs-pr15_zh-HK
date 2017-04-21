<properties 
   pageTitle="开始使用 Azure 数据湖分析使用 Azure 命令行界面 |Microsoft Azure" 
   description="了解如何使用 Azure 命令行界面来创建数据湖存储帐户，请创建使用 U-SQL 数据湖分析作业并提交作业。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>有关使用 Azure 的命令行界面 (CLI) 的 Azure 数据湖分析入门教程︰

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


了解如何使用 Azure CLI 创建 Azure 数据湖分析科目在[U SQL](data-lake-analytics-u-sql-get-started.md)中，定义数据湖分析作业，将作业提交到数据湖分析帐户。 有关数据湖分析的详细信息，请参阅[Azure 数据湖分析概述](data-lake-analytics-overview.md)。

在本教程中，您将开发一个制表符分隔的值 (TSV) 文件并将它转换为逗号分隔值 (CSV) 文件中读取的作业。 要通过同一教程使用其他支持的工具，请单击本节顶部的选项卡。

##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
- **Azure CLI**。 请参阅[安装和配置 Azure CLI](../xplat-cli-install.md)。
    - 下载并安装**预发布** [Azure CLI 工具](https://github.com/MicrosoftBigData/AzureDataLake/releases)来完成此演示。
- **身份验证**，使用下面的命令︰

        azure login
    使用工作或学校的帐户进行验证的详细信息，请参阅[连接到 Azure CLI 从 Azure 订阅](../xplat-cli-connect.md)。
- **切换到 Azure 资源管理器模式**，使用下面的命令︰

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>创建数据湖分析帐户

您可以运行任何作业之前，您必须具有数据湖分析帐户。 若要创建一个数据湖分析帐户，您必须指定以下各项︰

- **Azure 资源组**︰ 必须在 Azure 资源组中创建数据湖分析帐户。 [Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)使您可以使用作为一个组应用程序中的资源。 您可以部署、 更新或删除单个、 协调操作中的应用程序的所有资源。  

    若要枚举您的订阅中的资源组︰
    
        azure group list 
    
    若要创建新的资源组︰

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **数据湖分析帐户名**
- **位置**︰ 支持数据湖分析 Azure 数据中心之一。
- **默认数据湖帐户**︰ 每个数据湖分析帐户具有一个默认数据湖帐户。

    若要列出现有数据湖帐户︰
    
        azure datalake store account list

    若要创建新的数据湖帐户︰

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] 数据湖帐户名称只能包含小写字母和数字。



**若要创建一个数据湖分析帐户**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![数据湖分析显示帐户](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] 数据湖分析帐户名称只能包含小写字母和数字。


## <a name="upload-data-to-data-lake-store"></a>将数据上载到湖边的数据存储

在本教程中，您将处理某些搜索日志。  搜索日志可以存储在数据湖商店或 Azure Blob 存储中。 

Azure 门户提供一个用户界面，用于将某些示例数据文件复制到默认数据湖帐户，其中包括搜索日志文件。 请参阅[准备源数据](data-lake-analytics-get-started-portal.md#prepare-source-data)，将数据传到默认数据湖存储帐户。

若要上载使用 cli 的文件，请使用下面的命令︰

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

数据湖分析还可以访问 Azure Blob 存储。  用于将数据上载到 Azure Blob 存储，请参阅[使用 Azure CLI 使用 Azure 存储](../storage/storage-azure-cli.md)。

## <a name="submit-data-lake-analytics-jobs"></a>提交数据湖分析作业

在 U SQL 语言编写数据湖分析作业。 若要了解有关 U SQL 的详细信息，请参阅[开始使用 U SQL 语言](data-lake-analytics-u-sql-get-started.md)和[U SQL 语言参考](http://go.microsoft.com/fwlink/?LinkId=691348)。

**若要创建数据湖分析作业脚本**

- 与下面的 U SQL 脚本创建一个文本文件，将文本文件保存到您的工作站︰

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    该 U SQL 脚本读取源数据文件使用**Extractors.Tsv()**，，，然后创建 csv 文件使用**Outputters.Csv()**。 
    
    不要修改这两条路径，除非您将源文件复制到其他位置。  如果不存在数据湖分析将创建的输出文件夹。
    
    它是简单的文件存储在默认数据湖帐户使用相对路径。 您也可以使用绝对路径。  例如 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    必须使用绝对路径来访问文件中链接的存储帐户。  在链接的 Azure 存储帐户中存储的文件的语法如下︰
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] 目前不支持 azure Blob 容器公钥 blob 或公共容器的访问权限。      

    
**若要提交作业**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
可以使用以下命令列出作业、 获取作业的详细信息，并取消作业︰

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

作业完成后，您可以使用以下 cmdlet 列出该文件，并下载文件︰
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>请参见

- 若要查看同一教程使用其他工具，请单击页面顶部的选项卡上选择器。
- 更复杂的查询，请参阅[分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)。
- 首先开发 U SQL 应用程序，请参见[使用 Visual Studio 的数据湖工具开发 U-SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)。
- U SQL，请参阅[开始使用 Azure 数据湖分析 U SQL 语言](data-lake-analytics-u-sql-get-started.md)。
- 管理任务，请参阅[管理 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-manage-use-portal.md)。
- 若要获取数据分析湖泊功能的概述，请参阅[Azure 数据湖分析概述](data-lake-analytics-overview.md)。

