<properties
    pageTitle="在 Azure 的门户网站中创建 Azure BizTalk 服务 |Microsoft Azure"
    description="了解如何设置或创建在 Azure 门户; Azure BizTalk 服务MABS WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>创建使用 Azure 门户的 BizTalk 服务

在 Azure 门户创建 Azure BizTalk 服务。

> [AZURE.TIP] 若要登录到 Azure 的门户，您需要 Azure 帐户和 Azure 的订阅。 如果您没有帐户，您可以在几分钟内创建免费的试用帐户。 [Azure 的免费试用版](http://go.microsoft.com/fwlink/p/?LinkID=239738)，请参阅。

## <a name="create-a-biztalk-service"></a>创建的 BizTalk 服务
根据您选择的版本，并不是所有的 BizTalk 服务设置可能可用。

1. 登录到[Azure 的门户](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在底部导航窗格中，选择**新建**:  
![选择新建按钮][NEWButton]

3. 选择**应用程序的服务** > **BIZTALK 服务** > **自定义创建**︰  
![选择 BizTalk 服务，然后选择创建自定义][NewBizTalkService]

4. 输入的 BizTalk 服务设置︰

    <table border="1">
    <tr>
    <td><strong>BizTalk 服务名称</strong></td>
    <td>您可以输入任何名称，但要具体。 一些示例包括︰<br/><br/>
    <em>mycompany</em>。 biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>。 biztalk.windows.net<br/>
    <em>myapplication</em>。 biztalk.windows.net<br/><br/>"。 biztalk.windows.net"会自动添加到您输入的名称。 这将创建一个 URL，用于访问 BizTalk 服务，像<strong>https://<em>myapplication</em>。 biztalk.windows.net</strong>。
    </td>
    </tr>
    <tr>
    <td><strong>版</strong></td>
    <td>如果您在测试/开发阶段，选择<strong>开发人员</strong>。 如果您是在生产阶段，使用<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk 服务︰ 版图表</a>来确定<strong>最优</strong>、<strong>标准</strong>或<strong>基本</strong>是您的业务方案的正确选择。
    </td>
    </tr>
    <tr>
    <td><strong>地区</strong></td>
    <td>选择要承载 BizTalk 服务的地理区域。</td>
    </tr>
    <tr>
    <td><strong>域的 URL</strong></td>
    <td><strong>可选</strong>。 默认情况下，域 URL 是<em>YourBizTalkServiceName</em>。 biztalk.windows.net。 也可以输入自定义的域。 例如，如果您的域<em>contoso</em>，您可以输入︰ <br/><br/>
    <em>MyCompany</em>。 contoso.com<br/>
    <em>MyCompanyMyApplication</em>。 contoso.com<br/>
    <em>MyApplication</em>。 contoso.com<br/>
    <em>YourBizTalkServiceName</em>。 contoso.com<br/>
    </td>
    </tr>
    </table>
选择下一个箭头。

5. 输入存储和数据库设置︰

    <table border="1">
    <tr>
    <td><strong>监视/存档存储帐户</strong></td>
    <td>选择现有的存储帐户或创建新的存储帐户。 <br/><br/>如果您创建新的存储帐户，输入<strong>存储帐户名称</strong>。</td>
    </tr>
    <tr>
    <td><strong>跟踪数据库</strong></td>
    <td>如果您使用现有的 Azure SQL 数据库，另一个 BizTalk 服务不能使用它。 您需要的登录名和密码输入时是 Azure SQL 数据库服务器。<br/><br/><strong>提示</strong>在同一区域为 BizTalk 服务创建的跟踪数据库和监视/存档存储帐户。</td>
    </tr>
    </table>
选择下一个箭头。

6. 输入数据库设置:

    <table border="1">
    <tr>
    <td><strong>名称</strong></td>
    <td>在上一个屏幕中选择<strong>创建新的 SQL 数据库实例</strong>时可用。
    <br/><br/>
   输入通过 BizTalk 服务要使用的 SQL 数据库名称。</td>
    </tr>
    <tr>
    <td><strong>服务器</strong></td>
    <td>在上一个屏幕中选择<strong>创建新的 SQL 数据库实例</strong>时可用。
    <br/><br/>
   选择现有的 SQL 数据库服务器或创建新的 SQL 数据库服务器。</td>
    </tr>
    <tr>
    <td><strong>服务器登录名</strong></td>
    <td>输入登录的用户名。</td>
    </tr>
    <tr>
    <td><strong>服务器登录密码</strong></td>
    <td>输入的登录密码。</td>
    </tr>
    <tr>
    <td><strong>地区</strong></td>
    <td>在<strong>创建新的 SQL 数据库实例</strong>处于选中状态时可用。 选择您的 SQL 数据库的宿主的地理区域。</td>
    </tr>
    </table>

选中复选标记以完成该向导。 会出现进度图标︰  
![当完成时，将显示进度图标][ProgressComplete]

完成后，Azure BizTalk 服务是创建并准备好您的应用程序。 默认设置就足够了。 如果您想要更改默认设置，在左侧的导航窗格中，选择**BIZTALK 服务**，然后选择 BizTalk 服务。 其他设置将显示在[仪表板、 显示器和缩放比例选项卡](biztalk-dashboard-monitor-scale-tabs.md)的顶部。

根据 BizTalk 服务的状态，有不能完成某些操作。 有关这些操作的列表，请转到[BizTalk 服务状态图表](biztalk-service-state-chart.md)。


## <a name="post-provisioning-steps"></a>调配后步骤

-  [在本地计算机上安装证书](#InstallCert)
-  [添加一个生产就绪证书](#AddCert)
-  [获取访问控制命名空间](#ACS)

#### <a name="InstallCert"></a>在本地计算机上安装证书
资源调配的 BizTalk 服务的一部分，作为一个自签名的证书创建并与 BizTalk 服务订阅相关。 必须下载该证书并将它安装在您部署 BizTalk 服务应用程序或将消息发送到 BizTalk 服务终结点的计算机上。

1. 登录到[Azure 的门户](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BIZTALK 服务**，然后选择您的 BizTalk 服务订购。
3. 选择**仪表板**选项卡。
4. 选择**下载 SSL 证书**︰  
![修改 SSL 证书][QuickGlance]
5. 双击该证书，运行该向导以安装证书。 请确保安装在**受信任的根证书颁发机构**存储区的证书。

#### <a name="AddCert"></a>添加一个生产就绪证书
创建 BizTalk 服务用于在开发环境中只使用时将自动创建自签名的证书。 生产情况下，将其替换为一个生产就绪证书。

1. 在**仪表板**选项卡，选择**更新 SSL 证书**。
2. 浏览到包含您的 BizTalk 服务名称您私有 SSL 证书 (*CertificateName*.pfx)，输入密码，然后单击复选标记。

#### <a name="ACS"></a>获取访问控制命名空间

1. 登录到[Azure 的门户](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BIZTALK 服务**，然后选择您的 BizTalk 服务。
3. 在任务栏中，选择**连接信息**:  
![选择连接信息][ACSConnectInfo]

4. 复制访问控制值。

当部署 BizTalk 服务从 Visual Studio 项目时，您将输入此访问控制命名空间。 访问控制命名空间将自动创建的 BizTalk 服务。

对于任何应用程序，可以使用访问控制值。 Azure BizTalk 服务创建时，该访问控制命名空间控制与 BizTalk 服务部署身份验证。 如果您想要更改的订阅或管理命名空间，在左侧的导航窗格中选择**活动目录**，然后选择您的命名空间。 任务栏列出了您的选项。

单击**管理**打开访问控制管理门户。 在访问控制管理门户中，BizTalk 服务使用**服务标识**︰  
![在 Access 中的 ACS 服务标识控制管理门户][ACSServiceIdentities]

访问控制服务标识是凭据的一套允许应用程序或客户端直接与访问控制验证并收到一个令牌。

> [AZURE.IMPORTANT]BizTalk 用于服务**所有者**默认服务标识和**密码**值。 如果您使用对称密钥值而不是密码值，则可能会出现以下错误。<br/><br/>*无法连接到指定的凭据的访问控制管理服务帐户*

[管理 ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx)列出一些指导原则和建议。

## <a name="requirements-explained"></a>所述的要求

这些要求不适用于免费版。
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>您的需要</strong></td>
        <td><strong>为什么需要它</strong></td>
</tr>
<tr>
<td>Azure 的订阅</td>
<td>该订阅确定用户可以登录到 Azure 的门户。 帐户持有者在<a HREF="https://account.windowsazure.com/Subscriptions">Azure 订阅</a>创建订阅。
<br/><br/>
Azure 帐户可以有多个订阅，可以通过所允许的任何人进行管理。 例如，您的 Azure 帐户持有者会创建名为<em>BizTalkServiceSubscription</em>的预订并举 BizTalk 管理员在公司内 (例如，ContosoBTSAdmins@live.com)对此订阅的访问。 在此方案中，BizTalk 管理员登录到 Azure 的门户网站，订阅，包括 Azure BizTalk 服务中具有完全的管理员权限，向所有托管服务。 BizTalk 管理员不是 Azure 帐户持有人，因此不能访问任何帐单信息。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">管理订阅和 Azure 门户中存储帐户</a>提供了详细信息。
</td>
</tr>
<tr>
<td>SQL azure 数据库</td>
<td>表、 视图和存储的过程使用 BizTalk 服务，包括跟踪数据的存储。
<br/><br/>
创建 BizTalk 服务时，您可以使用现有 Azure SQL Server，Azure SQL 数据库，或自动创建新的服务器或数据库。
<br/><br/>
自动配置 SQL 数据库规模。 通常情况下，默认比例足以为 BizTalk 服务。 更改比例会影响价格。 请参阅<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">帐户和帐单在 SQL Azure 数据库</a>
<br/><br/>
<strong>备注</strong>
<br/>
<ul>
<li> 在创建一个新的 Azure SQL Server 和数据库时，将自动启用 Azure 服务。 BizTalk 服务需要 Azure 服务启用。</li>
<li>如果现有的 Azure SQL Server 上创建一个新的 Azure SQL 数据库，则不会更改服务器的防火墙规则。 因此，有可能其他 Azure 服务不允许访问该服务器的数据库。</li>
</ul>
</td>
</tr>
<tr>
<td>Azure 访问控制命名空间</td>
<td>使用 Azure BizTalk 服务进行身份验证。 当部署 BizTalk 服务从 Visual Studio 项目时，您将输入此访问控制命名空间。 创建 BizTalk 服务时，将自动创建的访问控制命名空间。</td>
</tr>

<tr>
<td>Azure 存储帐户</td>
<td>提供访问表、 blob 和队列，BizTalk 服务用于将以下内容保存为︰

<ul>
<li>监视该 BizTalk 服务的日志文件。 在 Azure 门户中的**监视**选项卡还显示监视输出。</li>
<li>创建 X12 或 AS2 合作伙伴之间的协议时，您可以启用存档功能，可用于存储消息属性。 此数据将保存在存储帐户。</li>
</ul>
<br/>
创建 BizTalk 服务时，您可以使用现有的存储帐户或自动创建新的存储帐户。
<br/><br/>
默认存储设置能够满足 BizTalk 服务。
<br/><br/>
创建存储帐户时，自动创建主键和辅助键。 这些键来控制对您的存储帐户的访问。 BizTalk 服务会自动使用主键。
<br/><br/>
有关详细信息，请参阅<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">存储</a>。
</td>
</tr>

<tr>
<td>专用的 SSL 证书</td>
<td>
当创建 Azure BizTalk 服务时，也会创建 HTTPS URL 包含 BizTalk 服务名称。 此 URL 自动配置为使用自签名仅开发证书。 对于生产环境，需要专用的 SSL 证书。
<br/><br/>
<strong>重要的 SSL 证书信息</strong>

<ul>
<li>证书到期日期必须小于 5 年。</li>
<li>所有的私有证书时需要密码。 知道此密码，并作为最佳做法，与您的管理员共享密码。</li>
<li>在测试/开发环境中使用自签名的证书。 当使用自签名的证书，将证书导入到您的个人证书存储区和受信任的根证书颁发机构证书存储区。</li>
</ul>
<br/>当生产证书请求发送到证书颁发机构时，给出以下证书属性︰
<br/>

<ul>
<li><strong>增强型密钥用法</strong>︰ 至少，Azure BizTalk 服务要求服务器进行身份验证。</li>
<li><strong>公用名称</strong>︰ 输入 Azure BizTalk 服务 URL 的完全限定的域名 (FQDN)。 在这篇文章，请参阅<a HREF="#BizTalk">创建 BizTalk 服务</a>。</li>
</ul>
<br/>
在 BizTalk 服务创建后，可以添加新的或不同的证书。
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>混合连接

当您创建一个 Azure BizTalk 服务时，**混合连接**选项卡不可用︰

![混合的连接选项卡][HybridConnectionTab]

使用混合连接 Azure 网站或移动的 Azure 服务连接到任何内部资源，它使用一个静态的 TCP 端口，如 SQL Server、 MySQL，HTTP Web Api、 移动服务和大多数自定义的 Web 服务。  混合连接和 BizTalk 适配器服务是不同的。 BizTalk 适配器服务用于将 Azure BizTalk 服务连接到内部的业务线 (LOB) 系统。

 请参阅[混合连接](integration-hybrid-connection-overview.md)若要了解详细信息，包括创建和管理混合连接。


## <a name="next-steps"></a>下一步行动

既然已创建的 BizTalk 服务，您可以熟悉不同[BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](biztalk-dashboard-monitor-scale-tabs.md)。 BizTalk 服务可供您的应用程序。 若要开始创建应用程序，请转到[Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=235197)。

## <a name="see-also"></a>请参见
- [BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)<br/>
- [BizTalk 服务︰ 状态图表](biztalk-service-state-chart.md)<br/>
- [BizTalk 服务︰ 备份和恢复](biztalk-backup-restore.md)<br/>
- [BizTalk 服务︰ 限制](biztalk-throttling-thresholds.md)<br/>
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](biztalk-issuer-name-issuer-key.md)<br/>
- [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [混合连接](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
