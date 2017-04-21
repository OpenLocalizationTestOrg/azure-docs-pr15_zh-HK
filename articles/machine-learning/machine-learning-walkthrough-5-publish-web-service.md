<properties
    pageTitle="步骤 5︰ 将机器学习 Web 服务部署 |Microsoft Azure"
    description="制定预防性解决方案演练中的步骤 5︰ 部署 Web 服务作为机器学习 Studio 中的预测实验。"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>演练步骤 5︰ 将 Azure 机器学习 Web 服务部署

这是演练中，[发展预测分析解决方案 Azure 机器学习中](machine-learning-walkthrough-develop-predictive-solution.md)的第五步


1.  [创建一个机器学习工作区](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [将现有数据上载](machine-learning-walkthrough-2-upload-data.md)
3.  [创建新的实验](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **将 Web 服务部署**
6.  [访问 Web 服务](machine-learning-walkthrough-6-access-web-service.md)

----------

若要使其他人有机会使用在本演练中，我们已建立了预测模型，我们将其部署为 Web 服务在 Azure 上。

到目前为止我们已经尝试过与培训我们的模型。 但已部署的服务不再要做培训-它通过评分在我们的模型基于用户输入生成的预测。 因此，我们要做一些准备工作，为***培训***实验从转换此试验***预测***试验。 

这是一个两步过程︰  

1. 转换*培训试验*，我们制作了*预测试验*
2. 作为 Web 服务部署预防性试验

但首先，我们需要稍微削减此试验。 我们在实验中，当前有两种不同型号，但是我们只需要一个模型时我们将此部署为 Web 服务。  

让我们假设我们决定提升的树模型时要使用的更好模型。 因此，第一个办法就是移除[两类支持向量机][two-class-support-vector-machine]模块，用于培训它的模块。 可能想要一份试验首先单击**另存为**实验画布的底部。

我们需要删除以下模块︰  

- [两类支持向量机][two-class-support-vector-machine]
- [培训模型][train-model]和[分数模型][score-model]连接到它的模块
- [规范化数据][ normalize-data] （两个）
- [评估模式][evaluate-model]

选择模块并按 Delete 键，或者右键单击该模块并选择**删除**。

现在，我们已经准备好部署此模型使用[两级放大的决策树][two-class-boosted-decision-tree]。

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>将培训实验转换为预测试验

将转换为预测实验涉及三个步骤︰

1. 保存的模型我们已经接受培训，然后替换我们的培训模块
2. 修剪试验去只所必需的培训模块
3. 定义 Web 服务将接受输入的位置和它所生成的输出

幸运的是，可以通过单击在实验画布 （选择**预测 Web 服务**选项） 的底部**设置了 Web 服务**完成所有三个步骤。

单击**设置启动 Web 服务**时，将发生下列情况︰

- 培训的模型将保存作为一个**培训模型**模块在模块组件面板左侧的实验画布 （可以在**培训模型**下找到它）。
- 卸下模块时使用的培训。 具体来说︰
  - [两类提升的诊断树][two-class-boosted-decision-tree]
  - [火车模型][train-model]
  - [将数据拆分][split]
  - 第二个[执行 R 脚本][execute-r-script]模块用于测试数据
- 已保存培训的模型被加回到实验。
- 添加**web 服务的输入**和**Web 服务输出**模块。

> [AZURE.NOTE] 实验已保存在已添加实验画布顶部的选项卡下的两个部分︰ 原始培训实验**培训试验**，在选项卡下并且新创建的预测实验受**试验的预测**。

我们需要一个额外的步骤与此特定的试验。
我们添加了两个[执行 R 脚本][execute-r-script]模块，以便提供对数据的权重函数来培训和测试。 我们不需要这样做最终的模型中。

机器学习 Studio 中移除一个[执行 R 脚本][execute-r-script]模块时将其删除[拆分][split]模块。 现在，我们现在可以卸下另一个，和连接[元数据编辑器][metadata-editor]直接到[分数模型][score-model]。    

我们的实验现在应该如下所示︰  

![评分培训的模型][4]  

> [AZURE.NOTE] 您可能会疑惑︰ 为什么我们预测实验中留下提示您德语信用卡数据数据集。 该服务将使用该用户的数据，没有原始数据集，因此在模型中为什么保留原始数据集？
>
>它是该服务不需要原始的信用卡数据，则返回 true。 但对于该数据，其中包括信息例如多少列有和哪些列是数字需架构。 此架构信息有必要解释用户的数据。 我们将这些组件连接，使评分模块具有数据集架构时该服务正在运行。 数据不使用，只是架构。  

运行最后一次试验 （单击**运行**）。如果您想要验证模型是否仍正常工作，请单击[分数模型]的输出[score-model]模块并选择**查看结果**。 您会看到显示的原始数据，以及信用风险值 （"评分标签"） 和计分的概率值 （"中的得分概率"。） 

## <a name="deploy-the-web-service"></a>将 Web 服务部署

您可以部署为经典的 Web 服务或基于 Azure 资源管理器上的新 Web 服务试验。

### <a name="deploy-as-a-classic-web-service"></a>作为经典 Web 服务部署 ###

要将源自我们实验经典 Web 服务部署，画布下面单击**部署 Web 服务**，然后选择**部署 Web 服务 [经典]**。 机器学习 Studio 部署了 Web 服务作为实验和将转到该 Web 服务的仪表板。 从这里，可以返回到实验 （**查看快照**或**查看最新版本**） 并运行 Web 服务 （请参见**测试 Web 服务**下） 的一个简单的测试。 此外，还有信息在此处创建的应用程序可以访问 Web 服务 （详见本演练的下一步）。

![Web 服务仪表板][6]

您可以通过单击**配置**选项卡来配置服务。 这里您可以修改服务名称 （它已默认情况下授予实验名称） 并为其提供说明。 对于输入和输出的列，您也可以赋予更多友好的标签。  

![配置 Web 服务][5]  

### <a name="deploy-as-a-new-web-service"></a>作为一种新的 Web 服务部署

要部署源自我们试验的新 Web 服务，请单击下面的画布和**部署 Web 服务 [新]****部署 Web 服务**。 机器学习 Studio 将转移到 Azure 机器学习 Web 服务部署实验页。

输入 Web 服务的名称，然后选择定价计划。 如果您有现有的定价计划，您可以选择它，否则您必须创建新的价格计划服务。 

1.  在**计划价格**下拉列表中选择现有的计划或选择**选择新的计划**选项。
2.  在**计划名称**中，键入一个名称，用于标识您的帐单上的计划。
3.  **每月计划层**中选择一个。 该计划层默认定位到默认区域和您的 Web 服务的计划被部署到该地区。

单击**部署**和 Web 服务随即打开**快速入门**页。

您可以通过单击**配置**菜单选项来配置服务。 这里您可以修改服务名称 （它已默认情况下授予实验名称） 并为其提供说明。 

若要测试 Web 服务选择，请单击**测试**菜单选项 （请参见**测试 Web 服务**的下面）。 创建可以访问 Web 服务的应用程序的信息，请单击**使用**菜单选项 （详细介绍本演练的下一步）。

> [AZURE.TIP] 您已经部署了它之后，可以更新 Web 服务。 例如，如果您想更改您的模型，编辑培训实验、 调整模型参数，然后单击**部署 Web 服务**。 然后选择**部署 Web 服务 [经典]**或**[新] 部署 Web 服务**。 重新部署实验，它将取代现在使用更新的模型的 Web 服务。  

## <a name="test-the-web-service"></a>测试 Web 服务

### <a name="test-a-classic-web-service"></a>传统的 Web 服务

机器学习 Studio 中或在 Azure 机器学习 Web 服务门户，您可以测试该服务。 在 Azure 机器学习 Web 服务门户中测试具有允许您启用的优点 

**在机器学习 Studio 中测试**

在**仪表板**页在**默认终结点**中单击**测试**按钮。 一个对话框将弹出，要求您输入数据的服务。 它们出现在原始的德国信用风险数据集相同的列。  

输入的一组数据，然后单击**确定**。 

**在 Azure 机器学习 Web 服务门户中测试**

在**仪表板**页面上，单击**默认终结点**下的**测试**预览链接。 测试页的 Azure 机器学习 Web 服务门户的 Web 服务终结点中打开，并要求您输入数据的服务。 它们出现在原始的德国信用风险数据集相同的列。

单击**测试请求的响应**。 

在 Web 服务中的数据通过**Web 服务输入**模块，通过[元数据编辑器]输入[metadata-editor]模块，并对[分数模型][score-model]模块，它就会被评定。 从 Web 服务通过**Web 服务的输出**结果被输出。

**测试新的 Web 服务**

在 Azure 机器学习 Web 服务门户中，请单击页面顶部的**测试**。 **测试**页将打开，您可以输入有关该服务的数据。 输入的字段显示对应于出现在原始的德国信用风险数据集中的列。 

输入的数据集，然后单击**测试请求的响应**。

测试结果将显示在输出列中的页的右侧。 

当测试在 Azure 机器学习 Web 服务门户时，您可以启用可用于测试请求响应服务的示例数据。 如果您在计算机学习 Studio 中创建 Web 服务，示例数据取自数据您用来训练您的模型。

> [AZURE.TIP] 方式，我们可以预测实验配置，整个时得到的[分数模型][score-model]模块将返回。 这包括所有输入的数据加上信用风险值和计分的概率。 如果您想要返回一些与众不同的如只贷风险值-无法插入的[项目的列]，然后[project-columns]之间[分数模型]的模块[score-model]和**Web 服务的输出**来消除不希望 Web 服务返回的列。 

## <a name="manage-the-web-service"></a>管理 Web 服务

**管理典型的 Web 服务**

一旦您已经部署了经典的 Web 服务，您可以从[Azure 的传统门户网站](https://manage.windowsazure.com)来进行管理。

1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com)。
2. 在 Microsoft Azure 服务面板中，单击**机器学习**。
3. 单击您的工作区。
4. 单击**Web 服务**选项卡。
5. 单击创建我们的 Web 服务。
6. 单击"默认值"终结点。

在这里，您可以执行某些操作，如监视 Web 服务正在执行的操作并进行性能调整，通过改变多少并发调用该服务可以处理。
甚至可以在 Azure 市场发布您的 Web 服务。

有关详细信息，请参阅︰

- [创建终结点](machine-learning-create-endpoint.md)
- [扩展 Web 服务](machine-learning-scaling-webservice.md)
- [将 Azure 机器学习 Web 服务发布到 Azure 市场](machine-learning-publish-web-service-to-azure-marketplace.md)

**管理 Azure 机器学习 Web 服务门户网站中的 Web 服务**

一旦您已经部署了 Web 服务，经典或新建，可以从[Azure 机器学习 Web 服务门户](https://servics.azureml.net)管理它。

监视 Web 服务的性能︰

1. 登录到[Azure 机器学习 Web 服务门户](https://servics.azureml.net)。
2. 单击**Web 服务**。
3. 单击您的 Web 服务。
4. 单击**仪表板**。

----------

**下一步︰[访问 Web 服务](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/