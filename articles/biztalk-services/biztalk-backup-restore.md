<properties 
    pageTitle="创建和还原备份在 BizTalk 服务 |Microsoft Azure" 
    description="BizTalk 服务包括备份和恢复。 了解如何创建和还原备份并确定获取备份。 MABS WABS" 
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


# <a name="biztalk-services-backup-and-restore"></a>BizTalk 服务︰ 备份和恢复

Azure BizTalk 服务包括备份和还原功能。 本主题介绍如何备份和还原使用 Azure 的传统门户的 BizTalk 服务。

您还可以备份使用[BizTalk 服务 REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584)的 BizTalk 服务。 

> [AZURE.NOTE] 混合连接无法进行备份，而不考虑该版本。 您必须重新创建混合连接。

## <a name="before-you-begin"></a>开始之前

- 备份和恢复不可能适用于所有版本。 请参阅[BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)。

- 使用 Azure 的传统门户网站，您可以创建按需备份或创建定时的备份。 

- 到同一 BizTalk 服务或新的 BizTalk 服务可以恢复备份的内容。 若要还原 BizTalk 服务使用相同的名称，必须删除现有的 BizTalk 服务和名称必须可用。 删除 BizTalk 服务后，可以比相同的名称，可在需要时间长。 如果您迫不及待能够使用相同的名称，然后还原到新的 BizTalk 服务。

- 可以将 BizTalk 服务还原到相同版本或更高版本。 还原到较低版本的 BizTalk 服务，从制作备份时，不支持。

    例如，使用基本版本的备份可以恢复到高级版。 使用高级版备份无法还原到标准版。

- EDI 控制号备份来维护控制号的连续性。 如果在上次备份之后处理邮件，还原此备份的内容可能会导致重复控制号。

- 一批有活动的消息，如果处理批处理**之前**运行备份。 创建备份时 （按需或计划），分批的邮件永远不会存储。 

    **如果备份在批处理中的活动消息，这些消息不进行备份，因此丢失。**

- 可选︰ 在 BizTalk 服务门户中中, 停止任何管理操作。


## <a name="create-a-backup"></a>创建备份

备份可以随时执行，并且完全由您控制。 本部分列出了使用 Azure 的传统门户网站，创建备份的步骤包括︰

