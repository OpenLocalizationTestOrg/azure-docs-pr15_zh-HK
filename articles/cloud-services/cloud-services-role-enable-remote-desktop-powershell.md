<properties
pageTitle="启用远程桌面连接中使用 PowerShell 的 Azure 云服务的角色"
description="如何配置 azure 的云服务应用程序使用 PowerShell 允许远程桌面连接"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>启用远程桌面连接中使用 PowerShell 的 Azure 云服务的角色

>[AZURE.SELECTOR]
- [Azure 的传统门户网站](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


远程桌面可以访问运行于 Azure 角色的桌面。 您可以使用远程桌面连接来诊断并排除问题的应用程序运行时。

本文介绍如何启用远程桌面使用 PowerShell 您云服务角色。 这篇文章所需的系统必备组件，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。 PowerShell 利用远程桌面扩展，以便在部署应用程序后，您可以启用远程桌面。


## <a name="configure-remote-desktop-from-powershell"></a>配置远程桌面从 PowerShell

[一组 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet 允许您启用指定的角色上的远程桌面或云服务部署的所有角色。 该 cmdlet 可以通过接受一个 PSCredential 对象的*凭据*参数的远程桌面用户指定的用户名和密码。

如果您以交互方式使用 PowerShell，可以轻松地设置 PSCredential 对象通过调用[获取凭据](https://technet.microsoft.com/library/hh849815.aspx)cmdlet。

```
$remoteusercredentials = Get-Credential
```

此命令将显示一个对话框，使您可以为远程用户输入的用户名和密码，以安全的方式。

由于 PowerShell 有助于自动化方案，还可以设置**PSCredential**对象不需要用户交互的方式。 首先，您需要设置一个安全的密码。 在开始使用纯文本密码将其转换为使用[ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx)安全字符串指定。 接下来，您需要将此安全字符串转换为使用[ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx)加密标准字符串。 现在您可以使用[一组内容](https://technet.microsoft.com/library/ee176959.aspx)保存此加密标准字符串。

因此，无需每次键入在密码，还可以创建一个安全的密码文件。 安全的密码文件也优于纯文本文件。 使用以下 PowerShell 创建安全密码文件︰

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] 在设置密码时，请确保您满足[复杂性要求](https://technet.microsoft.com/library/cc786468.aspx)。

要从安全密码文件创建凭据的对象，必须读取该文件的内容，并将它们转换回一个安全字符串，使用[ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx)。

[一组 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet 也接受*到期*参数，它指定用户帐户过期时**的日期时间**。 例如，可以设置帐户过期几天从当前日期和时间。

此 PowerShell 的示例演示如何在云服务上设置远程桌面扩展︰

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
您还可以选择指定的部署插槽和您想要启用远程桌面的角色。 如果未指定这些参数，该 cmdlet 将使**生产**部署插槽中的所有角色上的远程桌面。

远程桌面扩展程序与部署。 如果您创建新的部署服务，您需要启用该部署中的远程桌面。 如果要始终有启用了远程桌面，则应考虑将 PowerShell 脚本集成到您的部署工作流。


## <a name="remote-desktop-into-a-role-instance"></a>为角色实例的远程桌面
[获得 AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet 习惯到云服务的一个特定角色实例的远程桌面。 *LocalPath*参数用于下载本地 RDP 文件。 或者，您可以使用*启动*参数直接启动远程桌面连接对话框来访问云服务角色实例。

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>如果启用了远程桌面扩展服务检查
[获得 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet 显示启用或禁用服务部署远程桌面。 该 cmdlet 返回远程桌面用户和已启用远程桌面扩展的角色的用户名。 默认情况下，出现这种情况在部署插槽，您可以改为使用临时的插槽。

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>从服务中删除远程桌面扩展
如果已经启用了远程桌面扩展部署，并且需要更新远程桌面设置，首先删除该扩展名。 并使用新的设置再次启用它。 例如，如果您想要设置一个新的远程用户帐户或该帐户的密码过期。 已启用了远程桌面扩展的现有部署需要执行此操作。 对于新的部署，您可以只需扩展直接应用。

从部署中删除远程桌面扩展，您可以使用[删除 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) cmdlet。 您还可以选择指定的部署插槽和想要删除远程桌面扩展的角色。

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] 要完全删除扩展配置，则应调用*删除*cmdlet 使用**UninstallConfiguration**参数。
>
>**UninstallConfiguration**参数中卸载应用于此服务的任何扩展配置。 每个扩展配置为与服务配置相关联。 没有**UninstallConfiguration**解除<mark>部署</mark>扩展配置从调用*删除*cmdlet，从而有效地删除扩展。 但是，扩展配置将保持与该服务相关联。



## <a name="additional-resources"></a>其他资源

[如何配置云服务](cloud-services-how-to-configure.md)
