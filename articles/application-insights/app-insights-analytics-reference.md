<properties 
    pageTitle="在应用程序的见解分析中引用 |Microsoft Azure" 
    description="分析应用程序的见解是功能强大的搜索工具中的语句引用。 " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>分析的参考

[分析](app-insights-analytics.md)是一种功能强大的搜索功能的[应用程序的见解](app-insights-overview.md)。 这些页面描述分析查询语言。

> [AZURE.NOTE] [测试驱动器在模拟数据的分析](https://analytics.applicationinsights.io/demo)如果您的应用程序不会将数据发送到应用程序的见解尚。

## <a name="index"></a>索引


**让**[让](#let-clause)


**查询运算符和运算符**[计数](#count-operator) | [评估](#evaluate-operator) | [扩展](#extend-operator) | [联接](#join-operator) | [限制](#limit-operator) | [mvexpand](#mvexpand-operator) | [分析](#parse-operator) | [项目](#project-operator) | [项目离开](#project-away-operator) | [范围](#range-operator) | [减少](#reduce-operator) | [呈现指令](#render-directive) | [限制子句](#restrict-clause) | [排序](#sort-operator) | [总结](#summarize-operator) | [采取](#take-operator) | [顶部](#top-operator) | [顶部嵌套](#top-nested-operator) | [联合](#union-operator) | [，](#where-operator) | [位置中](#where-in-operator)

**聚合**[任何](#any) |  [argmax](#argmax) | [argmin](#argmin) | [平均](#avg) | [buildschema](#buildschema) | [计数](#count) | [countif](#countif) | [dcount](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [最大](#max) | [最小](#min) | [百分点](#percentile) | [百分点](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [stdev](#stdev) | [合计](#sum) | [方差](#variance)

**标量**[布尔值](#boolean-literals) | [布尔运算符](#boolean-operators) | [痕](#casts) | [标量比较](#scalar-comparisons) | [gettype](#gettype) | [希](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**数字**[算术运算符](#arithmetic-operators) | [数字文本](#numeric-literals) | [abs](#abs) | [bin](#bin) | [exp](#exp) | [占地](#floor) | [gamma](#gamma) | [日志](#log) | [rand](#rand) | [sqrt](#sqrt) | [如何](#todouble) | [toint](#toint) | [tolong](#tolong)

**日期和时间**[日期和时间表达式](#date-and-time-expressions) | [日期和时间文本](#date-and-time-literals) | [年前](#ago) | [日期部分的](#datepart) | [dayofmonth](#dayofmonth) | [星期](#dayofweek) | [关键字](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [现在](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [都](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**字符串**[Guid](#guids) | [模糊处理字符串](#obfuscated-string-literals) | [字符串](#string-literals) | [字符串比较](#string-comparisons) | [countof](#countof) | [提取](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [替换](#replace) | [拆分](#split) | [strcat](#strcat) | [strlen](#strlen) | [子字符串](#substring) | [tolower](#tolower) | [toupper](#toupper)

**数组、 对象和动态**[数组和对象文本](#array-and-object-literals) | [动态对象函数](#dynamic-object-functions) | [在 let 子句中的动态对象](#dynamic-objects-in-let-clauses) | [JSON 路径表达式](#json-path-expressions) | [名称](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [范围](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>让

### <a name="let-clause"></a>let 子句

**让-命名表的表格**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**让-命名值的标量**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda 让-命名函数**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Let 的子句将一个[名称](#names)绑定到表格式结果、 标量值或函数。 该子句是一个前缀为查询，并绑定的范围是该查询。 （让稍后在会话中使用的名称问题的方法并不提供）。

**语法**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *类型︰*`bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:*查询不能由一个 let 子句作为前缀。

**示例**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


自联接︰

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>查询运算符和运算符

查询转移您遥测源流，跟的过滤器管道的引用组成。 例如︰


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
每个筛选器，通过管道字符作为前缀`|`是一个*运算符*，使用某些参数的实例。 该运算符的输入是上述预期的结果的表。 在大多数情况下，任何参数是[标量表达式](#scalars)的输入列。 在少数情况下，参数是输入列的名称，并在少数情况下，该参数是第二个表。 查询的结果始终是一个表，即使它只具有一个列和一行。

查询可能包含单个换行符，但终止由一个空行。 它们可能包含注释之间`//`和行尾。

查询可能前缀由一个或多个[let 子句](#let-clause)，它定义标量、 表或查询中可以使用的函数。

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`用于在下面的查询示例表示上的管道或源表。
> 

### <a name="count-operator"></a>计数操作符

`count`运算符中输入记录集返回记录 （行） 的数目。

**语法**

    T | count

**参数**

* *T*︰ 表格式数据的记录将被计算在内。

**返回**

此函数可返回一个记录类型的列与表`long`。 唯一单元格的值是在*T 中*的记录数。 

**示例**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>计算运算符

`evaluate`是允许添加到查询的专用的算法的扩展机制。

`evaluate`必须是查询管道中的最后一个运算符 (可能除了`render`)。 它不能出现在函数体中。

[评估 autocluster](#evaluate-autocluster) | [评估篮](#evaluate-basket) | [评估 diffpatterns](#evaluate-diffpatterns) | [extractcolumns 评估](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>评估 autocluster

     T | evaluate autocluster()

AutoCluster 的数据中找到的离散特性 （尺寸） 的常见模式，为模式的小的数目将减少 （无论它是 100 或 100 k 行） 原始查询的结果。 AutoCluster 被开发来帮助分析故障 （例如异常、 崩溃），但可能可以从事任何筛选的数据集。 

**语法**

    T | evaluate autocluster( arguments )

**返回**

AutoCluster 返回 （通常较小） 组跨多个离散属性捕获与共享公共值数据的某些部分的模式。 每个模式都显示结果中的行。 

前两个栏是计数和捕获模式从原始查询的行的百分比。 其余的列是从原始查询和它们的值为列中的任一特定值或 * 表示变量值。 

请注意这些模式不是脱节︰ 它们可能会重叠，并且通常不包括原始的所有行。 某些行可能不会受到任何模式。

**提示**

* 使用`where`和`project`中输入管道减少到只是什么您感兴趣的数据。
* 当您发现一个有趣的行时，您可能想要深入研究进一步通过添加到其特定值您`where`筛选器。

**参数 （所有可选）**

* `output=all | values | minimal` 

    结果的格式。 计数和百分比列总是显示在结果中。 

 * `all`-输入中的所有列都都输出
 * `values`-筛选出具有唯一的列 ' *' 在结果中
 * `minimal`-还筛选出的原始查询中的所有行的相同列。 


* `min_percent=`*双精度*(默认值︰ 1)

    生成的行的最小百分比覆盖率。

    示例︰`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int*(默认︰ 25) 

    种子的数量确定的算法的初始本地搜索点数。 在某些情况下，根据结构的数据，增加种子数增加通过速度较慢的查询代价更高的搜索空间结果的数量 （或质量）。 Num_seeds 参数具有降低在两个方向的结果，以便它减少 5 下能达到可以忽略不计的性能改进并增加超过 50 很少会产生附加的图案。

    示例︰`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < 双 < 1*+ (默认︰ 0.5)

    为您提供一定的控制一般 （高覆盖率） 和信息性 （很多价值观） 之间的平衡。 不断增加的 size_weight 通常可以减少的模式，和每个图案往往涉及较大比例。 通常减少 size_weight 产生更多的共享的值和较小百分比覆盖率更具体模式。 在汽车发动机罩下公式是之间的标准一般得分和 size_weight 和 1 size_weight 作为重量的信息性分数加权的几何平均值。 

    示例︰`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*column_name*

    认为按照指定的量输入的每一行 （默认情况下每一行有 '1' 的权重），权重列的常见用法是考虑到帐户采样或桶/聚合已嵌入到每个行的数据。

    示例︰`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>评估的购物篮

     T | evaluate basket()

购物篮数据中找到所有的频繁模式的离散特性 （尺寸），将返回所有传入的原始查询的频率阈值的频繁模式。 购物篮保证在数据中查找所有的频繁模式，但不是能保证有多项式的运行时间。 查询的运行时间中的行数是线性，但在某些情况下可能指数中的列 （维度） 数。 购物篮根据最初开发购物篮分析数据挖掘的 Apriori 算法。 

**返回**

出现在多个指定的分数 （默认 0.05） 的事件中的所有模式。

**参数 （所有可选）**


* `threshold=`*0.015 < 双 < 1*(默认︰ 0.05) 

    设置最小比例的行被视为频繁 （具有较小比率的模式将不会返回）。

    示例︰`T | evaluate basket("threshold=0.02")`


* `weight_column=`*column_name*

    认为按照指定的量输入的每一行 （默认情况下每一行有 '1' 的权重），权重列的常见用法是考虑到帐户采样或桶/聚合已嵌入到每个行的数据。

    示例︰ T |评估 basket("weight_column=sample_Count")


* `max_dims=`*1 < int*(默认︰ 5)

    设置每次可选择篮中，默认情况下，可减小查询运行时限制最大不相关的维度数。


* `output=minimize` | `all` 

    结果的格式。 计数和百分比列总是显示在结果中。

 * `minimize`-筛选出具有唯一的列 ' *' 在结果中。
 * `all`-输入中的所有列都是输出。




#### <a name="evaluate-diffpatterns"></a>评估 diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns 对同一结构的两个数据集进行比较，发现模式的离散特性 （尺寸） 的特征两个数据集之间的差异。 Diffpatterns 被开发来帮助分析失败 （例如按给定的时间段内的非故障比较失败），但可能可以找到同一结构的任何两个数据集之间的差异。 

**语法**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**返回**

Diffpatterns 返回 （通常较小） 组的捕获的两个集合中的数据的不同部分 （即模式捕获第一组数据中的行的很大一部分和第二组中的行的百分比） 的模式。 每个模式都显示结果中的行。

前四个列的计数和原始查询从捕获的每个组中的模式的行的百分比，第五列是两组之间的差别 （以绝对百分比磅为单位）。 其余的列是从原始查询和它们的值为列中的任一特定值或 * 意味着变量值。 

请注意这些模式不是不同︰ 它们可能会重叠，并且通常不包括原始的所有行。 某些行可能不会受到任何模式。

**提示**

* 使用位置和要减少到只是什么您感兴趣的数据的输入管道中的项目。

* 当发现一个有趣的行时，可能要深入研究进一步通过将其特定的值添加到您在何处筛选。

**参数**

* `split=`*列名称*（必需）

    该列必须精确地两个值。 如果有必要，请创建此类的列︰

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*字符串*

    告知要仅查找的算法模式的目标数据集中具有更高的百分比，目标必须是拆分列这两个值之一。

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < 双 < 1*(默认︰ 0.05) 

    设置两个集之间的最小模式 （比率） 差异。

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    结果的格式。 计数和百分比列总是显示在结果中。 

 * `minimize`-筛选出具有唯一的列 ' *' 在结果中
 * `all`-输入中的所有列都都输出

* `weight_column=`*column_name*

    认为按照指定的量输入的每一行 （默认情况下每个行都有粗细值为"1"）。 一个权重列的一个常见用途是考虑到帐户采样或桶/聚合已嵌入到每个行的数据。

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>评估 extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns 用来丰富多是基于其类型的动态提取出 （半） 结构化的列的简单列的表。 当前，它支持仅用于 json 列这两个动态和 jsons 的序列化的字符串。


* `max_columns=`*int*(默认值︰ 10) 

    新添加的列的数目是动态的它可以是非常大的 （实际上是所有 json 记录中的非重复键数） 因此我们必须限制它。 新列将按降序顺序对其频率和最多 max_columns 添加到表。

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*双精度*(默认︰ 10.0) 

    另一种方法来限制通过忽略的列其频率低于 min_percent 的新列。

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*布尔值*(默认值︰ true) 

    如果为 true 则将对提取的列名称作为前缀添加复杂的列的名称。


* `prefix_delimiter=`*字符串*(默认:"_") 

    如果 add_prefix = true 此参数定义可用于连接新列的名称的分隔符。

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*布尔值*(默认值︰ false) 

    如果为 true 将在输出表中保留原始 (json) 列。


* `output=query | table` 

    结果的格式。 

 * `table`-输出是减去指定的输入的列加新列的输入列中提取接收到同一个表。
 * `query`-输出结果是一个字符串，表示的查询将获取表的结果。 




### <a name="extend-operator"></a>扩展运算符

     T | extend duration = stopTime - startTime

将一个或多个计算的列追加到表中。 


**语法**

    T | extend ColumnName = Expression [, ...]

**参数**

* *T:*输入的表。
* *列名称︰*若要添加列的名称。 [名称](#names)区分大小写，可以包含字母、 数字或 _ 字符。 使用`['...']`或`["..."]`引用关键字或与其他字符的名称。
* *表达式︰*通过在现有列计算。

**返回**

具有指定的其他列的输入表的副本。

**提示**

* 使用[`project`](#project-operator)，如果您还想要删除或重命名某些列。
* 不要使用`extend`只是为了获取长整型表达式中使用一个较短名称。 `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    本地表中的列已建立索引;新名称定义附加的列没有索引，因此查询很可能运行速度变慢。

**示例**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>联接运算符

    Table1 | join (Table2) on CommonColumn

合并两个表中的行匹配指定列的值。


**语法**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**参数**

* *表 1* -'左' 的联接。
* *表 2* -联接右侧。 它可以输出表格的嵌套的查询表达式。
* *CommonColumn* -在两个表中具有相同名称的列。
* *类型*-指定要匹配两个表中的行的方式。

**返回**

与表︰

* 每个列在两个表，包括匹配的键列。 如果存在名称冲突，右侧的列将自动更换。
* 对于每个匹配项之间输入的表行。 匹配是选定一个表中具有相同值的所有行`on`字段与其他表中的行。 

* `Kind`未指定

    只包含一行，从左侧的每个值匹配`on`键。 输出包含此行与行从右侧的每个匹配项的行。

* `Kind=inner`
 
     一行中没有匹配行的每个组合的输出从左和右。

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     除了内部匹配，还有行的每一行上的左 （和/或右侧），即使它有没有匹配项。 在这种情况下，不匹配的输出单元格包含空值。

* `kind=leftanti`

     从左侧没有匹配项，从右侧返回的所有记录。 结果表只是具有从左侧的列。 
 
如果有多个行具有相同的值，这些字段，您将获得所有组合的行。

**提示**

为了获得最佳性能︰

* 使用`where`和`project`之前减少在输入表中，行和列的号码`join`。 
* 如果总是小于另一个表，则使用它作为联接的左侧 （管道）。
* 联接匹配的列必须具有相同的名称。 如有必要，重命名其中一个表中的列，请使用 project 运算符。

**示例**

在某些条目标记开始和结束的活动日志中获取扩展活动。 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>限制运算符

     T | limit 5

返回指定的行数，从输入表。 没有要返回的记录能保证。 (若要返回特定的记录，请使用[`top`](#top-operator)。)

**别名**`take`

**语法**

    T | limit NumberOfRows


**提示**

`Take`是简单而有效的方式来查看您的结果的示例，以交互方式工作时。 请注意，它并不能保证产生任何特定的行，或在任何特定的顺序给出它们。

没有为隐式限制返回到客户端的行的数量即使不使用`take`。 提起这一限制，使用`notruncation`客户端请求选项。



### <a name="mvexpand-operator"></a>mvexpand 运算符

    T | mvexpand listColumn 

扩展从动态类型 (JSON) 单元格的列表，以便每个项都有一个单独的行。 所有其他单元格扩展行中的被重复。 

(请参阅[`summarize makelist`](#summarize-operator)的执行相反的功能。)

**示例**

假设输入的表︰

|A:int|B:string|D:dynamic|
|---|---|---|
|1|"你好"|{"钥匙":"值"。|
|2|"世界"|[0，1，"k"、"v"]|

    mvexpand D

结果是︰

|A:int|B:string|D:dynamic|
|---|---|---|
|1|"你好"|{"钥匙":"值"。|
|2|"世界"|0|
|2|"世界"|1|
|2|"世界"|"k"|
|2|"世界"|"v"|


**语法**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**参数**

* *列名称︰*在结果中指定的列的数组被扩展为多个行。 
* *ArrayExpression:*生成一个数组的表达式。 如果使用此窗体，则添加新列并保留现有联系人。
* *名称︰*新列的名称。
* *类型名称︰*将强制转换为特定类型的扩展的表达式
* *RowLimit:*行从原始的每一行生成最大数目。 默认值为 128。

**返回**

每个任何数组或数组表达式中指定的列中的值的多个行。

展开的列始终具有动态类型。 使用强制转换，如`todatetime()`或`toint()`如果您想要计算或聚合值。

受支持的属性包扩展的两种模式︰

* `bagexpansion=bag`︰ 属性包扩展到单个条目属性包。 这是默认扩展。
* `bagexpansion=array`︰ 展开为两个元素属性包`[`*键*`,`*值*`]`阵列结构，允许统一访问的键和值 （也一样，例如，对运行非重复计数聚合的属性名称）。 

**示例**


    exceptions | take 1 
  	| mvexpand details[0]

将异常记录拆分为每个项目中的详细信息字段的行。



### <a name="parse-operator"></a>分析运算符

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

从字符串中提取值。 可以使用简单或正则表达式匹配。

**语法**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**参数**

* `T`︰ 输入的表。
* `kind`: 
 * `simple`（默认值）︰`Match`字符串是无格式的字符串。
 * `relaxed`︰ 如果文本不分析作为列类型，请将列设置为 null，并且分析将继续 
 * `regex`:`Match`字符串是正则表达式。
* `Text`︰ 一列或另一个表达式的计算结果为，或可以转换为字符串。
* *匹配︰*匹配字符串中的下一个部分并放弃它。
* *列︰*将字符串中的下一部分分配给该列。 如果它不存在，则创建列。
* *类型︰*分析的字符串的下一步的一部分作为指定类型，如 int，迄今为止，两倍。 


**返回**

扩展的列的列表根据输入的表。

中的元素`with`子句反过来与源文本相匹配。 每个元素 chews 关闭的源文本块︰ 

* 原义字符串或正则表达式将匹配光标移由匹配的长度。
* 在正则表达式分析，正则表达式可以使用最小化运算符？ 尽可能快地移动到以下匹配项。
* 列名称与类型分析作为指定类型的文本。 除非类型 = 宽松，不成功的分析使整个模式匹配。
* 没有类型，或具有类型字符串列名称复制到以下匹配的字符的最小数目。
* *会跳转到以下匹配的字符的最小数目。您可以使用*的开始和结束的图案，或者后一种类型的字符串，或字符串的匹配项之间。

所有的分析模式中的元素必须匹配正确;否则，会不产生任何结果。 此规则的例外是，当类型 = 继续宽松，如果分析的类型化的变量失败，其余的分析。

**示例**

*简单︰*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | 计数器 | 存在 | 年份
---|---|---|---
1 | 2 | socks | 63

*放松︰*

当输入包含正确的匹配项的每个类型的列时，宽松的分析将产生相同的结果为简单的分析。 但如果一个类型化列不正确分析，宽松的分析继续处理其余部分的图案，而简单分析停止，将无法生成任何结果。


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | 存在 | 年份
---|---|---
1 |  socks | 63


*正则表达式︰*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

资源 | 快讯 | 锁定 | 释放 | 以前
---|---|---|---|---
计划程序 | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>project 运算符

    T | project cost=price*quantity, price

选择要包括、 重命名或删除，或者列，插入新的计算的列。 在结果中列的顺序指定参数的顺序。 仅在参数中指定的列包括在结果︰ 在输入中的任何其他正在丢弃。  (请参阅`extend`。)


**语法**

    T | project ColumnName [= Expression] [, ...]

**参数**

* *T:*输入的表。
* *列名称︰*在输出中显示列的名称。 如果该名称的栏必须出现在输入任何*表达式*。 [名称](#names)区分大小写，可以包含字母、 数字或 _ 字符。 使用`['...']`或`["..."]`引用关键字或与其他字符的名称。
* *表达式︰*引用的输入的列的可选标量表达式。 

    它是合法输入中返回具有相同名称的现有列的新计算的列。

**返回**

具有名称作为参数，并尽可能多的列的表作为输入表的行。

**示例**

下面的示例演示几种可完成的操作使用`project`运算符。 输入的表`T`有三种类型的列`int`: `A`， `B`，和`C`。 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>项目离开运算符

    T | project-away column1, column2, ...

排除指定的列。 结果都包含您命名的除外的所有输入的列。

### <a name="range-operator"></a>区域运算符

    range LastWeek from ago(7d) to now() step 1d

生成值的单个列的表。 请注意︰ 它不具有输入管道。 

|LastWeek|
|---|
|05-2015年-12 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**语法**

    range ColumnName from Start to Stop step Step

**参数**

* *列名称︰*输出表中的单个列的名称。
* *开始︰*输出中的最小值。
* *停止︰*正在生成输出 （或最高值，如果该值逐过程执行*步骤*绑定） 中的最高值。
* *步骤︰*两个连续的值之间的差异。 

参数必须为数值、 日期或时间跨度值。 他们不能引用任何表中的列。 （如果您希望计算基于输入表的范围，使用[范围*函数*](#range)，也许使用[mvexpand 运算符](#mvexpand-operator)。） 

**返回**

表一列称为*列名称*，其值为*启动*，请*启动* + *步骤*，...甚至*停止*。

**示例**  

```AIQL
range Steps from 1 to 8 step 3
```

包含单个列的表称为`Steps`的类型是`long`，其值为`1`， `4`，和`7`。

**示例**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

在过去的七天午夜的表。 纸盒 （基底） 函数将每次减少到一天的开始。

**示例**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

演示如何`range`可以使用运算符来创建小型、 点对点、 维度表，然后用来引入零位置源数据中的任何值。

### <a name="reduce-operator"></a>减少操作员

    exceptions | reduce by outerMessage

尝试在一起类似的记录进行分组。 对于每个组中，运算符输出`Pattern`它认为最能准确描述该组，和`Count`该组中的记录。


![](./media/app-insights-analytics-reference/reduce.png)

**语法**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**参数**

* *列名称︰*要检查的列。 它必须是字符串类型。
* *阈值︰*{0..1} 的范围中的值。 默认值为 0.001。 对于较大的输入阈值应很小。 

**返回**

两列， `Pattern` ， `Count`。 在许多情况下，图案将从列的完整值。 在某些情况下，它可以识别常见术语和替换与可变部分 ' *'。

例如，结果`reduce by city`可能包括︰ 

|模式 | 计数 |
|---|---|
| San * | 5182 |
| 圣 * | 2846 |
| 莫斯科 | 3726 |
| \*-亮-\* | 2730 |
| 巴黎 | 27163 |


### <a name="render-directive"></a>呈现指令

    T | render [ table | timechart  | barchart | piechart ]

呈现指示如何显示表的表示层。 它应该是管道的最后一个元素。 它是一个便捷的替代方式使用上的显示，使您能够用一种特定的表示方法保存查询的控件。

### <a name="restrict-clause"></a>限制子句 

指定表名称可供遵循的运算符的集。 例如︰

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>排序运算符 

    T | sort by country asc, price desc

对输入表中的行顺序由一个或多个列进行排序。

**别名**`order`

**语法**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**参数**

* *T:*输入要排序的表。
* *列︰**T*的作为排序依据的列。 值的类型必须是数字、 日期、 时间或字符串。
* `asc`以升序，低到高排序依据。 默认值是`desc`，高降序为低。

**示例**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
有一个特定的表跟踪中的所有行`ActivityId`、 按其时间戳进行排序。

### <a name="summarize-operator"></a>汇总运算符

将生成聚合内容输入表的表。
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

表显示在每个国家的数量、 平均请求持续时间和城市的一套。 在每个不同国家的输出没有行。 显示输出列的计数，平均持续时间、 城市和国家。 所有其它输入的列将被忽略。


    T | summarize count() by price_range=bin(price, 10.0)

表显示多少项具有价格在每个时间间隔 [0,10.0]，[10.0,20.0]，等等。 此示例包含用于计数的列，另一个用于的价格范围。 所有其它输入的列将被忽略。


**语法**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**参数**

* *列︰*结果列的可选名称。 默认值为一个名称出自表达式。 [名称](#names)区分大小写，可以包含字母、 数字或 _ 字符。 使用`['...']`或`["..."]`引用关键字或与其他字符的名称。
* *聚合︰*对一个聚合函数，如`count()`或`avg()`，带列名称作为参数。 请参见[聚合](#aggregations)。
* *GroupExpression:*列表达式，提供一组不同的值。 它通常是已经提供了有限的一组值，或者列名称或`bin()`与数字或时间列作为参数。 

如果您不使用的情况下提供数字或时间表达式`bin()`，分析自动将其应用的间隔`1h`的时间，或`1.0`号。

如果您没有提供*GroupExpression，*一个输出行总结整个表格。



**返回**

对输入的行进行排列成组具有相同值的`by`表达式。 然后指定的聚合函数计算每组，产生每个组的行。 结果包含`by`列和每个也至少一个列计算聚合。 （某些聚合函数返回的多个列）。

结果具有多个行的不同组合进行`by`值。 如果您想要汇总数值的范围上，使用`bin()`缩小为离散值的范围。

**请注意**

虽然在聚合和分组表达式，可以提供任意表达式，它是更高效地使用简单的列名称，或者应用`bin()`为一个数值列。



### <a name="take-operator"></a>采用运算符

[限制](#limit-operator)的别名


### <a name="top-operator"></a>top 运算符

    T | top 5 by Name desc nulls first

返回由指定的列进行排序的第一个*N*记录。


**语法**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**参数**

* *NumberOfRows:**T*要返回的行数。
* *Sort_expression:*对行进行排序所依据的表达式。 它通常是只是列名称。 您可以指定多个 sort_expression。
* `asc`或`desc`（默认值） 可能会出现为控件选择实际上是否从该区域的"顶"的"底部"。
* `nulls first`或`nulls last`控制空值的出现位置。 `First`默认值为`asc`，`last`是的默认值为`desc`。


**提示**

`top 5 by name`表面上相当于`sort by name | take 5`。 但是，它运行得更快，并且始终返回对结果进行排序，而`take`这样不能保证。

### <a name="top-nested-operator"></a>顶部嵌套运算符

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

产生分层的结果，其中每个级别是从上一级别分级。 它可用于回答问题，听起来像"5 要求是什么和什么是顶部 3 性能桶中，为每个成员，并对于每个成员，这前 3 位国家请求来自吗？"

**语法**

   T |聚合顶部嵌套列的 N [，...]

**参数**

* N:int-要返回或将传递到下一个级别的行数。 在查询中使用三个级别，其中 N 是 5、 3 和 3，将 45 行的总数。
* 列-聚合分组依据的列。 
* 聚合，[聚合函数](#aggregations)将应用于每一组行。 这些聚合的结果将确定要显示的顶级组。


### <a name="union-operator"></a>union 运算符

     Table1 | union Table2, Table3

采用两个或多个表并返回的所有行。 

**语法**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**参数**

* *表 1*，*表 2* ...
 *  表的名称，如`requests`，或表定义中的[let 子句](#let-clause);或
 *  一个查询表达式，如`(requests | where success=="True")`
 *  使用通配符指定表的一组。 例如，`e*`将形成以前其名称从例外表与 e 开始的 let 子句中定义的所有表的并集。
* `kind`: 
 * `inner`-结果具有的通用于所有输入表的列的子集。
 * `outer`-结果具有任意输入中发生的所有列。 未定义的输入行的单元格设置为`null`。
* `withsource=`*列名称︰*如果指定，输出将包含名为其值表示的源表发表每一行的*列名称*的列。

**返回**

具有相同行数的表输入的所有表，及尽可能多的列中输入唯一的列名称。

**示例**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
所有的表，其名称以"tt"的同盟。


**示例**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
已经生产或者不同用户数`exceptions`事件或`traces`在过去一天的事件。 在结果中，SourceTable 列将指示"查询"或"命令"。

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

此效率更高的版本产生相同的结果。 它在创建联合之前筛选的每个表。

### <a name="where-operator"></a>其中运算符

     requests | where resultCode==200

筛选表满足谓词的行的子集。

**别名**`filter`

**语法**

    T | where Predicate

**参数**

* *T:*表格格式输入的记录的筛选。
* *谓词︰*A `boolean` *T*的列的[表达式](#boolean)。它被计算*T*中的每一行。

**返回**

在*T*中*谓词*的行`true`。

**提示**

若要获取最快的性能︰

* 列名称和常量之间的**使用简单比较**。 (常数表-意味着常数以便`now()`，`ago()`是好的并且因此标量值分配使用[`let`子句](#let-clause)。)

    例如，喜欢`where Timestamp >= ago(1d)`到`where floor(Timestamp, 1d) == ago(1d)`。

* **Simplest 条款第一**︰ 如果您有多个子句与联合`and`，将首先涉及一栏的子句。 因此`Timestamp > ago(1d) and OpId == EventId`比反过来要好得多。


**示例**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

不超过 1 小时，和来自名为"Kuskus"的来源有两个列的值相同的记录。 

请注意，我们将放两列之间的比较上一次，因为它不能使用索引并强制执行扫描。


### <a name="where-in-operator"></a>在何处 in 运算符

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**语法**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**参数**

* `col`︰ 表列。
* `expr1`...: 标量表达式列表。

使用`in`用于只包括行的`col`等于一个表达式`expr1...`。

使用`!in`以在其中包含仅行`col`不等于任何表达式`expr1...`。  


## <a name="aggregations"></a>聚合

聚合是函数用于[汇总操作](#summarize-operator)中创建的组中的值进行组合。 例如，在该查询中，dcount() 是一个聚合函数︰

    requests | summarize dcount(name) by success

### <a name="any"></a>任何 

    any(Expression)

随机选择一个行的组，返回指定表达式的值。

这很有用，例如，当某些列包含大量相似的值 （例如，"错误文本"专栏） 和要取样复合组密钥的唯一值每次该列。 

**示例**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

查找行中的组，最小化/可使*ExprToMaximize*，并返回值的*ExprToReturn* (或`*`返回的整个行)。

**提示**︰ 通过传递列会自动重命名。 若要确保您正在使用正确的名称，检查的结果，使用`take 5`到另一个操作员管道结果之前。

**示例**

对于每个请求的名称，显示在长请求发生时︰

    requests | summarize argmax(duration, timestamp) by name

显示时间最长的请求，而不仅仅是时间戳的详细的信息︰

    requests | summarize argmax(duration, *) by name


查找每个跃点计数，其时间戳和其他数据的最小值︰

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>平均

    avg(Expression)

计算一组*表达式*的平均值。

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

返回承认*DynamicExpression*的所有值的最小模式。 

参数列类型应为`dynamic`-阵列或属性包。 

**示例**

    exceptions | summarize buildschema(details)

结果︰

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

请注意，`indexer`用来标记应使用数字索引的位置。 此架构中，一些有效的路径，就 （假设这些示例索引是否在范围内）︰

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**示例**

假定输入的列有三个动态值︰

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


将生成的架构︰

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

该架构告诉我们的︰

* 根对象是一个容器具有名为的四个属性 x、 y、 z 和 t。
* 名为"x"的可能的属性的类型"int"或"string"类型。
* 调用该属性可能的两种类型的"双精度"的"y"或称为"w"的类型"字符串"属性与另一个容器。
* ``indexer``关键字指示"z"和"t"是数组。
* "Z"数组中的每个项是整型或字符串。
* "t"是一个字符串数组。
* 每个属性是隐式地是可选的并且任何数组可能为空。

##### <a name="schema-model"></a>架构模型

返回架构的语法是︰

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

它们相当于 TypeScript 的类型批注，作为一个动态值编码的一个子集。 在 Typescript，为示例架构︰

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>计数

    count([ Predicate ])

返回计数的行的*谓词*计算结果为`true`。 如果指定任何*谓词*，则返回组中的记录总数。 

**性能提示**︰ 使用`summarize count(filter)`而不是`where filter | summarize count()`

> [AZURE.NOTE] 避免使用 count() 查找请求、 异常或发生其他事件的数目。 在操作中[采样](app-insights-sampling.md)时，保留在应用程序的见解中的数据点的数目将原始的事件数小于。 相反，使用`summarize sum(itemCount)...`。 ItemCount 属性反映了由每个保留的数据点的初始事件的数目。

### <a name="countif"></a>countif

    countif(Predicate)

返回计数的行的*谓词*计算结果为`true`。

**性能提示**︰ 使用`summarize countif(filter)`而不是`where filter | summarize count()`

> [AZURE.NOTE] 避免使用 countif （） 来查找请求、 异常或发生其他事件的数目。 [采样](app-insights-sampling.md)操作时，数据点的数目将少于的实际事件数。 相反，使用`summarize sum(itemCount)...`。 ItemCount 属性反映了由每个保留的数据点的初始事件的数目。

### <a name="dcount"></a>dcount

    dcount( Expression [ ,  Accuracy ])

返回组中的非重复值的*表达式*数目的估计。 (若要列出不同的值，请使用[`makeset`](#makeset)。)

*准确性*，如果指定，控制速度和准确性之间的平衡。

 * `0`= 最准确和最快捷的计算。
 * `1`默认情况下，平衡精度和计算时间;关于 0.8%错误。
 * `2`= 最准确、 最慢计算;关于 0.4%错误。

**示例**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

返回组中的*谓词*是如此的非重复值的*表达式*的行数估计。 (若要列出不同的值，请使用[`makeset`](#makeset)。)

*准确性*，如果指定，控制速度和准确性之间的平衡。

 * `0`= 最准确和最快捷的计算。
 * `1`默认情况下，平衡精度和计算时间;关于 0.8%错误。
 * `2`= 最准确、 最慢计算;关于 0.4%错误。

**示例**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

返回`dynamic`(JSON) 数组的*表达式*的组中的所有值。 

* *MaxListSize*是可选的整数限制返回的元素的最大数量 （默认值为*128*）。

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

返回`dynamic` *Expr*所需的组中的重复值组 (JSON) 数组。 (提示︰ 若要只计算不同的值，请使用[`dcount`](#dcount)。)
  
*  *MaxSetSize*是可选的整数限制返回的元素的最大数量 （默认值为*128*）。

**示例**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

请参阅[`mvexpand`运算符](#mvexpand-operator)对面的函数。


### <a name="max-min"></a>最大、 最小值

    max(Expr)

计算*表达式*的最大值。
    
    min(Expr)

计算*表达式*的最小值。

**提示**︰ 这使您的最小值或最大值本身-例如，最高或最低价格。 但是，如果您希望其他列中的行-例如，最低价格的供应商名称-使用[argmin 或 argmax](#argmin-argmax)。


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>百分点百分点、 percentilew、 percentilesw

    percentile(Expression, Percentile)

返回的*表达式*的组中指定百分比的估计值。 准确性取决于的百分点的地区的人口密度。
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

像`percentile()`，但计算 （这是比单独计算每个百分点的速度更快） 的百分点值数值。

    percentilew(Expression, WeightExpression, Percentile)

加权的百分点值。 使用此预聚合的数据。  `WeightExpression`是一个整数，指示由聚合的每一行表示原始行的数目。

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

像`percentilew()`，但计算大量的百分点值。

**示例**


值`duration`大于 95%的样本集和小于 5%的样本集，计算每个请求的名称︰

    request 
  	| summarize percentile(duration, 95)
      by name

省略"由..."来计算整个表格。

同时计算几个百分点的不同请求名称︰

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

结果显示请求 /Events/Index，5%的请求将得到响应在小于 2.44s，3.52s，在其中的一半，比 6.85s年慢 5%。

计算多个统计信息︰

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>加权的百分点

在数据已预聚合的情况下使用加权百分点值函数。 

例如，假设您的应用程序执行的每秒数千并且想要知道其滞后时间。 简单的解决方案是生成应用程序的见解请求或为每个操作的自定义事件。 这将创建大量的通信，尽管自适应采样才能生效，以减少它。 但您决定实现一个更好的解决方案︰ 您将编写的某些代码在您的应用程序之前将其发送到应用程序的见解聚合数据。 将在固定的时间间隔，到几个点，每分钟可能降低数据率发送聚合的摘要。

您的代码将以毫秒为单位的延迟测量的流。 例如︰
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

计算下列分类中的度量值︰`{ 10, 20, 30, 40, 50, 100 }`

定期，它使一系列 TrackEvent 调用，另一个用于每个桶中，在每次调用自定义度量︰ 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

分析，在中，您将看到这样一组类似下面的事件︰

`opCount` | `latency`| 含义
---|---|---
8 | 10 | = 8 10ms bin 中的操作
6 | 20 | = 6 20 毫秒 bin 中的操作
3 | 30 | = 在 30ms bin 3 项操作
1 | 40 | = 差值中 1 操作

为准确掌握事件延迟的原始分布，我们使用`percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

结果都是相同我们象使用纯`percentiles`原始集的度量值。

> [AZURE.NOTE] 加权的百分点并非适用于[采样数据](app-insights-sampling.md)，其中每个样本的行表示原始行，而不是箱的一个随机样本。 普通的百分点值函数是适合抽样数据。

#### <a name="estimation-error-in-percentiles"></a>估计的百分点值时出错

百分点值聚合提供了估计值使用[T 摘要](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf)。 

几个要点︰ 

* 在估计错误的边界随请求百分比的值。 最高的准确性是两端 [0..100] 的缩放，请百分点 0 和 100 的确切的最小值和最大值的分布。 准确性用而向中间的比例逐渐减少。 它是在中间最坏，限制在 1%。 
* 错误边界级别，不能在值上观察到。 假设 (X50) 的百分点值返回的值的 Xm。 估计保证至少 49%和 51%的 X 的值最是 Xm 小于。 在 Xm 和实际中的 X 值之间的差异没有理论限制。

### <a name="stdev"></a>标准差

     stdev(Expr)

返回组*Expr*的标准偏差。

### <a name="variance"></a>方差

    variance(Expr)

返回组*Expr*的方差。

### <a name="sum"></a>总和

    sum(Expr)

返回组的*Expr*的总和。                      


## <a name="scalars"></a>标量

[强制转换](#casts) | [的比较](#scalar-comparisons)
<br/>
[gettype](#gettype) | [希](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

支持的类型包括︰

| 类型      | 其他名称   | 等效.NET 类型 |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>强制转换

可以从一种类型转换为另一个。 一般情况下，如果转换有意义，它将起作用︰

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

检查一个字符串是否可以转换为特定类型︰

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>标量的比较

||
---|---
`<` |较低
`<=`|小于或等于
`>` |更多
`>=`|大于或等于
`<>`|不等于
`!=`|不等于 
`in`| 右操作数是一个 （动态） 数组，左的操作数等于其元素之一。
`!in`| 右操作数是一个 （动态） 数组和左的操作数不等于的任何元素。




### <a name="gettype"></a>gettype

**返回**

一个字符串，表示其单个参数的基础存储类型。 这是特别有用如果有种类的值`dynamic`︰ 在这种情况下`gettype()`将揭示如何编码值。

**示例**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>哈希

**语法**

    hash(source [, mod])

**参数**

* *来源*︰ 计算哈希标量的来源。
* *现代型*︰ 模值应用于哈希结果。

**返回**

给定的标量，给定的 mod 值 （如果已指定） 模 xxhash （长时间） 值。

**示例**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>iff

`iff()`函数计算第一个参数 （谓语），并返回的第二个或第三参数根据谓词是否值`true`或`false`。 第二个和第三个参数必须属于同一类型。

**语法**

    iff(predicate, ifTrue, ifFalse)


**参数**

* *谓词︰*表达式计算结果为`boolean`的值。
* *ifTrue:*获取计算的表达式，其值从函数返回如果*谓词*计算结果为`true`。
* *ifFalse:*获取计算的表达式，其值从函数返回如果*谓词*计算结果为`false`。

**返回**

此函数返回的值*ifTrue* ，如果*谓词*计算结果为`true`， *ifFalse*其他的值。

**示例**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>isnull，isnotnull notnull

    isnull(parsejson("")) == true

带有一个参数，指示是否为 null。

**语法**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**返回**

True 或 false，具体取决于是否值为空或非空。


|x|isnull(x)
|---|---
| "" | 假
|"x" | 假
|parsejson("")|真
|parsejson("[]")|假
|parsejson("{}")|假

**示例**

    T | where isnotnull(PossiblyNull) | count

请注意，有其他方法可以达到这种效果︰

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

一个查询或一个表达式的计算结果并返回单个值的结果。 此函数可用于分阶段计算;例如，计算总数的事件，然后将它用作比较基准。

**语法**

    toscalar(query)
    toscalar(scalar)

**返回**

计算所得的参数。 如果该参数是一个表，则返回的第一行第一列。 （很好的做法是排列参数具有一个列和行。

**示例**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>布尔值

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>布尔运算符

    and 
    or 

    

## <a name="numbers"></a>数字

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>数字文本

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>算术运算符

|| |
|---|-------------|
| + | 添加         |
| - | 相减    |
| * | 乘法运算    |
| / | 除      |
| % | 求模      |
||
|`<` |较低
|`<=`|小于或等于
|`>` |更多
|`>=`|大于或等于
|`<>`|不等于
|`!=`|不等于 


### <a name="abs"></a>abs

**语法**

    abs(x)

**参数**

* x-整数、 实数或时间跨度

**返回**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>纸盒、 基底

舍入到整数值给定的纸盒大小的倍数。 大量使用在[`summarize by`](#summarize-operator)查询。 如果您有分散的一组值，它们将分成较小的一组特定值。

别名`floor`。

**语法**

     bin(value, roundTo)
     floor(value, roundTo)

**参数**

* *值︰*数字、 日期或时间跨度。 
* *roundTo:*"存储空间"。 数字、 日期或*值*除以的时间跨度。 

**返回**

最接近*值*低于*roundTo*的倍数。  
 
    (toint((value/roundTo)-0.5)) * roundTo

**示例**

表达式 | 结果
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


下面的表达式将计算存储桶大小为 1 秒的持续时间，直方图︰

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>基底

别名[`bin()`](#bin)。

### <a name="gamma"></a>灰度系数

[伽玛函数](https://en.wikipedia.org/wiki/Gamma_function)

**语法**

    gamma(x)

**参数**

* *x:*实数

为正整数， `gamma(x) == (x-1)!` ， `gamma(5) == 4 * 3 * 2 * 1`。

请参阅[loggamma](#loggamma)。


### <a name="log"></a>日志

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`应实数 > 0。 否则，将返回 null。

### <a name="loggamma"></a>loggamma


自然对数[伽玛函数](#gamma)的绝对值。

**语法**

    loggamma(x)

**参数**

* *x:*实数


### <a name="rand"></a>rand

随机数字生成器。

* `rand()`-0.0 和 1.0 之间的实数
* `rand(n)`-一个介于 0 和 n-1




### <a name="sqrt"></a>sqrt

平方根函数。  

**语法**

    sqrt(x)

**参数**

* *x:*其实数 > = 0。

**返回**

* 一个正数，`sqrt(x) * sqrt(x) == x`
* `null`如果参数为负，或不能转换为`real`的值。 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>如何

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>日期和时间


[前](#ago) | [dayofmonth](#dayofmonth) | [星期](#dayofweek) |  [关键字](#dayofyear) |[日期部分的](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [现在](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [都](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>日期和时间文本

|||
---|---
**日期时间**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|时间始终都是以 utc 格式。 忽略日期使今天的一次。
`now()`|当前的时间。
`now(`-*时间跨度*`)`|`now()-`*时间跨度*
`ago(`*时间跨度*`)`|`now()-`*时间跨度*
**时间跨度**|
`2d`|2 天
`1.5h`|1.5 小时 
`30m`|30 分钟
`10s`|10 秒
`0.1s`|0.1 秒
`100ms`| 100 毫秒
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 天
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>日期和时间的表达式

表达式 |结果
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |较低
`<=`|小于或等于
`>` |更多
`>=`|大于或等于
`<>`|不等于
`!=`|不等于 




### <a name="ago"></a>前

从当前的 UTC 时钟时间给定时间跨度值中减去。 像`now()`，此函数可以被多次重复使用语句中所引用的 UTC 时钟时间将是相同的所有实例化。

**语法**

    ago(a_timespan)

**参数**

* *a_timespan*︰ 从当前的 UTC 时钟时间减去的时间间隔 (`now()`)。

**返回**

    now() - a_timespan

**示例**

时间戳过去小时中的所有行︰

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>日期部分

    datepart("Day", datetime(2015-12-14)) == 14

提取指定的日期部分的整数。

**语法**

    datepart(part, datetime)

**参数**

* `part:String`-{"年"、"月"、"天"、"小时"、"分钟"、"秒"、"毫秒"、"微秒"、"纳秒"}
* `datetime`

**返回**

Long 类型的值表示指定的部件。


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

在每月一天中的序号。

**语法**

    dayofmonth(a_date)

**参数**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>星期

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

整数天数上一个周日，为`timespan`。

**语法**

    dayofweek(a_date)

**参数**

* `a_date`: A `datetime`.

**返回**

`timespan`自午夜开始上述星期日，向下舍入到整数天数。

**示例**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>关键字

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

一年中一天中的序号。

**语法**

    dayofyear(a_date)

**参数**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday、 endofweek、 endofmonth、 endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getmonth

从日期时间获取月数 (1-12)。

**示例**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

从日期时间中获取年份。

**示例**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>现在

    now()
    now(-2d)

当前的 UTC 时钟时间，还可以抵消给定的时间跨度。 此函数可以被多次重复使用在一个语句中，被引用的时钟时间将所有实例相同。

**语法**

    now([offset])

**参数**

* *的偏移量︰*A `timespan`、 添加到当前的 UTC 时间。 默认值︰ 0。

**返回**

当前的 UTC 时钟时间为`datetime`。

    now() + offset

**示例**

标识该谓词的事件以来确定的时间间隔︰

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday、 startofweek、 startofmonth、 startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>都

别名`datetime()`。

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

检查一个字符串是否是有效的日期︰

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>totimespan

别名`timespan()`。

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

该整数结果表示通过标准 ISO 8601 中的周数。 一周的第一天是星期日，和一年的第一周是一周，其中包含该年的第一个星期四。 （部分下一年，周 1 天，因此可以包含一年的最后一天或第一天可以包含一些周 52 或 53 的上一年度）。


## <a name="string"></a>字符串

[countof](#countof) | [提取](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [替换](#replace) | [拆分](#split) | [strcat](#strcat) | [strlen](#strlen) | [子字符串](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>字符串文字

规则是 JavaScript 中一样。

可以在单一或双重引号字符括字符串。 

反斜杠 (`\`) 用于转义字符，如`\t`（选项卡）， `\n` （换行符） 和封闭引号字符的实例。

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>经过模糊处理的字符串

经过模糊处理的字符串常值是字符串，输出的字符串 （例如，跟踪时） 时，会隐藏分析。 模糊处理过程启动的所有模糊的字符替换 (`*`) 字符。

要形成经过模糊处理的字符串文本，请在`h`或 H。 例如︰

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>字符串比较

运算符|说明|区分大小写|真正的示例
---|---|---|---
`==`|等于 |是的| `"aBc" == "aBc"`
`<>` `!=`|不等于|是的| `"abc" <> "ABC"`
`=~`|等于 |不| `"abc" =~ "ABC"`
`!~`|不等于 |不| `"aBc" !~ "xyz"`
`has`|右键左 (RHS) 是整个术语中左左 (LHS)|不| `"North America" has "america"`
`!has`|RHS 不是完全在 LHS 术语|不|`"North America" !has "amer"` 
`hasprefix`|RHS LHS 中词的前缀是|不|`"North America" hasprefix "ame"`
`!hasprefix`|LHS 中任何术语的前缀不是 RHS|不|`"North America" !hasprefix "mer"`
`hassuffix`|RHS 是术语的后缀的 LHS|不|`"North America" hassuffix "rth"`
`!hassuffix`|并不是 RHS LHS 中任何术语的后缀|不|`"North America" !hassuffix "mer"`
`contains` | RHS 发生作为 LHS 的子字符串|不| `"FabriKam" contains "BRik"`
`!contains`| RHS LHS 中不会出现|不| `"Fabrikam" !contains "xyz"`
`containscs` | RHS 发生作为 LHS 的子字符串|是的| `"FabriKam" contains "Kam"`
`!containscs`| RHS LHS 中不会出现|是的| `"Fabrikam" !contains "Kam"`
`startswith`|RHS 是 LHS 初始字符串。|不|`"Fabrikam" startswith "fab"`
`!startswith`|RHS 不 LHS 初始字符串。|不|`"Fabrikam" !startswith "abr"`
`endswith`|RHS 是终端的 LHS 的子串。|不|`"Fabrikam" endswith "kam"`
`!endswith`|RHS 不 LHS 的终端子字符串。|不|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS 包含 RHS 的匹配项|是的| `"Fabrikam" matches regex "b.*k"`
`in`|等于的任何元素|是的|`"abc" in ("123", "345", "abc")`
`!in`|不等于任何元素|是的|`"bc" !in ("123", "345", "abc")`

使用`has`或`in`如果您要测试是否存在的整个词法的术语-，即一个或多个字母的单词被包围，非字母数字字符或开始或字段的末尾。 `has`执行速度更快，比`contains`，`startswith`或`endswith`。 第一个这些查询的运行速度更快︰

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

计算字符串中子字符串的匹配项。 普通的字符串匹配可能会重叠;正则表达式匹配不这样做。

**语法**

    countof(text, search [, kind])

**参数**

* *文本︰*一个字符串。
* *搜索︰*普通的字符串或正则表达式匹配*的文本*内部。
* *类︰*`"normal"|"regex"`默认`normal`。 

**返回**

可以在该容器中匹配的搜索字符串的次数。 普通的字符串匹配可能会重叠;正则表达式匹配不这样做。

**示例**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (不 2 ！)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>提取

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

获得从一个文本字符串的[正则表达式](#regular-expressions)匹配。 或者，它将提取子字符串转换为指定的类型。

**语法**

    extract(regex, captureGroup, text [, typeLiteral])

**参数**

* *正则表达式︰*一个[正则表达式](#regular-expressions)。
* *captureGroup:*正`int`常量，表示要提取的捕获组。 0 代表整个匹配，匹配的正则表达式，2 次或更后面的括号中的第一个 （括号） 的值为 1。
* *文本︰*A`string`进行搜索。
* *typeLiteral:*可选的类型文本 (例如， `typeof(long)`)。 如果提供，则将提取子字符串转换为该类型。 

**返回**

如果*正则表达式*查找匹配*文本*中︰ 根据指定的捕获组*captureGroup*，匹配的子字符串 （可选） 转换为*typeLiteral*。

如果没有匹配项，或类型转换将失败︰ `null`。 

**示例**

该示例字符串`Trace`的定义，将会搜索`Duration`。 匹配转换为`real`，然后乘以时间常数 (`1s`)，以便`Duration`的类型为`timespan`。 在此示例中，它等于 123.45 秒︰

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

此示例等效于`substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>isempty，isnotempty notempty

    isempty("") == true

如果参数为空字符串或 null，则返回 true。
请参阅[isnull](#isnull)。


**语法**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**返回**

指示参数是否为空字符串或 isnull。

|x|isempty(x)
|---|---
| "" | 真
|"x" | 假
|parsejson("")|真
|parsejson("[]")|假
|parsejson("{}")|假


**示例**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

拆分成各个部分的 URL。

**语法**

    parseurl(urlstring)

**参数**

* *urlstring:*一个 URL。

**返回**

一个对象，它包含作为字符串的部分。

**示例**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>替换

替换为另一个字符串的正则表达式的所有匹配项。

**语法**

    replace(regex, rewrite, text)

**参数**

* *正则表达式︰*[正则表达式](https://github.com/google/re2/wiki/Syntax)来搜索*文本*。 它可以包含在捕获组中 （括号'）。 
* *重写︰*替换为*matchingRegex*所做的任何匹配的 regex。 使用`\0`来指代整个匹配，`\1`的第一个捕获组，`\2`等用于后续的捕获组。
* *文本︰*一个字符串。

**返回**

之后使用*重写*评测替换*正则表达式*的所有匹配项的*文本*。 匹配不重叠。

**示例**

此语句︰

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

具有以下结果︰

| x    | str | 已更换|
|---|---|---|
| 1    | 数字是 1.000000  | 数字︰ 1.000000|
| 2    | 号码是 2.000000  | 号码为︰ 2.000000|
| 3    | 号码是 3.000000  | 号码为︰ 3.000000|
| 4    | 号码是 4.000000  | 号码为︰ 4.000000|
| 5    | 号码是 5.000000  | 号码为︰ 5.000000|
 



### <a name="split"></a>拆分

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

拆分给定分隔符根据给定的字符串，并返回包含 conatined 的子字符串的字符串数组。 （可选），可以为一个特定的子字符串返回的如果存在。

**语法**

    split(source, delimiter [, requestedIndex])

**参数**

* *来源*︰ 将根据给定的分隔符拆分源字符串。
* *分隔符*︰ 将用于拆分源字符串的分隔符。
* *requestedIndex*︰ 可选的从零开始索引`int`。 如果提供，返回的字符串数组将包含请求的子字符串，如果存在。 

**返回**

一个字符串数组，其中包含由指定的分隔符分隔的指定的源字符串的子字符串。

**示例**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

1 至 16 参数，它必须是字符串串联。

### <a name="strlen"></a>strlen

    strlen("hello") == 5

字符串的长度。

### <a name="substring"></a>子字符串

    substring("abcdefg", 1, 2) == "bc"

从指定索引开始的给定的源字符串中提取子字符串。 还可以指定请求的子字符串的长度。

**语法**

    substring(source, startingIndex [, length])

**参数**

* *来源︰*子字符串将来自源字符串。
* *启动索引︰*从零开始起始字符的位置请求的子字符串。
* *长度︰*一个可选参数，用于指定子字符串中的所请求的字符数。 

**返回**

从给定字符串的子字符串。 子字符串启动索引 （从零开始） 的字符位置开始，并持续到结尾的字符串或长度的字符，如果指定。

**示例**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

将字符串转换为小写。

### <a name="toupper"></a>toupper

    toupper("hello") == "HELLO"

将字符串转换为大写。



### <a name="guids"></a>Guid

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>数组、 对象和动态

[原义字符](#dynamic-literals) | [铸造](#casting-dynamic-objects) | [运算符](#operators) | [let 子句](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [范围](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [压缩](#zip)


下面是应用程序的见解异常对查询的结果。 中的值`details`是一个数组。

![](./media/app-insights-analytics-reference/310.png)

**索引︰**索引数组和 JavaScript 中一样的对象︰

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* 但是，使用`arraylength`和其他分析功能 (不".length"！)

**强制转换**在某些情况下有必要强制转换从对象中提取元素，因为其类型可能不同。 例如，`summarize...to`需要特定类型︰

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**原义字符**若要创建显式数组或属性包对象，将其保存为一个 JSON 字符串并强制转换︰

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:**若要提取分离对象的属性单独成行，使用 mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:**若要查找复杂对象中所有的路径︰

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:**若要查找承认所有表中表达式的值的最小架构︰

    exceptions | summarize buildschema(details)

结果︰

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

请注意，`indexer`用来标记应使用数字索引的位置。 此架构中，一些有效的路径，就 （假设这些示例索引是否在范围内）︰

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>数组和对象文本

若要创建一个动态文本，请使用`parsejson`(别名`todynamic`) 与 JSON 字符串参数︰

* `parsejson('[43, 21, 65]')`-数字的数组
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-包含一个数字的动态类型的单个值
* `parsejson('"21"')`-包含字符串的动态类型的单个值

请注意，与不同的 JavaScript，JSON 规定使用双引号 (`"`) 在字符串周围。 因此，它是通常可以容易地报价使用 JSON 编码的字符串文本使用单引号 (`'`)。

此示例创建一个动态值，然后使用其字段︰

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>动态对象函数

|||
|---|---|
| *值*`in` *数组*| 如果*数组*中的元素的*值*= =<br/>`where City in ('London', 'Paris', 'Rome')`
| *值*`!in` *数组*| 如果没有元素的*数组*，= =*值*
|[`arraylength(`数组`)`](#arraylength)| 如果它不是一个数组，则返回空
|[`extractjson(`路径、 对象`)`](#extractjson)|使用路径来导航到对象。
|[`parsejson(`来源`)`](#parsejson)| 变成一个动态对象的 JSON 字符串。
|[`range(`从，，到步骤`)`](#range)| 值的数组
|[`mvexpand`列表列](#mvexpand-operator) | 将复制的行中的指定单元格的列表中的每个值。
|[`summarize buildschema(`列`)`](#buildschema) |推断从列内容类型架构
|[`summarize makelist(`列`)`](#makelist)| 平展行组，并将列中的值放在数组中。
|[`summarize makeset(`列`)`](#makeset) | 平展行组，并将列中的值放在数组中，而无需重复。

### <a name="dynamic-objects-in-let-clauses"></a>在 let 子句中的动态对象


[Let 子句](#let-clause)存储动态值作为字符串，因此这两个子句是等效的并都需要`parsejson`(或`todynamic`) 之前正在使用︰

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

动态数组中元素的数目。

**语法**

    arraylength(array)

**参数**

* *数组︰*A`dynamic`值。

**返回**

*数组*中的元素的数目或`null`如果*数组*不是一个数组。

**示例**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

获取指定的元素路径表达式使用 JSON 文本。 还可以将提取的字符串转换为特定类型。


**语法**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**返回**

到数据源，其中包含有效的 JSON 字符串，还可以将值转换为另一种类型，这取决于第三个参数，此函数执行 JsonPath 查询。



**示例**

[方括号] 表示法和点表示法是等价的︰

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**性能提示**

* 在使用前应用 where 子句`extractjson()`
* 可以考虑[提取](#extract)与使用正则表达式匹配。 这可以运行在很大程度得更快，并在从模板生成 JSON 才有效。
* 使用`parsejson()`如果您需要从 JSON 中提取多个值。
* 请考虑让 JSON 分析在接收通过声明列的类型是动态。

### <a name="json-path-expressions"></a>JSON 路径表达式

|||
|---|---|
|`$`|根对象|
|`@`|当前对象|
|`[0]`|数组下标|
|`.`或`[0]` | 子|

*（我们不当前实现通配符、 递归、 联合或切片。*




### <a name="parsejson"></a>parsejson

解释`string`作为[JSON 值](http://json.org/)) 和返回的值与`dynamic`。 它是优于使用`extractjson()`时您需要提取的 JSON 复合对象的多个元素。

**语法**

    parsejson(json)

**参数**

* *json:*JSON 文档。

**返回**

类型的对象`dynamic`指定的*json*。

**示例**

在以下示例中，当`context_custom_metrics`是`string`，如下所示︰ 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

以下片段检索的值，然后`duration`插槽在对象中，并从其检索两个插槽、`duration.value`和 `duration.min`(`118.0`和`110.0`，分别)。

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>范围

`range()`函数 (请不要混淆与`range`运算符) 生成一个动态数组，包含一系列的等间距分布的值。

**语法**

    range(start, stop, step)

**参数**

* *开始︰*结果数组中的第一个元素的值。 
* *停止︰*结果数组中的最后一个元素的值或大于最后一个元素，则生成的数组和一个整数的最小值多*步骤*从*开始*。
* *步骤︰*数组中的两个连续的元素之间的差异。

**示例**

下面的示例返回`[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

下面的示例返回一个数组，该数组包含 2015 年的所有天︰

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

将字符串转换为一个动态值。

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

枚举所有路径表达式，标识在动态对象的叶子。 

**返回**

路径表达式的数组。

**示例**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

注意该 [0] 表示一个数组中，存在但未指定使用特定路径的索引。

### <a name="zip"></a>压缩

    zip(list1, list2, ...)

将一组列表组合为一个元组列表。

* `list1...`︰ 值的列表

**示例**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>名称

名称可以是最多 1024年个字符长。 区分大小写，可以包含字母、 数字和下划线 (`_`)。 

引用名称使用 [...'] or [" ..."] 包含其它字符，或将关键字用作名称。 例如︰

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|[路径\\file\n\'x\'] | 使用 \ 字符进行转义|
|"d-e.=/f#\n" | |
|[@'path\file'] | 没有转义符的 \ 是文本|
|[@"\now& 然后\"] | |
|[位置] | 使用语言关键字作为一个名称|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


