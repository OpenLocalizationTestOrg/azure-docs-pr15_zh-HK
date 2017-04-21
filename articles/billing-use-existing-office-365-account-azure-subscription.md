<properties
    pageTitle="共享一个 Azure AD 租户在 Office 365 和 Azure 订阅 |Microsoft Azure"
    description="了解如何共享您的 Office 365 Azure AD 租户和它的用户使用 Azure 的订阅，或进行相反转换"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>使用现有的 Office 365 帐户通过 Azure 订购，反之亦然
方案︰ 您已有的 Office 365 订阅并准备 Azure 的订阅，但您想要使用现有 Office 365 提供用户帐户为您 Azure 的订阅。 或者，您是 Azure 的订阅者，想要获取您现有的 Azure Active Directory 中的用户订阅了 Office 365。 本文介绍以达到这两项目是多么容易。

> [AZURE.NOTE] 这篇文章不能应用于企业协议 (EA) 的客户。 如果您需要更多的帮助，在这篇文章，[请联系支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)任何时刻能够快速解决您的问题。


## <a name="quick-guidance"></a>快速指南

- 如果您已经拥有 Office 365 预订并且想要注册 Azure，使用**您组织的帐户登录**选项。 然后继续 Azure 使用 Office 365 帐户注册过程。 请参阅[本文中稍后介绍的详细的步骤](#s1)。

- 如果已经有 Azure 的订阅并想要获取 Office 365 订阅，登录到 Office 365 Azure 帐户。 然后继续注册步骤。 完成注册后，Azure 订购属于同一个 Azure Active Directory 实例添加 Office 365 订阅。 有关详细信息，请参阅部分[本文内下文中的详细的步骤](#s2)。

>[AZURE.NOTE] 若要获得 Office 365 的订阅，该帐户用于注册必须是全局管理员或计费管理目录角色在 Azure Active Directory 租户的成员。 [了解如何确定在 Azure Active Directory 的角色](#how-to-know-your-role-in-your-azure-active-directory)。

将订阅添加到帐户时会发生什么情况，请参阅[本文后面的背景信息](#background-information)。

## <a name="detailed-steps"></a>详细的步骤
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>方案 1: Office 365 用户打算购买 Azure
在这种情况下，我们假定 Kelley 壁是具有 Office 365 的订阅，并规划到 Azure 订阅的用户。 有两个活动用户的增多和 Tricia Jane。 Kelley 的帐户是admin@contoso.onmicrosoft.com。

![Office 365 用户管理中心](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

若要注册 Azure，请执行以下步骤︰

1. 注册在[Azure.com](https://azure.microsoft.com/)Azure。 单击**免费试用**。 在下一页上，单击**立即开始**。

    ![免费尝试 Azure。](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. 单击**您组织的帐户登录**。

    ![登录到 Azure。](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. 使用 Office 365 帐户登录。 在这种情况下，它是 Kelley 的 Office 365 提供帐户。

    ![使用 Office 365 帐户登录。](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. 填写信息并完成注册过程。

    ![填写信息并完成注册。](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![单击开始管理我的服务。](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

现在您已经全部设置。 在 Azure 的门户中，您会看到相同的用户显示。 若要验证这一点，请执行以下步骤︰

1. 以前显示的屏幕中，单击**开始管理我的服务**。
2. 单击**浏览**，然后单击**撤销**。

    ![单击浏览，然后再单击 Active Directory。](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. 单击**用户**。

    ![用户选项卡](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. 列出所有用户，包括 Kelley，如预期的那样。

    ![用户列表](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>方案 2︰ 计划购买 Office 365 的 Azure 用户

在这种情况下，Kelley 壁是具有在该帐户下的 Azure 订阅的用户admin@contoso.onmicrosoft.com。 Kelley 想要订购 Office 365 提供和使用她已经与 Azure 位于同一个目录。

>[AZURE.NOTE] 若要获得 Office 365 订阅，用于登录的帐户必须是全局管理员或计费管理目录角色在 Azure Active Directory 租户的成员。 [了解如何知道在 Azure Active Directory 中的作用](#how-to-know-your-role-in-your-azure-active-directory)。

![Azure 门户订阅设置](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure 门户的 Active Directory 用户](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Office 365 的订阅，请执行以下步骤︰

1. 请转到[Office 365 的产品页](https://products.office.com/business)上，，然后选择适合您的计划。
2. 您选择的计划后，将显示以下页面。 未填写窗体。 在页面的右上角，单击**登录**。

    ![Office 365 试用页](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. 登录您的帐户凭据。 在此示例中，它是 Kelley 的帐户。

    ![Office 365 号中](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. 单击**立即尝试**。

    ![确认您的 Office 365 的订单。](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. 在订单收据页上，单击**继续**。

    ![Office 365 订单收货](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

现在您已经全部设置。 在 Office 365 管理中心，您会看到从 Contoso 目录显示为活动状态的用户的用户。 若要验证这一点，请执行以下步骤︰

1. 打开 Office 365 管理中心。
2. 展开**用户**，然后单击**活动用户**。

    ![Office 365 管理中心用户](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>如何知道您将 Azure Active Directory 中的角色

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**浏览**，然后单击**撤销**。

    ![在 Azure 门户中的活动目录](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. 单击**用户**。

    ![Azure 门户默认 Active Directory 用户](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. 单击用户。 在此示例中，用户是 Kelley 墙。

    请注意**组织角色**的域。

    ![Azure 的门户用户标识](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Azure 和 Office 365 订阅有关的背景信息
Office 365 和 Azure 使用 Azure Active Directory 服务来管理用户和预订。 作为容器，在其中您可以分组用户和预订考虑 Azure 的目录。 Azure 和 Office 365 提供预订服务使用相同的用户帐户，您需要确保在同一个目录中创建订阅的。 请记住以下几点︰

- 在下一个目录，而不是其他的方法解决获取创建的订阅。
- 用户属于目录，不是其他的方法解决。
- 订阅将落在创建订阅的用户目录中。 结果是，Office 365 订阅被绑定到 Azure 订阅的帐户时使用该帐户来创建 Office 365 订阅。

![背景信息](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

有关详细信息，请参阅[如何 Azure 订阅将与 Azure Active Directory 相关联](./active-directory/active-directory-how-subscriptions-associated-directory.md)。

>[AZURE.NOTE] Azure 订阅由在目录中的单个用户拥有。

>[AZURE.NOTE] Office 365 订阅归目录本身。 如果在目录中的用户具有所需的权限，它们可以应用于这些订阅。

## <a name="next-steps"></a>下一步行动
如果您希望能够从 Azure 订阅访问 Office 365 租户过去，分别获得您的 Azure 和 Office 365 的订阅，请参阅[关联订阅了 Azure Office 365 租户](billing-add-office-365-tenant-to-azure-subscription.md)。

> [AZURE.NOTE] 如果仍有问题，请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)以获取快速解决您的问题。
