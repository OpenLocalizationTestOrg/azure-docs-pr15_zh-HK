<properties 
    pageTitle="创建和管理混合连接 |Microsoft Azure" 
    description="了解如何创建混合连接、 管理的连接，并安装混合连接管理器。 MABS WABS" 
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
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>创建和管理混合连接


## <a name="overview-of-the-steps"></a>步骤概述
1. 在专用网络中输入**主机名**或**FQDN**的内部资源来创建混合连接。
2. 将您的 Azure 的 web 应用程序或 Azure 的移动应用程序链接到混合连接。
3. 将混合连接管理器安装在您的内部资源，然后连接到特定的混合连接。 Azure 的门户提供了安装和连接的单击式体验。
4. 管理混合连接和连接键。

本主题列出了这些步骤。 

> [AZURE.IMPORTANT] 也可以设置混合连接终结点的 IP 地址。 如果您使用的 IP 地址，您可能会或可能未达到的内部资源，具体取决于您的客户端。 该混合连接取决于客户端执行 DNS 查找。 在大多数情况下，__客户端__是在应用程序代码。 如果客户端没有执行 DNS 查找，（它不会像一个域名 (x.x.x.x) 解析的 IP 地址），然后通过混合连接不发送通讯。
>
> 对于示例 （伪代码），您定义**10.4.5.6**作为内部主机︰
> 
> **以下情况适用︰**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **以下情况下不起作用︰**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>创建一个混合连接

可以在 Azure 使用 Web 应用程序**或**使用 BizTalk 服务门户中创建混合连接。 

**若要创建使用 Web 应用程序的混合连接**，请参阅[连接 Azure Web 应用程序到内部部署资源](../app-service-web/web-sites-hybrid-connection-get-started.md)。 您可以从您的 web 应用程序，这是首选的方法来安装混合连接管理器 (HCM)。 

**要创建的 BizTalk 服务混合连接**︰

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BizTalk 服务**，然后选择 BizTalk 服务。 

    如果您没有现有的 BizTalk 服务，则可以[创建一个 BizTalk 服务](biztalk-provision-services.md)。
3. 选择**混合连接**选项卡︰  
![混合的连接选项卡][HybridConnectionTab]

4. 选择**创建一个混合的连接**，或在任务栏中选择**添加**按钮。 输入以下命令︰

    属性 | 说明
--- | ---
名称 | 混合连接名称必须唯一，且不能为 BizTalk 服务相同的名称。 您可以输入任意名称，但要特定与它的用途。 示例包括︰<br/><br/>工资*SQLServer*<br/>SupplyList*SharepointServer*<br/>客户*OracleServer*
主机名 | 输入完全合格的主机的名称，只有主机名称、 或内部资源的 IPv4 地址。 示例包括︰<br/><br/>mySQLServer<br/>*mySQLServer*。*域*。*yourCompany*.com 公司<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*。*yourCompany*.com<br/>10.100.10.10<br/><br/>如果您使用的 IPv4 地址，请注意客户端或应用程序代码可能无法解析的 IP 地址。 请参阅本主题顶部注意要点。
端口 | 输入内部资源的端口号。 例如，如果您使用的 Web 应用程序，请输入端口 80 或端口 443。 如果您使用的 SQL Server，请输入端口 1433年。

5. 选中复选标记来完成安装。 

#### <a name="additional"></a>其他

- 可以创建多个混合连接。 请参阅[BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)允许的连接数。 
- 每个混合连接创建两个连接字符串︰ 应用程序键收听该发送和内部键。 每一对都有主要和辅助键。 


## <a name="LinkWebSite"></a>将您的 Azure 应用程序服务 Web 应用程序或移动应用程序的链接

若要将 Web 应用程序或在 Azure 应用程序服务的移动应用程序链接到现有的混合连接，在混合连接刀片式服务器中选择**使用现有的混合连接**。 请参阅[访问内部资源使用 Azure 应用程序服务中的混合连接](../app-service-web/web-sites-hybrid-connection-get-started.md)。

## <a name="InstallHCM"></a>安装内部部署混合连接管理器

创建一个混合连接后，安装混合连接管理器的内部资源。 从 Azure 的 web 应用程序或从 BizTalk 服务可以下载它。 BizTalk 服务的步骤︰ 

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BizTalk 服务**，然后选择 BizTalk 服务。 
3. 选择**混合连接**选项卡︰  
![混合的连接选项卡][HybridConnectionTab]
4. 在任务栏中，选择**本地安装**︰  
![内部部署安装程序][HCOnPremSetup]
5. 选择**安装和配置**运行或下载混合连接管理器在本地系统上。 
6. 选择对勾以开始安装。 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>其他
- 混合连接管理器可以安装在以下操作系统上︰

    - Windows Server 2008 R2 （.NET Framework 4.5 + 和 Windows 管理框架 4.0 + 所需）
    - Windows Server 2012 (Windows 管理框架 4.0 + 所需)
    - Windows Server 2012 R2


- 安装混合连接管理器之后，将发生以下情况︰ 

    - 在 Azure 上承载混合连接被自动配置为使用主应用程序的连接字符串。 
    - 内部部署资源自动配置为使用主要的内部部署的连接字符串。

- 混合连接管理器必须使用内部部署有效的连接字符串进行授权。 Azure Web 应用程序或移动应用程序必须使用有效的应用程序的连接字符串进行授权。
- 您可以通过在另一台服务器上安装另一个实例的混合连接管理器扩展混合连接。 配置内部侦听器使用相同的地址作为第一个内部侦听程序。 在此情况下，通信量的内部部署活动侦听器之间是随机分布 （循环）。 


## <a name="ManageHybridConnection"></a>管理混合连接
管理混合连接，您可以︰

- 使用 Azure 的门户网站，然后转到 BizTalk 服务。 
- 使用[REST Api](http://msdn.microsoft.com/library/azure/dn232347.aspx)。

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>复制/再生混合连接字符串

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BizTalk 服务**，然后选择 BizTalk 服务。 
3. 选择**混合连接**选项卡︰  
![混合的连接选项卡][HybridConnectionTab]
4. 选择混合连接。 在任务栏中，选择**管理连接**:  
![管理选项][HCManageConnection]

    **管理连接**列出的应用程序和内部连接字符串。 您可以将连接字符串复制或重新生成访问密钥在连接字符串中使用。 

    **如果您选择再生**、 共享连接字符串中使用访问键将更改。 请执行以下操作︰
    - 在 Azure 经典门户中，**同步键**选择 Azure 应用程序中。
    - 重新运行**内部部署的安装程序**。 重新运行内部部署安装程序时，自动配置内部资源以使用已更新的主连接字符串。


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>使用组策略来控制混合连接所使用的内部资源

1. 下载[混合连接管理器管理模板](http://www.microsoft.com/download/details.aspx?id=42963)。
2. 提取文件。
3. 修改组策略的计算机，请执行以下操作︰  

    - 复制。ADMX 文件复制到*%WINROOT%\PolicyDefinitions*文件夹。
    - 复制。ADML 文件复制到*%WINROOT%\PolicyDefinitions\en-us*文件夹。

一旦被复制，您可以使用组策略编辑器更改的策略。




## <a name="next"></a>下一步

[将 Azure 的 Web 应用程序连接到内部资源](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[从 Azure 的 Web 应用程序连接到 SQL Server 内部](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[混合连接概述](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>请参见

[REST API，用于管理在 Microsoft Azure 上的 BizTalk 服务](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk 服务︰ 版图表](biztalk-editions-feature-chart.md)  
[创建使用 Azure 的传统门户的 BizTalk 服务](biztalk-provision-services.md)  
[BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
