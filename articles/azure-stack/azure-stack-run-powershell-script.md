<properties
    pageTitle="部署 Azure 堆栈 POC |Microsoft Azure"
    description="了解如何准备 Azure 堆栈 POC 和运行来部署 Azure 堆栈 POC PowerShell 脚本。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>部署 Azure 堆栈 POC
若要部署 Azure 堆栈 POC，您首先需要[准备部署计算机](#prepare-the-deployment-machine)，然后[运行 PowerShell 部署脚本](#run-the-powershell-deployment-script)。

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>下载并解压缩 Microsoft Azure 堆栈 POC TP2

在开始之前，请确保您至少 85 GB 的空间。

1. 下载的 Azure 堆栈 POC TP2 由包含以下 12 文件，合计大约 20 GB 的 zip 文件︰
    - 1 MicrosoftAzureStackPOC.EXE
2. 查看许可协议屏幕和自解压程序向导的信息，然后单击**下一步**。
3. 查看隐私声明屏幕和自解压程序向导的信息，然后单击**下一步**。
4. 选择文件以提取，单击**下一步**目标。
    - 默认值是︰ <drive letter>:\<当前文件夹 > \Microsoft Azure 堆栈 POC
5. 查看目标位置屏幕和自解压程序向导的信息，然后单击**提取**的 CloudBuilder.vhdx (~44.5 GB) 和 ThirdPartyLicenses.rtf 文件中提取。

> [AZURE.NOTE] 提取文件后，可以删除该 zip 文件来恢复计算机上的空间。 或者，您可以移动到其他位置，因此，如果您需要重新部署您的 zip 文件无需再次下载的 zip 文件。

## <a name="prepare-the-deployment-machine"></a>准备部署机

1. 请确保您可以实际连接到部署的计算机，或物理控制台访问 （例如 KVM)。 重新启动在步骤 9 中的部署计算机后，您将需要此访问权限。

2. 请确保部署计算机满足[最低要求](azure-stack-deploy.md)。 可以使用[Azure 堆栈技术预览 2 部署检查](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b)以确认您的需求。

3. 以本地管理员身份到 POC 计算机日志。

4. 将 CloudBuilder.vhdx 文件复制到 C:\CloudBuilder.vhdx。

    > [AZURE.NOTE] 如果您选择不使用建议的脚本要准备 POC 主机计算机 （步骤 5 – 第 7 步），则不要输入任何许可证密钥激活页。 试用版的 Windows 服务器 2016年图像是包括在内，并输入许可密钥，从而导致到期警告消息。

5. POC 的计算机上运行下面的 PowerShell 脚本，将 Azure 堆栈 TP2 支持文件下载︰

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    此脚本将 Azure 堆栈 TP2 支持文件下载到 $LocalPath 参数所指定的文件夹中。
    
6. 打开提升的 PowerShell 控制台并将目录更改为复制文件的位置。
    - 11 MicrosoftAzureStackPOC-N.BIN （其中 N 是 1-11）
7. 用鼠标右键单击 MicrosoftAzureStackPOC.EXE > 以管理员身份运行。

8. 运行 PrepareBootFromVHD.ps1 脚本。 此脚本与无人参与文件则都提供了与该版本一起提供的其他支持脚本。
    有五个参数，此 PowerShell 脚本︰
    - CloudBuilderDiskPath （必填） – 主机上 CloudBuilder.vhdx 的路径。
    - DriverPath （可选） – 允许您为虚拟硬盘中的主机添加其他驱动程序。
    - ApplyUnattend （可选） – 指定此开关参数以实现自动化的操作系统配置。 如果指定，则该用户必须提供 AdminPassword （需要提供随附的文件 unattend_NoKVM.xml） 启动时配置操作系统。
    如果不使用此参数，而不必进一步自定义使用泛型 unattend.xml 文件。 重启之后，您将需要 KVM 到完整的自定义项。
    - AdminPassword （可选） – 仅使用 ApplyUnattend 参数设置时，需要至少 6 个字符。
    - VHDLanguage （可选） – 指定 VHD 语言中，默认设置为"EN-US"。
    介绍了该脚本，并包含示例用法，但最常见的用法是︰
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        如果您运行此确切的命令，您必须输入提示符处 AdminPassword。

9. 脚本完成后，您必须确认重新启动。 如果有其他用户登录，则此命令将失败。 如果命令失败，请运行以下命令︰`Restart-Computer -force` 

10. 到 CloudBuilder.vhdx，在其中部署继续操作系统重新启动主机。

> [AZURE.IMPORTANT] Azure 堆栈需要接入互联网，直接或通过透明代理。 TP2 POC 部署支持一个 NIC 的网络。 如果您有多个 Nic，请确保只有一个已启用 （已禁用所有其他协议） 在下一节中运行部署脚本之前。

