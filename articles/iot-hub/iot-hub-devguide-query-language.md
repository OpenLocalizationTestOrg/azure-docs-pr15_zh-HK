<properties
 pageTitle="开发人员指南-查询语言 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-从 IoT 中心检索信息 twins、 方法和作业所用的查询语言的说明"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>引用-twins 和作业的查询语言

## <a name="overview"></a>概述

IoT 中心提供了功能强大的类似于 SQL 的语言，以检索有关[设备 twins]信息[lnk-twins]和[作业][lnk-jobs]。 本文给出了︰

* IoT 中枢的查询语言，主要功能介绍和
* 语言的详细的描述。

## <a name="getting-started-with-twin-queries"></a>双子星查询入门

[设备 twins] [lnk-twins]可以包含任意 JSON 对象作为标记和属性。 作为一个 JSON 文档包含所有双子星信息查询设备 twins 允许 IoT 集线器。
例如，假定您 IoT 集线器 twins 具有下列结构︰

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT 集线器将设备 twins 公开为一个名为**设备**的文档集合。
因此下面的查询中检索设备 twins 的整个集︰

        SELECT * FROM devices

> [AZURE.NOTE] [IoT 集线器 Sdk] [lnk-hub-sdks]支持的大型结果分页。

IoT 中心允许检索 twins 使用任意条件进行筛选。 例如，

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

使用**location.region**标记设置为**我们**检索 twins。
布尔运算符和算术比较支持的例如

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

检索所有 twins 位于配置为发送遥测频率每分钟少于美国。 为方便起见，它也可能是与**中**和**NIN** （不在） 运算符一起使用数组常量。 例如，

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

检索所有 twins 报告 wifi 或有线连接。 [WHERE 子句]引用[lnk-query-where]筛选功能的完整参考一节。

此外支持分组和聚合。 例如，

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

返回每个遥测配置状态的设备数。

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

上面的示例中阐释了其中三个设备报告成功配置，两个仍然应用配置，和一个报告错误的情况。

### <a name="c-example"></a>C# 示例

查询功能公开的[C# 服务 SDK] [lnk-hub-sdks]在**RegistryManager**类。
下面是查询的一个简单示例︰

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

请注意页面大小 （最多为 1000)，如何实例化**查询**对象，然后通过多次调用**GetNextAsTwinAsync**方法可以检索多个页面。
务必要注意查询对象公开了多个**下\***、 根据反序列化选项所需的查询，如双子星或作业对象或普通 Json 用于使用投影。

### <a name="node-example"></a>节点的示例

查询功能由[节点服务 SDK] [lnk-hub-sdks]在**注册表**对象。
下面是查询的一个简单示例︰

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

请注意页面大小 （最多为 1000)，如何实例化**查询**对象，然后通过多次调用**nextAsTwin**方法可以检索多个页面。
务必要注意查询对象公开了多个**下\***、 根据反序列化选项所需的查询，如双子星或作业对象或普通 Json 用于使用投影。

### <a name="limitations"></a>限制

目前，使用聚合时，才支持计划，即非聚合查询只能使用`SELECT *`。 此外，结合分组仅支持聚合。

## <a name="getting-started-with-jobs-queries"></a>开始作业的查询

[作业][lnk-jobs]提供一种方法来执行对设备的操作。 每个设备双子星包含的是一个名为**的作业**集合中的一部分的作业的信息。
在逻辑上，

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

目前，此集合是可作为**devices.jobs** IoT 中心查询语言中的查询。

例如，要获得影响单个设备的所有作业 （过去和计划），可以使用下面的查询︰

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

请注意，此查询提供的特定于设备的状态 （和可能的直接方法响应） 返回每个作业的了。
还有可能使用**devices.jobs**集合中的对象的所有属性上的任意布尔条件进行筛选。
例如，下面的查询︰

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

检索所有已完成的双子星更新作业设备**myDeviceId** 9 月 2016年之后创建。

还有可能要检索单个作业的每个设备的成果。

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>限制
目前，不支持**devices.jobs**上的查询︰

* 预测，即只`SELECT *`是可能的;
* 请参阅作业属性，如上所示; 除了设备双子星的条件
* 执行聚合，如计数、 平均值、 分组依据。

