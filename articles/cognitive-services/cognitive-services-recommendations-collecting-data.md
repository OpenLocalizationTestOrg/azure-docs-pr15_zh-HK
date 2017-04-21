<properties
    pageTitle="收集数据来训练您的模型︰ 机器学习建议 API |Microsoft Azure"
    description="Azure 的机器学习的建议-收集数据来训练您的模型"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>收集数据来训练您的模型 #

从您过去的交易记录，若要查找的项目应该是学习建议 API 推荐给特定用户。

创建一个模型后，您将需要提供两条信息，然后才能执行任何的培训︰ 编录文件，并使用数据。

>   如果您没有完成此操作，我们建议您完成[快速入门指南](cognitive-services-recommendations-quick-start.md)。


## <a name="catalog-data"></a>目录数据 ##

### <a name="catalog-file-format"></a>编录文件格式 ###

编录文件包含有关项目的信息将提供给您的客户。
目录数据应遵循以下格式︰

- 而功能-不`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- 与功能-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>编录文件中的示例行

没有功能︰

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

使用功能︰

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>格式的详细信息

| 名称 | 必填字段 | 类型 |  说明 |
|:---|:---|:---|:---|
| 项 Id |是的 | [A-z]、 [a-z]，[0-9] [_] & #40;下划线 & #41;，[-] & #40; 破折号 & #41;<br> 最大长度︰ 50 | 某一项的唯一标识符。 |
| 项名称 | 是的 | 任何字母数字字符<br> 最大长度︰ 255 | 项名称。 |
| 项目类别 | 是的 | 任何字母数字字符 <br> 最大长度︰ 255 | 该项目所属类别 （例如烹饪书籍，戏曲...）;可以为空。 |
| 说明 | 不行，除非功能存在 （但可以为空） | 任何字母数字字符 <br> 最大长度︰ 4000 | 此项目的说明。 |
| 功能列表 | 不 | 任何字母数字字符 <br> 最大长度︰ 4000;功能︰ 20 的最大数目 | 功能名称的逗号分隔列表 = 可用于增强模型建议的功能值。|

#### <a name="uploading-a-catalog-file"></a>上载目录文件

查看[API 参考](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1)目录文件上载。  

请注意，该编录文件的内容应作为请求正文。

如果多个目录文件上载到具有多个调用相同的模型后，我们将插入只新的目录项。 现有的项目将保留其原始值。 不能使用此方法来更新目录数据。

>   注意︰ 最大文件大小为 200 MB。
>   在目录中支持的最大项数为 100000 项条目。


## <a name="why-add-features-to-the-catalog"></a>向目录中添加功能为何？

推荐引擎创建统计模型，告诉您哪些项目有可能会喜欢或由客户购买。 当您有永远不会与之交互的新产品不能共同出现在单独创建一个模型。 假设您启动提供了一个新"儿童小提琴"在您的存储，因为您永远不会有销售该小提琴之前无法知道哪项建议与该小提琴。

话虽如此，如果引擎知道该小提琴的信息 （即音乐器材，孩子年龄 7-10，不高的小提琴，等等），然后该引擎可以了解从其他具有类似功能的产品。 例如，过去销售小提琴的并且通常购买 violins 的人倾向于购买古典音乐 Cd 和活页乐谱的立场。  系统可以找到这些特征之间的连接，并提供基于功能，而您新的小提琴已经很少使用的建议。

作为部分的目录数据，导入功能并且然后秩 （或模型中特征的重要性） 关联等级生成完成时。 功能级别可以因使用数据和类型的项目的模式。 但排名应一致使用项目，具有较小的变动。 功能级别是一个非负数字。 数字 0 表示功能已不旺 （如果调用此 API 之前的第一个名次生成完成时发生）。 排名属性化的日期被称为分数常用常新。


###<a name="features-are-categorical"></a>功能是分类

这意味着您应创建类似于一种类别的功能。 例如，价格 = 9.34 不是一个分类的功能。 另一方面，一种功能类似 priceRange = Under5Dollars 是一个分类的功能。 另一个常见错误是项的作为一项功能使用的名称。 这会导致项目是唯一的因此它将描述某一类别的名称。 确保功能表示的项目类别。


###<a name="how-manywhich-features-should-i-use"></a>应使用方式的多/功能？


最后建议建立支持最多 20 个功能使用构建一个模型。 您也可以在您的目录项将 20 多个功能，但您应执行级别生成并选取的功能仅排名高的。 （秩为 2.0 或更多的功能是非常好的功能，以便使用 ！）。 


###<a name="when-are-features-actually-used"></a>何时实际使用功能？

当没有足够的事务数据在单独的事务信息上提供建议时，模型会使用功能。 因此功能将在"冷项目"--项目交易记录很少对影响最大。 如果您的全部项目有足够的交易记录信息，您可能不需要丰富的功能模型。


###<a name="using-product-features"></a>使用产品功能

若要使用您的生成所需的一部分功能︰

1. 请确保您的目录具有功能时将其上载。

2. 触发排名生成。 它能做的重要性/排名的功能分析。

3. 触发生成的建议，设置以下生成参数︰ 设为 true，则返回 true，allowColdItemPlacement useFeaturesInModel，modelingFeatureList 应设置为您想要用来增强您的模型的功能以逗号分隔列表。 有关详细信息，请参阅[建议生成类型参数](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0)。





## <a name="usage-data"></a>使用率数据 ##
使用文件包含有关如何使用这些项目，或从您的业务的交易记录的信息。

#### <a name="usage-format-details"></a>使用格式详细信息
使用文件是使用文件中的每行表示用户和项目之间的交互的 CSV （逗号分隔值） 文件。 每行的格式，如下所示︰<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| 名称  | 必填字段 | 类型 | 说明
|-------|------------|------|---------------
|用户 Id|         是的|[A-z]、 [a-z]，[0-9] [_] & #40;下划线 & #41;，[-] & #40; 破折号 & #41;<br> 最大长度︰ 255 |用户的唯一标识符。
|项 Id|是的|[A-z]、 [a-z]，[0-9] [& #95;] & #40;下划线 & #41;，[-] & #40; 破折号 & #41;<br> 最大长度︰ 50|某一项的唯一标识符。
|时间|是的|日期格式︰ YYYY/MM/DDTHH:MM:SS (例如 2013年/06/20T10:00:00)|数据的时间。
|事件|不 | 下列选项之一︰<br>• 单击<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• 购买| 交易记录的类型。 |

#### <a name="sample-rows-in-a-usage-file"></a>使用文件中的示例行

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>使用文件上载

对上载使用文件的[API 参考](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2)来看。
请注意，您需要通过使用文件的内容作为 HTTP 调用的主体。

>  注意︰

>  最大文件大小︰ 200 MB。 您可能会上载使用的多个文件。

>  您需要在开始将使用率数据添加到您的模型之前上载目录文件。 在培训阶段将采用只编录文件中的项。 将忽略所有其他项。

## <a name="how-much-data-do-you-need"></a>您需要多少数据？

您的模型质量较多地依赖于数据的数量和质量。
系统了解到当用户购买不同的项目 （我们称此联的事件）。 对于 FBT 的生成，还有一点需要知道哪些项目购买同样的交易记录中。 

好的经验法则是让大多数项目处于 20 交易记录或更多，因此，如果您在您的目录中有 10000 项，我们建议您有至少 20 次大约 200000 的交易记录的交易记录的数量。 再次，这是经验法则。 您需要对您的数据进行试验。

一旦您创建了一个模型，您可以执行[离线计算](cognitive-services-recommendations-buildtypes.md)检查模型可能执行程度。
