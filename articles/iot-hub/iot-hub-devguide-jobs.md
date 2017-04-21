<properties
 pageTitle="开发人员指南 》 的工作 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-在多个设备上运行作业级排产连接到网络集线器"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>在多个设备 （预览） 的计划作业

## <a name="overview"></a>概述

Azure IoT 中心由先前的文章所述，使大量的构建基块 ([设备两路输出属性和标记][lnk-twin-devguide]和[云至设备方法][lnk-dev-methods])。  通常情况下，IoT 后端应用程序启用设备管理员和操作员更新并 IoT 设备批量和在预定的时间与其进行交互。  作业将封装设备双子星更新和对照一套设备的 C2D 方法在计划的时间执行。  例如，操作员使用的后端应用程序将启动并跟踪作业重新启动一套设备不会对建筑物的操作中断一次构建 43 和基底 3。

### <a name="when-to-use"></a>何时使用

请考虑使用作业时︰ 解决方案后结束来安排和跟踪进度的需求任何一套设备上的下列活动︰

- 更新设备两路输出所需属性
- 更新设备双子星标记
- 调用 C2D 的方法

## <a name="job-lifecycle"></a>作业生命周期

由解决方案后端启动和维护 IoT 中心的作业。  您可以启动某个作业通过面向服务的 URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) 和通过面向服务的 URI 执行作业进度的查询 (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`)。  作业开始后，作业的查询将使后端应用程序以刷新正在运行的作业的状态。

> [AZURE.NOTE] 在启动作业时，属性名称和值只能包含可打印美国 ASCII 字母，以外任何在下列一组︰ ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关使用作业的详细信息。

## <a name="jobs-to-execute-direct-methods"></a>作业执行直接方法

下面是执行[直接方法]的 HTTP 1.1 请求详细信息[lnk-dev-methods]上一套设备使用一个作业︰

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>作业来更新设备两路输出属性

以下是 HTTP 1.1 请求更新使用作业设备两路输出属性的详细信息︰

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>针对作业进度查询

下面是 HTTP 1.1 请求详细信息用于[查询作业][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
从响应提供了 continuationToken。  

## <a name="jobs-properties"></a>作业属性

下面是属性和相应的说明，可用于在查询时作业或作业结果的列表。

| 属性 | 说明 |
| -------------- | -----------------|
| **作业 Id** | 应用程序提供的作业的 ID。 |
| **开始时间** | 应用程序提供作业的开始时间 (iso-8601)。 |
| **结束时间** | IoT 中心提供为完成作业的日期 (ISO 8601)。 该作业达到已完成状态后，才有效。 | 
| **类型** | 作业的类型︰ |
| | **scheduledUpdateTwin**︰ 用于更新一套双子星所需属性或标记的作业。 |
| | **scheduledDeviceMethod**︰ 用于调用设备方法，对两路输出的一组作业。 |
| **状态** | 作业的当前状态。 可能的状态的值︰ |
| | **待定**︰ 计划和等待领取由作业服务。 |
| | **计划**︰ 计划在未来一段时间。 |
| | **运行**︰ 当前处于活动状态的作业。 |
| | **取消**︰ 作业已被取消。 |
| | **失败**︰ 作业失败。 |
| | **完成**︰ 作业已经完成。 |
| **deviceJobStatistics** | 作业的执行有关的统计信息。 |

在预览时，该 deviceJobStatistics 对象可用于仅在作业完成后。

| 属性 | 说明 |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | 作业中的设备数量。 |
| **deviceJobStatistics.failedCount** | 其中作业失败的设备数。 |
| **deviceJobStatistics.succeededCount** | 在该作业成功的设备数。 |
| **deviceJobStatistics.runningCount** | 当前正在运行该作业的设备数。 |
| **deviceJobStatistics.pendingCount** | 处于挂起状态，运行该作业的设备数。 |


### <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣以下 IoT 集线器教程︰

- [计划和广播作业][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
