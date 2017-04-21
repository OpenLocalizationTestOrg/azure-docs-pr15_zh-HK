<properties
    pageTitle="生成建议使用 Mahout 和基于 Linux 的 HDInsight |Microsoft Azure"
    description="了解如何使用 Apache Mahout 机器学习库生成影片与基于 Linux 的 HDInsight (Hadoop) 的建议。"
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>通过对 HDInsight 中的基于 Linux 的 Hadoop 使用 Apache Mahout 生成影片建议

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

了解如何使用[Apache Mahout](http://mahout.apache.org)机器学习与 Azure HDInsight 库生成影片的建议。

Mahout 是[机器学习][ ml] Apache Hadoop 库。 Mahout 包含用于处理数据，例如，筛选、 分类和聚类的算法。 在本文中，将使用一个推荐引擎来生成影片建议根据您的朋友已经了解的电影。

> [AZURE.NOTE] 本文档中的步骤要求 HDInsight 群集上基于 Linux 的 Hadoop。 在 Mahout 中使用基于 Windows 群集的信息，请参阅[通过使用 HDInsight 中的基于 Windows 的 Hadoop 使用 Apache Mahout 生成影片建议](hdinsight-mahout.md)

##<a name="prerequisites"></a>系统必备组件

* 基于 linux * 的 Hadoop，HDInsight 群集上。 有关创建一条信息，请参阅[开始使用 HDInsight 中的基于 Linux 的 Hadoop][getstarted]

##<a name="mahout-versioning"></a>Mahout 版本控制

Mahout HDInsight 群集中包含的版本有关详细信息，请参阅[HDInsight 版本和 Hadoop 组件](hdinsight-component-versioning.md)。

> [AZURE.WARNING] 虽然可以将 Mahout 的不同版本上传到 HDInsight 群集，完全支持 HDInsight 群集提供的组件和 Microsoft 支持将有助于确定和解决与这些组件相关的问题。
>
> 自定义组件接收商业上合理的支持，以帮助您进一步排查该问题。 这可能导致解决问题或要求您能够进行深入的专业技能，为该技术在其中找到的开放源代码技术可用的频道。 例如，有许多社区站点可以使用，如︰ [HDInsight 的 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight)、 [http://stackoverflow.com](http://stackoverflow.com)。 Apache 项目还有项目网站上[http://apache.org](http://apache.org)，例如︰ [Hadoop](http://hadoop.apache.org/)，[触发](http://spark.apache.org/)。

##<a name="recommendations"></a>理解的建议

Mahout 所提供的功能之一是建议引擎。 此引擎接受数据的格式为`userID`， `itemId`，和`prefValue`（该项目的用户首选项）。 Mahout 然后可以执行共同出现的分析，以确定︰_拥有某一项的首选项的用户也具有这些其他项目的首选项_。 Mahout 然后确定用户与类似项目的首选项，可用来提出建议。

下面是使用电影非常简单的示例︰

* __共同的出现__︰ 李先生，Alice 和 Bob 所有喜欢_星球大战_，_回到帝国罢工_，并_返回 Jedi_。 Mahout 确定用户喜欢这些电影之一也像其他两个。

* __共同的出现__︰ 小明和小红还喜欢_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。 Mahout 确定还喜欢以前的三个电影的用户喜欢这三种。

* __相似性建议__︰ 因为李先生喜欢的前三个电影，Mahout 看电影，其他具有相似参数很喜欢，但李先生不监视 （喜欢/等级）。 在这种情况下，Mahout 建议_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。

###<a name="understanding-the-data"></a>了解数据

为方便起见， [GroupLens 研究][movielens]提供评级数据与 Mahout 兼容的格式的电影。 该数据位于群集的默认存储在`/HdiSamples/HdiSamples/MahoutMovieData`。

有两个文件︰ `moviedb.txt` （电影中，有关信息） 和`user-ratings.txt`。 虽然 moviedb.txt 用于提供用户友好的文本信息，当显示分析的结果，在分析期间，使用用户 ratings.txt 文件。

用户 ratings.txt 中包含的数据具有结构的`userID`， `movieID`， `userRating`，和`timestamp`，告诉我们如何高，每个用户评价影片。 下面是数据的一个示例︰


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>运行分析

使用下面的命令运行建议作业︰

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] 作业可能需要几分钟才能完成，并且可以运行多个 MapReduce 作业。

##<a name="view-the-output"></a>查看输出

1. 完成作业后，可以使用下面的命令以查看生成的输出︰

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    输出将显示如下所示︰

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    第一列是`userID`。 中包含的值 [和] 是`movieId`:`recommendationScore`。

2. 输出，以及 moviedb.txt，可用于显示用户友好的详细信息。 首先，我们需要使用以下命令在本地文件复制︰

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    这会将输出数据复制到名为**recommendations.txt**的当前目录，以及电影数据文件中的文件。

3. 使用下面的命令创建一个新的 Python 脚本，将查找电影名称建议输出中的数据︰

        nano show_recommendations.py

    当编辑器打开时，使用以下内容作为该文件的内容︰

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    按下**Ctrl-X**、 **Y**和最后**输入**要保存的数据。

3. 使用下面的命令使可执行文件︰

        chmod +x show_recommendations.py

4. Python 脚本运行。 以下假定所使用的所有文件都下载到其中的目录中︰

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    这要看用户 ID 为 4 的生成的建议。

    * **用户 ratings.txt**文件用于检索用户已分级的影片
    * **Moviedb.txt**文件用于检索的电影名称
    * **Recommendations.txt**用于检索此用户的影片建议

    此命令的输出将类似如下︰

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>删除临时数据

Mahout 作业不要删除处理作业时创建的临时数据。 `--tempDir`参数指定示例作业隔离到特定路径中很容易删除的临时文件中。 若要删除临时文件，请使用下面的命令︰

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] 如果您想要再次运行该命令，您还必须删除的输出目录。 使用以下方法来删除此目录︰
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>下一步行动

现在，您已经学习了如何使用 Mahout，发现 HDInsight 上使用数据的其他方法︰

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [与 HDInsight 的小猪](hdinsight-use-pig.md)
* [MapReduce 与 HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
