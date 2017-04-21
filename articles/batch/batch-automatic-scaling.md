<properties
    pageTitle="自动缩放比例计算节点在 Azure 批池中 |Microsoft Azure"
    description="启用自动缩放云池动态调整池中的计算节点的数目上。"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>自动缩放 Azure 批池中的计算节点

自动缩放，Azure 批服务可以动态添加或删除在池中根据您定义的参数的计算节点。 您可能会节省时间和金钱通过自动调整大小的应用程序 — 使用的计算能力将节点添加为您的工作任务需求增加，然后将其删除时它们会降低。

启用自动缩放池的计算节点上的定义，如*自动缩放公式*与它相关联的[PoolOperations.EnableAutoScale] [ net_enableautoscale] [批.NET](batch-dotnet-get-started.md)库中的方法。 批次服务然后使用以下公式确定计算节点来执行您的工作负载所需的数目。 批次响应服务标准定期收集到的数据样本，并调整池中按可配置的时间间隔基于公式的计算节点的数目。

您可以启用自动缩放时创建池时，或在现有池。 您还可以更改现有公式上的池，则"自动缩放"启用。 批次提供之前将它们分配到池，计算公式，以及监控的自动缩放运行状态的能力。

## <a name="automatic-scaling-formulas"></a>自动缩放公式

自动缩放公式是一个字符串值，定义包含一个或多个语句，并分配到池的[autoScaleFormula] [rest_autoscaleformula]元素 (批处理 REST) 或[CloudPool.AutoScaleFormula] [net_cloudpool_autoscaleformula]属性 (批处理.NET)。 当分配到池，批服务使用公式确定目标的池中的计算节点处理 （在以后的时间间隔更多） 的下一个间隔。 公式的字符串不能超过 8KB 的大小，可以包括最多 100 个语句用分号分隔，可以包含分行符和注释。

您可以将自动缩放公式与使用批处理自动缩放比例"语言"。 配方的语句是服务定义的变量 （由批服务定义的变量） 和用户定义的变量 （您定义的变量） 可以包含的自由格式的表达式。 通过使用内置类型、 运算符和函数，它们可以执行各种操作这些值。 例如，语句可能会采取以下形式︰

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

