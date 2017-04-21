<properties
    pageTitle="教程︰ Azure Active Directory 集成与按需 Proofpoint |Microsoft Azure"
    description="了解如何根据需要配置单一登录 Azure Active Directory 和 Proofpoint 之间。"
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
    ms.date="10/05/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>与按需 Proofpoint 的教程︰ Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Proofpoint 即需即装。

与 Azure AD 集成 Proofpoint 按需提供以下好处︰

- 您可以根据需要有权访问 Proofpoint Azure AD 中进行控制。
- 您可以启用自动获取登录点播 （单一登录，或 SSO） Proofpoint Azure 广告客户与您的用户。
- 您可以管理您的帐户在一个中央位置，Azure 的传统门户网站。

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置按需 Proofpoint Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- Proofpoint 需单个登录订阅


若要测试步骤在本教程中，请按照这些建议操作︰

- 不要使用您的生产环境，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以[获得一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库按需添加 Proofpoint。
2. 配置和测试 Azure AD 单一登录。


## <a name="add-proofpoint-on-demand-from-the-gallery"></a>从库中添加 Proofpoint 即需即装
要到 Azure AD 按需配置的 Proofpoint 集成，需要添加 Proofpoint 按需从库到托管的 SaaS 应用程序的列表。

1. 在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录图标][1]
2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部菜单**的应用程序**。

    ![应用程序菜单项][2]

4. 单击页面底部的**添加**。

    ![添加按钮][3]

5. 在**您想要执行**对话框中，单击**添加从库应用程序**。

    ![选择的库中添加应用程序][4]

6. 在搜索框中，键入**Proofpoint 即需即装**。

    ![框中键入"Proofpoint 上需"](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_01.png)

7. 在结果窗格中，选择**Proofpoint 即需即装**，，然后单击**完成**添加应用程序。



##  <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您将配置和测试 Azure AD 单一登录 Proofpoint 根据一个名为 Britta Simon 的测试用户需要与。

对于单一登录工作，Azure 广告需要知道对应的用户点播的 Proofpoint 在给用户在 Azure 的广告。 换句话说，您需要建立在按需 Proofpoint Azure AD 用户和相关的用户之间的链接关系。

要创建此链接关系 Azure AD 作为即需即装的 Proofpoint 中的**用户名**的值中指定的**用户名**的值。

要配置和 Azure AD 单一登录点播的 Proofpoint 与测试，完成以下过程︰

1. [Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)，以使用户可以使用此功能。
2. [创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)，Azure AD 单一登录 Britta Simon 与测试。
3. [创建要求测试用户 Proofpoint](#creating-a-proofpoint-ondemand-test-user)，可以按需链接到的她的 Azure 广告表示 Proofpoint 中具有 Britta Simon 的副本。
4. [Azure 广告测试用户分配](#assigning-the-azure-ad-test-user)、 启用 Britta Simon Azure AD 单一登录使用。
5. [测试单一登录](#testing-single-sign-on)，验证该配置有效。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您 Azure AD single sign-on 等传统门户中启用，并在您 Proofpoint 上要求的应用程序中配置单一登录。

1. 在经典的门户中，在**需求上的 Proofpoint**应用程序集成页上，单击**配置单一登录**，打开**配置单一登录**对话框中。

    !["配置单一登录"按钮][6]

2. 在**您想怎样用户能够登录到按需分配的 Proofpoint**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    !["Microsoft Azure 广告单登录"选项按钮](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_03.png)

3. 在**配置应用程序设置**页中，请执行以下步骤︰

    ![在"配置应用程序设置"页上的框填充](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_04.png)

    一。 在**号上的 URL**框中，键入的 URL 到您 Proofpoint 要求的应用程序上注册用户。 使用下面的模式︰ **https://\<主机名\>.pphosted.com/ppssamlsp_hostname**

    b。 在**标识符**框中，键入 URL 使用以下模式︰ **https://\<主机名 / >.pphosted.com/ppssamlsp**

    c。 在**答复 URL**框中，键入 URL 使用以下模式︰ **https://\<主机名 / >.pphosted.com:portnumber/v1/samlauth/samlconsumer**

    d。 单击**下一步**。

4. 在**配置单一登录 Proofpoint 按需在**页上，请执行以下步骤︰

    !["配置单一登录在 Proofpoint 即需即装"页上使用"下载证书"按钮](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_05.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。

5. 若要配置为您的应用程序的 SSO，联系上需要支持团队 Proofpoint 和向他们提供以下︰

    • 下载证书

    • 实体 ID

    • SAML SSO URL

6. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![复选框，确认您已配置单一登录][10]

7. 在**单一登录确认**页上，单击**完成**。  

    ![确认页][11]


### <a name="create-an-azure-ad-test-user"></a>创建 Azure 广告测试用户
在本节中，您将创建用传统门户网站命名 Britta Simon 的测试用户。


![测试用户在用户列表中][20]

1. 在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录图标](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_09.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单上，单击**用户**。

    ![用户菜单项](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png)

4. 若要打开**添加用户**对话框的底部工具栏上单击**添加用户**。

    ![添加用户按钮](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png)

5. **请告诉我们有关此用户**在页上，请执行以下步骤︰ ![框填写"告诉我们有关该用户"页](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_05.png)

    一。 在**类型的用户**框中，选择**您的组织中的新用户**。

    b。 在**用户名**框中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**页中，请执行以下步骤︰![框填写的"用户配置文件"页](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_06.png)

    一。 在**第一个**框中，键入**Britta**。  

    b。 在**姓氏**框中，键入**Simon**。

    c。 在**显示名称**框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**页上，单击**创建**。

    ![创建一个临时密码按钮](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_07.png)

8. 在**获得临时密码**页上，请执行以下步骤︰

    !["获得暂时密码"的密码信息页](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_08.png)

    一。 记下在**新密码**框中的值。

    b。 单击**完成**。   



### <a name="create-a-proofpoint-on-demand-test-user"></a>要求测试用户创建 Proofpoint

在本节中，您创建用户调用 Britta Simon Proofpoint 即需即装。 请与 Proofpoint 合作需求支持小组要求平台 Proofpoint 中添加用户。


### <a name="assign-the-azure-ad-test-user"></a>Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Proofpoint 根据需要授予她访问使用 Azure 单一登录。

![用户信息，显示通过直接方法启用了访问权限][200]

1. 在经典的门户中，在目录视图中，打开应用程序视图的顶部的菜单上单击**应用程序**。

    ![应用程序菜单项][201]

2. 在应用程序的列表中，选择**按需 Proofpoint**。

    ![Proofpoint 按需所选的应用程序的列表](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_50.png)

3. 顶部的菜单上，单击**用户**。

    ![用户菜单项][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部的工具栏上，单击**指派**。

    ![分派按钮][205]


### <a name="test-single-sign-on"></a>测试单一登录

在本节中，您测试 Azure AD 单一登录配置使用访问权限面板。

当您单击**按需 Proofpoint**图块后盖上的时，您应自动签名到您需要应用程序上的 Proofpoint。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_205.png
