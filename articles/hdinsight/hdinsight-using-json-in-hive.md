<properties
   pageTitle="分析并处理 JSON 文档与 HDInsight 中的配置单元 |Microsoft Azure"
   description="了解如何使用 JSON 文档并对其进行分析使用 HDInsight 中的配置单元。"
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>处理和分析使用 HDInsight 中的配置单元的 JSON 文档

了解如何处理和分析 JSON 文件使用 HDInsight 中的配置单元。 在本教程中将使用下面的 JSON 文档

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

该文件可以位于wasbs://processjson@hditutorialdata.blob.core.windows.net/。 在 HDInsight 中使用 Azure Blob 存储的详细信息，请参阅[使用 Hadoop 在 HDInsight 中使用兼容 HDFS Azure Blob 存储](hdinsight-hadoop-use-blob-storage.md)。 如果需要，您可以将文件复制到群集的默认容器。

在本教程中，您将使用该配置单元控制台。  打开配置单元控制台的说明，请参阅[使用配置单元与 Hadoop 在 HDInsight 与远程桌面](hdinsight-hadoop-use-hive-remote-desktop.md)。

##<a name="flatten-json-documents"></a>平面化 JSON 文档

下一节中列出的方法要求的 JSON 文档中的单个行。 因此必须拼合到字符串的 JSON 文档。 如果 JSON 文档已被压扁的可以跳过此步骤并直接转到下一节对分析 JSON 数据。

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

原始的 JSON 文件位于**wasbs://processjson@hditutorialdata.blob.core.windows.net/**。 *StudentsRaw*配置单元表指向原始未平整的 JSON 文档。

*StudentsOneLine*配置单元表将**/json/学生/路径下，HDInsight 的默认文件系统中存储的数据。

INSERT 语句填充平展的 JSON 数据的 StudentOneLine 的表。

SELECT 语句应仅返回第 1 行。

以下是 SELECT 语句的输出︰

