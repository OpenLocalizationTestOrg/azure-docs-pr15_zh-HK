<properties
   pageTitle="Azure AD 连接: 系统必备组件和硬件 |Microsoft Azure"
   description="本主题描述必备和 Azure AD 连接的硬件要求"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>先决条件 Azure AD 连接
本主题介绍必备组件和 Azure AD 连接的硬件要求。

## <a name="before-you-install-azure-ad-connect"></a>在安装 Azure AD 连接之前
在安装 Azure AD 连接之前，有几件事情需要。

### <a name="azure-ad"></a>Azure 的广告
- Azure 的订阅或[Azure 的试用订阅](https://azure.microsoft.com/pricing/free-trial/)。 它只需要访问 Azure 的门户，而不是针对使用 Azure AD 连接。 如果您正在使用 PowerShell 或 Office 365 不需要 Azure 的订阅以使用 Azure AD 连接。 如果您拥有 Office 365 许可证您还可以使用 Office 365 门户。 使用付费的 Office 365 许可证还可以到 Azure 入口从 Office 365 门户。
    - 您还可以在[Azure 门户](https://portal.azure.com)使用 Azure 广告预览功能。 此门户不需要 Azure 的许可证。
- [添加验证域和](active-directory-add-domain.md)您打算使用 Azure 的广告中。 例如，如果您打算使用 contoso.com 为用户，则请确保此域已经过验证，不只使用 contoso.onmicrosoft.com 默认域。
- Azure AD 租户可增加 50 k 的默认对象。 当您验证您的域时，该限制被增至 300 k 对象。 如果在 Azure 广告需要开启支持案例有进一步增加的限制，就需要更多的对象。 如果需要超过 500 个 k 对象则需要 Office 365、 Azure AD 基本、 Azure AD 津贴或企业移动套件许可证。

### <a name="prepare-your-on-premises-data"></a>准备内部数据
- 查看[可以在 Azure AD 中的可选同步功能](active-directory-aadconnectsyncservice-features.md)和评估应启用哪些功能。

### <a name="on-premises-servers-and-environment"></a>的内部部署服务器和环境
- AD 架构版本和林功能级别必须是 Windows Server 2003 或更高版本。 域控制器可以运行任何版本，只要满足架构和林级要求。
- 如果您计划使用**密码写回**功能域控制器必须是 Windows Server 2008 （与最新的 SP) 或更高版本。 如果 Dc 是 2008 年 (pre-R2) 中，则也必须将应用[修补程序 KB2386717](http://support.microsoft.com/kb/2386717)。
- Azure 广告所使用的域控制器必须是可写的。 不支持用于 RODC （只读域控制器） 和 Azure AD 连接并不遵循任何写重定向。
- Azure AD 连接不能安装在小型企业服务器或 Windows 服务器基本套件。 服务器必须使用 Windows 服务器或更好的标准。
- Azure AD 连接服务器必须安装一个完整 GUI。 它不支持在服务器核心安装。
- Windows Server 2008 上或更高版本，则必须安装 azure AD 连接。 如果使用快速设置，此服务器可能是域控制器或成员服务器。 如果您使用自定义设置，服务器也可以是独立，并不一定会加入到域。
- 如果您在 Windows Server 2008 上安装了 Azure AD 连接，请确保从 Windows Update 应用最新的修补程序。 安装不能开始与未应用修补程序的服务器。
- 如果您打算使用**密码同步**功能，Azure AD 连接服务器必须是 Windows Server 2008 R2 SP1 或更高版本。
- Azure AD 连接服务器必须具有[.NET Framework 4.5.1](#component-prerequisites)或更高版本， [Microsoft PowerShell 3.0](#component-prerequisites)或更高版本已安装。
- 如果部署 Active Directory 联合身份验证服务时，AD FS 或 Web 应用程序代理的安装位置的服务器必须是 Windows Server 2012 R2 或更高版本。 [Windows 远程管理](#windows-remote-management)必须启用对这些服务器进行远程安装。
- 如果在部署 Active Directory 联合身份验证服务，则需要[SSL 证书](#ssl-certificate-requirements)。
- 如果在部署 Active Directory 联合身份验证服务，则需要配置[名称解析](#name-resolution-for-federation-servers)。
- Azure AD 连接需要一个 SQL Server 数据库来存储标识数据。 默认情况下安装 SQL Server 2012年直通 LocalDB （浅版本的 SQL Server Express） 并在本地计算机上创建该服务的服务帐户。 SQL Server Express 具有 10 GB 大小限制，您可以管理大约 100000 个对象。 如果您需要管理有较多的目录对象，您需要安装向导指向不同的 SQL Server 安装。
- 如果您使用单独的 SQL Server，则适用这些要求︰
    - Azure AD 连接支持 SQL Server 2014年到 Microsoft SQL Server （带有 SP4) 的 SQL Server 2008年中的所有特点。 Microsoft Azure SQL 数据库作为数据库是**不受支持**。
    - 您必须使用不区分大小写的 SQL 排序规则。 这些都用来标识\_CI_ 在它们的名称。 此方法是**不支持**使用区分大小写的排序规则，由\_CS_ 在它们的名称。
    - 您只能有一个同步引擎，每个数据库实例。 此方法是**不支持**使用 FIM/MIM 同步、 目录同步或 Azure AD 同步共享数据库实例。

### <a name="accounts"></a>帐户
- 要结合 Azure 的广告目录的 Azure AD 全局管理员帐户。 这必须是一个**学校或单位的帐户**，不能为**Microsoft 客户**。
- 如果您使用快速设置或从目录同步升级，然后必须具有企业管理员帐户为本地活动目录。
- [在 Active Directory 帐户](./connect/active-directory-aadconnect-accounts-permissions.md)如果您使用自定义设置安装路径。

### <a name="azure-ad-connect-server-configuration"></a>Azure AD 连接服务器配置
- 如果全局管理员已启用的 MFA，URL **https://secure.aadcdn.microsoftonline-p.com**必须在受信任的站点列表中。 提示您添加到受信任的站点列表如果未添加之前提示您输入一个 MFA 的挑战。 您可以使用 Internet Explorer 将其添加到受信任的站点。

### <a name="connectivity"></a>连接
- Azure AD 连接服务器需要 intranet 和 internet 的 DNS 解析。 DNS 服务器必须能够解析名称都将内部部署 Active Directory 以及 Azure 广告的终结点。
- 如果 Intranet 上有防火墙，您需要在 Azure AD 连接服务器和域控制器之间打开的端口，然后看到[Azure AD 连接端口](active-directory-aadconnect-ports.md)的详细信息。
- 如果必须打开您代理服务器或防火墙的限制可访问的 Url，然后介绍在[Office 365 Url 和 IP 地址的范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)中的 Url。
    - 如果您使用的 Microsoft 云在德国或 Microsoft Azure 政府云，然后看到[Azure AD 连接同步服务实例注意事项](active-directory-aadconnect-instances.md)的 Url。
- 默认情况下使用 TLS 1.0 与 Azure 广告进行通信是 azure AD 连接。 您可以通过更改此到 TLS 1.2[的 Azure AD 连接启用 TLS 1.2](#enable-tls-12-for-azure-ad-connect)中的步骤。
- 如果将出站代理服务器用于连接到 Internet，必须能够连接到互联网和 Azure AD 安装向导和 Azure AD 连接同步添加**C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config**文件中的以下设置。 必须在文件的底部输入该文本。 在此代码中， &lt;PROXYADRESS&gt;表示实际的代理服务器的 IP 地址或主机名。

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- 如果您的代理服务器要求身份验证，[服务帐户](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts)必须位于域中，并且必须使用自定义的设置安装路径来指定[自定义服务帐户](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components)。 您还需要对 machine.config 不同更改。 与此 machine.config 安装向导更改，同步引擎响应身份验证请求的代理服务器。 所有的安装向导中使用页，不**配置**页中，签名包括在用户的凭据。 在结尾的安装向导的**配置**页中，上下文切换到由您创建的[服务帐户](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts)。 Machine.config 部分应如下所示。

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

有关[默认代理元素](https://msdn.microsoft.com/library/kd3cf2ex.aspx)的详细信息，请参阅 MSDN。

如果有连接问题，请参阅[连接问题的疑难解答](active-directory-aadconnect-troubleshoot-connectivity.md)。

### <a name="other"></a>其他
- 可选︰ 测试用户帐户验证同步。

## <a name="component-prerequisites"></a>必备组件

### <a name="powershell-and-net-framework"></a>PowerShell 和.Net 框架
Azure AD 连接取决于 Microsoft PowerShell 和.NET Framework 4.5.1。 您需要此版本或更高版本安装在您的服务器上。 这取决于您的 Windows 服务器版本，执行以下操作︰

- Windows 服务器 2012R2
  - 默认情况下安装 Microsoft PowerShell，不不需要任何操作。
  - 通过 Windows Update 提供了.NET Framework 4.5.1 和更高版本。 请确保您的 Windows 服务器，在控制面板中安装最新的更新。
- Windows 服务器 2008R2 和 Windows Server 2012
  - 最新版本的 Microsoft PowerShell 位于**Windows 管理框架 4.0**， [Microsoft 下载中心](http://www.microsoft.com/downloads)上可用。
  - 在[Microsoft 下载中心](http://www.microsoft.com/downloads)上有.NET Framework 4.5.1 和更高版本。
- Windows Server 2008
  - PowerShell 最新支持的版本位于**Windows 管理框架 3.0**， [Microsoft 下载中心](http://www.microsoft.com/downloads)上可用。
 - 在[Microsoft 下载中心](http://www.microsoft.com/downloads)上有.NET Framework 4.5.1 和更高版本。

### <a name="enable-tls-12-for-azure-ad-connect"></a>Azure AD 连接启用 TLS 1.2
Azure AD 连接使用 TLS 1.0，默认情况下加密同步引擎服务器和 Azure 的广告之间的通信。 您可以更改此配置.Net 应用程序默认情况下，在服务器上使用 TLS 1.2。 可以在[Microsoft 安全公告 2960358](https://technet.microsoft.com/security/advisory/2960358)找到 TLS 1.2 有关的详细信息。

1. Windows Server 2008 上无法启用 TLS 1.2。 您需要 Windows Server 2008R2 或更高版本。 请确保您已经为您的操作系统安装.Net 4.5.1 修补程序，请参阅[Microsoft 安全公告 2960358](https://technet.microsoft.com/security/advisory/2960358)。 您可能有这样或更高版本已经安装在您的服务器上。
2. 如果您使用 Windows Server 2008R2，请确保启用了 TLS 1.2。 在 Windows Server 2012 服务器和更高版本，应已启用 TLS 1.2。
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. 对于所有操作系统，设置此注册表项并重新启动服务器。
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. 如果您还想要同步引擎服务器与远程 SQL Server 之间启用 TLS 1.2，然后确保已安装了[Microsoft SQL Server 支持 TLS 1.2](https://support.microsoft.com/kb/3135244)所需的版本。

## <a name="prerequisites-for-federation-installation-and-configuration"></a>联合身份验证安装和配置的前提条件

### <a name="windows-remote-management"></a>Windows 远程管理
当使用 Azure AD 连接部署 Active Directory 联合身份验证服务或 Web 应用程序代理，请检查下面的要求以确保连接和配置将会成功︰

- 如果目标服务器加入域，请确保已启用 Windows 远程管理
    - 在提升 PSH 命令窗口中，使用命令`Enable-PSRemoting –force`
- 如果目标服务器的非域连接的 WAP 机，有几个额外要求
    - 目标机器 （WAP 计算机）︰
         - 确保 winrm (Windows 远程管理 / WS 管理) 通过服务管理单元中运行服务
         - 在提升 PSH 命令窗口中，使用命令`Enable-PSRemoting –force`
    - 在其 （如果目标计算机是域非域联接或不受信任） 运行该向导的计算机︰
        - 在提升 PSH 命令窗口中，使用命令`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - 在服务器管理器中︰
             - 添加到计算机池 DMZ WAP 主机 (服务器管理器-> 管理-> 添加服务器...使用 DNS 选项卡)
             - 服务器管理器所有服务器选项卡︰ 右键单击 WAP 服务器并选择管理另存为...，WAP 机输入凭据设置本地 （不是域）
             - 若要验证远程 PSH 连接，在服务器管理器的所有服务器选项卡中︰ 右键单击 WAP 服务器并选择 Windows PowerShell。  远程会话 PSH 应打开，以确保可以建立远程 PowerShell 会话。

### <a name="ssl-certificate-requirements"></a>SSL 证书要求
**要点︰**具有强烈建议在 AD FS 服务器场中的所有节点以及所有 Web 应用程序代理服务器使用相同的 SSL 证书。

- 该证书必须包含 X509 证书。
- 可以在测试实验室环境中的联合身份验证服务器上使用自签名的证书。 但是，对于生产环境中，我们建议您从公共 CA 获取证书。
    - 如果使用不受信任公共证书，确保每个 Web 应用程序代理服务器上安装的证书是受信任和所有联合服务器上本地服务器
- 该证书的标识必须匹配的联合身份验证服务名 (例如，sts.contoso.com)。
    - 标识是类型 dNSName 的任一主题备用名称 (SAN) 扩展，或者，如果没有 SAN 的条目，使用者名称指定为通用名称。  
    - 只要其中之一与联合身份验证服务名称相匹配，可以出现在证书中，SAN 的多个项。
    - 如果您打算使用联接的工作场所，其他 SAN 是必需的值**enterpriseregistration。** 跟您的组织的用户主体名称 (UPN) 后缀，如**enterpriseregistration.contoso.com**。
- 不支持基于 CryptoAPI 下一代 (CNG) 密钥和密钥存储提供程序的证书。 这意味着您必须使用基于 CSP （加密服务提供程序） 和不 KSP （密钥存储提供程序） 的证书。
- 支持通配符证书。

### <a name="name-resolution-for-federation-servers"></a>联合身份验证服务器的名称解析
- 建立了内联网 （内部 DNS 服务器） 和外部 (公用 DNS 域注册通过) 的 AD FS 联合身份验证服务名称 (例如 sts.contoso.com) 的 DNS 记录。 对于 intranet DNS 记录确保您使用的记录和不 CNAME 记录。 这是必需的 windows 身份验证，您加入域的计算机中正常工作。
- 如果您要部署多个 AD FS 服务器或 Web 应用程序代理服务器，然后确保您已配置了负载平衡器和 AD FS 联合身份验证服务名称 (例如 sts.contoso.com) 的 DNS 记录指向负载平衡器。
- 对于 windows 集成身份验证可用于企业内部网中使用 Internet Explorer 浏览器应用程序，请确保 AD FS 联合身份验证服务名称 (例如 sts.contoso.com) 被添加到 intranet 区域在 IE 中。 这可以通过组策略进行控制并部署到所有加入域的计算机。

## <a name="azure-ad-connect-supporting-components"></a>支持组件的 azure AD 连接
以下是 Azure AD 连接装有 Azure AD 连接的服务器安装的组件的列表。 此列表是基本的速成版安装。  如果您选择安装同步服务页上使用不同的 SQL Server，则 SQL Express LocalDB 没有安装本地。

- Azure AD 连接的运行状况
- Microsoft 在线服务登录助手 （安装但不依赖） 的 IT 专业人员
- Microsoft SQL Server 2012年命令行实用程序
- Microsoft SQL Server 2012年明示 LocalDB
- Microsoft SQL Server 2012年的本机客户端
- Microsoft Visual C++ 2013年重新分发包

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure AD 连接的硬件要求
下表显示了 Azure AD 连接同步计算机的最低要求。

| 在 Active Directory 中的对象数 | CPU | 内存 | 硬盘容量 |
| ------------------------------------- | --- | ------ | --------------- |
| 少于 10000 | 1.6 g h z | 4 GB | 70 GB |
| 10000-50000 | 1.6 g h z | 4 GB | 70 GB |
| 50000-万美元 | 1.6 g h z | 16 GB | 100 GB |
| 完整的 SQL Server 版本则需要 100000 或更多对象|  |  |  |
| 100000-300000 | 1.6 g h z | 32 GB | 300 GB |
| 300000 – 600000 | 1.6 g h z | 32 GB | 450 GB |
| 超过 600000 | 1.6 g h z | 32 GB | 500 GB |

AD FS 或 Web 应用程序服务器运行的计算机的最低要求是如下︰

- CPU︰ 双核心 1.6 g h z 或更高版本
- 内存︰ 2GB 或更高版本
- Azure VM: A2 配置或更高版本

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