公式通常包含对值执行操作，可获得上一个语句中的多个语句。 例如，首先我们获得一个值为`variable1`，然后将其传递到一个函数来填充`variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

使用这些语句在公式中，您的目标是到达数池应缩放--的计算节点的**专用的节点****目标**数。 这个数字可能会更高，低，或与当前池中的节点数相同。 批处理计算池的自动缩放公式按特定时间间隔 （下面将讨论[自动缩放时间间隔](#automatic-scaling-interval)）。 然后它将调整目标池中自动缩放公式指定的是在计算时数为节点数。

作为一个快速的示例，下面两行自动缩放比例的公式指定的节点数，应调整根据活动任务，最多 10 个计算节点数︰

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

本文下面几节讨论的各种实体构成自动缩放公式，包括变量、 运算符、 操作和函数。 您将了解如何获取批内的各种计算资源和任务指标。 这些指标可用于智能地调节池的节点计数根据资源利用率和任务状态。 然后，您将学习如何构造公式并启用自动缩放池上使用批处理的剩余部分和.NET Api。 我们将完成的几个示例公式。

> [AZURE.IMPORTANT] Azure 批处理中的每个帐户，可用于处理核心 （和计算节点） 的最大数目受到限制。 批次服务将创建最核心限制的节点。 因此，它不可能达到目标数的计算节点所指定的公式。 查看和增加帐户配额的信息，请参阅[配额和 Azure 批服务的限制](batch-quota-limit.md)。

## <a name="variables"></a>变量

自动缩放比例公式中，您可以使用同时**服务定义**和**用户定义**的变量。 服务定义变量是内置于批次服务中，一些读写的和一些都是只读的。 用户定义的变量是*您*定义。 在上面两行的示例公式`$TargetDedicated`是服务定义的变量，而`$averageActiveTaskCount`是用户定义的变量。

下表显示批服务定义的读-写和只读变量。

您可以**获取**和**设置**用于管理池中的计算节点的序号这些服务定义的变量的值︰

| 读写的服务定义变量 | 说明 |
| --- | --- |
| $TargetDedicated | 为该池的**专用计算节点**数**目标**。 这是池应缩放到的计算节点的数目。 因为池无法到达的节点数的目标可能是一个"目标"数字。 如果目标节点数再次通过后续自动缩放比例计算修改前池已达到初始目标，这将会发生。 如果批处理帐户或核心节点配额达到之前达到目标节点数，它同样会发生。 |
| $NodeDeallocationOption | 计算节点要从池中删除时发生的操作。 可能值包括︰<ul><li>**requeue**-立即终止任务，并将它们放回在作业队列中，以便他们将重新排定日程。<li>**终止**— 立即终止任务并从工作队列中删除它们。<li>**taskcompletion**-等待当前运行的任务可以完成，然后从池中删除该节点。<li>**retaineddata**-等待清理从池中删除节点之前的节点上的所有本地任务保留数据。</ul> |

您可以**获取**这些服务定义的变量来进行基于从批服务指标的调整的值︰

| 只读的服务定义的变量 | 说明 |
| --- | --- |
| $CPUPercent | CPU 使用率平均百分比。 |
| $WallClockSeconds | 消耗的秒数。 |
| $MemoryBytes | 平均使用的兆字节数。 |
| $DiskBytes | 在本地磁盘上使用千兆字节的平均数量。 |
| $DiskReadBytes | 读取的字节数。 |
| $DiskWriteBytes | 写入的字节数。 |
| $DiskReadOps | 执行的读的磁盘操作的计数。 |
| $DiskWriteOps | 执行写入磁盘操作的计数。 |
| $NetworkInBytes | 入站字节数。 |
| $NetworkOutBytes | 出站字节数。 |
| $SampleNodeCount | 计算节点计数。 |
| $ActiveTasks | 处于活动状态的任务的数目。 |
| $RunningTasks | 在正在运行状态的任务数目。 |
| $PendingTasks | $ActiveTasks 和 $RunningTasks 的总和。 |
| $SucceededTasks | 已成功完成的任务的数目。 |
| $FailedTasks | 失败的任务的数目。 |
| $CurrentDedicated | 当前数量的专用计算节点。 |

> [AZURE.TIP] 上面显示的只读、 服务定义变量是提供各种方法，以访问与每个相关联的数据的*对象*。 有关更多信息，请参见下面的[获取样本数据](#getsampledata)。

## <a name="types"></a>类型

在公式中支持这些**类型**。

- 双精度
- doubleVec
- doubleVecList
- 字符串
- 时间戳，时间戳是复合的结构，其中包含以下成员︰

    - 年份
    - 月 (1-12)
    - 一天 (1-31)
    - 工作日 （中号，例如，星期一的 1 的格式）
    - 小时 （在 24 小时的数字格式，例如 13 指下午 1 点）
    - 分钟 (00-59)
    - 第二个 (00-59)
- timeinterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>操作

这些**操作**可以在上面列出的类型。

| 操作                             | 受支持的运算符   | 结果类型   |
| ------------------------------------- | --------------------- | ------------- |
| 双*运算符*双              | +, -, *, /            | 双精度            |
| timeinterval 双*运算符*        | *                     | timeinterval      |
| doubleVec*运算符*双           | +, -, *, /            | doubleVec         |
| doubleVec*运算符*doubleVec        | +, -, *, /            | doubleVec         |
| timeinterval*运算符*双        | *, /                  | timeinterval      |
| timeinterval*运算符*timeinterval  | +, -                  | timeinterval      |
| timeinterval*运算符*时间戳     | +                     | 时间戳         |
| 时间戳*运算符*timeinterval     | +                     | 时间戳         |
| *操作员*的时间戳的时间戳        | -                     | timeinterval      |
| *运算符*的双                      | -, !                  | 双精度            |
| timeinterval*运算符*                | -                     | timeinterval      |
| 双*运算符*双              | < < =、 = =、 > =、 >，！ =  | 双精度            |
| 字符串*运算符*              | < < =、 = =、 > =、 >，！ =  | 双精度            |
| *操作员*的时间戳的时间戳        | < < =、 = =、 > =、 >，！ =  | 双精度            |
| timeinterval*运算符*timeinterval  | < < =、 = =、 > =、 >，！ =  | 双精度            |
| 双*运算符*双              | & &、 & #124; & #124;      | 双精度            |

当测试三元运算符具有一个双精度值 (`double ? statement1 : statement2`)、 非零值为**true**，且零为**false**。

## <a name="functions"></a>函数

这些预定义的**函数**是可供您用于定义自动缩放公式的形式。

| 函数                          | 返回类型   | 说明
| --------------------------------- | ------------- | --------- |
| avg(doubleVecList)                | 双精度        | 在 doubleVecList 中返回的所有值的平均值。
| len(doubleVecList)                | 双精度        | 返回从 doubleVecList 创建向量的长度。
| lg(double)                        | 双精度        | 返回日志的双基 2。
| lg(doubleVecList)                 | doubleVec     | 返回 componentwise 日志基 2 的 doubleVecList。 Vec(double) 必须显式传递参数。 否则，则假定为双 lg(double) 版本。
| ln(double)                        | 双精度        | 返回双精度型的自然对数。
| ln(doubleVecList)                 | doubleVec     | 返回 componentwise 日志基 2 的 doubleVecList。 Vec(double) 必须显式传递参数。 否则，则假定为双 lg(double) 版本。
| log(double)                       | 双精度        | 返回日志的双以 10 为基。
| log(doubleVecList)                | doubleVec     | 返回 componentwise 日志的 doubleVecList 以 10 为基。 Vec(double) 必须显式传递一个双参数中。 否则，则假定为双 log(double) 版本。
| max(doubleVecList)                | 双精度        | 在 doubleVecList 中返回的最大值。
| min(doubleVecList)                | 双精度        | 在 doubleVecList 中返回最小值。
| norm(doubleVecList)               | 双精度        | 返回从 doubleVecList 创建向量的两个标准。
| 百分点值 (双精度 p doubleVec v) | 双精度        | 返回向量 v 的百分点值元素。
| rand （)                            | 双精度        | 返回一个随机值介于 0.0 和 1.0 之间。
| range(doubleVecList)              | 双精度        | 在 doubleVecList 中返回最小值和最大值之间的差异。
| std(doubleVecList)                | 双精度        | 在 doubleVecList 中返回值的样本标准偏差。
| stop （)                            |               | 停止自动缩放表达式的计算结果。
| sum(doubleVecList)                | 双精度        | 返回 doubleVecList 的所有组成部分的总和。
| 时间 (日期时间字符串 ="")          | 时间戳     | 如果未不传递任何参数，如果当前时间的时间戳或时间戳的日期时间字符串传递给它。 受支持的日期时间格式为 W3C DTF 和 RFC 1123。
| val (doubleVec v，双 i)        | 双精度        | 返回的元素的位置，则 i 向量 v 中以零起始的索引值。

一些上面的表中描述的函数可接受列表作为参数。 以逗号分隔的列表是*双*和*doubleVec*的任意组合。 例如︰

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList*值将转换为单个*doubleVec*之前求值。 例如，如果`v = [1,2,3]`，然后再调用`avg(v)`等效于调用`avg(1,2,3)`。 调用`avg(v, 7)`等效于调用`avg(1,2,3,7)`。

## <a name="getsampledata"></a>获取示例数据

自动缩放比例公式运算上提供的批次服务度量数据 （示例）。 一个公式增大或缩小池大小根据它从服务中获取的值。 上面描述的服务定义的变量将提供各种方法，以访问与该对象相关联的数据的对象。 例如，下列表达式显示一个请求以获取 CPU 使用率的最后五分钟︰

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| 方法 | 说明 |
| --- | --- |
| GetSample() | `GetSample()`方法返回的数据样本向量。<br/><br/>一个示例是值得的度量数据的 30 秒。 换句话说，样本获取每隔 30 秒。 但如下面所述，还有何时收集样本时，可用公式之间存在延迟。 在这种情况下，在给定的时间段内并不是所有样本都可能可用于计算的公式。<ul><li>`doubleVec GetSample(double count)`<br/>指定从最近所收集的样本获取的样本数。<br/><br/>`GetSample(1)`返回最后一个可用的示例。 衡量标准等`$CPUPercent`，但是，这不应因为它是不可能知道*在*收集样本时。 它可能是新的或者，由于系统问题，可能是要古老得多。 最好是在这种情况下，要使用如下所示的时间间隔。<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>指定用于收集样本数据的时限。 另外，它还指定必须在请求的时间框架中可用的样本的百分比。<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`是否存在 CPUPercent 历史记录中的所有示例的最后十分钟，就会返回 20 个采样。 如果历史记录的最后一分钟没有，但是，则会返回只有 18 样本。 在这种情况下︰<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`将失败，因为只有 90%的样本有。<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`都会成功。<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>指定一个时间范围来收集数据，具有开始时间和结束时间。<br/><br/>正如前面提到，没有在收集样本时，时，可用公式之间的延迟。 这必须被认为是当您使用`GetSample`方法。 请参阅`GetSamplePercent`之下。|
| GetSamplePeriod() | 历史样本数据集中返回的期间所取的样本。 |
| Count() | 返回的跃点的历史记录中的样本总数。 |
| HistoryBeginTime() | 返回最早的可用数据示例度量的时间戳。 |
| GetSamplePercent() |返回给定的时间间隔已有样本的百分比。 例如︰<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>因为`GetSample`如果返回样本的百分比小于`samplePercent`指定，您可以使用`GetSamplePercent`方法来检查第一次。 然后可以执行其他操作，如果没有足够的样本都存在，而无需停止自动缩放比例计算。|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>示例、 示例百分比和*GetSample()*方法

