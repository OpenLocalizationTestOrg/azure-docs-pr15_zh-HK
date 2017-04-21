<properties 
    pageTitle="如何监控云服务 |Microsoft Azure" 
    description="了解如何使用 Azure 的传统门户网站监控云服务。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>如何监控云服务

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

您可以监视`key`Azure 的经典门户中对云服务的性能指标。 您可以设置监视到最小并为每个服务角色，详细的级别，并可以自定义监视显示。 详细的监视数据存储中存储帐户，可以访问门户以外。 

监视在 Azure 的传统门户网站中显示是高度可配置的。 您可以选择您想要监视在**监视器**页面上的指标列表中的度量标准，您可以选择哪些指标**监视器**页面和操控板上的图表中绘制的标准。 

## <a name="concepts"></a>概念

默认情况下，最少的监控提供一种新的云服务，使用性能计数器收集从主机操作系统的角色实例 （虚拟机）。 最小的指标仅限于 CPU 百分比、 数据中，传出的数据、 磁盘读吞吐量和磁盘写入吞吐量。 通过配置详细的监视，您可以接收基于虚拟机 （角色实例） 中的性能数据的其他指标。 详细的指标可以更仔细地分析应用程序操作过程中出现的问题。

默认情况下角色实例的性能计数器数据是取样，每隔 3 分钟从角色实例转移。 当您启用详细的监视时，原始的性能计数器数据进行聚合为每个角色实例和每个角色的角色实例之间间隔为 5 分钟，1 小时和 12 小时。 10 天后被清除的聚合的数据。

启用详细的监视后，聚合的监视数据存储在表中存储帐户。 若要启用详细监视角色，必须链接到存储帐户配置诊断程序的连接字符串。 可以为不同的角色使用不同的存储帐户。

请注意，启用详细的监视会增加您的存储成本与数据存储、 数据传输和存储交易记录相关。 最少监视不需要存储帐户。 在最少的监视级别公开的度量标准数据不存储在您的存储帐户，即使您设置为详细的监视级别。


## <a name="how-to-configure-monitoring-for-cloud-services"></a>如何︰ 配置监控云服务

使用以下过程来配置详细或最少的监控在 Azure 的传统门户网站中。 

### <a name="before-you-begin"></a>在开始之前

- 创建存储帐户存储监视数据。 可以为不同的角色使用不同的存储帐户。 有关详细信息，请参阅帮助的**存储帐户**，或请参阅[如何创建存储帐户](/manage/services/storage/how-to-create-a-storage-account/)。

- 对于您的云服务角色启用 Azure 诊断。 请参阅[配置诊断为云服务](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore)。

确保在角色配置有诊断连接字符串。 您不能打开详细监视，直到启用 Azure 诊断程序和诊断程序连接字符串包含在角色配置。   

> [AZURE.NOTE] 项目目标 Azure SDK 2.5 未在项目模板中自动包括诊断程序的连接字符串。 对于这些项目，您需要手动添加到角色配置的诊断程序的连接字符串。

**若要手动添加到角色配置的诊断程序的连接字符串**

1. 在 Visual Studio 中打开了云服务项目
2. 双击要打开在角色设计器，然后选择**设置**选项卡的**角色**
3. 查找名为**Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**的设置。 
4. 如果没有此设置，则单击**添加设置**按钮，将新设置的类型更改为**连接字符串**并将其添加到配置
5. 设置连接字符串的值**...**按钮上单击。 这将打开一个对话框，该对话框允许您选择的存储帐户。

    ![Visual Studio 设置](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>将监视级别更改为详细或最小

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)，打开**配置**页中的云服务部署。

2. 在**级别**上，单击**详细**或**最小**。 

3. 单击**保存**。

打开详细的监视之后，您应该开始看到 Azure 的传统门户网站中的监控数据，在一个小时内。

原始的性能计数器数据和聚合的监视数据存储在由部署 ID 的角色限定的表中存储帐户。 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>如何︰ 接收警报的云服务标准

