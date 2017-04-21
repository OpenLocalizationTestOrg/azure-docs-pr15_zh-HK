<properties
    pageTitle="发布 Azure AD 应用程序代理的应用程序 |Microsoft Azure"
    description="发布到云与 Azure AD 应用程序代理服务器的内部应用程序。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>发布应用程序使用 Azure AD 应用程序代理服务器

Azure AD 应用程序代理服务器可帮助您通过发布内部访问 internet 的应用程序支持远程工作人员。 现在，您应该已经[启用的 Azure 的传统门户应用程序代理](active-directory-application-proxy-enable.md)。 这篇文章将引导您完成发布的应用程序在您的本地网络上运行，并提供安全的远程访问来自网络外部的步骤。 完成这篇文章后，您可以配置应用程序，具有个性化的信息或安全要求。

> [AZURE.NOTE] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](active-directory-editions.md)。

## <a name="publish-an-app-using-the-wizard"></a>发布应用程序使用向导

1. 在[Azure 经典门户](https://manage.windowsazure.com/)管理员身份登录。
2. 转到 Active Directory 并选择启用应用程序代理服务器的位置的目录。

    ![Active Directory 的图标](./media/active-directory-application-proxy-publish/ad_icon.png)

3. 单击**应用程序**选项卡，然后单击屏幕底部的**添加**按钮

    ![添加应用程序](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. 选择**发布，将可以从外部网络访问的应用程序**。

    ![发布的应用程序将可以从外部网络访问](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. 提供有关您的应用程序的以下信息︰

    - **名称**︰ 您的应用程序的用户友好名称。 它必须在您的目录中的唯一性。
    - **内部 URL**︰ 应用程序代理服务器连接器用于访问应用程序内部专用网络地址。 在后端服务器进行发布，同时其余的服务器是未发布，可以提供一个特定的路径。 这种方式，可以发布不同的站点，在同一服务器上，并为每个提供其自己的名称和访问规则。

        > [AZURE.TIP] 如果您发布一个路径，确保它包括所有必要的图像、 脚本和样式表为您的应用程序。 例如，如果您的应用程序位于 https://yourapp/app，并使用位于 https://yourapp/media 的图像，然后应作为路径发布 https://yourapp/。

    - **预身份验证方法**︰ 如何应用程序代理，从而使他们获得访问您的应用程序之前验证用户。 从下拉菜单中选择一个选项。

        - Azure 的 Active Directory︰ 签名用 Azure 的广告，对其权限的目录和应用程序进行身份验证的用户重定向应用程序代理。
        - 直通︰ 用户不需要进行身份验证才能访问该应用程序。

    ![应用程序属性](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. 若要完成此向导，请单击屏幕底部的复选标记。 在 Azure 的广告现在定义应用程序。


## <a name="assign-users-and-groups-to-the-application"></a>将用户和组分配给应用程序

为了使您的用户能够访问发布的应用程序，您需要将它们分配单独或成组。 （切记要将自己分配访问权限，太。这就要求每个用户拥有的 Azure 基本或更高版本的许可证。 可以单独或组分配的许可证。 更多详细信息，请参阅[将用户分配给应用程序](active-directory-applications-guiding-developers-assigning-users.md)。 

对于需要预身份验证的应用程序，此授权使用应用程序的权限。 不需要预身份验证的应用程序，用户可以仍将分配给应用程序，使其出现在他们的应用程序列表，例如 MyApps。

1. 添加应用程序向导后，您看到快速启动页为您的应用程序。 若要管理谁有权访问该应用程序，选择**用户和组**。

    ![应用程序代理服务器快速启动指定用户的屏幕抓图](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. 搜索特定组中您的目录，或显示您的所有用户。 要显示搜索结果，请单击该复选标记。

    ![搜索的组或用户的屏幕抓图](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. 选择每个用户或组，您想要分配给此应用程序，然后单击**分配**。 要求您确认此操作。

> [AZURE.NOTE] 用于集成 Windows 身份验证的应用程序，您可以从内部活动目录分配只有用户和同步的组。 使用 Microsoft 帐户和来宾登录的用户无法使用 Azure 活动目录应用程序代理发布的应用程序的分配。 请确保您的用户登录凭据，您要发布的应用程序位于同一个域的一部分。

## <a name="test-your-published-application"></a>您已发布的应用程序进行测试

一旦发布您的应用程序，可以通过导航到发布的 URL 来测试出来。 请确认您可以访问它，它将正确呈现并一切按预期的方式工作。 如果您有问题或得到一条错误消息，请尝试[故障排除指南](active-directory-application-proxy-troubleshoot.md)。

## <a name="configure-your-application"></a>配置应用程序

您可以修改已发布的应用程序或设置高级选项卡上的配置页面。 在此页上，您可以自定义您的应用程序更改名称或上传一个徽标。 您还可以管理如预身份验证方法或多因素身份验证的访问规则。

![高级的配置](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


发布应用程序后使用 Azure 活动目录应用程序代理，它们出现在 Azure 的广告，在应用程序列表，并可以那里管理。

如果已发布的应用程序，您可以禁用应用程序代理服务，它们将不再可以从您的专用网络外部访问。 这不会删除应用程序。

若要查看应用程序并确保它是可访问，双击该应用程序的名称。 如果禁用该应用程序代理服务器服务和应用程序不可用，在屏幕的顶部会出现一条警告消息。

要删除一个应用程序，请在列表中选择一个应用程序，然后单击**删除**。

## <a name="next-steps"></a>下一步行动

- [发布应用程序使用您自己的域名](active-directory-application-proxy-custom-domains.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)
- [使用声明感知应用程序](active-directory-application-proxy-claims-aware-apps.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
