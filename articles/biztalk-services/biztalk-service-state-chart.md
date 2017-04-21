<properties 
    pageTitle="允许在不同的状态或状态在 BizTalk 服务任务 |Microsoft Azure" 
    description="在不同的 MABS 状态允许执行的操作/操作︰ 停止、 启动、 重新启动、 暂停、 继续、 删除、 扩展、 更新配置，并且支持了" 
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
    ms.date="08/15/2016" 
    ms.author="mandia"/>



# <a name="biztalk-services-service-state-chart"></a>BizTalk 服务︰ 服务状态图表
根据 BizTalk 服务的当前状态，有可以或无法在 BizTalk 服务上执行的操作。

例如，您配置新的 BizTalk 服务在 Azure 的传统门户。 它成功完成时，BizTalk 服务处于活动状态。 处于活动状态时，您可以停止该 BizTalk 服务。 如果成功停止，BizTalk 服务进入停止状态。 如果停止失败，BizTalk 服务转到 StopFailed 的状态。 在 StopFailed 状态下，您可以重新启动 BizTalk 服务。 如果您尝试不允许的操作，如简历 BizTalk 服务中，出现下列错误︰

**不允许操作**

要配置 BizTalk 服务，请转到[BizTalk 服务︰ 资源调配使用 Azure 传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=302280)。

下表列出了可以在 BizTalk 服务处于特定状态时完成的操作。 ✔ 意味着处于该状态时允许进行此操作。 空白项表示处于该状态时，无法执行操作。

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>启动、 停止、 重新启动、 暂停，恢复和删除操作
<table border="1">
<tr>
        <th colspan="15">操作或操作</th>
</tr>

<tr>
        <th rowspan="18">BizTalk 服务状态</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>启动</th>
        <th>停止</th>
        <th>重新启动</th>
        <th>挂起</th>
        <th>简历</th>
        <th>删除</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>活动</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>已禁用</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>挂起</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>停止</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>服务更新失败</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>规模、 更新配置和备份操作
<table border="1">
<tr>
        <th colspan="15">操作或操作</th>
</tr>

<tr>
        <th rowspan="18">BizTalk 服务状态</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>小数位数</th>
        <th>更新配置</th>
        <th>备份</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>活动</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>已禁用</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>挂起</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>停止</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>服务更新失败</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>请参见
- [BizTalk 服务︰ 资源调配使用 Azure 经典门户](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk 服务︰ 开发人员、 基本、 标准和高级版图表](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk 服务︰ 备份和恢复](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk 服务︰ 限制](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