## <a name="basics-of-an-iot-hub-query"></a>IoT 中心查询的基础知识

每个 IoT 中心查询包括选择和 FROM 子句和可选位置和 GROUP BY 子句。 每个查询的 JSON 文档，例如设备 twins 集合上运行。 FROM 子句表示文档集合进行迭代 （**设备**或**devices.jobs**）。 然后，应用 WHERE 子句中的筛选器。 对于聚合，此步骤的结果组合成指定 GROUP BY 子句中，并为每个组，则生成一个行根据 SELECT 子句中指定。

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM 子句

**从 < from_specification >**子句可以假设只有两个值︰**从设备**到查询设备 twins 或**从 devices.jobs**，查询作业每个设备的详细信息。

## <a name="where-clause"></a>WHERE 子句

**在其中 < filter_condition >**子句是可选的。 它指定从集中的 JSON 文档必须满足才会包含在结果的条件。 任何 JSON 文档必须取值为"true"要包括在结果中所指定的条件。

[表达式和条件]一节中描述的允许的条件[lnk-query-expressions]。

## <a name="select-clause"></a>SELECT 子句

SELECT 子句 （**选择 < select_list >**） 是强制性的并指定值将从查询中检索。 它指定要用于生成新的 JSON 对象的筛选 （和 （可选） 分组） 从集合的子集的每个元素的投影阶段生成新的 JSON 对象，用 SELECT 子句中指定的值来构造的 JSON 值。

这是 SELECT 子句的语法︰

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

其中**attribute_name**是指任何属性从集合的 JSON 文档。 SELECT 子句的一些示例可在[入门双子星查询][lnk-query-getstarted]部分。

目前，选择子句不同于**选择\***twins 在聚合查询中只支持。

## <a name="group-by-clause"></a>GROUP BY 子句

**GROUP BY < group_specification >**子句是一个可选步骤，可以执行后在 WHERE 子句中，并指定在选择投影之前指定的筛选器。 将一组属性的值所基于的文档。 这些组用于生成聚合在 SELECT 子句中指定的值。

使用 GROUP BY 的一个示例是查询的︰

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

分组依据的正规语法是︰

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

其中**attribute_name**是指任何属性从集合的 JSON 文档。 

目前，仅支持 GROUP BY 子句，查询 twins 时。

## <a name="expressions-and-conditions"></a>表达式和条件

在高级别，*表达式*︰

* 计算结果为 （即布尔值、 数字、 字符串、 数组或对象），JSON 类型的实例和
* 由操作数据来自设备 JSON 文档并使用内置运算符和函数的常数定义。

*条件*是计算结果为布尔型，即任何与布尔值**true**为**false** （包括**空**、**未定义**，任何对象或数组的实例、 任何字符串和清楚地布尔值**false**） 被视为不同的常量的表达式。

表达式的语法是︰

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

其中︰

| 符号 | 定义 |
| -------------- | -----------------|
| attribute_name | 从集合中的 JSON 文档的任何属性。 |
| unary_operator | 根据运算符部分所有的一元运算符。 |
| binary_operator | 根据运算符部分任何二进制运算符。 |
| decimal_literal | 以十进制表示法表示浮点数。 |
| hexadecimal_literal | 一个由字符串"0x"跟十六进制数字的字符串表示的数字。 |
| 字符串文字 | 字符串文字是 Unicode 字符串由零个或多个 Unicode 字符序列或转义序列。 字符串括在单引号 (撇号: ') 或双引号 (引号:")。 允许使用转义符︰ `\'`， `\"`， `\\`，`\uXXXX`由 4 位十六进制数字的 Unicode 字符。 |

### <a name="operators"></a>运算符

支持下列运算符︰

| 系列 | 运算符 |
| -------------- | -----------------|
| 算术运算 | +,-,*,/,% |
| 逻辑 | 和，或不 |
| 比较 |  =、 ！ =，<>，，< =、 > =、 <> |

## <a name="next-steps"></a>下一步行动

了解如何在您的应用程序使用[IoT 集线器 Sdk]中执行查询[lnk-hub-sdks]。

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md