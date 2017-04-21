<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Zendesk |Microsoft Azure" 
    description="了解如何使用 Zendesk Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>教程︰ 与 Zendesk 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Zendesk 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Zendesk 租户
  
后学完本教程，您已分配给 Zendesk Azure AD 用户将能够到 Zendesk 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Zendesk
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-zendesk-tutorial/IC773083.png "方案")

##<a name="enabling-the-application-integration-for-zendesk"></a>启用应用程序集成为 Zendesk
  
本部分的目的是概述如何启用应用程序集成为 Zendesk。

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>若要启用应用程序集成为 Zendesk，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-zendesk-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zendesk-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zendesk-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-zendesk-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Zendesk**。

    ![应用程序库](./media/active-directory-saas-zendesk-tutorial/IC773084.png "应用程序库")

7.  在结果窗格中，选择**Zendesk**，，然后单击**完成**添加应用程序。

    ![Zendesk](./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Zendesk。  
Zendesk 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Zendesk**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![单一登录](./media/active-directory-saas-zendesk-tutorial/IC773086.png "单一登录")

2.  在**您想怎样来登录到 Zendesk 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-zendesk-tutorial/IC773087.png "配置单一登录")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-zendesk-tutorial/IC773088.png "配置应用程序的 URL")
  
    一。 在**Zendesk 号中 URL**文本框中，键入您的 URL 使用以下模式︰`https://<tenant-name>.zendesk.com`

    b。 单击**下一步**。



4.  在**配置单一登录 Zendesk 在**页上，单击**下载证书**，然后保存您的 compiter 在本地证书文件。

    ![配置单一登录](./media/active-directory-saas-zendesk-tutorial/IC777534.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Zendesk 公司网站。

6.  单击**管理**。

7.  在左侧的导航窗格中，单击**设置**，然后单击**安全**。

    ![安全](./media/active-directory-saas-zendesk-tutorial/IC773089.png "安全")

8.  在**安全**页中，单击**管理和代理**选项卡。

9.  选择**单一登录 (SSO) 和 SAML**，然后选择**SAML**。

10. 在 Azure 广告门户中，在**配置单一登录 Zendesk 在**页上，将**SAML SSO URL**值，复制，然后将其粘贴到**SAML SSO URL**文本框。

11. 在 Azure 广告门户中，在**配置单一登录 Zendesk 在**页上，**远程注销 URL**值，复制，然后将其粘贴到**远程注销 URL**文本框。

    ![单一登录](./media/active-directory-saas-zendesk-tutorial/IC773090.png "单一登录")

12. 从导出的证书，复制的**指纹**值，然后将其粘贴到**证书指纹**文本框。

    >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

13. 单击**保存**。

14. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zendesk-tutorial/IC773093.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到**Zendesk**，他们必须到**Zendesk**配置。  
对于**Zendesk**，资源调配是手动任务。

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>若要设置到 Zendesk 的用户帐户，请执行以下步骤︰

1.  登录到**Zendesk**租户。

2.  选择**客户列表**选项卡。

3.  选择**用户**选项卡，然后单击**添加**。

    ![添加用户](./media/active-directory-saas-zendesk-tutorial/IC773632.png "添加用户")

4.  键入电子邮件地址的现有 Azure 广告所需帐户设置，然后单击**保存**。

    ![新用户](./media/active-directory-saas-zendesk-tutorial/IC773633.png "新用户")

>[AZURE.NOTE] 您可以使用任何其他 Zendesk 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Zendesk 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>要将用户分配到 Zendesk 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Zendesk**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zendesk-tutorial/IC773094.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zendesk-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
