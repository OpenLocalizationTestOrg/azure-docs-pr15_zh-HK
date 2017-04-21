<properties 
   pageTitle="使用 Visual Studio 的顶点执行视图中数据湖工具 |Microsoft Azure" 
   description="了解如何使用顶点执行视图到考试数据湖分析作业。" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>使用 Visual Studio 中数据湖工具的顶点执行视图

了解如何使用顶点执行视图到考试数据湖分析作业。

## <a name="prerequisites"></a>系统必备组件

- 使用 Visual Studio 的数据湖工具开发 U SQL 脚本的一些基本知识。  请参阅[教程︰ 开发 U SQL 脚本使用 Visual Studio 数据湖工具](data-lake-analytics-data-lake-tools-get-started.md)。

## <a name="open-the-vertex-execution-view"></a>打开顶点执行视图

对于某些工作，可以单击左下角的"顶点执行查看"链接。 可能会提示您第一次加载配置文件，可能需要一些时间，具体取决于您的网络连接。

![数据湖分析工具顶点执行视图](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>了解顶点执行视图

输入顶点执行视图后, 有三个部分︰

![数据湖分析工具顶点执行视图](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- 顶点选择器︰ 左边是顶点选择器。  （如前 10 个数据读取，或按阶段选择），您可以选择顶点的功能。

    最常见的使用筛选器之一，是在关键路径上的顶点。 关键路径是 U SQL 作业的最长路径。 它可用于优化您的作业通过检查哪些顶点花费最长时间。

- 上方窗格中︰

    ![数据湖分析工具顶点执行视图](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    此视图还显示所有顶点的运行状态。 相应地将时间转换到您本地计算机上，并以不同颜色显示不同的状态。

- 底部中心窗格中︰

    ![数据湖分析工具顶点执行视图](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - 过程名称︰ 顶点实例的名称。 它由不同部分在 StageName |VertexName |VertexRunInstance。 例如，SV7_Split [62].v1 顶点表示第二个正在运行的实例 （.v1，从 0 开始的索引） 的舞台 SV7_Split 中的顶点编号 62。
    - 全部数据读/写︰ 读/写此顶点时数据。
    - 状态/退出状态︰ 最终状态顶点结束时。
    - 退出代码中的故障类型︰ 错误时失败，顶点。
    - 创建原因︰ 为什么顶点已创建。
    - 资源延迟/进程延迟/PN 队列延迟︰ 顶点等待资源，来处理数据，并在队列中保留所需的时间。
    - 当前正在运行的顶点或其创建者的进程/创建者 GUID: GUID。
    - 版本︰ 正在运行 （系统可能安排新顶点的实例很多原因，例如故障切换时，计算冗余，等等。） 的顶点的第 N 个实例
    - 创建时的版本。
    - 处理创建开始时间/进程排队时间/进程开始时间/处理完成时间︰ 当顶点过程开始创建;当顶点进程启动时要排队;当某个顶点进程启动时;当完成某些顶点。

## <a name="next-steps"></a>下一步行动

- 若要获取数据分析湖泊功能的概述，请参阅[Azure 数据湖分析概述](data-lake-analytics-overview.md)。
- 首先开发 U SQL 应用程序，请参见[使用 Visual Studio 的数据湖工具开发 U-SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)。
- U SQL，请参阅[开始使用 Azure 数据湖分析 U SQL 语言](data-lake-analytics-u-sql-get-started.md)。
- 管理任务，请参阅[管理 Azure 数据湖分析使用 Azure 的门户](data-lake-analytics-manage-use-portal.md)。
- 若要记录的诊断信息，请参阅[访问诊断日志中的 Azure 数据湖分析](data-lake-analytics-diagnostic-logs.md)
- 更复杂的查询，请参阅[分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)。
- 要查看作业的详细信息，请参阅[使用作业浏览器和 Azure 数据湖分析作业的作业视图](data-lake-analytics-data-lake-tools-view-jobs.md)