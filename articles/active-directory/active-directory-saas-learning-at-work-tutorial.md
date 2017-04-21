<properties
    pageTitle="教程︰ 与在工作学习 Azure Active Directory 集成 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和学习之间在工作。"
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


# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a>与在工作学习的教程︰ Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成在工作学习。

与 Azure AD 集成在工作学习提供了以下好处︰

- 您可以在 Azure AD 上班时有权访问学习控制
- 您可以使用户可以自动获取签名的 （单一登录） 的工作学习到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置在工作学习 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在工作 （沙巴云） 单点登录启用订阅学习


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加学习在工作
2. 配置和测试 Azure AD 单一登录


## <a name="adding-learning-at-work-from-the-gallery"></a>从库中添加学习在工作
上班时到 Azure AD 配置集成在一起学习，您需要学习在工作从库向列表中添加托管的 SaaS 应用程序。

**要从库中添加在工作学习，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]
2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**在工作中学习**。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_01.png)

7. 在结果窗格中，选择**在工作学习**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您将配置和测试 Azure AD 单一登录与在基于一个名为"Britta Simon"的测试用户的工作学习。

单一登录工作，需要知道在工作学习中的对应的用户是什么给用户在 Azure AD Azure 的广告。 换句话说，Azure AD 用户和在工作学习中的相关的用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为在工作学习中的**用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录在工作的学习与测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建工作学习测试用户](#creating-a-predictix-price-reporting-test-user)**的链接到她的 Azure 广告表示的工作学习中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您 Azure AD single sign-on 等传统门户中启用并配置单一登录您学习在工作应用程序中。


**要在工作学习 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在传统的门户网站，**在工作学习**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样用户能够登录到在工作学习**上**Azure AD Single Sign-on**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_04.png) 

    一。 在**符号上 URL**文本框中，键入您的用户登录到您的工作应用程序使用以下模式在学习与使用的 URL:`https://\<company name\>.sabacloud.com/Saba/Web/<company code>`
    
    b。 在**标识符**文本框中，键入 URL 使用以下模式: https://<company name>.sabacloud.com/Saba/SAML/sso/alias/<company name>``

    c。 单击**下一步**
 
4. 在**配置单一登录学习在工作在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_05.png)

    一。 单击**下载元数据**，然后保存该文件在您的计算机上。

    b。 单击**下一步**。


5. 若要配置为您的应用程序的 SSO，工作 （沙巴云） 支持团队联系学习和向他们提供以下︰

    • 下载元数据

    •**颁发者 Url**

    • **SAML SSO URL**

    •**一个注销服务 URL**

6. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。


![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-an-learning-at-work-test-user"></a>在工作测试用户创建学习

在本节中，您创建用户调用 Britta Simon 在工作学习中。 请使用学习工作支持团队工作平台在学习中添加用户。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon 在工作学习中授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要学习在工作分配 Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**在工作学习**。

    ![配置单一登录](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中工作平铺在学习时，您应该获取自动签名上到工作应用程序在您学习。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_205.png
