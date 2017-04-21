<properties 
    pageTitle="教程︰ Azure Active Directory 集成业务的收存箱与 |Microsoft Azure" 
    description="了解如何使用业务的 Azure Active Directory 收存箱启用单一登录、 自动化资源调配，以及更多 ！" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>教程︰ 与业务的收存箱的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和收存箱用于业务集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在业务的收存箱测试租户
  
在完成本教程之后, 已归入收存箱的业务将能够登录单个应用程序在您的业务的收存箱到 Azure AD 用户公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用用于收存箱为业务应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "方案")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>启用用于收存箱为业务应用程序集成
  
本部分的目的是概述如何启用的收存箱的业务应用程序集成。

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>若要启用的收存箱的业务应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**为企业收存箱**。

    ![应用程序库](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "应用程序库")

7.  在结果窗格中，选择**收存箱的业务**，，然后单击**完成**添加应用程序。

    ![为企业收存箱](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "为企业收存箱")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到业务的收存箱。

作为此过程的一部分，您都需要将 base 64 编码的证书上传到您的商业租户的收存箱。 如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**收存箱为业务**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "配置单一登录")

2.  在**您想怎样来登录到业务的收存箱的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "配置单一登录")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    一。 登录到您的收存箱的商业租户。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "配置单一登录")

    b。 在左侧导航窗格中，单击**管理控制台**。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "配置单一登录")

    c。 在**管理控制台**上，单击在左侧的导航窗格中的**身份验证**。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "配置单一登录")

    d。 在**单一登录**部分中，选择**启用单一登录**，然后单击**更多**来扩展该部分。  

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "配置单一登录")

    电子。 复制的 URL，**用户可以通过输入其电子邮件地址登录，或者它们可以直接转到**旁边。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "配置单一登录")

    f。 在 Azure 的传统门户网站，在**为企业收存箱登录**URL 文本框中，粘贴 URL。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "配置单一登录")  



4. 在**配置单一登录为企业收存箱在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。  

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "配置单一登录")


5. 在您收存的商业租户，在**身份验证**页的**单一登录**部分中，执行以下步骤︰ 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "配置单一登录")

    一。 单击**所需**。

    b。 在 Azure 经典门户中，在**配置单一登录为企业收存箱在**对话框页面上，复制的**登录页的 URL**值，，然后将其粘贴到**登录 URL**文本框。


    c。 创建您已下载的证书的**Base 64 编码**的文件。 

    > [AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。


    d。 单击**"选择证书"**按钮，然后浏览到您的**base 64 编码的证书文件**。


    电子。 单击**"保存更改"**按钮以完成您的商业租户的收存箱上的配置。


6. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。 

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "配置单一登录")



##<a name="configuring-user-provisioning"></a>配置用户设置
  
本部分的目的是概述如何启用用户供应的 Active Directory 用户帐户添加到业务的收存箱。


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1. 在 Azure 传统门户网站，在**收存箱为业务**应用程序集成页上，单击**配置用户设置**以打开**配置用户设置**对话框。

2. 在启用用户供应到收存箱业务页，单击启用用户提供打开登录到收存箱链接使用 Azure AD 对话。  

    ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "用户资源调配")

3. 在**登录到收存箱与 Azure 的广告链接**对话框中，登录到您的商业租户的收存箱。 

    ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "用户资源调配")



4. 单击**允许**授予 Azure 广告权收存箱。 

    ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "用户资源调配")



5. 若要完成配置，请单击**完成**按钮。  

    ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "用户资源调配")




##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>若要将用户分配到业务的收存箱，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**收存箱为业务**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "是")
  


现在应该等待 10 分钟，并验证该帐户已被同步到收存箱的业务。

作为第一个验证步骤，您可以检查供应状态，通过 Azure 的经典门户上的**收存箱为业务**应用程序集成页面中单击**仪表板**。

![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "分配用户")


已成功完成的用户供应周期的相关状态指示。

![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "分配用户")


如果您想要测试单个登录设置，打开后盖。
关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。




## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)