## <a name="run-the-powershell-deployment-script"></a>运行 PowerShell 部署脚本

1. 以本地管理员身份到 POC 计算机日志。 使用前面的步骤中指定的凭据。

2. 打开提升的 PowerShell 控制台。

3. 继续，运行以下命令︰`cd C:\CloudDeployment\Configuration`

4. 运行部署命令︰`.\InstallAzureStackPOC.ps1`

5. **输入密码**提示时，输入密码，然后确认它。 这是给所有的虚拟机的密码。 一定要记录下来。

6. Azure Active Directory 帐户中输入的凭据。 此用户必须是目录租户中的全局管理员。

7. 部署过程可能要花几个小时，在此期间系统会自动重新启动一次。

    > [AZURE.IMPORTANT] 如果您想要监视部署进度，请以 azurestack\AzureStackAdmin 身份登录。 如果您在登录以本地管理员身份后机器加入到域，您不会看到部署进度。 不重新运行部署，而是以身份登录 azurestack\AzureStackAdmin 来验证其正在运行。

    如果部署成功，PowerShell 控制台显示︰**完成︰ 操作部署**。

    如果部署失败，您可以尝试[重新运行它](azure-stack-rerun-deploy.md)。 或者，您可以[重新部署](azure-stack-redeploy.md)它从零开始。

### <a name="deployment-script-examples"></a>部署脚本示例

如果您 AAD 的身份只是与一个 AAD 目录相关联︰

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

如果您 AAD 的身份与更比一个 AAD 目录相关联︰

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

如果您的环境中没有启用 DHCP，则必须包含下面这些附加参数 （提供的示例用法） 上方的选项之一︰

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>InstallAzureStackPOC.ps1 的可选参数

| 参数 | 必需/可选 | 说明 |
| --------- | ----------------- | ----------- |
| AADAdminCredential | 可选 | 设置 Azure Active Directory 用户名称和密码。 这些 Azure 的凭据可以是 Microsoft 客户或组织 ID。 若要使用 Microsoft 帐户凭据，请省略此参数在该 cmdlet。 在部署过程中忽略此参数提示 Azure 身份验证弹出。 这将创建在部署期间使用的身份验证和刷新标记。 |
| AADDirectoryTenantName | 必填 | 设置客户端目录。 此参数用于指定一个特定的目录位置 AAD 帐户具有管理多个目录的权限。 完整名称的格式为 AAD 目录租户<directoryName>。 onmicrosoft.com。 |
| AdminPassword | 必填 | POC 部署过程中创建的所有虚拟机都设置为本地管理员帐户和所有其他用户帐户。 此密码必须与当前主机上的本地管理员密码相匹配。 |
| AzureEnvironment | 可选 | 选择您想要注册此 Azure 堆栈部署的 Azure 环境。 选项包括*公用 Azure*， *Azure 的中国*， *Azure 的美国政府*。 |
| EnvironmentDNS | 可选 | DNS 服务器将 Azure 堆栈部署过程中创建。 若要允许内部解决方案戳的外部名称解析的计算机，提供您现有的基础架构 DNS 服务器。 戳中 DNS 服务器转发到此服务器的未知的名称解析请求。 |
| NatIPv4Address | 所需的 DHCP NAT 支持 | MAS BGPNAT01 设置静态的 IP 地址。 只有 DHCP 无法分配有效的 IP 地址来访问互联网时，才应使用此参数。 |
| NatIPv4DefaultGateway | 所需的 DHCP NAT 支持 | 设置默认网关的静态 IP 地址用于 MAS BGPNAT01。 只有 DHCP 无法分配有效的 IP 地址来访问互联网时，才应使用此参数。  |
| NatIPv4Subnet | 所需的 DHCP NAT 支持 | 用于 DHCP 通过 NAT 支持 IP 子网前缀。 只有 DHCP 无法分配有效的 IP 地址来访问互联网时，才应使用此参数。  |
| PublicVLan | 可选 | 设置 VLAN id。 MAS BGPNAT01 和主机必须配置 VLAN ID 才可访问物理网络 （和 Internet） 才使用此参数。 例如，`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| 重新运行 | 可选 | 使用此标志来重新运行部署。  使用以前的所有输入。 不支持重新输入以前提供的数据，因为几个唯一值是生成的用于部署。 |
| 时间服务器 | 可选 | 如果您需要指定一个特定的时间服务器，请使用此参数。 |

## <a name="next-steps"></a>下一步行动

[连接到 Azure 堆栈](azure-stack-connect-azure-stack.md)