![拼合的 JSON 文档。][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>分析配置单元中的 JSON 文档

配置单元提供了三种不同的机制在 JSON 文档上运行查询︰

- 使用 GET\_JSON\_对象 UDF （用户定义的函数）
- 使用 JSON_TUPLE UDF
- 使用自定义的 SerDe
- 编写您自己使用 Python 或其他语言的 UDF。 [这篇文章]，请参阅[hdinsight-python]上运行的配置单元的 Python 代码。

### <a name="use-the-getjsonobject-udf"></a>使用 GET\_JSON_OBJECT UDF
配置单元提供了内置的 UDF 调用[获取 json 对象](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)的 JSON 查询在运行时可执行。 此方法采用两个参数--表名和方法名的平展的 JSON 文档和 JSON 字段需要进行分析的。 让我们看一个示例，请参阅此 UDF 的工作原理。

获得第一名和每个学生的姓

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

在控制台窗口中运行此查询时，这里是输出。

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

有几个限制获取 json_object UDF。

- 因为在查询中的每个字段都需要重新分析查询，它会影响性能。
- 获得\_JSON_OBJECT() 返回数组的字符串表示形式。 若要将该时间转换到配置单元阵列，您需要使用正则表达式来替换方括号 [和] 然后还调用来获取数组的拆分。


这是配置单元 wiki 建议使用 json_tuple 的原因。  

### <a name="use-the-jsontuple-udf"></a>使用 JSON_TUPLE UDF

通过配置单元提供的另一个 UDF 被称为[json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple)效果好于[get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)。 该方法采用一组键和 JSON 字符串，并返回使用一个函数值的元组。 下面的查询返回的 JSON 文档从学生 id 和成绩︰

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

此脚本在控制台中配置单元的输出︰

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_元组在配置单元使 json 使用[横向视图](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)语法\_元组通过将 UDT 函数应用到原始表的每一行创建的虚拟表。  复杂的 JSONs 太笨拙，这成为横向视图重复使用。 此外，JSON_TUPLE 无法处理嵌套的 JSONs。


###<a name="use-custom-serde"></a>使用自定义的 SerDe

SerDe 是最适合分析嵌套的 JSON 文档，它允许您定义 JSON 的架构，并使用该架构来分析文档。 在本教程中，您将使用一个由[rcongiu](https://github.com/rcongiu)更受欢迎 SerDe。

**若要使用自定义的 SerDe:**

1. 安装[Java SE 开发工具包 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR)。 如果您打算使用 HDInsight 的 Windows 部署，选择 Windows X64 版本的 jdk

    >[AZURE.WARNING] JDK 1.8 不能用此 SerDe。

    在安装完成后，添加新的用户环境变量︰

    1. 从 Windows 屏幕打开**查看高级系统设置**。
    2. 单击**环境变量**。  
    3. 添加一个新**JAVA_HOME**环境变量指向**C:\Program Files\Java\jdk1.7.0_55**或任何地方安装您的 JDK。

    ![JDK 的正确配置值的设置][image-hdi-hivejson-jdk]

2. 安装[Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    通过转到控件将 bin 文件夹添加到您的路径面板--> 编辑帐户 Environment 变量为系统变量。 下面的屏幕快照显示了如何执行此操作。

    ![Maven 设置][image-hdi-hivejson-maven]

3. 克隆[配置单元的 JSON SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github 网站中的项目。 你可以通过单击"下载 Zip"按钮，如下面的屏幕快照中所示。

    ![克隆项目][image-hdi-hivejson-serde]

4︰ 转到已下载此包和类型"mvn 包"的文件夹。 这将创建您可以再将复制到群集所必需的 jar 文件。

5︰ 转到您下载此包的根文件夹下的目标文件夹。 将 json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar 文件上载到您的群集的头节点。 我通常把它放在配置单元的二进制文件夹下︰ C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin 或类似。

6︰ 配置单元提示时，键入"添加 jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar"。 由于我的情况而言，罐是在 C:\apps\dist\hive-0.13.x\bin 文件夹中，可以直接添加名称的 jar 如下所示︰

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

现在，您就可以使用 SerDe 运行查询对 JSON 文档了。

下面的语句创建一个包含表定义的架构

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

列出的名字和姓氏的学生

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

下面是配置单元控制台的结果。

![SerDe 查询 1][image-hdi-hivejson-serde_query1]

若要计算的 JSON 文档的分数总和

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

上面的查询使用[横向视图分解](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)UDF 展开的分数数组，以便可以求和。

下面是配置单元控制台的输出。

![SerDe 查询 2][image-hdi-hivejson-serde_query2]

若要查找某个给定的学生有的得分超过 80 分主题选择  
      jt.StudentClassCollection.ClassId 从 json_table jt 横向视图分解 (jt.StudentClassCollection.Score) 集合作为分数，分数 > 80;

上面的查询返回与获取不同的配置单元阵列\_json\_返回字符串的对象。

![SerDe 查询 3][image-hdi-hivejson-serde_query3]

如果希望 skil 格式不正确的 JSON，然后此 SerDe 的[wiki 页面](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master)中所述您可以通过键入下面的代码来实现的︰  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>摘要
总之，您选择的配置单元中的 JSON 运算符的类型取决于您的方案。 如果您拥有一个简单的 JSON 文档，并且您只需一个字段来查找的 — 您可以选择使用配置单元 UDF get\_json\_对象。 如果您有多个键查找您可以使用 json_tuple。 如果有一个嵌套的文档时，您应使用 JSON SerDe。

其他相关文章，请参见

- [用于配置单元和 HiveQL 与 HDInsight 在 Hadoop 分析 Apache log4j 文件示例](hdinsight-use-hive.md)
- [使用 HDInsight 中的配置单元分析航班延迟数据](hdinsight-analyze-flight-delay-data.md)
- [Twitter 使用分析数据中 HDInsight 的配置单元](hdinsight-analyze-twitter-data.md)
- [运行 Hadoop 作业使用 DocumentDB 和 HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
