<properties 
    pageTitle="SQL 语法和 SQL 查询 DocumentDB |Microsoft Azure" 
    description="对于 DocumentDB，NoSQL 数据库了解 SQL 语法、 数据库概念和 SQL 查询。 SQL 可以用作 JSON 查询语言 DocumentDB 中。" 
    keywords="sql 语法、 sql 查询、 sql 查询、 json 查询语言、 数据库概念和 sql 查询，聚合函数"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL 查询和 DocumentDB 中的 SQL 语法
Microsoft Azure DocumentDB 支持查询的文档用作 JSON 查询语言 SQL (结构化查询语言)。 DocumentDB 是真正的无模式的。 由于其承诺直接在数据库引擎中的 JSON 数据模型，它提供了自动索引的 JSON 文档而无需显式模式或创建辅助索引。 

为 DocumentDB 设计的查询语言时，我们必须记住两个目标︰

-   而不是在发明新的 JSON 查询语言，我们希望支持 SQL。 SQL 是一种最熟悉和最常用的查询语言。 DocumentDB SQL 对 JSON 文档丰富查询提供正式的编程模型。
-   作为 JSON 文档数据库可以直接在数据库引擎中执行 JavaScript，我们希望为我们的查询语言为基础使用 JavaScript 的编程模型。 DocumentDB SQL 被植根于 JavaScript 的类型系统、 表达式求值和函数调用。 此中打开跨 JSON 文档、 自我联接、 空间查询和调用用户定义函数 (Udf) 完全用 JavaScript 编写的在其他功能的关系预测、 分层导航提供自然的编程模型。 

我们相信，这些功能是减少应用程序和数据库之间摩擦的关键和至关重要的开发人员的效率。

