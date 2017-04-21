<properties
    pageTitle="快速入门指南︰ 机器学习的文本分析 Api |Microsoft Azure"
    description="Azure 的机器学习的文本分析的快速入门指南"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>文本分析 Api 来检测主观意图、 关键的短语，主题和语言入门

<a name="HOLTop"></a>

此文档介绍了如何为板载您的服务或应用程序中使用[文本分析 Api](//go.microsoft.com/fwlink/?LinkID=759711)。
您可以使用这些 Api 来检测主观意图、 关键的短语，主题和语言从您的文本。 [单击此处可查看交互体验的演示。](//go.microsoft.com/fwlink/?LinkID=759712)

请参阅[API 定义](//go.microsoft.com/fwlink/?LinkID=759346)的 Api 的技术文档。

本指南适用于版本 2 的 Api。 版本 1 的 Api，[请参阅本文档](../machine-learning/machine-learning-apps-text-analytics.md)的详细信息。

本教程结束时，您将能够以编程方式检测︰

- **观点**的是正的或负的文本？

- **关键短语**-什么是人们在单篇文章中讨论过？

- **主题**-什么是人们在很多文章讨论？

- **语言**的语言文本编写的吗？

请注意，此 API 费用提交的单据，每 1 宗交易。 举一个例子，如果您请求 1000年文档在一个调用中的主观意图将扣除 1000年的交易记录。



<a name="Overview"></a>
## <a name="general-overview"></a>一般概述 ##

本文是一个分步指南。 我们的目标是指导您完成训练模型，并以您指向资源，您可以将其投入生产所必需的步骤。 本练习将需要大约 30 分钟。

对于这些任务，您需要一个编辑器并调用 rest 风格的终结点以您选择的语言。

让我们开始吧 ！

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>任务 1-为文本分析 Api 签名 ####

在此任务中，您将注册文字分析服务。

1. 导航到[Azure 门户](//go.microsoft.com/fwlink/?LinkId=761108)中**认知服务**并确保作为 API 类型选中了**文本分析**。

1. 选择一个计划。 您可以选择**可用层每个月 5000 的交易记录**。 因为是免费的计划，将不收取使用该服务。 您将需要登录到 Azure 订购。 

1. 完成其他字段并创建您的帐户。

1. 您注册文本分析后，发现您的**API 密钥**。 复制主键，使用 API 服务时将需要它。


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>任务 2-检测到主观意图、 关键的词组和语言 ####

很容易地检测在文本中的主观意图、 关键的词组和语言。 返回[演示体验](//go.microsoft.com/fwlink/?LinkID=759712)，以编程方式将得到相同的结果。

>[AZURE.TIP] 观点分析，我们建议您将文本拆分成句子。 这通常会导致较高的精度，在主观意图的预测。

请注意，支持的语言如下所示︰

| 功能 | 支持的语言代码 |
|:-----|:----|
| 观点 | `en`（英文） `es` （西班牙语） `fr` （法语） `pt` （葡萄牙语） |
| 关键的词组 | `en`（英文） `es` （西班牙语） `de` （德语） `ja` （日语） |


1. 您将需要将标头设置为以下。 请注意，JSON 是当前 Api 只接受输入的格式。 不支持 XML。

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. 接下来，设置在 JSON 中的您输入的行的格式。 对于主观意图、 关键的词组和语言，该格式是相同的。 请注意，每个 ID 应该是唯一的会将系统返回的 ID。 一个文档可以提交的最大大小是 10 KB，并已提交输入最大总大小为 1 MB。 可以一次调用提交 1000 个文档。 速率限制存在 100 个电话，每分钟的速度-因此我们建议您提交一次调用中的文档的数量很大。 语言是可选参数，应指定如果分析非英语文本。 输入的示例如下所示，其中可选参数`language`的观点分析或关键短语抽取是包括︰

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. 先到有观点、 关键的词组和语言的输入系统**开机自检**打电话。 Url 的外观，如下所示︰

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. 此调用将返回 JSON 格式具有 Id 的响应，并且检测到属性。 （与排除错误详细信息） 的主观意图的输出的示例所示。 对于主观意图，将为每个文档返回 0 和 1 之间的分数︰

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>任务 3-检测中的文本集的主题 ####

这是一个新发布的 API 返回前检测到的列表的主题提交文本记录。 主题标识与关键短语，它可以是一个或多个相关的单词。 该 API 旨在适用于较短的人的书面文字评论和用户反馈等。

此 API 要求**最少 100 个文本记录**提交，但旨在检测跨数百至数千条记录的主题。 任何非英语记录或记录少于三句话将被丢弃，因此将不会分配到的主题。 为主题检测单个文档可以提交的最大大小是 30KB，并提交输入最大总大小是 30 MB。 主题检测是限于 5 提交每隔 5 分钟的速率。

有两个其他**可选**的输入的参数，可有助于提高结果的质量︰

- **停止词。**  将从整个主题检测管线中排除这些单词和他们关闭窗体 （如复数）。 使用这样的常用词语 （例如，"问题"、"错误"和"用户"可以适当选择软件有关的客户投诉）。 每个字符串应该是一个单词。
- **停止的短语**-将从返回主题列表中排除这些短语。 用于排除不想在结果中看到的一般主题。 例如，"Microsoft"和"Azure"应该是适当的选择要排除的主题。 字符串可以包含多个单词。

请按照下列步骤来检测您的文本中的主题。

1. Json 格式输入的格式设置。 这一次，可以定义停止词和短语的停止。

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. 任务 2 中的定义使用相同的页眉，请**开机自检**主题终结点的调用︰

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. 这将返回`operation-location`的标题中的响应，其中，值是查询的结果的主题的 URL:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. 查询返回`operation-location`定期使用**GET**请求。 每分钟一次建议。

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. 终结点将返回的响应包括`{"status": "notstarted"}`处理前,`{"status": "running"}`处理时和`{"status": "succeeded"}`的输出一次完成。 然后可以使用将按以下格式 （备注详细信息如本示例从已排除错误格式和日期） 的输出︰

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

请注意，从主题的成功响应`operations`终结点会有下面的架构︰

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

此响应的每个部分的说明如下所示︰

**主题**

| 密钥 | 说明 |
|:-----|:----|
| 标识 | 每个主题的的唯一标识符。 |
| 分数 | 分配给主题的文档数。 |
| keyPhrase | 汇总的单词或短语的主题。 |

**topicAssignments**

| 密钥 | 说明 |
|:-----|:----|
| documentId | 文档的标识符。 相当于输入中包含 ID。 |
| topicId | 已分配给该文档的主题 ID。 |
| 距离 | 文档主题隶属关系介于 0 和 1 之间的分数。 距离越低分数越高主题隶属关系是。 |

**错误**

| 密钥 | 说明 |
|:-----|:----|
| 标识 | 输入文档错误引用的唯一标识符。 |
| 消息 | 错误消息。 |

## <a name="next-steps"></a>下一步行动 ##

祝贺您 ！ 现在，您已经完成对您的数据使用文本分析。 现在，您可能希望考虑使用[电源 BI](//powerbi.microsoft.com)之类的工具来直观地显示数据，以及自动化您的见解，为您提供的文本数据的实时视图。

若要查看如何使用文本分析功能，如观点，作为 bot 的一部分，请在 Bot 框架网站看到[情感机器人](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot)示例。
