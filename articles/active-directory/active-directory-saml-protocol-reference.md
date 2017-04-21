<properties
    pageTitle="Azure AD SAML 协议引用 |Microsoft Azure"
    description="本文概述了 Azure Active Directory 中的单一登录和单个 Sign-Out SAML 配置文件。"
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Azure Active Directory 如何使用 SAML 协议

Azure 活动目录 (AD Azure) 使用 SAML 2.0 协议使应用程序能够为用户提供单一登录体验。 Azure AD 的[单一登录](active-directory-single-sign-on-protocol-reference.md)和[注销的单](active-directory-single-sign-out-protocol-reference.md)SAML 配置文件说明如何在身份提供程序服务中使用 SAML 断言、 协议和绑定。

SAML 协议要求身份标识提供程序 (Azure AD) 和服务提供程序 （应用程序） 来交换关于自己的信息。

当应用程序注册 Azure 的广告时，应用程序开发人员注册 Azure 广告联盟相关的信息。 这包括**重定向 URI**和应用程序的**元数据的 URI** 。

Azure AD 使用云服务的**元数据 URI**来检索签名密钥和注销的云服务的 URI。 如果应用程序不支持元数据 URI，开发人员必须联系 Microsoft 技术支持提供注销 URI 和签名密钥。

Azure 的 Active Directory 公开租户特定和通用 （租户无关） 单一登录和单注销终结点。 这些 Url 表示可寻址的位置--它们不是只是标识符-，这样就可以为要读取的元数据的终结点。

 - 特定于租户的终点是位于`https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`。  <TenantDomainName>占位符表示一个已注册的域名或 TenantID Azure AD 租户 GUID。 例如，contoso.com 租户的联合元数据是在︰ https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- 独立于租户的终点是位于`https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`。在此终结点地址，**常见**出现，而不是租户域的名称或 id。

Azure 广告发布的联合元数据文档有关的信息，请参阅[联合元数据](active-directory-federation-metadata.md)。
