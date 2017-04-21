<properties
    pageTitle="配置单元和猪的 HDInsight 中使用 Python |Microsoft Azure"
    description="了解如何在 HDInsight，在 Azure 上 Hadoop 技术堆栈中使用 Python 用户定义函数 (UDF) 从配置单元和小猪。"
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
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/07/2016" 
    ms.author="larryfr"/>

#<a name="use-python-with-hive-and-pig-in-hdinsight"></a>使用 Python 配置单元和 HDInsight 中的小猪

配置单元和小猪非常适合处理的数据在 HDInsight，但有时您需要较一般用途语言。 配置单元和小猪，可以创建用户定义函数 (UDF) 使用不同的编程语言。 在本文中，您将学习如何使用 Python UDF 从配置单元和小猪。

##<a name="requirements"></a>要求

* HDInsight 群集 (Windows 或基于 Linux 的)

* 文本编辑器

    > [AZURE.IMPORTANT] 如果您正在使用基于 Linux 的 HDInsight 服务器，但创建 Windows 客户端上的 Python 文件，您必须使用编辑器 LF 将用作行尾。 如果不能确定是否您的编辑器使用换行符或 CRLF，请参阅[疑难解答](#troubleshooting)部分中的步骤，删除 HDInsight 群集上使用的实用程序的 CR 字符。
    
##<a name="python"></a>Python 在 HDInsight

默认情况下，HDInsight 3.0 和更高版本的群集上安装 Python2.7。 配置单元可以用于与此版本的 Python （配置单元和 Python 使用标准输出/STDIN 之间传递数据） 流处理。

HDInsight 还包括 Jython，是用 Java 编写的 Python 实现。 小猪能理解如何去交谈 Jython 而不必求助于流中，以便更好地使用小猪时。 但是，您也可以用于正常 Python (C Python)，以及小猪。

##<a name="hivepython"></a>配置单元和 Python

Python 可以用作 UDF 从配置单元通过**变换**HiveQL 语句。 例如，下面的 HiveQL 调用存储在**streaming.py**文件中的 Python 脚本。

**基于 linux * 的 HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'python streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**基于 Windows 的 HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] 在基于 Windows 的 HDInsight 群集上**USING**子句必须指定 python.exe 的完整路径。 这始终是`D:\Python27\python.exe`。

下面是此示例的用途︰

1. **将文件添加**语句开头的文件向**streaming.py**文件分布式缓存，因此它是群集中所有节点可以访问。

2. **选择转换...使用**语句从**hivesampletable**中，选择数据，然后将客户机 id、 devicemake 和 devicemodel 传递给**streaming.py**脚本。

3. **AS**子句说明了从**streaming.py**返回的字段

<a name="streamingpy"></a>这里是 HiveQL 示例所使用的**streaming.py**文件。

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

由于我们使用流，此脚本必须执行下列操作︰

1. 从标准输入读取数据。 这通过使用`sys.stdin.readline()`在此示例中。

2. 尾部换行符被删除使用`string.strip(line, "\n ")`，因为我们只是需要的文本数据和未行指示符结尾。

2. 在进行流处理时，单个行包含用制表符字符之间的每个值的所有值。 因此`string.split(line, "\t")`可用于拆分处返回的字段只是每个选项卡上输入。

3. 处理完成后，必须为一条线，并用一个选项卡之间的每个字段到 STDOUT 写入输出。 这通过使用`print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`。

4. 所有发生在`while`循环中，程序会重复，直到无`line`被读取，此时`break`退出循环和脚本终止。

此外，该脚本只是串联的输入的值`devicemake`， `devicemodel`，并计算串联值的哈希值。 很简单的但它描述了从配置单元调用任何 Python 脚本的运行方式的基础知识︰ 循环播放，读取输入直到没有更多，分离的每行输入在选项卡，进程，编写单个制表符分隔输出行。

请参阅有关如何在 HDInsight 群集上运行此示例的[运行示例](#running)。

##<a name="pigpython"></a>小猪和 Python

可通过**生成**语句作为 UDF 从猪的 Python 脚本。 没有两种方法可以完成此操作。使用 Jython (Python 实现在 Java 虚拟机) 和 C Python (常规 Python)。 

它们的主要区别是 Jython 在 JVM 上运行，并且本身可以调用从猪 （还在 JVM 上运行。）C Python 是一个外部进程 （用 C 语言编写）因此从猪的 JVM 上的数据发送到在 Python 的过程中，运行该脚本，然后的输出被发送回小猪。

要确定猪是否使用 Jython 或 C Python 运行该脚本，__注册__引用时使用的 Python 脚本从猪的拉丁语。 这就告诉小猪的解释器使用并为该脚本创建哪些别名。 下面的示例中为__myfuncs__与小猪一起注册脚本︰

* __若要使用 Jython__:`register '/path/to/pig_python.py' using jython as myfuncs;`
* __若要使用 C Python__:`register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [AZURE.IMPORTANT] 当使用 Jython，pig_jython 文件的路径可以是本地路径或 WASB: / / 路径。 但是，当使用 C Python，必须引用用来提交豚作业的节点的本地文件系统上的文件。

一次过注册，猪的拉丁文本示例是相同的两个︰

    LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

下面是此示例的用途︰

1. 第一行将加载示例数据文件中， **sample.log**到**日志**中。 由于此日志文件没有一致的架构，它还定义了每个记录 （在此例中，**行**） 为**chararray**。 实质上，Chararray 是一个字符串。

2. 下一步的行筛选出 null 值的字段，存储到**日志**操作的结果。

3. 接下来，它循环访问**日志**中的记录并使用**生成**调用加载**myfuncs**为 Python/Jython 脚本中包含的**create_structure**方法。  **行**用于将当前记录传递给该函数。

4. 最后，输出转储到标准输出使用**转储**命令。 这只是在操作完成; 之后会立即显示结果在实际脚本中像通常那样**存储**数据到一个新文件。

实际的 Python 脚本文件也是类似与 C Python Jython，唯一的真正区别是，您必须从导入__猪的\_实用工具__时使用 C Python。 以下是__猪的\_python.py__脚本︰

<a name="streamingpy"></a>

    # Uncomment the following if using C Python
    #from pig_util import outputSchema

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail

> [AZURE.NOTE] pig_util 并不需要担心安装;它可自动用于脚本。

还记得，我们以前只是定义输入不一致的架构，因此输入 chararray 作为**行**吗？ Python 脚本的作用是将数据转换为输出一致的架构。 其工作原理如下︰

1. **@outputSchema**语句定义将返回到猪的数据的格式。 在这种情况下，它是**数据袋**，这是猪的数据类型。 包包含了以下字段，它们都是 chararray （字符串）︰

    * 日期-创建该日志条目的日期
    * 时间-日志条目的创建的时间
    * 类名-为创建项的类名
    * 级别的日志级别
    * 详细信息 — 详细日志条目详细信息

2. 接下来，**定义 create_structure(input)**定义猪将传递到行项的函数。

3. 示例数据， **sample.log**，主要是符合日期、 时间、 类名、 级别和我们想要返回的主体架构。 但它也包含字符串*java.lang.Exception*，需要进行修改以匹配的模式开头的几行。 **If**语句检查，然后 massages 移动到结束时，将数据中的行与我们预期的输出架构的*java.lang.Exception*字符串的输入的数据。

4. 接下来，**拆分**命令用于拆分处的前四个空格字符的数据。 这将导致分配**日期**、**时间**、**类名**、**级别**，和**详细**的五个值。

5. 最后，对猪的返回值。

当数据返回给小猪时，它将具有一致的架构中定义**@outputSchema**语句。

##<a name="running"></a>运行示例

如果您正在使用基于 Linux 的 HDInsight 群集，使用**SSH**执行以下步骤。 如果您使用的基于 Windows HDInsight 群集和 Windows 客户端，使用**PowerShell**步骤。

###<a name="ssh"></a>SSH

使用 SSH 的详细信息，请参阅<a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">使用 SSH 上从 Linux、 Unix 或 OS X HDInsight 基于 Linux 的 Hadoop 使用</a> <a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用</a>。

1. 使用 Python 示例[streaming.py](#streamingpy)和[pig_python.py](#jythonpy)，创建您的开发计算机上的文件的本地副本。

2. 使用`scp`文件复制到 HDInsight 群集。 例如，以下会将文件复制到一个名为**mycluster**的群集。

        scp streaming.py pig_python.py myuser@mycluster-ssh.azurehdinsight.net:

3. 使用 SSH 连接到群集。 例如，以下将连接到群集命名为用户**myuser** **mycluster** 。

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. 从 SSH 会话中，添加到群集的 WASB 存储以前上载的 python 文件。

        hdfs dfs -put streaming.py /streaming.py
        hdfs dfs -put pig_python.py /pig_python.py

后上传文件，使用以下步骤运行配置单元和猪的作业。

####<a name="hive"></a>配置单元

1. 使用`hive`命令以启动配置单元外壳。 您应该会看到`hive>`提示后外壳程序已加载。

2. 在输入以下`hive>`提示。

        add file wasbs:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'python streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. 在输入后的最后一行，应开始作业。 最终它将返回输出类似于以下。

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig"></a>小猪

1. 使用`pig`命令来启动外壳程序。 您应该会看到`grunt>`提示后外壳程序已加载。

2. 输入以下语句在`grunt>`运行 Python 脚本使用 Jython 解释器的提示。

        Register wasbs:///pig_python.py using jython as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. 输入以下行后, 应开始作业。 最终它将返回输出类似于以下。

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. 使用`quit`退出 Grunt shell 中，然后使用以下方法来编辑本地文件系统上的 pig_python.py 文件︰

    nano pig_python.py

5. 一次在编辑器中，请取消注释以下行删除`#`从行首的字符︰

        #from pig_util import outputSchema

    一旦进行了更改，可以使用 Ctrl + X 退出编辑器。 选择 Y，然后输入要保存的更改。

6. 使用`pig`命令来重新启动外壳程序。 一旦您在`grunt>`提示，使用以下方法来运行使用 C Python 解释器的 Python 脚本。

        Register 'pig_python.py' using streaming_python as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

    此作业完成后，您会看到相同的输出作为之前运行使用 Jython 脚本。

###<a name="powershell"></a>PowerShell

这些步骤将使用 Azure PowerShell。 如果这已经不是安装和配置开发计算机上，请使用下列步骤之前看到[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. 使用 Python 示例[streaming.py](#streamingpy)和[pig_python.py](#jythonpy)，创建您的开发计算机上的文件的本地副本。

2. 使用下面的 PowerShell 脚本上载**streaming.py**和**猪的\_python.py**文件复制到服务器。 替换的 Azure HDInsight 群集和**streaming.py**的路径名称和**猪的\_python.py**脚本的前三行上的文件。

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\pig_python.py"

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
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
        
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "pig_python.py" `
            -Container $container `
            -Context $context

    此脚本检索信息 HDInsight 群集，然后提取帐户和默认的存储帐户，键，会将文件上载到根容器。

    > [AZURE.NOTE] [将 HDInsight 中的 Hadoop 作业的数据上载](hdinsight-upload-data.md)文档中找不到上载脚本的其他方法。

上传文件之后, 使用以下 PowerShell 脚本开始作业。 作业完成时输出应写入 PowerShell 控制台。

####<a name="hive"></a>配置单元

下面的脚本将运行__streaming.py__脚本。 之前运行，它将提示您输入您的 HDInsight 群集的 HTTPs/管理员帐户信息。

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
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
    
    # If using a Windows-based HDInsight cluster, change the USING statement to:
    # "USING 'D:\Python27\python.exe streaming.py' AS " +
    $HiveQuery = "add file wasbs:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'python streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
    #   -Clustername $clusterName `
    #   -JobId $job.JobId `
    #   -DefaultContainer $container `
    #   -DefaultStorageAccountName $storageAccountName `
    #   -DefaultStorageAccountKey $storageAccountKey `
    #   -HttpCredential $creds `
    #   -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

**配置单元**作业的输出应类似于以下内容︰

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig-jython"></a>小猪 (Jython)

以下将使用__pig_python.py__脚本，使用 Jython 解释器。 之前运行，它将提示您输入 HDInsight 群集的 HTTPs/管理员信息。

> [AZURE.NOTE] 远程提交时使用 PowerShell 的作业，它不能用于 C Python 解释器。

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

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
            
    $PigQuery = "Register wasbs:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
        
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

**小猪**作业的输出应类似于如下︰

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="troubleshooting"></a>故障排除

###<a name="errors-when-running-jobs"></a>在运行作业时的错误

在运行时配置单元作业，可能会遇到类似于以下错误︰

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.
    
此问题可能被引起的 streaming.py 文件中的行尾。 许多窗口编辑器默认到行尾，但 Linux 应用程序通常使用 CRLF 期望 LF。

如果您使用的不能创建 LF 行尾，或者不能确定正在使用哪种行尾的编辑器，使用 PowerShell 下列删除之前将文件上载到 HDInsight CR 字符︰

    $original_file ='c:\path\to\streaming.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)

###<a name="powershell-scripts"></a>PowerShell 脚本

这两个用于运行代码示例的示例 PowerShell 脚本包含的注释的行，会显示该作业的错误输出。 如果您未看到预期的输出作业，请取消注释以下行，如果错误信息表明有问题，请参阅。

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

错误信息 (STDERR，) 并将该结果的作业 (STDOUT，) 还记录到您在以下位置的群集的默认 blob 容器中。

为此作业.|看一看这些文件 blob 容器中
---|---
配置单元|/ HivePython/stderr<p>/ HivePython/标准输出
小猪|/ PigPython/stderr<p>/ PigPython/标准输出

##<a name="next"></a>下一步行动

如果您需要加载默认情况下未提供的 Python 模块，请参阅有关如何执行此操作的示例[部署到 Azure HDInsight 模块的方式](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx)。

有关使用其他方法小猪、 配置单元，以及有关使用 MapReduce，请参阅以下。

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)

* [使用 HDInsight 的小猪](hdinsight-use-pig.md)

* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)