核心操作的自动缩放公式是获取任务和资源的指标数据，然后调整基于该数据的池大小。 在这种情况下，务必要清楚地了解自动缩放公式与交互的方式度量数据或"示例"。

**示例**

批次服务定期采用*样本*的任务和资源方面的标准并将它们提供给您的自动缩放公式。 这些示例批处理服务记录每隔 30 秒。 但是，有通常是一定程度的延迟，导致延迟之间那些样本的记录时间和供 （并且可以通过读取） 时自动缩放公式。 此外，由于各种因素，如网络或其他基础设施问题，样本可能未记录的特定时间间隔。 这将导致"缺少"的样本。

**抽样百分比**

当`samplePercent`被传递给`GetSample()`方法或`GetSamplePercent()`方法被调用时，"百分比"是指总*可能*由批服务记录的样本数和实际*提供*给您的自动缩放公式的样本数之间的比较。

让我们看一下 10 分钟时间跨度为例。 因为样本记录每隔 30 秒，在 10 分钟时间跨度中，记录的批次的样本的最大总数将为 20 个采样 (每分钟 2 个)。 但是，由于报告机制或 Azure 的基础结构中的一些其他问题固有的滞后，有可能只有 15 可供阅读自动缩放公式的示例。 这意味着，对于该 10 分钟期间，只有**75%**的记录的样本总数实际上提供给您的公式。

