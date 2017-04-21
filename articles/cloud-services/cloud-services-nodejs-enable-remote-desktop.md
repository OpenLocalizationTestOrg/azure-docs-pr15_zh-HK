<properties 
    pageTitle="启用远程桌面的云服务 (Node.js)" 
    description="了解如何启用 Azure Node.js 应用程序所在的虚拟机的远程桌面访问。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>启用在 Azure 中的远程桌面

远程桌面使您可以访问的桌面运行在 Azure 角色实例。 可以使用远程桌面连接以将虚拟机配置或应用程序的疑难解答。

> [AZURE.NOTE] 本文适用于 Node.js Azure 云服务作为承载的应用程序。


## <a name="prerequisites"></a>系统必备组件

- 安装和配置[Azure Powershell](../powershell-install-configure.md)。
- 将 Node.js 应用程序部署到 Azure 的云服务。 有关详细信息，请参阅[生成并部署 Node.js 对 Azure 云服务的应用程序](cloud-services-nodejs-develop-deploy-app.md)。


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>步骤 1︰ 使用 Azure PowerShell 配置用于远程桌面访问服务

若要使用远程桌面，您需要使用用户名、 密码和证书更新 Azure 服务定义和配置。 

从包含您的应用程序的源文件的计算机执行以下步骤。

1. 以管理员身份运行**Windows PowerShell** 。 （从**「 开始 」 菜单**或**启动屏幕**，请搜索**Windows PowerShell**。）

2.  定位到包含服务定义 (.csdef) 和服务 (.cscfg) 配置文件的目录。

3. 输入以下 PowerShell cmdlet:

        Enable-AzureServiceProjectRemoteDesktop

4. 在出现提示时，输入用户名和密码。

    ![启用 azureserviceprojectremotedesktop][enable-rdp]

3.  输入下面的 PowerShell cmdlet，若要发布所做的更改︰

        Publish-AzureServiceProject

    ![将发布 azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>步骤 2︰ 连接到角色实例

发布更新服务定义之后，您可以连接到角色实例。

1.  在[Azure 的传统门户网站]中，选择**云服务**，然后选择服务。

    ![Azure 的传统门户网站][cloud-services]

2.  **实例**，请单击，然后单击**生产**或**临时**以查看您的服务的实例。 选择一个实例，然后单击页面底部的**连接**。

    ![实例页][3]

2.  当您单击**连接**时，web 浏览器会提示您保存.rdp 文件。 打开此文件。 （例如，如果您正在使用 Internet Explorer，单击**打开**。）

    ![提示若要打开或保存.rdp 文件][4]

3.  当打开文件时，将显示以下安全提示︰

    ![Windows 安全提示][5]

4.  单击**连接**安全提示将显示用于输入凭据以访问该实例。 输入 [步骤 1] 中创建的密码 [第 1 步︰ 配置远程桌面访问使用 Azure PowerShell 的服务]，然后单击**确定**。

    ![用户名/密码提示][6]

当进行连接时，远程桌面连接将显示在 Azure 实例的桌面。 

![远程桌面会话][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>步骤 3: 将服务配置为禁用远程桌面访问 

当您不再需要远程桌面连接到云环境中的角色实例时，禁用远程桌面访问使用[Azure PowerShell]。

1.  输入以下 PowerShell cmdlet:

        Disable-AzureServiceProjectRemoteDesktop

2.  输入下面的 PowerShell cmdlet，若要发布所做的更改︰

        Publish-AzureServiceProject

## <a name="additional-resources"></a>其他资源

- [远程访问在 Azure 角色实例] 
- [远程桌面使用 Azure 角色]
- [Node.js 开发人员中心](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure 的传统门户网站]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[远程访问在 Azure 角色实例]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[远程桌面使用 Azure 角色]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 