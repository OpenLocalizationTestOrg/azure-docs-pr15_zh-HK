<properties
   pageTitle="在多租户应用程序授权 |Microsoft Azure"
   description="如何在多租户应用程序中执行授权"
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
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>在多租户应用程序的基于角色和基于资源的授权

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

本文是[一系列的一部分]。 此外，还有伴随这一系列完整[的示例应用程序]。

我们[参考实现]是核心 1.0 ASP.NET 应用程序。 在本文中我们将看两种常规方法授权，使用授权提供在 ASP.NET 1.0 中核心 Api。

-   **基于角色的授权**。 授权根据分配给用户的角色的操作。 例如，某些操作需要管理员角色。
-   **基于资源的授权**。 授权根据特定资源的操作。 例如，每个资源都有一个所有者。 所有者可以删除资源;其他用户则不能。

典型的应用程序将使用两者的混合。 例如，若要删除某个资源时，用户必须是资源的所有者_或_管理员。


## <a name="role-based-authorization"></a>基于角色的授权

[Tailspin 调查][Tailspin]应用程序中定义的以下角色︰

- 管理员。 可以执行任何调查属于该组织的所有 CRUD 操作。
- 创建者。 可以创建新的调查
- 读取器。 可以读取任何属于该组织的调查

角色适用于_用户_的应用程序。 在调查应用程序时，用户是管理员、 创建者或读取器。

有关如何定义和管理角色的讨论，请参阅[应用程序角色]。

无论您管理角色的方式，将类似授权码。 ASP.NET 1.0 核心引入了抽象称为[授权策略][policies]。 使用此功能，您可以在代码中，定义授权策略并将这些策略应用于控制器操作。 从控制器分离的策略。

### <a name="create-policies"></a>创建策略

若要定义一个策略，首先创建一个实现类`IAuthorizationRequirement`。 它是最简单的方法从`AuthorizationHandler`。 在`Handle`方法，检查相关的报销申请。

这里是从 Tailspin 调查应用程序示例︰

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] 请参见[SurveyCreatorRequirement.cs]

此类定义为用户创建一个新的调查的要求。 用户必须是 SurveyAdmin 或 SurveyCreator 角色中。

在您启动的类，定义命名的策略，其中包含一个或多个要求。 如果有多个要求，则用户必须满足_每个_要求获得授权。 下面的代码定义了两种策略︰

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] 请参见[Startup.cs]

此代码还将设置身份验证方案，告诉的 ASP.NET 授权失败时，应运行哪些身份验证中间件。 在这种情况下，我们指定 cookie 身份验证中间件，因为 cookie 身份验证中间件可以将用户重定向到"禁止"页。 禁止访问页上的位置设置 AccessDeniedPath 选项的 cookie 的中间件;请参阅[配置身份验证中间件]。

### <a name="authorize-controller-actions"></a>授权控制器操作

最后，要授权 MVC 控制器中的操作，策略中设置`Authorize`属性︰

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

在早期版本的 ASP.NET，您可设置**角色**属性对属性︰

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

在 ASP.NET 核心 1.0 中，仍支持这样，但是它有一些缺点与授权策略︰

-   该示例假定特定的声明类型。 策略可以检查任何声明类型。 角色是声明的类型。
-   角色名称是硬编码到该属性。 使用策略，授权逻辑是在一个位置，配置使其更容易与更新或甚至负载。
-   策略启用更复杂的授权决定 (如年龄 > = 21)，不能表示简单的角色成员资格。

## <a name="resource-based-authorization"></a>基于资源授权

_基于资源的授权_发生时授权取决于将受操作影响的特定资源。 在 Tailspin 调查应用程序，每个调查都有一个所有者和零对多参与者。

-   所有者可以读取、 更新、 删除、 发布和取消发布调查。
-   所有者可以将参与者分配调查。
-   参与者可以读取和更新调查。

请注意"所有者"和"参与者"不是应用程序角色;它们存储每应用数据库在调查中。 若要检查用户是否可以删除一项调查，例如，应用程序检查用户是否为此次调查的负责人。

在 ASP.NET 核心 1.0 中，通过从**AuthorizationHandler**派生并重写的**处理**方法来实现基于资源的授权。

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

请注意，此类强类型，为调查对象。  在启动时注册 DI 类︰

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

若要执行的授权检查，使用**IAuthorizationService**接口，您可以插入到您的控制器。 下面的代码检查用户是否可以读取调查︰

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

因为我们在传递`Survey`对象时，此调用将调用`SurveyAuthorizationHandler`。

在授权代码中，一个好的方法是聚合所有的用户的基于角色和基于资源的权限，则检查聚合设置与所需的操作。
下面是从调查应用程序的一个示例。 应用程序定义了几种权限类型︰

- 管理
- 参与者
- 创建者
- 所有者
- 读取器

应用程序还定义了一套调查可能的操作︰

- 创建
- 读取
- 更新
- 删除
- 发布
- Unpublsh

下面的代码创建一个特定的用户和调查的权限的列表。 请注意，此代码在用户应用程序角色，并在调查的所有者/参与者字段查找。

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] 请参阅[SurveyAuthorizationHandler.cs]。

在多租户应用程序，您必须确保该权限不"泄漏"到另一个租户的数据。 讨论参与者权限允许跨租户在调查应用程序，执行&mdash;可以将某人分配从 contriubutor 作为另一个租户。 其他权限类型被限制为属于该用户的租户的资源。 要强制实施此要求，该代码授予权限之前检查租户 ID。 (`TenantId`字段分配创建调查后。)

下一步是检查针对权限 （读取、 更新、 删除等） 的操作。 调查应用程序通过使用查阅表格的函数实现此步骤︰

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>下一步行动

- 阅读本系列的下一篇文章︰[保护后端 web 的多租户应用程序的 API][web-api]
- 若要了解有关在 ASP.NET 1.0 核心资源基于授权的详细信息，请参阅[资源基于授权][rbac]。

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[一系列的一部分]: guidance-multitenant-identity.md
[应用程序角色]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[参考实现]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[配置身份验证中间件]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[示例应用程序]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
