<properties 
    pageTitle="教程︰ 配置入站同步工作日 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 站同步启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>教程︰ 配置入站同步的工作日
>[AZURE.NOTE]Azure 的活动目录 (AD) 津贴是适用于在中国的客户使用 Azure AD 全球实例。    
当前 Microsoft Azure 服务由 21Vianet 中国中不支持 azure AD 特优。    

本教程的目的是向您显示您需要在工作日和 Microsoft Azure 广告从工作日的人导入到 Microsoft Azure 的广告执行的步骤。    
 在本教程中介绍的方案假定您已经有以下各项︰  

-   有效的 Azure 订购  
-   在工作日中一个租户  

在本教程中介绍的方案由以下构造块组成︰  

1.  启用应用程序集成的工作日  
2.  创建一个集成的系统用户  
3.  创建安全组  
4.  将集成系统用户分配给安全组  
5.  配置安全组选项  
6.  激活安全策略更改  
7.  在 Microsoft Azure AD 配置的用户导入  

##<a name="enabling-the-application-integration-for-workday"></a>启用应用程序集成的工作日

本部分的目的是概述如何启用应用程序集成的队伍。    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>若要启用应用程序集成为工作日，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。    

    ![活动目录](./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "活动目录")  

2.  从**目录**列表中，选择要为其启用目录集成的目录。    

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。    

    ![应用程序](./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "应用程序")  

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。    

    ![要做什么？](./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "要做什么？")  

5.  在**搜索框**中，键入**工作日**。    

    ![工作日](./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "工作日")  

6.  在结果窗格中，选择**工作日**，，然后单击**完成**添加应用程序。    

    ![工作日](./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "工作日")  

##<a name="creating-an-integration-system-user"></a>创建一个集成的系统用户

1.  在**工作日工作台**，在搜索框中，输入**创建用户**，然后单击链接，**创建集成系统用户**。     

    ![创建用户](./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "创建用户")  

2.  完成创建集成系统用户任务的新的集成系统用户提供用户名和密码。  取消选中要求新密码在下一次登录选项，因为此用户将以编程方式在登录。    
    其默认值为 0，这将防止用户的会话超时过早离开会话超时分钟。    

    ![创建集成系统用户](./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "创建集成系统用户")  

##<a name="creating-a-security-group"></a>创建安全组

在本教程中所述的情况下，您需要创建一个不受约束的集成系统安全组并将用户分配给它。    

1.  输入在搜索框中创建安全组，然后单击链接，创建安全组。     

    ![CreateSecurity 组](./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity 组")  

2.  完成创建安全组任务。  选择集成系统安全组 — — 不受约束的租用安全组类型下拉列表中，从创建该成员将被显式添加到安全组。     

    ![CreateSecurity 组](./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity 组")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>将集成系统用户分配给安全组

1.  在搜索框中输入编辑安全组，然后单击链接，**编辑安全组**。     

    ![编辑安全组](./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "编辑安全组")  

2.  搜索并选择新的集成安全组的名称    

    ![编辑安全组](./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "编辑安全组")  

3.  新的集成系统用户添加到新的安全组。       

    ![系统安全组](./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "系统安全组")  

##<a name="configuring-security-group-options"></a>配置安全组选项

在此步骤中，您将授予新安全组权限的 Get 和 Put 操作由下列域安全策略保护的对象︰  

-   外部帐户设置  
-   辅助数据︰ 公用工作报告  
-   所有职位的工作人员的数据︰  
-   工作人员的数据︰ 当前人员信息  
-   在工作人员的个人资料的工作人员数据︰ 职务  

&nbsp;  

1.  在搜索框中，输入域安全策略，然后单击链接域安全策略的功能区域。     

    ![域安全策略](./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "域安全策略")  

2.  搜索系统，并选择系统功能区域。  单击该按钮标记，确定。     

    ![域安全策略](./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "域安全策略")  

3.  在系统功能区域的安全策略列表中，展开安全管理，然后选择外部帐户设置的域安全策略。     

    ![域安全策略](./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "域安全策略")  

4.  单击编辑权限按钮，然后在屏幕上编辑权限，具有 Get 和 Put 集成权限的安全组的列表添加新安全组。     

    ![编辑权限](./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "编辑权限")  

5.  重复第 1 步上面，以返回到屏幕上的选择功能区域中，和这一次，搜索人员、 选择人力资源功能区域，然后单击标记为确定按钮。    

    ![域安全策略](./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "域安全策略")  

6.  在人力资源功能区域的安全策略列表中，展开工作人员数据︰ 人员，并重复上面的步骤 4 为每个剩余的安全策略︰    

    -   辅助数据︰ 公用工作报告  
    -   所有职位的工作人员的数据︰  
    -   工作人员的数据︰ 当前人员信息  
    -   在工作人员的个人资料的工作人员数据︰ 职务    

    ![域安全策略](./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "域安全策略")  

##<a name="activating-security-policy-changes"></a>激活安全策略更改

1.  输入激活在搜索框中，然后单击链接，激活挂起的安全策略更改。    

    ![激活](./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "激活")  

2.   通过为审计目的，输入注释，然后单击按钮标记，确定开始激活挂起的安全策略更改任务。      

    ![激活挂起的安全](./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "激活挂起的安全")  

3.  通过选中复选框标记为确认和单击按钮标记，确定完成该任务的下一个屏幕上。     

    ![激活挂起的安全](./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "激活挂起的安全")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>在 Microsoft Azure AD 配置的用户导入

本部分的目的是概述如何配置 Microsoft Azure AD 工作日从导入的人。    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>若要在 Microsoft Azure AD 配置用户导入，请执行以下步骤︰

1.  在**工作日**应用程序集成页上，单击**配置用户导入**以打开**配置设置**对话框。    

2.  上的**设置和管理凭据**页中，请执行以下步骤，，然后单击下一步:    

    ![设置和管理凭据](./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "设置和管理凭据")    

    1.  在**工作日管理员用户名**文本框中，键入您在[创建一个集成的系统用户](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser)部分中创建的用户的名称。    
    2.  在**工作日管理员密码**文本框中，键入您在[创建一个集成的系统用户](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser)部分中创建的用户的密码。    
    3.  在**工作日租户 URL**文本框中，键入 URL 或工作日租户。    

3.  在**测试连接**页上，单击**开始测试**以确认连接，，然后单击**下一步**。    

    ![测试连接](./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "测试连接")  

4.  在**资源调配选项**页上，单击**下一步**。    

    ![自动配置选项](./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "自动配置选项")  

5.  在**开始调配**对话框中，单击**完成**。    

    ![开始设置](./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "开始设置")  

您现在可以转到**用户**部分，检查工作日用户是否已导入。    
