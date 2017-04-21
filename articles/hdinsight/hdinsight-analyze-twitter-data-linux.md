<properties
    pageTitle="Twitter 使用分析数据 Apache 配置单元上 HDInsight |Microsoft Azure"
    description="了解如何使用 Python 存储包含特定的关键字，然后使用在 HDInsight 上配置单元和 Hadoop，将原始的 TWitter 数据转换为可搜索的配置单元表的 Tweets。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Twitter 使用分析数据中 HDInsight 的配置单元

在此文档中，您将通过使用流式 API Twitter 获得 tweets，然后在基于 Linux 的 HDInsight 群集上处理 JSON 使用 Apache 配置单元带格式的数据。 结果将发送包含某个特定词大部分 tweets 的 Twitter 用户的列表。

> [AZURE.NOTE] 虽然可以使用本文档的各个部分与基于 Windows 的 HDInsight 群集 (如 Python)，许多步骤基于使用基于 Linux 的 HDInsight 群集。 向基于 Windows 群集特定的步骤，请参阅[使用 HDInsight 中的配置单元的分析使用 Twitter 的数据](hdinsight-analyze-twitter-data.md)。

###<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- __基于 linux * 的 Azure HDInsight 群集__。 创建群集的信息，请参阅创建群集的步骤[开始使用基于 Linux 的 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) 。

- __SSH 客户端__。 在 SSH 中使用基于 Linux 的 HDInsight 的详细信息，请参阅下列文章︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__和[pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>获取源 Twitter

Twitter 允许您为通过 REST API 的 JavaScript 对象符号 (JSON) 文档检索[每个 tweet 的数据](https://dev.twitter.com/docs/platform-objects/tweets)。 [OAuth](http://oauth.net) ，则需要进行身份验证的 api。 此外必须创建一个_使用 Twitter 的应用程序_包含用于访问 API 的设置。

###<a name="create-a-twitter-application"></a>创建一个 Twitter 的应用程序

1. 从 web 浏览器登录到[https://apps.twitter.com/](https://apps.twitter.com/)。 如果您没有一个 Twitter 帐户，请单击**现在注册**链接。
2. 单击**创建新应用程序**。
3. 输入**名称**和**说明**，**网站**。 您可以建立**网站**域的 URL。 下表显示了一些示例值来使用︰

  	| 字段 | 值 |
  	|:----- |:----- |
  	| 名称  | MyHDInsightApp |
  	| 说明 | MyHDInsightApp |
  	| 网站 | http://www.myhdinsightapp.com |
    
4. **是的我同意**，检查，然后单击**创建 Twitter 应用程序**。
5. 单击**权限**选项卡。 默认的权限是**只读模式**。 这足以使本教程。
6. 单击**键和访问令牌**选项卡。
7. 单击**创建我的访问令牌**。
8. 在页面的右上角单击**测试 OAuth** 。
9. 请记下**使用者密钥**、**用户机密**、**访问令牌**和**访问令牌的密钥**。 您以后需要值。

>[AZURE.NOTE] 当您在 Windows 中使用 curl 命令时，而不是单引号选项值使用双引号。

###<a name="download-tweets"></a>下载 tweets

下面的 Python 代码将从 Twitter 下载 10000 tweets，并将它们保存到名为__tweets.txt__的文件。

> [AZURE.NOTE] 由于 Python 已经安装在 HDInsight 集群上，执行以下步骤。

1. 连接到使用 SSH 的 HDInsight 群集︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    如果您使用密码来保护您的 SSH 用户帐户时，将提示您重新输入一遍。 如果您使用公用密钥，您可能需要使用`-i`参数指定的匹配的私钥。 例如， `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`。
        
    在 SSH 中使用基于 Linux 的 HDInsight 的详细信息，请参阅下列文章︰
    
    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows)
    
2. 默认情况下，HDInsight head 节点上未安装的__pip__实用程序。 使用以下方法来安装，然后再更新此实用程序︰

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. 若要下载 tweets 的代码依赖于[Tweepy](http://www.tweepy.org/)和[进度栏](https://pypi.python.org/pypi/progressbar/2.2)。 若要安装它们，请使用下面的命令︰

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] 有关删除 python openssl，安装 python 开发、 libffi 开发、 libssl 开发、 pyOpenSSL 和请求 [安全] 位是从 Python 连接到 Twitter 通过 SSL 时避免 InsecurePlatform 警告。
    >
    > Tweepy v3.2.0 用来避免处理 tweets 时可能发生的[错误](https://github.com/tweepy/tweepy/issues/576)。

4. 使用下面的命令创建一个名为__gettweets.py__的新文件︰

        nano gettweets.py

5. 使用以下内容作为__gettweets.py__文件的内容。 替换的占位符信息__使用者\_秘密__，__使用者\_键__，__访问 /\_标记__，和__访问\_令牌\_秘密__从 Twitter 应用程序的信息。

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. 使用__Ctrl + X__，然后__Y__若要保存该文件。

7. 使用下面的命令来运行该文件并下载 tweets:

        python gettweets.py

    进度指示器应该出现，并且下载并保存到文件中的 tweets 统计达 100%。

    > [AZURE.NOTE] 如果花费进度栏提前很长时间，则应更改筛选器来跟踪趋势的主题;当有大量的有关要筛选的主题的 tweets，可以很快获得需要 10000 tweets。

###<a name="upload-the-data"></a>上载数据

若要将数据上传到 WASB （使用 HDInsight 的分布式的文件系统），使用下面的命令︰

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

这在一个群集中的所有节点都可以都访问的位置中存储的数据。

##<a name="run-the-hiveql-job"></a>运行 HiveQL 作业


1. 使用下面的命令来创建一个包含 HiveQL 语句的文件︰

        nano twitter.hql
    
    使用以下内容作为该文件的内容︰

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. 按__Ctrl + X__，再按__Y__若要保存该文件。

4. 使用下面的命令来运行 HiveQL 文件中包含︰

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    这将加载配置单元外壳、 运行在__twitter.hql__文件中，HiveQL 和最后返回`jdbc:hive2//localhost:10001/>`提示。
    
5. 在 beeline 提示符下，使用以下方法来验证您可以创建的__twitter.hql__文件中 HiveQL __tweets__表中选择数据︰
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    这将返回最多 10 个包含消息文本中的单词__Azure__的 tweets。

##<a name="next-steps"></a>下一步行动

在本教程中，我们已经看到如何将非结构化的 JSON 数据集转换为结构化的配置单元表查询、 研究，并对 Azure 上使用 HDInsight 分析从 Twitter 的数据。 若要了解详细信息，请参阅︰

- [开始使用 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [使用 HDInsight 的航班延迟数据分析](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
