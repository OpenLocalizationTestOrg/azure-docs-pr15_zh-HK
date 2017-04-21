<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Qlik 意义上企业 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Qlik 检测企业之间。"
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>教程︰ 与 Qlik 检测企业的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Qlik 意义上的企业。

与 Azure AD 集成 Qlik 意义上企业提供以下好处︰

- 您可以控制有权访问 Qlik 意义上企业 Azure AD 中
- 您可以启用用户自动获得签名的 Qlik （单一登录） 的意义上企业到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Qlik 意义上企业 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- Qlik 检测企业单一登录上已启用的订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Qlik 意义上企业
2. 配置和测试 Azure AD 单一登录


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>从库中添加 Qlik 意义上企业
若要配置集成到 Azure AD Qlik 意义上的企业，需要将 Qlik 意义上企业从库添加到托管的 SaaS 应用程序的列表。

**若要从库添加 Qlik 意义上的企业，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]
2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Qlik 意义上的企业**。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. 在结果窗格中，选择**Qlik 意义上的企业**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，配置和测试 Azure AD 单一登录 Qlik 意义上企业基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 Qlik 意义上企业中对应的用户是什么给用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的用户在 Qlik 意义上的企业之间的链接关系需要建立。

此链接关系的建立在 Azure AD Qlik 意义上企业中的**用户名**的值为指定的**用户名**的值。

