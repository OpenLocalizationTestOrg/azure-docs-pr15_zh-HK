<properties 
    pageTitle="教程︰ 配置入站同步工作日 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 作为标识数据源的工作日。" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>教程︰ 配置入站同步的工作日


本教程的目的是向您显示您需要在工作日和 Azure 广告从工作日的人导入到 Azure 的广告执行的步骤。 

在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure AD 最优订购
-   在工作日中一个租户
  
在本教程中介绍的方案由以下构造块组成︰

1. 启用应用程序集成的工作日 


2. 创建一个集成的系统用户 


3. 创建安全组 


4. 将集成系统用户分配给安全组 


5. 配置安全组选项 


6. 激活安全策略更改 


7. 在 Azure 广告中配置的用户导入 



##<a name="enabling-the-application-integration-for-workday"></a>启用应用程序集成的工作日
  
本部分的目的是概述如何启用应用程序集成为工作日。

### <a name="steps"></a>步骤如下︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "添加应用程序")

  
5. 在搜索框中，键入**工作日**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "添加应用程序从 gallerry")

6. 在结果窗格中，选择工作日，然后单击完成添加应用程序。

    ![应用程序库](./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "应用程序库")





## <a name="creating-an-integration-system-user"></a>创建一个集成的系统用户

### <a name="steps"></a>步骤如下︰


1. 在**工作日工作台**中，输入在搜索框中创建用户，然后单击**创建集成系统用户**。 

    ![创建用户](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "创建用户")



2. 完成**创建集成系统用户**任务的新的集成系统用户提供用户名和密码。  取消选中要求新密码在下一次登录选项，因为此用户将以编程方式在登录。 其默认值为 0，这将防止用户的会话超时过早离开会话超时分钟。 

    ![创建集成系统用户](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "创建集成系统用户")
 




## <a name="creating-a-security-group"></a>创建安全组

在本教程中所述的情况下，您需要创建一个不受约束的集成系统安全组并将用户分配给它。

### <a name="steps"></a>步骤如下︰

1. 输入在搜索框中创建安全组，然后单击**创建安全组**。 

    ![CreateSecurity 组](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity 组")
 

2. 完成创建安全组任务。  选择集成系统安全组 — — 不受约束的租用安全组类型下拉列表中，从创建该成员将被显式添加到安全组。 

    ![CreateSecurity 组](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity 组")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>将集成系统用户分配给安全组

### <a name="steps"></a>步骤如下︰


1. 在搜索框中输入编辑安全组，然后单击**编辑安全组**。 

    ![编辑安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "编辑安全组")
 
 

2. 搜索并选择新的集成安全组的名称。 

    ![编辑安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "编辑安全组")

 

3. 新的集成系统用户添加到新的安全组。 

    ![系统安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "系统安全组")  




## <a name="configuring-security-group-options"></a>配置安全组选项

在此步骤中，您将授予**获取**和**设置**由下列域安全策略保护的对象上的操作的新安全组权限︰

- 外部帐户设置

- 辅助数据︰ 公用工作报告

- 所有职位的工作人员的数据︰

- 工作人员的数据︰ 当前人员信息

- 在工作人员的个人资料的工作人员数据︰ 职务

 
### <a name="steps"></a>步骤如下︰

1. 在搜索框中，输入域安全策略，然后单击链接域安全策略的功能区域。  

    ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "域安全策略")  
 

2. 搜索系统，并选择**系统**功能区域。  单击**确定**。  

    ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "域安全策略")  


3. 在系统功能区域的安全策略列表中，展开安全管理，然后选择外部帐户设置的域安全策略。  

    ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "域安全策略")  


4. 单击**编辑权限**，然后在**编辑权限**对话框页到具有集成的**获取**和**设置**权限的安全组的列表添加新的安全组。 

    ![编辑权限](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "编辑权限")  

 

5. 重复第 1 步上面，以返回到屏幕上的选择功能区域中，和这一次，搜索人员、 选择人力资源功能区域，然后单击**确定**。

    ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "域安全策略")  
 

6. 在人力资源功能区域的安全策略列表中，展开工作人员数据︰ 人员，并重复上面的步骤 4 为每个剩余的安全策略︰

     - 辅助数据︰ 公用工作报告

     - 所有职位的工作人员的数据︰

     - 工作人员的数据︰ 当前人员信息

     - 在工作人员的个人资料的工作人员数据︰ 职务


    ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "域安全策略")  







## <a name="activating-security-policy-changes"></a>激活安全策略更改

### <a name="steps"></a>步骤如下︰

1. 输入激活在搜索框中，然后单击链接，激活挂起的安全策略更改。 

    ![激活](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "激活") 
 

2. 首先激活挂起的安全策略更改任务用于审计目的，输入注释，然后单击**确定**。 

    ![激活挂起的安全](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "激活挂起的安全")   
 

3. 通过选中复选框标记确认，完成下一个屏幕上的任务，然后单击**确定**。 

    ![激活挂起的安全](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "激活挂起的安全")  





## <a name="configuring-user-import-in-azure-ad"></a>在 Azure 广告中配置的用户导入

本部分的目的是概述如何配置 Azure AD 工作日从导入的人。


### <a name="steps"></a>步骤如下︰


1. 在**工作日**应用程序集成页上，单击**配置用户导入**以打开**配置设置**对话框。


2. 在**设置和管理凭据**页中，执行以下步骤，，然后单击**下一步**: 

    ![设置和管理凭据](./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "设置和管理凭据")  
 
    一。 在工作日 admin 用户的名称文本框中，键入您在创建创建的用户的名称集成系统用户部分。

    b。 在工作日 admin 密码文本框中，键入在创建创建的用户的密码集成系统用户部分。

    c。 在工作日租户的 URL 文本框中，键入 URL 或工作日租户。


3. 在**测试连接**页上，单击**开始测试**以确认连接，，然后单击**下一步**。 

    ![测试连接](./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "测试连接")  
 

4. 在**资源调配**选项页上，单击**下一步**。 

    ![自动配置选项](./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "自动配置选项")


5. 在**开始调配**对话框中，单击**完成**。 

    ![开始设置](./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "开始设置")
 

您现在可以转到**用户**部分，检查工作日用户是否已导入。



## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)
