<properties
    pageTitle="教程︰ Azure Active Directory 集成与 8 x 8 虚拟办公室 |Microsoft Azure"
    description="了解如何配置单一登录 Azure 的活动目录和 8 x 8 虚拟办公室之间。"
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>教程︰ Azure Active Directory 集成与 8 x 8 虚拟办公室

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 8 x 8 虚拟办公室。

集成 8 x 8 Azure 广告与虚拟办公室为您提供了以下好处︰

- 您可以控制有权访问 8 x 8 虚拟办公室 Azure AD 中
- 您可以使用户可以自动获取签名的 8 x 8 （单一登录） 的虚拟办公室其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

若要配置与 8 x 8 虚拟办公室 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 8 x 8 虚拟办公室单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Microsoft Azure AD 单一登录。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 8 x 8 虚拟办公室
2. 配置和测试 Microsoft Azure AD 单一登录


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>从库中添加 8 x 8 虚拟办公室
若要配置集成到 Azure 广告 8 x 8 虚拟办公室，您需要将 8 x 8 虚拟办公室从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 8 x 8 虚拟办公室，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。
    
    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**8 x 8 虚拟办公室**。

    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. 在结果窗格中，选择**8 x 8 虚拟办公室**，然后单击**完成**添加应用程序。

    ![在库中选择应用程序](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>配置和测试 Microsoft Azure AD 单一登录
本部分的目的是向您展示如何配置和测试 Microsoft Azure AD 单一登录与 8 x 8 虚拟办公室根据一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 8 x 8 到 Azure AD 中的某个用户的虚拟办公室是在对应的用户。 换句话说，Azure AD 用户和 8 x 8 中相关的用户之间的链接关系虚拟办公室需要建立。

此链接关系的建立在 Azure AD 为 8 x 8 虚拟办公室中的**用户名**的值指定的**用户名**的值。

要配置和测试 Microsoft Azure AD 单一登录与 8 x 8 虚拟办公室，您需要完成以下构造块︰

1. **[配置 Microsoft Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Microsoft Azure AD Single Sign-on Britta Simon 与测试。
3. **[8 x 8 虚拟办公室测试用户](#creating-a-8x8-virtual-office-test-user)**-8 x 8 虚拟办公室，链接到她的 Azure 广告表示具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon 使用 Microsoft Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>配置 Microsoft Azure AD 单一登录

在本节中，您启用 Microsoft Azure AD 单一登录中经典的门户和 8 x 8 虚拟办公室应用程序中配置单一登录。

**与 8 x 8 虚拟办公室配置 Microsoft Azure AD 单一登录，请执行以下步骤︰**

1. 在经典的门户，在**8x8 虚拟 Office**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. **如何将用户登录到 8 x 8 虚拟办公室要**在页上，选择**Microsoft Azure AD 单一登录**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤，并单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    一。 在**回复 URL**文本框中，键入︰`https://sso.8x8.com/saml2`

    b。 单击**下一步**

4. 在**8x8 虚拟办公室的单一登录配置**页上，执行以下步骤，然后单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。

5. 登录到您作为管理员的 8 x 8 虚拟办公室租户。
6. 选择应用程序面板上的**虚拟办公室帐户经理**。

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. 选择**业务**帐户来管理并单击**登录**按钮。

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. 单击菜单中的**帐户**选项卡。

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. 在帐户列表中，单击**单一登录**。

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. 在身份验证方法下选择**Signle 登录**并单击**SAML**。

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. 将**SAML SSO URL**、**单唱歌出服务 URL**和**颁发者 URL**从 Azure AD 复制到**登录 URL**、**注销 URL**和**颁发者 URL**中 8 x 8 虚拟办公室。 

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![在应用程序端配置](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. 单击**浏览器**按钮来上载从 Azure AD 下载的证书。

13. 单击**保存**按钮。

14. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

15. 在**单一登录确认**页上，单击**完成**。  

    ![Azure 的广告单登录][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。
    
![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-8x8-virtual-office-test-user"></a>8 x 8 虚拟办公室测试用户

本节的目的是创建一个名为 Britta Simon 8 x 8 虚拟办公室用户。 8 x 8 虚拟办公室支持中及时的资源调配，这是默认情况下启用。

没有为您在本部分中的操作项。 期间有人试图访问 8 x 8 虚拟办公室，如果它尚不存在，将创建新用户。 

> [AZURE.NOTE] 如果您需要手动创建用户，您需要与 8 x 8 虚拟办公室支持团队联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 8 x 8 虚拟办公室授予她访问使用 Azure 单一登录。
    
![分配用户][200]

**要 Britta Simon 为 8 x 8 虚拟办公室，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201]

2. 在应用程序列表中，选择**8 x 8 虚拟办公室**。

    ![配置单一登录](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. 在菜单上，单击**用户**。
    
    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试使用接入面板单一登录配置 Microsoft Azure 的广告。

8 x 8 虚拟办公室拼访问权限面板中的单击时，您应该获取自动签名等 8 x 8 虚拟办公室应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
