<properties
    pageTitle="部署在 Azure 堆栈上的 MySQL 资源提供程序 |Microsoft Azure"
    description="要部署到 Azure 叠 MySQL 资源提供程序的详细的步骤。"
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>部署在 Azure 堆栈用于 WebApps MySQL 资源提供程序

> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

本文按照 MySQL 资源提供程序的设置的详细的步骤上 Azure 堆栈证明 (POC) 的概念，以便可以在 Azure 堆栈，包括使用 MySQL 作为后端 WordPress 站点上开始[使用 MySQL 数据库](azure-stack-mysql-rp-deploy-short.md)创建的[Azure Web 应用程序](azure-stack-webapps-deploy.md)的使用。

## <a name="set-up-steps-before-you-deploy"></a>在部署之前设置的步骤

部署资源提供程序之前，您需要︰

- 具有默认 Windows 服务器映像与.NET 3.5
- 请关闭 Internet Explorer (IE) 增强的安全性
- 安装最新版本的 Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>创建映像的 Windows 服务器包括.NET 3.5

如果下载了 Azure 堆栈位 2/23/2016年之后因为默认基本 Windows Server 2012 R2 映像包含在此下载内容及更高版本的.NET 3.5 框架，您可以跳过此步骤。

如果您下载 2/23/2016年之前，您需要使用.NET 3.5 映像创建 Windows Server 2012 R2 数据中心 VHD 并集是作为平台映像存储库中的默认图像。

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>关闭 IE 增强安全和启用 cookie

若要部署的资源提供，您 PowerShell 集成脚本环境 (ISE) 作为管理员运行，因此您需要允许 Internet Explorer 配置文件用于为管理员和用户登录登录到 Azure Active Directory 中的 cookie 和 JavaScript。

**若要关闭 IE 增强的安全性︰**

1. 登录到 AzureStack/管理员，Azure 堆栈的概念验证 (PoC) 计算机，然后打开服务器管理器。

2. 管理员和用户关闭**IE 增强的安全配置**。

3. **ClientVM.AzureStack.local**虚拟机以管理员的身份登录，然后打开服务器管理器。

4. 管理员和用户关闭**IE 增强的安全配置**。

**若要启用 cookie:**

1. 在 Windows 启动屏幕上，单击**所有应用程序**、 **Windows**附件，右键单击**Internet Explorer**，指向**更多**，然后选择**以管理员身份运行**。

2. 如果出现提示，**建议使用的安全性**，请检查，然后单击**确定**。

3. 在 Internet Explorer 中单击**工具 （齿轮） 图标** &gt; **Internet 选项** &gt; **隐私**选项卡。

4. 单击**高级**，确保这两种**接受**按钮被选中，单击**确定**，然后再次单击**确定**。

5. 关闭 Internet Explorer 并重新启动以管理员身份 PowerShell ISE。

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>安装 Azure PowerShell 的 Azure 堆栈兼容版本

1. 从您的客户端虚拟机中卸载任何现有的 Azure PowerShell。

2. 以 AzureStack/管理员身份登录到 Azure 堆栈 POC 机器。

3. 使用远程桌面时，登录到**ClientVM.AzureStack.local**虚拟机作为管理员。

4. 打开控制面板，单击**卸载程序**&gt;单击**Azure PowerShell** &gt;单击**卸载**。

