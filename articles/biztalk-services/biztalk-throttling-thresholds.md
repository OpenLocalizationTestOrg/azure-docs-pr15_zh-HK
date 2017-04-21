<properties 
    pageTitle="了解有关在 BizTalk 服务限制 |Microsoft Azure" 
    description="了解有关阻止阈值和 BizTalk 服务生成运行时行为。 限制基于内存使用和消息数。 MABS WABS" 
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





# <a name="biztalk-services-throttling"></a>BizTalk 服务︰ 限制

Azure BizTalk 服务实施服务限制基于两个条件︰ 内存使用情况，并同时处理的消息数。 本主题列出调整阈值，并限制条件发生时描述的运行时行为。

## <a name="throttling-thresholds"></a>阻止阈值

下表列出了限制源和阈值︰

||说明|低阈值|高阈值|
|---|---|---|---|
|内存|系统总内存可用/PageFileBytes %。 <p><p>总的可用 PageFileBytes 大约是系统的 2 倍 RAM。|60%|70%|
|消息处理|同时处理的消息数|40 * 的内核数|100 * 的内核数|

当达到高阈值时，Azure BizTalk 服务开始限制。 当达到低阈值限制停止。 例如，您的服务正在使用 65%系统内存。 在此情况下，该服务不会限制。 服务开始使用 70%的系统内存。 在此情况下，该服务将遏制，继续限制直到该服务使用 60%（低门槛） 系统内存。

Azure BizTalk 服务跟踪调节状态 （合并的状态与正常状态） 和限制的持续时间。


## <a name="runtime-behavior"></a>运行时行为

当 Azure BizTalk 服务进入阻止状态时，将发生以下情况︰

- 带宽限制是每个角色实例。 例如︰<br/>
RoleInstanceA 节流。 不阻止 RoleInstanceB。 在此情况下，RoleInstanceB 中的消息处理像预期的那样。 RoleInstanceA 中的消息将被丢弃并失败，出现以下错误︰<br/><br/>
**服务器正忙。请再试一次。**<br/><br/>
- 任何请求源不轮询或下载邮件。 例如︰<br/>
管线从外部 FTP 源中提取消息。 执行请求的角色实例进入阻止状态。 在此情况下，管道停止直到角色实例停止限制下载其他消息。
- 一个响应发送到客户端，以便客户端可以重新提交消息。
- 您必须等待，直到限制得到解决。 具体来说，您必须等待，直到达到低阈值。

## <a name="important-notes"></a>重要说明
- 不能禁用带宽限制功能。
- 不能修改阻止阈值。
- 带宽限制被实现整个系统。
- Azure SQL 数据库服务器还具有内置调节。

## <a name="additional-azure-biztalk-services-topics"></a>Azure BizTalk 服务的其他主题

-  [安装 SDK 的 Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [教程︰ Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>请参见
- [BizTalk 服务︰ 开发人员、 基本、 标准和高级版图表](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk 服务︰ 资源调配使用 Azure 经典门户](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk 服务︰ 资源调配的状态图表](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk 服务︰ 备份和恢复](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
