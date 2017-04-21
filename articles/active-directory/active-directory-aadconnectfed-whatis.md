<properties
    pageTitle="Azure AD 连接和联盟 |Microsoft Azure"
    description="此页是使用 Azure AD 连接的 AD FS 操作有关的所有文档的集中位置"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD 连接和联盟

Azure AD 连接允许您配置联合身份验证与内部部署 AD FS 和 Azure 的广告。 与联合身份验证登录，可以使用户能够登录到基于 Azure AD 服务其内部的密码，然后在公司网络中，而无需重新输入其密码。 AD FS 的联合身份验证选项允许您部署一个新或 Windows Server 2012 R2 服务器场中指定现有的 AD FS。

本主题是有关联合身份验证的信息的主服务器为所有与之相关的其他主题相关的 Azure AD 连接和列表链接的功能。 Azure AD 连接的链接，请参阅使用 Azure Active Directory 集成内部标识。

## <a name="azure-ad-connect---federation-topics"></a>Azure AD 连接-联合主题

| 主题 | 它涵盖了什么以及何时读取 |
|:------|:-----------|
| **Azure AD 连接用户登录选项** ||
| [了解用户登录选项](active-directory-aadconnect-user-signin.md) | 各个用户登录选项的描述和对 Azure 登录用户体验的影响 |
| **AD FS 安装使用 Azure AD 连接**||
| [必备组件](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | 通过 Azure AD 连接的 AD FS 安装成功的先决条件|
| [配置 AD FS 服务器场](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | 如何安装新的 AD FS 场使用 Azure AD 连接 |
| **修改 AD FS 配置** | |
| [修复信任](active-directory-aadconnect-federation-management.md#reparing-the-trust) | 如何修复当前信任之间内部 AD FS 和 Office 365 / Azure |
| [添加新的 AD FS 服务器](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | 扩大与其他 AD FS 服务器开机自检初始安装的 AD FS 服务器场 |
| [添加新的 AD FS WAP 服务器](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | 与其他 WAP 服务器开机自检初始安装扩展 AD FS 服务器场 |
| [添加一个新的联盟的域](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | 添加另一个域以进行联合使用 Azure 的广告 |
|**后安装任务**||
| [添加自定义公司徽标 / 图](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| 通过指定将 AD FS 在登录页显示的自定义徽标来修改登录体验 |
| [添加签入说明](active-directory-aadconnect-federation-management.md#add-sign-in-description) | 更改登录 AD FS 在登录页上的说明 | 
| [修改 AD FS 声明规则](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | 修改/添加对应于 Azure AD 连接的同步配置 AD FS 声明规则 |


## <a name="additional-resources"></a>其他资源

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
* [在 Azure 中的 AD FS 部署](active-directory-aadconnect-azure-adfs.md)
* [高可用性跨地区 AD FS Azure 使用 Azure 流量管理器中的部署](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


