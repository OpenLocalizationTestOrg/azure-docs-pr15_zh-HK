<properties
    pageTitle="将机器学习 web 服务部署 |Microsoft Azure"
    description="如何将培训实验转换为预测实验，准备进行部署，然后将其部署为 Azure 机器学习 web 服务。"
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
    ms.date="10/04/2016"
    ms.author="garye"/>

# <a name="deploy-an-azure-machine-learning-web-service"></a>将 Azure 机器学习 web 服务部署

Azure 机器学习使您能够生成、 测试和部署预测分析的解决方案。

从高级角度-的-来看，这是三个步骤︰

- **[创建培训实验]**的 Azure 机器学习 Studio 是协作可视化开发环境，用于训练和测试使用您提供的培训数据的预测分析模型。
- **[将其转换为预测实验]**-一次您的模型已定型的现有数据和您就可以使用它来成绩新数据、 准备并优化预测实验。
- **将其部署为 web 服务**-您可以部署[新]的或[传统]的 Azure web 服务作为预测实验。 用户可以将数据发送到您的模型，并接收您的模型的预测。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>创建培训实验

要训练预测分析模型，则使用 Azure 机器学习 Studio 创建培训实验包含培训数据加载、 数据进行必要的准备，应用机器学习算法，并评估结果的各种模块的位置。 可以反复测试实验和尝试不同的机器学习算法进行比较和评估的结果。

创建和管理培训试验的过程是更全面地在其他地方讲述。 有关详细信息，请参阅下列文章︰

