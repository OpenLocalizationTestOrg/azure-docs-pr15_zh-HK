<properties 
    pageTitle="如何自定义 Azure API 管理开发人员门户使用模板 |Microsoft Azure" 
    description="了解如何自定义 Azure API 管理开发人员门户网站使用的模板。" 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>如何自定义 Azure API 管理开发人员门户网站使用的模板

Azure 的 API 管理提供了几个自定义功能，允许管理员[自定义开发人员门户的外观和感觉](api-management-customize-portal.md)，以及自定义页面内容的开发人员门户使用一组配置页面本身的内容的模板。 使用[DotLiquid](http://dotliquidmarkup.org/)的语法中和一套提供本地化的字符串资源、 图标和页上的控件，可以非常灵活地配置页面的内容，您可以根据使用这些模板。

## <a name="developer-portal-templates-overview"></a>开发人员门户网站模板概述

开发人员门户网站模板开发人员门户中的 API 管理服务实例的管理员管理。 要管理开发模板，请导航到 Azure 经典门户中 API 管理服务实例，单击**浏览**。

![开发人员门户][api-management-browse]

如果您已经在出版商门户，您可以通过单击**开发人员门户**访问开发人员门户。

![开发人员门户菜单][api-management-developer-portal-menu]

要访问开发人员门户网站模板，请单击左侧显示自定义菜单上的自定义图标，单击**模板**。

![开发人员门户网站模板][api-management-customize-menu]

模板列表显示多种类别涵盖开发人员门户中的不同页面的模板。 每个模板都不同，但对其进行编辑和发布所做的更改的步骤都是相同。 若要编辑模板，请单击模板的名称。

![开发人员门户网站模板][api-management-templates-menu]

单击模板可转到开发人员门户页，该页可自定义该模板。 在此示例中的**产品列表**显示模板。 **产品列表**模板控制由红色矩形屏幕区域。 

![产品列表模板][api-management-developer-portal-templates-overview]

某些模板，像**用户配置文件**的模板，自定义同一页面的不同部分。 

![用户配置文件模板][api-management-user-profile-templates]

为每个开发人员门户模板编辑器具有两个部分显示在页面的底部。 左侧显示的模板，请编辑窗格和右侧将显示模板的数据模型。 

模板编辑窗格中包含控件的外观和行为的开发人员门户中相应的页面的标记。 在模板中的标记使用了[DotLiquid](http://dotliquidmarkup.org/)的语法。 DotLiquid 的一个流行编辑器是[DotLiquid 的设计](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)。 编辑过程中，对模板所做的任何更改都实时显示在浏览器中，但不是您的客户可以看到直到您[保存](#to-save-a-template)并[发布](#to-publish-a-template)模板。

![模板标记][api-management-template]

**模板数据**窗格中提供可供特定的模板中使用的实体数据模型的指南。 它通过显示开发人员门户中当前显示的实时数据提供了本指南。 您可以展开模板窗格通过单击**数据模板**窗格右上角的矩形。

![模板数据模型][api-management-template-data]

在前面的示例有两个产品开发人员门户中显示，取自**模板数据**窗格中显示的数据，如下面的示例中所示。

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

**产品列表**模板中的标记处理的数据，从而提供所需的输出循环访问集合的产品向每个单独的产品显示的信息和链接。 注意`<search-control>`和`<page-control>`标记中的元素。 这些控制搜索和分页页面上的控件的显示方式。 `ProductsStrings|PageTitleProducts`为本地化的字符串引用包含`h2`页的页眉文本。 列表的字符串资源、 页控件和图标可用于在开发人员门户网站模板中使用，请参阅[API 管理开发人员门户网站模板参考](https://msdn.microsoft.com/library/azure/mt697540.aspx)。

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>若要保存模板

要保存模板，请单击在模板编辑器中保存。

![保存模板][api-management-save-template]

在发布才会实时开发人员门户中保存的更改。

## <a name="to-publish-a-template"></a>若要发布的模板

已保存的模板可以分别或一起发布。 若要发布一个单独的模板，单击发布模板编辑器中。

![发布模板][api-management-publish-template]

单击**是**以确认并使该模板开发人员门户上的活。

![确认将发布][api-management-publish-template-confirm]

若要发布所有未发布当前模板版本，请在模板列表中单击**发布**。 未发布的模板由模板名称后面的星号指定。 在此示例中，正在发布**产品列表**和**产品**模板。

![发布模板][api-management-publish-templates]

单击**发布自定义项**以确认。

![确认将发布][api-management-publish-customizations]

新发布的模板是开发人员门户中立即生效。

## <a name="to-revert-a-template-to-the-previous-version"></a>若要恢复到以前版本的模板

若要恢复到以前的已发布版本的模板，请单击还原在模板编辑器。

![还原模板][api-management-revert-template]

单击**是**确认。

![确认][api-management-revert-template-confirm]

还原操作完成后，为开发人员门户中实时模板的以前发布的版本。

## <a name="to-restore-a-template-to-the-default-version"></a>若要还原为默认版本的模板

还原到其默认版本的模板是一个两步过程。 首先必须还原的模板，然后必须将已恢复的版本发布。

要恢复为默认版本的单个模板单击模板编辑器中的还原。

![还原模板][api-management-reset-template]

单击**是**确认。

![确认][api-management-reset-template-confirm]

若要还原到其默认版本的所有模板，模板列表上单击**都还原默认模板**。

![还原模板][api-management-restore-templates]

已还原的模板必须再发布分别或同时通过[将模板发布](#to-publish-a-template)中的步骤。

## <a name="developer-portal-templates-reference"></a>开发人员门户网站模板参考

对于开发人员门户网站模板、 字符串资源、 图标和页上控件的参考信息，请参阅[API 管理开发人员门户网站模板参考](https://msdn.microsoft.com/library/azure/mt697540.aspx)。

## <a name="watch-a-video-overview"></a>观看视频概述

观看下面的视频以了解如何使用模板开发人员门户中的 API 和操作页中添加讨论板和分级。

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