**GetSample() 和样本范围**

自动缩放比例公式将会增大和缩小您的池-添加节点或删除节点。 因为节点需要花钱的您希望确保您的公式，使用足够的数据为基础的分析的智能方法。 因此，我们建议您在公式中使用趋势分析类型分析。 此类型将增长和收缩好池根据收集到的样本*范围*。

为此，请使用`GetSample(interval look-back start, interval look-back end)`返回的样本**向量**︰

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

当批处理计算上面的行时，它将返回样本的范围为值的向量。 例如︰

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

一旦您已收集的样本向量，然后可以使用功能，如`min()`， `max()`，和`avg()`的收集范围从中派生出有意义的值。

为了提高安全性，您可以强制到*失败*的公式计算如果一定样本百分比小于可用在特定时间段内。 当您强制执行失败的公式计算时，指示批停止进一步的公式计算，如果指定的百分比的示例不是--并且不会进行池大小不变。 若要指定所需的样本数的百分比进行评估是成功的指定的第三个参数为`GetSample()`。 在这里，指定样本的 75%的要求︰

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

它也是重要的是，由于前面提到在样品实用性，可以始终指定一个时间范围超过一分钟的外观后开始时间延迟。 这是因为需要大约一分钟，对样本用于传播到系统中，因此取样范围内`(0 * TimeInterval_Second, 60 * TimeInterval_Second)`通常不能使用。 同样，可使用的百分比参数`GetSample()`强制一个特定的示例百分比要求。

