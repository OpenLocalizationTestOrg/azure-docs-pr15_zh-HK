<properties
    pageTitle="生成建议使用 Mahout 和基于 WIndows 的 HDInsight |Microsoft Azure"
    description="了解如何使用 Apache Mahout 机器学习库生成影片与基于 Windows 的 HDInsight (Hadoop) 的建议。"
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>使用 Hadoop 在 HDInsight 中使用 Apache Mahout 生成影片的建议

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

了解如何使用[Apache Mahout](http://mahout.apache.org)机器学习与 Azure HDInsight 库生成影片的建议。

> [AZURE.NOTE] 此文档中的步骤需要 Windows 客户端和基于 Windows 的 HDInsight 群集。 Mahout 从 Linux、 OS X 或 Unix 客户端使用基于 Linux 的 HDInsight 群集的信息，请参阅[通过使用 HDInsight 中的基于 Linux 的 Hadoop 使用 Apache Mahout 生成影片建议](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>您将了解

Mahout 是[机器学习][ ml] Apache Hadoop 库。 Mahout 包含用于处理数据，例如，筛选、 分类和聚类的算法。 在本文中，将使用一个推荐引擎来生成影片建议根据您的朋友已经了解的电影。 您还将了解如何执行分类与诊断目录林。 这将阐述如下︰

* 如何通过使用 Windows PowerShell 运行 Mahout 作业

* 如何从 Hadoop 命令行运行 Mahout 作业

* 如何在 HDInsight 3.0 和 HDInsight 2.0 群集上安装 Mahout

    > [AZURE.NOTE] Mahout 提供 HDInsight 3.1 版本的群集。 如果您使用的早期版本的 HDInsight，看到继续前您的[安装 Mahout](#install) 。

##<a name="prerequisites"></a>系统必备组件

- **在 HDInsight 中的基于 Windows 的 Hadoop 群集**。 有关创建一条信息，请参阅[开始使用 Hadoop 在 HDInsight][getstarted]
- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>通过使用 Windows PowerShell 生成建议

> [AZURE.NOTE] 虽然作业中使用本部分的工作原理是使用 Windows PowerShell 的许多类附带 Mahout 当前不能使用 Windows PowerShell，必须使用 Hadoop 命令行运行。 不能使用 Windows PowerShell 的类的列表，请参阅[疑难解答](#troubleshooting)部分。
>
> 使用 Hadoop 命令行来运行 Mahout 作业的示例，请参阅[使用 Hadoop 命令行的分类数据](#classify)。

Mahout 所提供的功能之一是建议引擎。 此引擎接受数据的格式为`userID`， `itemId`，和`prefValue`（该项目的用户首选项）。 Mahout 然后可以执行共同出现的分析，以确定︰_拥有某一项的首选项的用户也具有这些其他项目的首选项_。 Mahout 然后确定用户与类似项目的首选项，可用来提出建议。

下面是使用电影非常简单的示例︰

* __共同的出现__︰ 李先生，Alice 和 Bob 所有喜欢_星球大战_，_回到帝国罢工_，并_返回 Jedi_。 Mahout 确定用户喜欢这些电影之一也像其他两个。

* __共同的出现__︰ 小明和小红还喜欢_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。 Mahout 确定还喜欢以前的三个电影的用户喜欢这三种。

* __相似性建议__︰ 因为李先生喜欢的前三个电影，Mahout 看电影，其他具有相似参数很喜欢，但李先生不监视 （喜欢/等级）。 在这种情况下，Mahout 建议_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。

###<a name="understanding-the-data"></a>了解数据

为方便起见， [GroupLens 研究][movielens]提供评级数据与 Mahout 兼容的格式的电影。 该数据位于群集的默认存储在`/HdiSamples/MahoutMovieData`。

有两个文件︰ `moviedb.txt` （电影中，有关信息） 和`user-ratings.txt`。 虽然 moviedb.txt 用于提供用户友好的文本信息，当显示分析的结果，在分析期间，使用用户 ratings.txt 文件。

用户 ratings.txt 中包含的数据具有结构的`userID`， `movieID`， `userRating`，和`timestamp`，告诉我们如何高，每个用户评价影片。 下面是数据的一个示例︰


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>运行作业

使用下面的 Windows PowerShell 脚本来运行使用 Mahout 推荐引擎的影片数据的作业︰

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout 作业不要删除处理作业时创建的临时数据。 `--tempDir`参数指定示例作业将临时文件隔离为特定的目录中。

Mahout 作业不会返回到标准输出的输出。 相反，它将其存储在指定的输出目录中为__部件-r-00000__。 该脚本将此文件下载到__output.txt__您的工作站上的当前目录中。

以下是此文件的内容示例︰

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

第一列是`userID`。 中包含的值 [和] 是`movieId`:`recommendationScore`。

###<a name="view-the-output"></a>查看输出

尽管生成的输出可能会在应用程序中使用确定，但不便于阅读。 `moviedb.txt`从服务器可用于解决`movieId`向影片的名称，但是您必须首先下载它并分级文件从服务器使用以下脚本︰

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

一旦您已下载的文件，使用 PowerShell 以下脚本显示影片名称的建议︰

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

下面是运行该脚本的一个示例︰

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

输出应类似于以下内容︰

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>使用 Hadoop 命令行对数据进行分类

可使用 Mahout 的分类方法之一是生成一个[随机的林][forest]。 这是涉及使用培训数据来生成决策树，然后用来对数据进行分类需要多个步骤。 这将使用 Mahout 所提供的__org.apache.mahout.classifier.df.tools.Describe__类。 当前必须运行使用 Hadoop 命令行。

###<a name="load-the-data"></a>加载数据

1. 从[NSL KDD 数据集](http://nsl.cs.unb.ca/NSL-KDD/)下载下面的文件。

  * [KDDTrain +。ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff)︰ 培训文件

  * [KDDTest +。ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff)︰ 测试数据

2. 打开每个文件和删除的行开头的顶部'@',，然后保存文件。 如果不删除这些，您将收到错误消息，此数据使用 Mahout 时。

2. 将文件上载到__示例中的数据__。 您可以使用以下脚本来执行此操作。 __群集名称__替换 HDInsight 群集的名称。 用名称替换文件名 fo 文件上载。

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>运行作业

1. 此作业所需 Hadoop 命令行。 启用远程桌面 HDInsight 群集，然后按照下面的说明在[连接到 HDInsight 群集使用 RDP](hdinsight-administer-use-management-portal.md#rdp)连接到它。

3. 连接后，使用__Hadoop 命令行__图标打开 Hadoop 命令行︰

    ![hadoop cli][hadoopcli]

3. 使用以下命令生成的文件描述符 （__KDDTrain +.info__），它使用 Mahout。

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    `N 3 C 2 N C 4 N C 8 N 2 C 19 N L`描述了文件中的数据的属性。 例如，L 表示一个标签。

4. 通过使用以下命令生成决策树的目录林︰

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    此操作的输出存储在位于在 __ wasbs://user/ HDInsight 群集存储__nsl 林__目录&lt;用户名 > / nsl 林 nsl forest.seq。 &lt;用户名 > 是用于远程桌面会话的用户名称。 此文件不是由人类可读的。

5. 通过对__KDDTest +.arff__数据集进行分类测试林中。 使用下面的命令︰

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    此命令将返回类似于以下分类过程的摘要信息︰

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  此作业也会产生一个位于__wasbs:///example/data/predictions/KDDTest+.arff.out__的文件。 但是，此文件不是由人类可读的。

> [AZURE.NOTE] Mahout 作业不会覆盖文件。 如果想要再次运行这些作业，则必须删除以前的作业所创建的文件。

##<a name="troubleshooting"></a>故障排除

###<a name="install"></a>安装 Mahout

Mahout HDInsight 3.1 群集上安装，它可以手动安装 HDInsight 3.0 或 HDInsight 2.1 群集上使用以下步骤︰

1. Mahout 要使用的版本取决于您的群集的 HDInsight 版本。 通过 Azure 门户中查看群集的属性，您可以找到群集版本。

  * __对于 HDInsight 2.1 中__，您可以下载一个包含[Mahout 0.9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)的 Java 归档文件 (JAR) 文件。

  * __对于 HDInsight 3.0__，则必须[生成源 Mahout] [ build] ，并指定由 HDInsight 提供的 Hadoop 版本。 安装生成页面上列出的前提条件，下载源，然后使用下面的命令创建 Mahout jar 文件︰

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Mahout 1.0 发布时，应该能够与 HDInsight 3.0 使用预先构建的软件包。

2. 将 jar 文件上载到您的群集的默认存储区中的__示例/jar__ 。 在以下脚本中的群集名称替换 HDInsight 群集的名称和文件名替换__mahout-coure-0.9-job.jar__文件的路径。.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>无法覆盖文件

Mahout 作业执行清理处理期间创建的临时文件。 此外，作业不会覆盖现有的输出文件。

若要避免错误，运行 Mahout 作业时，删除临时文件和输出文件之间运行，或使用唯一的临时和输出目录的名称。

###<a name="cannot-find-the-jar-file"></a>找不到 JAR 文件

HDInsight 3.1 群集包括 Mahout。 路径和文件名称包括在群集安装的 Mahout 的版本号。 Windows PowerShell 示例脚本在本教程中使用的路径，自 11 月 2015年但版本号将在将来的更改更新到 HDInsight。 要确定您的群集的 Mahout JAR 文件的当前路径，请使用下面的 Windows PowerShell 命令，然后脚本并修改引用返回的文件路径︰

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>不能使用 Windows PowerShell 的类

使用以下类 mahout 作业返回由 Windows PowerShell 各种错误消息︰

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

若要运行作业，使用这些类，连接到 HDInsight 群集中，并使用 Hadoop 命令行运行的作业。 [使用 Hadoop 命令行的分类数据](#classify)的示例，请参见

##<a name="next-steps"></a>下一步行动

现在，您已经学习了如何使用 Mahout，发现 HDInsight 上使用数据的其他方法︰

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [与 HDInsight 的小猪](hdinsight-use-pig.md)
* [MapReduce 与 HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
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
 
