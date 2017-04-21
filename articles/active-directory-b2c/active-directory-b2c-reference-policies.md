<properties
    pageTitle="Azure 的活动目录 B2C︰ 可扩展的策略框架 |Microsoft Azure"
    description="一个主题在 Azure 活动目录 B2C 的可扩展的策略框架以及如何创建不同的策略类型"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure 的活动目录 B2C︰ 可扩展的策略框架

## <a name="the-basics"></a>基础知识

Azure 活动目录 (AD Azure) B2C 的可扩展的策略框架是该服务的核心实力。 策略充分描述使用者身份体验，例如注册、 登录或配置文件编辑。 例如，注册策略允许您控制行为通过配置以下设置︰

- 帐户使用者可以使用它来注册该应用程序的类型 （Facebook 等社交帐户或本地帐户，如电子邮件地址）。
- 属性 （例如，名字、 邮政编码和鞋号） 要在注册过程收集使用者。
- 使用多因素身份验证。
- 外观与感觉的所有的注册页。
- 信息 （其表现为一个令牌中的声明），该应用程序时接收运行完成的策略。

您可以在您的组织中创建多个不同类型的策略并根据需要在您的应用程序中使用它们。 策略可以跨应用程序重用。 这使开发人员可以定义和修改使用最少的使用者身份经验或对他们的代码做任何更改。

策略是可用于通过一个简单的开发接口。 您的应用程序触发策略使用标准的 HTTP 身份验证请求 （请求中传递策略参数） 和接收一个自定义的令牌作为响应。 例如，之间的唯一区别请求调用注册策略，这些调用的签入策略是使用"p"查询字符串参数中的策略名称︰

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

有关策略框架的详细信息，请参阅此[博客文章](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx)。

## <a name="create-a-sign-up-policy"></a>创建注册策略

若要使您的应用程序上注册，您将需要创建注册策略。 此策略描述的经验的使用者将经过在注册过程和应用程序将接收成功注册的标记的内容。

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**注册策略**。
3. 单击**+ 添加**刀片式服务器顶部。
4. **名称**确定应用程序所使用的注册策略名称。 例如，输入"SiUp"。
5. 单击**标识提供程序**，然后选择"电子邮件注册"。 （可选） 如果已配置，还可以选择社会身份提供商。 单击**确定**。
6. 单击**注册属性**。 在这里，选择您想要在注册过程中从使用者收集的属性。 例如，选择"国家/地区"、"显示名称"和"邮政编码"。 单击**确定**。
7. 单击**应用程序声明**。 在这里，选择您想返回发送回您成功注册体验后的应用程序的标记中声明。 例如，选择"显示名称""标识提供程序"，"邮政编码"，"用户是新"和"用户的对象 ID"。
8. 单击**创建**。 请注意，刚才创建的策略将显示为"**B2C_1_SiUp**"( **B2C\_1\_**自动添加片段)**注册策略**刀片式服务器中。
9. 通过单击"**B2C_1_SiUp**"中打开该策略。
10. 在**应用程序**下拉列表中的选择"Contoso B2C app"和`https://localhost:44321/`在**回复 URL / 重定向 URI**下拉列表。
11. 单击**立即运行**。 将打开新的浏览器选项卡，并可以通过注册您的应用程序的使用者经验来运行。

    > [AZURE.NOTE]
    它占用策略的创建和更新几分钟才能生效。

## <a name="create-a-sign-in-policy"></a>创建签入策略

若要启用登录您的应用程序，您需要创建签入策略。 此策略描述的经验的使用者将经过期间登录和应用程序将收到的标记的内容在成功登录。

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**签入策略**。
3. 单击**+ 添加**刀片式服务器顶部。
4. **名称**确定应用程序所使用的登录策略名称。 例如，输入"SiIn"。
5. 单击**标识提供程序**，然后选择"本地帐户登录"。 （可选） 如果已配置，还可以选择社会身份提供商。 单击**确定**。
6. 单击**应用程序声明**。 在这里，您选择要返回在发送回应用程序之后成功登录体验令牌中的声明。 例如，选择"显示名称"、"标识提供程序"、"邮政编码"和"用户的对象 ID"。 单击**确定**。
7. 单击**创建**。 请注意，刚才创建的策略将显示为"**B2C_1_SiIn**"( **B2C\_1\_**自动添加片段)**登录策略**刀片式服务器中。
8. 通过单击"**B2C_1_SiIn**"中打开该策略。
9. 在**应用程序**下拉列表中的选择"Contoso B2C app"和`https://localhost:44321/`在**回复 URL / 重定向 URI**下拉列表。
10. 单击**立即运行**。 将打开新的浏览器选项卡，并可以通过登录到您的应用程序的使用者经验来运行。

    > [AZURE.NOTE]
    它占用策略的创建和更新几分钟才能生效。

## <a name="create-a-sign-up-or-sign-in-policy"></a>创建注册或登录策略

