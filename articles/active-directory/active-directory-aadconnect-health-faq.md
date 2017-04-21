<properties
    pageTitle="Azure AD 连接健康常见问题解答"
    description="本常见问题解答回答有关 Azure AD 连接的健康问题。 此常见问题解答解释有关使用该服务，包括计费模型、 功能、 限制和支持问题。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD 连接的常见问题解答 (FAQ) 的运行状况

本常见问题解答回答有关 Azure AD 连接的健康问题。 此常见问题解答解释有关使用该服务，包括计费模型、 功能、 限制和支持问题。

## <a name="general-questions"></a>一般性问题



**问︰ 管理多个 Azure 的广告目录。如何切换到使用 Azure 活动目录津贴？**

您可以切换不同 Azure AD 承租人在右上角选择当前签名中的用户名，然后选择相应的帐户。 如果此处未列出的帐户，选择注销，然后使用全局管理员凭据已启用登录 Azure 活动目录特优的目录。

## <a name="installation-questions"></a>安装问题



**问︰ 什么是在单个服务器上安装 Azure AD 连接健康代理的影响？**

安装 Microsoft Azure AD 连接健康代理 ADFS，Web 应用程序代理服务器，Azure AD 连接 (sycn) 服务器，域控制器的影响是最小相对于 CPU，内存消耗网络带宽和存储。

下面的数字都是近似值。

- CPU 消耗︰ 增加约 1%
- 内存消耗︰ 达 10%的总系统内存

>[AZURE.NOTE] 在无法传达给 Azure 的代理，代理到定义的最大限制存储的数据本地。 代理将覆盖在"最近最少提供服务"的基础上"缓存"的数据。

- Azure AD 连接健康代理的本地缓冲区存储︰ 大约 20 MB
- 对于 AD FS 服务器，建议您配置 AD FS 审核通道的 Azure AD 连接健康代理来处理所有审核数据，则将覆盖它之前的 1024 MB (1 GB) 的磁盘空间。

**问︰ 将必须在 Azure AD 连接健康代理在安装期间重新启动我的服务器？**

不。 所代理的安装不需要您重新启动服务器。 但是，某些先决条件步骤的安装可能需要重新启动服务器。

例如，在 Windows Server 2008 R2 安装.Net 4.5 框架需要重新启动服务器。


**问︰ 没有 Azure AD 连接卫生服务工作通过直通的 http 代理服务器？**

是的。  对于继续操作，您可以配置健康代理转发出站 http 请求使用 HTTP 代理服务器。 有关详细信息，请参阅[配置 Azure AD 连接健康代理程序以使用 HTTP 代理。](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
如果您需要配置一个代理，代理注册时，您可能需要事先修改 Internet Explorer 代理服务器设置。
1. 打开 Internet Explorer-> 设置-> Internet 选项-> 连接局域网设置->。
2. 选择为 LAN 使用代理服务器。
3. 如果您的 HTTP 和 HTTPS/安全有不同的代理服务器的端口，请选择高级。

**连接到 Http 代理时，问︰ Azure AD 连接的运行状况服务支持基本身份验证？**

不。 目前不支持指定任意用户名/密码进行基本身份验证的机制。


**问︰ 什么版本的 AD DS 支持 AD DS 的 Azure AD 连接的健康？**

而安装在以下操作系统版本支持 AD DS 的监视︰

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>操作问题



**问︰ 我是否需要在 AD FS 应用程序代理服务器或 Web 应用程序代理服务器上启用审核？**

否，审核不需要 Web 应用程序代理 (WAP) 服务器上启用。 在 AD FS 服务器上启用它。


**问︰ 如何做 Azure AD 连接的运行状况警报获得解决？**

成功条件获得解决 azure AD 连接的运行状况警报。 Azure AD 连接健康代理检测，并定期向该服务报告的成功条件。 对于几个警报，抑制是基于时间的。 换句话说，如果在 72 小时内从警报生成内未看到相同的错误条件，警报自动解决。




**问︰ 需要哪种防火墙端口打开 Azure AD 连接健康代理工作？**

您需要具有 TCP/UDP 端口 443 和 5671 打开 Azure AD 连接健康代理能够与 Azure 广告健康服务终结点进行通信。


**问︰ 为什么看到两个具有相同名称的 Azure AD 连接健康门户服务器？**

从服务器中删除代理时，服务器是不会自动删除从 Azure AD 连接门户。  如果您手动从服务器上删除代理或删除服务器本身，您需要从 Azure AD 连接健康门户中手动删除服务器条目。 有关详细信息，请转到[删除服务器或服务的实例。](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

如果映像服务器或创建新的服务器具有相同的详细信息 （例如计算机名称） 并没有从 Azure AD 连接健康门户删除已注册的服务器，安装代理程序在新服务器上，您可能会看到两个条目具有相同的名称。  
在这种情况下，您应删除手动属于旧服务器的项。 此服务器的数据应该是过时的。

## <a name="related-links"></a>相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康代理安装](active-directory-aadconnect-health-agent-install.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [对于同步使用 Azure AD 连接运行状况](active-directory-aadconnect-health-sync.md)
* [与 AD DS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adds.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
