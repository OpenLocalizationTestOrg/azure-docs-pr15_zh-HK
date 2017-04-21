<properties 
    pageTitle="如何使用属性在 Azure API 管理策略" 
    description="了解如何在 Azure API 管理策略中使用属性。" 
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


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>如何使用属性在 Azure API 管理策略

API 管理策略是一个强大的功能，允许发布者更改通过配置 API 的行为的系统。 策略是在请求按顺序执行的语句的集合或一个 API 的响应。 可以使用文本值，策略表达式和属性构造策略声明。 

每个 API 管理服务实例具有键/值对的服务实例的全局属性的集合。 这些属性可以用于所有 API 配置和策略都管理常量字符串值。 每个属性具有下列特性。


| 属性 | 类型            | 说明                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| 名称      | 字符串          | 属性的名称。 它可以包含字母、 数字、 句点、 短划线，和下划线字符。 |
| 值     | 字符串          | 属性的值。 它不能为空或只包含空白。                           |
| 秘密    | 布尔值         | 确定值的秘密，或不应该进行加密。                                |
| 标记      | 字符串的数组 | 可选标记时提供可用于筛选的属性列表。                               |

在**属性**选项卡上的出版商门户配置属性。 在以下示例中，配置三个属性。

![属性][api-management-properties]

属性值可以包含字符串和[策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)。 下表显示了前面三个示例属性和它们的特性。 值`ExpressionProperty`是一个策略表达式，返回一个包含当前日期和时间字符串。 该属性`ContosoHeaderValue`被标记为机密，因此不显示其值。

| 名称               | 值                      | 秘密 | 标记    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | 假  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | 真   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | 假  |         |

## <a name="to-use-a-property"></a>若要使用属性

要在策略中使用属性，将在一双对类似的大括号内的属性名称`{{ContosoHeader}}`，如下面的示例中所示。

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

在此示例中，`ContosoHeader`用作中的标头的名称`set-header`策略，和`ContosoHeaderValue`用作该标头的值。 当此策略计算期间的请求或响应 API 管理网关， `{{ContosoHeader}}` ，`{{ContosoHeaderValue}}`替换的相应属性值。

属性可以用作完整特性或元素值前面的示例中所示，但也可插入或它们与文本表达式，如下面的示例中所示的部件组合在一起︰`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

属性还可以包含策略表达式。 在以下示例中，`ExpressionProperty`使用。

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

这项策略是在计算时，`{{ExpressionProperty}}`替换它的值为︰ `@(DateTime.Now.ToString())`。 由于值为策略表达式，则计算该表达式，策略继续其执行。

调用的范围中的属性具有的策略操作，可以测试此操作，开发人员门户。 在以下示例中，与前面的示例调用操作`set-header`策略的属性。 请注意该响应包含两个属性中使用策略配置的自定义标头。

![开发人员门户][api-management-send-results]

如果您看一下[API 检查器跟踪](api-management-howto-api-inspector.md)调用包含属性与前面的两个示例策略，您可以看到两个`set-header`插入以及策略表达式计算包含策略表达式的属性的属性值的策略。

![API 检查器跟踪][api-management-api-inspector-trace]

请注意，属性值可以包含策略表达式，而属性值不能包含其他属性。 如果文本中包含的属性引用使用属性的值，如`Property value text {{MyProperty}}`，该属性引用不会被替换，并且将属性值的一部分。

## <a name="to-create-a-property"></a>创建属性

若要创建一个属性，单击**属性**选项卡上**添加属性**。

![添加属性][api-management-properties-add-property-menu]

**名称**和**值**是所需的值。 如果此属性值是一个秘密，选中**这是一个秘密**复选框。 输入一个或多个可选的标记，以帮助组织您的属性，然后单击**保存**。

![添加属性][api-management-properties-add-property]

当保存一个新属性时，**搜索属性**文本框填充与新属性的名称并显示新的属性。 若要显示所有属性，请清除**搜索属性**文本框并按 enter 键。

![属性][api-management-properties-property-saved]

创建一个属性，该属性使用 REST API 的信息，请参阅[创建一个属性，该属性使用 REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put)。

## <a name="to-edit-a-property"></a>若要编辑属性

若要编辑某个属性，请单击可编辑属性旁边的**编辑**。

![编辑属性][api-management-properties-edit]

根据需要进行更改，然后单击**保存**。 如果您更改的属性名，引用该属性的任何策略将自动更新以使用新的名称。

![编辑属性][api-management-properties-edit-property]

编辑属性使用 REST API 的信息，请参阅[编辑属性使用 REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch)。

## <a name="to-delete-a-property"></a>要删除属性

要删除属性，请单击要删除的属性旁边的**删除**。

![删除属性][api-management-properties-delete]

单击**是，删除它**以确认。

![确认删除][api-management-delete-confirm]

>[AZURE.IMPORTANT] 如果该属性引用的任何策略，将无法成功地将其删除之前从所有使用该策略中删除属性。

删除一个属性，该属性使用 REST API 的信息，请参阅[删除一个属性，该属性使用 REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete)。

## <a name="to-search-and-filter-properties"></a>搜索和筛选器属性

**属性**选项卡包括搜索和筛选功能来帮助您管理您的属性。 若要筛选由属性名的属性列表，请在**搜索属性**的文本框中输入搜索条件。 若要显示所有属性，请清除**搜索属性**文本框并按 enter 键。

![搜索][api-management-properties-search]

按标记值筛选属性列表，输入**标记筛选器**文本框中的一个或多个标记。 若要显示所有属性，请清除**筛选标记的**文本框按 enter 键。

![筛选器][api-management-properties-filter]

## <a name="next-steps"></a>下一步行动

-   了解更多有关使用策略
    -   [在 API 管理策略](api-management-howto-policies.md)
    -   [策略引用](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>观看视频概述

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

