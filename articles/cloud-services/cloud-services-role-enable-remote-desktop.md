<properties 
pageTitle="启用远程桌面连接的 Azure 的云服务中的角色" 
description="如何配置 azure 的云服务应用程序，以允许远程桌面连接" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>启用远程桌面连接的 Azure 的云服务中的角色

>[AZURE.SELECTOR]
- [Azure 的传统门户网站](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


远程桌面可以访问运行于 Azure 角色的桌面。 您可以使用远程桌面连接来诊断并排除问题的应用程序运行时。 

您可以启用远程桌面连接中您的角色在开发过程中通过在服务定义中包括远程桌面模块也可以选择要启用远程桌面扩展到远程桌面。 首选的方法是使用远程桌面扩展，因为即使在部署应用程序而无需重新部署应用程序后，您可以启用远程桌面。 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>从 Azure 经典门户配置远程桌面
Azure 的传统门户网站使用的远程桌面扩展方法，因此即使在部署应用程序后，您可以启用远程桌面。 云服务的**配置**页允许您启用远程桌面，更改本地管理员帐户用于连接到虚拟机，该证书用于身份验证和设置过期日期。 


1. 单击**云服务**，单击云服务的名称，然后单击**配置**。

2. 单击**远程**。
    
    ![云服务远程](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] 所有角色实例将重新都启动，当您第一次启用远程桌面，请单击确定 （复选标记）。 若要避免重新启动，用密码进行加密的证书必须安装在角色上。 若要防止重新启动计算机，[传云服务的证书](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service)，然后返回到该对话框。
    

3. **角色**，选择您想要更新或选择**全部**所有角色的角色。

4. 可进行如下更改︰
    
    - 若要启用远程桌面，请选择**启用远程桌面**复选框。 若要禁用远程桌面，请清除复选框。
    
    - 创建要在远程桌面连接角色实例中使用的帐户。
    
    - 更新现有的帐户的密码。
    
    - 选择上载的证书用于身份验证 （使用**证书**页上的**上载**证书上载），或创建新的证书。 
    
    - 更改远程桌面配置的到期日期。

5. 完成配置更新之后，单击**确定**（复选标记）。


## <a name="remote-into-role-instances"></a>为角色实例的远程
一旦角色启用了远程桌面，您可以将远程到通过各种工具的角色实例。

要从 Azure 经典门户连接角色实例︰
    
  1.   单击以打开**实例**页上的**实例**。
  2.   选择一个已配置的远程桌面的角色实例。
  3.   单击**连接**，然后按照说明进行操作以打开桌面。 
  4.   单击**打开**，然后**连接**以启动远程桌面连接。 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>使用 Visual Studio 远程进入角色实例

在 Visual Studio 中，服务器资源管理器︰

1. 展开**Azure\\云服务\\[云服务名称]**节点。
2. 展开**临时**或**生产**。
3. 展开单个角色。
4. 用鼠标右键单击其中一个角色实例单击**连接时使用远程桌面...**，，然后输入用户名和密码。 

![服务器资源管理器的远程桌面](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>使用 PowerShell 的 RDP 文件
您可以使用[Get AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet 检索 RDP 文件。 然后可以使用远程桌面连接使用 RDP 文件访问云服务。

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>以编程方式下载通过服务管理 REST API 的 RDP 文件
[下载 RDP 文件](https://msdn.microsoft.com/library/jj157183.aspx)其余部分操作可用于下载的 RDP 文件。 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>若要配置的服务定义文件中的远程桌面

此方法允许您在开发过程中的应用程序中启用远程桌面。 这种方法需要加密的密码将存储在您的服务配置文件和远程桌面配置的任何更新将需要重新部署应用程序。 如果您想要避免您应使用上面描述的远程桌面扩展基于方法的这些不足之处。  

您可以使用 Visual Studio[启用远程桌面连接](../vs-azure-tools-remote-desktop-roles.md)使用服务定义文件的方法。  
下面的步骤描述所做的更改所需的服务模型文件来启用远程桌面。 在发布时，Visual Studio 将自动进行这些更改。

### <a name="set-up-the-connection-in-the-service-model"></a>在服务模型中的连接设置 
使用**导入**元素到[ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef)文件导入**远程访问**模块和**RemoteForwarder**模块。

服务定义文件应类似于下面的示例使用`<Imports>`添加元素。

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
[ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg)文件应当类似于下面的示例，请注意`<ConfigurationSettings>`和`<Certificates>`元素。 指定的证书必须[上载到云服务](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service)。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>其他资源

[如何配置云服务](cloud-services-how-to-configure.md)