我们建议开始通过观看下面的视频，其中 Aravind Ramachandran 显示 DocumentDB 的查询功能，并通过访问我们[查询运动场](http://www.documentdb.com/sql/demo)，可以在此试用 DocumentDB，并对我们的数据集执行 SQL 查询。

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

然后，返回到本文中，我们将开始与 SQL 查询教程来指导您完成一些简单的 JSON 文档和 SQL 命令。

## <a name="getting-started-with-sql-commands-in-documentdb"></a>要开始使用 DocumentDB 中的 SQL 命令
要查看 SQL DocumentDB 在工作，让我们开始与几个简单的 JSON 文档并引导反对这样做一些简单的查询。 考虑这些两个的 JSON 文档有关的两个系列。 请注意，使用 DocumentDB，我们不需要显式创建的任何架构或辅助索引。 我们只需要插入到 DocumentDB 集合的 JSON 文档并随后查询。 现在，我们有简单的 JSON 文档为莹系列、 父母、 孩子 （和他们的宠物），地址和注册信息。 文档包含字符串、 数字、 布尔值、 数组和嵌套的属性。 

**文档**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


这里是第二个文档有一个细微的差异 –`givenName`和`familyName`而不是使用`firstName`， `lastName`。

**文档**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



现在让我们来尝试几个查询这些数据，可以了解一些 DocumentDB SQL 的关键方面。 例如，以下查询将返回文档 id 字段与匹配之处`AndersenFamily`。 因为它是`SELECT *`，查询的输出结果是完整的 JSON 文档︰

**查询**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


现在考虑这种情况，我们需要重新格式化形状不同的 JSON 输出。 地址的城市具有相同的名称作为状态时，此查询项目具有两个选定字段的名称和市/县，一个新的 JSON 对象。 在这种情况下，"纽约州，NY"匹配。

**查询**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**结果**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


下一个查询返回其 id 匹配的系列中的子级的所有给定名称`WakefieldFamily`按居住的城市。

**查询**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**结果**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


我们想要吸引人们关注几个方面值得注意 DocumentDB 查询语言，到目前为止我们看到的示例︰  
 
-   DocumentDB SQL 使用 JSON 值的因为它涉及树形状而不是行和列的实体。 因此，该语言可以像引用任何任意深度的树中的节点`Node1.Node2.Node3…..Nodem`、 相似关系的两个零件参照引用的 SQL `<table>.<column>`。   
-   结构化的查询语言的合作架构的数据。 因此，需要动态绑定类型系统。 相同的表达式可能产生不同的文档的不同类型。 查询的结果 JSON 是有效值，但不是能保证是一个固定的架构。  
-   DocumentDB 只支持严格的 JSON 文档。 这意味着类型系统和表达式被限制为只处理 JSON 类型。 请对[JSON 规范](http://www.json.org/)有关更多详细信息，参阅。  
-   DocumentDB 集合是免架构容器的 JSON 文档。 在虚拟机内和跨文档集合中的数据实体中的关系是隐式捕获的包容而不是主键和外键关系。 这是一个重要的方面，需要指出根据下文所讨论的文档内联接。

## <a name="documentdb-indexing"></a>DocumentDB 索引

我们在 DocumentDB SQL 语法之前，值得探索中 DocumentDB 的索引设计。 

数据库索引的目的是提供其各种形式和形状的查询服务 （如 CPU 和输入/输出） 的最小的资源消耗的同时提供了很好的吞吐量和滞后时间。 通常情况下，选择正确的索引对于查询数据库的要求量计划和试验。 这种方法会带来架构灵活的数据库，其中的数据并不符合严格的架构和迅速发展一个的难题。 

因此，当我们设计索引子系统 DocumentDB，我们设置了下列目标︰

-   索引文档而无需架构︰ 索引子系统并不要求任何架构信息或做出任何假设架构的文档。 

-   支持高效、 丰富的具有层次结构，并且关系查询︰ 索引支持 DocumentDB 查询语言有效地，其中包括层次结构和关系的预测支持。

-   对换持续卷的写入操作的一致性查询支持︰ 较高写入吞吐量工作负载，使用一致的查询，索引更新联机增量、 高效并且面对持续卷的写入操作。 一致的索引更新至关重要提供一致性级别的用户配置文档服务查询服务。

-   支持多租户︰ 给定保留基于模型的资源治理跨租户，分配每个复制副本的系统资源 （CPU、 内存和每秒输入/输出操作） 的预算内执行索引更新。 

-   存储效率︰ 索引的磁盘上的存储开销的成本效益是有限和可预测。 这是至关重要的因为 DocumentDB 允许开发人员做出基于成本权衡开销与查询性能的索引。  

示例演示了如何配置集合的索引策略，请参阅 MSDN 上的[DocumentDB 示例](https://github.com/Azure/azure-documentdb-net)。 让我们现在进入的 DocumentDB SQL 语法的详细信息。


## <a name="basics-of-a-documentdb-sql-query"></a>DocumentDB SQL 查询的基础知识
每个查询包含 SELECT 子句和可选的 FROM 和每个 ANSI SQL 标准的 WHERE 子句。 通常，对于每个查询中，枚举在 FROM 子句中的源。 然后在 WHERE 子句中的筛选器应用于要检索的 JSON 文档子集的源。 最后，SELECT 子句用于请求的 JSON 值选择列表中的项目。
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM 子句
`FROM <from_specification>`子句是可选的除非源进行过滤或以后查询中提取的。 本节的目的是指定必须在其运行查询的数据源。 整个集合通常是源，但可以改为指定集合的子集。 

如查询`SELECT * FROM Families`表示，整个系列集合是要对其进行枚举源。 可以使用特殊标识符根来表示而不被使用集合名称的集合。 下面的列表包含强制执行每个查询的规则︰

- 该集合可以是别名，如`SELECT f.id FROM Families AS f`或只是`SELECT f.id FROM Families f`。 这里`f`相当于`Families`。 `AS`是一个可选的关键字与别名的标识符。

-   注意，一旦化名，原始数据源不能绑定。 例如，`SELECT Families.id FROM Families f`在语法上无效，因为无法再解析的标识符"家族"。

-   所有需要引用的属性必须是完全限定的。 如果没有遵守严格的架构，这被实施以避免任何不明确的绑定。 因此，`SELECT id FROM Families f`是语法无效，因为该属性`id`未绑定。
    
### <a name="sub-documents"></a>子文档
此外可以给一小部分减少了源。 例如，枚举子树中每个文档，到子根可能会源，如下面的示例中所示。

**查询**

    SELECT * 
    FROM Families.children

**结果**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

虽然上面的示例中使用的源数组，对象可能还用作源，它是在下面的示例中显示的内容。 任何有效 JSON 值 （未定义），可以在源代码中找到将被视为包含在查询的结果。 如果没有一些系列`address.state`的值，将在查询结果中排除。

**查询**

    SELECT * 
    FROM Families.address.state

**结果**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE 子句
WHERE 子句 (**`WHERE <filter_condition>`**) 是可选的。 它指定必须满足的 JSON 文档源提供的条件是结果的一部分。 任何 JSON 文档必须评估所指定的条件为"true"要考虑的结果。 为了确定可以是结果的一部分的源文档的绝对最小子索引图层使用 WHERE 子句。 

下面的查询请求文档包含名称属性值是`AndersenFamily`。 没有一个名称属性中，任何其他文档或值不符合`AndersenFamily`被排除在外。 

**查询**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


前面的示例显示一个简单的相等查询。 DocumentDB SQL 还支持各种标量表达式。 最常用的有二元和一元表达式。 属性引用的源 JSON 对象中也是有效的表达式。 

下面的二进制运算符目前支持，如下面的示例中所示，可以在查询中使用︰  
<table>
<tr>
<td>算术运算</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>按位</td>    
<td>|，&，^，<<, >>，>>> （零填充右移） </td>
</tr>
<tr>
<td>逻辑</td>
<td>和，或不</td>
</tr>
<tr>
<td>比较</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>字符串</td> 
<td>||（串联）</td>
</tr>
</table>  

让我们看一看一些使用二元运算符的查询。

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


一元运算符 +，-，~ 不也受支持，并可在查询内如下面的示例所示︰

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



除了二元和一元运算符，也允许属性引用。 例如，`SELECT * FROM Families f WHERE f.isRegistered`将返回包含该属性的 JSON 文档`isRegistered`属性的值等于 JSON`true`值。 任何其他值 (假的、 空的、 未定义的`<number>`， `<string>`， `<object>`，`<array>`等) 会导致从结果中排除的源文档。 

### <a name="equality-and-comparison-operators"></a>相等性和比较运算符
下表显示的相等比较的结果在 DocumentDB SQL 中任何两个的 JSON 类型之间。
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>操作</strong>
         </td>
         <td valign="top">
            <strong>未定义</strong>
         </td>
         <td valign="top">
            <strong>空值</strong>
         </td>
         <td valign="top">
            <strong>布尔值</strong>
         </td>
         <td valign="top">
            <strong>编号</strong>
         </td>
         <td valign="top">
            <strong>字符串</strong>
         </td>
         <td valign="top">
            <strong>对象</strong>
         </td>
         <td valign="top">
            <strong>数组</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>未定义<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>空值<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>布尔值<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>编号<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>字符串<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>对象<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
         <td valign="top">
未定义 </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>数组<strong>
         </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
未定义 </td>
         <td valign="top">
            <strong>还行</strong>
         </td>
      </tr>
   </tbody>
</table>

如其他比较运算符 >，> =、 ！ =，< 和 < =，以下规则适用︰   

-   未定义不同类型的比较结果。
-   比较两个或两个数组中未定义的结果。   

如果筛选器中的标量表达式的结果是未定义的相应的文档将不会包含在结果中，因为未定义逻辑上不能等同于"true"。

### <a name="between-keyword"></a>关键字之间
您可以使用 BETWEEN 关键字来表达对中的值范围如 ANSI SQL 查询。 之间可用于对字符串或数字。

例如，此查询返回所有系列文档的第一个孩子的成绩是 1-5 （均包括） 之间。 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

与在 ANSI SQL 的您还可以使用 BETWEEN 子句如下面的示例在 FROM 子句中。

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

更快的查询执行时间，请记住创建索引创建策略的使用对任何数值属性/路径 BETWEEN 子句中筛选出的范围索引类型。 

使用 BETWEEN DocumentDB 和 ANSI SQL 中的主要区别是可以表达对混合类型的属性范围查询 — 例如，可能有"成绩"是数字 (5) 一些文档和其他人 ("grade4") 中的字符串中。 在这些情况下，如在 JavaScript 中，在"未定义"的两种不同类型结果的文档之间的比较将被跳过。

### <a name="logical-and-or-and-not-operators"></a>逻辑 (AND、 OR 和 NOT) 运算符
逻辑运算符对布尔值进行操作。 对于这些运算符逻辑事实表下表所示。

OR|真|假|未定义
---|---|---|---
真|真|真|真
假|真|假|未定义
未定义|真|未定义|未定义

和|真|假|未定义
---|---|---|---
真|真|假|未定义
假|假|假|假
未定义|未定义|假|未定义

不|  |
---|---
真|假
假|真
未定义|未定义

### <a name="in-keyword"></a>关键词
IN 关键字可用于检查是否与指定的值匹配列表中的任何值。 例如，此查询将返回家族的所有文档 id，是一个"WakefieldFamily"或"AndersenFamily"。 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

此示例返回所有文档状态所在的任何指定的值。

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>三元 （？） 和联合 （？） 运算符
三元和联合运算符可用于构建类似于流行的编程语言，如 C# 和 JavaScript 的条件表达式。 

构建新动态的 JSON 属性时，可以非常方便 （？） 三元运算符。 例如，现在您可以编写查询以将它们归类人类可读的形式如以下所示初级/中级/高级类级别。
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

您还可以嵌套到等在下面的查询运算符的调用。
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

作为与其他查询运算符，如果缺少在任何文档中，条件表达式中引用的属性，或进行比较的类型是不同的然后这些文档将被排除在查询结果中。

联合 （？） 运算符可以用于有效地 （也检查属性存在 已定义） 的文档中。 这是有用的针对半结构化查询时或混合类型的数据。 例如，此查询返回"姓氏"（如果存在） 或"姓氏"如果它不存在。

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>带引号的属性访问器
您还可以访问属性使用带引号的属性运算符`[]`。 例如， `SELECT c.grade` ，`SELECT c["grade"]`是等效的。 当需要进行转义包含空格，特殊字符，或碰巧共享 SQL 关键字或保留的字的名称相同的属性，此语法非常有用。

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT 子句
SELECT 子句 (**`SELECT <select_list>`**) 是必需的指定的值将从查询检索，只是喜欢在 ANSI SQL。 已筛选在源文档的子集被传递到投影阶段，其中检索指定的 JSON 值，构造新的 JSON 对象传递到其上的每个输入的绘图。 

下面的示例演示一个典型的选择查询。 

**查询**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>嵌套的属性
我们将在下面的示例中，将投影两个嵌套的属性`f.address.state`， `f.address.city`。

**查询**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


投影还支持 JSON 表达式，如下面的示例中所示。

**查询**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


让我们看一下角色的`$1`在此处。 `SELECT`子句需要创建一个 JSON 对象，并不提供任何密钥，因为我们使用隐式参数变量名称开头`$1`。 例如，此查询将返回两个隐式参数变量，标记为`$1`， `$2`。

**查询**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>别名
现在让我们扩展上面的示例使用显式别名的值。 一样是别名使用的关键字。 请注意，它是可选时将作为第二个值的投影所示`NameInfo`。 

以防查询有两个具有相同名称的属性，必须使用别名来重命名一个或两个属性，以便消除他们预计结果中的歧义。

**查询**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>标量表达式
除了属性引用 SELECT 子句也支持如常量、 算术表达式、 逻辑表达式等标量表达式。例如，下面是一个简单的"Hello World"查询。

**查询**

    SELECT "Hello World"

**结果**

    [{
      "$1": "Hello World"
    }]


下面是一个更复杂的示例，使用标量表达式。

**查询**

    SELECT ((2 + 11 % 7)-2)/3   

**结果**

    [{
      "$1": 1.33333
    }]


在以下示例中，标量表达式的结果是一个 boolean 值。

**查询**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**结果**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>对象和数组创建
DocumentDB SQL 的另一个关键功能是创建数组/对象。 在前面的示例中，请注意，我们创建了一个新的 JSON 对象。 同样，一个可以还构造数组，如下面的示例中所示。

**查询**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**结果**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>关键字值
**值**关键字使您能够返回 JSON 值。 例如，如下所示的查询将返回标量`"Hello World"`而不是`{$1: "Hello World"}`。

**查询**

    SELECT VALUE "Hello World"

**结果**

    [
      "Hello World"
    ]


下面的查询返回的 JSON 值而无需`"address"`在结果中的标签。

**查询**

    SELECT VALUE f.address
    FROM Families f 

**结果**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

下面的示例进行扩展以显示如何返回 JSON 基元值 （JSON 树的叶级别）。 

**查询**

    SELECT VALUE f.address.state
    FROM Families f 

**结果**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* 运算符
特殊的运算符 （*） 支持以项目作为文档-是。 使用时，它必须是唯一的计划的字段。 而类似的查询`SELECT * FROM Families f`是有效的`SELECT VALUE * FROM Families f `，`SELECT *, f.id FROM Families f `是无效。

**查询**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**结果**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>TOP 运算符
TOP 关键字可用于限制查询中的值的数目。 结果集与 ORDER BY 子句一起使用时顶部，仅限于第 N 值的个数有序;否则，它返回前的 N 个结果中未定义的顺序。 作为最佳实践，在 SELECT 语句中，总是使用 ORDER BY 子句 TOP 子句。 这是预知指示哪些行受顶部的唯一办法。 


**查询**

    SELECT TOP 1 * 
    FROM Families f 

**结果**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

可以使用顶部，与一个常量值 （如上所示） 或使用参数化的查询变量值。 有关详细信息，请参阅下面的参数化的查询。

## <a name="order-by-clause"></a>ORDER BY 子句
正如在 ANSI SQL 的可以在查询时包括一个可选的 Order By 子句。 子句还可以包括一个可选的升序/降序参数，以指定必须在其中检索结果的顺序。 在 Order By 更详细地了解，请参阅[DocumentDB 订单通过演练](documentdb-orderby.md)。

例如，下面是检索系列按顺序排列的驻留的城市名称的查询。

**查询**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**结果**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

和这里查询检索的创建日期，将被视为代表新纪元的数字顺序的系列的时间，例如，运行时间自 1970 年 1 月 1 日在秒。

**查询**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**结果**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>高级的数据库概念和 SQL 查询
### <a name="iteration"></a>迭代
通过**IN**关键字 DocumentDB SQL 提供支持的迭代 JSON 数组中添加新的构造。 人源迭代提供支持。 让我们开始与下面的示例︰

**查询**

    SELECT * 
    FROM Families.children

**结果**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

现在让我们看一下另一个对集合中的子项执行迭代的查询。 请注意输出数组中的差异。 本示例拆分`children`并将结果合并到单个阵列中。  

**查询**

    SELECT * 
    FROM c IN Families.children

**结果**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

这可进一步用于筛选数组的每个个人项目上，如下面的示例中所示。

**查询**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**结果**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>加入
在关系数据库中，需要进行跨表连接是非常重要的。 它是逻辑的必然结果对设计标准化的架构。 与此相反，DocumentDB 处理免架构文档的非规范化的数据模型。 这是逻辑相当于一个"自联接"。

语言支持的语法是 < from_source1 > < from_source2 > 联接联接......加入 < from_sourceN >。 总之，这将返回**N**元组 （ **N**值的元组） 一套。 每个元组已通过其各自的集循环访问所有集合别名生成值。 换句话说，这是完全参与联接的集合的矢量积。

下面的示例演示使用 JOIN 子句的工作原理。 在下面的示例中，自个源中的每个文档的叉乘积结果为空，空集是空的。

**查询**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**结果**  

    [{
    }]


在以下示例中，连接是之间的文档根目录和`children`子根。 它是两个 JSON 对象之间跨产品。 儿童是一个数组并不有效联接中因为我们正在处理一个根的子数组。 因此结果包含只有两个结果，因为在阵列中的每个文档的叉积产生正好只有一个文档。

**查询**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**结果**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


下面的示例演示一个更传统的联接︰

**查询**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**结果**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



首先要注意的是`from_source`的**联接**子句是一个迭代器。 因此，流在本例情况如下︰  

-   展开每个数组中的子元素**c** 。
-   应用矢量积与每个子元素中的第一步平面化的**c** **f**文档的根。
-   最后，项目本身的根对象**f** name 属性。 

第一个文档 (`AndersenFamily`) 包含只有一个子元素，因此结果集包含只与此文档相对应的单个对象。 第二个文档 (`WakefieldFamily`) 包含两个孩子。 因此，跨产品生成一个单独的对象为每个孩子，从而导致两个对象，一个用于此文档相对应的每个孩子。 请注意，两个这些文档中的根域相同，就像希望跨产品。

真正的实用程序的连接的形状，否则很难对项目中的矢量积中为窗体元。 此外，正如我们将看到在下面的示例，您可以筛选元组允许的用户选择整体由元组满足某个条件的组合。

**查询**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**结果**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



本示例是前面的示例中的自然扩展，并执行双联接。 因此，跨产品可以看作下面的伪代码。

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`有一个孩子有一个宠物。 因此，跨产品产生一行 (1*1*1) 从本族。 WakefieldFamily，但是有两个孩子，但只有一个子"Jesse"宠物。 Jesse 虽然有 2 宠物。 因此跨产品从本族产生 1*1*2 = 2 行。

在下一个示例中，一个附加的筛选器上没有`pet`。 这不包括所有的宠物名字不"阴影"的元组。 请注意，我们能够生成元组阵列，筛选器上任何元素的元组，和项目元素的任何组合。 

**查询**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**结果**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript 的集成
DocumentDB 提供用于直接在存储的过程和触发器的集合上执行基于的 JavaScript 应用程序逻辑的编程模型。 这样两个︰

-   要做高性能事务 CRUD 操作和 JavaScript 运行时直接在数据库引擎中的深度集成通过集合中的文档与查询的能力。 
-   控制流、 可变范围和分配以及集成的异常处理与数据库事务基元的自然建模。 关于 JavaScript 集成 DocumentDB 支持的详细信息，请参阅 JavaScript 服务器端编程文档。

###<a name="user-defined-functions-udfs"></a>用户定义的函数 (Udf)
在这篇文章中已定义的类型，以及 DocumentDB SQL 提供支持的用户定义函数 (UDF)。 特别是，在其中开发人员可以在零个或多个参数传递和返回单个参数结果回支持标量 Udf。 这些参数被合法 JSON 值来检查。  

DocumentDB SQL 语法扩展以支持使用这些用户定义函数的自定义应用程序逻辑。 Udf 可以使用 DocumentDB 进行注册，然后被引用作为 SQL 查询的一部分。 事实上，Udf 精心设计的查询进行调用。 作为这种选择的必然结果，Udf 不具有访问其他 JavaScript 类型 （存储的过程和触发器） 的上下文对象。 以只读方式执行查询，因为它们可以运行主或辅助副本。 因此，Udf 旨在与其他 JavaScript 类型不同的辅助副本上运行。

下面是如何 UDF 可以注册在 DocumentDB 数据库，特别是下一个文档集合的示例。

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
前面的示例创建的 UDF 名称为`REGEX_MATCH`。 它接受两个 JSON 字符串值`input`， `pattern` ，并检查如果第一个匹配该模式指定第二个使用 JavaScript 的 string.match() 函数。


我们现在可以在查询中的投影使用此 UDF。 Udf 必须限定使用区分大小写的前缀"udf。" 当从查询中调用。 

>[AZURE.NOTE] 3/17/2015年前 DocumentDB 支持 UDF 调用，而无需"udf。" 如选择 REGEX_MATCH() 作为前缀。 此调用模式已被否决。  

**查询**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**结果**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF 也可在一个筛选器如下所示在以下示例中，还具备"udf。" 前缀︰

**查询**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**结果**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


从本质上讲，Udf 是有效的标量表达式，可用于预测和筛选器。 

若要展开对能力的 Udf，让我们看一下另一个示例使用条件逻辑︰

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
下面是一个示例执行的 UDF。

**查询**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**结果**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


如前面的示例中展示，Udf 集成 DocumentDB SQL 提供丰富的可编程接口来执行复杂的过程、 条件逻辑装 JavaScript 运行时功能的帮助下 JavaScript 语言的强大功能。

DocumentDB SQL 参数 udf 的源中的每个文档在提供处理该 UDF 的当前阶段 （WHERE 子句或 SELECT 子句）。 结果被无缝地结合在整体执行管道。 如果属性引用了由 UDF 参数在中不可用的 JSON 值、 参数均被视为未定义的因此该 UDF 调用将完全跳过。 同样如果 UDF 的结果是未定义的它不是包括在结果中。 

总之，Udf 是强大的工具，为复杂的业务逻辑作为查询的一部分。

### <a name="operator-evaluation"></a>运算符计算
DocumentDB，美德的 JSON 数据库中，通过绘制使用 JavaScript 运算符和计算语义的平行线。 DocumentDB 试图保留 JavaScript 语义在 JSON 支持方面，虽然在某些情况下将偏离操作评估。

DocumentDB SQL 中与传统 SQL，在值类型通常不称为直到实际从数据库中检索值。 为了有效地执行查询，大多数运算符具有严格的类型要求。 

DocumentDB SQL 不执行隐式转换，与不同的 JavaScript。 例如，查询如下所示`SELECT * FROM Person p WHERE p.Age = 21`匹配包含其值为 21 Age 属性的文档。 其 Age 属性匹配字符串"21"或其他可能无限变化的任何其他文档"021"、"21.0"、"0021"、"00021"，将不匹配等。 这是相反对字符串值是隐式强制转换为数字 JavaScript (根据运算符，例如: = =)。 这种选择是关键匹配 DocumentDB SQL 中的有效索引。 

## <a name="parameterized-sql-queries"></a>参数化的 SQL 查询
DocumentDB 支持查询参数表示与熟悉@表示法。 参数化的 SQL 提供了可靠的处理和转义的用户输入，防止意外泄露的数据通过 SQL 注入。 

例如，可以编写一个查询，将姓氏和地址状态作为参数，并执行各种姓氏和地址状态基于用户输入的值。

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

此请求可以再发送到 DocumentDB 作为 JSON 参数化查询类似如下所示。

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

顶部的参数可以设置使用参数化的查询类似如下所示。

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

参数值可以是任何有效的 JSON (字符串、 数字、 布尔值为 null，甚至是数组或嵌套 JSON)。 也因为 DocumentDB 是架构较少，对任何类型不验证参数。

##<a name="built-in-functions"></a>内置函数
DocumentDB 还支持常见的操作，如用户定义函数 (Udf) 查询内使用大量的内置函数。

<table>
<tr>
<td>数学函数</td> 
<td>ABS、 天花板、 EXP、 地板、 日志、 LOG10、 电源、 倒圆角、 符号、 SQRT、 方形、 TRUNC、 ACOS、 ASIN、 ATAN、 ATN2、 COS，COT、 度、 PI、 弧度，SIN、 和 TAN</td>
</tr>
<tr>
<td>类型检查功能</td>    
<td>IS_ARRAY、 IS_BOOL、 IS_NULL、 IS_NUMBER、 IS_OBJECT、 IS_STRING、 IS_DEFINED 和 IS_PRIMITIVE</td>
</tr>
<tr>
<td>字符串函数</td>   
<td>CONCAT、 包含、 ENDSWITH、 INDEX_OF、 左、 长度、 较低、 LTRIM、 替换、 复制、 反向、 右、 RTRIM、 STARTSWITH、 子字符串和左上</td>
</tr>
<tr>
<td>数组函数</td>    
<td>ARRAY_CONCAT、 ARRAY_CONTAINS、 ARRAY_LENGTH 和 ARRAY_SLICE</td>
</tr>
<tr>
<td>空间的函数</td>  
<td>ST_DISTANCE、 ST_WITHIN、 ST_ISVALID 和 ST_ISVALIDDETAILED</td>
</tr>
</table>  

如果您当前使用的用户定义的函数 (UDF) 为其目前可用的内置函数，因为它将运行速度更快，则应使用相应的内置功能，效率更高。 

### <a name="mathematical-functions"></a>数学函数
数学函数每个执行计算，通常基于输入的值作为参数，并返回一个数字值。 下面是支持内置数学函数的表。

<table>
<tr>
<td><strong>使用</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>返回指定数值表达式的绝对 （正） 值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">天花板上 (num_expr)</a></td> 
<td>大于或等于指定数值表达式，可返回的最小整数值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">基底 (num_expr)</a></td> 
<td>返回小于或等于指定数值表达式的最大整数。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>返回指定数值表达式的指数。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">日志 （num_expr [、 基]）</a></td> 
<td>返回指定数值表达式，或使用指定的底的对数的自然对数</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>返回指定数值表达式的以 10 为基数的对数值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">倒圆角 (num_expr)</a></td> 
<td>返回一个舍入到最接近的整数值的数值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>返回一个数字值，截断为最接近的整数值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">SQRT (num_expr)</a></td>   
<td>返回指定数值表达式的平方根。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">方块 (num_expr)</a></td>   
<td>返回指定数值表达式的平方。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">电源 （num_expr，num_expr）</a></td>   
<td>为指定的值返回指定数值表达式的幂。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">号 (num_expr)</a></td>   
<td>返回指定数值表达式的符号值 （-1，0，1）。</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>返回的角度以弧度为单位，其余弦为指定的数值表达式;也称为反余弦值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>返回的角度以弧度为单位，其正弦为指定数值表达式。 这也称为反正弦。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>返回的角度以弧度为单位，其正切值是指定的数值表达式。 这也称为反正切值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>返回的角度以弧度为单位，从原点到点 （y，x） 的射线正 x 轴之间在 x 和 y 是两个指定的浮点表达式的值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>返回以弧度为单位，指定表达式中的指定角度的三角余弦。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>返回以弧度为单位，指定数值表达式的指定角度的三角余切。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">度 (num_expr)</a></td> 
<td>返回相应的角度以弧度表示的指定角度的度数。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI （)</a></td>   
<td>返回 PI 的常数值。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">弧度 (num_expr)</a></td> 
<td>数值表达式，以度为单位输入时，返回弧度。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>返回以弧度为单位，指定表达式中的指定角度的三角正弦。</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>返回指定表达式中的输入表达式的正切值。</td>
</tr>

</table> 

例如，您现在可以运行类似下面的查询︰

**查询**

    SELECT VALUE ABS(-4)

**结果**

    [4]

与 ANSI SQL DocumentDB 的函数之间的主要区别是它们设计与架构较少和混合模式数据很好地工作。 例如，如果您有一个文档位置的大小属性已丢失，或一个非数字值如"未知"，则文档跳过，而不是返回一个错误。

### <a name="type-checking-functions"></a>类型检查功能
类型检查函数允许您检查 SQL 查询中的表达式的类型。 类型检查函数可用于确定动态文档中的属性的类型变量或未知时。 以下是受支持的内置类型检查功能的表。

<table>
<tr>
  <td><strong>使用</strong></td>
  <td><strong>说明</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></td>
  <td>返回布尔值，是否该值的类型一个数组。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></td>
  <td>返回布尔值，是否值的类型为布尔值。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></td>
  <td>返回一个布尔值，指示是否为 null 值的类型。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></td>
  <td>返回布尔值，是否该值的类型一个数字。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></td>
  <td>返回布尔值，是否该值的类型一个 JSON 对象。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></td>
  <td>返回布尔值，是否该值的类型字符串。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></td>
  <td>返回布尔值，是否该属性分配一个值。</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></td>
  <td>返回布尔值，是否该值的类型字符串、 数字、 布尔值或空值。</td>
</tr>

</table>

您现在可以使用这些函数，运行查询，如下所示︰

**查询**

    SELECT VALUE IS_NUMBER(-4)

**结果**

    [true]

### <a name="string-functions"></a>字符串函数
下面的标量函数对字符串输入值执行运算，返回字符串、 数字或布尔值。 这里是内置的字符串函数的列表︰

使用|说明
---|---
[长度 (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|返回指定的字符串表达式的字符数
[CONCAT （str_expr、 str_expr [，str_expr]）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|返回一个字符串，表示连接两个或多个字符串值的结果。
[（str_expr、 num_expr、 num_expr） 的子字符串](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|返回字符串表达式的一部分。
[STARTSWITH （str_expr，str_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|返回一个 boolean 类型的值，该值指示是否第一个字符串表达式，与第二个结束
[ENDSWITH （str_expr，str_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|返回一个 boolean 类型的值，该值指示是否第一个字符串表达式，与第二个结束
[包含 （str_expr，str_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|返回一个 boolean 类型的值，该值指示是否第一个字符串表达式，包含第二个。
[INDEX_OF （str_expr，str_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|返回第二个首次出现的起始位置字符串表达式中第一个指定的字符串表达式，则为-1，如果找不到该字符串。
[左 （str_expr，num_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|返回指定数目的字符的字符串的左侧的部分。
[向右 （str_expr，num_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|返回指定数目的字符的字符串的右端。
[LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|删除前导空格后返回的字符串表达式。
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|返回截断所有尾随空格后的字符串表达式。
[较低 (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|返回后将大写字符数据转换为小写的字符串表达式。
[上限 (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|返回将小写字符数据转换为大写后的字符串表达式。
[替换 （str_expr、 str_expr、 str_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|与另一个字符串值替换所有出现的指定的字符串值。
[复制 （str_expr，num_expr）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|将一个字符串值，重复指定的次数。
[反向 (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|返回一个字符串值的反向顺序。

使用这些功能，现在可以运行类似下面的查询。 例如，可以返回系列名称大写，如下所示︰

**查询**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**结果**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

或如在此示例中的连接字符串︰

**查询**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**结果**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


此外可以在 WHERE 子句来筛选结果，如下面的示例中使用字符串函数︰

**查询**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**结果**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>数组函数
下面的标量函数执行数组输入的值和返回数字、 布尔值或数组值上的操作。 这里是内置数组函数的列表︰

使用|说明
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|返回指定表达式的元素的数目。
[ARRAY_CONCAT arr_expr，arr_expr [(arr_expr]）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|返回一个数组，是连接两个或多个数组值的结果。
[ARRAY_CONTAINS （arr_expr，表达式）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|返回布尔值，该值指示数组中是否包含指定的值。
[ARRAY_SLICE arr_expr，num_expr [(num_expr]）](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|返回数组表达式的一部分。

数组函数可以用于操作在 JSON 数组。 例如，下面是一个查询，返回的所有文档，其中父母之一是"复用 Wakefield"。 

**查询**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**结果**

    [{
      "id": "WakefieldFamily"
    }]

这里是另一个示例使用 ARRAY_LENGTH 来获取每个家庭的孩子数。

**查询**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**结果**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>空间的函数

DocumentDB 支持地理空间查询以下开放地理空间联盟 (OGC) 内置函数。 在地理空间上的更多详细信息在 DocumentDB 支持，请参阅[使用 Azure DocumentDB 在地理空间数据](documentdb-geospatial.md)。 

<table>
<tr>
  <td><strong>使用</strong></td>
  <td><strong>说明</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE （point_expr，point_expr）</td>
  <td>返回两个 GeoJSON 点表达式之间的距离。</td>
</tr>
<tr>
  <td>ST_WITHIN （point_expr，polygon_expr）</td>
  <td>返回指示第一个参数中指定的 GeoJSON 点是否位于第二个参数中的 GeoJSON 多边形内的布尔表达式。</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>返回一个布尔值，该值指示指定的 GeoJSON 点或多边形表达式是否有效。</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>返回一个 JSON 值，包含一个布尔值并指定的 GeoJSON 点或多边形表达式是否有效，如果无效，此外作为字符串值的原因。</td>
</tr>
</table>

空间的函数可用于执行针对空间数据的接近 querries。 例如，下面是一个查询，返回不超过 30 公里的 ST_DISTANCE 内置函数使用指定位置的所有系列文档。 

**查询**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**结果**

    [{
      "id": "WakefieldFamily"
    }]

如果包括空间索引在索引策略，然后"距离查询"提供有效地通过索引。 对空间索引的详细信息，请参阅下面的部分。 如果没有为指定的路径对空间索引，仍然可以通过指定执行空间查询`x-ms-documentdb-query-enable-scan`请求标头值设置为"true"。 在.NET 中，这可以通过将可选的**FeedOptions**参数传递给查询[EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery)设置为 true。 

ST_WITHIN 可用于检查是否点位于多边形。 多边形常用来表示如 zip 代码、 状态边界或自然形态的边界。 再次如果包括空间索引在索引策略，然后"内"查询提供有效地通过索引。 

ST_WITHIN 中的多边形参数可以包含单声响铃，即多边形不能包含在它们的孔。 检查点 ST_WITHIN 查询一个多边形中允许的最大数目为[DocumentDB 限制](documentdb-limits.md)。

**查询**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**结果**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] 类似于在 DocumentDB 查询，如果既参数格式不正确或无效，则它的计算结果为**不明确**的和计算的文档，可以跳过从查询结果中指定的位置值的方式不匹配类型作品。 如果查询未不返回任何结果，请运行 ST_ISVALIDDETAILED 到调试 spatail 类型无效的原因。     

ST_ISVALID 和 ST_ISVALIDDETAILED 可以用来检查空间对象是否有效。 例如，以下查询检查点的有效性不足的区域的纬度值 (-132.8)。 ST_ISVALID 返回的只是一个布尔值，ST_ISVALIDDETAILED 返回布尔值和一个字符串，包含为何被认为无效的原因。

**查询**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**结果**

    [{
      "$1": false
    }]

这些函数也可用于验证的多边形。 例如，在这里我们使用 ST_ISVALIDDETAILED 来验证未闭合的多边形。 

**查询**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**结果**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
对于 DocumentDB 包装空间的函数，这些函数和 SQL 语法。 现在让我们看看如何 LINQ 查询的工作原理和它如何与语法交互我们看到目前为止。

## <a name="linq-to-documentdb-sql"></a>LINQ to SQL DocumentDB
LINQ 是表示作为查询的对象的流上计算一个.NET 编程模型。 DocumentDB 提供了客户端通过促进从 LINQ 的子集映射 JSON 和.NET 对象之间的转换与 linq 结合使用的接口库查询 DocumentDB 查询。 

下图显示了支持 LINQ 查询使用 DocumentDB 的体系结构。  使用 DocumentDB 客户端，开发人员可以创建**IQueryable**对象直接查询 DocumentDB 查询提供程序，然后将 LINQ 查询转换成 DocumentDB 查询。 然后，查询被传递给 DocumentDB 服务器以检索以 JSON 格式的结果集。 结果返回到客户端上的.NET 对象的流反序列化。

![体系结构支持 LINQ 查询使用 DocumentDB-SQL 语法、 JSON 查询语言、 数据库概念和 SQL 查询][1]
 


### <a name="net-and-json-mapping"></a>.NET 和 JSON 映射
.NET 对象到 JSON 文档之间的映射是自然-每个数据成员的字段映射到一个 JSON 对象，其中的字段名称映射到对象的"密钥"部分，"值"部分是以递归方式映射到对象的值部分。 请考虑以下示例。 创建家族对象映射到 JSON 文档如下所示。 并且，反之亦然，JSON 文档映射回.NET 对象。

**C# 类**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ to SQL 翻译
DocumentDB 查询提供程序执行最佳的精力映射的 LINQ 查询转换为 DocumentDB SQL 查询。 在下面的描述中，我们假定读者已 LINQ 的基本的了解。

首先，类型系统中，我们支持所有 JSON 基元类型 – 数值类型、 布尔值、 字符串和 null。 只有这些 JSON 类型受支持。 支持下面的标量表达式。

-   常量值，这些查询进行计算的时间包括基元数据类型的常量值。

-   属性/数组索引表达式 – 这些表达式引用对象或数组元素的属性。

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   算术表达式-其中包括对数字和布尔值的常用算术表达式。 有关完整列表，请参阅 SQL 规范。

        2 * family.children[0].grade;
        x + y;

-   这些功能包括字符串比较表达式的字符串值为某个常量字符串值进行比较。  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   对象/数组创建表达式的返回这些表达式的复合值类型或匿名类型对象或此类对象的数组。 这些值可以嵌套。

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>支持 LINQ 运算符的列表
这是在 DocumentDB.NET SDK 随附的 LINQ 提供程序中支持 LINQ 运算符列表。

-   **选择**︰ 投影转换为 SQL 选择包括对象构造
-   **其中**︰ 过滤器转换为 SQL 中，和支持之间的翻译 & &，| |和 ！ 与 SQL 运算符
-   **SelectMany**︰ 允许阵列到 SQL JOIN 子句的展开。 可用于链/嵌套表达式数组元素进行筛选
-   **和 OrderByDescending**︰ 转换为顺序由升序/降序︰
-   **CompareTo**︰ 转换为范围的比较。 因为它们不在.NET 可比较通常用于字符串
-   **花**︰ 转换为 SQL 顶部用于限制查询结果
-   **数学函数**︰ 支持从翻译。NET 的 Abs，Acos，Asin，Atan，天花板，Co、 Exp、 地板、 日志、 Log10、 Pow、 往返、 号、 正弦、 Sqrt、 茶色、 截断到等效的 SQL 内置函数。
-   **字符串函数**︰ 支持从翻译。网络的连接，包含，EndsWith IndexOf、 计数、 ToLower、 TrimStart、 替换、 反向、 TrimEnd、 StartsWith、 子字符串、 ToUpper 到等效的 SQL 内置函数。
-   **数组函数**︰ 支持从翻译。NET 的 Concat、 包含和为等效的 SQL 内置函数的计数。
-   **地理空间扩展功能**︰ 支持的方法存根 （stub） 内，IsValid 和 IsValidDetailed 的距离，从转换为等效的 SQL 内置函数。
-   **用户定义的函数扩展函数**︰ 支持翻译从存根方法 UserDefinedFunctionProvider.Invoke 给相应的用户定义的函数。
-   **杂项**︰ 支持联合和条件运算符的转换。 可以翻译字符串包含、 ARRAY_CONTAINS 或 SQL 入，具体取决于上下文包含。

### <a name="sql-query-operators"></a>SQL 查询运算符
下面是一些示例，阐释如何 LINQ 标准查询运算符的一些翻译到 DocumentDB 查询。

#### <a name="select-operator"></a>选择操作员
语法是`input.Select(x => f(x))`，其中`f`是一个标量表达式。

**LINQ lambda 表达式**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda 表达式**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda 表达式**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany 运算符
语法是`input.SelectMany(x => f(x))`，其中`f`是一个标量表达式，返回的集合类型。

**LINQ lambda 表达式**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>其中运算符
语法是`input.Where(x => f(x))`，其中`f`是一个标量表达式，它返回一个布尔值。

**LINQ lambda 表达式**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda 表达式**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>复合 SQL 查询
上述运算符可以组合以形成更强大的查询功能。 由于 DocumentDB 支持嵌套的集合，组合可以连接或嵌套。

#### <a name="concatenation"></a>串联 

语法是`input(.|.SelectMany())(.Select()|.Where())*`。 串联在一起的查询可以使用可选启动`SelectMany`查询跟多个`Select`或`Where`运算符。


**LINQ lambda 表达式**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda 表达式**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda 表达式**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda 表达式**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>嵌套

语法是`input.SelectMany(x=>x.Q())`其中 Q 是`Select`， `SelectMany`，或`Where`运算符。

在嵌套查询中，内部查询应用于外部集合中的每个元素。 其中一个重要功能就是内部查询可以参考类似外部集合中的元素的字段自联接。

**LINQ lambda 表达式**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda 表达式**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda 表达式**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>执行 SQL 查询
DocumentDB 公开通过 REST API，可以调用任何语言能够发出 HTTP/HTTPS 请求的资源。 另外，DocumentDB 还提供.NET，Node.js，JavaScript 和 Python 等多种常用语言的编程库。 REST API 和各种库都支持通过 SQL 查询。 .NET SDK 支持 LINQ 查询以及 SQL。

下面的示例演示如何创建一个查询，并将其提交对 DocumentDB 数据库帐户。

### <a name="rest-api"></a>REST API，
DocumentDB 通过 HTTP 提供开放式的 RESTful 编程模型。 可以使用 Azure 订阅设置数据库的帐户。 DocumentDB 资源模型包含套在数据库帐户，其中的每个是可寻址的逻辑和稳定的 uri 的资源。 一组资源被称为本文档中的订阅源。 数据库帐户包含一套数据库，每个包含多个集合，哪些在中打开的每个包含文档、 Udf 和其他资源类型。

使用这些资源的基本交互模型是通过 HTTP 谓词 GET、 传、 开机自检和删除其标准的解释。 开机自检谓词用于为创建了一个新的资源、 执行存储的过程或发出 DocumentDB 查询。 查询始终读取只具没有负面影响的操作。

下面的示例显示 DocumentDB 查询包含两个示例文档的集合对到目前为止我们已审阅有关的公告。 查询 JSON name 属性上有一个简单的筛选。 请注意，使用`x-ms-documentdb-isquery`和内容类型︰`application/query+json`标头，以表示该操作是查询。


**请求**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**结果**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


第二个示例显示更复杂的查询联接返回多个结果。

**请求**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**结果**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


如果查询的结果无法适合单个页面的结果，然后 REST API 返回通过延续标记`x-ms-continuation-token`响应标头。 客户端可以通过包括标头中后续的结果分页结果。 此外可以通过控制每页的结果数`x-ms-max-item-count`号头。

若要管理查询数据一致性策略，使用`x-ms-consistency-level`像所有 REST API 请求标头。 对于会话一致性，十分需要也传回最新`x-ms-session-token`Cookie 中查询请求的标头。 请注意查询的集合的索引策略也会影响查询结果的一致性。 与索引策略设置默认情况下，集合索引始终是当前文档中的内容，查询结果将匹配选择的数据的一致性。 如果索引策略宽松到迟缓，查询可以返回陈旧的结果。 有关详细信息，请参阅[DocumentDB 一致性级别] [consistency-levels]。

如果配置集合的索引策略不支持指定的查询，DocumentDB 服务器返回 400"请求失败"。 这将返回范围查询路径配置 （相等） 的哈希查找，并明确从索引中排除的路径。 `x-ms-documentdb-query-enable-scan`指定标头以使查询索引不可用时执行扫描。

### <a name="c-net-sdk"></a>C# (.NET) SDK
.NET SDK 支持 LINQ 和 SQL 查询。 下面的示例演示如何执行此文档中前面引入简单筛选查询。


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


此示例将每个文档中的相等的两个属性进行比较，使用匿名的投影。 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


下一个示例显示联接，通过 LINQ SelectMany 表示。


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



.NET 客户端自动循环中的 foreach 块上面所示的查询结果中的所有页。 REST API，一节中介绍的查询选项也是可用在.NET SDK 中使用`FeedOptions`，`FeedResponse`在 CreateDocumentQuery 方法的类。 可以使用控制的页数`MaxItemCount`设置。 

您可以通过创建显式控制分页`IDocumentQueryable`使用`IQueryable`对象，然后通过读取` ResponseContinuationToken`的值并将其传递回作为`RequestContinuationToken`在`FeedOptions`。 `EnableScanInQuery`可以设置启用扫描时查询不能支持的配置的索引策略。 对于已分区的集合，您可以使用`PartitionKey`对一个运行查询分区 （尽管 DocumentDB 可以自动提取此查询文本），和`EnableCrossPartitionQuery`运行可能需要对多个分区运行的查询。 

包含查询的更多示例，请参阅[DocumentDB.NET 示例](https://github.com/Azure/azure-documentdb-net)。 

### <a name="javascript-server-side-api"></a>JavaScript 服务器端 API 
DocumentDB 提供用于直接在使用存储的过程和触发器的集合上执行基于的 JavaScript 应用程序逻辑的编程模型。 在集合级别注册的 JavaScript 逻辑即可颁发在给定集合中的文档的操作的数据库操作。 这些操作封装在环境 ACID 事务。

下面的示例显示如何在 JavaScript 服务器 API 中使用 queryDocuments 进行查询从内部存储过程和触发器。


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>聚合函数

对聚合函数的本机支持的工作方式，但如果在此期间需要总和或计数功能，您可以获得相同的结果，使用不同的方法。  

在读取的路径︰

- 您可以通过检索数据，计数本地执行聚合函数。 它已建议使用廉价的查询投影像`SELECT VALUE 1`而不是完整的文档，如`SELECT * FROM c`。 这将有助于最大限度地在每一页结果，从而避免更多往返服务，如果需要处理的文档数。
- 存储的过程也可以用于最小化网络延迟的重复往返。 计算给定的筛选查询的计数示例中的存储过程，请参阅[Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js)。 存储的过程可以使用户能够将以高效的方式执行聚合以及丰富的业务逻辑结合起来。

在写入路径︰

- 另一个常见的模式是预聚合中的"写入"路径的结果。 "读取"请求量高于"写"的请求时，这是特别有吸引力。 一次预聚合，则结果将通过单一的读取请求。  在 DocumentDB 中预聚合的最佳办法是设置一个触发器，它与每个"写"调用和更新元数据文档包含最新的结果被具体化的查询。 例如，请看[UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js)示例中，更新 maxSize，minSize，totalSize 元数据文档中找不到。 可以扩展此示例更新计数器，总和，等等。

##<a name="references"></a>引用
1.  [Azure DocumentDB 简介][introduction]
2.  [DocumentDB SQL 规范](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB.NET 示例](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB 的一致性级别][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  Javascript 规范[http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  对于大型数据库[http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)查询求值技术
10. 查询处理中并行关系数据库系统中，IEEE 计算机协会出版社，1994
11. Lu，Ooi，茶色查询处理在并行关系数据库系统中，IEEE 计算机协会出版社，1994年。
12. Christopher Olston，发布簧片、 Utkarsh Srivastava，Ravi Kumar，Andrew Tomkins︰ 猪的拉丁语︰ 用于数据处理、 SIGMOD 2008 不以外国语言。
13.     G。 Graefe。 查询优化顾框架。 IEEE 数据工程师 Bull.、 18(3): 1995年。


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