要配置和 Azure AD 单一登录 Qlik 意义上企业与测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 Qlik 意义上企业测试用户](#creating-a-qliksense-enterprise-test-user)**的链接到她的 Azure 广告表示的 Qlik 检测企业中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您 Azure AD single sign-on 等传统门户中启用并配置单一登录 Qlik 意义上企业应用程序中。


**Qlik 检测企业 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**Qlik 意义上企业**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. **您想怎样用户能够登录到 Qlik 意义上企业**在页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    一。 在**符号上 URL**文本框中，键入您登录到 Qlik 意义上企业应用程序使用以下模式用户使用的 URL: **https://\<Qlik 意义完全限定主机名\>: 443 / < 虚拟代理前缀\>/samlauthn/**。
    
    > [AZURE.NOTE] 请注意此 URI 的末尾尾部反斜杠。  它是必需的。

    b。 单击**下一步**
 
4. 在**配置单一登录 Qlik 意义上企业在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    一。 单击**下载元数据**，然后保存该文件在您的计算机上。  做好准备，以便上载到 Qlik 检测服务器之前编辑此元数据文件。

    b。 单击**下一步**。

5. 准备联合元数据 XML 文件，以便您可以到 Qlik 检测服务器上载的。

    > [AZURE.NOTE] 之前向 Qlik 检测服务器上载 IdP 元数据，该文件需要编辑删除信息，以确保正确操作之间 Azure 的广告和 Qlik 检测服务器。

    ![QlikSense][qs24]

    一。 打开一个文本编辑器中从 Azure 下载的 FederationMetaData.xml 文件。

    b。 搜索值**RoleDescriptor**。  将四个条目 （两个成对的开始和结束元素标记）。

    c。 RoleDescriptor 的标记和所有的信息在此期间从文件中删除。

    d。 保存该文件并使其附近在本文档后面的使用。

6. 可以创建虚拟代理配置的用户定位到 Qlik 检测 Qlik 管理控制台 (QMC)。

7. 在 QMC，单击虚拟代理菜单项。

    ![QlikSense][qs6] 

8. 在屏幕的底部，单击创建新的按钮。

    ![QlikSense][qs7]

9. 显示虚拟代理编辑屏幕。  在屏幕的右侧是使可见的配置选项的菜单。

    ![QlikSense][qs9]

10. 检查标识菜单选项后，输入的 Azure 虚拟代理配置的标识信息。

    ![QlikSense][qs8]  
    
    一。 说明字段是虚拟代理配置的友好名称。  有关说明中输入一个值。
    
    b。 前缀字段标识虚拟代理终结点连接到使用 Azure AD 单一登录 Qlik 检测。  输入该虚拟代理服务器的唯一的前缀名称。

    c。 会话不活动超时 （分钟） 是通过此虚拟代理服务器的连接超时。

    d。 该会话 cookie 的标头名称是该 cookie 的名称存储为用户身份验证成功后收到 Qlik 检测会话的会话标识符。  此名称必须是唯一的。

11. 单击身份验证菜单选项，以使其可见。  身份验证屏幕会出现。

    ![QlikSense][qs10]

    一。 **匿名访问模式**下拉列表确定是否匿名用户可以通过虚拟代理访问 Qlik 检测。  默认选项是没有匿名用户。

    b。 **身份验证方法**下拉列表确定将使用的身份验证方案的虚拟代理。  从下拉列表中选择 SAML。  更多的选项将显示结果。

    c。 在**SAML 主机的 URI 字段**中，输入主机名用户将输入此 SAML 虚拟代理服务器通过访问 Qlik 检测。  主机名是 Qlik 检测服务器的 uri。

    d。 在**SAML 实体 ID**，请输入 SAML 主机的 URI 字段中输入相同的值。

    电子。 **SAML IdP 元数据**是**从 Azure AD 配置编辑联合元数据**部分中前面部分编辑的文件。  **IdP 元数据中上, 传之前需要编辑该文件**中删除信息，以确保正确操作之间 Azure AD 和 Qlik 检测服务器。  **请参考上面的说明，如果该文件尚未对其进行编辑。**  如果在编辑文件单击浏览按钮并选择要将其上载到虚拟代理配置的编辑元数据文件。

    f。 输入属性名称或 SAML 属性表示**用户 Id** Azure 的 AD 架构引用将发送给 Qlik 检测服务器。  在 Azure 应用程序屏幕公告配置提供了架构参考信息。  若要使用名称属性，**输入 http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**。

    g。 到 Azure AD Qlik 检测服务器身份验证时将被附加到用户的**用户目录**中输入的值。  硬编码的值必须用**方括号 []**括起来。  若要使用在 Azure AD SAML 断言中发送的属性，输入此文本框中**不带**方括号中特性的名称。

    h。 **SAML 签名算法**设置服务提供程序 （在本例中 Qlik 检测服务器） 证书签名的虚拟代理配置。  如果 Qlik 检测服务器使用受信任的证书，使用 Microsoft 增强 RSA 和 AES 加密提供程序生成，变为**sha-256**SAML 签名算法。

    我。 SAML 属性映射节允许其他属性，如组发给安全规则中使用 Qlik 检测。

12. 在负载平衡以使其可见的菜单选项，请单击。  在负载平衡屏幕会出现。

    ![QlikSense][qs11]

13. 单击添加新服务器节点按钮，选择引擎节点或 Qlik 传感节点将发送到会话负载平衡的目的，并单击添加按钮。

    ![QlikSense][qs12]

14. 单击高级菜单选项，以使其可见。 高级屏幕会出现。

    ![QlikSense][qs13]

    一。 主机白名单标识主机名连接到 Qlik 检测服务器时被接受。  **请输入主机名连接到 Qlik 检测服务器时，将指定的用户。** 主机名是没有 https:// SAML 主机 uri 的值相同。

15. 单击应用按钮。

    ![QlikSense][qs14]

16. 单击确定以接受警告消息，指出链接到虚拟代理服务器代理服务器将重新启动。

    ![QlikSense][qs15]

17. 在屏幕的右侧，显示关联项目菜单。  单击代理服务器菜单选项。

    ![QlikSense][qs16]

18. 显示代理服务器屏幕。  单击链接按钮底部链接到虚拟代理服务器的代理服务器。

    ![QlikSense][qs17]

19. 选择代理节点，将支持此虚拟代理连接，请单击链接按钮。  链接之后, 将在相关联的代理下，列出代理。

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. 大约过了 5 到 10 秒，将显示刷新 QMC 消息。  单击刷新 QMC 按钮。

    ![QlikSense][qs20]

21. 当 QMC 刷新时，请单击虚拟代理菜单项。 新的 SAML 虚拟代理条目是在屏幕上表中列出。  单击虚拟代理项。

    ![QlikSense][qs51]

22. 在屏幕的底部，将会激活的下载 SP 元数据按钮。  单击下载 SP 元数据按钮将元数据保存到一个文件。

    ![QlikSense][qs52]

23. 打开该 sp 的元数据文件。  观察**entityID**条目和**AssertionConsumerService**项。  这些值是等于**标识符**和**登录 URL**在 Azure AD 应用程序配置。 如果它们不匹配然后您应该替换它们在 Azure AD 应用程序配置向导。

    ![QlikSense][qs53]

24. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

25. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。


![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>创建一个 Qlik 意义上企业测试用户

在本节中，您创建一个名 Britta Simon Qlik 意义上企业中的用户。 请使用 Qlik 意义上企业支持小组以 Qlik 意义上企业平台中添加的用户。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Qlik 意义上企业授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon Qlik 意义上的企业，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Qlik 意义上的企业**。

    ![配置单一登录](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


## <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中的 Qlik 检测企业拼贴时，您应该获取自动签名对 Qlik 意义上企业应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png