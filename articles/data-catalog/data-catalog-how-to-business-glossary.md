<properties
    pageTitle="如何设置业务术语表，用于控制标记 |Microsoft Azure"
    description="如何文章突出在 Azure 数据目录中的业务术语表来定义和使用标记的常见业务词汇注册数据资产。"
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/21/2016"
    ms.author="maroche"/>

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>如何设置控制标记为业务术语表

## <a name="introduction"></a>介绍

Azure 数据目录提供了数据源发现，功能，使用户能够很容易地发现并理解执行分析和做出决定所需的数据源。 当用户可以找到并了解了最广泛的可用的数据源，这些发现功能将使最大的影响。

标记一个促进更好地理解资产数据的数据目录功能。 标记可用于某项资产或一列，又容易地发现通过搜索或浏览资产，相关联的关键字和使用户能够更容易地理解上下文和资产的意图。

但是，标记有时会导致其自身的问题。 可以使用标记中引入的问题的一些示例包括︰

1.  用户使用上的一些资产和展开在他人时标记的文本的缩写。 这种不一致阻碍发现的资产，即使目的是具有相同标签标记的资产。
2.  这意味着不同的事物在不同的上下文中的标记。 例如，在客户数据集称为"销售收入"的标签可能意味着按客户的收入但上季度的销售数据集相同的标记可能意味着公司的季度收益。  

为了解决上述及其他类似的挑战，数据目录包括业务术语表。

数据目录的业务术语表允许对文档关键业务术语组织和它们的定义来创建常见的业务词汇。 此治理使整个组织内的使用情况数据的一致性。 在业务术语表中定义了条款，一旦他们可以分配给数据资产在目录中，使用相同的方法作为标记，从而使_控制标记_。

> [AZURE.NOTE] 在这篇文章中所描述的功能是仅在标准版的 Azure 数据目录中可用。 免费版不提供管辖标记或业务术语表的功能。

## <a name="glossary-availability-and-privileges"></a>术语表的可用性和特权

*业务术语表是标准版的 Azure 数据目录中可用。免费版本的数据目录不包含一个术语表。*

业务术语表可以通过数据目录门户的导航菜单中的"术语表"选项。  

![访问的业务术语表](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


数据目录管理员和术语表管理员角色的成员可以创建、 编辑和删除术语表中的业务术语表。 数据目录中的所有用户可以查看的术语定义，并且可以标记资产与术语表。

![添加新术语](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>创建术语表

数据目录管理员和术语表管理员可以通过单击新术语上的创建新的术语按钮以创建包含下列字段的术语︰

* 这一术语的业务定义
* 说明可捕捉的用途或资产列业务规则
* 利益干系人最了解这一术语的列表
* 父级术语，定义了术语分为层次结构


## <a name="glossary-term-hierarchies"></a>词汇表术语层次结构

数据目录业务术语表提供为层次结构的术语描述的业务词汇的能力。 这使公司能够创建更好地代表其业务分类术语的分类。

搜索项的名称必须是唯一的给定级别的层次结构-不允许重复的名称。 一个层次结构中的级别数目没有限制，但层次结构时有三个级别或更少通常更浅显易懂。

层次结构中的业务术语表的使用是可选的。 词汇表术语离开父术语字段保留为空将在词汇表中创建术语平 （非分层） 列表。  

## <a name="tagging-assets-with-glossary-terms"></a>标记的资产与术语表

在目录中定义术语后, 标记资产的经验优化搜索术语表，当用户键入其标签。 数据目录门户会显示匹配用户选择从的术语的列表。 如果用户从列表中选择某个术语表术语添加到该资产为标记 （也 词汇表标记）。 用户还可以选择键入一词不在词汇表 （也来创建新的标记 用户标记）。

![用一个用户标记和两个词汇表标记的数据资产标记](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] 用户标记是标记的唯一在免费版本的数据目录中受支持的类型。

### <a name="hover-behavior-on-tags"></a>将鼠标指针悬停在标记行为
数据目录入口在两种类型的标记是在视觉上不同，带有不同的悬停行为。 当用户悬停在某个用户标记上时，他们可以看到标记文本的用户或用户已添加标记。 当用户悬停在词汇表标记上时，他们还可以看到定义的术语表术语，并将其打开以查看完整的术语定义的业务术语表的链接。

### <a name="search-filters-for-tags"></a>搜索筛选器标记
词汇表标记和用户标记可搜索，也可以作为筛选器搜索中的应用。

## <a name="summary"></a>摘要
在 Azure 数据目录，和管辖标记这样，业务术语表允许数据资产识别、 管理和发现以一致的方式。 业务术语表可以提升组织的用户之间的业务词汇的学习，并支持有意义的元数据捕获，使资产发现和了解 breeze。

## <a name="see-also"></a>请参见

- [业务术语表操作的 REST API 文档](https://msdn.microsoft.com/library/mt708855.aspx)
