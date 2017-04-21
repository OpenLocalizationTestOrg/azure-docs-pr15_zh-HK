<properties
    pageTitle="SAML 协议出的 azure 单一登录 |Microsoft Azure"
    description="本文介绍了 Azure Active Directory 中的单个 Sign-Out SAML 协议"
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
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="single-sign-out-saml-protocol"></a>一个注销 SAML 协议

Azure 活动目录 (AD Azure) 支持 SAML 2.0 的 web 浏览器退出的单个配置文件。 对于单注销能够正常运行，Azure 广告必须注册其元数据 URL 期间应用程序注册。 Azure 广告从元数据中获取注销 URL 和云服务的签名密钥。 Azure AD 使用签名密钥验证传入的 LogoutRequest 上的签名，并使用 LogoutURL 被注销后将用户重定向。

如果云服务不支持元数据的终结点，应用程序中注册后，开发人员必须联系 Microsoft 技术支持提供注销 URL 和签名密钥。

此关系图显示 Azure 的广告单注销过程的工作流。

![单一登录工作流出](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

云服务发送`LogoutRequest`对 Azure AD 的消息，以指示会话已终止。 下面的摘选显示了一个示例`LogoutRequest`元素。

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

`LogoutRequest`发送到 Azure AD 的元素需要以下属性︰

- `ID`︰ 此标识的注销请求。 值`ID`不应以数字开头。 典型的做法是将**id**附加到 GUID 的字符串表示形式。

- `Version`︰ 将此元素的值设置为**2.0**。 此值是必需的。

- `IssueInstant`︰ 这是`DateTime`与协调世界时 (UTC) 值和[往返格式 ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)的字符串。 Azure 广告需要这种类型的值，但不会强制。

- `Consent`， `Destination`， `NotOnOrAfter` ，`Reason`属性将被忽略，如果将它们包括在`LogoutRequest`元素。

### <a name="issuer"></a>颁发者

`Issuer`元素在`LogoutRequest`必须完全匹配**ServicePrincipalNames**在云服务中的一个在 Azure 的广告。 通常情况下，此设置为应用程序注册过程中指定**的应用程序 ID URI** 。

### <a name="nameid"></a>NameID

值`NameID`元素必须完全匹配`NameID`被注销的用户。
## <a name="logoutresponse"></a>LogoutResponse

Azure 广告发送`LogoutResponse`在响应中`LogoutRequest`元素。 下面的摘选显示了一个示例`LogoutResponse`。

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD 组`ID`， `Version` ，`IssueInstant`中的值以`LogoutResponse`元素。 它还将设置`InResponseTo`元素的值与`ID`的特性`LogoutRequest`，吸引响应。

### <a name="issuer"></a>颁发者

Azure 的广告将此值设置为`https://login.microsoftonline.com/<TenantIdGUID>/`，<TenantIdGUID>是 Azure AD 租户的租户 ID。

值进行计算`Issuer`元素，使用**应用程序 ID URI**的值在应用程序注册期间提供。

### <a name="status"></a>状态

Azure 广告使用`StatusCode`元素在`Status`元素以指示成功或失败退出。 在注销的尝试失败，`StatusCode`元素还可以包含自定义错误消息。
