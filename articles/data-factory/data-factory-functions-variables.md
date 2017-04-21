<properties 
    pageTitle="数据工厂函数和系统变量 |Microsoft Azure" 
    description="提供 Azure 数据工厂函数和系统变量的列表" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure 数据工厂-函数和系统变量
本文提供了有关函数和变量 Azure 数据工厂支持的信息。
  
## <a name="data-factory-system-variables"></a>数据工厂系统变量

变量名称 | 说明 | 对象作用域 | JSON 范围和使用案例
------------- | ----------- | ------------ | ------------------------
WindowStart | 当前活动窗口中运行的时间间隔开始日期 | 活动 | <ol><li>指定数据的选择查询。 请参阅连接器[数据移动活动](data-factory-data-movement-activities.md)文章中引用的文章。</li><li>将参数传递给配置单元脚本 （如上所示的示例）。</li>
WindowEnd | 当前活动窗口中运行的时间间隔结束 | 活动 | 与上面的相同
SliceStart | 所生成的数据切片的时间间隔开始日期 | 活动<br/>数据集 | <ol><li>使用[Azure Blob](data-factory-azure-blob-connector.md)和[文件系统数据集](data-factory-onprem-file-system-connector.md)时，指定动态文件夹路径和文件名。</li><li>数据工厂函数活动输入集合中与指定输入依赖项。</li></ol>
SliceEnd | 当前正在生成的数据切片的时间间隔结束 | 活动<br/>数据集 | 与上面的相同。 

> [AZURE.NOTE] 当前数据工厂需要严格指定如果在活动中的计划符合指定输出数据集的可用性方面的计划。 这意味着 WindowStart、 WindowEnd 和 SliceStart 以及 SliceEnd 始终映射到相同的时间段和单个输出切片。
 
## <a name="data-factory-functions"></a>数据工厂函数 

可以在数据工厂连同上面提到的系统变量使用函数，用于以下目的︰

1.  指定数据的选择查询 （请参阅连接器文章所引用的[数据移动活动](data-factory-data-movement-activities.md)文章。

    若要调用数据工厂函数的语法是︰**$$**数据的选择查询和活动和数据集中的其他属性。  
2. 在活动数据工厂函数指定输入依赖项输入集合 （请参见上面的示例）。

    $$ 不需要指定输入依赖项的表达式。   

在下面的示例中， **sqlReaderQuery**在 JSON 文件中的属性赋予**Text.Format**函数返回一个值。 此示例还使用一个名为**WindowStart**，它表示活动窗口运行的开始时间的系统变量。
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>函数

下表列出了在 Azure 数据工厂的所有功能︰

类别 | 函数 | 参数 | 说明
-------- | -------- | ---------- | ----------- 
时间 | AddHours(X,Y) | X︰ 日期时间 <br/><br/>Y: int | 向给定时间 X 小时 Y。 <br/><br/>示例︰ 2013/9/5 12:00:00 + 2 小时 = 9/5/2013年下午 2:00:00
时间 | AddMinutes(X,Y) | X︰ 日期时间 <br/><br/>Y: int | 向 X Y 分钟。<br/><br/>示例︰ 2013/9/15 下午 12: 00:00 + 15 分钟 = 9/15/2013年下午 12: 15:00
时间 | StartOfHour(X) | X︰ 日期时间 | 获取起始时间为 X 的小时组成部分表示的小时。 <br/><br/>例如︰ StartOfHour 9/15/2013年下午 05: 10:23 是 9/15/2013年 05: 00:00
日期 | AddDays(X,Y) | X︰ 日期时间<br/><br/>Y: int | 向 X Y 天。<br/><br/>示例︰ 2013/9/15 下午 12:00:00 + 2 天 = 9/17/2013年下午 12:00:00
日期 | AddMonths(X,Y) | X︰ 日期时间<br/><br/>Y: int | 向 X Y 个月。<br/><br/>示例︰ 2013/9/15 下午 12:00:00 + 1 月 = 10/15/2013年下午 12:00:00 
日期 | AddQuarters(X,Y) | X︰ 日期时间 <br/><br/>Y: int | 添加 Y * X 到 3 个月。<br/><br/>示例︰ 2013/9/15 下午 12:00:00 + 1 季度 = 12/15/2013年下午 12:00:00
日期 | AddWeeks(X,Y) | X︰ 日期时间<br/><br/>Y: int | 添加 Y * X 之前 7 天<br/><br/>示例︰ 2013/9/15 下午 12:00:00 + 1 周 = 9/22/2013年下午 12:00:00
日期 | AddYears(X,Y) | X︰ 日期时间<br/><br/>Y: int | 向 X Y 年。<br/><br/>示例︰ 2013/9/15 下午 12:00:00 + 1 年 = 9/15/2014年下午 12:00:00
日期 | 之内处理完 | X︰ 日期时间 | 获取的日部分中的 X。<br/><br/>示例︰ 一天的 9/15/2013年是 9，12:00:00。 
日期 | DayOfWeek(X) | X︰ 日期时间 | 获取 X 周部分的一天。<br/><br/>示例: 星期 9/15/2013年是星期日下午 12:00:00。
日期 | DayOfYear(X) | X︰ 日期时间 | 获取中的年部分中的 X 表示该年的一天。<br/><br/>示例︰<br/>12/1/2015年: 2015年 335 天<br/>12/31/2015年: 2015年 365 天<br/>12/31/2016年: 366 天 （闰年） 2016年
日期 | DaysInMonth(X) | X︰ 日期时间 | 获取的月部分中的参数 X 表示的月中的天数。<br/><br/>例如︰ DaysInMonth 9/15/2013年的由于在 9 月每月有 30 天是 30。
日期 | EndOfDay(X) | X︰ 日期时间 | 获取表示 X 天 （每天组件） 的结束日期-时间。<br/><br/>例如︰ EndOfDay 9/15/2013年下午 05:10:23 是 9/15/2013年下午 11:59:59。
日期 | EndOfMonth(X) | X︰ 日期时间 | 获取表示的月部分中的参数 X 个月的结束。 <br/><br/>例如︰ EndOfMonth 9/15/2013年下午 05:10:23 是 9/30/2013 11:59:59 （代表 9 月月底的日期时间）
日期 | StartOfDay(X) | X︰ 日期时间 | 获取所表示的日部分中的参数 X 天的开始。<br/><br/>例如︰ StartOfDay 9/15/2013年下午 05:10:23 是 9/15/2013年 12:00:00 AM。
日期时间 | From(X) | X︰ 字符串 | 分析为日期时间字符串 X。
日期时间 | Ticks(X) | X︰ 日期时间 | 获取刻度参数 X 的属性。一个刻度等于 100 毫微秒为单位。 此属性的值表示 0001 年 1 月 1 日午夜 12:00:00 以来所经历的计时周期数。 
文本 | Format(X) | X︰ 字符串变量 | 设置文字的格式。

#### <a name="textformat-example"></a>Text.Format 示例

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

请参见[自定义日期和时间格式字符串](https://msdn.microsoft.com/library/8kb3ddd4.aspx)主题描述不同的格式选项，您可以使用 (例如︰ yy 与 yyyy)。 

> [AZURE.NOTE] 使用在其他函数中的功能时，您不需要使用**$$**的内部函数的前缀。 例如︰ $$Text.Format (PartitionKey eq \\'my_pkey_filter_value\\和 RowKey ge \\' {0:yyyy-毫米-dd︰ 分︰ 秒}\\'，Time.AddHours (SliceStart，-6))。 在此示例中，请注意， **$$** **Time.AddHours**函数不使用前缀。 
  

