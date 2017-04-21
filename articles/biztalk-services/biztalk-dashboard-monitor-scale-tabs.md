<properties 
    pageTitle="仪表板监视器、 小数位数和配置，并在 BizTalk 服务混合连接 |Microsoft Azure" 
    description="了解有关这些控件和传统门户选项卡上的 BizTalk 服务的性能监视︰ 仪表板、 显示器、 比例、 配置和混合连接。 MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>查看仪表板、 显示器、 比例、 配置和混合连接选项

BizTalk 服务创建和部署应用程序之后，可以更改某些 BizTalk 服务设置和监视应用程序性能。 

Azure 的传统门户网站打开时，您会自动放在**所有项目**选项卡。 若要查看您的 BizTalk 服务，选择**所有项目**选项卡中的 BizTalk 服务，或选择**BIZTALK 服务**选项卡;然后选择您的 BizTalk 服务名称。

这与下列选项卡中打开一个新窗口。 本主题描述了这些选项卡。

## <a name="quick-start-quick-startquickstart"></a>快速入门 (![快速入门][QuickStart])
根据 BizTalk 服务版本，所有列出的选项可能不可用。 
<table border="1">
    <tr>
        <td><strong>获取工具</strong></td>
        <td>下载了 BizTalk 服务 SDK 在本地开发计算机上安装 Visual Studio 项目模板。 这些模板创建的<strong>BizTalk 服务</strong>(bridge) 和到 BizTalk 服务部署的<strong>BizTalk 服务项目</strong>（变换） Visual Studio 项目。
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">我如何开始使用 Azure BizTalk 服务 SDK</a>和<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalk 服务 SDK 安装</a>列出的步骤开始。
        </td>
    </tr>
    <tr>
        <td><strong>创建伙伴协议</strong></td>
        <td>打开在其中添加合作伙伴并创建 X12，AS2，Azure 上承载 Azure BizTalk 服务门户和 EDIFACT EDI 协议。
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI 消息 BizTalk 服务门户上配置组件</a>列出的步骤开始。
        </td>
    </tr>

<tr>
        <td><strong>了解更多有关 BizTalk 服务</strong></td>
        <td>转到<a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">学习中心</a>了解更多有关 Azure BizTalk 服务。</td>
</tr>
</table>


在任务栏的底部，您可以︰

<table border="1">

<tr>
<td><strong>管理</strong>应用程序部署</td>
<td>打开的 Azure BizTalk 服务门户。 BizTalk 服务门户是 EDI 配置，包括添加合作伙伴和创建 X12，AS2，入口和 EDIFACT 协议。
<br/><br/>
这是<strong>创建合作伙伴协议</strong>相同的<strong>快速启动</strong>选项卡上。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI 消息 BizTalk 服务门户上配置组件</a>BizTalk 服务门户上提供了详细信息。</td>
</tr>

<tr>
<td><strong>连接信息</strong>的访问控制 Namespace</td>
<td>当您选择的连接信息时，访问控制 Namespace，默认的颁发者和默认密钥被显示。 您可以将这些值复制。
<br/><br/>
您还可以打开访问控制门户。 <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">创建访问控制 Namespace</a>访问控制门户上提供了详细信息。</td>
</tr>

<tr>
<td><strong>同步参数</strong>中存储帐户</td>
<td>创建存储帐户时，自动创建主键和辅助键。 这些加密密钥控制对您的存储帐户的访问。 BizTalk 服务会自动使用主键。 <strong>同步参数</strong>使用户为主键和辅助键之间切换而不会中断 BizTalk 服务。
<br/><br/>
例如，您希望对存储帐户使用一个新的主键 BizTalk 服务。 若要此操作︰
<br/><br/>
<ol>
<li>选择您的 BizTalk 服务并选择<strong>同步密钥</strong>。 选择辅助键。 执行此操作时，使用辅助键启动 BizTalk 服务。</li>
<li>在 Azure 经典门户中，选择您的存储帐户，重新生成主键。 请记住，BizTalk 服务使用辅助键。</li>
<li>选择您的 BizTalk 服务并选择<strong>同步密钥</strong>。 现在，选择主键。 这是您重新生成新的主键。</li>
<li>在 Azure 经典门户中，选择您的存储帐户和第二密钥重新生成。</li>
</ol>
<br/>
此过程称为"翻转键"。 目的是使用户能够为主键和辅助键之间切换而不会中断 BizTalk 服务。</td>
</tr>

