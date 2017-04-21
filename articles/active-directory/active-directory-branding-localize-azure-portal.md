<properties
pageTitle="添加特定于语言的公司品牌到签入网页中 Azure Active Directory 预览 |Microsoft Azure"
description="了解如何添加一个语言特定公司品牌的图片和文本到 Azure 登录页"
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
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>添加特定于语言的公司品牌到签入网页中 Azure Active Directory 预览

为了避免混淆，很多公司想要在所有的网站和服务管理中应用一致的外观和感觉。 Azure Active Directory 预览提供了此功能，允许您自定义的外观与您的公司徽标和自定义配色方案的登录页面。 [在预览中是什么？](active-directory-preview-explainer.md) 登录页面是您登录到 Office 365 或其他基于 web 的应用程序使用 Azure 的广告为您的身份标识提供程序时，将显示页。 使用此页面以输入您的凭据，您进行交互。

## <a name="customizing-the-sign-in-page-for-another-language"></a>自定义其他语言的登录页

仅当您已创建了自定义登录页与所述[添加到登录页面的品牌公司](active-directory-branding-custom-signon-azure-portal.md)，可以到您的自定义登录页中添加特定于语言的元素。 您可以配置每个目录的一个登录页面，带有一组默认的自定义元素。 您已配置页面元素的默认设置后，您可以配置不同的区域设置的多个版本。 您还可以混合和匹配不同的元素。 例如，您可以︰

- 创建默认**登录页面图像**的适用于所有文化中，然后创建特定版本的英语和法语。 当您将您的浏览器设置为这两种语言之一时，特定于语言的图像就会出现，虽然默认图所显示的所有其他语言。

- 配置不同的标志，为您的组织 （例如，日语和希伯来语版本）。

我们建议您保留的语言变体数量较低，出于维护和性能方面的考虑。

**要添加到您的目录品牌的公司︰**

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

    ![打开用户管理](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. 在**用户和组**刀片式服务器，选择**公司品牌**。

4. 在**的用户和组的公司品牌**刀片式服务器，选择**添加语言**命令。

    ![添加特定于语言的品牌要素](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. 修改您要自定义的元素。 所有元素都是可选的。

6. 单击**保存**。

可能要花费一小时做品牌显示登录页面的任何更改的时间。

## <a name="next-steps"></a>下一步行动

[添加公司品牌到登录页面](active-directory-branding-custom-signon-azure-portal.md)
