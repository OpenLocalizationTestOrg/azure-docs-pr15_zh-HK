<properties 
    pageTitle="监视和管理 Azure 数据工厂管线" 
    description="了解如何使用监视和管理应用程序来监视和管理 Azure 数据工厂和管道。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>监视和管理新的监视和管理应用程序使用 Azure 数据工厂管线
> [AZURE.SELECTOR]
- [使用 Azure 门户/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
- [使用监视和管理应用程序](data-factory-monitor-manage-app.md)

本文介绍了如何监视、 管理和调试您的管线，以及创建通知，以便获得有关使用**监视和管理应用程序**的故障通知。 您还可以观看下面的视频来了解如何使用监视和管理应用程序。
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>启动监视和管理应用程序
若要启动监视和管理应用程序，请单击数据工厂**数据工厂**刀片式服务器上的**监视和管理**平铺。

![监视数据工厂主页上的平铺](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

您应该看到一个单独的选项卡/窗口中启动的监视和管理应用程序。  

![监视和管理应用程序](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] 如果您查看 web 浏览器被卡住在"授权..."，禁用/取消选中**禁止第三方 cookie 和站点数据**设置 （或） 保持启用**login.microsoftonline.com**例外和创建然后尝试再次启动该应用程序。


如果看不到活动窗口底部的列表中，单击刷新列表工具栏上的**刷新**按钮。 此外，设置正确的值的**开始时间**和**结束时间**的筛选器。  


## <a name="understanding-the-monitoring-and-management-app"></a>了解的监视和管理应用程序
在左侧有三个选项卡 （**资源管理器**、**监视视图**和**警报**），默认情况下选中第一个选项卡 （资源管理器）。 

### <a name="resource-explorer"></a>资源管理器
请参阅以下内容︰ 

- 资源浏览器**树视图**的左窗格中。
- **关系图视图**顶部。
- **活动窗口**的中间窗格中底部的列表。
- **属性**/**活动窗口资源管理器**的右窗格中的选项卡。 

在资源管理器中，可以看到数据工厂在树视图中的所有资源 （管道、 数据集、 链接的服务）。 在资源管理器中选择对象时，请注意以下︰ 

- 在关系图视图中突出显示关联的数据工厂实体。
- 关联活动窗口 (单击[此处](data-factory-scheduling-and-execution.md)了解有关活动窗口) 底部活动窗口列表中突出显示。  
- 在属性窗口的右窗格中所选对象的属性。 
- 如果适用的所选对象的 JSON 定义。 例如︰ 链接的服务或数据集或管道。 

![资源管理器](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

请参阅[计划和执行](data-factory-scheduling-and-execution.md)有关活动窗口中的详细概念性信息的文章。 

### <a name="diagram-view"></a>关系图视图
数据工厂的关系图视图提供了单一的监视和管理数据工厂和它的资产。 在关系图视图中选择一个数据工厂实体 （数据集/管道） 时，请注意以下︰
 
- 在树视图中选择数据工厂实体
- 在活动窗口列表中突出显示关联的活动窗口。
- 在属性窗口中选定对象的属性

当管道已启用 （未处于暂停状态） 时，该显示，绿线。 

![管线运行](./media/data-factory-monitor-manage-app/PipelineRunning.png)

您会注意到有三个命令按钮，在关系图视图管线。 您可以使用第二个按钮暂停管道。 暂停不终止当前正在运行的活动，让他们继续完成。 第三个按钮暂停管道，并终止其现有的执行活动。 第一个按钮可以恢复管道。 当管道被暂停时，您注意到平铺，如下所示的颜色变化的管道。

![暂停/继续上平铺](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

您可以多重选择 （使用 ctrl 键） 的两个或多个管道，并使用命令栏按钮暂停/恢复多个管线一次。

![暂停/继续命令栏上](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

您可以通过管道铺中，用鼠标右键单击，然后单击**打开管道**看到管道中的所有活动。

![打开管道菜单](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

在打开的管线视图中，您将看到在管道中的所有活动。 在此示例中，没有一个活动︰ 复制活动。 若要返回到以前的视图，请单击顶部的痕迹导航菜单中数据工厂名称。

![打开的管道](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

在管线视图中，当您单击一个输出数据集或当您将鼠标移输出数据集，您将看到活动窗口弹出窗口为该数据集。

![活动窗口弹出式菜单](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

您可以单击活动窗口的右窗格中的**属性**窗口中看到的它的详细信息。 

![活动窗口属性](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

在右窗格中，切换到**活动窗口资源管理器**选项卡以查看更多详细信息。

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

您还可以查看**解析变量**的每个活动的运行试图在**尝试**部分。 

![解析的变量](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

切换到**脚本**选项卡以查看所选对象的 JSON 脚本定义。   

![脚本选项卡](./media/data-factory-monitor-manage-app/ScriptTab.png)

您可以看到活动窗口中的三个位置︰

- 在关系图视图 （中间窗格） 中的活动窗口弹出窗口。
- 活动窗口资源管理器的右窗格中。
- 活动窗口的底部窗格中的列表。

在弹出的活动窗口和活动窗口资源管理器中，可以滚动到上一周和下一周使用左右箭头键。

![活动窗口资源管理器向左/向右箭头](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

在关系图视图的底部，您将看到放大，缩小，缩放以适应、 放大 100%、 锁定版式按钮。 锁定布局按钮防止您意外地在关系图视图中移动表和管线，默认情况下为 ON。 您可以将其关闭并移动实体关系图中。 当您关闭此功能时，可以使用最后一个按钮自动定位表和管线。 此外可以放大 / 缩小使用鼠标滚轮。

![关系图视图缩放命令](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>活动窗口列表
活动窗口列表底部的中间窗格中显示您在资源管理器或关系图视图中选择该数据集的所有活动窗口。 默认情况下，该列表位于降序排序，这意味着，您将看到最新活动窗口的顶部。 

![活动窗口列表](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

此列表不会自动刷新，因此使用工具栏上的刷新按钮手动刷新它。  


活动窗口可以处于以下状态之一︰

<table>
<tr>
    <th align="left">状态</th><th align="left">子状态</th><th align="left">说明</th>
</tr>
<tr>
    <td rowspan="8">等待</td><td>ScheduleTime</td><td>时间已经不是活动窗口运行。</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>上游的依赖项未准备就绪。</td>
</tr>
<tr>
<td>ComputeResources</td><td>计算资源将不可用。</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>所有活动实例都都忙于运行其他活动窗口。</td>
</tr>
<tr>
<td>ActivityResume</td><td>活动已暂停，直至其恢复不能运行的活动窗口。</td>
</tr>
<tr>
<td>重试</td><td>活动执行重试。</td>
</tr>
<tr>
<td>验证</td><td>验证尚未开始。</td>
</tr>
<tr>
<td>ValidationRetry</td><td>正在等待要进行重试的验证。</td>
</tr>
<tr>
<tr
<td rowspan="2">正在进行</td><td>验证</td><td>正在验证。</td>
</tr>
<td></td>
<td>正在处理的活动窗口。</td>
</tr>
<tr>
<td rowspan="4">失败</td><td>超时</td><td>执行比时间长，所允许的活动。</td>
</tr>
<tr>
<td>已取消</td><td>用户操作被取消。</td>
</tr>
<tr>
<td>验证</td><td>验证失败。</td>
</tr>
<tr>
<td></td><td>未能生成和/或验证活动窗口。</td>
</tr>
<td>准备就绪</td><td></td><td>活动窗口可供消耗。</td>
</tr>
<tr>
<td>跳过</td><td></td><td>活动窗口不会处理。</td>
</tr>
<tr>
<td>无</td><td></td><td>活动窗口的使用有不同的状态，但已被重置。</td>
</tr>
</table>


当您单击列表中的活动窗口时，您可以在右侧显示有关该**活动 Windows 资源管理器**或**属性**窗口中的详细信息。

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>刷新活动窗口  
详细信息不会自动刷新，因此您使用**刷新**按钮手动刷新活动窗口列表中的命令栏上 （第二个按钮）。  
 

### <a name="properties-window"></a>属性窗口
在最右侧的窗格中的监视和管理应用程序是属性窗口。 

![属性窗口](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

它将显示在资源管理器 （树型视图） （或） 关系图视图 （或） 活动窗口列表中选定的项的属性。 

### <a name="activity-window-explorer"></a>活动窗口资源管理器

在最右侧的窗格中的监视和管理应用程序是**活动窗口浏览器**窗口。 它显示有关活动窗口弹出或活动窗口列表中选择活动窗口的详细信息。 

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

通过单击顶部的日历视图中，您可以切换到另一个活动窗口。 您还可以使用**左箭头键**/顶部看到活动窗口的上/下一周的**右箭头**按钮。

您可以使用**重新运行**活动窗口或**刷新**的底部窗格中的工具栏按钮详细信息窗格中。 

### <a name="script"></a>脚本 
可以使用**脚本**选项卡以查看所选数据工厂实体 （链接的服务、 数据集和管道） 的 JSON 定义。 

![脚本选项卡](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>使用系统视图
监视和管理应用程序可以包含允许您查看数据工厂最近/失败/进行中活动窗口的预构建的系统视图 （**最近活动窗口**、**活动窗口失败**、**进行中活动窗口**）。 

通过单击切换到**监视视图**选项卡在左边。 

![监视视图选项卡](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

目前，有三个受支持的系统视图。 选择一个选项以查看新活动窗口 （或） 失败的活动窗口 （或） 正在进行中的活动窗口中活动窗口列表 （位于中间的窗格的底部）。 

选择**最近的活动窗口**选项时，您将看到所有最近的活动窗口中的**最后一次尝试时**按降序顺序。 

可以使用**失败活动窗口**视图以查看列表中的所有失败的活动窗口。 失败的活动窗口列表中选择以在**属性**窗口 （或）**活动窗口资源管理器**的详细信息，请参阅。 您还可以下载失败的活动窗口中的任何日志。 


## <a name="sorting-and-filtering-activity-windows"></a>排序和筛选活动窗口
将命令栏中的**开始时间**和**结束时间**设置更改为筛选活动窗口。 开始时间和结束时间更改后，单击按钮以刷新活动窗口列表的结束时间。

![开始时间和结束时间](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] 目前，所有时间都均以 UTC 格式中的监视和管理应用程序。 

在**活动窗口列表**中，单击某一列的名称 (例如︰ 状态)。 

![活动窗口列表列菜单](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

您可以执行以下操作︰

- 按升序顺序排序。
- 按降序顺序排序。
- 筛选器由一个或多个值 （准备就绪，等待，等等。）

在列上指定筛选器时，您会看到启用的字段以指示列中的值筛选的值的筛选器按钮。 

![在活动窗口列表列筛选](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

可以使用同一个弹出窗口以清除筛选器。 若要清除活动的窗口列表的所有筛选，请单击命令栏上的都清除过滤器按钮。 

![清除活动窗口列表中的所有筛选器](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>执行批处理操作

### <a name="rerun-selected-activity-windows"></a>重新运行选定的活动窗口
选择活动窗口，请单击第一个命令栏按钮的向下箭头并选择**重新运行** / **重新运行与上游管道中**。 当您选择**重新运行与上游管道中**的选项时，它将重新运行所有上游活动窗口。 
    ![重新运行活动窗口](./media/data-factory-monitor-manage-app/ReRunSlice.png)

此外可以从列表中选择多个活动窗口，然后在相同的时间重新运行它们。 您可能想要筛选基于状态的活动窗口 (例如︰**失败**)，然后更正导致故障的活动窗口的问题后重新运行失败的活动窗口。 请参阅下一节有关筛选列表中的活动窗口的详细信息。  

### <a name="pauseresume-multiple-pipelines"></a>暂停/恢复多个管线
可以多重选择 （使用 ctrl 键） 的两个或多个管道，并使用命令栏按钮 （在下图中的红色矩形突出显示） 暂停/继续它们一次。

![在命令栏上挂起/恢复](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>创建警报 
通知页，可以创建一个警报，查看、 编辑或删除现有的警报。 您还可以禁用/启用警报。 若要查看通知页，单击警报选项卡。

![警报标识栏](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>若要创建警报

1. 单击**添加警报**添加通知。 查看详细信息页。 

    ![创建警报-详细信息页面](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. 指定的**名称**和**说明**该警报，然后单击**下一步**。 您应该看到**过滤器**页。

    ![创建警报-筛选器页](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. 选择**事件**、**状态**、 和**子状态**（可选） 若要提醒您，该数据工厂服务要在其，然后单击**下一步**。 您应该看到**收件人**页。

    ![创建通知的收件人页](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. 选择**电子邮件订阅管理**选项和/或输入**其他管理员的电子邮件**，并单击**完成**。 您应该看到列表中的预警。 
    
    ![通知列表](./media/data-factory-monitor-manage-app/AlertsList.png)

在通知列表中，使用与编辑/删除/禁用/启用警报的警报关联的按钮。 

### <a name="eventstatussubstatus"></a>子状态状态/事件 /
下表列出的可用事件和状态 （和子状态）。

事件名称 | 状态 | 子状态
-------------- | ------ | ----------
运行启动的活动 | 启动 | 启动
运行完成的活动 | 成功 | 成功 
运行完成的活动 | 失败| 发生故障的资源分配<br/><br/>执行失败<br/><br/>超时<br/><br/>验证失败<br/><br/>放弃
按需 HDI 群集创建启动 | 启动 | &nbsp; |
按需 HDI 群集创建成功 | 成功 | &nbsp; |
按需 HDI 群集删除 | 成功 | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>编辑/删除禁用警报


![警报按钮](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