- [在 Azure 机器学习 Studio 中创建简单的实验](machine-learning-create-experiment.md)
- [开发一种预测性解决方案使用 Azure 机器学习](machine-learning-walkthrough-develop-predictive-solution.md)
- [将训练数据导入到 Azure 机器学习 Studio](machine-learning-data-science-import-data.md)
- [管理 Azure 机器学习 Studio 中的试验迭代](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>将培训实验转换为预测试验

已经培训您的模型，就可以预测实验成绩新数据转换培训实验。

通过将转换为预测实验，得到培训的模型准备就绪可以部署作为评分的 web 服务。 Web 服务的用户可以发送到您的模型的输入的数据和模型将发送回的预测结果。 在转换为预测实验后，请记住您希望您的模型可由他人使用。

若要转换到预测实验培训实验，实验画布的底部**运行**，单击**设置 Web 服务**，然后选择**预测的 Web 服务**。

![将转换为分数实验](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

有关如何执行这种转换的详细信息，请参阅[转换到预测实验机器学习培训实验](machine-learning-convert-training-experiment-to-scoring-experiment.md)。

以下步骤描述了部署预防性试验作为一种新的 web 服务。 您还可以作为经典 web 服务部署试验。

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>作为新的 web 服务部署预防性试验

既然已准备预防性试验，您可以将其部署为 Azure 的 web 服务。 使用 web 服务，用户可以将数据发送到您的模型，该模型将返回其预测。

若要部署预防性实验，单击实验画布的底部**运行**。 实验完成后运行，单击**部署 Web 服务**，选择**[新建]部署 Web 服务**。  机器学习 Web 服务门户的部署页将打开。

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>机器学习 Web 服务门户部署实验页

在部署实验页上，请输入该 web 服务的名称。
选择定价计划。 如果您有现有的定价计划，您可以选择它，否则您必须创建新的价格计划服务。

1.  **价格计划**中下拉列表中，选择现有的计划或选择**选择新计划**选项。
2.  在**计划名称**中，键入将识别您的帐单上的计划的名称。
3.  **每月计划层**中选择一个。 该计划层默认定位到默认区域和您的 web 服务的计划被部署到该地区。

单击**部署**和 web 服务随即打开**快速入门**页。

Web 服务的快速启动页为您提供访问和指导创建 web 服务后，您将执行的常见任务。 从这里，可以轻松访问的测试页和页上使用。

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>测试您的 web 服务

若要测试您的新 web 服务，请单击在常见任务下的**测试 web 服务**。 在测试页上，您可以测试您的 web 服务为请求-响应服务 (RR) 或批处理执行服务 (BES)。

RR 测试页显示输入、 输出和为实验所定义任何全局参数。 若要测试 web 服务，可以手动输入相应的值或提供一个以逗号分隔值 (CSV) 格式的文件包含的测试值。

要测试使用 RR，从列表视图模式，请输入适当的值，然后单击**测试请求-响应**。 预测结果显示在左侧的输出列中。

![将 web 服务部署](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

若要测试您的 BE，请单击**批处理**。 在批次测试页上，单击浏览在您的输入并选择 CSV 文件包含适当的采样值。 如果您没有一个 CSV 文件，并创建使用机器学习 Studio 预测实验，可以下载用于预测实验的数据集，并使用它。

若要下载的数据集，打开机器学习 Studio。 打开预测实验并右击实验的输入。 从上下文菜单中，选择**数据集**，然后选择**下载**。

![将 web 服务部署](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

单击**测试**。 执行批处理作业的状态显示**测试批处理作业**的权利。

![将 web 服务部署](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

在**配置**页中，可以更改说明、 标题、 更新存储帐户密码，并启用 web 服务的示例数据。

![配置 web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

一旦您已经部署了 web 服务，您可以︰

- **访问**它通过 web 服务 API。
- **管理**通过 Azure 机器学习 web 服务门户或 Azure 的传统门户网站。
- **更新**它如果模型发生变化。

### <a name="access-the-web-service"></a>访问 web 服务

一旦您部署 web 服务从机器学习 Studio，可以将数据发送到服务并以编程方式接收响应。

**使用**页面提供了您需要访问您的 web 服务的所有信息。 例如，提供 API 密钥以允许授权的访问该服务。

有关访问的机器学习的 web 服务的详细信息，请参阅[如何使用 Azure 机器学习已部署的 web 服务](machine-learning-consume-web-services.md)。

### <a name="manage-your-new-web-service"></a>管理您的新 web 服务

您可以管理您的经典 web 服务机学习 Web 服务门户。 从[门户的主页](https://services.azureml-test.net/)上，单击**Web 服务**。 从 web 服务页中，您可以删除或复制服务。 若要监视特定的服务，请单击该服务，然后单击**仪表板**。 要监视与 web 服务相关联的批处理作业，请单击**批处理请求日志**。

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>作为经典 web 服务部署预防性试验

既然已充分准备好了预测试验，您可以将其部署为 Azure 的 web 服务。 使用 web 服务，用户可以将数据发送到您的模型，该模型将返回其预测。

若要部署预防性实验，在实验画布的底部**运行**，然后单击**部署 Web 服务**。 安装 web 服务，则放在 web 服务面板中。

![将 web 服务部署](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

机器学习 Web 服务门户或机器学习 Studio 中，您可以测试 web 服务。

若要测试请求的响应的 web 服务，请单击 web 服务面板中的**测试**按钮。 弹出一个对话框要求您输入数据的服务。 这些都是评分的实验所需的列。 输入的一组数据，然后单击**确定**。 通过 web 服务生成的结果将显示在仪表板的底部。

您可以单击**测试**预览链接来测试您的服务在 Azure 机器学习 Web 服务门户以前显示在新建 web 服务部分。

若要测试该批处理执行服务，请单击**测试**预览链接。 在批次测试页上，单击浏览在您的输入并选择 CSV 文件包含适当的采样值。 如果您没有一个 CSV 文件，并创建使用机器学习 Studio 预测实验，可以下载用于预测实验的数据集，并使用它。

![测试 web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

在**配置**页面中，可以更改服务的显示名称并为其提供说明。 名称和说明将显示在[Azure 的传统门户网站](http://manage.windowsazure.com/)中管理您的 web 服务。

通过输入字符串的每一列**输入架构**、**输出架构**和**Web 服务参数**下，可以输入的数据、 输出数据和 web 服务参数提供说明。 在提供 web 服务的示例代码文档中使用这些说明。

您可以启用日志记录，您会看到访问您的 web 服务时出现的任何故障诊断。 有关详细信息，请参阅[为机器学习 web 服务启用日志记录](machine-learning-web-services-logging.md)。

![配置 web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

在 Azure 机器学习 Web 服务门户中类似于以前显示在新建 web 服务部分中的过程，您还可以配置为 web 服务的终结点。 选项各不相同，您可以添加或更改服务说明、 启用日志记录，以及启用示例数据以供测试。

### <a name="access-the-web-service"></a>访问 web 服务

一旦您部署 web 服务从机器学习 Studio，可以将数据发送到服务并以编程方式接收响应。

仪表板提供了您需要访问您的 web 服务的所有信息。 例如，提供 API 密钥以允许授权的访问该服务，并提供了 API 帮助页以帮助您开始编写代码。

有关访问的机器学习的 web 服务的详细信息，请参阅[如何使用 Azure 机器学习已部署的 web 服务](machine-learning-consume-web-services.md)。

### <a name="manage-the-web-service"></a>管理 web 服务

有可以执行的操作的各种监视 web 服务。 您可以更新它，并将其删除。 此外可以到经典 web 服务在部署时创建的默认终结点以及其他终结点。

有关详细信息，请参阅[管理 Azure 机器学习区](machine-learning-manage-workspace.md)和[管理 web 服务使用 Azure 机器学习 Web 服务门户](machine-learning-manage-new-webservice.md)。

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>更新 web 服务

您可以更改您的 web 服务，例如使用更多的训练数据，更新模型和部署再次，覆盖原来的 web 服务。

要更新 web 服务，请打开原始预测实验用来部署 web 服务，请通过单击**SAVE AS**的可编辑副本。 进行更改，然后单击**部署 Web 服务**。

因为您已经部署了此试验之前，您将系统询问您是否要覆盖 （经典 Web 服务） 或更新现有服务的 （新 web 服务）。 单击**是**或**更新**现有的 web 服务将停止，并将部署新预测实验被部署在原地。

> [AZURE.NOTE] 如果您在原始 web 服务进行配置更改，例如，输入新的显示名称或说明，需要重新输入这些值。

更新您的 web 服务的一个方法是以编程方式对模型进行重新培训。 有关详细信息，请参阅[重新培训机器学习模型以编程方式](machine-learning-retrain-models-programmatically.md)。


<!-- internal links -->
[创建培训实验]: #create-a-training-experiment
[将其转换为预测试验]: #convert-the-training-experiment-to-a-predictive-experiment
[新增功能]: #deploy-the-predictive-experiment-as-a-new-Web-service
[经典]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
