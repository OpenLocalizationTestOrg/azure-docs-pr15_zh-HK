<properties
    pageTitle="生成类型和模型的质量︰ 建议 API |Microsoft Azure"
    description="Azure 机学习建议-快速入门指南"
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
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>生成类型和模型的质量 #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>支持的生成类型 ##

建议 API 目前支持两种生成类型︰*推荐*和*FBT*。 每个生成使用不同的算法，每一个都有不同的优势。 此文档介绍了每个这些生成和比较生成的模型质量的方法。

如果您没有完成此操作，我们建议您完成[快速入门指南](cognitive-services-recommendations-quick-start.md)。

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>建议的生成类型 ###

建议生成类型使用矩阵因数分解提供建议。 它生成基于您的交易记录以描述每个项目，[潜在的特征](https://en.wikipedia.org/wiki/Latent_variable)向量，然后使用这些潜在的向量比较相似的项目。

如果培训模型基于在电子产品商店中购买和提供 Lumia 650 电话作为模型的输入时，模型将返回一组倾向的人可能会购买 Lumia 650 电话购买的项目。 这些项目可能无法互补。 在此示例中，则可能因为喜欢 Lumia 650 的人可能会像其它手机模型，将返回其它手机。

建议生成具有两个功能，使其更具吸引力︰

**建议生成支持*冷项目*位置 * *

没有长时间使用的项目称为冷项目。 例如，如果您收到的电话您永远不会有销售之前装运，系统无法推断该产品的交易记录的单独的建议。 这意味着系统应学习从产品本身有关的信息。

如果您想要使用冷项目位置，您需要提供您的项目目录中的每个功能信息。 下面是目录的您的前几行可能如下所示 （注意键 = 值格式的功能）。

>Pro2 6CX 00001、 曲面、 曲面、 键入 = 硬件，存储 = 128gb 的容量，内存 = 4 G，制造商 = Microsoft

>73 H 00013，唤醒 Xbox 360 游戏、 类型 = 软件、 语言 = 英语，评估值 = 成熟

>忠实爱好者 0F05、 Minecraft Xbox 360 游戏、 * 类型 = 软件、 语言 = 西班牙语，评估值 = 青年

您还需要设置以下生成参数︰

| 生成参数         | 备注
|------------------     |-----------
|*useFeaturesInModel*     | 设置为**true**。  指示是否功能可用于增强的推荐模型。
|*allowColdItemPlacement*   | 设置为**true**。 指示是否建议应还推冷项目通过特征的相似性。
| *modelingFeatureList*   | 功能可以使用建议版本中增强建议的名称的逗号分隔列表。 例如，"语言，存储"对于前面的示例。

**建议生成支持用户的建议**

建议生成支持[用户的建议](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)。 这意味着，它可以提供基于其交易历史记录创建为用户的个性化的建议。 用户的建议，可以提供用户 ID 或用户的交易记录的最近历史记录。

一个典型示例，您可能希望应用用户的建议是在登录上欢迎页面。 那里可以推广应用于特定用户的内容。

也可以在用户即将签出时应用建议生成类型。 此时，您有用户即将购买时，项的列表并且可以提供基于当前的市场购物篮的建议。

#### <a name="recommendations-build-parameters"></a>建议构建参数

| 名称  |   说明 |    键入时， <br>  有效的值 <br> （默认值）
|-------|-------------------|------------------
| *NumberOfModelIterations* |   执行模型的迭代次数所反映的总体计算时间和模型精度。 越高数量，更准确地那么模型，但计算时间较长。  |   整数， <br>  10 至 50 <br>(40)
| *NumberOfModelDimensions* |   维数与相关的模型将尝试在您的数据中找到的功能的数量。 增加的维度数将允许更好地为较小的簇微调的结果。 但是，维度太多将会阻止模型查找项之间的关联性。 |  整数， <br> 10 至 40 <br>(20) |
| *ItemCutOffLowerBound* |  定义项应为其被认为是模型的一部分在使用点的最小数目。 |        整数， <br> 2 个或更多 <br> (2) |
| *ItemCutOffUpperBound* |  定义项应在它被认为是模型的一部分的使用点的最大数目。 |  整数， <br>2 个或更多<br> (2147483647) |
|*UserCutOffLowerBound* |   定义用户必须具有执行被认为是模型的一部分的交易记录的最小数目。 | 整数， <br> 2 个或更多 <br> (2)
| *UserCutOffUpperBound* |  定义用户必须具有执行被认为是模型的一部分的交易记录的最大数目。 | 整数， <br>2 个或更多 <br> (2147483647)|
| *UseFeaturesInModel* |    指示是否功能可用于增强的推荐模型。 |     布尔值<br> 默认值︰ True
|*ModelingFeatureList* |    功能可以使用建议版本中增强建议的名称的逗号分隔列表。 它取决于非常重要的功能。 |    最多 512 个字符的字符串，
| *AllowColdItemPlacement* |    指示是否建议应还推冷项目通过特征的相似性。 | 布尔值 <br> 默认值︰ False
| *EnableFeatureCorrelation*    | 指示是否在推理中可用功能。 | 布尔值 <br> 默认值︰ False
| *ReasoningFeatureList* |  以逗号分隔列表中的功能的名称用于推理方面的句子，如建议的解释。 它取决于对客户很重要的功能。 | 最多 512 个字符的字符串，
| *EnableU2I* | 启用个性化的建议，也称为用户项 (U2I) 的建议。 | 布尔值 <br>默认值︰ True
|*EnableModelingInsights* | 定义是否应执行离线计算来收集建模的见解 （即精度和多样性指标）。 如果设置为 true，数据的一个子集不能用于培训，因为它需要保留模型的测试。 阅读有关[脱机评估](#OfflineEvaluation)详细信息。 | 布尔值 <br> 默认值︰ False
| *SplitterStrategy* | 如果设置为*true*时启用建模的见解，这是如何为评估目的而拆分数据。  | 字符串、 *RandomSplitter*或*LastEventSplitter* <br>默认值︰ RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>FBT 生成类型 ###

经常购买组合在一起 (FBT) 生成执行两个或三个不同的产品联合在一起出现的次数进行计数分析。 然后对基于相似性函数 （**共同出现**， **Jaccard**，**提起**） 集进行排序。

**Jaccard**和**提起**看作正常化共同出现的方法。  这意味着这些物料才会退回他们售票点和种子项目。

在 Lumia 650 电话示例中，仅当在同一会话中作为 Lumia 650 电话购买电话 X 将返回电话 X。 这可能是不太可能发生，因为我们希望如此项补充到 Lumia 650 返回;例如，屏幕保护程序或 Lumia 650 的电源适配器。

目前，两项被认为购买同一会话中，如果它们具有相同的用户 ID 和时间戳的交易中出现。

FBT 生成不支持冷项目，因为根据定义他们希望能购买同一事务中的两个项目。 虽然 FBT 生成可以返回的项目 （三个） 集，它们不支持个性化的建议，因为它们接受作为输入的单个种子项目。


#### <a name="fbt-build-parameters"></a>FBT 生成参数

| 名称  |   说明 |       键入时，  <br> 有效的值 <br> （默认值）
|-------|---------------|-----------------------
| *FbtSupportThreshold* | 如何保守是模型。 共同的次数要被视为用于建模的项目。 |  整数， <br> 3 50 <br> (6)
| *FbtMaxItemSetSize* | 界限频繁集中的项目的数。| 整数  <br> 2-3 <br> (2)
| *FbtMinimalScore* | 频繁集应具有要包含在返回的结果中的最低得分。 越高越好。 | 双 <br> 0 及以上 <br> (0)
| *FbtSimilarityFunction* | 定义要由该生成使用的相似性函数。 **提起**倾向于运气、**联次**倾向于可预测性和**Jaccard**是两者之间的妥协。 | 字符串，  <br>  <i>cooccurrence，提起 jaccard</i><br> 默认值︰ <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>建立评估和选择 ##

本指南可以帮助您确定是否应使用建议版本或 FBT 生成，但它没有提供一个明确的答案，在情况下，您可以使用其中的一个。 另外，即使您知道您想要使用 FBT 的生成类型，您可能仍要选择**Jaccard**或作为相似性函数**提起**。

两个不同版本之间进行选择的最好办法是对其进行测试，在现实生活中 （在线评估） 和跟踪不同版本的转换率。 无法根据建议单击测量转换率、 实际数购买建议所示，或甚至在实际采购金额时所显示的不同建议。 您可以选择您根据您的业务目标的转换率指标。

在某些情况下，可能需要将其投入生产之前评估模型的离线。 而离线计算不是在线评估的替代，它可以作为一个指标。

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>离线计算  ##

离线计算的目标是预测精度 （将购买推荐项目之一的用户数） 和建议 （建议的项目数） 的多样性。
作为精度和多样性指标评估的一部分，系统查找用户的示例并拆分这些用户分为两组的交易记录︰ 训练数据集和测试集。

> [AZURE.NOTE]若要使用脱机的衡量标准，必须在您使用数据时间戳。
> 时间数据时需要拆分训练和测试数据集之间的正确用法。

> 此外，离线计算可能不产生任何小使用文件的结果。 进行评估是全面的应在测试数据集的 1000 个使用点的最小。

<a name="Precision"></a>
### <a name="precision-at-k"></a>精度在 k ###
下表表示的精度在 k 离线计算的输出。

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|百分比 |   13.75 | 18.04   | 21 |  24.31 | 26.61
|在测试中的用户 |    10000 |    10000 |    10000 |    10000 |    10000
|用户被视为 | 10000 |    10000 |    10000 |    10000 |    10000
|用户不会被视为 | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
在前面的表中， *k*表示显示给客户的建议的数目。 表中读取，如下所示:"如果在测试期间，向客户显示的只是一个建议，只有 13.75 个用户将已购买的建议。" 此语句基于假定模型的训练与采购数据。 另一种方式说这番话是在 1 处的精度为 13.75。

您会注意到，更多的项目显示给客户，客户购买的推荐的项目的可能性上升。 对于前面的实验中，概率几乎两倍 26.61%时 5 项建议。

#### <a name="percentage"></a>百分比
显示与*k*建议中至少一个交互的用户的百分比。 计算百分比除以视为用户总数由至少一个建议与交互的用户数。 用户认为的详细信息，请参阅。

#### <a name="users-in-test"></a>在测试中的用户
在该行中的数据表示的测试数据集的用户的总数。

#### <a name="users-considered"></a>用户被视为
只有系统至少建议*k*项基于模型生成使用练习数据集时，才会考虑用户。

#### <a name="users-not-considered"></a>用户不会被视为
在该行中的数据表示不考虑任何用户。 没有至少收到用户*k*推荐项目。

用户不会被视为 = 用户认为用户在测试 –

<a name="Diversity"></a>
### <a name="diversity"></a>多元化 ###
多样性的指标来衡量的建议的项目的类型。 下表表示的多样性的离线计算的输出。

|百分点值存储桶 |    0-90|  90-99| 99-100
|------------------|--------|-------|---------
|百分比        | 34.258 | 55.127| 10.615


汇总项目建议︰ 100000

建议的唯一项︰ 954

#### <a name="percentile-buckets"></a>百分点值存储桶
每个百分点桶的范围 （介于 0 和 100 之间的范围的最小值和最大值） 表示。 近 100 项是最受欢迎的项目，并接近于 0 的项是最不受欢迎。 例如，如果百分比值为 99-100%桶 10.6，它意味着 10.6%的建议返回仅顶端的 1%最受欢迎的项目。 最小值的 %桶属于价内税，和最大值是独占的除了 100。
#### <a name="unique-items-recommended"></a>建议的唯一项目
建议使用跃点数的唯一数据项显示不同评估返回的项的数目。
#### <a name="total-items-recommended"></a>建议的项目总数
总项目建议指标显示的数项建议。 某些可能重复项。

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>离线计算度量值 ###
精度和多样性的离线指标可能有用，当您选择要使用的生成。 在生成时为各自 FBT 或建议的一部分生成参数︰

-   *EnableModelingInsights*生成参数设置为**true**。
-   （可选） 选中*splitterStrategy* (或者*RandomSplitter*或*LastEventSplitter*)。
*RandomSplitter*将拆分训练中的使用率数据和测试设置基于给定的*randomSplitterParameters*测试 %和随机的种子值。
*LastEventSplitter*将拆分训练中的使用率数据和测试集基于每个用户最后一次交易记录。

该操作将触发生成的数据的子集用于培训和使用其余的数据来计算评估指标。  生成完成后，若要获取输出的计算中，您需要调用[Get 生成标准 API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f)，传递各自的*modelId*和*buildId*。

 下面是示例计算的 JSON 输出。


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
