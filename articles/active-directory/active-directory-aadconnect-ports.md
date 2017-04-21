<properties
    pageTitle="Azure AD 连接︰ 端口 |Microsoft Azure"
    description="此页是 Azure AD 连接打开所需端口的技术参考页"
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
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>混合的身份所需的端口和协议
以下文档是一个技术的引用，以提供所需的端口和协议的实施混合身份解决方案所需的信息。 使用下图，请参阅相应的表。

![什么是 Azure AD 连接](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>表 1-Azure AD 连接和本地广告
下表描述的端口和协议所需的 Azure AD 连接服务器之间的通信和内部广告。

协议 | 端口 | 说明
--------- | --------- |---------
DNS|53 (TCP/UDP)| 在目标目录林中的 DNS 查找。
Kerberos|88 (TCP/UDP)| 对 AD 林中的 Kerberos 身份验证。
MS RPC |135 (TCP/UDP)| 将它绑定到 AD 林时，请使用 Azure AD 连接向导的初始配置过程。
LDAP|389 (TCP/UDP)| 用于从广告的数据导入。 使用 Kerberos 登录和封条加密数据。
LDAP/SSL|636 (TCP/UDP)| 用于从广告的数据导入。 数据传输是经过签名和加密。 只有当您使用 SSL 使用。
RPC |49152-65535 (随机高 RPC Port)(TCP/UDP)| 将它绑定到 AD 林时，请使用 Azure AD 连接的初始配置过程。 有关详细信息，请参见[KB929851](https://support.microsoft.com/kb/929851)、 [KB832017](https://support.microsoft.com/kb/832017)和[KB224196](https://support.microsoft.com/kb/224196) 。

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>表 2-Azure AD 连接和 Azure 的广告
下表描述的端口和协议所需的 Azure AD 连接服务器和 Azure 的广告之间的通信。

协议 |端口 |说明
--------- | --------- |---------
HTTP|80 (TCP/UDP)| 用于下载 Crl （证书吊销列表） 来验证 SSL 证书。
HTTPS|443(TCP/UDP)| 用于同步使用 Azure 的广告。

为列表中的 Url 和 IP 地址，则需要在防火墙中打开，请参阅[Office 365 的 Url 和 IP 地址范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)。

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>表 3-Azure AD 连接和联合身份验证服务器/WAP
下表描述的端口和协议所需的 Azure AD 连接服务器和联盟/WAP 服务器之间的通信。  

协议 |端口 |说明
--------- | --------- |---------
HTTP|80 (TCP/UDP)| 用于下载 Crl （证书吊销列表） 来验证 SSL 证书。
HTTPS|443(TCP/UDP)| 用于同步使用 Azure 的广告。
WinRM|5985| WinRM 侦听器

## <a name="table-4---wap-and-federation-servers"></a>表 4-WAP 和联合身份验证服务器
下表描述的端口和协议所需的联合服务器和 WAP 服务器之间的通信。

协议 |端口 |说明
--------- | --------- |---------
HTTPS|443(TCP/UDP)| 用于身份验证。

## <a name="table-5---wap-and-users"></a>表 5-WAP 和用户
下表描述的端口和协议所需的用户和 WAP 服务器之间的通信。

协议 |端口 |说明
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| 用于设备的身份验证。
TCP|49443 (TCP)| 用于身份验证的证书。

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>表 6a 和 6b 的 Azure AD 连接健康代理为 （AD FS/同步） 和 Azure 的广告
下表描述了终结点、 端口和协议所需的 Azure AD 连接健康代理和 Azure 的广告之间的通信

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>表 6a 的端口和协议的 Azure AD 连接健康代理为 （AD FS/同步） 和 Azure 的广告
下表描述下列出站端口和协议所需的 Azure AD 连接健康代理和 Azure 的广告之间的通信。  

协议 |端口  |说明
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| 出站
Azure 服务总线|5671 (TCP/UDP)| 出站

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6b 的 Azure AD 连接健康代理为 （AD FS/同步） 的终结点和 Azure 的广告
终结点的列表，请参阅[Azure AD 连接健康代理的要求部分](active-directory-aadconnect-health-agent-install.md#requirements)。