此策略在处理单个配置两个使用者注册和登录体验。 消费者会根据所处环境的正确路径 （注册编译或登录） 下处于领先地位。 它还描述了该应用程序将接收成功注册或登录后的标记的内容。  代码示例以供注册或登录策略是[可从以下站点](active-directory-b2c-devquickstarts-web-dotnet-susi.md)。

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**注册或签入策略**。
3. 单击**+ 添加**刀片式服务器顶部。
4. **名称**确定应用程序所使用的注册策略名称。 例如，输入"SiUpIn"。
5. 单击**标识提供程序**，然后选择"电子邮件注册"。 （可选） 如果已配置，还可以选择社会身份提供商。 单击**确定**。
6. 单击**注册属性**。 在这里，选择您想要在注册过程中从使用者收集的属性。 例如，选择"国家/地区"、"显示名称"和"邮政编码"。 单击**确定**。
7. 单击**应用程序声明**。 在这里，您选择要返回在发送回应用程序后成功注册或登录体验令牌中的声明。 例如，选择"显示名称""标识提供程序"，"邮政编码"，"用户是新"和"用户的对象 ID"。
8. 单击**创建**。 请注意，刚才创建的策略将显示为"**B2C_1_SiUpIn**"( **B2C\_1\_**自动添加片段)**注册或登录策略**刀片式服务器中。
9. 通过单击"**B2C_1_SiUpIn**"中打开该策略。
10. 在**应用程序**下拉列表中的选择"Contoso B2C app"和`https://localhost:44321/`在**回复 URL / 重定向 URI**下拉列表。
11. 单击**立即运行**。 打开新的浏览器选项卡，然后配置运行通过注册或登录使用者体验。

    > [AZURE.NOTE]
    它占用策略的创建和更新几分钟才能生效。

## <a name="create-a-profile-editing-policy"></a>创建一个配置文件，编辑策略

要启用编辑在您的应用程序的配置文件，您将需要创建编辑策略的配置文件。 此策略描述使用者将经过编辑的配置文件和应用程序将接收成功完成的标记的内容期间的经验。

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**配置文件编辑策略**。
3. 单击**+ 添加**刀片式服务器顶部。
4. **名称**确定编辑应用程序所使用的策略名称的配置文件。 例如，输入"SiPe"。
5. 单击**标识提供程序**，然后选择"电子邮件地址"。 （可选） 如果已配置，还可以选择社会身份提供商。 单击**确定**。
6. 单击**配置文件属性**。 在这里，选择属性，使用者可以查看和编辑。 例如，选择"国家/地区"、"显示名称"和"邮政编码"。 单击**确定**。
7. 单击**应用程序声明**。 在这里，选择您希望返回发送回编辑经验的成功配置文件后应用程序的标记中声明。 例如，选择"显示名称"和"邮政编码"。
8. 单击**创建**。 请注意，刚才创建的策略将显示为"**B2C_1_SiPe**"( **B2C\_1\_**自动添加片段)**配置文件编辑策略**刀片式服务器中。
9. 通过单击"**B2C_1_SiPe**"中打开该策略。
10. 在**应用程序**下拉列表中的选择"Contoso B2C app"和`https://localhost:44321/`在**回复 URL / 重定向 URI**下拉列表。
11. 单击**立即运行**。 将打开新的浏览器选项卡上，并且您可以通过编辑应用程序中的使用者经验的配置文件运行。

    > [AZURE.NOTE]
    它占用策略的创建和更新几分钟才能生效。
    
## <a name="create-a-password-reset-policy"></a>创建密码重置策略

要启用精准密码重置您的应用程序，您需要创建密码重置策略。 租户级密码重置指定选项的注意[这里](active-directory-b2c-reference-sspr.md)仍然适用于登录策略。 此策略描述的经验的使用者将经过在密码重置和标记应用程序将接收成功完成的内容。

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**重设密码策略**。
3. 单击**+ 添加**刀片式服务器顶部。
4. **名称**确定密码重置您的应用程序所使用的策略名称。 例如，输入"SSPR"。
5. 单击**标识提供程序**，然后选择"重置密码使用的电子邮件地址"。 单击**确定**。
6. 单击**应用程序声明**。 在这里，选择您希望返回成功密码重置体验后发送回您的应用程序的标记中声明。 例如，选择"用户的对象 ID"。
7. 单击**创建**。 请注意，刚才创建的策略将显示为"**B2C_1_SSPR**"( **B2C\_1\_**自动添加片段)**密码重置策略**刀片式服务器中。
8. 通过单击"**B2C_1_SSPR**"中打开该策略。
9. 在**应用程序**下拉列表中的选择"Contoso B2C app"和`https://localhost:44321/`在**回复 URL / 重定向 URI**下拉列表。
10. 单击**立即运行**。 将打开新的浏览器选项卡上，并且您可以运行您的应用程序中通过密码重置使用者体验。

    > [AZURE.NOTE]
    它占用策略的创建和更新几分钟才能生效。

## <a name="additional-resources"></a>其他资源

- [标记、 会话和单一登录配置](active-directory-b2c-token-session-sso.md)。
