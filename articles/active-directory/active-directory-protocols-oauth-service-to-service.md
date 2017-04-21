<properties
    pageTitle="Azure AD 服务到服务授权使用 OAuth2.0 |Microsoft Azure"
    description="本文介绍了如何使用 HTTP 消息来实现服务的服务身份验证使用 OAuth2.0 客户端凭据授予流。"
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

# <a name="service-to-service-calls-using-client-credentials"></a>使用客户端凭据的服务调用到服务

OAuth 2.0 客户端凭据授予流允许 web 服务 （*机密客户端*） 使用自己的凭据进行身份验证时调用另一个 web 服务，而不是模拟用户。 在这种情况下，客户端通常是中间层 web 服务、 后台程序服务或网站。

## <a name="client-credentials-grant-flow-diagram"></a>客户端凭据授予流程关系图

下图说明了如何对客户端凭据授予在 Azure 活动目录 (AD Azure) 流工作原理。

![OAuth2.0 客户端凭据授予流](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. 客户端应用程序到 Azure AD 令牌发放终结点进行身份验证，并请求访问令牌。
2. Azure AD 令牌发放终结点发出的访问令牌。
3. 访问令牌用于对受保护的资源进行身份验证。
4. 从受保护的资源的数据返回给 web 应用程序。

## <a name="register-the-services-in-azure-ad"></a>在 Azure AD 中注册这些服务

在 Azure 活动目录 (AD Azure) 注册调用服务和接收服务。 有关详细说明，请参阅[添加、 更新，以及删除应用程序](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>请求访问令牌

若要请求一个访问令牌，使用 HTTP 发送到特定于租户的 Azure AD 终结点。

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>对服务的访问令牌请求

对服务的访问令牌请求包含以下参数。

| 参数 | | 说明 |
|-----------|------|------------|
| response_type | 必填 | 指定请求的响应类型。 客户端凭据授予流中的值必须是**client_credentials**。|
| client_id | 必填 | 指定调用的 web 服务的 Azure AD 客户端 id。 若要查找调用应用程序的客户机 ID，Azure 管理门户中，单击**Active Directory**目录单击该应用程序，和，然后单击**配置**。|
| client_secret | 必填 |  输入密钥在 Azure AD 中注册调用 web 服务。 若要创建一个密钥，Azure 管理门户中，单击**Active Directory**目录单击该应用程序，和，然后单击**配置**。 |
| 资源 | 必填 | 输入接收 web 服务应用程序 ID URI。 若要在 Azure 管理门户中查找应用程序 ID URI， **Active Directory**单击目录，单击应用程序中，和，然后单击**配置**。 |

## <a name="example"></a>示例

以下 HTTP POST 请求访问标记为 https://service.contoso.com/ 的 web 服务。 `client_id`标识 web 服务请求的访问令牌。

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>对服务的访问令牌响应

成功的响应包含以下参数的 JSON OAuth 2.0 响应。

| 参数   | 说明 |
|-------------|-------------|
|access_token |请求的访问令牌中。 调用的 web 服务可以使用此标记对接收 web 服务进行身份验证。 |
|access_type  | 表示标记类型值。 Azure 的广告支持的唯一类型为**载体**。 持有者标记有关的详细信息，请参阅[OAuth 2.0 授权框架︰ 持有者标记用法 (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)。
|expires_in   | 多长时间访问令牌无效 （以秒为单位）。|
|expires_on   |当访问令牌过期时间。 该日期表示为秒数从 1970年-01-01T0:0:0Z UTC 直到过期时间。 此值用于确定缓存的令牌的生存期。 |
|资源     | 应用程序 ID 接收 web 服务的 URI。 |

## <a name="example"></a>示例

下面的示例演示对 web 服务的访问令牌的请求到成功的响应。

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>请参见

* [OAuth 2.0 Azure 做广告](active-directory-protocols-oauth-code.md)
