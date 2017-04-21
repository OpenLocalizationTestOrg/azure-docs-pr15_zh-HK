<properties 
    pageTitle="如何创建和发布 Azure API 管理产品" 
    description="了解如何创建和发布 Azure API 管理产品。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>如何创建和发布 Azure API 管理产品

在 Azure API 管理，产品包含一个或多个 Api，以及使用配额和使用条款。 一旦产品发布，则开发人员可以订阅产品和开始使用该产品的 Api。 该主题提供了创建产品、 添加一个 API，并将其发布为开发人员提供指南。

## <a name="create-product"></a>创建产品

操作是添加和配置为发布者门户中的 API。 若要访问发布者门户，单击**管理**API 管理服务的 Azure 传统门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

单击在左侧显示**产品**页中，菜单中的**产品**，然后单击**添加产品**。

![产品][api-management-products]

![新产品][api-management-add-new-product]

在**名称**字段和**说明**字段中的产品的说明输入产品的描述性名称。

在 API 管理产品可以是**打开**的或**受保护**。 受保护的产品必须订阅了，才可以使用，打开时可以不订阅使用的产品。 检查**需要订阅**创建一个受保护的产品，要求订阅。 这是默认设置。

检查**需要订阅审批**如果希望由管理员审阅并接受或拒绝订阅到该产品的尝试。 如果未选中此框，则订阅尝试将自动批准。 有关预订的详细信息，请参阅[查看订阅服务器产品][]。

若要允许开发人员帐户订阅产品多次，选中**允许多个订阅**复选框。 如果不选中此复选框，每个开发人员帐户可以订阅仅一次的产品。

![无限多个订阅][api-management-unlimited-multiple-subscriptions]

要限制多个同时进行的订阅数，请检查**的同时预订的限制数**复选框并输入订阅限制。 在以下示例中，同时订阅仅限于四个每个开发人员帐户。

![四个多个订阅][api-management-four-multiple-subscriptions]

一旦配置了所有新的产品选项，请单击**保存**以创建新的产品。

![产品][api-management-products-page]

>默认情况下新产品是未发布，并对**管理员组**只是可见的。

要配置产品，请单击**产品**选项卡中的产品名称。

## <a name="add-apis"></a>添加到产品的 Api

**产品**页包含四个链接配置︰**摘要**、**设置**、**可见**性和**订阅服务器**。 **摘要**选项卡是可以在其中添加 Api 和发布或取消发布产品。

![摘要][api-management-new-product-summary]

在发布产品之前，您需要添加一个或多个 Api。 若要执行此操作，请单击**添加到产品的 API**。

![添加 Api][api-management-add-apis-to-product]

选择所需的 Api，然后单击**保存**。

## <a name="add-description"></a>向产品中添加描述性信息

**设置**选项卡使您可以提供详细的信息如它的用途、 Api，它提供了访问和其他有用的信息产品。 内容将被调用的 API，可以编写纯文本或 HTML 标记中的开发人员的目标。

![产品设置][api-management-product-settings]

检查**需要订阅**创建要求订阅将使用的受保护的产品或清除该复选框以创建不订阅即可调用打开产品。

如果要手动批准所有产品订阅请求，请选择**批准订阅**。 默认情况下会自动授予产品的所有订阅。

允许开发人员帐户订阅产品多次，**允许多个订阅**复选框，可以选择指定一个限制。 如果不选中此复选框，每个开发人员帐户可以订阅仅一次的产品。

（可选） 描述的产品的订阅服务器使用该产品时必须接受使用条款**使用条款**字段中填入即可。

## <a name="publish-product"></a>发布产品

在调用中产品的 Api 之前，必须先发布产品。 产品的**摘要**选项卡上，单击**发布**，然后单击**是，将其发布**确认。 要将以前发布的产品为私有，请单击**取消发布**。

![发布产品][api-management-publish-product]

## <a name="make-visible"></a>使产品开发人员对可见

**可见性**选项卡允许您选择哪些角色都能看到开发人员门户网站上的产品和产品预订。

![产品的可见性][api-management-product-visiblity]

若要启用或禁用开发人员组中可见性的一种产品，检查或取消选中组旁边的复选框，然后单击**保存**。

>有关详细信息，请参阅[如何创建和使用组来管理开发人员帐户在 Azure API 管理][]。

## <a name="view-subscribers"></a>查看产品的订户

**订阅服务器**选项卡列出的开发人员已经订阅了该产品。 通过单击开发者的名称可以查看详细信息和设置每个开发人员。 在本示例中没有开发人员尚未订阅产品。

![开发人员][api-management-developer-list]

## <a name="next-steps"></a>下一步行动

添加所需的 Api 并该产品发布后，开发人员可以订阅该产品并且开始调用 Api。 教程演示这些物品以及高级的产品配置，请参阅[如何创建和配置在 Azure API 管理高级的产品设置][]。

有关如何使用产品的详细信息，请参见下面的视频。

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[查看订阅服务器产品]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[如何创建和使用组来管理 Azure API 管理中的开发人员帐户]: api-management-howto-create-groups.md
[如何在创建和配置高级的产品设置 Azure API 管理]: api-management-howto-product-with-rules.md 