[按需备份](#backupnow)

[计划备份](#backupschedule)

#### <a name="backupnow"></a>按需备份
1. 在 Azure 的传统门户网站，选择**BizTalk 服务**，然后选择 BizTalk 服务所需的备份。
2. 在**仪表板**选项卡中选择**备份**页面底部。
3. 输入备份的名称。 例如，输入*myBizTalkService*备份*日期*。
4. 选择一个 blob 存储帐户并选择要启动备份复选标记。

完成备份后，备份您输入的名称与某个容器中存储帐户创建。 此容器包含 BizTalk 服务备份配置。

#### <a name="backupschedule"></a>计划备份

1. 在 Azure 的传统门户网站，选择**BizTalk 服务**、 选择要计划的备份的 BizTalk 服务名称，然后选择**配置**选项卡。
2. 将**备份状态**设置为**自动**。 
3. 选择要将备份存储、 输入**频率**来创建备份，**存储帐户**和长短保留备份 （**保留天数**）︰

    ![][AutomaticBU]

    **备注**   
    - 在**保留天数**，保留期间必须大于备份的频率。
    - 选择**始终保持至少一个备份**，即使它已经超过保留期。
    

4. 选择**保存**。


当计划的备份作业运行时，它在您输入的存储帐户创建 （存储的备份数据） 的容器。 容器的名称被命名为*BizTalk 服务名称-日期-时间*。 

如果 BizTalk 服务仪表板将显示为**故障**状态︰

![上一次定时备份状态][BackupStatus] 

该链接将打开管理服务操作日志以帮助解决。 请参阅[BizTalk 服务︰ 使用操作日志疑难解答](http://go.microsoft.com/fwlink/p/?LinkId=391211)。

## <a name="restore"></a>还原

从 Azure 的传统门户网站或[还原 BizTalk 服务 REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582)，您可以还原备份。 这一节列出的步骤来恢复使用传统的门户。

#### <a name="before-restoring-a-backup"></a>在还原备份之前

- 可以还原 BizTalk 服务时输入新的跟踪、 归档和监控存储。

- 相同的 EDI 运行时数据还原。 EDI 运行备份存储的控制编号。 还原的控件数是从备份的时间顺序。 如果在上次备份之后处理邮件，还原此备份的内容可能会导致重复控制号。

#### <a name="restore-a-backup"></a>还原备份

1. 在 Azure 经典门户中，选择**新建** > **应用程序服务** > **BizTalk 服务** > **还原**︰

    ![还原备份][Restore]

2. 在**备份 URL**，选择文件夹图标，然后展开将 BizTalk 服务配置备份存储的 Azure 存储帐户。 展开的容器，并在右窗格中，选择相应.txt 文件备份。 
<br/><br/>
选择**打开**。

3. 在**还原 BizTalk 服务**页上，输入一个**BizTalk 服务名称**和用于恢复的 BizTalk 服务验证**域 URL**、**版**和**区域**。 跟踪数据库，则**创建新的 SQL 数据库实例**︰

    ![][RestoreBizTalkService]

    选择下一个箭头。

4.  验证 SQL 数据库的名称，请输入该服务器的物理位置创建 SQL 数据库的服务器和用户名/密码。


    如果您想要配置的 SQL 数据库版本，大小和其他属性，选择**配置高级数据库设置**。 

    选择下一个箭头。

5. 创建新的存储帐户或现有的存储帐户输入 BizTalk 服务。

7. 选择开始还原的复选标记。

还原成功完成后，新的 BizTalk 服务已列入在 Azure 的传统门户的 BizTalk 服务页上挂起状态。



### <a name="postrestore"></a>还原备份后

BizTalk 服务总是处于**挂起**状态还原。 在此状态下，可以进行任何配置更改，新环境之前起作用，包括︰

- 如果您创建使用 Azure BizTalk 服务 SDK 的 BizTalk 服务应用程序，您可能需要更新这些应用程序要使用的还原环境中的访问控制 (ACS) 凭据。

- 还原一个 BizTalk 服务来复制现有的 BizTalk 服务环境。 在此情况下，如果有协议中的原始的 BizTalk 服务门户配置使用源 FTP 文件夹，您可能需要更新协议新还原环境中使用不同的源 FTP 文件夹。 否则，可能有两种不同协议试图引入相同的消息。

- 如果还原可以有多个 BizTalk 服务环境，确保您指定 Visual Studio 应用程序、 PowerShell 的 cmdlet，REST Api 或贸易合作伙伴管理 OM Api 中正确的环境的目标。

- 最好在新还原的 BizTalk 服务环境上配置自动的备份。

在 Azure 经典门户启动 BizTalk 服务，选择恢复的 BizTalk 服务并在任务条形图中选择**恢复**。 



## <a name="what-gets-backed-up"></a>获取备份

创建备份时，将备份以下各项︰

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>备份项目</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk 服务门户</strong></td>
</tr> 
<tr>
<td>配置和运行时</td> 
<td>
<ul>
<li>合作伙伴和配置文件的详细信息</li>
<li>伙伴协议</li>
<li>部署自定义程序集</li>
<li>桥接器部署</li>
<li>证书</li>
<li>部署的转换</li>
<li>管道</li>
<li>创建和保存在 BizTalk 服务门户网站模板</li>
<li>X12 ST01 和 GS01 映射</li>
<li>控制号 (EDI)</li>
<li>AS2 消息 MIC 值</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk 服务</strong></td>
</tr> 
<tr>
<td>SSL 证书</td> 
<td>
<ul>
<li>SSL 证书数据</li>
<li>SSL 证书密码</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk 服务设置</td> 
<td>
<ul>
<li>刻度单位计数</li>
<li>版</li>
<li>产品版本</li>
<li>地区/数据中心</li>
<li>访问控制服务 (ACS) 命名空间和密钥</li>
<li>跟踪数据库连接字符串</li>
<li>归档存储帐户连接字符串</li>
<li>监视存储帐户连接字符串</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>其他项目</strong></td>
</tr> 
<tr>
<td>跟踪数据库</td> 
<td>当创建 BizTalk 服务时，跟踪数据库详细信息输入，包括 Azure SQL 数据库服务器和跟踪数据库名称。 跟踪数据库不会自动备份。
<br/><br/>
<strong>重要</strong><br/>
如果跟踪数据库中被删除，并且数据库需要恢复，则必须存在以前的备份。 如果备份不存在，跟踪数据库，其数据不可恢复。 在此情况下，相同的数据库名称创建新的跟踪数据库。 建议各地区复制。</td>
</tr> 
</table>

## <a name="next"></a>下一步

在 Azure 的传统门户网站中创建 Azure BizTalk 服务，请转到[BizTalk 服务︰ 资源调配使用 Azure 传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=302280)。 若要开始创建应用程序，请转到[Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=235197)。

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

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