5. [下载最新的 Azure PowerShell 支持 Azure 堆栈](http://aka.ms/azstackpsh)并安装它。

    安装 PowerShell 之后，您可以运行此验证 PowerShell 脚本以确保可以连接到 Azure 堆栈实例 （应显示登录 web 页）。

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>引导资源提供商部署 PowerShell

1. 将 Azure 堆栈 POC 的远程桌面连接到 clientVm.AzureStack.Local 和 azurestack 作为登录\\azurestackuser。

2. [MySQL RP 二进制文件的下载](http://aka.ms/masmysqlrp)文件并将其解压到 d:\\MySQLRP。

3. 运行 d:\\MySQLRP\\Bootstrap.cmd 文件以管理员身份 (azurestack\administrator)。

    在 PowerShell ISE 中打开 Bootstrap.ps1 文件。

4. 当 windows PowerShell ISE 完成加载时，请单击"播放"按钮，或按 F5。

    将加载两个主要的选项卡，每个包含的所有脚本和部署 MySQL 资源提供程序所需的文件。

## <a name="prepare-prerequisites"></a>准备系统必备组件

单击**准备系统必备组件**选项卡︰

- 创建所需的证书
- 下载到 Azure 堆栈的 MySQL 的二进制文件
- 将项目上载到 Azure 堆栈上存储帐户
- 发布库项目

### <a name="create-the-required-certificates"></a>创建所需的证书
这个**新建的 SslCert.ps1**脚本添加\_。AzureStack.local.pfx SSL 证书为 d:\\MySQLRP\\系统必备组件\\BlobStorage\\容器文件夹。 该证书保护的资源提供者和本地实例的 Azure 资源管理器之间的通信。

1. 在主要**准备系统必备组件**选项卡，单击**New SslCert.ps1**选项卡并运行它。

2. 在出现提示时，键入私钥，**请记下的此密码**保护的 PFX 密码。 您将稍后需要它。

### <a name="download-mysql-binaries-to-your-azure-stack"></a>下载到 Azure 堆栈的 MySQL 的二进制文件

1. 选择**下载 MySqlServer.ps1**选项卡，然后运行它。
2. 出现提示时，请单击**是**中确认对话框中接受 EULA。

    此命令将两个 zip 文件添加到 D:\MySql\Prerequisites\BlobStorage\Container 文件夹。

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>上载到 Azure 堆栈上存储帐户的所有项目

1. 单击**上载-Microsoft.MySql-RP.ps1**选项卡，然后运行它。

2. 在 Windows PowerShell 凭据请求对话框中，键入 Azure 堆栈服务管理员凭据。

3. 提示的 Azure 活动目录租户 id，请键入 Azure Active Directory 租户完全限定的域名称︰ 例如，microsoftazurestack.onmicrosoft.com。

    弹出窗口询问凭据。

    > [AZURE.TIP] 如果没有出现的弹出菜单，您要么没有关闭 IE 增强的安全性，要在这台机器和用户启用 JavaScript 或您没有接受了 IE 中的 cookie。 请参阅[设置在部署之前的步骤](#set-up-steps-before-you-deploy)。

4. 键入 Azure 堆栈服务管理员凭据，然后单击**登录**。

### <a name="publish-gallery-items-for-later-resource-creation"></a>发布以后资源创建的库项目

选择**发布 GalleryPackages.ps1**选项卡，然后运行它。 此脚本将两个市场的项目添加到可用来部署数据库资源市场项目的 Azure 堆栈 POC 门户的市场。

## <a name="deploy-the-mysql-resource-provider-vm"></a>部署虚拟机的 MySQL 资源提供

现在，您已经准备 Azure 堆栈 PoC 的必要证书和市场项目，您可以部署一个 SQL Server 资源提供程序。 请单击**部署 MySQL 提供程序**选项卡︰

   - 提供了部署过程引用 JSON 文件中的值
   - 部署资源提供程序
   - 更新本地 DNS
   - 注册的 SQL Server 资源提供程序适配器

### <a name="provide-values-in-the-json-file"></a>提供在 JSON 文件中的值

单击**Microsoft.MySqlprovider.Parameters.JSON**。 此文件的 Azure 资源管理器模板需要正确地部署到 Azure 叠的参数。

1. 填写在 JSON 文件中的**空**参数︰

    - 请确保您为 MySQL 资源提供虚拟机提供的**adminusername**和**adminpassword** 。

    - 请确保您[准备 prequisites](#prepare-prerequisites)步骤中所记下的**SetupPfxPassword**参数中提供的密码。

    - 请确保您提供的**basicAuthUserName**和**basicAuthPassword**参数。 **记下这些值。** 您将需要他们以后要注册的资源提供程序。

2. 单击**保存**。

### <a name="deploy-the-resource-provider"></a>部署资源提供程序

1. 单击**部署-Microsoft.Mysql-provider.PS1**选项卡，然后运行该脚本。
2. 在 Azure Active Directory 在出现提示时键入您组织的名称。
3. 在弹出的窗口中，将提交堆栈 Azure 服务管理员凭据。

完全部署可能需要一些高利用率的 Azure 堆栈 Poc 在 15 到 45 分钟。

### <a name="update-the-local-dns"></a>更新本地 DNS

1. 单击**注册-Microsoft.MySQL-fqdn.ps1**选项卡，然后运行该脚本。
2. 当提示您为 Azure 活动目录租户 ID 时，输入您 Azure Active Directory 租户完全合格的域名称︰ 例如， **microsoftazurestack.onmicrosoft.com**。

### <a name="register-the-sql-rp-resource-provider"></a>注册 SQL RP 资源提供程序

1. 单击**注册-Microsoft.My-provider.ps1**选项卡，然后运行该脚本。

2. 当系统提示您输入凭据时，使用的**basicAuthUserName**和**basicAuthPassword**参数为记。

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>验证使用 Azure 堆栈门户部署

1. 登录 ClientVM，以**AzureStack\User**身份再次登录。

2. 在桌面上，请单击**Azure 堆栈 POC 门户**和登录为服务管理门户

3. 验证部署成功完成。 单击**浏览** &gt; **资源组**中，单击您使用的资源组 （默认值为**MySQLRP**），然后确保刀片式服务器 （上半部分） 的精要部分读取**部署成功**。


4. 验证注册成功。 单击**浏览** &gt; **资源供应商**，然后寻找**本地 MySQL**。

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>创建第一个 MySQL 数据库来测试部署

1. 以服务管理员身份登录到 Azure 堆栈 POC 门户

2. 单击**+**按钮&gt;**自定义** &gt; **MySQL 服务器和数据库**。

3. 与数据库的详细信息窗体中填写。

    **记下您输入的"服务器名称"。** 您的数据库的连接字符串包含"服务器名称"作为用户名的一部分︰ 例如， ** "user@ <ServerName>"**。 您将需要在连接到数据库时输入用户名以这种格式︰ 例如，当部署 MySQL 网站使用 Azure 网站资源提供程序


## <a name="next-steps"></a>下一步行动

请尝试其他的[PaaS 服务](azure-stack-tools-paas-services.md)如[SQL Server 资源提供者](azure-stack-sql-rp-deploy-short.md)和[Web 应用程序资源提供程序](azure-stack-webapps-deploy.md)。
