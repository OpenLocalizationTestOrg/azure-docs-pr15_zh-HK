<properties
    pageTitle="Azure 广告联盟兼容性列表"
    description="此页面包含可用于实现单一登录的非 Microsoft 身份提供程序。"
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
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure 广告联盟兼容性列表
Azure 的 Active Directory 提供了单一登录，而不需要任何非 Microsoft 解决方案增强 Office 365 和混合和仅云实现其他 Microsoft 联机服务的应用程序访问安全。 Office 365，与微软的在线服务，大多数集成与 Azure Active Directory 目录服务、 身份验证和授权。 Azure 的 Active Directory 还提供了对数千种 SaaS 应用程序的单一登录和内部部署 web 应用程序。 请参阅支持的 SaaS 应用程序的 Azure Active Directory 应用程序库。

对于非 Microsoft 联合身份验证解决方案已投资开发的组织，本主题包含用于通过使用非 Microsoft 标识提供程序从"Azure Active Directory 联合身份验证兼容性列表"下面用 Microsoft 联机服务配置单一登录的 Windows 服务器的 Active Directory 用户指南。 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford 的计算机组](http://oxfordcomputergroup.com/)，第三方，代表 Microsoft 测试非 Microsoft 身份提供程序，对照一套通用的用例中使用 Azure Active Directory 这些单个登录体验。

有关如何获取您在此处列出的第三方身份标识提供程序的信息，请联系 Oxford 计算机组在[idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com)。

>[AZURE.IMPORTANT] Oxford 计算机组测试这些单个登录方案的联合身份验证功能。 Oxford 计算机组没有执行任何测试的同步，双因素身份验证，这些单个登录方案等组件。

>登录到 UPN 的备选 ID 的使用还没有在此程序中的测试。



- [Azure 的活动目录](#azure-active-directory)
- [最佳的 IDM 的虚拟身份服务器联合身份验证服务](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli 联合标识管理器 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [CFS 3.0 RadiantOne](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ 访问管理器 4.0.1 版](#netiq-access-manager-401) 
- [与 x-11.6 x 访问策略管理器 BIG-IP ver.11.3 BIG-IP](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware 工作区门户 2.1 版](#vmware-workspace-portal-version-21) 
- [签名和转 5.3](#signampgo-53) 
- [3.0 版 IceWall 联盟](#icewall-federation-version-30) 
- [CA 安全群](#ca-secure-cloud) 
- [戴尔一标识云访问管理器 v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil 单 4.5 登录](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] 由于这些都是第三方产品，Microsoft 不提供有关部署、 配置、 故障排除、 最佳做法、 等问题和疑问这些标识提供程序支持。 有关支持和与这些标识提供程序有关的问题，请直接联系支持第三方。

>这些第三方身份提供程序的互操作性测试与 Microsoft 云服务使用 WS 联合身份验证和 WS 信任的协议。 测试中未包括使用 SAML 协议。

## <a name="azure-active-directory"></a>Azure 的活动目录 
Azure 的 Active Directory 可以验证用户的身份通过联盟内部活动的目录使用或不使用密码同步本地联合服务器。 

下面是此登录体验的方案支持列表︰ 


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|
|现代应用程序使用 ADAL 如 Office 2016| 支持|无|

Azure Active Directory 使用 AD FS 的详细信息，请参阅[活动目录联合身份验证服务 (ADF)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

有关使用 Azure Active Directory 与密码同步的详细信息请参阅[Azure AD 连接](active-directory-aadconnect.md)。


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>最佳的 IDM 的虚拟身份服务器联合身份验证服务 
最佳 IDM 虚拟标识服务器联合身份验证服务可以驻留在客户的内部活动目录的用户进行身份验证。

以下是该方案支持矩阵这单一的登录体验︰


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |为客户端访问的详细信息策略请参阅[将访问权限限制到 Office 365 服务基于客户端的位置上。](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 实现广泛使用的 WS 联合身份验证标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无 （早期版本必须升级到 6.11|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关如何配置此 PingFederate 说明 STS 为 Active Directory 用户，提供单一登录即可下载 pdf[这里。](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

PingFederate 说明如何配置此 STS 为 Active Directory 用户提供单一登录体验，请参阅[这里。](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

PingFederate 说明如何配置此 STS 为 Active Directory 用户提供单一登录体验，请参阅[这里。](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify 帮助内部联合身份验证服务器所在的要求没有 Office 365 提供联盟单一登录体验。

下面是此单个登录体验的方案支持列表︰


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |不支持客户端访问控制 

有关 Centrify 的详细信息，请参阅[这里。](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli 联合标识管理器 6.2.2 
IBM Tivoli 联合身份管理器 6.2.2 用 IBM 安全访问管理器为 Microsoft 应用程序 1.4 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

关于 IBM Tivoli 联合身份管理器的详细信息，请参阅[IBM 的 Microsoft 应用程序的安全访问管理器。](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单点登录体验和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关 SecureAuth 的详细信息，请参阅[SecureAuth IdP](http://go.microsoft.com/?linkid=9845293)。

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 累计发行 4
CA SiteMinder 联盟 12.52 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关 CA SiteMinder 的详细信息，请参阅[CA SiteMinder 联盟。](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>CFS 3.0 RadiantOne 
RadiantOne 云联合身份验证服务 (CFS) 3.0 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关 RadiantOne CFS 的详细信息，请参阅[RadiantOne CFS。](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 


| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |集成的 Windows 身份验证要求其他 web 服务器和 Okta 应用程序的安装的程序。|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

关于 Okta 的详细信息，请参阅[Okta。](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
随着测试在五月 2014 OneLogin 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |集成的 Windows 身份验证|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关 OneLogin 的详细信息，请参阅[OneLogin。](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ 访问管理器 4.0.1 版 
NetIQ 访问管理器 4.0.1 版实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |* Kerberos 合同支持|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |不支持集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

* NetIQ 支持通过配置 Kerberos 合同的 Kerberos 身份验证。  这种配置的帮助信息，请联系 NetIQ 或查看安装指南。 关于 NetIQ 访问管理器的详细信息，请参阅[NetIQ 访问管理器。](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP 与访问策略管理器 BIG-IP ver. x – 11.6 x 11.3 
BIG-IP 使用访问策略管理器，(APM) BIG-IP ver. 11.3 x – 11.6 x 实现广泛使用的 SAML 标识标准提供单点登录体验和属性交换框架。

下面是此单个登录体验的方案支持列表︰ 

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  不受支持 |不受支持|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

关于 BIG-IP 访问策略管理器的详细信息，请参阅[BIG-IP 访问策略管理器。](https://f5.com/products/modules/access-policy-manager) 

有关如何配置此 BIG-IP 访问策略管理器说明 STS 为活动目录用户，提供单一登录体验下载 pdf[这里。](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware 工作区门户 2.1 版 
VMware 工作区门户 2.1 版实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |不支持集成的 Windows 身份验证|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 | 支持 |不支持集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

关于 VMware 工作区门户 2.1 版的详细信息，下载 pdf[这里。](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>签名和转 5.3 
签名和转 5.3 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |支持 Kerberos 协定 |
| Lync、 办公室订阅、 CRM 的富客户端应用程序 | 支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|


符号和转到 5.3 支持通过配置 Kerberos 合同的 Kerberos 身份验证。  有关此配置的帮助信息，请联系 Ilex 或查看安装指南[这里。](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>3.0 版 IceWall 联盟 
3.0 版的 IceWall 联盟实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |不支持集成的 Windows 身份验证|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 | 支持 |不支持集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

IceWall 联合身份验证的更多信息，请参见[此处](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/)和[这里。](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA 安全群 

CA 安全云实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |不支持集成的 Windows 身份验证|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 | 支持 |不支持集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

CA 安全云有关的详细信息，请参阅[CA 安全云。](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>戴尔一标识云访问管理器 v7.1 
戴尔一标识云访问管理器实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |无|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 |  支持 |无|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|

有关戴尔一个标识云访问管理器的详细信息，请参阅[戴尔一个标识云访问管理器。](http://software.dell.com/products/cloud-access-manager)

 有关如何配置此 STS 提供单一登录体验给 Office 365 用户说明，请参阅[配置 Office 365 用户。](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil 单 4.5 登录 
AuthAnvil 一个符号上 4.5 实现广泛使用的 WS 联合身份验证/WS 信任标识标准提供单一登录和属性交换框架。

下面是此单个登录体验的方案支持列表︰

| 客户端 |支持  |例外情况|
| --------- | --------- |--------- |
| 如 Exchange Web 访问和 SharePoint Online 的基于 web 的客户端 | 支持 |不支持集成的 Windows 身份验证|
| Lync、 办公室订阅、 CRM 的富客户端应用程序 | 支持 |不支持集成的 Windows 身份验证|
| 如 Outlook 和动态同步电子邮件丰富客户端 |  支持 |无|


有关详细信息，请参阅[AuthAnvil 单一登录。](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
