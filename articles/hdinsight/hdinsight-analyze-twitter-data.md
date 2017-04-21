<properties
    pageTitle="Twitter 使用分析数据 Hadoop 在 HDInsight |Microsoft Azure"
    description="了解如何使用配置单元 Twitter 在 Hadoop 在 HDInsight 查找特定单词的使用频率上的数据进行分析。"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Twitter 使用分析数据中 HDInsight 的配置单元

社交网站是大数据采用的主要驱动力之一。 公共 Api 提供的类似 Twitter 的网站是有用的分析和了解流行趋势的数据源。 在本教程中，您将获取 tweets 使用流式 API 中，Twitter，然后使用 Apache 配置单元在 Azure HDInsight 以获取发送包含某个特定词大部分 tweets 的 Twitter 用户的列表。

> [AZURE.NOTE] 本文档中的步骤要求基于 Windows HDInsight 群集。 基于 linux * 的群集特定的步骤，请参阅[使用 HDInsight (Linux) 中的配置单元的分析使用 Twitter 的数据](hdinsight-analyze-twitter-data-linux.md)。


##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- 使用 Azure PowerShell 的**工作站**安装和配置。 

    执行 Windows PowerShell 脚本，您必须以管理员身份运行 Azure PowerShell 并设*RemoteSigned*执行策略。 请参阅[运行 Windows PowerShell 脚本][powershell-script]。

    在运行之前 Windows PowerShell 脚本，确保使用以下 cmdlet 相连 Azure 订购︰

        Login-AzureRmAccount

    如果您有多个 Azure 的订阅，可以使用以下 cmdlet 设置当前订阅︰

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Azure HDInsight 群集**。 有关群集配置的说明，请参阅[开始使用 HDInsight] [hdinsight-get-started]或[群集调配 HDInsight] [hdinsight-provision]。 在本教程后面部分，您将需要群集名称。

下表列出了在本教程中使用的文件︰

文件|说明
---|---
/tutorials/twitter/data/tweets.txt|配置单元作业的源数据。
/tutorials/twitter/output|该配置单元作业的输出文件夹。 默认配置单元作业输出文件的名称是**000000_0**。
tutorials/twitter/twitter.hql|HiveQL 脚本文件。
/tutorials/twitter/jobstatus|Hadoop 作业状态。


##<a name="get-twitter-feed"></a>获取 Twitter 订阅源

在本教程中，您将使用[流式 Api 使用 Twitter][twitter-streaming-api]。 特定于使用 Twitter 流，您将使用的 API 是[筛选器]状态/[twitter-statuses-filter]。

>[AZURE.NOTE] 在公钥 Blob 容器中已上载包含 10000 tweets 的文件和配置单元脚本文件 （在下一节中介绍）。 如果您希望使用已上载的文件，可以跳过此部分。

