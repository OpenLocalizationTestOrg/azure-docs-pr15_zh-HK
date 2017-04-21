<properties
    pageTitle="Azure 的活动目录 B2C︰ 创建 Azure 活动目录 B2C 租户 |Microsoft Azure"
    description="有关如何创建 Azure 活动目录 B2C 租户主题"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure 的活动目录 B2C︰ 创建 Azure AD B2C 租户

要开始使用 Microsoft Azure 活动目录 (AD Azure) B2C，请按照本文中概述的三个步骤。

## <a name="step-1-sign-up-for-an-azure-subscription"></a>第 1 步︰ 注册 Azure 的订阅

如果您已经有了 Azure 的订阅，请跳过此步骤。 如果不是，注册[Azure 订阅](../active-directory/sign-up-organization.md)并访问 Azure AD B2C。

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>步骤 2︰ 创建 Azure AD B2C 租户

使用以下步骤创建新的 Azure AD B2C 租户。 目前 B2C 功能不能打开在您现有的租户。

1. 作为订阅管理员登录到[Azure 的传统门户网站](https://manage.windowsazure.com/)。 这是相同的工作或学校帐户或用来注册 azure 的 Microsoft 帐户相同。
2. 单击**新建** > **应用程序服务** > **Active Directory** > **目录** > **自定义创建**。

    ![开始创建一个租户的屏幕抓图](./media/active-directory-b2c-get-started/new-directory.png)

3. 选择您的组织**名称**、**域名**和**国家或地区**。
4. 检查所说︰**这是 B2C 目录**的选项。
5. 单击复选标记以完成的动作。

    ![复选标记以创建一个 B2C 目录的屏幕抓图](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. 您的租户现已创建，将会出现在 Active Directory 扩展。 您还会组织的全局管理员。 您可以根据需要添加其他全局管理员。

    > [AZURE.IMPORTANT]
    如果您计划 B2C 租户用于生产应用程序中，阅读的文章[与预览 B2C 承租人](active-directory-b2c-reference-tenant-type.md)的生产规模。 请注意，存在一些已知问题删除现有 B2C 租户和重新使用相同的域名创建它时。 您必须使用不同的域名创建 B2C 租户。

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>第 3 步︰ 导航到 Azure 门户上 B2C 功能刀片式服务器

1. 导航到 Active Directory 扩展在左侧的导航栏上。
2. 查找您租户**目录**选项卡，然后单击它。
3. 单击**配置**选项卡。
4. 单击**B2C 管理**部分中的**管理 B2C 设置**链接。

    ![B2C 的目录配置的屏幕抓图](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Azure 门户显示 B2C 功能刀片式服务器将打开新的浏览器选项卡或窗口中。

    > [AZURE.IMPORTANT]
    它可以为您的租户可以在 Azure 的门户网站上访问最多 2-3 分钟。 之后不久将解决此问题，请重试这些步骤。 如果没有，请与支持部门联系。

6. 固定到您 Startboard 此刀片式服务器，以便于访问。 （功能刀片式服务器的右上角的红色标记是针工具）。

    ![B2C 功能刀片式服务器的屏幕抓图](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    您可以管理用户和组、 自助服务密码重置配置，以及公司品牌功能的[Azure 的传统门户网站](https://manage.windowsazure.com/)上您租户。

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>方便地访问在 Azure 门户功能 B2C 刀片式服务器

若要提高发现能力，我们已向 B2C 功能刀片式服务器在 Azure 的门户网站上添加一个快捷方式。

1. 以 B2C 租户全局管理员身份登录到 Azure 的门户。 如果您已登录到不同的租户，开关承租人 （右上角）。
2. 在左侧导航，请单击**浏览**。
3. 单击**Azure AD B2C**访问 B2C 功能刀片式服务器。

    ![浏览到 B2C 功能刀片式服务器屏幕快照](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>下一步行动

了解如何向应用程序注册 Azure AD B2C 以及如何构建一个快速启动的应用程序通过读取[Azure 活动目录 B2C︰ 注册您应用程序](active-directory-b2c-app-registration.md)。
