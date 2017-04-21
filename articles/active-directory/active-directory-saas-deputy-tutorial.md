<properties
    pageTitle="教程︰ Azure Active Directory 集成与副 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和副之间。"
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>教程︰ Azure Active Directory 集成与副

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成副。

与 Azure AD 集成副提供了以下好处︰

- 您可以控制有权访问副 Azure AD 中
- 您可以使用户可以自动获取签名上到副 （单一登录） 其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

若要配置与副 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了副单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加副
2. 配置和测试 Azure AD 单一登录


## <a name="adding-deputy-from-the-gallery"></a>从库中添加副
若要配置副集成到 Azure 广告，您需要将副从库添加到托管的 SaaS 应用程序的列表。

**要从库添加副，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。
    
    ![应用程序][2]

4. 单击页面底部的**添加**。
    
    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**副**。

    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. 在结果面板中，选择**副**，，然后单击**完成**添加应用程序。

    ![在库中选择应用程序](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与副根据一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道副在 Azure AD 用户对应的用户。 换句话说，Azure AD 用户和相关的用户在副之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为副中的**用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录与副测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建副测试用户](#creating-a-deputy-test-user)**-中链接到她的 Azure 广告表示的副具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您启用 Azure AD 单一登录的传统门户和副应用程序中配置单一登录。

**要副 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**副**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样用户能够登录到副**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
    
    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. 在**配置应用程序设置**对话框页面上，如果您希望将应用程序配置中**IDP 启动模式**，请执行以下步骤，并单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    一。 在**标识符**文本框中，键入一个 URL 使用以下模式︰ `https://<your-subdomain>.<region>.deputy.com`。

    b。 在**回复 URL**文本框中，键入 URL 使用以下模式︰ `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`。

    c。 单击**下一步**。

4. 如果您想要**配置的应用程序设置**对话框页面上**SP 启动模式**中配置应用程序，然后单击**"显示高级设置 （可选）"**然后输入的**URL 上的符号**并单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    一。 在**符号上 URL**文本框中，键入 URL 使用以下模式︰ `https://<your-subdomain>.<region>.deputy.com`。

    b。 单击**下一步**。

    > [AZURE.NOTE] 副地区后缀为 opitional，或者它应该使用其中一种︰ au |na |欧盟 | 为 | la | af | | 企业 au | 企业 na | 企业欧盟 | 企业-为 |企业 la |企业 af |企业的

5. 在**配置单一登录处副**页上，执行以下步骤，然后单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    
6. 导航到下面的 URL: https://(your-subdomain).deputy.com/exec/config/system_config。 转到**安全设置**，单击**编辑**。

    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. 在 Azure 经典门户中，配置单一登录处副页上复制 SAML SSO URL 中。 

8. 在此**安全设置**页面中，请执行以下步骤。

    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    一。 实现**社会的登录**。

    b。 在记事本中打开您的 Base64 编码证书，将其内容复制到剪贴板，然后将其粘贴到**OpenSSL 证书**文本框。

    c。 在 SAM SSO URL 文本框中，键入`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d。 在 SAM SSO URL 文本框中，替换`<your subdomain>`与您的子域。

    电子。 在 SAM SSO URL 文本框中，替换`<saml sso url>`Azure 的传统门户网站从复制的 SAML SSO url。

    f。 单击**保存设置**。

9. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

10. 在**单一登录确认**页上，单击**完成**。  
    
    ![Azure 的广告单登录][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-deputy-test-user"></a>一个副测试用户

为了使 Azure AD 用户能够登录到副，他们必须被调配到副。 在副手的情况下设置为手动任务。

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到副公司网站。

2.  在顶部导航窗格中，单击**用户**。

    ![人员](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "人员")

3.  单击**添加用户**按钮，然后单击**添加一个人**。

    ![添加人员](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "添加人员")

4.  执行以下步骤，然后单击**保存并邀请**。

    ![新用户](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "新用户")

    一。 在**名称**文本框中，键入**Britta**和**Simon**。  

    b。 在**电子邮件**文本框中，键入您要提供的 Azure 的广告帐户的电子邮件地址。

    c。 在**可以在**文本框中，键入 bussniess 名称。

    d。 单击**保存并邀请**按钮。

    >[AZURE.NOTE]AAD 帐户持有者将收到一封电子邮件，点击链接以确认他们的帐户，再将变为活动状态。 您可以使用任何其他副用户帐户创建工具或 Api 由副来设置 AAD 用户帐户。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 授予副她访问使用 Azure 单一登录。
    
![分配用户][200]

**要为副 Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。
    
    ![分配用户][201]

2. 在应用程序列表中，选择**副**。
    
    ![配置单一登录](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. 在菜单上，单击**用户**。
    
    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。
    
    ![分配用户][205]

### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。
 
单击访问权限面板中的副平铺时，您应该获取自动签名对副应用程序。

## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