<tr>
<td><strong>删除</strong>应用程序</td>
<td>当您选择删除 BizTalk 服务和部署到它的所有项目将被都删除。</td>
</tr>
</table>


## <a name="dashboard"></a>仪表板
根据 BizTalk 服务版本，所有列出的选项可能不可用。 

当您选择您的 BizTalk 服务名称时，将显示仪表板选项卡。 在仪表板中，您可以︰

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>使用概述︰ 显示用混合连接数
此外会显示 GB 的数据使用情况。 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>跃点数图︰ 显示性能指标的固定的列表
这些指标提供有关 BizTalk 服务的运行状况的实时值。 您还可以选择的**相对**或**绝对**的值和**间隔**测量数据在图表中显示的时间范围。 

这些性能指标的说明，请转到[可用指标](#Metrics)本主题中。


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>快速浏览︰ 列出 BizTalk 服务属性

<table border="1">

<tr>
<td><strong>更新跟踪数据库凭据</strong></td>
<td>更改的用户名称和密码用于登录到跟踪数据库。</td>
</tr>
<tr>
<td><strong>更新的 SSL 证书</strong></td>
<td>可以更新该 BizTalk 服务以使用不同的 SSL 证书。 时，会自动将创建自行签署式 SSL 证书<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">创建 BizTalk 服务</a>。</td>
</tr>
<tr>
<td><strong>下载证书</strong></td>
<td>您可以下载使用 BizTalk 服务到本地计算机的 SSL 证书。</td>
</tr>
<tr>
<td><strong>状态</strong></td>
<td>显示您的 BizTalk 服务的当前状态。 请参阅<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk 服务︰ 服务状态图表</a>。 </td>
</tr>
<tr>
<td><strong>服务 URL</strong></td>
<td>为 BizTalk 服务 URL。 这是相同的如创建 BizTalk 服务时输入<strong>的域 URL</strong> 。</td>
</tr>
<tr>
<td><strong>公用虚拟 IP (VIP) 地址</strong></td>
<td>分配到 BizTalk 服务的 IP 地址。 它用于输入的所有终结点并为出站通信的源地址。 此 IP 地址属于 BizTalk 服务，只要它创建。 如果您删除此 BizTalk 服务的 IP 地址分配给另一个 BizTalk 服务。</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>通过 BizTalk 服务的身份验证。</td>
</tr>
<tr>
<td><strong>版</strong></td>
<td>列表创建 BizTalk 服务时输入版本。</td>
</tr>
<tr>
<td><strong>位置</strong></td>
<td>显示 BizTalk 服务的宿主的地理区域。</td>
</tr>
<tr>
<td><strong>创建</strong></td>
<td>显示的日期和时间创建的 BizTalk 服务。</td>
</tr>
<tr>
<td><strong>跟踪数据库</strong></td>
<td>存储跟踪表 BizTalk 服务使用 SQL Azure 数据库名称。 
<br/><br/>跟踪数据库
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">所述要求</a>提供详细信息。</td>
</tr>
<tr>
<td><strong>监视/存档存储</strong></td>
<td>存储 BizTalk 服务监视输出 Azure 存储帐户名。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">所述要求</a>提供存储帐户详细信息。</td>
</tr>
<tr>
<td><strong>订阅名</strong></td>
<td>列出了 BizTalk 服务的宿主的订阅。 订阅管理到 Azure 的传统门户网站的访问。</td>
</tr>
<tr>
<td><strong>订购 ID</strong></td>
<td>当创建订阅时，会自动生成订阅 ID。 当使用 REST Api，您可能需要输入预订 id。</td>
</tr>
</table>

[BizTalk 服务︰ 资源调配使用 Azure 传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=302280)列出的步骤创建的 BizTalk 服务。


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>管理同步密钥的连接信息和任务条形图中删除︰

<table border="1">

<tr>
<td><strong>管理</strong>应用程序部署</td>
<td>打开 Azure BizTalk 服务门户。 BizTalk 服务门户是 EDI 配置，包括添加合作伙伴和创建 X12，AS2，入口和 EDIFACT 协议。
<br/><br/>
这是<strong>创建合作伙伴协议</strong>相同的<strong>快速启动</strong>选项卡上。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI 消息 BizTalk 服务门户上配置组件</a>BizTalk 服务门户上提供了详细信息。</td>
</tr>
<tr>
<td><strong>连接信息</strong>的访问控制 Namespace</td>
<td>显示访问控制 Namespace、 默认的颁发者和默认参数值;可以复制的。
<br/><br/>
您还可以打开访问控制门户。 此访问控制门户网站是使用左侧的导航窗格中的活动目录选项相同。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">管理 ACS Namespace</a>提供了有关访问控制门户的更多信息。</td>
</tr>
<tr>
<td><strong>同步参数</strong>中存储帐户</td>
<td>创建存储帐户时，自动创建主键和辅助键。 这些加密密钥控制对您的存储帐户的访问。 BizTalk 服务会自动使用主键。 <strong>同步参数</strong>使用户为主键和辅助键之间切换而不会中断 BizTalk 服务。
<br/><br/>
例如，您希望对存储帐户使用一个新的主键 BizTalk 服务。 若要此操作︰
<br/><br/>
<ol>
<li>选择您的 BizTalk 服务并选择<strong>同步密钥</strong>。 选择辅助键。 执行此操作时，使用辅助键启动 BizTalk 服务。</li>
<li>在 Azure 经典门户中，选择您的存储帐户，重新生成主键。 请记住，BizTalk 服务使用辅助键。</li>
<li>选择您的 BizTalk 服务并选择<strong>同步密钥</strong>。 现在，选择主键。 这是您重新生成新的主键。</li>
<li>在 Azure 经典门户中，选择您的存储帐户和第二密钥重新生成。</li>
</ol>
<br/>
此过程称为"翻转键"。 目的是使用户能够为主键和辅助键之间切换而不会中断 BizTalk 服务。</td>
</tr>

<tr>
<td><strong>删除</strong>应用程序</td>
<td>将删除 BizTalk 服务和部署到它的所有项目。</td>
</tr>
</table>


## <a name="monitor"></a>监视器
不适用于免费版。

选择您的 BizTalk 服务名称时，监视器选项卡可用，并且显示以下︰

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>跃点数图︰ 显示选定的性能指标
这些指标提供有关 BizTalk 服务的运行状况的实时值。 您选择的显示的性能指标。 六个性能指标最多可以同时显示。 

您还可以选择的**相对**或**绝对**的值和**间隔**测量数据所显示的时间范围。 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>若要删除或在图表中显示度量值︰
1. 选择**监视**选项卡。
2. 在任务栏中选择**添加度量值**︰  
![选择添加度量值][AddMetrics]
3. 检查您想要显示的性能指标。
4. 选中的复选标记，以返回到**监视器**选项卡。
5. 选择的指标来显示关系图中的该度量值旁边的圆圈。  

    例如， **CPU 使用**跃点数为灰色;图中未显示其输出︰  
![CPU 使用率指标将显示为灰色][GrayedMetric]  

    若要启用**CPU 使用率**规格，若要在图表中显示其输出的圆出选择灰色︰  
![CPU 使用率指标已启用][EnabledMetric]

6. 要从显示图表和列表中删除一个指标，选择任务栏中**删除指标**。 要添加到列表中的度量标准后，选择在任务条形图中**添加度量值**，检查指标，选择复选标记以返回到**监视器**选项卡。 选择灰出圆启用度量。

## <a name="Metrics"></a>可用的度量值
下列性能计数器/指标有︰

<table border="1">

<tr>
<td><strong>RountdTrip 延迟</strong></td>
<td>显示所有桥跨的平均时间以毫秒 (ms) 为单位来处理邮件的时间直到消息完全处理通过 BizTalk 服务接收消息。 成功处理的消息进行计数。<br/><br/>
下列事件发生时，会创建一个时间戳︰
<ul>
<li>邮件所进入网关</li>
<li>消息路由到目标</li>
<li>目标响应</li>
<li>目标确认响应发送到网关</li>
</ul>
<br/>
此度量显示以下计算得出的︰
<br/><br/>
[目标确认响应发送到网关]-[消息进入网关]</td>
</tr>
<tr>
<td><strong>在源失败</strong></td>
<td>显示失败的邮件的总数由 BizTalk 服务时拉来自源终结点。</td>
</tr>
<tr>
<td><strong>CPU 使用率</strong></td>
<td>列出所有角色实例的平均 %处理器时间。</td>
</tr>
<tr>
<td><strong>处理滞后时间</strong></td>
<td>在所有的桥接器，不包括在目标中所花费的时间显示的平均占用时间以毫秒 (ms) 为单位来处理邮件 BizTalk 服务。 成功处理的消息进行计数。<br/><br/>
每个下列事件发生时，会创建一个时间戳︰

<ul>
<li>邮件所进入网关</li>
<li>消息路由到目标</li>
<li>目标响应</li>
<li>目标确认响应发送到网关</li>
</ul>
<br/>此度量显示以下计算得出的︰<br/><br/>
[目标确认响应发送到网关]-[消息进入网关]-[目标响应] + [封邮件被路由到目标]</td>
</tr>
<tr>
<td><strong>在过程中的故障</strong></td>
<td>显示失败的邮件通过 BizTalk 服务处理期间所有桥跨一定时间间隔内的总数。</td>
</tr>
<tr>
<td><strong>发送消息</strong></td>
<td>显示所有桥跨一定时间间隔内通过 BizTalk 服务发送的邮件的总数。 当管道发送的消息到达路由目标时，该指标就会增加。 此统计数据并不表示一条消息被成功处理。<br/><br/>
请求-回复的情况下，在规格，就会增加，当路由目标发送回执确认回管道。</td>
</tr>
<tr>
<td><strong>接收到的邮件</strong></td>
<td>显示所有桥跨一定时间间隔内接收的 BizTalk 服务的邮件总数。 由管道接收到一封新邮件时，该指标就会增加。</td>
</tr>
<tr>
<td><strong>过程中的消息</strong></td>
<td>显示当前正在处理的 BizTalk 服务在时间间隔内的邮件的总数。</td>
</tr>
<tr>
<td><strong>已处理的邮件</strong></td>
<td>显示成功地处理了 BizTalk 服务所有的桥跨在时间间隔内的邮件的总数。 当管道成功接收消息并将其成功路由到目标时，该指标就会增加。</td>
</tr>
</table>


## <a name="scale"></a>小数位数
在缩放比例选项卡，可以添加或减去的 BizTalk 服务所使用的单位数量。 默认情况下，没有一个单位配置。 可以添加其他单位来调整 BizTalk 服务。 如果增加比例，不断增加的吞吐量。 资源量还会增加，包括部署的桥、 协议、 LOB 连接和处理能力。 例如，则增加到两个单位的比例从 1 个单位。 在此情况下，可以部署双倍的桥接器、 双协议、 双 LOB 的连接，和两倍的处理能力。

某些 BizTalk 版本不提供缩放选项。 在此情况下，一个单位被允许。 若要确定多少单位可以扩展您的版本，请参考[BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)。

增加单位的数目，可能会影响价格。 如果您增加单位，选择**保存**显示一条消息，告诉您帐单会受到影响。 然后选择继续。 当您增加单位，BizTalk 服务状态从活动更改更新数。 在更新状态时，BizTalk 服务继续运行。

[BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)定义的"单位"。


## <a name="configure"></a>配置
不适用于混合连接。

将备份状态设置为无或自动。 如果设置为无时，会自动不创建任何备份。 如果设置为自动，将配置的频率和时间的备份，以保存备份文件的备份位置。 

[BizTalk 服务︰ 备份和还原](biztalk-backup-restore.md)提供了详细信息。 


## <a name="HybridConnections"></a>混合连接
混合连接将 Azure 应用程序，如 Web 应用程序或移动在 Azure 应用程序服务中，连接到内部资源，它使用一个静态的 TCP 端口，如 SQL Server、 MySQL，HTTP Web Api 和大多数自定义的 Web 服务。 在 BizTalk 服务在 Azure 的传统门户网站中管理混合连接。

在 Azure 应用程序服务创建混合连接，请参阅[访问内部资源使用 Azure 应用程序服务中的混合连接](../app-service-web/web-sites-hybrid-connection-get-started.md)。

若要创建或管理 Azure BizTalk 服务中混合连接，请参阅[混合连接](integration-hybrid-connection-overview.md)。



## <a name="next"></a>下一步
既然您已熟悉不同的选项卡，您可以了解有关的 Azure BizTalk 服务功能︰

- [BizTalk 服务︰ 限制](biztalk-throttling-thresholds.md)  
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](biztalk-issuer-name-issuer-key.md)  
- [BizTalk 服务︰ 备份和恢复](biztalk-backup-restore.md)

## <a name="see-also"></a>请参见
- [混合连接](integration-hybrid-connection-overview.md)  
- [BizTalk 服务︰ 开发人员、 基本、 标准和高级版图表](biztalk-editions-feature-chart.md)  
- [BizTalk 服务︰ 资源调配使用 Azure 经典门户](biztalk-provision-services.md)  
- [BizTalk 服务︰ BizTalk 服务状态图表](biztalk-service-state-chart.md)  
- [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
