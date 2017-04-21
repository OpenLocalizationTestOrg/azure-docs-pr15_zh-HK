<properties
    pageTitle="如何管理 Azure AD 中的联合身份验证证书 |Microsoft Azure"
    description="了解如何自定义您的联合身份验证证书的到期日期和如何进行更新证书将很快到期。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>管理的联盟单一登录 Azure 的 Active Directory 中的证书

本文介绍了 Azure Active Directory，为了建立联盟单一登录 (SSO) 对 SaaS 应用程序创建的证书与相关的常见问题的解决方案。

这篇文章只是相关的应用程序配置为使用**Azure AD Single Sign-on**，如下面的示例所示︰

![Azure 的广告单登录](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>如何自定义您联合身份验证的证书的到期日期

默认情况下，证书有效期设置为两年后。 通过执行下列步骤，您可以选择您的证书的不同的过期日期。 包括屏幕快照示例中，为了使用队伍，但这些步骤可以适用于任何联盟的 SaaS 应用程序。

1. 在 Azure Active Directory，您的应用程序，快速启动页上单击**配置单一登录**。

    ![打开该 SSO 配置向导。](./media/active-directory-sso-certs/config-sso.png)

2. **Azure AD Single Sign-on**，选择，然后单击**下一步**。

3. 键入**登录 URL**的应用程序，并**配置证书用于联盟单一登录**选中的复选框。 然后单击**下一步**。

    ![Azure 的广告单登录](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. 在下一页上，选择**生成新的证书**，选择证书的有效时间。 然后单击**下一步**。

    ![生成新的证书](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. 接下来，单击**下载证书**。 若要了解如何将证书传到特定 SaaS 应用程序，请单击**查看配置说明**。

    ![下载，然后上载证书](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. 不会启用证书，直到您选择底部的对话框中的确认复选框，然后按提交。

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>如何续订证书不久将到期

如下所示的续订步骤理想情况下应导致为您的用户没有大量的停机时间。 屏幕抓图，在此节功能队伍中使用作为示例，但这些步骤可以应用于任何联盟的 SaaS 应用程序。

1. 在 Azure Active Directory，您的应用程序，快速启动页上单击**配置单一登录**。

    ![打开 SSO 配置向导](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. 在对话框中的第一页， **Azure AD Single Sign-on**应该已经是处于选中状态，因此单击**下一步**。

3. 在第二页上，选中**配置联盟单一登录使用的证书**复选框。 然后单击**下一步**。

    ![Azure 的广告单登录](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. 在下一页上，选择**生成新的证书**，选择新的证书的有效时间。 然后单击**下一步**。

    ![生成新的证书](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. 请单击**下载证书**。 为成功 rewnew 您的证书，您必须执行以下两个步骤︰

    - SaaS 应用程序的单一登录配置屏幕上传新的证书。 若要了解如何为您的特定 SaaS 应用程序执行此操作，请单击**查看配置说明**。

    - Azure 做广告，选择以启用新的证书，该对话框底部的确认复选框，然后单击**下一步**将提交。

    > [AZURE.IMPORTANT] 将禁用单一登录到应用程序完成这两个步骤的时刻之一，但它将在完成第二步后再次启用。 因此，若要最小化停机时间，请准备一小段彼此的时间内完成这两个步骤。

    ![下载，然后上载证书](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [应用程序访问权限和单一登录使用 Azure 活动目录](active-directory-appssoaccess-whatis.md)
- [基于 SAML 的单一登录疑难解答](active-directory-saml-debugging.md)
