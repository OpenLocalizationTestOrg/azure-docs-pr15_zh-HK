<properties 
    pageTitle="疑难解答使用操作日志的 BizTalk 服务 |Microsoft Azure" 
    description="疑难解答使用操作日志的 BizTalk 服务。 MABS WABS" 
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


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>使用操作日志 BizTalk 服务︰ 疑难解答

## <a name="what-are-the-operation-logs"></a>操作日志有哪些？
操作日志将使您可以查看您的 Azure 服务，包括 BizTalk 服务上执行的操作的历史记录日志的 Azure 经典门户提供的管理服务功能。 这使您可以查看与您最早 180 天的 BizTalk 服务订购管理操作相关的历史数据。

> [AZURE.NOTE]此功能仅捕获在 BizTalk 服务上，例如在服务启动时的管理操作的日志备份最多，等等。 这种操作而不考虑他们执行的是从 Azure 的传统门户网站或通过使用[BizTalk 服务 REST Api](http://msdn.microsoft.com/library/azure/dn232347.aspx)跟踪。 有关使用管理服务跟踪的操作的完整列表，请参阅[操作跟踪使用 Azure 管理服务](#bizops)。<br/><br/>
这不会捕获到 BizTalk 服务 （如处理消息通过桥接器，等等。） 的运行时相关的活动的日志。 若要查看这些日志，请使用 BizTalk 服务门户跟踪视图。 有关详细信息，请参阅[跟踪消息](http://msdn.microsoft.com/library/azure/hh949805.aspx)。

## <a name="view-biztalk-services-operation-logs"></a>BizTalk 服务操作日志视图
1. 在 Azure 经典门户中，选择**管理服务**，然后选择**操作日志**选项卡。
2. 您可以筛选基于不同的参数，如订阅、 日期范围、 服务类型 （例如 BizTalk 服务）、 服务名称或状态的操作 （成功、 失败） 的日志。
3. 复选标记以查看筛选的列表中选择。 下图显示了与 testbiztalkservice 相关的活动︰ ![查看操作日志][ViewLogs] 
4. 若要查看有关特定操作的更多信息，选择行，并在底部的任务栏中单击**详细信息**。


## <a name="bizops"></a>操作跟踪使用 Azure 管理服务
下表列出了使用 Azure 管理服务跟踪的操作︰

操作名称 | 任务
--- | ---
CreateBizTalkService | 要创建新的 BizTalk 服务操作
DeleteBizTalkService | 要删除 BizTalk 服务操作
RestartBizTalkService | 要重新启动 BizTalk 服务操作
StartBizTalkService | 要启动 BizTalk 服务操作
StopBizTalkService | 要停止 BizTalk 服务操作
DisableBizTalkService | 要禁用 BizTalk 服务操作
EnableBizTalkService | 要启用 BizTalk 服务操作
BackupBizTalkService | 要备份一个 BizTalk 服务操作
RestoreBizTalkService | 要从指定的备份中还原 BizTalk 服务操作
SuspendBizTalkService | 操作挂起 BizTalk 服务
ResumeBizTalkService | 要恢复 BizTalk 服务操作
ScaleBizTalkService | 要向上或向下扩展 BizTalk 服务操作
ConfigUpdateBizTalkService | 要更新的配置 BizTalk 服务操作
ServiceUpdateBizTalkService | 要升级或降级到不同版本的 BizTalk 服务操作
PurgeBackupBizTalkService | 要清除 BizTalk 服务在固定时段以外的备份操作


## <a name="see-also"></a>请参见
- [备份 BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [从备份中还原 BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk 服务︰ 开发人员、 基本、 标准和高级版图表](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk 服务︰ 资源调配使用 Azure 经典门户](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk 服务︰ 资源调配的状态图表](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk 服务︰ 限制](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
