<properties
    pageTitle="Azure 的 Active Directory B2C︰ 多因素身份验证 |Microsoft Azure"
    description="如何启用受 Azure 活动目录 B2C 的面向消费者的应用程序中的多因素身份验证"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure 的活动目录 B2C︰ 面向消费者的应用程序中启用多因素身份验证

Azure 的活动目录 (AD Azure) B2C 与集成在一起直接[Azure 多因素身份验证](../multi-factor-authentication/multi-factor-authentication.md)，以便您可以添加第二个图层的安全注册和登录体验面向消费者的应用程序中。 然后，您可以这样做而无需编写一行代码。 目前，我们支持电话和文本消息验证。 如果您已经创建了注册和登录策略，您仍可以启用多因素身份验证。

> [AZURE.NOTE]
创建注册和登录的策略，而不仅仅是通过编辑现有策略时，您也可以启用多因素身份验证。

此功能可帮助应用程序处理方案，如下所示︰

- 您不需要多因素身份验证来访问一个应用程序，但是您需要使用它来访问另一个。 例如，使用者可以登录到一个自动保险应用程序使用社交或本地帐户，但是访问家庭保险应用程序所在的目录中注册之前，必须验证电话号码。
- 不需要多因素身份验证，通常情况下，访问某个应用程序，但您需要使用它来访问其中的敏感部分。 例如，使用者可以登录到一个银行应用程序与社会或本地帐户和检查帐户余额，但必须在尝试电子转账之前验证电话号码。

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>修改注册策略以启用多因素身份验证

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**注册策略**。
3. 单击您注册的策略 (例如，"B2C_1_SiUp") 以将其打开。
4. 单击**多因素身份验证**，启用**状态**为**ON**。 单击**确定**。
5. 单击**保存**顶部的刀片式服务器。

可以使用策略上的"立即运行"功能来验证使用者体验。 确认下列信息︰

多因素身份验证步骤之前在您的目录中创建一个使用者帐户。 在步骤中，使用者被要求提供他或她的电话号码，并验证它。 如果验证成功，则电话号码附以供以后使用的使用者帐户。 即使使用者取消或减弱，可以询问他或她，要再次在下一次登录验证电话号码，（与多因素身份验证已启用）。

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>修改登录策略以启用多因素身份验证

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**签入策略**。
3. 单击您登录的策略 (例如，"B2C_1_SiIn") 以将其打开。 单击**编辑**顶部的刀片式服务器。
4. 单击**多因素身份验证**，启用**状态**为**ON**。 单击**确定**。
5. 单击**保存**顶部的刀片式服务器。

可以使用策略上的"立即运行"功能来验证使用者体验。 确认下列信息︰

当使用者登陆 （使用社交或本地帐户），他或她如果验证的电话号码连接到使用者帐户，需要进行验证。 如果连接没有电话号码，则被要求使用者提供一个，并验证它。 在成功的验证电话号码附以供以后使用的使用者帐户。

## <a name="multi-factor-authentication-on-other-policies"></a>其他策略的多因素身份验证

注册和登录策略上面所述，还有可能要启用多因素身份验证注册或登录策略和密码重置策略。 它将很快在编辑策略的配置文件。
