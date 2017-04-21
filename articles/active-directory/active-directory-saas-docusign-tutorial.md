<properties
    pageTitle="教程︰ Azure Active Directory 集成与 DocuSign |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 DocuSign 之间。"
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>教程︰ 与 DocuSign 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 DocuSign 的集成。
在本教程中介绍的方案假定您已经有以下各项︰

- 有效的 Azure 订购
- 在 DocuSign 中承租人



在本教程中介绍的方案由以下构造块组成︰

1. [启用应用程序集成为 DocuSign](#enabling-the-application-integration-for-docusign) 


2. [配置单一登录](#configuring-single-sign-on) 


3. [配置帐户设置](#configuring-account-provisioning) 


4. [分配用户](#assigning-users) 

    ![配置单一登录][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>启用应用程序集成为 DocuSign

本部分的目的是概述如何启用应用程序集成为 DocuSign。

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>若要启用应用程序集成为 DocuSign，请执行以下步骤︰

1. 在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![配置单一登录][1]

2. 从目录列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![配置单一登录][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在做什么对话框中，单击**添加剪辑库中的应用程序**。

    ![配置单一登录][4]


6. 在搜索框中，键入**DocuSign**。

    ![配置单一登录][5]

7. 在结果窗格中，选择**DocuSign**，，然后单击**完成**添加应用程序。

    ![配置单一登录][6]


## <a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 DocuSign。


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1. 在 Azure 传统门户网站，在**DocuSign 应用程序集成**页上，单击**配置单一登录**配置单一登录对话框打开。

    ![配置单一登录][7]

2. 在**您想怎样来登录到 DocuSign 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击下一步。

    ![配置单一登录][8]

3. 在**配置应用程序设置**页中，请执行以下步骤︰

    ![配置单一登录][61]

    一。 在**登录 URL**文本框中，键入`https://account.docusign.com/*`。  

    b。 在**标识符**文本框中，键入`https://account.docusign.com/*`。  
   
    c。 单击**下一步**。 


    > [AZURE.TIP] 在 URL 符号和标识符值只是占位符。 在本主题的后面部分介绍了说明如何检索您的环境的实际值。
 

4. 在**配置单一登录 DocuSign 在**页上，单击**下载证书**，然后保存在您的计算机的本地证书文件。

    ![配置单一登录][10]


5. 在不同的 web 浏览器窗口中，为您的**DocuSign 管理门户的**管理员身份登录。


6. 在左侧导航菜单中，单击**域**。

    ![配置单一登录][51]

7. 在右窗格中，单击**索赔域**。

    ![配置单一登录][52]

8. 上**索赔域**对话框中的**域名**文本框中，键入您的公司域，然后单击**索赔**。 请确保验证域，并且处于活动状态。

    ![配置单一登录][53]

9. 在左侧的菜单上，单击**标识提供程序**  

    ![配置单一登录][54]

10. 在右窗格中，单击**添加标识提供程序**。 
    
    ![配置单一登录][55]

11. 在**标识提供程序设置**页中，请执行以下步骤︰

    ![配置单一登录][56]


    一。 在**名称**文本框中，键入您的配置的唯一名称。 请不要使用空格。

    b。 在 Azure 经典门户中，复制的颁发者 URL 中，，然后将其粘贴到**标识提供商颁发者**文本框。

    c。 在 Azure 经典门户中，复制的**远程登录 URL**，，然后将其粘贴到**标识提供程序的登录 URL**文本框。

    d。 在 Azure 经典门户中，复制的**远程注销 URL**，，然后将其粘贴到**标识提供程序注销 URL**文本框。

    电子。 选择**登录身份验证请求**。

    f。 **通过发送身份验证请求**，选择**开机自检**。

    g。 **发送通过注销请求**，请选择**开机自检**。 


12. 在**自定义属性映射**部分中，选择想要使用 Azure 广告声明映射的字段。 在此示例中， **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**的值映射**电子邮件地址**索赔。 这是从 Azure 广告电子邮件声明为默认声明名称。 

    > [AZURE.NOTE] 使用相应的**用户标识符**映射到 Docusign 的用户映射的用户从 Azure 的广告。 选择适当的字段并输入适当的值基于您的组织的设置。

    ![配置单一登录][57]

13. **标识提供程序证书**部分中，单击**添加证书**，然后上载您下载从 Azure AD 传统门户网站证书。   

    ![配置单一登录][58]

14. 单击**保存**。

15. **标识提供程序**部分中，单击**操作**，然后单击**终结点**。   

    ![配置单一登录][59]



10. 在 Azure 的传统门户网站，请返回到**配置应用程序设置**页。 

16. 在**DocuSign 管理门户**，在**视图 SAML 2.0 终结点**部分执行，以下步骤︰

    ![配置单一登录][60]

    一。 复制**服务提供商颁发者 URL**，然后将其粘贴到 Azure 的传统门户网站上的**标识符**文本框。

    b。 复制**服务提供商登录 URL**，并粘贴在 Azure 的传统门户网站上的**签名在 URL**文本框。

    c。  单击**关闭**  


10. 在 Azure 的传统门户网站，单击**下一步**。 


15. 在 Azure 的传统门户网站，选择**单一登录配置确认**，，然后单击**下一步**。

    ![应用程序][14]

10. 在**单一登录确认**页上，单击**完成**。

    ![应用程序][15]
 

## <a name="configuring-account-provisioning"></a>配置帐户设置

本部分的目的是概述如何启用 Active Directory 用户帐户添加到 DocuSign 的用户供应。

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1. 在**Azure 的传统门户网站**，在**DocuSign 应用程序集成**页上，单击**配置帐户设置**以打开配置用户设置对话框。

    ![配置帐户设置][30]

2. 在**设置和管理凭据**页中，若要启用自动用户供应，提供 DocuSign 帐户的凭据具有足够的权限，然后单击**下一步**。 

    ![配置帐户设置][31]

3. 在**测试连接**对话框中，单击**开始测试**，测试成功后单击**下一步**。

    ![配置帐户设置][32]

3. 在**确认**页上，单击**完成**。

    ![配置帐户设置][33]
 

## <a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>要将用户分配到 DocuSign 中，执行以下步骤︰

1. 在**Azure 的传统门户网站**中，创建一个测试帐户。

2. 在**DocuSign 应用程序集成**页上，单击**将用户分配**。

    ![分配用户][40]
 

3. 选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![分配用户][41]


您现在应等待 10 分钟，并验证该帐户已被同步到 DocuSign。

作为第一个验证步骤，您可以检查供应状态，通过单击在 Azure 的经典门户上的 DocuSign 应用程序集成页面 D 中的仪表板。

![分配用户][42]

由相关的状态指示已成功完成资源调配周期用户︰

![分配用户][43]


如果您想要测试单个登录设置，打开后盖。

关于访问面板的详细信息，请参阅访问权限面板简介。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png