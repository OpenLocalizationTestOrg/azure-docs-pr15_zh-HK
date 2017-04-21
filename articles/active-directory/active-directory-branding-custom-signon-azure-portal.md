<properties
pageTitle="自定义登录页在 Azure Active Directory 预览 |Microsoft Azure"
description="了解如何添加一个公司品牌到 Azure 登录页"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>添加到签入网页 Azure Active Directory 预览中品牌公司

为了避免混淆，很多公司想要在所有的网站和服务管理中应用一致的外观和感觉。 Azure Active Directory 预览提供了此功能，允许您自定义的外观与您的公司徽标和自定义配色方案的登录页面。 [在预览中是什么？](active-directory-preview-explainer.md) 登录页面是您登录到 Office 365 或其他基于 web 的应用程序使用 Azure 的广告为您的身份标识提供程序时，将显示页。 使用此页面以输入您的凭据，您进行交互。

如果您想要在此页面上显示您公司的品牌、 颜色以及其他自定义元素，请参阅下面的图像，以了解两种体验之间的区别。

下面的屏幕快照所示和在台式机**前**自定义登录页 Office 365 的示例︰

![Office 365 登录页面自定义项之前](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

下面的屏幕快照所示和在桌面计算机**后**自定义登录页 Office 365 的示例︰

![自定义后 office 365 登录页](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>自定义登录页

通常，如果您需要基于浏览器的访问您的云应用程序和订阅您的组织的服务，可以使用登录页。

如果您的更改应用到登录页面，它会占用一小时所做的更改显示的时间。

当您访问使用特定于租户的 URL 如 https://outlook.com/**contoso**.com 或 https://mail 的服务时，才会出现品牌的登录页面。**contoso**。 com。

当您访问具有非租户特定 Url 的服务 (例如︰ https://mail.office365.com)，非品牌的登录页出现。 在这种情况下，您的品牌后会显示您输入您的用户 ID 或您选择的用户图块。

> [AZURE.NOTE]
>
- 您的域名必须显示为"活动"中的**域**部分 Azure 门户已配置的品牌。 有关详细信息，请参阅[添加自定义域名](active-directory-domains-add-azure-portal.md)。
- 登录页面的品牌并不会延续到使用者登录页面的 Microsoft。 如果您使用 Microsoft 帐户登录，您可能会看到用户图块由 Azure 广告，呈现品牌的列表，但您的组织的品牌不适用于 Microsoft 帐户登录页面。

在登录页上，**使我保持登录**复选框允许用户关闭并重新打开浏览器时保持登录。 

   ![我签名中](./media/active-directory-branding-custom-signon-azure-portal/01.png)

它不会影响会话生存期。 您可以隐藏在 Azure Active Directory 登录页上的复选框。
是否显示该复选框取决于**使我保持登录已禁用**的设置。

   ![我签名中](./media/active-directory-branding-custom-signon-azure-portal/02.png)


若要隐藏复选框，配置此设置为**是**。 

> [AZURE.NOTE] SharePoint Online 和 Office 2010 的一些功能依赖于用户可以选中此复选框。 如果您配置此设置为隐藏，您的用户可能会看到其他和意外提示进行登录。




**要添加到您的目录品牌的公司︰**

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

    ![打开用户管理](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. 在**用户和组**刀片式服务器，选择**公司品牌**。

4. 在**的用户和组的公司品牌**刀片式服务器，选择**编辑**命令。

    ![编辑自定义品牌](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. 修改您要自定义的元素。 所有元素都是可选的。

6. 单击**保存**。

可能要花费一小时做品牌显示登录页面的任何更改的时间。

## <a name="next-steps"></a>下一步行动

[添加特定于语言的公司品牌](active-directory-branding-localize-azure-portal.md)
