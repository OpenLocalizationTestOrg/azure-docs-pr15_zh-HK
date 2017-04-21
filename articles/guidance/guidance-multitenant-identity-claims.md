<properties
   pageTitle="在多租户应用程序基于声明的身份处理 |Microsoft Azure"
   description="如何使用声称颁发者验证和授权"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>使用基于声明的身份在多租户应用程序

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

本文是[一系列的一部分]。 此外，还有伴随这一系列完整[的示例应用程序]。

## <a name="claims-in-azure-ad"></a>在 Azure 广告的索赔

当用户登录时，Azure 广告发送一个 ID 令牌，其中包含一组与用户有关的声明。 声明是只是一条信息，表示为键/值对。 例如， `email` = `bob@contoso.com`。  声明了某个颁发者&mdash;在此情况下，Azure AD&mdash;是对用户进行身份验证并创建索赔的实体。 因为您信任颁发者确信索赔。 （相反，如果您不信任该颁发者，不相信这些声明 ！）

在高级别︰

1.  验证用户的身份。
2.  IDP 发送一组声明。
3.  应用程序规范化或补充索赔 （可选）。
4.  该应用程序使用声明才能做出授权决定。

在连接 OpenID，索赔就这套被控制身份验证请求的[范围参数]。 但是，Azure 广告发出一组有限的索赔通过 OpenID 连接;请参阅[支持令牌和声明类型]。 如果您希望用户的详细信息，您需要使用 Azure 广告图形 API。

以下是一些从一个应用程序可能通常关心的 AAD 的声明︰

在 ID 标记中声明类型 |    说明
-----------------------|--------------
aud | 谁发出的令牌。 这将是该应用程序的客户端 id。 通常情况下，不需要担心这一点，因为中间件自动对它进行验证。 示例︰`"91464657-d17a-4327-91f3-2ed99386406f"`
组   | AAD 用户是其成员的组的列表。 示例︰`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
iss  | OIDC 令牌[颁发者]。 示例︰`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
名称    | 用户的显示名称。 示例︰`"Alice A."`
oid | 在 AAD 用户的对象标识符。 此值是用户的不可变的和非可重复使用的标识符。 使用此值，没有电子邮件，作为唯一标识符的用户;可以更改电子邮件地址。 如果您的应用程序中使用 Azure 广告图形 API，对象 ID 是用于查询配置文件信息的值。 示例︰`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
角色   | 应用程序的用户的角色列表。 示例︰`["SurveyCreator"]`
tid | 租户 id。 此值是 Azure AD 中的组织的唯一标识符。 示例︰`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | 用户人工可读的显示名称。 示例︰`"alice@contoso.com"`
upn | 用户主要名称。 示例︰`"alice@contoso.com"`

下表列出声明类型中的 ID 标记的显示。 在 ASP.NET 核心 1.0，OpenID 连接中间件将转换某些声明类型填充用户主体的声明集合︰

-   oid >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   tid >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   upn >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>声明转换

在身份验证流程中，过程可能需要修改您接到 IDP 的声明。 在 ASP.NET 核心 1.0 中，您可以执行在**AuthenticationValidated**事件声明转换从 OpenID 连接中间件。 （请参见[身份验证事件]）。

在会话的身份验证 cookie 存储在**AuthenticationValidated**过程中添加任何索赔。 他们不被推回到 Azure 的广告。

下面是一些声明转换的示例︰

-   **声明规范化**或跨用户一致提出索赔。 这是特别相关，如果您从多个 IDPs，可能会使用不同的声明类型相似的信息得到索赔。
例如，Azure 广告发送"upn"的一项声明，其中包含用户的电子邮件。 其他 IDPs 可能发送"电子邮件"索赔。 下面的代码将转换的"电子邮件"声明"upn"声明︰

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- 添加**默认声明值**并不存在的索赔&mdash;为例，将为用户分配默认角色。 在某些情况下这可以简化授权逻辑。
- 添加具有特定于应用程序的用户信息的**自定义声明类型**。 例如，可在数据库中存储有关用户的某些信息。 您可以添加此信息的自定义声明身份验证票证。 报销申请存储在 cookie 中，因此您只需要获得每次登录会话的数据库。 另一方面，也要避免创建过大的 cookie，因此您需要考虑 cookie 大小和数据库查找之间的平衡。   

完成身份验证流程后，声明是在`HttpContext.User`。 此时，您应该将它们视为只读集合&mdash;如使用它们来做出授权决定。

## <a name="issuer-validation"></a>颁发者验证
在连接 OpenID，颁发者声明 ("iss") 标识 IDP 颁发的 ID 标记。 OIDC 身份验证流的一部分是验证颁发者声明匹配实际的颁发者。 OIDC 中间件为您处理此操作。

在 Azure 广告，颁发者值都是唯一每个 AD 租户 (`https://sts.windows.net/<tenantID>`)。 因此，应用程序应该做其他检查，以确保该颁发者代表承租人有权登录到该应用程序。

单租户应用程序，就可以检查颁发者是您自己的组织。 事实上，OIDC 中间件自动执行此操作的默认值。 在多租户应用程序中，您需要允许多个颁发者，对应于不同的承租人。 下面是使用常规方法︰

-   在 OIDC 中间件选项中，设置为 false 的**ValidateIssuer** 。 这将关闭自动检查。
-   当租户注册时，存储在用户数据库租户和颁发者。
-   每当用户登录，查看数据库中的颁发者。 如果没有找到该颁发者，则意味着该租户没有注册。 您可以将他们重定向到注册页面。
-  您还可以黑名单某些承租人;例如，对于没有支付其订阅的客户。

有关更详细的讨论，请参阅[签约租户多租户应用程序中的服务和][signup]。

## <a name="using-claims-for-authorization"></a>使用授权的声明

申请报销，用户的身份不再是一个单一的实体。 例如，用户可能具有电子邮件地址、 电话号码、 生日、 性别等。也许用户的 IDP 存储所有这些信息。 但时对用户进行身份验证，您通常将获得索赔作为其中的一个子集。 在此模型中，用户的身份是简单地捆绑包的声明。 在进行有关用户的授权决定后，您将查找某一组索赔。 换句话说，问题"可以用户 X 执行动作 Y"最终成为"没有的用户 X 有归属 Z"。

以下是一些基本模式检查索赔。

-  若要检查用户具有特定索赔的特定值︰

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    此代码检查用户是否具有值"管理员"角色索赔。 它正确地处理任何角色声明或多个角色要求用户在其上的情况。

    **ClaimTypes**类定义常用的声明类型的常量。 但是，可以为声明类型使用任何字符串值。

-   预期最那里一个值时，声明类型，获取单个值︰
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   获取声明类型的所有值︰

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

有关详细信息，请参阅[在多租户应用程序的基于角色和基于资源的授权][authorization]。

## <a name="next-steps"></a>下一步行动

- 阅读本系列的下一篇文章︰[签约租户多租户应用程序中的服务和][signup]
- ASP.NET 核心 1.0 文档中了解更多有关[基于声明的授权]

<!-- Links -->
[一系列的一部分]: guidance-multitenant-identity.md
[范围参数]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[受支持的标记和报销申请类型]: ../active-directory/active-directory-token-and-claims.md
[颁发者]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[身份验证事件]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[基于声明的授权]: https://docs.asp.net/en/latest/security/authorization/claims.html
[示例应用程序]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