> [AZURE.IMPORTANT] 我们**强烈建议**，**避免依赖于*仅*在`GetSample(1)`在您自动缩放公式 * *。 这是因为`GetSample(1)`实质上是说给批机构中，"给我了，不管你的多久上一次采样。" 由于它只是一个单一样本，并且它可能是一个较旧的示例，它可能不代表的新的任务或资源状态的较大的图片。 如果您使用`GetSample(1)`，请确保它是一个更大的语句并不依赖于您的公式的唯一的数据点的一部分。

## <a name="metrics"></a>指标

当您定义公式时，可以使用**资源**和**任务**指标。 调整的目标根据获取和评估的指标数据池中的专用节点数。 请参见[变量](#variables)上面详细信息每个跃点计数。

<table>
  <tr>
    <th>跃点计数</th>
    <th>说明</th>
  </tr>
  <tr>
    <td><b>资源</b></td>
    <td><p><b>资源指标</b>基于计算节点以及节点数的 CPU、 带宽和内存使用情况。</p>
        <p> 这些服务定义的变量可用于调整基于节点计数︰</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>这些服务定义的变量可用于调整基于节点资源使用情况︰</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>任务</b></td>
    <td><p><b>任务指标</b>是基于状态的任务，例如活动、 挂起、 和完成。 以下服务定义变量可用于进行池大小调整根据任务指标︰</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>编写公式的自动缩放比例

通过使用上述组件中，语句生成自动缩放公式，然后合并到完整公式中的那些语句。 在本节中，我们将创建示例自动缩放公式可以执行一些实际比例的决策。

首先，我们定义一下我们新的自动缩放公式的要求。 该公式应︰

1. **增加**目标池中如果 CPU 使用率很高的计算节点数。
2. **降低**CPU 使用率较低时池中的计算节点的目标数。
3. 始终将**最大**节点数限制为 400。

为*增加*期间 CPU 使用率过高的节点数，我们将定义填充用户定义变量的语句 (`$totalNodes`) 在最后 10 分钟的最小平均 CPU 使用率是 70%以上使用的值是当前的节点，但仅当目标数的 110%。 否则，我们将使用专用的当前值。

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

为*减少*在低 CPU 使用率的节点数，我们的公式中的下一语句设置相同`$totalNodes`变量为当前节点是否在过去的 60 分钟的平均 CPU 使用率低于 20%的目标数的 90%。 否则，请使用当前值的`$totalNodes`，我们在上面的语句填充。

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

现在限制到**最大**400 的专用的计算节点的目标数量︰

```
$TargetDedicated = min(400, $totalNodes)
```

下面是完整的公式︰

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>创建已启用自动缩放的池

要创建一个新池启用自动缩放，可以使用以下方法之一︰

**批次.NET**

1. 使用[BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx)创建池。
1. 将[CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx)属性设置为`true`。
1. 设置[CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx)属性的自动缩放公式。
1. （可选）将[CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx)属性设置 （默认值为 15 分钟）。
1. 提交与[CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx)或[CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx)的池。

**批次 REST API，**

* [添加到帐户池](https://msdn.microsoft.com/library/azure/dn820174.aspx)︰ 指定`enableAutoScale`和`autoScaleFormula`中您要配置自动按比例缩放池创建它时的 REST API 请求的元素。

下面的代码段使用[批处理.NET]创建一个已启用自动缩放的池[net_api]库。 该池的自动缩放公式设置目标的节点数为 5 上周一, 和上一周每隔一天的 1。 [自动缩放的间隔](#automatic-scaling-interval)设置为 30 分钟。 这和其他 C# 代码段在这篇文章中的，在"myBatchClient"是[BatchClient]的正确初始化的实例[net_batchclient]。

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

除了批 REST API 和.NET SDK，您可以使用任一[批 Sdk](batch-technical-overview.md#batch-development-apis)、[批 PowerShell cmdlet](batch-powershell-cmdlets-get-started.md)和[批处理 CLI](batch-cli-get-started.md)使用自动缩放。

> [AZURE.IMPORTANT] 您必须在创建时启用自动缩放的池，**未**指定`targetDedicated`参数。 此外，如果您希望手动调整已启用自动缩放的池 (例如，对于[BatchClient.PoolOperations.ResizePool][net_poolops_resizepool])，然后您必须第一个**禁用**自动缩放在池中，然后调整其大小。

### <a name="automatic-scaling-interval"></a>自动缩放比例间隔

默认情况下，批服务每隔**15 分钟**调整根据其自动缩放公式的池的大小。 此时间间隔是可配置的但是，通过使用以下池属性︰

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] （批处理.NET）
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

最小间隔是 5 分钟，和最大值为 168 小时。 如果指定的时间间隔，在此范围之外，则批处理服务将返回错误的请求 (400) 错误。

> [AZURE.NOTE] 自动缩放当前不用于响应更改，在不到一分钟，但而不是用来调整池逐渐当您运行一个工作负荷的大小。

## <a name="enable-autoscaling-on-an-existing-pool"></a>启用自动缩放上现有池

如果您已经创建一个池一定数量的计算节点通过使用*targetDedicated*参数，您仍然可以启用池上的自动缩放。 每个批次 SDK 提供了"启用自动缩放比例"操作，例如︰

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] （批处理.NET）
*  [启用自动缩放池上][ rest_enableautoscale] (REST API)

启用自动缩放上现有池时，以下规则适用︰

* 如果自动按比例缩放目前池上**禁用**时发出的"启用自动缩放比例"请求，您*必须*指定有效的自动缩放公式时发出请求。 *（可选）*您可以指定自动缩放比例计算时间间隔。 如果不指定一个时间间隔，则使用默认值为 15 分钟。

* 如果自动缩放比例目前池上**启用**，您可以指定自动缩放公式、 计算时间间隔，或二者。 这两个属性，则不能将其省略。

  * 如果指定自动缩放比例计算时间间隔，然后停止现有的评估计划，并启动新的计划。 新的计划开始时间是"启用自动缩放比例"请求发出的。

  * 如果您省略自动缩放公式或计算时间间隔、 批服务继续使用该设置的当前值。

> [AZURE.NOTE] 如果在创建池时为*targetDedicated*参数指定值，则被忽略自动缩放公式进行求值时。

该 C# 代码段使用[批处理.NET] [net_api]库，以使自动缩放上现有池︰

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>更新公式的自动缩放比例

在现有的已启用自动缩放的池上使用相同的"启用自动缩放比例"请求*更新*公式 (例如，对于[EnableAutoScale] [net_enableautoscale]批次.NET 中)。 没有特殊"更新自动缩放"操作。 例如，如果执行下面代码时"myexistingpool"已启用自动缩放，其自动缩放公式将被替换的内容`myNewFormula`。

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>更新自动缩放时间间隔

更新自动缩放公式，在使用相同的[EnableAutoScale] [net_enableautoscale]方法来更改现有已启用自动缩放的池的自动缩放比例计算时间间隔。 例如，若要将自动缩放比例计算时间间隔设置为已启用自动缩放的池的 60 分钟︰

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>自动缩放比例公式求值

您可以将它应用于一个池之前评估公式。 这种方式，您可以执行"测试运行"的公式，以将公式放到生产环境之前的语句的计算结果，请参阅。

自动缩放公式求值，则必须第一个**启用自动缩放**上具有**有效的公式**的池。 如果您想要测试还没有启用自动缩放池上的公式，则可以使用单行公式`$TargetDedicated = 0`首先启用自动缩放。 然后，使用下列选项之一以您想要测试的公式求值︰

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx)或[EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    这些批处理.NET 方法需要评估现有池和一个包含自动缩放公式字符串的 ID。 评估结果包含在返回的[AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx)实例。

* [自动缩放公式求值](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    在此 REST API 请求，指定 URI 中的池 ID 和请求正文中的*autoScaleFormula*元素中的自动缩放公式。 该操作的响应包含任何可能与公式相关的错误信息。

在此[批.NET] [net_api]代码段中，我们为公式求值之前将其应用于[CloudPool][net_cloudpool]。 如果该池不具有已启用自动缩放，但我们让它第一次。

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

鉴别成功的公式，在此段中将产生与下面类似的输出︰

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>获取有关自动缩放运行的信息

若要确保您的公式按预期方式运行，我们建议您定期检查结果的自动缩放批处理执行您的池的"运行"。 做这样，获取 （或刷新） 参考池，检查其运行的最后一次自动缩放属性。

批.NET 中的[CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx)属性具有多个属性提供有关最新自动按比例缩放通过批服务运行执行池上的信息。

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

在 REST API，[获取有关池的信息](https://msdn.microsoft.com/library/dn820165.aspx)请求返回池中，其中包括最新自动按比例缩放以[autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)运行信息的信息。

下面的 C# 代码段使用批处理.NET 库打印池"myPool"上运行的最后一次自动缩放的有关信息︰

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

前一段的示例输出︰

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>自动缩放比例公式的示例

让我们看一看几个公式显示调整量的计算资源池中的不同方法。

### <a name="example-1-time-based-adjustment"></a>示例 1︰ 基于时间的调整

也许您想要调整根据一周中的天和一天时间，以增大或减小池中的节点数进行相应的池大小。

此公式首先获得当前时间。 如果它是一个工作日 (1-5)，工作时间 （上午 8 点到下午 6 点） 内目标池大小设置为 20 个节点。 否则，它就设置 10 个节点。

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>示例 2︰ 基于任务的调整

在此示例中，池的大小是根据队列中的任务数调整。 注意注释和换行的公式的字符串中可以接受。

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>示例 3︰ 核算的并行任务

这是基于任务数的池大小调整的另一个例子。 该公式同时考虑了[MaxTasksPerComputeNode] [net_maxtasks]已经为该池设置的值。 这是在已启用您的池的[并行任务执行](batch-parallel-node-tasks.md)的情况下特别有用。

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>示例 4︰ 设置初始池大小

此示例演示使用初始时间段将池的大小设置为一定数量的节点自动缩放公式的 C# 代码段。 然后它调整池的大小根据运行的数量和活动任务的初始时间段过去后。

下面的代码段中的公式︰

- 将初始池大小设置为四个节点。
- 在池的生命周期的第一个 10 分钟内不会调整池的大小。
- 10 分钟后，获得的最大值的数目在过去的 60 分钟内正在运行并处于活动状态的任务。
  - 如果这两个值是 0 （表示没有任务未运行或活动在前 60 分钟），池大小设置为 0。
  - 如果其中任何一个值是否大于零，则会不进行任何更改。

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>下一步行动

* [最大化 Azure 批处理计算节点并行任务的资源使用](batch-parallel-node-tasks.md)包含有关如何您可以执行多个任务同时在池中的计算节点上的详细信息。 除了自动缩放，此功能可帮助降低某些工作负载，从而节省资金的作业周期。

* 另一种效率增强器，确保您的批处理应用程序以最佳方式查询批次服务。 [高效查询批处理 Azure 服务](batch-efficient-list-queries.md)，请在中，您将学习如何限制交叉线路查询可能上千个计算节点或任务的状态时的数据量。

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
