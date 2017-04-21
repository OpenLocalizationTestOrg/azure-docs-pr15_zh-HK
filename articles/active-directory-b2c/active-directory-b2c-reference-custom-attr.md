<properties
    pageTitle="Azure 的活动目录 B2C︰ 自定义属性 |Microsoft Azure"
    description="如何在 Azure 活动目录 B2C 中使用自定义特性来收集关于您的使用者信息"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure 的活动目录 B2C︰ 使用自定义特性来收集关于您的使用者信息

附带有内置的一组信息 （属性） 的 Azure 活动目录 (AD Azure) B2C 目录︰ 名字、 姓氏、 城市、 邮政编码和其他属性。 但是，每个面向消费者的应用程序具有从消费者收集在特性上的特殊要求。 使用 Azure AD B2C，可以扩展一的组属性，存储在每个使用者帐户上。 可以在[Azure 的门户网站](https://portal.azure.com/)上创建自定义属性，并使用它在您的注册策略，如下所示。 您还可以读取和写入这些属性，使用[Azure 广告图形 API](active-directory-b2c-devquickstarts-graph-dotnet.md)。

> [AZURE.NOTE]
自定义特性，请使用[Azure 广告图形 API 目录架构扩展](https://msdn.microsoft.com/library/azure/dn720459.aspx)。

## <a name="create-a-custom-attribute"></a>创建自定义属性

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**用户属性**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 提供自定义属性 (例如，"ShoeSize") 和 （可选）**描述**的**名称**。 单击**创建**。

    > [AZURE.NOTE]
    目前仅"字符串"**数据类型**。

现可在列表中的**用户属性**，并且在您注册策略中使用的自定义特性。

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>在您注册策略中使用的自定义属性

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 单击**注册策略**。
3. 单击您注册的策略 (例如，"B2C_1_SiUp") 以将其打开。 单击**编辑**顶部的刀片式服务器。
4. 单击**注册属性**并选择自定义属性 (例如，"ShoeSize")。 单击**确定**。
5. 单击**应用程序声明**并选择自定义属性。 单击**确定**。
6. 单击**保存**顶部的刀片式服务器。

可以使用策略上的"立即运行"功能来验证使用者体验。 现在应该看到"ShoeSize"的注册，使用者过程中收集的属性列表中并在发送回您的应用程序的标记中看到它。

## <a name="notes"></a>备注

- 注册策略，同时还注册或登录策略和编辑策略的配置文件中使用自定义特性。
- 没有已知自定义特性的限制。 仅创建在任何策略，并将其添加到列表中的**用户属性**时不使用它的第一次。
