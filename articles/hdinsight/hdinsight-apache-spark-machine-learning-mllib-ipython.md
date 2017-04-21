<properties 
    pageTitle="使用 Apache 触发生成上 HDInsight 的机器学习应用程序 |Microsoft Azure" 
    description="如何使用 Apache 触发笔记本构建机器学习应用程序的分步指导" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>机器学习︰ HDInsight linux 群集数据使用 Apache 触发 MLlib 食品检查预测分析

> [AZURE.TIP] 本教程中也有作为 Jupyter 笔记本在您在 HDInsight 中创建一个触发 (Linux) 群集。 笔记本的经验，可以从笔记本本身运行的 Python 代码段。 若要执行从本教程的笔记本内，创建触发群集时，启动 Jupyter 笔记本 (`https://CLUSTERNAME.azurehdinsight.net/jupyter`)，然后在**Python**文件夹下运行**触发机器学习的食品检测数据的预测分析使用 MLLib.ipynb**笔记本。


这篇文章演示了如何使用**MLLib**，触发的内置计算机学习库，打开数据集上执行简单的预测分析。 MLLib 是一种核心触发库，提供了大量可用于机器学习任务，包括适合的实用程序的实用程序︰

* 分类

* 回归

* 群集

* 主题建模

* 单值分解 (SVD) 和主体组件分析 (PCA)

* 假设测试和计算示例统计信息

本文介绍了通过物流回归*分类*的简单方法。

## <a name="what-are-classification-and-logistic-regression"></a>分类和物流的回归是什么？

*分类*，常用的机器学习任务，是按类别排序输入的数据的过程。 它是要弄清楚如何分配"标签"以输入您提供的数据的分类算法的工作。 例如，您可以考虑接受股票信息作为输入，并将股票划分为两个类别的机器学习算法的︰ 应出售其股票和股票应该保留的。