[Tweets 数据](https://dev.twitter.com/docs/platform-objects/tweets)存储在包含复杂的嵌套的结构的 JavaScript 对象符号 (JSON) 格式。 而不是使用传统的编程语言编写代码行数，则可以转换为此嵌套的结构配置单元表，以便它可以查询由结构化查询语言 (SQL)-喜欢语言称为 HiveQL。

Twitter 使用 OAuth 其 api 提供的授权的访问。 OAuth 是使用户能够批准其代表作用而不共享其密码的应用程序的身份验证协议。 详细信息可从 Hueniverse 在[oauth.net](http://oauth.net/)或优秀[OAuth 初学者指南](http://hueniverse.com/oauth/)中找到。

若要使用 OAuth 的第一步是使用 Twitter，开发人员网站上创建新的应用程序。

**若要创建一个 Twitter 的应用程序**

1. 登录到[https://apps.twitter.com/](https://apps.twitter.com/)。 如果您没有一个 Twitter 帐户，请单击**现在注册**链接。
2. 单击**创建新应用程序**。
3. 输入**名称**和**说明**，**网站**。 您可以建立**网站**域的 URL。 下表显示了一些示例值来使用︰

    字段|值
    ---|---
    名称|MyHDInsightApp
    说明|MyHDInsightApp
    网站|http://www.myhdinsightapp.com

4. **是的我同意**，检查，然后单击**创建 Twitter 应用程序**。
5. 单击**权限**选项卡。 默认的权限是**只读模式**。 这足以使本教程。
6. 单击**键和访问令牌**选项卡。
7. 单击**创建我的访问令牌**。
8. 在页面的右上角单击**测试 OAuth** 。
9. 请记下**使用者密钥**、**用户机密**、**访问令牌**和**访问令牌的密钥**。 在本教程后面部分，您将需要值。

在本教程中，您将使用 Windows PowerShell 进行 web 服务调用。 C#.NET 示例，请参见[使用 HBase HDInsight 在分析实时 Twitter 观点][hdinsight-hbase-twitter-sentiment]。 其他常用工具可以进行 web 服务调用是[*卷曲*][curl]。 可从[此处]下载 curl[curl-download]。

>[AZURE.NOTE] 当您在 Windows 中使用 curl 命令时，而不是单引号选项值使用双引号。

**若要获取 tweets**

1. 打开 Windows PowerShell 集成脚本环境 (ISE)。 （在 Windows 8 启动屏幕上，键入**PowerShell_ISE** ，然后单击**Windows PowerShell ISE**。 请参阅[启动 Windows 8 上的 Windows PowerShell 和 Windows][powershell-start]。)

2. 将以下脚本复制到脚本窗格中︰

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. 在脚本中设置第一个五到八个变量︰


    变量|说明
    ---|---
    $clusterName|这是要用来运行该应用程序在 HDInsight 群集的名称。
    $ oauth_consumer_key|这是您记下前面创建的 Twitter 应用程序时的 Twitter 应用程序**使用者密钥**。
    $ oauth_consumer_secret|这是前面记下的 Twitter 应用程序**使用者密码**。
    $ oauth_token|这是前面记下的 Twitter 应用程序**访问令牌**。
    $ oauth_token_secret|这是 Twitter 应用程序**访问令牌密钥**之前记下。
    $destBlobName|这是输出 blob 名称。 默认值是**tutorials/twitter/data/tweets.txt**。 如果您更改了默认值，您将需要相应地更新 Windows PowerShell 脚本。
    $trackString|Web 服务将返回到这些关键字相关的 tweets。 默认值是**Azure 云、 HDInsight**。 如果要更改默认值，则将相应更新 Windows PowerShell 脚本。
    $lineMax|值决定了该脚本将读取多少 tweets。 大约需要三分钟阅读 100 tweets。 您可以设置一个更大的数字，但会花费更多的时间来下载。

5. 按**f5 键**以运行该脚本。 如果您遇到了问题，作为一种变通方法，选择所有行，然后按**f8 键**。
6. 您应看到"完成 ！" 在输出的末尾。 任何错误消息会以红色显示。

作为验证过程中，您可以放在 Azure Blob 存储使用 Azure 存储资源管理器或 Azure PowerShell 检查输出文件中， **/tutorials/twitter/data/tweets.txt**。 一个 Windows PowerShell 脚本示例列出的文件，请参阅[使用 Blob 存储与 HDInsight][hdinsight-storage-powershell]。



##<a name="create-hiveql-script"></a>创建 HiveQL 脚本

使用 Azure PowerShell，可以一次运行多个 HiveQL 语句一或打包到一个脚本文件的 HiveQL 语句。 在本教程中，您将创建一个 HiveQL 脚本。 必须将脚本文件上载到 Azure Blob 存储。 在下一部分中，您将使用 Azure PowerShell 运行脚本文件。

>[AZURE.NOTE] 已上载的配置单元脚本文件和一个包含 10000 tweets 公钥 Blob 容器中。 如果您希望使用已上载的文件，可以跳过此部分。

HiveQL 脚本将执行以下︰

1. 案例表中**除去 tweets_raw 表**已存在。
2. **创建 tweets_raw 配置单元表**。 此临时配置单元结构化的表包含的数据用于进一步提取、 转换和加载 (ETL) 处理。 在分区上的信息，请参阅[配置单元教程][apache-hive-tutorial]。  
3. 从源文件夹中，/tutorials/twitter/data**加载数据**。 现在已被大 tweets 数据集中嵌套的 JSON 格式转换为临时的配置单元表结构。
3. 案例表中**除去 tweets 表**已存在。
4. **创建 tweets 表**。 可以使用配置单元对 tweets 数据集查询之前，您需要运行另一个 ETL 过程。 此 ETL 过程定义更详细的表架构，有"twitter_raw"表中存储的数据。  
5. **插入覆盖表**。 这个复杂的配置单元脚本将启动一套长 MapReduce 作业 Hadoop 群集。 这取决于您的数据集和群集的大小，这可能需要大约 10 分钟。
6. **插入覆盖目录**。 运行查询并输出到文件的数据集。 此查询将返回"Azure"发送包含该单词的大部分 tweets 的 Twitter 用户的列表。

**若要创建配置单元脚本并将其上载到 Azure**

1. 打开 Windows PowerShell ISE。
2. 将以下脚本复制到脚本窗格中︰

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. 在脚本中设置的前两个变量︰

    变量|说明
    ---|---
    $clusterName|输入要用来运行该应用程序的 HDInsight 群集名称。
    $subscriptionID|输入您的 Azure 的订阅 id。
    $sourceDataPath|Azure Blob 存储位置的配置单元查询在其中读取的数据。 您不需要更改此变量。
    $outputPath|在配置单元查询将输出结果 Azure Blob 存储位置。 您不需要更改此变量。
    $hqlScriptFile|位置和 HiveQL 脚本文件的文件名。 您不需要更改此变量。

5. 按**f5 键**以运行该脚本。 如果您遇到了问题，作为一种变通方法，选择所有行，然后按**f8 键**。
6. 您应看到"完成 ！" 在输出的末尾。 任何错误消息会以红色显示。

作为验证过程中，您可以放在 Azure Blob 存储使用 Azure 存储资源管理器或 Azure PowerShell 检查输出文件中， **/tutorials/twitter/twitter.hql**。 一个 Windows PowerShell 脚本示例列出的文件，请参阅[使用 Blob 存储与 HDInsight][hdinsight-storage-powershell]。  


##<a name="process-twitter-data-by-using-hive"></a>通过使用配置单元处理 Twitter 的数据

您已经完成所有准备工作。 现在，您可以调用配置单元脚本并检查结果。

### <a name="submit-a-hive-job"></a>提交配置单元作业

使用下面的 Windows PowerShell 脚本来运行该配置单元脚本。 您将需要设置第一个变量。

>[AZURE.NOTE] 要在最后两节中使用 tweets 和您上载的 HiveQL 脚本，请将 $hqlScriptFile 设置为"/ tutorials/twitter/twitter.hql"。 若要使用为您上载至公钥 blob 的那些，将 $hqlScriptFile 设置为"wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"。

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>检查结果

使用下面的 Windows PowerShell 脚本来检查配置单元作业输出。 您将需要设置前两个变量。

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] 配置单元表使用 \001 作为字段分隔符。 分隔符不在输出中显示。

分析结果已放在 Azure Blob 存储后，可以将数据导出到 SQL Azure 数据库/SQL 服务器、 将数据导出到 Excel 中，通过使用电源查询，或通过配置单元 ODBC 驱动程序连接到数据应用程序。 有关详细信息，请参阅[使用 Sqoop 与 HDInsight][hdinsight-use-sqoop]，[分析飞行延迟数据使用 HDInsight][hdinsight-analyze-flight-delay-data]，[将 Excel 连接到电源查询 HDInsight][hdinsight-power-query]，和[将 Excel 连接到 Microsoft 配置单元 ODBC 驱动程序的 HDInsight][hdinsight-hive-odbc]。

##<a name="next-steps"></a>下一步行动

在本教程中，我们已经看到如何将非结构化的 JSON 数据集转换为结构化的配置单元表查询、 研究，并对 Azure 上使用 HDInsight 分析从 Twitter 的数据。 若要了解详细信息，请参阅︰

- [开始使用 HDInsight][hdinsight-get-started]
- [分析与 HBase 中 HDInsight 的实时 Twitter 主观意图][hdinsight-hbase-twitter-sentiment]
- [使用 HDInsight 的航班延迟数据分析][hdinsight-analyze-flight-delay-data]
- [将 Excel 连接到电源查询 HDInsight][hdinsight-power-query]
- [将 Excel 连接到 Microsoft 配置单元 ODBC 驱动程序的 HDInsight][hdinsight-hive-odbc]
- [使用 HDInsight Sqoop][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
