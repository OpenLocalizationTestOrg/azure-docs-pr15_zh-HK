
<properties
    pageTitle="从 API 的 DataMarket 建议迁移到 Azure 认知服务建议 API |Microsoft Azure"
    description="Azure 的机器学习的建议-建议认知服务迁移"
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
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>从 API 的 DataMarket 建议迁移到 Azure 认知服务建议 API
本文演示如何从[Microsoft DataMarket 建议 API](https://datamarket.azure.com/dataset/amla/recommendations)将迁移到[Microsoft Azure 认知服务建议 API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)。

DataMarket 建议 API 将停止接受新的客户 2016 年 12 月 31，并将被否决 2017，2 月 28。

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>如何开始使用 Azure 认知服务建议 API？

若要迁移到认知服务建议 API，请执行以下操作︰

1.  如果您尚没有 Azure 的预订，其中一个[注册](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1)。 

1.  从[快速启动指南](cognitive-services-recommendations-quick-start.md)注册了一个认知的服务建议 API 和使用方法以编程方式获得逐步说明。 

1.  请转至[登录页上认知服务建议 API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)来了解服务和发现文档。

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>我用建议用户界面来构建我的模型。 有关认知的服务建议 API 是否有类似的工具？

绝对 ！ 新[建议用户界面](http://recommendations-portal.azurewebsites.net/)的 URL 是 http://recommendations-portal.azurewebsites.net。 

>[AZURE.NOTE] 这里 DataMarket 凭据不起作用。 注册订阅的 Azure 门户，并获得使用新[建议用户界面](http://recommendations-portal.azurewebsites.net/)所需的帐户密码。
如果您没有帐户密钥，请参阅任务 1 的[快速入门指南](cognitive-services-recommendations-quick-start.md)。

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>新的 API 格式是 DataMarket 建议 API 相同？

API 支持在 DataMarket 版本中，这些方案为支持相同的方案和流程，但实际 API 接口已被更新，以符合[Microsoft REST API 准则](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)。 Api 是更加一致，工作更好地使用工具支持 Swagger。

要了解每个 Api，签出[API 资源管理器](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db)。
使用*重试*该按钮来测试 API 调用。 尚未更改由建议 API （目录和使用文件） 的文件的格式，因此您可以继续使用相同的文件和/或基础架构生成来生成这些文件。

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>一些认知服务建议 API 中的新功能有哪些？

最近两个月，我们已为认知服务建议 API 发布新功能︰
-   建议的培训和测试而无需编写一行代码模型的用户界面
-   批次评分，以次为您提供数千个建议
-   构建查询的建议模型质量指标支持
-   支持业务规则
-   枚举并下载使用和目录文件的能力
-   秩的生成支持查询项建议模型中的功能质量
-   额外的功能，在目录中的产品搜索

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Microsoft 在停止支持 DataMarket 建议 API？

[建议在 DataMarket 上的 API](https://datamarket.azure.com/dataset/amla/recommendations)将停止后 2016 年 12 月 31，接受新的客户，将被完全否决由 2017，2 月 28。 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>如果没有我需要重新创建我认知的服务建议 API 中的模型的数据？

我们想要实现这一过渡尽可能轻松地为您。 我们可以帮助您将您的旧模型从您的 DataMarket 帐户移到新的 Azure 认知服务建议 API 订阅。 与我们联系，邮箱︰ [mlapi@microsoft.com](mailto://mlapi@microsoft.com)。 我们将要求您提供您 DataMarket 订阅 ID 和您认知服务的订阅 ID，然后我们将与您的新帐户关联模型。
