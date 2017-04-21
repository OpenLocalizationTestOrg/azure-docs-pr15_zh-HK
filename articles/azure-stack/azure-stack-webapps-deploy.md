<properties
    pageTitle="将一个 Web 应用程序资源提供程序添加到 Azure 堆栈 |Microsoft Azure"
    description="部署在 Azure 堆栈中的 Web 应用程序的详细的指导"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>将 Web 应用程序资源提供程序添加到 Azure 堆栈

> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

将 Web 应用程序资源提供程序添加到 Azure 堆栈有七个步骤︰

1.  下载所需的组件。
2.  创建证书使用 Azure 堆栈 Web 应用程序。
3.  使用安装程序下载阶段并安装 Azure 堆栈 Web 应用程序。 
4.  验证 Web 应用程序安装。
5.  创建 DNS 记录的前端和管理服务器负载平衡器。
6.  使用 ARM 注册新部署的 Web 应用程序资源提供程序。
7.  测试驱动的 Web 应用程序资源提供程序。

## <a name="download-required-components"></a>下载所需的组件

1.  下载[Azure 堆栈的应用程序服务安装预览程序](http://aka.ms/azasinstaller)。 
2.  下载[Azure 堆栈的应用程序服务部署帮助器脚本](http://aka.ms/azashelper)。 
3.  从帮助器脚本压缩文件中提取文件，应该有三个脚本︰
    - 创建 AppServiceCerts.ps1
    - 创建 AppServiceDnsRecords.ps1
    - 注册 AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>创建通过 Azure 堆栈 Web 应用程序要使用的证书

第一个脚本使用 Azure 堆栈证书颁发机构创建 3 证书所需的 Web 应用程序工作正常。 确保您正在 PowerShell 以 azurestack\administrator ClientVM 上运行的脚本︰
1.  在 PowerShell 会话中运行为**azurestack\administrator**，执行**创建 AppServiceCerts.ps1**脚本。  这将创建三个证书，该脚本，所需的 Web 应用程序相同的文件夹中。
2.  输入的密码保护的 pfx 文件并根据需要进入 Azure 堆栈 Web 应用程序安装程序，请记下的它。

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>使用安装程序下载并安装 Azure 堆栈 Web 应用程序

将 appservice.exe 安装程序︰
1.  提示用户接受 Microsoft 和第三方 Eula。
2.  收集的 Azure 堆栈部署信息。
3.  以指定的堆栈 Azure 存储帐户创建 blob 容器。
4.  下载安装 Azure 堆栈 Web 应用程序的资源提供程序所需的文件。
5.  准备安装来部署 Web 应用程序资源提供程序在 Azure 堆栈环境中。
6.  将文件上载到应用程序服务存储帐户。
7.  甩掉 Azure 资源管理器模板所需的信息存在。

以下步骤将指导您完成安装步骤︰

>[AZURE.NOTE] 必须使用提升的权限的帐户 （本地或域管理员） 可执行安装程序。 如果您在登录为 azurestack\azurestackuser，则将提示您提升凭据。 

1.  运行**azurestack\administrator**appservice.exe。 
2.  单击**部署使用 Azure 资源管理器**。

![Azure 堆栈的应用程序服务技术预览 1 安装程序][1]

3.  查看并接受 Microsoft 软件预发行许可条款，然后单击**下一步**。
4.  查看并接受第三 partylicense 条款，然后单击**下一步**。
5.  查看应用程序的云服务的配置信息并单击**下一步**。

![Azure 堆栈的应用程序服务技术预览 1 应用程序服务云配置][2]

6. （Azure 堆栈订阅框旁边），请单击**连接**。

![Azure 堆栈的应用程序服务技术预览 1 应用程序服务云配置屏幕两][3]

7.  在 Azure 堆栈身份验证窗口中提供您的**Azure 活动目录服务管理员帐户**和**密码**，然后单击**登录**。
**注意︰**这是您提供部署 Azure 堆栈时 Azure Active Directory 帐户。
    - 单击在**Azure 堆栈的订阅内容**旁边的框的右侧的**向下箭头**，然后选择您的订购。

![Azure 堆栈的应用程序服务技术预览 1 订阅选择][5]

8.  单击**向下箭头键**在**Azure 堆栈位置**旁边的框的右侧。
    - 选择**本地**。
9. 输入您的管理员**名称**。
10. 输入管理员**密码**。
11. 查看**SQL Server 的详细信息**，必要时进行更改。
12. 查看**系统管理员登录帐户**，必要时进行更改。
13. 输入**系统管理员密码**。
14. 单击**下一步**。  此时安装程序现在将为 SQL Server 提供验证连接详细信息。

![Azure 堆栈的应用程序服务技术预览 1 订阅选择][4]    

15. 单击**Web 应用程序默认的 SSL 证书文件**旁边的**浏览**，定位到**webapps。AzureStack.Local**证书[之前创建](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)。
16. 输入设置时创建的证书的**证书密码**。
17. 单击**资源提供程序的 SSL 证书文件**旁边的**浏览**并导航到**管理。AzureStack.Local**证书[之前创建](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)。
18. 输入设置时创建的证书的**证书密码**。
19. 单击**资源提供程序的根证书文件**旁边的**浏览**并导航到**管理。AzureStack.Local**证书[之前创建](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)。
20. 单击**下一步**安装程序将验证提供的证书密码。

![Azure 堆栈的应用程序服务技术预览 1 证书详细信息][6]

完成部署需要大约 45-60 分钟。

![Azure 堆栈的应用程序服务技术预览 1 安装进度][7]

21. 安装程序成功完成后请单击**退出**。

## <a name="validate-web-apps-installation"></a>验证 Web 应用程序安装

1.  在**Azure 堆栈主机计算机**上打开**Hyper-V 管理器**。
2.  找到的**CN0 虚拟机**和虚拟机**连接**。
![Azure 堆栈的应用程序服务技术预览 1 Hyper-V 管理器][8]
3.  在此虚拟机的桌面双击**Web 云管理控制台**。
![Azure 堆栈的应用程序服务技术预览 1 管理控制台][9]
4.  导航到**受控服务器**。
5.  当所有的机器都**已准备好**进入下一步。 
![Azure 堆栈的应用程序服务技术预览 1 受控的服务器状态][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>创建 DNS 记录的管理服务器和前端的负载平衡器
1.  **Azurestack\administrator**作为打开 PowerShell 实例。
2.  导航到下载并在[预备步骤](#Download-Required-Components)中提取脚本的位置。
3.  这将运行**创建 AppServiceDnsRecords.ps1**脚本，请创建启用的门户网站和 web 应用程序请求路由到前端服务器的 DNS 条目。  在 ARM 部署的 Web 应用程序，两个软件负载平衡器 (SLBs)，在中创建网络资源提供程序。 他们指向管理服务器和前端服务器。 门户和 Azure 堆栈的应用程序服务资源提供程序的基于 ARM 请求转到管理服务器。

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>ARM 将新部署的 Azure 堆栈 Web 应用程序提供程序注册
1.  **Azurestack\administrator**作为打开 PowerShell 实例。
2.  导航到下载并在[预备步骤](#Download-Required-Components)中提取脚本的位置。
3.  运行**注册 AppServiceResourceProvider.ps1**脚本。 

>[AZURE.NOTE] 键入用户名和密码**完全相同 （包括大小写）**的**虚拟机管理员**和安装中的**密码**字段输入或资源提供程序注册将失败。

## <a name="test-drive-azure-stack-web-apps"></a>测试驱动器 Azure 堆栈 Web 应用程序

既然您已经部署并注册 Web 应用程序资源提供程序，您可以测试以确保承租人可以将部署 web 应用程序。

1.  在 Azure 堆栈门户中，单击新建，单击 Web + 移动，请单击 Web 应用程序。
2.  在 Web 应用程序刀片式服务器，键入 Web 应用程序框中的名称。
3.  在资源组中，单击新建，然后在资源组框中键入一个名称。 
4.  请单击应用程序服务计划/位置，然后单击新建。
5.  应用程序服务计划刀片式服务器，在框中键入名称的应用程序服务计划。
6.  单击定价层自由共享共享单击选择，单击确定和，然后单击创建。
7.  在一分钟，为新的 web 应用程序图块将出现在仪表盘上。 单击该图块。
8.  在 web 应用程序刀片式服务器，单击浏览以查看此应用程序的默认网站。


**部署一个 WordPress、 DNN 或 Django 网站 （可选）**

1. 在 Azure 堆栈门户中，请单击"+"，转到 Azure 的市场上，部署 Django 网站，并等待成功完成。 Django 的 web 平台使用文件系统的基于数据库，不需要任何额外的资源提供程序，如 SQL 或 MySQL。  

2. 如果您还部署 MySQL 资源提供程序，您可以部署从市场的 WordPress 网站。 数据库参数系统提示您时，输入的用户名称*User1@Server1*（加上用户名和您选择的服务器名称）。

3. 如果您还部署 SQL Server 资源提供程序，您可以部署 DNN 网站从市场。 当提示您为数据库参数时，运行连接到您的资源供应商的 SQL Server 的计算机选取数据库。

## <a name="next-steps"></a>下一步行动

您还可以尝试其他[平台即服务 (PaaS) 服务](azure-stack-tools-paas-services.md)，如[SQL Server 资源提供者](azure-stack-sql-rp-deploy-short.md)和[MySQL 资源提供程序](azure-stack-mysql-rp-deploy-short.md)。

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
