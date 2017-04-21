<properties
    pageTitle="教程︰ 与 Marketo 的 Azure Active Directory 集成 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Marketo 之间。"
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
    ms.date="09/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>教程︰ 与 Marketo 的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Marketo。

与 Azure AD 集成 Marketo 提供了以下好处︰

- 您可以控制有权访问 Marketo Azure AD 中
- 您可以使用户可以自动获取签名上 Marketo （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Marketo Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 Marketo 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Marketo
2. 配置和测试 Azure AD 单一登录


## <a name="adding-marketo-from-the-gallery"></a>从库中添加 Marketo
若要配置 Marketo 集成到 Azure 的广告，您需要将 Marketo 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 Marketo，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Marketo**。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_01.png)

7. 在结果窗格中，选择**Marketo**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，配置和测试 Azure AD 单一登录 Marketo 基于一个名为"Britta Simon"的测试用户。

单一登录工作，Azure 广告需要知道 Marketo 对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的 Marketo 中用户之间的链接关系需要建立。

此链接关系的建立在 Azure AD Marketo**的用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录 Marketo 的测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 Marketo 测试用户](#creating-a-predictix-price-reporting-test-user)**-中，链接到她的 Azure 广告表示 Marketo 具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您启用 Azure AD 单一登录的传统门户和 Marketo 应用程序中配置单一登录。


**要 Marketo Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**Marketo**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样来登录到 Marketo 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_04.png) 

    一。 在**标识符**文本框中，键入 URL 使用以下模式︰`https://saml.marketo.com/sp`
    
    b。 在**回复 URL**文本框中，键入 URL 使用以下模式︰`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    c。 单击**下一步**
 
4. 在**配置单一登录 Marketo 在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_05.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。


5. 获得您应用程序 Munchkin Id、 Marketo 使用管理凭据登录并执行下列操作︰

    一。 对 Marketo 应用程序使用管理凭据登录。

    b。 单击顶部导航窗格上的管理按钮。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

    c。 导航到集成菜单，然后单击 Munchkin 链接
    
    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)

    d。 复制 Munchkin Id 显示在屏幕上并完成 Azure AD 配置向导在您回复的 URL。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png)

6.  若要配置 SSO 应用程序中，请执行以下步骤︰

    一。 对 Marketo 应用程序使用管理凭据登录。

    b。 单击顶部导航窗格上的管理按钮。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

    c。 导航到集成菜单上，单击在单一登录

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 

    d。 要启用 SAML 设置，请单击编辑按钮上
    
    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 

    电子。 **启用**单一登录设置

    f。 输入复制从 Azure AD 配置向导颁发者 ID。

    g。 在实体 ID 的文本框中输入的 URL 作为**http://saml.marketo.com/sp**

    h。 选择用户 ID 位置**名称标识符元素**作为

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)

    > [AZURE.NOTE] 如果您的用户标识符不是 UPN 值，然后在属性选项卡中的值更改。
     
    我。 上传您从 Azure AD 配置向导已下载的证书。 保存设置。

    j。 编辑的重定向页面设置

    k。 从 Azure AD 配置向导中的**登录 URL**文本框复制登录 URL。

    l。 将注销 URL 复制从 Azure AD 配置向导的**注销 URL**文本框中。

    米。 错误 URL 中复制您 Marketo 实例的 URL 并单击 Save 按钮以保存设置。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

7. 要启用 SSO 用户，请完成以下操作︰

    一。 对 Marketo 应用程序使用管理凭据登录。

    b。 单击顶部导航窗格上的**管理**按钮。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

    c。 导航到**安全**菜单，单击**登录**设置 

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)

    d。 检查**需要 SSO**选项并保存设置。
    
    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

8. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

8. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。


![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-marketo-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-an-marketo-test-user"></a>创建一个 Marketo 的测试用户

在本节中，您创建称为 Marketo Britta Simon 的用户。 请按照以下步骤创建用户 Marketo 的平台中。

1. 对 Marketo 应用程序使用管理凭据登录。

2. 单击顶部导航窗格上的**管理**按钮。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. 导航到**安全**菜单，单击**用户**和角色

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. 单击用户选项卡上的**邀请新用户**链接

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. 在邀请新用户向导填写以下信息

    一。 在文本框中输入用户的**电子邮件**地址

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)

    b。 在文本框中输入**名字**
    
    c。 在文本框中输入**姓氏**

    d。 单击下一步

6. 在**权限**选项卡中选择的用户角色，然后单击下一步

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)

7. 单击发送按钮发送用户邀请

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. 用户将会收到电子邮件通知，必须单击的链接，并更改以激活该帐户的密码。 


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Marketo 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**要赋予 Marketo Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Marketo**。

    ![配置单一登录](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

Marketo 拼贴访问权限面板中的单击时，您应该获取自动签名对 Marketo 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_205.png