物流的回归是用于分类的算法。 触发的物流回归 API 可用于*二元分类*，或对输入的数据进行分类，分为两个组。 关于物流的回归测试的详细信息，请参阅[维基百科](https://en.wikipedia.org/wiki/Logistic_regression)。

总之，物流回归的过程产生了*物流功能*，可用于预测输入的向量位于一个组或其他的可能性。  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>我们试图在本文中完成什么？

您将使用触发采集通过[芝加哥城市数据门户](https://data.cityofchicago.org/)的食品检查数据 (**Food_Inspections1.csv**) 执行一些预测性的分析。 此数据集包含在芝加哥，包括有关被检测到的每个食品建立信息、 发现 （如果有的话），冲突和检查的结果进行食品检验有关的信息。 CSV 数据文件已在位于**/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**的群集与关联的存储帐户。

在以下步骤中，您将开发模型，以便查看所需的通过或失败食品检查。 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>开始构建使用触发 MLlib 的机器学习应用程序

1. 从[Azure 门户网站](https://portal.azure.com/)，startboard，从单击触发群集的拼贴 （如果您将它固定到 startboard）。 您还可以向下**浏览所有**群集导航 > **HDInsight 群集**。   

2. 从触发群集刀片式服务器，**群集的仪表板**，请单击，然后单击**Jupyter 笔记本**。 出现提示时，输入群集管理员凭据。

    > [AZURE.NOTE] 通过在浏览器中打开下面的 URL，还可能会达到 Jupyter 笔记本为群集。 __群集名称__替换您的群集的名称︰
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. 创建新的笔记本。 单击**新建**，然后单击**PySpark**。

    ![创建一个新的 Jupyter 笔记本](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "创建一个新的 Jupyter 笔记本")

3. 创建并打开名为 Untitled.pynb 的新笔记本。 单击顶部的笔记本名称并输入好记的名称。

    ![提供笔记本的名称](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "提供笔记本的名称")

3. 由于在创建笔记本使用 PySpark 内核，您不需要显式创建任何上下文。 运行第一个代码单元格时，将为您自动创建添姿加配置单元的上下文。 您可以开始构建您的机器学习的应用程序通过导入该方案所需的类型。 若要执行此操作，将光标置于单元格，请按**SHIFT + ENTER**。


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>输入 dataframe 构造

我们可以使用`sqlContext`在结构化数据上执行转换。 第一项任务是到触发 SQL *dataframe*加载示例数据 ((**Food_Inspections1.csv**))。 

1. 由于原始数据是采用 CSV 格式，我们需要使用触发上下文来将该文件的每一行拉到内存作为非结构化文本;然后，您可以使用 Python 的 CSV 库进行单独分析每一行。 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. 我们现在将作为 RDD 的 CSV 文件。 让我们从了解数据的架构 RDD 检索一行。


        inspections.take(1)


    您应该看到类似于下面的输出︰

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. 以上输出为我们提供了了解架构的输入的文件;该文件包括每个设施、 设施、 通讯、 检验、 和的位置，除了其他的事项的数据的类型的名称。 我们选择了几个栏，将有利于我们预测分析，并对结果分组为 dataframe，我们用它来创建临时表。


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. 现在，我们有*dataframe*， `df` ，我们可以在其上执行我们的分析。 我们还提供临时表调用**CountResults**。 我们提供了 4 列的 dataframe 感兴趣︰ **id**、**名称**、**结果**和**冲突**。 
    
    让我们得到数据的一个小示例︰

        df.show(5)

    您应该看到类似于下面的输出︰

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>理解数据

1. 让我们先来了解我们数据集所包含的内容。 例如，在**结果**列中的不同值是什么？


        df.select('results').distinct().show()

    
    您应该看到类似于下面的输出︰

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. 快速的可视化效果可以帮助我们如何分布这些结果的原因。 我们已经具有临时表**CountResults**中的数据。 针对要更好地了解如何分配结果的表，您可以运行下面的 SQL 查询。

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql`魔术跟`-o countResultsdf`可确保 Jupyter 服务器 (通常是群集中的 headnode) 在本地保存查询的输出中。 输出为具有指定的名称**countResultsdf** [Pandas](http://pandas.pydata.org/) dataframe 保持不变。
    
    您应该看到类似于下面的输出︰
    
    ![SQL 查询输出](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL 查询输出")

    有关详细信息`%%sql`的神奇功能，以及其他 magics 与 PySpark 内核，可查看[Jupyter 笔记本与触发 HDInsight 群集上可用的内核](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels)。

3. Matplotlib，用来构造数据，可视化的库还可用于创建绘图。 必须从本地持久化的**countResultsdf** dataframe 创建绘图，因为代码段必须以`%%local`魔术。 这将确保在 Jupyter 服务器上本地运行的代码。

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    您应该看到类似于下面的输出︰

    ![结果输出](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. 您可以看到有 5 检查可以有不同的结果︰
    
    * 乐的业务 
    * 失败
    * 传递
    * 带有条件，Pss 和
    * 破产 

    让我们开发模型可以猜到的食品检测，结果给出冲突。 由于物流的回归是一种二进制的分类方法，意义分为两个类别组数据︰**失败**和**传递**。 "传递带条件"仍是处理过程中，因此当我们训练模型，我们将考虑两个结果相当。 因此，我们会将它们从我们的训练集的其他结果 （"业务乐"、"外出时的业务"） 的数据是没有用的。 这应该是没有问题，因为这两类仍占很小比例的结果。

5. 让我们继续操作并转换我们现有的 dataframe (`df`) 到每个检查都作为标签冲突对新 dataframe。 在我们的例子中，标签的`0.0`表示出现故障时，标签的`1.0`表示成功和标签的`-1.0`表示这两个除了一些结果。 计算新的数据帧时，我们会过滤掉那些其他的结果。


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    让我们从已标记的数据，查看它的外观中检索一行。


        labeledData.take(1)


    您应该看到类似于下面的输出︰
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>从输入 dataframe 创建物流的回归模型

我们最后的任务是将标记的数据转换成可以通过物流回归分析的格式。 物流的回归算法的输入应该是一组*标签特征向量对*，其中"特征向量"是数字表示以某种方式输入的点的向量。 因此，我们需要一种方法转换的"违规"列中，半结构化且包含大量的注释在自由文本，指向数组的一台机器可以容易地理解的真实数字。 

一个标准的机器学习方法来处理自然语言是分配不同的每个单词"索引"，然后将一个向量传递给机器学习算法，以便每个索引值包含该单词在文本字符串中的相对频率。 

MLLib 提供了简便的方法来执行此操作。 首先，我们将"标记"来获取单个单词中的每个字符串，每个冲突字符串，然后，我们将使用`HashingTF`转换特征向量，然后交给物流回归算法来构造模型的每个组的标记。 我们将使用"管道"的序列中执行所有这些步骤。


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>评估在一个单独的测试数据集模型

我们可以使用创建的模型我们早*预测*到的结果将是新的检验，根据观察到的冲突。 我们训练数据集**Food_Inspections1.csv**上的此模型。 让我们使用第二个数据集， **Food_Inspections2.csv**，为*评估*这种新的数据模型的强度。 此第二个数据集 (**Food_Inspections2.csv**) 已应与群集相关的默认存储容器中。

1. 下面的代码段创建新的 dataframe， **predictionsDf** ，其中包含生成的模型预测。 代码段还会创建临时表**的预测**基于 dataframe。


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    您应该看到类似于下面的输出︰
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. 查看预测之一。 运行此代码段︰

        predictionsDf.take(1)

    您将看到测试数据集内的第一项的预测。

3. `model.transform()`方法将对任何新数据与相同的架构，应用相同的转换并得出如何对数据进行分类预测。 我们可以做一些简单的统计数据，以了解我们的预测的准确性如何︰


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    输出如下所示︰
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    使用触发物流回归为我们提供了精确的模型的冲突说明英语和是否给定的业务会通过或失败食品检测之间的关系。 

## <a name="create-a-visual-representation-of-the-prediction"></a>创建预测的可视化表示形式

我们现在可以构建最终的可视化效果，以帮助我们有关此测试的结果的原因。 

1. 我们开始通过提取不同的预测和结果从前面创建的**预测**临时表。 以下查询将分开为*true_positive*、 *false_positive*、 *true_negative*和*false_negative*的输出。 在下面的查询中，我们将关闭可视化通过`-q`并且还可以保存输出 (使用`-o`) 作为可以与再使用的 dataframes`%%local`魔术。 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. 最后，使用下面的代码段来生成使用**Matplotlib**绘图。

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    您应看到下面的输出。
    
    ![预测输出](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    在此图中，"肯定"的结果是指失败的食品检测中，而负面的结果是指通过检查。

## <a name="shut-down-the-notebook"></a>关闭笔记本

运行应用程序之后，您应该关闭笔记本以释放资源。 为此，请从笔记本上的**文件**菜单上，单击**关闭并暂停**。 这将关闭和关闭笔记本。


## <a name="seealso"></a>请参见


* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>方案

* [触发与 BI︰ 执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)

* [机器学习与触发︰ 用于分析使用 HVAC 数据的生成温度 HDInsight 中使用触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [HDInsight 用于构建实时流的应用程序中触发流︰ 使用触发](hdinsight-apache-spark-eventhub-streaming.md)

* [在 HDInsight 中使用触发网站日志分析](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>创建和运行应用程序

* [创建独立的应用程序使用 Scala](hdinsight-apache-spark-create-standalone-application.md)

* [在群集上使用晚触发远程运行作业](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>工具和扩展

* [使用 HDInsight 工具插件为 IntelliJ 创意来创建和提交触发 Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)

* [使用 HDInsight 工具插件为 IntelliJ 创意来触发应用程序进行远程调试](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [在 HDInsight 上触发群集使用 Zeppelin 笔记本](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [内核可用于触发 HDInsight 群集中的 Jupyter 笔记本](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [外部包使用 Jupyter 笔记本](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [在您的计算机上安装 Jupyter 并连接到一个 HDInsight 触发的群集](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>管理资源

* [管理在 Azure HDInsight Apache 触发群集的资源](hdinsight-apache-spark-resource-manager.md)

* [跟踪和调试 HDInsight 在 Apache 触发群集上运行的作业](hdinsight-apache-spark-job-debugging.md)
