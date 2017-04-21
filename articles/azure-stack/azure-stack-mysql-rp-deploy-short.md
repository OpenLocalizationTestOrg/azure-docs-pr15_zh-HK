<properties
    pageTitle="作为 Azure 堆栈上的 PaaS 使用 MySQL 数据库 |Microsoft Azure"
    description="了解部署 MySQL 资源提供程序，并作为 Azure 堆栈上服务提供 MySQL 的快速步骤。"
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>作为 Azure 堆栈上的 PaaS 使用 MySQL 数据库

> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

您可以部署在 Azure 堆栈上的一个 MySQL 资源提供程序。 部署资源提供程序之后，可以创建 MySQL 服务器和通过 Azure 资源管理器部署模板的数据库并提供 MySQL 数据库作为一种服务。 MySQL 数据库，是常用的网站上，支持许多网站平台。 部署资源提供程序之后，您可以创建 WordPress 的网站从 Azure Web 应用程序平台作为服务 (PaaS) 加载项的 Azure 堆栈。

## <a name="quick-steps-to-deploy-the-resource-provider"></a>快速部署的资源提供程序的步骤
如果您已经熟悉 Azure 堆栈，请使用这些步骤。 如果您想了解更多信息，请每一节中的链接或直接转到[部署在 Azure 堆栈 POC MySQL 数据库资源提供程序适配器](azure-stack-mysql-rp-deploy-long.md)。

1.  请确保该[完成所有安装步骤](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy)之前部署资源提供程序︰

    - .NET 3.5 框架已经设置在基本的 Windows 服务器映像。 （如果在 2 月，23，2016 后, 下载的 Azure 堆栈位则可以跳过此步骤。）
    - [安装了兼容的 Azure 堆栈的 Azure PowerShell 版](http://aka.ms/azStackPsh)。
    - 在 ClientVM，[启用增强的安全关闭 Internet Explorer 和 cookie](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies)的 Internet Explorer 安全设置。

2. [下载的 MySQL 资源提供程序的二进制文件文件](http://aka.ms/masmysqlrp)并将其解压 Azure 的堆栈的概念 (PoC)，证据在 ClientVM 上。

3. [运行 bootstrap.cmd 脚本和脚本](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment)。

    一组脚本分为两个主要选项卡打开在 PowerShell 集成脚本环境 (ISE)。 运行所有已加载的脚本按顺序从左到右中每个选项卡。

    1. 在**准备**选项卡中从左到右到运行的脚本︰

        - 创建一个通配符证书来保护资源提供者和 Azure 资源管理器之间的通信。
        - 接受的 MySQL EULA 条款并下载 MySQL 的二进制文件。
        - 将证书及所有其他项目上载到 Azure 栈的存储帐户。
        - 这样，您可以部署 MySQL 资源库通过发布库包。

        > [AZURE.IMPORTANT] 如果在提交 Azure Active Directory 租户后无明显原因中的任何脚本挂起，您的安全设置可能会阻止已部署运行时所需的 DLL。 若要解决此问题，请寻找资源提供程序文件夹中的 Microsoft.AzureStack.Deployment.Telemetry.Dll、 右击它、 单击**属性**，然后在**常规**选项卡中检查**取消阻止**。

    2. 在**部署**选项卡中从左到右到运行的脚本︰

        - 资源提供程序，MySQL 服务器和数据库，您将实例化将承载[部署虚拟机 (VM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) 。 此脚本引用 JSON 参数文件，您需要在运行脚本之前更新一些值。
        - [注册的本地 DNS 记录](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS)将映射到资源供应商 VM。
        - [注册资源提供](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider)与本地 Azure 资源管理器中。

        > [AZURE.IMPORTANT] 所有的脚本假定基本操作系统映像满足先决条件 (.NET 3.5 安装，JavaScript 和 ClientVM 和 Azure PowerShell 安装最新版本上启用 cookie)。 如果在运行脚本时出现错误，请仔细检查您满足了先决条件。

5. [测试新的 MySQL 资源提供程序](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)，部署 MySQL 数据库从 Azure 堆栈门户。 单击**创建** &gt; **自定义** &gt; **MySQL 服务器和数据库**。

这应该会看到启动 MySQL 资源提供程序以及运行在 25 分钟的时间。
