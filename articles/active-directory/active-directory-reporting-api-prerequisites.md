<properties
    pageTitle="若要访问报告 API 的 Azure 广告的先决条件。 |Microsoft Azure"
    description="了解有关访问报告 API 的 Azure 广告的前提条件"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>要访问报告 API 的 Azure 广告系统必备组件 

[Azure AD 报告 Api](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview)为您提供编程访问通过基于 REST 的 Api 的一组数据。 从不同的编程语言和工具，您可以调用这些 Api。

报告的 API 使用[OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx)授权访问 web Api。 

若要准备报告的 API 的访问，您必须︰

1. 在 Azure AD 租户中创建应用程序 

2. 授予应用程序适当的权限来访问 Azure 的广告数据

3. 收集您的目录的配置设置

疑问、 问题或反馈，请与[帮助 AAD 报告](mailto:aadreportinghelp@microsoft.com)。


## <a name="create-an-azure-ad-application"></a>创建 AD Azure 应用程序

若要配置目录访问报告 API 的 Azure 广告，您必须登录 Azure 经典门户使用 Azure 预订管理员帐户也是 Azure AD 租户中的全局管理员目录角色的成员。

> [AZURE.IMPORTANT] 像这样的"管理员"权限的凭据下运行的应用程序可以是功能非常强大，因此请务必保证应用程序的 ID/密码凭据的安全。


1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/01.png) 

2. 从**活动目录**列表中，选择您的目录。

3. 在菜单上，单击**应用程序**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/02.png) 

4. 在底部栏上，单击**添加**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/03.png) 

5. 在**要做什么？**对话框中，单击**添加我的公司正在开发的应用程序**。 

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/04.png) 


6. 在**告诉我们有关您的应用程序**对话框中，请执行以下步骤︰ 

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/05.png) 

    一。 在**名称**文本框中，键入一个名称 (例如︰ 报告 API 应用程序)。

    b。 选择**Web 应用程序和/或 web API**。

    c。 单击**下一步**。


7. 在**应用程序属性**对话框中，请执行以下步骤︰ 

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/06.png) 

    一。 在**登录 URL**文本框中，键入`https://localhost`。

    b。 在**应用程序 ID URI**文本框中，键入```https://localhost/ReportingApiApp```。

    c。 单击**完成**。



## <a name="grant-your-application-permission-to-use-the-api"></a>授予您使用 API 的应用程序权限

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)，在左侧的导航窗格中，单击**活动目录**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/01.png) 

2. 从**活动目录**列表中，选择您的目录。

3. 在菜单上，单击**应用程序**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/02.png)


3. 在应用程序列表中，选择新建应用程序。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/07.png)

4. 在菜单上，单击**配置**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/08.png)


5. **向其他应用程序的权限**部分中的**Azure Active Directory**资源，请单击**应用程序权限**下拉列表，然后选择**读取目录数据**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/09.png)


5. 在底部栏上，单击**保存**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>收集您的目录的配置设置

本节说明如何从目录获取下列设置︰

- 域名称
- 客户机 ID
- 客户端密码

配置报告的 API 调用时，您需要这些值。 


### <a name="get-your-domain-name"></a>获取您的域名

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/01.png) 

2. 从**活动目录**列表中，选择您的目录。

3. 在菜单上，单击**域**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/11.png) 

4. 在**域名称**列中，复制您的域名。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>获取应用程序的客户端 ID

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/01.png) 

2. 从**活动目录**列表中，选择您的目录。

3. 在菜单上，单击**应用程序**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/02.png) 

4. 在应用程序列表中，选择新建应用程序。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/07.png)

4. 在菜单上，单击**配置**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/08.png)

4. 复制您的**客户端 ID**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>获取应用程序的客户端密码

若要获取应用程序的客户端密码，您需要创建一个新项并保存它时保存新的密钥，因为它不可能以后不再检索此值的值。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/01.png) 

2. 从**活动目录**列表中，选择您的目录。

3. 在菜单上，单击**应用程序**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/02.png) 

4. 在应用程序列表中，选择新建应用程序。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/07.png)

4. 在菜单上，单击**配置**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/08.png)

5. 在**键**部分中，执行以下步骤︰ 

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/14.png)

    一。 在持续时间列表中，选择持续时间

    b。 在底部栏上，单击**保存**。

    ![注册应用程序](./media/active-directory-reporting-api-prerequisites/10.png)

    c。 复制注册表项值。

## <a name="next-steps"></a>下一步行动

- 从 Azure 的 AD 报告 API 以编程方式访问的数据吗？ 签出[入门 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)。

- 如果您想要找出有关 Azure Active Directory 报告的详细信息，请参阅[Azure 活动目录报告指南](active-directory-reporting-guide.md)。  
