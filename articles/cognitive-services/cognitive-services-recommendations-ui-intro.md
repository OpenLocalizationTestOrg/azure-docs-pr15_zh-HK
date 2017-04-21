<properties
    pageTitle="Recommnendations 用户界面构建模型 |Microsoft Azure"
    description="Azure 的机器学习建议-建议用户界面使用构建一个模型"
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
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>建议用户界面使用构建一个模型

本文是一个分步指南。 我们的目标是指导您完成培训使用[建议用户界面](https://recommendations-portal.azurewebsites.net/)模型所必需的步骤。
经过练习使您了解构建一个模型，然后以编程方式执行的过程。 它还帮助您熟悉用户界面，这是很方便的随着您开始使用服务。

本练习大约需要 30 分钟。

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>第 1 步-符号中到 UI 的建议 ##

1. 如果您有没有完成此操作，您需要[注册](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1)新[建议 API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)订阅。 您可以注册**Azure**服务[http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1)和符号使用 Azure 的帐户。 *任务 1* [入门指南](cognitive-services-recommendations-quick-start.md)》 中提供了注册过程的详细的说明 

1. 一旦获得**密钥**为您建议 API 的订阅，请转到[建议用户界面](https://recommendations-portal.azurewebsites.net/)。 

1. 在**帐户密钥**字段中，输入您的密钥登录到门户中，然后单击**登录**按钮。

    ![建议用户界面︰ 登录对话框。][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>步骤 2-让我们收集一些培训数据 ##

您可以创建一个生成之前，引擎将需要两条信息︰ 目录文件和一组使用文件。 

编录文件包含有关项目的信息将提供给您的客户。 使用文件包含有关如何使用这些项目，或从您的业务的交易记录的信息。

通常您会查询这些个信息存储数据库。 今天，我们已经为您提供一些示例数据，以便您可以了解如何使用建议的 API。

您可以从[http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData)下载数据。 复制，然后将**Books.Zip**文件解压缩到您的本地计算机上的文件夹。 例如，为**c:\data**。

[收集数据来训练模型](cognitive-services-recommendations-collecting-data.md)文中找不到目录和使用文件的架构的详细的信息。
 
在本练习中，我们将使用一个小文件，以便您不必非常等待很长时间的培训。 如果您想尝试一个更真实的文件，我们还将**MsStoreData.zip**包含示例从 Microsoft 存储在[相同的位置](http://aka.ms/RecoSampleData)中的交易记录。

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>第 3 步-创建一个项目并上载目录和使用数据 ##

后登录[建议用户界面](https://recommendations-portal.azurewebsites.net/)，您可以看到项目页。 如果您以前创建的任何项目，您应在此处看到它们。
项目 （也称为*模型*API 参考中） 是一个用于目录和使用数据。 您可以创建多个*生成*项目内。 我们将引导您完成下一步中的过程。

1. 要创建新项目时，文本框 （类似"MyFirstModel"将工作） 上键入名称并单击**添加项目**。
 
    ![建议用户界面: 项目页。][reco_projects]

1. 获取创建项目，请单击**添加编录文件**部分中的**浏览文件**按钮。 上传你在第 2 步中的目录。 如果您保存它在*c:\data*，您需要导航到该文件夹。

    ![建议用户界面︰ 向项目中添加数据。][reco_firstmodel]

1. 上载目录后，请单击**添加使用文件**部分中的**浏览文件**按钮。 添加的 usage_large.txt 文件。

> **如果我有使用数据的大型文件？**
>
> 可以将小于 200 MB 的唯一使用文件上载。 话虽如此，系统可保存最多 2GB 积累的事务数据，因此如有必要，您可以上载多个文件。
> 您可能不需要这么大的数据来生成好的模型，它不只是大小的重要的数据，但数据的质量。 它是常见的使用率数据，其中交易大部分都只是在少数几项受欢迎，还有"小信号"的其他项目。

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>第 4 步-让我们做一些培训 ！ ##

现在，您已上载目录和使用数据，我们就可以训练引擎，以便它可以学会从我们的数据模式。

1.  单击**新的生成**按钮。

1.  为生成类型选择**的建议**。 注意，我们支持分级构建和 FBT （经常购买在一起） 生成类型。

    ![建议用户界面︰ 生成对话框。][reco_build_dialog.png]


    FBT 生成允许您确定的产品通常购买/使用同一事务内的模式。
    排名生成用于标识感兴趣的功能。 
    我们将不会很深到 FBT 或排名建立在此研讨会上，但如果您想您应该签出[生成类型和模型质量文档页中找到](cognitive-services-recommendations-buildtypes.md)。

1. 单击**生成**按钮。 生成过程大约需要五分钟，如果您正在使用的资料数据。 针对大数据集时间更长。

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>在第 5 步-让我们找出机器学习的了 ！ ##

完成生成后，您会注意到新生成的生成的列表中。 此生成可以查询项和用户的建议。

1. 完成生成后，请单击**得分**。 这允许您播放与模型和看到哪些项建议。

    ![建议用户界面︰ 成绩按钮][reco_score_button]

1. 选择一个项目以查看哪些项作为该项目的建议。 请注意，是否没有足够的交易记录预测对特定项目的建议，系统不会返回该项目的任何建议。  如果由于某种原因必须不返回任何建议的项目，请尝试评分的其他项目。

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>步骤 6-下一步行动 ##
祝贺您 ！ 您的培训模型，然后从该模型得到的建议。  建议用户界面是一个有用的工具，使您可以查看您的项目的状态和生成。 

既然您已经有一个模型，您可能需要了解如何以编程方式执行上述所有步骤。 要不要学习如何以编程方式调用 API，我们邀请您来检查[建议的 API 参考](http://go.microsoft.com/fwlink/?LinkId=759348)，并下载[建议示例应用程序](http://go.microsoft.com/fwlink/?LinkID=759344)。


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
