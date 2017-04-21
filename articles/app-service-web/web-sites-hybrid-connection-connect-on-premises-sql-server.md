<properties
    pageTitle="从 Azure 应用程序服务使用混合连接在一个 web 应用程序连接到 SQL Server 内部"
    description="在 Microsoft Azure 上创建一个 web 应用程序并将其连接到内部部署 SQL Server 数据库"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>从 Azure 应用程序服务使用混合连接在一个 web 应用程序连接到 SQL Server 内部

混合连接可以连接到使用一个静态的 TCP 端口的内部资源的[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序。 受支持的资源包括 Microsoft SQL Server，MySQL，HTTP Web Api，应用程序服务和大多数自定义的 Web 服务。

在本教程中，您将学习如何在[Azure 门户网站](http://go.microsoft.com/fwlink/?LinkId=529715)中创建的应用程序服务 web 应用程序、 连接到内部部署本地 SQL Server 数据库使用新的混合连接功能的 web 应用程序，创建一个简单的 ASP.NET 应用程序将使用混合连接，并将其部署到应用程序服务 web 应用程序的应用程序。 在 Azure 上已完成的 web 应用程序是内部成员资格数据库中存储用户凭据。 本教程假定使用 Azure 或 ASP.NET 没有经验。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。
>
>混合连接功能的 Web 应用程序部分是仅在[Azure 门户](https://portal.azure.com)中可用。 在 BizTalk 服务中创建的连接，请参阅[混合连接](http://go.microsoft.com/fwlink/p/?LinkID=397274)。  

## <a name="prerequisites"></a>系统必备组件 ##

若要完成本教程，您将需要以下产品。 所有有免费版本，以便您可以开始免费为完全的 Azure 开发。

- **Azure 订阅**-免费订阅，请参阅[Azure 免费试用版](/pricing/free-trial/)。

- **Visual Studio 2013年**-下载 Visual Studio 2013年的免费试用版，请参阅[Visual Studio 下载](http://www.visualstudio.com/downloads/download-visual-studio-vs)。 在继续操作之前安装这个软件。

- **Microsoft.NET Framework 3.5 Service Pack 1** -如果您的操作系统是 Windows 8.1、 Windows Server 2012 R2、 Windows 8，Windows Server 2012，Windows 7 或 Windows Server 2008 R2，您可以在控制面板 > 程序和功能 > 打开或关闭 Windows 功能。 否则，您可以从[Microsoft 下载中心](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22)下载它。

- **SQL Server 2014年使用表达工具**-Microsoft SQL Server Express 在[Microsoft Web 平台数据库页](http://www.microsoft.com/web/platform/database.aspx)的免费下载。 选择**表示**(不 LocalDB) 版本。 **表达与工具**版本包括 SQL Server 管理 Studio 中，您将使用在本教程中。

- **SQL Server 管理 Studio 速成**-这是，上面提到的工具下载包含 SQL Server 2014年表达，但如果您需要单独安装它，您可以从下载并安装它[SQL Server Express 下载页面](http://www.microsoft.com/web/platform/database.aspx)。

本教程假定您拥有 Azure 订阅，您已安装了 Visual Studio 2013，并且您已安装或启用.NET Framework 3.5。 本教程演示了如何安装 SQL Server 2014年明示适用于 Azure 混合连接功能 （静态 TCP 端口的默认实例） 的配置。 开始本教程之前从上面提到，如果没有 SQL Server 安装位置下载 SQL Server 2014年表达的工具。

### <a name="notes"></a>备注 ###
若要使用内部部署 SQL Server 数据库或 SQL Server Express 数据库混合连接，需要静态端口上启用 TCP/IP。 在 SQL Server 上的默认实例使用静态端口 1433，而命名的实例则不能。

在其安装内部混合连接管理器代理的计算机︰

- 必须拥有超过 Azure 的出站连接︰

端口|为什么
---|---
80|HTTP 端口进行证书验证和 （可选） 用于数据连接的**要求**。
443|**可选**的数据连接。 如果为 443 的出站连接不可用，则使用 TCP 端口 80。
5671 和 9352|**推荐**，但可选的数据连接。 请注意此模式通常会产生更高的吞吐量。 如果这些端口的出站连接不可用，则使用 TCP 端口 443。
- 必须能够到达*主机名称*︰*端口号*的内部资源。

这篇文章中的步骤假定您使用的浏览器从计算机将承载内部混合连接代理程序。

如果您已经有 SQL Server 安装在配置和符合上述条件的环境中，可以跳并着手[的创建一个 SQL Server 数据库部署](#CreateSQLDB)。

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>答︰ 安装 SQL Server Express，启用 TCP/IP，并创建一个 SQL Server 数据库内部 ##

本节说明了如何安装 SQL Server Express，启用 TCP/IP，并创建一个数据库，以便 web 应用程序将使用 Azure 门户。

### <a name="install-sql-server-express"></a>安装 SQL Server 速成版 ###

1. 要安装 SQL Server Express，请运行您下载的**SQLEXPRWT_x64_ENU.exe**或**SQLEXPR_x86_ENU.exe**文件。 SQL Server 安装中心向导出现。

    ![SQL Server 安装][SQLServerInstall]

2. 选择**新的 SQL Server 独立安装或将功能添加到现有安装**。 按照说明进行操作，接受默认的选项和设置，直到您到达的**实例配置**页面。

3. 在**实例配置**页上，选择**默认实例**。

    ![选择默认实例][ChooseDefaultInstance]

    默认情况下，SQL Server 默认实例侦听请求的 SQL Server 客户端静态端口 1433，即混合连接功能的要求。 命名的实例使用动态端口和 UDP，不支持的混合连接。

4. 接受**服务器配置**页上的默认设置。

5. 在**数据库引擎配置**页上，在**身份验证模式**，选择**混合模式 （SQL Server 身份验证，Windows 身份验证）**，并提供一个密码。

    ![选择混合的模式][ChooseMixedMode]

    在本教程中，您将使用 SQL Server 身份验证。 请务必记住您所提供的密码，因为您以后将需要。

6. 完成向导以完成安装的剩余步骤。

### <a name="enable-tcpip"></a>启用 TCP/IP ###
要使用 TCP/IP，请您将使用 SQL Server 配置管理器，它安装在安装 SQL Server Express 时。 要继续按照中[为 SQL Server 中启用 TCP/IP 网络协议](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx)的步骤。

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>创建一个 SQL Server 数据库内部 ###

Visual Studio 的 web 应用程序所需的 Azure 可以访问成员资格数据库。 这就要求 SQL Server 或 SQL Server Express 数据库 （不是 LocalDB 数据库，默认情况下使用 MVC 模板），因此接下来，您将创建成员资格数据库。

1. 在 SQL Server 管理 Studio 中，连接到您刚才安装 SQL Server。 （如果**连接到服务器**不自动显示对话框的左窗格中导航到**对象资源管理器**中，单击**连接**，然后单击**数据库引擎**。） ![连接到服务器][SSMSConnectToServer]

    对于**服务器类型**，选择**数据库引擎**。 对于**服务器名称**中，可以使用**localhost**或您正在使用的计算机的名称。 选择**SQL Server 身份验证**，然后再以 sa 用户名称和您在前面创建的密码登录。

2. 若要通过使用 SQL Server 管理 Studio 中创建一个新数据库，用鼠标右键单击在对象资源管理器中的**数据库**，然后单击**新建数据库**。

    ![创建新的数据库][SSMScreateNewDB]

3. 在**新建数据库**对话框中，为数据库名，输入 MembershipDB，然后单击**确定**。

    ![提供数据库名称][SSMSprovideDBname]

    请注意，您不做任何更改到数据库此时。 成员身份信息将自动以后添加 web 应用程序在运行时。

4. 在对象资源管理器中，展开**数据库**，如果您将看到已创建成员资格数据库。

    ![MembershipDB 创建][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B。 在 Azure 门户网站中创建 web 应用程序 ##

> [AZURE.NOTE] 如果您已经在 Azure 门户，您想要使用在本教程中创建 web 应用程序，您可以跳到[创建混合连接和 BizTalk 服务](#CreateHC)，从那里继续。

1. 在[Azure 门户](https://portal.azure.com)中，单击**新建** > **Web + 移动** > **Web 应用程序**。

    ![新建按钮][New]

2. 配置您的 web 应用程序，然后单击**创建**。

    ![网站名称][WebsiteCreationBlade]

3. 过一会，创建 web 应用程序和其 web 应用程序刀片出现。 刀片式服务器是一个垂直方向滚动的仪表板，使您能够管理您的 web 应用程序。

    ![运行的网站][WebSiteRunningBlade]

    若要验证 web 应用程序处于活动状态，可以单击**浏览**图标以显示的默认页。

接下来，您将创建一个混合连接和 BizTalk 服务 web 应用程序。

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C。 创建一个混合连接和 BizTalk 服务 ##

1. 返回在门户中，转到设置和单击**网络** > **配置混合连接终结点**。

    ![混合连接][CreateHCHCIcon]

2. 在混合连接刀片式服务器，单击**添加** > **新的混合连接**。

3. 在**创建混合连接**刀片式服务器︰
    - 对于**名称**提供连接的名称。
    - 对于**主机名**，请输入您的 SQL Server 主机计算机的计算机名。
    - 对于**端口**，请输入 1433 （SQL Server 的默认端口）。
    - 单击**BizTalk 服务** > **新的 BizTalk 服务**和 BizTalk 服务为输入一个名称。

    ![创建一个混合连接][TwinCreateHCBlades]

5. 单击**确定**两次。

    当此过程完成时，闪烁绿色**成功****通知**区域将和**混合连接**刀片式服务器将显示状态为**未连接**的新混合连接。

    ![创建一个混合连接][CreateHCOneConnectionCreated]

此时，您已完成云混合连接基础结构的重要组成部分。 接下来，您将创建相应的内部部分。

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. 安装内部混合连接管理器才能完成连接 ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

现在，混合连接基础结构已完成，您将创建的 web 应用程序使用它。

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E。 创建一个基本的 ASP.NET web 项目、 编辑的数据库连接字符串，和本地运行项目 ##

### <a name="create-a-basic-aspnet-project"></a>创建一个基本的 ASP.NET 项目 ###
1. 在 Visual Studio 中，在**文件**菜单上创建一个新项目︰

    ![新的 Visual Studio 项目][HCVSNewProject]

2. 在**新建项目**对话框中的**模板**部分中，选择**Web**并选择**ASP.NET Web 应用程序**，然后单击**确定**。

    ![ASP.NET Web 应用程序中选择][HCVSChooseASPNET]

3. 在**新的 ASP.NET 项目**对话框中，选择**MVC**，，然后单击**确定**。

    ![MVC 中选择][HCVSChooseMVC]

4. 创建项目后，将出现应用程序自述文件页。 不要运行 web 项目。

    ![自述文件页][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>编辑应用程序的数据库连接字符串 ###

在此步骤中，您可以编辑告知在何处查找本地 SQL Server Express 数据库的应用程序的连接字符串。 连接字符串是在应用程序的 Web.config 文件，其中包含应用程序的配置信息。

> [AZURE.NOTE] 若要确保应用程序使用的数据库，您在 SQL Server Express，并不是在 Visual Studio 默认 LocalDB，值得您运行您的项目之前完成此步骤。

1. 在解决方案资源管理器中双击 Web.config 文件。

    ![Web.config][HCVSChooseWebConfig]

2. 编辑**connectionStrings**部分以指向您的本地计算机，并按照下面的示例中的语法上的 SQL Server 数据库︰

    ![连接字符串][HCVSConnectionString]

    当撰写的连接字符串时，请记住以下原则︰

    - 如果您要连接到命名实例而不是默认实例 (例如，YourServer\SQLEXPRESS)，则必须配置您的 SQL Server 使用静态端口。 有关配置静态端口的信息，请参阅[如何配置 SQL Server 侦听特定的端口](http://support.microsoft.com/kb/823938)。 默认情况下，命名的实例使用 UDP 和不支持的混合连接的动态端口。

    - 建议您指定的端口 (默认情况下，如该示例中所示的 1433) 上的连接字符串，以便您可以确保您本地 SQL Server 已启用 TCP 和是否使用了正确端口。

    - 记住要使用 SQL Server 身份验证连接，在连接字符串中指定的用户 ID 和密码。

3. 单击**保存**保存 Web.config 文件的 Visual Studio 中。

### <a name="run-the-project-locally-and-register-a-new-user"></a>本地运行该项目，并注册一个新用户 ###

1. 现在，您的新 web 项目在本地运行调试下单击浏览按钮。 此示例使用 Internet Explorer。

    ![运行项目][HCVSRunProject]

2. 默认 web 页面的右上方，选择**注册**注册一个新帐户︰

    ![注册一个新账户][HCVSRegisterLocally]

3. 输入用户名和密码︰

    ![输入用户名和密码][HCVSCreateNewAccount]

    这会自动保存您的应用程序的成员资格信息您本地 SQL Server 上创建一个数据库。 其中一个表 (**dbo。AspNetUsers**) 保存的 web 应用程序一样，您只需输入的用户凭据。 在本教程后面部分，您将看到此表。

4. 关闭浏览器窗口的默认网页。 这将停止在 Visual Studio 中的应用程序。

您现在可以为下一步，是发布到 Azure 应用程序并对其进行测试。

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F。 发布到 Azure 的 web 应用程序并测试它 ##

现在您将发布您的应用程序服务 web 应用程序到应用程序，然后进行测试以查看以前配置的混合连接的使用方式来将您的 web 应用程序连接到您的本地计算机上的数据库。

### <a name="publish-the-web-application"></a>发布 web 应用程序 ###

1. 您可以下载您的 Azure 门户应用程序服务 web 应用程序的发布配置文件。 在刀片式服务器为您的 web 应用程序，**获取发布配置文件**，请单击，然后将该文件保存到您的计算机。

    ![下载发布配置文件][PortalDownloadPublishProfile]

    接下来，您会将此文件导入 Visual Studio 的 web 应用程序。

2. 在 Visual Studio 中，用鼠标右键单击解决方案资源管理器中的项目名称并选择**发布**。

    ![选择发布][HCVSRightClickProjectSelectPublish]

3. 在**发布网站**对话框中，在**配置文件**选项卡上选择**导入**。

    ![导入][HCVSPublishWebDialogImport]

4. 浏览到您已下载的发布配置文件，请选择它，然后单击**确定**。

    ![浏览到配置文件][HCVSBrowseToImportPubProfile]

5. 您发布的信息导入并显示在对话框中的**连接**选项卡上。

    ![单击发布][HCVSClickPublish]

    单击**发布**。

    当发布完成时，浏览器将启动并显示现在熟悉的 ASP.NET 应用程序，只不过现在它处于实时 Azure 的云 ！

接下来，将使用实时 web 应用程序以查看操作中的混合连接。

### <a name="test-the-completed-web-application-on-azure"></a>在 Azure 上已完成的 web 应用程序进行测试 ###

1. 在顶部右侧的 Azure 上网页，选择**以用户身份登录**。

    ![测试日志中][HCTestLogIn]

2. 您的应用程序服务 web 应用程序现在连接到本地计算机上的 web 应用程序的成员资格数据库。 若要验证这一点，请使用先前输入的本地数据库中的相同凭据登录。

    ![问候问候][HCTestHelloContoso]

3. 若要进一步测试新的混合连接，注销 Azure 的 web 应用程序和注册为另一个用户。 提供了一个新的用户名和密码，然后单击**注册**。

    ![测试注册另一个用户][HCTestRegisterRelecloud]

4. 要验证已通过混合连接本地数据库中已存储新用户的凭据，请在您的本地计算机上打开 SQL 管理 Studio。 在对象资源管理器中展开的**MembershipDB**数据库，然后再展开**表**。 用鼠标右键单击**dbo。AspNetUsers**成员资格表，选择要查看的结果的**选择前 1000年行**。

    ![查看结果][HCTestSSMSTree]

5. 现在，本地成员资格表显示这两个帐户的本地，创建一个，一个在 Azure 的云环境中创建。 在云中创建已保存到本地数据库通过 Azure 的混合连接功能。

    ![在内部数据库中注册的用户][HCTestShowMemberDb]

现在，您已经创建并部署 ASP.NET web 应用程序使用 Azure 的云在 web 应用程序与本地 SQL Server 数据库之间的混合连接。 祝贺您 ！

## <a name="see-also"></a>请参见 ##
[混合连接概述](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[乔什扭转引入了混合连接 （频道 9 视频）](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[混合连接概述](/services/biztalk-services/)

[BizTalk 服务︰ 仪表板、 显示器、 比例、 配置和混合连接选项卡](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[构建无缝应用程序可移植性 (视频频道 9) 与真实的混合云](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[在 Azure 应用程序服务使用混合连接来访问本地资源](../app-service-web/web-sites-hybrid-connection-get-started.md)

[ASP.NET 身份概述](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
