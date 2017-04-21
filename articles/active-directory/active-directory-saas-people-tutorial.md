<properties
    pageTitle="教程︰ Azure Active Directory 集成与人 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和人之间。"
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


# <a name="tutorial-azure-active-directory-integration-with-people"></a>教程︰ Azure Active Directory 集成与人

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成人员。

与 Azure AD 集成人员提供了以下好处︰

- 您可以控制在 Azure AD 具有访问权的人员中
- 您可以使用户可以自动获取签名的 （单一登录） 的人其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置与人的 Azure AD 集成，您需要以下各项︰

- Azure 的订阅
- 在已启用的订阅了用户单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加人员
2. 配置和测试 Azure AD 单一登录


## <a name="adding-people-from-the-gallery"></a>从库中添加人员
若要配置集成到 Azure 的广告人，需要将人从库添加到托管的 SaaS 应用程序的列表。

**从库中添加人员，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 
 
    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**人**。

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. 在结果窗格中，选择的**人**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与人基于一个名为"Britta Simon"的测试用户。

若要配置和测试 Azure AD 单一登录的人，需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建用户测试用户](#creating-a-people-test-user)**的链接到她的 Azure 广告表示的人员具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录的用户应用程序中。



**要人 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 传统门户网站，**用户**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    [配置单一登录][6] 

2. 在**您想怎样来登录到的用户的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
 
    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_03.png) 

3. 在**配置应用程序设置**对话框页上，执行以下步骤，然后单击**下一步**:
 
    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_04.png) 

    一。 在**符号上 URL**文本框中，键入您的用户为登录到用户应用程序使用以下模式使用的 URL: **"https://\<公司名称\>.peoplehr.com/"**。 

    b。 如果您不知道您组织的 URL，请与联系人们支持团队通过[customerservices@peoplehr.com](mailto:customerservices@peoplehr.com)以获取它。  

    c。 在**标识符**文本框中，键入租户 URL。 

    d。 在**回复 URL**文本框中，键入下面的模式中的 URL:"**https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**"。

    电子。 单击**下一步**


4. 在**配置单一登录用户在**页面上，请执行以下步骤，然后单击**下一步**:

    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_05.png) 

    一。 单击**下载元数据**，然后保存该文件在您的计算机上。

    b。 单击**下一步**。


5. 若要获得 SSO 配置您的应用程序，您需要登录到您作为管理员的人租户。
    
    一。 在左侧菜单中，单击**设置**。
    
    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_001.png) 

    b。 单击**"公司"**。
    
    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_002.png) 

    c。 在**"上载单一登录上 SAML 元数据文件"**，请单击**浏览**上载下载元数据文件。

    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。
 
    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  

    ![Azure 的广告单登录][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。
在用户列表中，选择**Britta Simon**。

![创建 Azure AD 用户][20]


**若要创建在 Azure 广告人测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
 
    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-people-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-people-test-user"></a>一个人测试用户

本节的目的是创建人员称为 Britta Simon 的用户。 人们不支持在及时的资源调配使您需要联系的人的支持小组来手动创建用户。




### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 向用户授予她访问使用 Azure 单一登录。

![分配用户][200] 

**要赋予人们 Britta Simon，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**的人**。

    ![配置单一登录](./media/active-directory-saas-people-tutorial/tutorial_people_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。
人拼贴访问权限面板中的单击时，您应该获取自动签名对用户应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png
