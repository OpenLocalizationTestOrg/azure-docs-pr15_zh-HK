<properties
    pageTitle="升级到版本 2 的文本分析 API |Microsoft Azure"
    description="Azure 的机器学习的文本分析-升级到版本 2"
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

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>升级到版本 2 的文本分析 API #

本指南将指导您完成升级您的代码中使用[的 api 的第一个版本](../machine-learning/machine-learning-apps-text-analytics.md)使用第二个版本的过程。 

如果没有使用该 API，并且想要了解更多，可以**[更多了解这里的 API](//go.microsoft.com/fwlink/?LinkID=759711)**或**[按照快速的入门指南](//go.microsoft.com/fwlink/?LinkID=760860)**。 技术参考，请参阅**[API 定义](//go.microsoft.com/fwlink/?LinkID=759346)**。

### <a name="part-1-get-a-new-key"></a>第 1 部分。 获取新密钥 ###

首先，您需要从**Azure 门户网站**获取新的 API 密钥︰

1. 定位到[Cortana 智能库](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2)通过文本分析服务。 在这里，您还会发现文档和代码示例的链接。

1. 单击**注册**。 此链接将带您到 Azure 的管理门户，您可以注册为服务。

1. 选择一个计划。 您可以选择**可用层每个月 5000 的交易记录**。 因为是免费的计划，将不收取使用该服务。 您将需要登录到 Azure 订购。 

1. 您注册文本分析后，将为您提供一个**API 键**。 复制此项时，您将需要使用它时使用的 API 服务。

### <a name="part-2-update-the-headers"></a>第二部分。 更新邮件头 ###

更新已提交的标头值如下所示。 注意帐户密钥不能再编码。

**版本 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**第 2 版**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>第 3 部分。 更新的基 URL ###

**版本 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**第 2 版**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>一部分 4a。 更新主观意图、 关键的词组和语言的格式 ###

#### <a name="endpoints"></a>终结点 ####

获取终结点现在已过时，因此应作为 POST 请求提交的所有输入。 更新为以下所示的终结点。

| |版本 1 单个终结点|版本 1 批次终结点|版本 2 终结点|
|---|---|---|---|
|呼叫类型|获取|发布|发布|
|观点|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|关键的词组|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|语言|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>输入的格式 ####

注意现在接受的唯一的开机自检格式，因此您应重新格式化任何输入以前相应地使用单个文档端点。 输入是不区分大小写。

**版本 1 （批）**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**第 2 版**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>从主观意图的输出 ####

**版本 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**第 2 版**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>从关键的词组的输出 ####

**版本 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**第 2 版**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>从语言的输出 ####


**版本 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**第 2 版**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>一部分 4b。 更新主题的格式 ###

#### <a name="endpoints"></a>终结点 ####

| |版本 1 终结点 | 版本 2 终结点|
|---|---|---|
|提交的主题检测 （开机自检）|```StartTopicDetection```|```topics```|
|获取主题结果 （获取）|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>输入的格式 ####

**版本 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**第 2 版**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>提交结果 ####

**版本 1 （开机自检）**

以前，当该作业完成后，您将收到下面的 JSON 输出，其中作业 Id 将追加到 URL 以获取输出。

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**版本 2 （开机自检）**

响应现在将包含标头值，如下所示，其中`operation-location`作为终结点使用轮询的结果︰

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>操作结果 ####

**版本 1 （获取）**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**版本 2 （获取）**

像以前那样，返回**定期轮询输出**（建议的期间是每分钟） 直到输出。 

完成的主题 API 后，状态读取`succeeded`将返回。 然后，这将包括输出结果如下所示的格式︰

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>第 5 部分。 测试它 ！ ###

现在，您应该很好去 ！ 测试您的代码使用一个小的示例，以确保您可以成功地处理您的数据。
