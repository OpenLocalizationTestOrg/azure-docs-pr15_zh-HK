<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Google 应用程序 |Microsoft Azure"
    description="了解如何使用 Azure Active Directory Google 应用程序启用单一登录、 自动化资源调配，以及更多 ！"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-google-apps-with-azure-active-directory"></a>教程︰ 如何与 Azure Active Directory 集成 Google 应用程序

本教程将向您展示如何将您的 Google 应用程序环境连接到 Azure 活动目录 (AD Azure)。 您将学习如何配置单一登录 Google 应用程序、 如何启用自动的用户供应，并如何将用户有权访问 Google 应用程序分配。 

##<a name="prerequisites"></a>系统必备组件

1. 若要通过[Azure 的传统门户网站](https://manage.windowsazure.com)访问 Azure Active Directory，首先必须有效 Azure 订购。

2. 对[Google 应用程序，以工作](https://www.google.com/work/apps/)或[教育的 Google 应用程序](https://www.google.com/edu/products/productivity-tools/)，必须具有有效的租户。 对于这两个服务，您可以使用免费的试用帐户。

##<a name="video-tutorial"></a>视频教程

如何在 2 分钟内启用单一登录 Google 应用程序︰

> [AZURE.VIDEO enable-single-sign-on-to-google-apps-in-2-minutes-with-azure-ad]

##<a name="frequently-asked-questions"></a>常见问题

1. **问︰ 将 Chromebooks 和其他镶边设备兼容使用 Azure AD 单一登录吗？**

    答︰ 是的用户将能够登录到他们使用其 Azure AD 凭据的 Chromebook 设备。 请参阅此[Google 应用程序支持文章](https://support.google.com/chrome/a/answer/6060880)的信息上为什么可能获得提示用户输入凭据两次。

2. **问︰ 如果我启用了单一登录，用户将能够使用 Azure AD 凭据登录到 Google 的任何产品，如 Google 教室、 GMail、 Google 驱动器、 YouTube 等？**

    答︰ 是的这取决于[哪个 Google 应用程序](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583)您选择为您的组织启用或禁用。

3. **问︰ 可以为我 Google 应用程序的用户的子集的单一登录启用吗？**

    答︰ 没有，启用单一登录将立即要求所有的 Google 应用程序用户与他们的 Azure AD 凭据进行身份验证。 由于 Google 应用程序不支持具有多个标识提供程序，您的 Google 应用程序环境标识提供程序可以是 Azure 广告或 Google — 但不是能同时在同一时间。

4. **问︰ 如果用户在登录窗口之间，将它们自动进行身份验证到 Google 应用程序不提示输入密码的情况下？**

    答︰ 有两个选项用于启用该方案。 首先，用户无法登录到 Windows 10 通过[Azure 活动目录联接](active-directory-azureadjoin-overview.md)的设备。 或者，用户可以登录到加入域到已启用单一登录到 Azure 广告通过[Active Directory 联合身份验证服务 (AD FS)](active-directory-aadconnect-user-signin.md)部署内部部署 Active Directory 的 Windows 设备。 当然，这两个选项都需要按照下面以启用单一登录之间 Azure AD 教程和 Google 应用程序。

##<a name="step-1-add-google-apps-to-your-directory"></a>步骤 1︰ 将 Google 应用程序添加到您的目录

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![从左侧的导航窗格中选择活动目录。][0]

2. 从**目录**列表中，选择您想要添加到 Google 应用程序的目录。

3. 在顶部的菜单中单击**应用程序**。

    ![单击应用程序。][1]

4. 单击页面底部的**添加**。

    ![单击添加以添加新的应用程序。][2]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![单击库中添加应用程序。][3]

6. 在**搜索框**中，键入**Google 应用程序**。 然后从结果中，选择**Google 应用程序**，然后单击**完成**添加该应用程序。

    ![添加 Google 应用程序。][4]

7. 现在应该看到 Google 应用程序的快速启动页︰

    ![在 Azure 广告的 Google 应用程序快速起始页][5]

##<a name="step-2-enable-single-sign-on"></a>步骤 2︰ 启用单一登录

1. 在 Azure 的广告，Google 应用程序，快速启动页上单击**配置单一登录**按钮。

    ![配置单一登录按钮][6]

2. 将会打开一个对话框，您会看到一个屏幕，询问"您想怎样来登录到 Google 应用程序的用户吗？" **Azure AD Single Sign-on**，选择，然后单击**下一步**。

    ![选择 Azure 的广告单登录][7]

    > [AZURE.NOTE] 若要了解更多有关不同选项的有关单一登录方式进行登录，[请单击此处](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

3. 在**配置应用程序设置**页中，对于**符号在 URL**字段中，您的 Google 应用程序中的类型租户使用以下格式的 URL:`https://mail.google.com/a/<yourdomain>`

    ![在租户 URL 中键入][8]

4. 在**自动配置单一登录**页上，键入域中的 Google 应用程序租户。 然后按**配置**按钮。

    ![键入您的域名，然后按配置。](./media/active-directory-saas-google-apps-tutorial/ga-auto-config.png)

    > [AZURE.NOTE] 如果您愿意手动配置单一登录，请参阅[可选步骤︰ 手动配置单一登录](#optional-step-manually-configure-single-sign-on)

5. 登录到您的 Google 应用程序管理员帐户。 然后单击**允许**以允许 Azure Active Directory 中 Google 应用程序订阅进行配置更改。

    ![键入您的域名，然后按配置。](./media/active-directory-saas-google-apps-tutorial/ga-consent.PNG)

6. 在 Azure Active Directory 配置 Google 应用程序租户时，请等待几秒钟。 当它完成后时，单击**下一步**。

10. 在最后一页的对话框中，键入电子邮件地址如果您想要接收电子邮件通知错误和与这种单一登录配置维护相关的警告。

    ![键入您的电子邮件地址。][14]

11. 单击**完成**关闭对话框。 若要测试您的配置，请参阅下面的一节题为[分配到 Google 应用程序的用户](#step-4-assign-users-to-google-apps)。

##<a name="optional-step-manually-configure-single-sign-on"></a>可选步骤︰ 手动配置单一登录

如果您愿意手动安装单一登录设置，请完成以下步骤︰

1. 在 Azure 的广告，Google 应用程序，快速启动页上单击**配置单一登录**按钮。

    ![配置单一登录按钮][6]

2. 将会打开一个对话框，您会看到一个屏幕，询问"您想怎样来登录到 Google 应用程序的用户吗？" **Azure AD Single Sign-on**，选择，然后单击**下一步**。

    ![选择 Azure 的广告单登录][7]

    > [AZURE.NOTE] 若要了解更多有关不同选项的有关单一登录方式进行登录，[请单击此处](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

3. 在**配置应用程序设置**页中，对于**符号在 URL**字段中，您的 Google 应用程序中的类型租户使用以下格式的 URL:`https://mail.google.com/a/<yourdomain>`

    ![在租户 URL 中键入][8]

4. 在**自动配置单一登录**页上，选择标记为**手动配置该应用程序的单一登录**复选框。 然后单击**下一步**。

    ![选择手动配置。](./media/active-directory-saas-google-apps-tutorial/ga-auto-skip.PNG)

4. **配置单一登录在 Google 应用程序**页面上**下载证书**，请单击，然后保存在您的计算机的本地证书文件。

    ![下载此证书。][9]

5. 在浏览器中，打开一个新标签， [Google 应用程序管理控制台](http://admin.google.com/)使用管理员帐户登录。

6. 单击**安全**。 如果您看不到该链接，则它可能被隐藏在屏幕的底部**其他控件**菜单下。

    ![单击安全。][10]

7. 在**安全**页中，单击**设置单一登录 (SSO)。**

    ![单击 SSO。][11]

8. 执行以下配置更改︰

    ![配置 SSO][12]

    - 选择**具有第三方身份标识提供程序的安装 SSO**。

    - 在 Azure 广告，复制的**单一登录服务的 URL**，并将其粘贴到 Google 应用程序的**登录页的 URL**字段。

    - 在 Azure 广告，复制**单个退出服务 URL**，并将其粘贴到 Google 应用程序中的**退出页面 URL**字段。

    - 在 Azure 广告，复制的**更改密码 URL**，并将其粘贴到 Google 应用程序中的**更改密码 URL**字段。

    - 在 Google 应用程序，**验证证书**，上载您在步骤 4 中下载该证书。

    - 单击**保存更改**。

9. 在 Azure 广告，选择单一登录配置确认复选框启用您上载到 Google 应用程序的证书。 然后单击**下一步**。

    ![检查确认复选框][13]

10. 在最后一页的对话框中，键入电子邮件地址如果您想要接收电子邮件通知错误和与这种单一登录配置维护相关的警告。 

    ![键入您的电子邮件地址。][14]

11. 单击**完成**关闭对话框。 若要测试您的配置，请参阅下面的一节题为[分配到 Google 应用程序的用户](#step-4-assign-users-to-google-apps)。

##<a name="step-3-enable-automated-user-provisioning"></a>第 3 步︰ 使用户自动化资源调配

> [AZURE.NOTE] 自动完成用户供应到 Google 应用程序的另一个可行的选择是使用[Google 应用程序目录同步 (GADS)](https://support.google.com/a/answer/106368?hl=en)规定内部部署 Active Directory Google 应用程序标识。 与此相反的是，在本教程中的解决方案规定 Azure Active Directory （云） 用户和已启用邮件的组添加到 Google 的应用程序。

1. 登录到[Google 应用程序管理控制台](http://admin.google.com/)使用您的管理员帐户，然后单击**安全性**。 如果您看不到该链接，则它可能被隐藏在屏幕的底部**其他控件**菜单下。

    ![单击安全。][10]

2. 在**安全**页中，单击**API 参考**。

    ![单击 API 参考。][15]

3. 选择**启用 API 访问**。

    ![单击 API 参考。][16]

    > [AZURE.IMPORTANT] 要调配到 Google 应用程序，他们的用户名在 Azure Active Directory*必须*到一个自定义的域绑定的每个用户。 例如，如下所示的用户名bob@contoso.onmicrosoft.com将不会被接受 Google 应用程序，而bob@contoso.com将被接受。 可以通过在 Azure AD 中编辑其属性来更改现有用户的域。 下文介绍了如何将自定义的域设置 Azure Active Directory 和 Google 应用程序的说明。

4. 如果您没有添加一个自定义域名到 Azure 活动目录，然后按照下面的步骤︰

    - 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。 在目录列表中，选择目录。 

    - 从顶级菜单中，单击**域**，然后单击**添加自定义的域**上。

        ![添加一个自定义的域][17]

    - 在**域名称**字段键入域名称。 这应该是您打算使用的 Google 应用程序的同一个域名。 准备好后，请单击**添加**按钮。

        ![键入您的域名。][18]

    - 单击**下一步**转到验证页面。 若要验证您自己的域，您必须编辑按照此页上提供的值的域的 DNS 记录。 您可以选择验证使用**MX 记录**或**TXT 记录**，具体取决于您所选择的**记录类型**的选项。 有关如何验证域名与 Azure 广告更全面说明，请参阅[添加到 Azure 广告自己域名](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409)。

        ![请验证您的域名。][19]

    - 所有您想要添加到您的目录的域重复以上步骤。

5. 现在，您已验证所有使用 Azure AD 域，您必须现在它们再次验证与 Google 应用程序。 对于与 Google 应用程序尚未注册每个域，请执行以下步骤︰

    - 在[Google 应用程序管理控制台](http://admin.google.com/)中，单击**域**。

        ![单击域][20]

    - 单击**添加域或域别名**。

        ![添加新域][21]

    - 选择**添加其他域**，然后键入您想要添加的域的名称。

        ![键入您的域名][22]

    - 单击**继续验证域所有权和**。 然后按照步骤来验证您自己的域名。 有关如何验证您的 Google 应用程序域的全面说明，请参阅[验证您站点所有权与 Google 应用程序](https://support.google.com/webmasters/answer/35179)。

    - 对您希望添加到 Google 应用程序的所有其他域重复上述步骤。

    > [AZURE.WARNING] 如果您更改您的 Google 应用程序租户，主域和如果您已配置单一登录使用 Azure 的广告，则您将需要重复执行步骤 3 下的[第二步︰ 启用单一登录](#step-two-enable-single-sign-on)。

6. 在[Google 应用程序管理控制台](http://admin.google.com/)中，单击**管理**角色。

    ![Google 应用程序上单击][26]

7. 确定您想要使用来管理用户设置的管理员帐户。 对于该帐户**的管理员角色**，编辑该角色的**权限**。 请确保它具有所有**管理 API 权限**启用，以便该帐户可用于资源调配。

    ![Google 应用程序上单击][27]

    > [AZURE.NOTE] 如果您正在配置的生产环境，最好就是在 Google 应用程序中创建新的管理员帐户，专门为这一步。 这些帐户必须拥有必要的 API 权限管理角色与之相关联。

8. 在 Azure Active Directory 中的顶级菜单中，单击**应用程序**，然后单击对**Google 应用程序**。

    ![Google 应用程序上单击][23]

9. 在 Google 应用程序的快速启动页中，单击**配置用户设置**。

    ![配置用户资源调配][24]

10. 在打开的对话框中，单击**启用用户供应**到您想要使用来管理资源调配 Google 应用程序管理员帐户进行身份验证。

    ![提供启用][25]

11. 确认您想要授予 Azure Active Directory 权限对 Google 应用程序组织进行更改。

    ![确认的权限。][28]

12. 单击**完成**关闭对话框。

##<a name="step-4-assign-users-to-google-apps"></a>步骤 4︰ 将用户分配到 Google 应用程序

1. 若要测试您的配置，开始在该目录中创建新的测试帐户。

2. 在 Google 应用程序快速启动页上，单击**分配用户**按钮。

    ![对指定用户单击][29]

3. 选择测试用户，然后单击**分配**按钮在屏幕的底部︰

 - 如果还没有启用自动的用户供应，然后您会看到以下提示︰

        ![Confirm the assignment.][30]

 - 如果您已经启用了资源调配的自动化的用户，您将看到提示定义用户应在 Google 应用程序具有何种角色。 新调配的用户应在 Google 应用程序环境中出现几分钟的时间。

4. 要测试您的单一登录设置，请打开[https://myapps.microsoft.com](https://myapps.microsoft.com/)，在访问权限面板然后登录到测试帐户中，和**Google 应用程序**上单击。

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-google-apps-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-google-apps-tutorial/add-app.png
[3]: ./media/active-directory-saas-google-apps-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-google-apps-tutorial/add-gapps.png
[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png
