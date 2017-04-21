<properties
    pageTitle="教程︰ Azure Active Directory 集成 Skydesk 电子邮件 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Skydesk 电子邮件之间。"
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>教程︰ Azure Active Directory 集成 Skydesk 电子邮件

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 Skydesk 电子邮件。

与 Azure AD 集成 Skydesk 电子邮件提供了以下好处︰

- 您可以控制有权访问电子邮件 Skydesk Azure AD 中
- 您可以其 Azure 的广告帐户使用户可以自动获取签名对 Skydesk （单一登录） 的电子邮件
- 您可以管理您的帐户在一个中心位置的 Azure Active Directory 经典门户

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置电子邮件 Skydesk Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了电子邮件 Skydesk 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Skydesk 电子邮件
2. 配置和测试 Azure AD 单一登录


## <a name="adding-skydesk-email-from-the-gallery"></a>从库中添加 Skydesk 电子邮件
若要配置 Skydesk 电子邮件集成到 Azure 的广告，您需要将 Skydesk 电子邮件从库添加到托管的 SaaS 应用程序的列表。

**要从库中添加 Skydesk 电子邮件，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Skydesk 电子邮件**。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. 在结果窗格中，选择了**Skydesk 电子邮件**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本节的目的是要向您展示如何配置和 Azure AD 单一登录 Skydesk 电子测试基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道在 Azure AD 用户 Skydesk 电子邮件中对应的用户。 换句话说，Azure AD 用户和 Skydesk 电子邮件相关的用户之间的链接关系需要建立。

此链接关系的建立在 Azure AD Skydesk 电子邮件中的**用户名**的值中指定的**用户名**的值。

要配置和测试 Azure AD 单一登录 Skydesk 电子邮件，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建一个 Skydesk 电子邮件测试用户](#creating-a-Skydesk-Email-test-user)**-Skydesk 电子邮件链接到她的 Azure 广告表示具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录 Skydesk 电子邮件应用程序中。



**要配置 Azure AD 单一登录 Skydesk 电子邮件，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**Skydesk 电子邮件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**您想怎样用户能够登录到 Skydesk 电子邮件**页面上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰
 
    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    一。 在符号上 URL 文本框中，键入您登录到 Skydesk 电子邮件应用程序使用以下模式用户使用的 URL: **"https://mail.skydesk.jp/portal/\<公司名称\>"**。

    b。 单击**下一步**。


4. 在**配置单一登录在 Skydesk 电子邮件**页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。


5. 若要启用 SSO **Skydesk 电子邮件**中的，请执行以下步骤︰
 
    一。 登录到您的 Skydesk 电子邮件帐户以管理员的身份。

    b。 在顶部菜单中，单击设置，然后选择组织 

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c。 从左面板中单击的域上。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d。 单击添加域。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    电子。 输入您的域名，然后验证域。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f。 在左面板中单击**SAML**验证

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. 在**SAML 验证**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] 若要使用基于 SAML 的身份验证，则应该**验证域**或**门户 URL**安装程序。 您可以设置门户网站具有唯一名称的 URL。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    一。 在 Azure AD 经典门户中， **SAML SSO URL**值，复制，然后将其粘贴到**登录 URL**文本框。

    b。 在 Azure AD 经典门户中，复制的**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**注销**URL 文本框。

    c。 **更改密码 URL**是可选因此将其留空。

    d。 **从文件中获取键**选择您下载的 Skydesk 电子邮件证书，请单击，然后单击**打开**上载证书。

    电子。 作为**算法**，选择**RSA**。

    f。 单击**确定**以保存所做的更改。


7. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

8. 在**单一登录确认**页上，单击**完成**。  
  
    ![Azure 的广告单登录][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-skydesk-email-test-user"></a>创建一个 Skydesk 电子邮件测试用户

在本节中，您创建一个名 Britta Simon Skydesk 电子邮件的用户。

一。 Skydesk 通过电子邮件在左面板中单击的**用户访问**，然后输入您的用户名。 

![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] 如果您需要创建批量用户，您需要与 Skydesk 电子邮件支持团队联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon Skydesk 电子邮件授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon Skydesk 电子邮件，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。
 
    ![分配用户][201] 

2. 在应用程序列表中，选择**Skydesk 电子邮件**。

    ![配置单一登录](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中的 Skydesk 电子邮件拼贴时，您应该获取自动签名对 Skydesk 电子邮件应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
