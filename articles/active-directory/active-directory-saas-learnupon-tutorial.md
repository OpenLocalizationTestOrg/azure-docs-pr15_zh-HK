<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Novatus |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 LearnUpon 之间。"
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>教程︰ 与 LearnUpon 的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 LearnUpon。  
与 Azure AD 集成 LearnUpon 提供了以下好处︰

- 您可以控制有权访问 LearnUpon Azure AD 中
- 您可以使用户可以自动获取签名的 LearnUpon （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure Active Directory 经典 


如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 LearnUpon Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 LearnUpon 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 LearnUpon
2. 配置和测试 Azure AD 单一登录


## <a name="adding-learnupon-from-the-gallery"></a>从库中添加 LearnUpon
若要配置 LearnUpon 集成到 Azure 的广告，您需要将 LearnUpon 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 LearnUpon，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**LearnUpon**。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. 在结果窗格中，选择**LearnUpon**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和测试 Azure AD 单一登录 LearnUpon 基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 LearnUpon 在 Azure AD 用户对应的用户。 换句话说，Azure AD 用户和 LearnUpon 中的相关的用户之间的链接关系需要建立。  
此链接关系的建立在 Azure 广告作为**用户名**LearnUpon 中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录使用 LearnUpon，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 LearnUpon 测试用户](#creating-a-learnupon-test-user)**的链接到她的 Azure 广告表示的 LearnUpon 中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录，LearnUpon 应用程序中。



**以 LearnUpon Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**LearnUpon**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**您想怎样来登录到 LearnUpon 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤:。

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    一。 在**回复 URL**文本框中，键入断言使用者服务 URL 使用以下模式︰`https://\<companyname\>.learnupon.com/saml/consumer`

    b。 单击**下一步**。 


4. 在**配置单一登录 LearnUpon 在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。 我们需要此证书和元数据的 Url （实体 ID、 SSO URL 中的符号和符号出 URL） LearnUpon 侧设置 SSO。

    b。 单击**下一步**。




1. 在中打开另一个浏览器实例和登录 LearnUpon 使用管理员帐户。 

1. 单击**设置**选项卡。

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. **单一登录的 SAML**，单击，然后单击要配置 SAML 设置的**常规设置**。

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. 在**常规设置**部分中，执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    一。 选择**启用**。

    b。 为**版本**，选择**2.0**。

    c。 为**跳过条件**，选择**否**。

    d。 **SAML 令牌张贴参数名称**文本框中，在上面的指示对 SAML 使用者 URL 请求发布参数的名称类型包含 SAML 断言以验证和身份验证-例如**SAMLResponse**。

    电子。 在**名称标识符的格式**文本框中，键入一个值以指示在 SAML 断言的用户标识符 （电子邮件地址） 所在-例如**urn: oasis︰ 姓名︰ tc: SAML:1.1:nameid 的电子邮件地址格式︰**。

    f。 在**标识提供程序位置**文本框中，键入一个值以指示在用户发送到如果他们从 Azure 的经典门户登录屏幕您上载的图标上单击。

    g.in Azure 的传统门户网站，复制**单个 Sign-Out 服务 URL**，，然后将其粘贴到**注销 URL** textbos。

    h。 **管理手指打印**，请单击，然后上载您已下载的证书的指纹。 


1. 单击**用户设置**，然后执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    一。 在**名字标识符格式**文本框中，键入值，告诉我们在 SAML 断言的用户名字所在-例如︰ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ givenname**。

    b。 在**最后一个名称标识符的格式**文本框中，键入值，告诉我们在 SAML 断言用户姓氏所在-例如︰ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ 姓**。


6. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  

    ![Azure 的广告单登录][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-learnupon-test-user"></a>LearnUpon 测试用户

本节的目的是创建一个名 Britta Simon LearnUpon 中的用户。 LearnUpon 支持中及时的资源调配，这是默认情况下启用。

没有为您在本部分中的操作项。 在试图访问 LearnUpon，如果它尚不存在，将创建新用户。 [Azure 的广告单登录配置](#configuring-azure-ad-single-single-sign-on)。

> [AZURE.NOTE] 如果您需要手动创建用户，您需要与 LearnUpon 的支持团队联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon LearnUpon 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon LearnUpon，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**LearnUpon**。

    ![配置单一登录](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
单击访问权限面板中的 LearnUpon 平铺时，您应该获取自动签名对 LearnUpon 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
