
<properties
    pageTitle="在批处理中获取建议︰ 机器学习建议 API |Microsoft Azure"
    description="Azure 机器学习建议 — 分批获得建议"
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
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>在批处理中获得建议

>[AZURE.NOTE] 批次中获得的建议会比每次收到建议一个更复杂。 检查如何获取单个请求的建议有关的信息的 Api:

> [项目到项目的建议](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [用户对项目的建议](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> 批次评分仅适用于 7 月 21，2016年之后创建的生成。


还有一些情况下，您需要在其中一次获得多项的建议。 例如，您可能有兴趣创建建议缓存或甚至分析建议，您得到的类型。

计分的批处理操作，如我们所说的是异步操作。 您需要提交请求，等待该操作完成后，，然后收集您的结果。  

若要使用更多精确，这些是要遵循的步骤︰

1.  如果您还没有，创建 Azure 存储容器。
2.  描述您建议请求到 Azure Blob 存储的每个输入的文件上传。
3.  启动批处理作业评分。
4.  等待异步操作完成。
5.  完成操作后，请从 Blob 存储收集的结果。

让我们看一下每个步骤。

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>如果您还没有，创建存储容器

转到[Azure 的门户](https://portal.azure.com)，如果您还没有创建新的存储帐户。 若要执行此操作，请导航到**新建** > **数据** + **存储** > **存储帐户**。

您有存储帐户后，您需要创建 blob 容器将存储的输入和输出的批处理执行。

描述输入文件上载您建议的每个请求到 Blob 存储 — 我们这里叫文件 input.json。
一个容器后，您需要上载的文件描述了每个请求，您需要执行从建议服务。

一批可以执行某个特定生成中只有一种类型的请求。 我们将介绍如何在下一节中定义此信息。 现在，让我们假设，我们将执行从特定生成的项目建议。 输入的文件则包含为每个请求 （在此情况下，种子项目） 输入的信息。

这是一种 input.json 文件如下所示︰

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

如您所见，该文件是一个 JSON 文件，其中每个请求都有发送建议请求所需的信息。 创建类似 JSON 文件请求，您需要完成，并将其复制到您刚创建 Blob 存储容器。

## <a name="kick-start-the-batch-job"></a>启动批处理作业

下一步是提交新的批处理作业。 有关详细信息，请检查[API 参考](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/)。

请求正文的 api 需要定义在其中输入、 输出和错误文件需要存储的位置。 它还需要定义是访问这些位置所需的凭据。 此外，您需要指定几个参数适用于整批 （类型请求，请在模型/生成来使用，每次调用，结果数等的建议。）

这是一种请求主体应如下所示︰

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

此处需要注意几个重要事项︰

-   目前， **authenticationType**应始终设置为**PublicOrSas**。

-   您需要获取共享访问签名 (SAS) 标记为允许建议 API，用于读取和写入从到 Blob 存储帐户。 在[建议 API 页](../storage/storage-dotnet-shared-access-signature-part-1.md)中可以找到有关如何生成 SAS 令牌的详细信息。

-   目前支持仅**apiName**是**ItemRecommend**，用来进行项目到项目的建议。 批处理当前不支持用户对项目的建议。

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>等待异步操作完成

当您启动批处理操作时，响应将返回操作位置标头，用来跟踪该操作所需的信息。
您可以通过使用[检索操作状态 API]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da)，用于跟踪生成操作的操作一样跟踪操作。

## <a name="get-the-results"></a>得到的结果

完成该操作，假设没有错误后，您可以从您的输出收集结果 Blob 存储。

下面的示例显示输出可能如下所示。 在此示例中，我们将展示一批 （为简便起见） 只有两个请求的结果。

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>了解有关限制

-   每个订阅一次可以调用一个批处理作业。
-   批处理作业输入的文件不能超过 2 MB。
