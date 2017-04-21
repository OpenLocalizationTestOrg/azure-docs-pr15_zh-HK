<properties
    pageTitle="教程︰ 与销售队伍 Azure Active Directory 集成 |Microsoft Azure"
    description="了解如何使用 Azure Active Directory 队伍启用单一登录、 自动化资源调配，以及更多 ！"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>教程︰ 如何与 Azure Active Directory 集成销售队伍

本教程将向您展示如何连接到将 Azure Active Directory 的销售环境。 您将学习如何配置单一登录与队伍、 如何启用自动的用户供应和如何分配用户有权访问销售队伍。

##<a name="prerequisites"></a>系统必备组件

1. 若要通过[Azure 的传统门户网站](https://manage.windowsazure.com)访问 Azure Active Directory，首先必须有效 Azure 订购。

2. [Salesforce.com](https://www.salesforce.com/)中，必须有一个有效的租户。

> [AZURE.IMPORTANT] 如果您使用的 Salesforce.com**试用**帐户，然后您将无法配置自动化的用户供应。 试用帐户将不具有必要的 API 访问启用之前他们就会购买。
> 
> 您可以绕过此限制使用[免费的开发者帐户](https://developer.salesforce.com/signup)来完成本教程。

如果您使用的队伍沙盒环境，请参阅[销售队伍沙盒集成教程](https://go.microsoft.com/fwLink/?LinkID=521879)。

##<a name="video-tutorials"></a>视频教程

您可以按照本教程中使用下面的视频。

**视频教程第一部分︰ 如何启用单一登录**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**视频教程第二部分︰ 如何自动化用户资源调配**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>步骤 1︰ 添加到您的目录的队伍

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![从左侧的导航窗格中选择活动目录。][0]

2. 从**目录**列表中，选择您想要添加到销售队伍的目录。

3. 在顶部的菜单中单击**应用程序**。

    ![单击应用程序。][1]

4. 单击页面底部的**添加**。

    ![单击添加以添加新的应用程序。][2]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![单击库中添加应用程序。][3]

6. 在**搜索框**中，键入**队伍**。 然后从结果中，选择**队伍**并单击**完成**添加该应用程序。

    ![添加销售队伍。][4]

7. 现在应该看到队伍的快速启动页︰

    ![在 Azure 的广告的销售队伍的快速起始页][5]

##<a name="step-2-enable-single-sign-on"></a>步骤 2︰ 启用单一登录

1. 单一登录配置之前，必须设置和部署自定义的域为您的销售环境。 有关如何执行此操作的说明，请参阅[设置的域名称](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US)。

2. 在队伍的快速起始页在 Azure 的广告中，单击**配置单一登录**按钮。

    ![配置单一登录按钮][6]

3. 将会打开一个对话框，您会看到一个屏幕，询问"您想怎样用户能够登录到队伍吗？" **Azure AD Single Sign-on**，选择，然后单击**下一步**。

    ![选择 Azure 的广告单登录][7]

    > [AZURE.NOTE] 若要了解更多有关不同选项的有关单一登录方式进行登录，[请单击此处](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. **配置应用程序设置**页上填写**号在 URL**中队伍域 URL 使用以下格式键入︰
 - 企业客户︰`https://<domain>.my.salesforce.com`
 - 开发人员帐户︰`https://<domain>-dev-ed.my.salesforce.com` 

    ![在您登录 URL 类型][8]

5. 在**配置单一登录队伍在**页上，在**下载证书**，请单击，然后保存在您的计算机的本地证书文件。

    ![下载证书][9]

6. 在您的浏览器和登录到您的销售队伍管理员帐户中打开新选项卡。

7. **管理员**导航窗格下，单击以展开相关的部分的**安全控制**。 然后单击**单一登录设置**。

    ![单击安全控制下的单一登录设置][10]

8. 在**单一登录设置**页上，单击**编辑**按钮。

    ![单击编辑按钮][11]

    > [AZURE.NOTE] 如果您不能启用单一登录队伍帐号设置，您可能需要联系销售队伍的支持，以便将为您启用该功能。

9. **SAML 启用**，选择，然后单击**保存**。

    ![选择已启用的 SAML][12]

10. 要配置 SAML 单一登录设置，请单击**新建**。

    ![选择已启用的 SAML][13]

11. 在**SAML 单一登录设置编辑**页上，进行以下配置︰

    ![应进行配置的屏幕抓图][14]

 - 对于**名称**字段中，键入此配置的友好名称。 **名称**自动填充**API 名称**文本框中提供一个值。

 - 在 Azure 的广告，将**颁发者 URL**值，复制，然后将其粘贴到队伍中的**颁发者**字段。

 - 在**实体 Id 文本框**中，键入您销售队伍的域名使用以下模式︰
     - 企业客户︰`https://<domain>.my.salesforce.com`
     - 开发人员帐户︰`https://<domain>-dev-ed.my.salesforce.com`

 - 单击**浏览**或**选择文件**以打开**选择要上载的文件**对话框中，选择您销售队伍的证书，然后单击**打开**上载证书。

 - 对于**SAML 标识类型**，选择**断言包含用户的 salesforce.com 用户名**。

 - **SAML 标识位置**，选择**标识是主题语句的 NameIdentifier 元素中**

 - 在 Azure 广告，复制的**远程登录 URL**值，，然后将其粘贴到队伍中的**身份提供程序的登录 URL**字段。

 - 对于**服务提供程序初始化请求绑定**，请选择**HTTP 重定向**。

 - 最后，单击**保存**以应用 SAML 单一登录设置。

12. 在队伍中的左侧的导航窗格中，单击**域管理**，以便展开相关的部分，，然后单击**我的域**。

    ![在我的域上单击][15]

13. 向下滚动到**验证配置**部分，然后单击**编辑**按钮。

    ![单击编辑按钮][16]

14. 在**认证服务**部分中，选择您的 SAML SSO 配置的友好名称，然后单击**保存**。

    ![选择 SSO 配置][17]

    > [AZURE.NOTE] 如果选择了多个身份验证服务，然后当用户试图启动单一登录到您的销售环境，他们将提示您选择要登录的身份验证服务。 如果您不希望出现这种情况，您应**保持所有未选中其他身份验证服务**。

15. 在 Azure 广告，选择单一登录配置确认复选框启用的证书，您将上载到销售队伍。 然后单击**下一步**。

    ![检查确认复选框][18]

16. 在最后一页的对话框中，键入电子邮件地址如果您想要接收电子邮件通知错误和与这种单一登录配置维护相关的警告。 

    ![键入您的电子邮件地址。][19]

17. 单击**完成**关闭对话框。 若要测试您的配置，请参见一节下面[为用户分配销售队伍](#step-4-assign-users-to-salesforce)。

##<a name="step-3-enable-automated-user-provisioning"></a>第 3 步︰ 使用户自动化资源调配

1. 在队伍的 Azure 广告快速起始页，单击**配置用户设置**按钮。

    ![单击配置用户设置按钮][20]

2. 在**配置用户设置**对话框中，键入您的销售队伍管理用户名和密码。

    ![键入您的管理员用户名或密码][21]

    > [AZURE.NOTE] 如果您配置的生产环境，最好的办法是在队伍中创建新的管理员帐户，专门为这一步。 该帐户必须具有**系统管理员**配置文件分配给它的销售队伍。

3. 若要获取队伍安全令牌，打开新的选项卡和符号到同一队伍管理帐户。 在页面的右上角，单击您的名称，，然后单击**我**的设置。

    ![单击您的名称，然后单击我的设置][22]

4. 在左侧的导航窗格中，**个人**展开相关的部分中，单击，然后单击**重置我安全令牌**。

    ![单击您的名称，然后单击我的设置][23]

5. 在**重置我安全令牌**页上，单击**重置安全令牌**的按钮。

    ![阅读的警告。][24]

6. 检查与此管理员帐户关联的电子邮件收件箱。 查找包含新的安全性令牌的 Salesforce.com 发的电子邮件。

7. 复制标记、 转到 Azure 的广告窗口，并将其粘贴到**用户安全令牌**的域。 然后单击**下一步**。

    ![安全令牌中粘贴][25]

8. 在确认页中，可以选择配置故障发生时收到电子邮件通知。 单击**完成**关闭对话框。

    ![键入您的电子邮件地址以接收通知][26]

##<a name="step-4-assign-users-to-salesforce"></a>步骤 4︰ 将用户分配给队伍

1. 要测试您的配置，请首先在目录中创建一个新的测试帐户。

2. 在队伍快速启动页上，单击**分配用户**按钮。

    ![对指定用户单击][27]

3. 选择测试用户，然后单击**分配**按钮在屏幕的底部︰

 - 如果还没有启用自动的用户供应，然后您会看到以下提示︰

        ![Confirm the assignment.][28]

 - 如果您已经启用了资源调配的自动化的用户，您将看到定义的用户都应该有什么样的队伍配置文件的提示。 新调配的用户应销售环境中出现几分钟的时间。

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] 如果您将资源调配给队伍**开发人员**环境，您必须为每个配置文件可用的许可证数量很少。 因此，最好是到有可用的 4999 许可证**联网免费用户**配置文件设置用户。

4. 若要测试您的单一登录设置，打开后盖在[https://myapps.microsoft.com](https://myapps.microsoft.com/)，然后登录到测试帐户，单击**队伍**。

##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