您可以接收基于云服务监控指标的警报。 在 Azure 的传统门户网站的**管理服务**页上，可以创建规则以您选择的指标达到您指定的值时触发预警。 您也可以选择当事件被触发时发送的电子邮件。 有关详细信息，请参阅[如何︰ 接收警报通知和 Azure 中管理预警规则](http://go.microsoft.com/fwlink/?LinkId=309356)。

## <a name="how-to-add-metrics-to-the-metrics-table"></a>如何︰ 添加到度量表的指标

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，打开**监视器**页面的云服务。

    默认情况下，指标表显示可用的度量值的子集。 下图显示一个云服务，它只限于 Memory\Available 兆字节数性能计数器，用角色级别的聚合数据的默认详细指标。 使用**添加度量值**来选择其他的聚合和角色级别指标，以监视 Azure 的传统门户网站。

    ![详细显示](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. 将度量值添加到度量表︰

    1. 单击以打开**选择的度量值**，如下所示**添加度量值**。

        第一个可用指标被展开以显示可用的选项。 为每个度量，顶部选项显示所有角色聚集监视数据。 此外，您可以选择要显示的数据的各个角色。

        ![添加度量值](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. 若要选择显示的标准

        - 通过展开监视选项的跃点数，请单击箭头。
        - 选择您希望显示每个监视选项的复选框。

        规格表中，可以显示最多 50 个指标。

        > [AZURE.TIP] 在详细的监视，指标列表可以包含几十个指标。 若要显示一个滚动条，请将鼠标悬停在对话框的右侧。 若要筛选列表，请单击搜索图标，在搜索框中输入文本，如下所示。
    
        ![添加度量值搜索](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. 选择度量值之后，单击确定 （复选标记）。

    所选的度量值添加到度量表，如下所示。

    ![监视器指标](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. 规格表中删除一个指标，指标来选择它，单击，然后单击**删除指标**。 （您只能看到**删除指标**时已选定的指标。）

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>将自定义度量值添加到度量表

监视级别**详细**提供门户上的默认度量值，您可以监视的列表。 此外，您可以监视任何自定义度量值或通过门户应用程序所定义的性能计数器。

以下步骤假定您在**详细**监视级别上打开，并且已配置应用程序来收集和传输自定义性能计数器。 

在门户中显示自定义性能计数器，您需要更新 wad 控件容器中的配置︰
 
1. 打开您的诊断程序存储帐户 wad 控件容器 blob。 可以使用 Visual Studio 或任何其他存储浏览器来执行此操作。

    ![Visual Studio 的服务器资源管理器](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. 定位使用**DeploymentId/角色/RoleInstance**的模式来查找角色实例的配置为 blob 路径。 

    ![Visual Studio 的存储资源管理器](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. 下载您角色的实例的配置文件并更新其包含的任何自定义性能计数器。 例如，若要监视*驱动器 C*的*磁盘写入的字节数/秒* **PerformanceCounters\Subscriptions**节点下添加

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. 保存所做的更改并将配置文件上载到覆盖该 blob 中的现有文件相同的位置。
5. 切换到 Azure 的经典门户配置中详细模式。 如果已在详细模式中，必须切换到最小并返回到详细。
6. 自定义性能计数器将现在会出现在**添加度量值**对话框中。 

## <a name="how-to-customize-the-metrics-chart"></a>如何︰ 自定义指标的图表

1. 在指标表中，选择最多 6 度量指标图表上绘制。 若要选择一个度量，请单击位于其左侧的复选框。 若要从标准图表中删除一个指标，清除指标表中其复选框。

    指标表中选择度量值时，度量值添加到标准图表。 在缩小显示， **n 多个**下拉列表包含指标不适合显示的标题。

 
2. 若要显示的相对价值 （最终只为每个度量值） 和绝对数值 （Y 轴显示） 之间切换，选择相对或绝对图表的顶部。

    ![相对或绝对](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. 若要更改时间范围指标图表显示，在图表的顶部选择 1 小时、 24 小时或 7 天。

    ![监视器显示周期](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    在仪表板指标图表上绘制标准的方法是不同的。 一套标准的指标，也可添加或删除选择公制头指标。

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>若要自定义仪表板上的度量标准图表

1. 打开为云服务的仪表板。

2. 添加或删除图表中的度量标准︰

    - 要绘制一个新的指标，选择在图表标题中的度量复选框。 窄的展品，由***n*??metrics**绘制指标无法显示图表标题区域中单击下箭头。

    - 要删除在图表绘制的跃点数，请清除复选框，通过它的标头。

3. **相对**和**绝对**之间切换显示。

4. 选择 1 小时、 24 小时或要显示的数据的 7 天。

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>如何︰ 访问详细监视 Azure 经典门户外部数据

详细的监视数据存储在表中为每个角色指定的存储帐户。 对于每个云服务部署，六个表创建的角色。 创建两个表的每个 （5 分钟，1 小时和 12 小时）。 这些表中的一个存储角色级别聚合;另一个表存储角色实例的聚合。 

表名称采用以下格式︰

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

其中︰

- *deploymentID*是云服务部署与分配的 GUID

- *aggregation_interval* = 5 分钟、 1 小时或 12 H

- 角色级别聚合 = R

- 为角色实例聚合 = RI

例如下, 表将存储详细监视数据聚合按 1 小时间隔︰

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
