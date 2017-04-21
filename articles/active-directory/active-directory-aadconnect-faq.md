<properties
    pageTitle="Azure AD 连接︰ 常见问题 |Microsoft Azure"
    description="此页面包含有关常见问题 Azure AD 连接。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD 连接的常见问题解答

## <a name="general-installation"></a>一般的安装
**问︰ 将启用 2FA Azure 的 AD 全局管理员是否进行安装？**  
从 2 月 2016年的生成，与支持此功能。

**问︰ 有什么办法来安装无人值守的 Azure AD 连接吗？**  
它仅支持安装 Azure AD 连接时使用安装向导。 无人参与和无提示安装不受支持。

**问︰ 我有一个无法连接到一个域的目录林。如何安装 Azure AD 连接？**  
从 2 月 2016年的生成，与支持此功能。

**问︰ 服务器核心 AD DS 健康代理处理？**  
是的。 后安装代理，您可以完成使用以下 PowerShell commandlet 的注册过程︰ 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>网络
**问︰ 我有一个防火墙的网络设备或限制最长时间连接的其他内容可以保持我的网络上打开。多长时间我客户端超时阈值时应使用 Azure AD 连接？**  
所有网络软件、 物理设备或任何其他限制连接可以保持打开状态的最长时间的操作应使用至少 5 分钟 （300 秒） 的值，该值作为 Azure Active Directory 和 Azure AD 连接客户端的安装位置的服务器之间的连接。 这也适用于所有以前发布的 Microsoft 标识同步工具。

**问︰ 是否支持 SLDs （单个标签域）？**  
否，Azure AD 连接不支持内部林/域使用 SLDs。

**问︰"点"NetBios 名为受支持？**  
否，Azure AD 连接不支持内部林/域的 NetBios 名称包含一个句点的位置"。"在名称中。

## <a name="federation"></a>联合身份验证
**问︰ 如果我收到一封电子邮件，要求我继续我的 Office 365 证书，做什么**  
使用中如何续订证书[续订证书](active-directory-aadconnect-o365-certs.md)主题概括的指南。

**问︰ 我有"自动更新信赖方"设置为 O365 信赖方。是否有采取任何措施，当我令牌签名证书自动滑过？**  
使用[续订证书](active-directory-aadconnect-o365-certs.md)文章中概述的指南。

## <a name="environment"></a>环境
**问︰ 它支持重命名服务器安装了 Azure AD 连接后是吗？**  
不。 更改服务器名称将导致同步引擎无法连接到 SQL 数据库，该服务将无法启动。

## <a name="identity-data"></a>标识数据
**问︰ Azure 广告中的 （范围内） UPN 属性不匹配上 prem UPN-为什么？**  
请参阅以下文章︰

- [在 Office 365，Azure 或 Intune 用户名不匹配的本地 UPN 或替代登录 ID](https://support.microsoft.com/en-us/kb/2523192)
- [更改用户帐户使用不同的联盟的域的 UPN 后更改不同步完成 Azure 活动目录同步工具](https://support.microsoft.com/en-us/kb/2669550)

您还可以配置 Azure 广告允许同步引擎更新[Azure AD 连接的同步服务功能](active-directory-aadconnectsyncservice-features.md)中所述的范围。

## <a name="custom-configuration"></a>自定义配置
**问︰ 哪里记载 PowerShell cmdlet 的 Azure AD 连接？**  
在此网站上记载的 cmdlet，除了在 Azure AD 连接中找到其他 PowerShell cmdlet 不支持供客户使用。

**问︰ 可以使用在中找到时"服务器导出/导入"*同步服务管理器*servers? * * 之间移动配置  
不。 此选项不会检索所有配置设置，不应使用。 而是应使用向导的第二个服务器上创建基本配置并使用同步规则编辑器生成 PowerShell 脚本服务器之间移动任何自定义规则。 请参阅[移动从活动到临时服务器的自定义配置](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server)。

## <a name="troubleshooting"></a>故障排除
**问︰ 如何获取 Azure AD 连接的帮助？**

[搜索 Microsoft 知识库 (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- 搜索 Microsoft 知识库 (KB) 的技术解决方案常见的中断修复问题，Azure AD 连接的支持。

[Microsoft Azure Active Directory 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- 您可以搜索和浏览的技术问题并回答来自社区或通过单击提出自己的问题[这里](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)。

[Azure AD 连接客户支持](https://manage.windowsazure.com/?getsupport=true)

- 使用此链接以获得支持通过 Azure 的门户。
