<properties
    pageTitle="Azure 堆栈的应用程序服务技术预览 1 开始之前 |Microsoft Azure"
    description="部署在 Azure 堆栈上的 Web 应用程序之前，先完成步骤"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
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
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>在您开始使用 Azure 堆栈 Web 应用程序之前

> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

您将需要安装 Azure 堆栈 Web 应用程序的几个项目︰

- 已完成的部署的[Azure 堆栈技术预览 1](azure-stack-run-powershell-script.md)
- Azure 堆栈系统小的 Azure 堆栈 Web 应用程序部署中的空间不足。  所需的空间大约是 20 GB 的磁盘空间。
- [正在运行 SQL Server 的服务器](#SQL-Server)。

>[AZURE.NOTE] 以下步骤全部发生在客户端虚拟机。

## <a name="before-you-deploy-azure-stack-web-apps"></a>Azure 堆栈 Web 应用程序部署之前

部署资源提供程序，您必须以管理员身份运行 PowerShell 集成脚本 Environment(ISE)。 由于这个原因，您需要允许 Internet Explorer 配置文件用来登录到 Azure Active Directory 中的 cookie 和 JavaScript。

## <a name="turn-off-internet-explorer-enhanced-security"></a>关闭 Internet Explorer 增强安全性

1.  登录到**AzureStack/管理员**，Azure 堆栈的概念验证 (POC) 计算机，然后打开**服务器管理器**。
2.  管理员和用户关闭**Internet Explorer 增强的安全配置**。
3.  ClientVM.AzureStack.local 虚拟机以管理员的身份登录，然后打开**服务器管理器**。
4.  管理员和用户关闭**Internet Explorer 增强的安全配置**。

## <a name="enable-cookies"></a>启用 cookie

1.  选择**开始**>**所有应用程序**，> **Windows 附件**。 用鼠标右键单击**Internet Explorer** > **更多** > **以管理员身份运行**。
2.  如果系统提示您，**建议使用的安全性**，选择，然后选择**确定**。
3.  在 Internet Explorer 中选择**工具**（齿轮图标），> **Internet 选项** > **隐私** > **高级**。
4.  选择**高级**。 确保两个**接受**复选框都被选中，然后再次选择**确定**和**确定**。
5.  关闭 Internet Explorer，并重新启动以管理员身份 PowerShell ISE。

## <a name="install-the-latest-version-of-azure-powershell"></a>安装最新版本的 Azure PowerShell

1.  以**AzureStack/管理员**身份登录到 Azure 堆栈 POC 计算机。
2.  使用远程桌面连接到 ClientVM.AzureStack.local 虚拟机以管理员身份登录。
3.  打开**控制面板**并选择**卸载程序**。 
4.  在**Microsoft Azure PowerShell-11 月 2015年**上右键单击，然后选择**卸载**。
5.  卸载完成后，重新启动 ClientVM.AzureStack.local 虚拟机
6.  下载并安装最新的[Azure PowerShell](http://aka.ms/azstackpsh)。


## <a name="sql-server"></a>SQL Server

默认情况下，Azure 堆栈 Web 应用程序安装程序设置为使用 Azure 堆栈 SQL 资源提供程序以及安装 SQL Server。 当您安装 SQL Server 资源提供程序 (SQL RP) 确保您记下的数据库管理员的用户名和密码。 您需要同时安装 Azure 堆栈 Web 应用程序时。
注意︰ 您还将部署和使用另一台服务器来运行 SQL Server 的选项。 您可以选择在完成 Azure 堆栈 Web 应用程序安装程序中的选项时要使用的 SQL Server 实例。
