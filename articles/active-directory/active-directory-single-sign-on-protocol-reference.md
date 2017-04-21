<properties
    pageTitle="SAML 协议的 azure 单一登录 |Microsoft Azure"
    description="本文介绍了 Azure Active Directory 中的单个符号上 SAML 协议"
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

# <a name="single-sign-on-saml-protocol"></a>单一登录 SAML 协议

本文介绍了 SAML 2.0 身份验证请求和响应的 Azure 活动目录 (AD Azure) 支持单一登录。

下面的协议关系图描述单个登录序列。 云服务 （服务提供商） 使用 HTTP 重定向绑定传递`AuthnRequest`（身份验证请求） 元素对 Azure AD （标识提供程序）。 然后 azure 广告使用绑定来发送 HTTP post`Response`到云服务的元素。

![单一登录工作流](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

若要请求身份验证的用户，云服务发送`AuthnRequest`元素到 Azure 的广告。 一个示例 SAML 2.0`AuthnRequest`可能如下所示︰

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 标识 | 必填 | Azure 广告使用此特性来填充`InResponseTo`返回的响应特性。 ID 必须不以数字开头，因此常见的策略是在前面添加类似"id"为 GUID 的字符串表示形式的字符串。 例如，`id6c1c178c166d486687be4aaf5e482730`是一个有效的 id。 |
| 版本 | 必填 | 这应该是**2.0**。|
| IssueInstant | 必填 | 这是与 UTC 值和[往返格式 ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)的日期时间字符串。 Azure 广告需要这种类型，日期/时间值，但不是计算或者使用值。 |
| AssertionConsumerServiceUrl | 可选 | 如果提供，则它必须匹配`RedirectUri`Azure 广告中的云服务。 |
| ForceAuthn | 可选 | 如果提供，这应该是假的。 任何其他值将导致错误。|
| IsPassive | 可选 | 如果提供，这应该是假的。 任何其他值将导致错误。 |  

所有其他`AuthnRequest`特性，如同意、 目标、 AssertionConsumerServiceIndex、 AttributeConsumerServiceIndex 和提供程序名称也**被忽略**。

Azure 的广告还会忽略`Conditions`元素在`AuthnRequest`。

### <a name="issuer"></a>颁发者

`Issuer`元素在`AuthnRequest`必须完全匹配**ServicePrincipalNames**在云服务中的一个在 Azure 的广告。 通常情况下，此设置为应用程序注册过程中指定**的应用程序 ID URI** 。

包含的示例 SAML 摘录`Issuer`元素如下所示︰

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

此元素请求响应中的特定名称 ID 格式和中是可选的`AuthnRequest`发送到 Azure AD 的元素。

一个示例`NameIdPolicy`元素如下所示︰

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

如果`NameIDPolicy`提供，可包括其可选`Format`属性。 `Format`属性可以具有下列值; 之一任何其他值将导致错误。

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory 发出 NameID 索赔作为成对的标识符。
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory 发出的电子邮件地址格式的 NameID 声明。
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`︰ 此值允许 Azure Active Directory 选择声明格式。 Azure 的 Active Directory 发出 NameID 成对的标识符。

不包括`SPNameQualifer`属性。 Azure AD 忽略`AllowCreate`属性。

### <a name="requestauthncontext"></a>RequestAuthnContext

`RequestedAuthnContext`元素指定所需的身份验证方法。 它是可选的`AuthnRequest`发送到 Azure AD 的元素。 Azure 的广告支持只有一个`AuthnContextClassRef`值︰ `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`。

### <a name="scoping"></a>作用域

`Scoping`元素，它包括标识提供程序的列表，则可选中`AuthnRequest`发送到 Azure AD 的元素。

如果提供，则不包括`ProxyCount`属性，`IDPListOption`或`RequesterID`元素，因为它们不受支持。

### <a name="signature"></a>签名

不包括`Signature`元素在`AuthnRequest`的元素，因为 Azure 广告不支持签名的身份验证请求。

### <a name="subject"></a>主题

Azure AD 忽略`Subject`元素的`AuthnRequest`元素。

## <a name="response"></a>响应

请求登录成功完成时，Azure 的广告发到云服务的响应。 成功登录尝试的示例响应如下所示︰

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>响应

`Response`元素包含授权请求的结果。 Azure AD 组`ID`， `Version` ，`IssueInstant`中的值以`Response`元素。 它还将设置以下属性︰

- `Destination`︰ 在登录成功完成时，此选项设置为`RedirectUri`的服务提供商 （云服务）。
- `InResponseTo`︰ 此选项设置为`ID`的特性`AuthnRequest`启动响应元素。

### <a name="issuer"></a>颁发者

Azure AD 组`Issuer`元素到`https://login.microsoftonline.com/<TenantIDGUID>/`，<TenantIDGUID>是 Azure AD 租户的租户 ID。

例如，与颁发者元素的示例响应可能如下所示︰

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>状态

`Status`元素所传达的成功或失败的登录。 它包括`StatusCode`元素，其中包含代码或一组嵌套代码表示请求的状态。 它还包括`StatusMessage`元素，其中包含在登录过程中生成的自定义错误消息。

<!-- TODO: Add a authentication protocol error reference -->

以下是不成功的登录尝试的 SAML 响应。

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>断言

除了`ID`， `IssueInstant` ， `Version`，Azure 广告设置中的以下元素`Assertion`响应中的元素。

#### <a name="issuer"></a>颁发者

此选项设置为`https://sts.windows.net/<TenantIDGUID>/`，<TenantIDGUID>是 Azure AD 租户的租户 ID。

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>签名

Azure 广告签名响应成功登录进行断言。 `Signature`元素中包含的数字签名来云服务可以使用身份验证源验证断言的完整性。

若要生成此数字签名，Azure 广告使用中的签名密钥`IDPSSODescriptor`其元数据文档中的元素。

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>主题

此参数指定是在断言语句的主题的主体。 它包含`NameID`元素，它表示已通过身份验证的用户。 `NameID`值为仅指向标记的受众对象是服务提供商的目标的标识符。 它是持久-它可以撤销，但永远不会重新分配。 也是不透明的在于它不会泄露任何有关用户并不能用作标识符的属性查询。

`Method`的特性`SubjectConfirmation`元素始终设置为`urn:oasis:names:tc:SAML:2.0:cm:bearer`。

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>条件

此元素指定条件，用于定义可接受使用 SAML 断言。

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` ，`NotOnOrAfter`属性指定的时间间隔，其间断言是否有效。

- 值`NotBefore`属性是否相等，或略有 （少于一秒） 的值比晚`IssueInstant`特性`Assertion`元素。 Azure 广告不考虑任何本身和云服务 （服务提供商） 之间的时差，到这次不会添加任何缓冲区。
- 值`NotOnOrAfter`特性是以后的 70 分钟`NotBefore`属性。

#### <a name="audience"></a>访问群体

此属性包含标识目标的受众的 URI。 Azure 的广告将此元素的值设置为的值`Issuer`元素的`AuthnRequest`号上的启动。 要计算`Audience`值，请使用值为`App ID URI`的应用程序注册期间指定。

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

像`Issuer`值，`Audience`值必须严格符合 Azure AD 中代表云服务的服务主体名称之一。 但是，如果值的`Issuer`元素不是一个 URI 的值，`Audience`在响应中的值是`Issuer`值前缀为`spn:`。

#### <a name="attributestatement"></a>AttributeStatement

此属性包含有关的主题或用户的声明。 下面的摘录部分包含一个示例`AttributeStatement`元素。 省略号表示元素可以包含多个属性和属性值。

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **名称声明**︰ 的值`Name`属性 (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) 如是的经过身份验证的用户的用户主体名称`testuser@managedtenant.com`。
- **ObjectIdentifier 声明**︰ 的值`ObjectIdentifier`属性 (`http://schemas.microsoft.com/identity/claims/objectidentifier`) 是`ObjectId`表示在 Azure AD 身份验证的用户的目录对象。 `ObjectId`是不可变的全局唯一的并重新使用的身份验证的用户的安全标识符。

#### <a name="authnstatement"></a>AuthnStatement

此元素断言在特定时间特定方式断言主题已进行身份验证。

- `AuthnInstant`属性指定与 Azure AD 用户身份验证的时间。
- `AuthnContext`元素指定用于对用户进行身份验证的身份验证上下文。

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
