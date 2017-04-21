<properties
    pageTitle="在机器学习 Studio 在简单实验 |Microsoft Azure"
    description="本机器学习教程将指导您完成一个简单数据科学实验。 我们将预测回归算法使用的汽车的价格。"
    keywords="试验中，线性回归分析、 机器学习算法、 计算机学习教程、 预测建模技术，数据科学实验"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>机器学习教程︰ 在 Azure 机器学习 Studio 中创建第一个数据科学实验

本机器学习教程将指导您完成一个简单数据科学实验。 我们将创建线性回归模型，根据不同的变量如一辆汽车的价格进行预测和技术规范。 为此，我们将使用 Azure 机器学习 Studio 开发和循环上一个简单的预测性分析试验。

*预测分析*是一种使用当前数据预测未来结果的数据科学。 预测分析的一个非常简单的示例，注意数据科学初学者视频 4︰[预测与简单模型的答案](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md)(运行时︰ 7:42)。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>机器学习 Studio 如何帮助？

机器学习 Studio 容易设置使用拖放模块预测建模技术已预编程的实验。 要运行实验，并预测了答案，您将使用机器学习 Studio*创建模型*、*火车模型*和*分数和测试模型*。

输入机器学习 Studio: [https://studio.azureml.net](https://studio.azureml.net)。 如果您已经登录到计算机学习 Studio 之前，请单击**此处登录**。 否则为单击**注册**，免费和付费的选项之间进行选择。

关于机器学习 Studio 的更多常规信息，请参阅[机器学习 Studio 是什么？](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>五个步骤来创建一个实验

学习教程这台计算机，您将按照构建在计算机学习 Studio 中才能创建、 训练，以及分数模型实验的五个基本步骤︰

- 创建模型
    - [步骤 1︰ 获取数据]
    - [步骤 2︰ 预处理数据]
    - [步骤 3︰ 将功能定义]
- 火车模型
    - [第 4 步︰ 选择并应用一种学习算法]
- 成绩和测试模型
    - [第 5 步︰ 预测新汽车价格]

[步骤 1︰ 获取数据]: #step-1-get-data
[步骤 2︰ 预处理数据]: #step-2-preprocess-data
[步骤 3︰ 将功能定义]: #step-3-define-features
[第 4 步︰ 选择并应用一种学习算法]: #step-4-choose-and-apply-a-learning-algorithm
[第 5 步︰ 预测新汽车价格]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>步骤 1︰ 获取数据

有大量供您选择，机器学习 Studio 附带的示例数据集，可以从许多来源导入数据。 对于本示例，我们将使用包含的示例数据集，**汽车价格数据 (Raw)**。
此数据集包括对大量的个别汽车，包括制作、 型号、 技术规格和价格等信息的条目。

1. 通过单击**+ 新**的机器学习 Studio 窗口底部开始新的实验，选择**试验**，，然后选择**空白试验**。 选择默认实验画布顶部的名称并将其重命名为有意义的名称，例如，**汽车价格预测**。

2. 左侧画布是实验的一个数据集和模块的调色板。 此调色板来查找标记为**汽车价格数据 ((Raw))**的数据集的顶部的搜索框中键入**的汽车**。

    ![调色板搜索][screen1a]

3. 将数据集拖动到画布上实验。

    ![数据集][screen1]

若要查看此数据，请单击底部的汽车数据集的输出端口，然后选择**可视化**。

![模块输出端口][screen1c]

在数据集中的变量显示为列，并显示为行的一辆汽车的每个实例。 最右边列 （26 列和标题为"价格"） 是我们要试着预测的目标变量。

![数据集可视化效果][screen1b]

单击"**x**"在右上角关闭可视化效果窗口。

## <a name="step-2-preprocess-data"></a>步骤 2︰ 预处理数据

数据集通常需要一些预处理之前可以对它进行分析。 您可能已经注意到存在不同的行的列中缺少的值。 这些缺少的值需要进行清理，使模型可以正确地分析数据。 在我们的例子中，我们将删除任何缺少值的行。 另外，**规范化损失**列有丢失的值有很大一部分以便我们将排除该列从模型完全。

> [AZURE.TIP] 清除输入数据中缺少的值是使用大部分模块的先决条件。

首先我们将删除**规范化损失**吧，然后我们将删除任何已丢失的数据的行。

1. 要[选择的列在数据集中]查找模块调色板顶部的搜索框中键入**选择列**[select-columns]模块，然后拖到实验画布并将其连接到**汽车的价格数据 (Raw)**数据集的输出端口。 本模块，我们可以选择我们想要包括或排除模型中的数据列。

2. 选择的[数据集中选择列][select-columns]模块并单击**属性**窗格中的**启动列选择器**。

    - 在左侧，单击**使用规则**
    - 在**开始使用**，请单击**所有列**。 这指示[在数据集选择列][select-columns]通过 （除了那些我们要排除） 的所有列。
    - 从下拉列表选择**排除**和**列的名称**，然后单击文本框内部。 显示列的列表。 选择**规范化损失**，并且它将添加到文本框中。
    - 单击复选标记 （确定） 按钮以关闭列选定器。

    ![选择列][screen3]

    **选择数据集中的列**的属性窗格中表示，它将通过所有列从除**规范化损失**数据集。

    ![选择数据集属性中的列][screen4]

    > [AZURE.TIP] 通过双击模块和输入文本，可以将注释添加到模块。 这可以帮助您快速查看模块进行实验中。 在这种情况下，双击[选择在数据集内的列][select-columns]模块和类型注释"排除规范化损失。"

3. 拖[干净缺失数据][clean-missing-data]实验模块画布并将其连接到[数据集中选择列][select-columns]模块。 在**属性**窗格中，选择要通过删除缺少值的行清理数据的**清洗模式**下**删除整行**。 双击该模块并键入注释"删除缺少的值行"。

    ![干净的缺失数据属性][screen4a]

4. 通过实验画布下，单击**运行**运行实验。

实验完成后，所有的模块有一个绿色的复选标记，以指示它们已成功完成。 此外注意到右上角中的**已完成的运行**状态。

![第一个实验运行][screen5]

我们到目前为止，实验中所做的只是清理数据。 如果您想要查看的已清除的数据集，请单击左的输出端口的[干净的缺失数据]的[clean-missing-data]模块 （"已清理数据集"），并选择**可视化**。 请注意，**规范化损失**该列不再包括在内，并且没有任何缺少的值。

现在，数据是干净的我们就可以指定我们将预测模型中使用哪些功能。

## <a name="step-3-define-features"></a>步骤 3︰ 将功能定义

在机器学习，*功能*是单个可衡量属性的东西感兴趣。 我们集中的每一行表示一个汽车，和每一列是该汽车的功能。

找到一个好功能创建预测模型的一套要求试验和要解决的问题有关的知识。 某些功能是更好的预测比其他目标。 此外，某些功能具有强关联与其他功能 (例如，城市的 mpg 与高速公路 mpg)，因此它们将不向模型添加新的信息并将它们删除。

让我们构建一个模型，在我们的数据集使用的功能的一个子集。 可以回来和选择不同的功能，实验再次运行，并查看是否获得了较好的效果。 但是，若要开始，让我们尝试以下功能︰

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. 将另一个[数据集中选择列][select-columns]实验模块画布并将它连接到的[干净的缺失数据]的左的输出端口[clean-missing-data]模块。 双击该模块，然后键入"选择功能的预测。

2. 在**属性**窗格中，单击**启动列选定器**。

3. 单击**规则**。

4. 下**开始与**单击**列**，然后选择筛选器行中的**包括**和**列名称**。 输入列名称的列表。 这指示该模块通过我们指定的列。

    > [AZURE.TIP] 通过运行试验，我们已确保，我们的数据的列定义通过从数据集中[清理缺失数据][clean-missing-data]模块。 这意味着您连接其他模块还可以从数据集的信息。

5. 单击复选标记 （确定） 按钮。

![选择列][screen6]

这将产生的学习算法，在接下来的步骤中将使用的数据集。 稍后，您可以返回并重试与选择不同的功能。

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>第 4 步︰ 选择并应用一种学习算法

现在，数据准备就绪后，构建预测模型组成的培训和测试。 我们将使用我们的数据来训练模型，然后进行测试以查看如何关闭它是无法预测价格的模型。 现在，不要担心我们为什么需要进行训练，然后进行测试模型。

*分类*和*回归分析*是两种类型的监察机器学习技术。 分类预测来自一组定义的类别，例如一种颜色 （红色、 蓝色或绿色） 的应答。 回归分析用于预测数字。

因为我们想要预测价格，是一个数字，我们将使用一个回归模型。 对于本示例，我们将训练简单*线性回归*模型，并在下一步，我们将测试它。

1. 对培训和将其拆分为单独的培训和测试集的测试，我们使用我们的数据。 选择并拖动[拆分数据][split]实验模块画布并将其连接到最后一个[数据集中选择列]的输出[select-columns]模块。 将**分数中第一个输出数据集的行**设置为 0.75。 这种方式，我们将使用 75%的数据训练模型，并回保留 25%的测试。

    > [AZURE.TIP] 通过更改**随机种子**参数，您可以生成不同的随机样本，来培训和测试。 此参数控制伪随机数生成器的种子。

2. 运行实验。 这样，[选择在数据集内的列][select-columns]和[分割数据][split]模块，可以将列定义传递给我们将下一步添加的模块。  

3. 若要选择学习算法，展开的画布上，左侧的模块组件面板中的**机器学习**类别，再展开**初始化模型**。 这显示模块可用于初始化机器学习算法的几种的类别。

    此试验，选择[线性回归][linear-regression]模块下**回归**类别 （您还可以找到该模块通过调色板搜索框中键入"线性回归"），并将其拖到实验画布。

4. 查找并拖动[火车模型][train-model]实验画布上的模块。 左输入的端口连接到输出的[线性回归][linear-regression]模块。 右输入的端口相连的[拆分数据]培训数据输出 （左端口）[split]模块。

5. 选择[火车模型][train-model]模块，在**属性**窗格中单击**启动列选定器**，然后选择**价格**列。 这是将我们的模型预测值。

    ![选择"价格"列][screen7]

6. 运行实验。

结果是训练有素的回归模型，可用于成绩新样本进行预测。

![应用机器学习算法][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>第 5 步︰ 预测新汽车价格

现在，我们已培训使用 75%的我们的数据模型，我们可以使用它得分其它 25%的数据，以如何更好地了解我们的模型函数。

1. 找到并将[评分模型][score-model]实验模块画布和[火车模型]的输出连接左输入的端口[train-model]模块。 右输入的端口连接到测试数据输出 （右端口）[拆分数据]的[split]模块。  

    ![分数模型模块][screen8a]

2. 运行试验并查看[分数模型]的输出[score-model]模块中，单击输出端口，然后选择**可视化**。 输出显示价格的预测的值，并测试数据中的已知的值。  

3. 最后，若要测试结果的质量，选择并拖动该[评估模型][evaluate-model]实验模块画布，并且[分数模型]的输出连接左输入的端口[score-model]模块。 (有两个输入的端口由于[计算模型][evaluate-model]模块可用于比较两个模型。)

4. 运行实验。

若要查看该[评估模型]的输出[evaluate-model]模块中，单击输出端口，然后选择**可视化**。 我们的模型显示了以下统计信息︰

- **意味着绝对的错误**(MAE): 绝对的错误 （*错误*为预测的值和实际值之间的差异） 的平均值。
- **根的平均平方的错误**(RMSE): 测试数据集上所作的预测的平方误差平均值的平方根。
- **相对绝对错误**︰ 相对于实际值和平均值的所有实际值之间的绝对差异的绝对错误的平均值。
- **相对平方错误**︰ 相对于实际值和平均值的所有实际值的平方差的平方错误的平均值。
- **系数的确定**︰ 也称为**R 平方值**，它指示模型如何更好地适合数据的统计指标。

对于每个错误的统计数据，小越好。 较小的值表示的预测更加密切地与实际值相匹配。 **系数的确定**，其值越接近一 (1.0)，更好地预测。

![评估结果][screen9]

最终实验应如下所示︰

![机器学习教程︰ 完成使用预测建模技术的线性回归实验。][screen10]

## <a name="next-steps"></a>下一步行动

现在，您已经完成的第一个机器学习教程，具有实验设置，可以循环来尝试改进模型。 例如，您可以更改在您预测中使用的功能。 也可以修改的属性的[线性回归][linear-regression]算法或完全尝试不同的算法。 甚至可以添加多个机器一次学习算法进行实验和比较两个使用[评估模型][evaluate-model]模块。

> [AZURE.TIP] 使用实验画布**SAVE AS**按钮复制实验的任何迭代。 在画布上单击**查看运行历史记录**，您可以看到所有小版本的实验。 请参阅[管理试验 Azure 机器学习 Studio 中的迭代][runhistory]了解详情。

[runhistory]: machine-learning-manage-experiment-iterations.md

当您对您的模型满意后时，才可以部署它作为 web 服务以用于使用新数据预测汽车的价格。 请参见[部署 Azure 机器学习 web 服务][publish]了解详情。

[publish]: machine-learning-publish-a-machine-learning-web-service.md

创建预测模型技术的更广泛而且很详细演练，培训、 积分和部署模型，请参见[开发预防性解决方案使用 Azure 机器学习][walkthrough]。

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
