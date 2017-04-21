<properties
    pageTitle="怎么我的 WebApi 项目 （Visual Studio Azure Active Directory 连接服务） |Microsoft Azure "
    description="介绍了 MVC 项目通过使用 Visual Studio 的到 Azure AD 连接的 WebApi 会发生什么情况"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>怎么我的 WebApi 项目 （Visual Studio Azure Active Directory 连接服务）

> [AZURE.SELECTOR]
> - [入门教程](vs-active-directory-webapi-getting-started.md)
> - [发生了什么事](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>已添加的引用

###<a name="nuget-package-references"></a>NuGet 程序包的引用

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET 引用

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>代码更改

###<a name="code-files-were-added-to-your-project"></a>将代码文件添加到项目

身份验证启动类， **App_Start/Startup.Auth.cs**已添加到您的项目包含启动 Azure AD 身份验证逻辑。

###<a name="startup-code-was-added-to-your-project"></a>启动代码添加到您的项目

如果您的项目中已经启动类，**配置**方法更新，以包括对`ConfigureAuth(app)`。 否则，启动类被添加到您的项目。


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>您的 app.config 或 web.config 文件具有新配置值。

添加了下面的配置项。
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure AD 应用程序创建

Azure 的广告应用程序向导中选择的目录中创建。

[了解更多有关 Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>如果我签*禁用单独的用户帐户身份验证*，什么其他更改我的项目？
NuGet 将包引用被移除，并且已删除的文件和备份。 这取决于您的项目的状态时，可能需要手动删除其他参考资料或文件，或者修改相应的代码。

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet 将包引用删除 （对于那些存在）

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>代码文件备份和删除 （对于那些存在）

以下每个文件的备份，并从项目中移除。 备份文件都位于备份文件夹内的项目目录的根目录。

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>代码文件的备份 （对于那些存在）

以下每个文件备份之前被替换。 备份文件都位于备份文件夹内的项目目录的根目录。

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>如果我签*读取目录数据*，哪些其他更改我的项目？

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>向 web.config 或 app.config 进行了其他更改

添加了以下的额外的配置项。

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Azure 活动目录应用程序已更新
Azure 活动目录应用程序已更新以包括*读取目录数据*权限和其他键创建它然后被用作*ida︰ 密码*在`web.config`文件。

[了解更多有关 Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
