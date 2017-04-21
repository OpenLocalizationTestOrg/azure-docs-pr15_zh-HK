<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Jive |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Jive 之间。"
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>教程︰ 与 Jive 的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Jive。

与 Azure AD 集成 Jive 提供了以下好处︰

- 您可以控制有权访问 Jive Azure AD 中
- 您可以使用户可以自动获取签名的 Jive （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Jive Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 Jive 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Jive
2. 配置和测试 Azure AD 单一登录


## <a name="adding-jive-from-the-gallery"></a>从库中添加 Jive
若要配置到 Azure AD Jive 的集成，您需要将 Jive 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 Jive，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]
2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Jive**。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. 在结果窗格中，选择**Jive**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，配置和测试 Azure AD 单一登录 Jive 基于一个名为"Britta Simon"的测试用户。

单一登录工作，Azure 的广告需要知道 Jive 对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的 Jive 在用户之间的链接关系需要建立。

此链接关系的建立在 Azure AD Jive**的用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录 Jive 与测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 Jive 测试用户](#creating-a-jive-test-user)**的链接到她的 Azure 广告表示的 Jive 在具有 Britta Simon 的副本。
4. **[配置用户供应](#configuring-user-provisioning)**的大纲如何启用用户供应的 Active Directory 用户帐户添加到 Jive。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
6. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录 Jive 应用程序中。

**要 Jive Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户， **Jive**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. **如何将用户注册到 Jive 要**在页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    一。 在**符号上 URL**文本框中，键入您登录到使用以下模式 Jive 应用程序用户所使用的 URL: **https://\<客户名称\>。 jivecustom.com**。
    
    b。 单击**下一步**
 
4. 在**配置单一登录 Jive 在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。


5. 登录到您作为管理员的 Jive 租户。

6. 在菜单上，单击"**Saml**"。

    ![在应用程序端配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    一。 在**Genaral**选项卡下，选择**启用**。

    b。 单击"**保存所有的 saml 设置**"按钮。

7. 导航到"**Idp 元数据**"选项卡。

    ![在应用程序端配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    一。 下载元数据 XML 文件，将内容复制，然后将其粘贴到文本框中**的身份提供程序 (IDP) 元数据**。

    b。 单击"**保存所有的 saml 设置**"按钮。 

8. 请转到"**用户属性映射**"选项卡。

    ![在应用程序端配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    一。 在**电子邮件**文本框中，复制并粘贴**邮件**值的属性名称。

    b。 在**名字**文本框中，复制并粘贴**givenname**值的属性名称。

    c。 在**姓氏**文本框中，复制并粘贴**姓**值的属性名称。
    
9. 在 Azure 广告门户中，选择一个登录配置进行确认，，然后单击**下一步**。
![Azure 的广告单登录][10]

10. 在**单一登录确认**页上，单击**完成**。  
  ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。


![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



###<a name="creating-a-jive-test-user"></a>创建 Jive 测试用户

在本节中，您创建用户调用 Jive Britta Simon。 请使用 Jive 支持团队 Jive 平台中添加用户。


###<a name="configuring-user-provisioning"></a>配置用户设置
  
本部分的目的是概述如何启用用户供应 Jive 到活动目录的用户帐户。  
作为此过程的一部分，您将需要提供将用户安全令牌，您需要从 Jive.com 请求。
  
下面的屏幕快照显示在 Azure AD 相关对话框的示例︰

![配置用户资源调配](./media/active-directory-saas-jive-tutorial/IC698794.png "配置用户资源调配")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  在 Azure 管理门户中， **Jive**应用程序集成在页上，单击**配置用户设置**以打开**配置用户设置**对话框。

2.  在**输入 Jive 启用自动用户提供的凭据**页提供以下配置设置︰

    1.  在**Jive 管理员用户名**文本框中，键入具有**系统管理员**配置文件中指定的 Jive.com Jive 帐户名。

    2.  在**Jive 管理员密码**文本框中，键入此帐户的密码。

    3.  在**Jive 租户 URL**文本框中，键入 Jive 租户 URL。

        >[AZURE.NOTE]Jive 租户 URL 为您的组织用于登录到 Jive 的 URL。  
        通常情况下，URL 具有以下格式︰ **www。\<组织\>。 jive.com**。

    4.  单击**验证**来验证您的配置。

    5.  单击**下一步**按钮以打开**确认**页。

3.  在**确认**页上，单击以保存配置的复选标记。
  
您现在可以创建一个测试帐户、 等待 10 分钟并验证帐户已被同步到 Jive.com。




### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Jive 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**要赋予 Jive Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Jive**。

    ![配置单一登录](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

Jive 拼贴访问权限面板中的单击时，您应该获取自动签名对 Jive 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
