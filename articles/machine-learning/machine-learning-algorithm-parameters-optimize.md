<properties
    pageTitle="选择要优化 Azure 机器学习算法的参数 |Microsoft Azure"
    description="解释如何选择最佳参数设置为在 Azure 机器学习算法。"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>选择参数以优化 Azure 机器学习算法

本主题描述如何选择适当的 hyperparameter 设置为在 Azure 机器学习算法。 大部分的机器学习算法具有参数设置。 在训练模型时，您需要为这些参数提供值。 培训的模型的功效取决于您选择的模型参数。 查找最佳的参数设置的过程被称为*模型选择*。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

有多种方法来进行模型选择。 在机器学习中交叉验证是使用最广泛的模型选择方法之一，它是默认的模型选择机制在 Azure 机器学习。 由于 Azure 机器学习支持 R 和 Python，总是可以通过 R 或 Python 实现他们自己的模型选择机制。

有过程中查找最佳的参数集的四个步骤︰

1.  **定义参数空间**︰ 算法中，首先确定您需要考虑的精确参数值。
2.  **定义交叉验证设置**︰ 决定如何选择数据集的交叉验证折痕。
3.  **定义度量**︰ 决定哪些指标用于确定的最佳参数，如精度套、 根平均平方错误、 精度、 撤回或 f 分数。
4.  **火车，评估并比较**︰ 对于每个参数值的唯一组合，由执行和基于错误度量定义交叉验证。 后评估和比较，您可以选择最佳模型。

下图举例说明如何做到这一点在 Azure 机器学习中所示。

![找到一组最优参数](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>定义参数空间
您可以定义的参数集在模型的初始化步骤。 所有的机器学习算法的参数窗格具有两种培训师模式︰*单个参数*和*参数范围*。 选择参数范围模式。 在参数范围模式中，可以输入多个值，为每个参数。 您可以在文本框中输入以逗号分隔的值。

![两类提升的决策树，单个参数](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 或者，您可以定义网格和总点与**使用范围生成器**生成的最大和最小点。 默认情况下，参数值会生成线性尺度。 但如果选中**对数刻度**，则在对数刻度所生成的值 （即，相邻点的比是常数而不是它们的差）。 对于整数参数，可以通过使用连字符来定义范围。 例如，所有的整数，介于 1 和 10 （均包括） 窗体的参数集"1 10"手段。 此外支持混合的模式。 例如，参数设置"1-10，20，50"包括整数 1 至 10，20 和 50。

![两类提升的决策树，参数范围](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>定义交叉验证折页
[分区和示例][partition-and-sample]模块可以用于随机的数据分配折痕。 在以下的示例配置模块，我们可以定义五个折叠并折大量随机分配示例实例。

![分区和示例](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>定义度量
[调整模型 Hyperparameters] [tune-model-hyperparameters]模块根据经验选择给定的算法和数据集参数的最佳组合提供了支持。 以及其他的信息相关的培训模式，此模块的**属性**窗格中包括规格，确定最佳的参数集。 它有两个不同的下拉列表框的分类和回归算法，分别。 如果正在考虑算法是一种分类算法，回归跃点数将被忽略，反之亦然。 在这个示例中，衡量标准是**准确性**。   

![扫描参数](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>培训、 评估和比较  
相同的[优化模型 Hyperparameters] [tune-model-hyperparameters]模块对应于参数集的所有模型，计算各种规格，并培训然后创建基于您选择的度量的最佳培训的模型。 此模块具有强制性的两个输入︰

* 未经培训的学员
* 数据集

该模块还具有输入一个可选的数据集。 数据集与折到强制数据集输入的信息。 如果数据集未分配任何折页的信息，然后 10-fold 交叉验证默认情况下自动执行。 如果未完成折分配并验证数据集提供数据集的可选端口，然后定型测试模式选择，并第一个数据集用来训练的每个参数组合的模型。

![提升的决策树分类器](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

在验证数据集然后计算模型。 模块的左的输出端口作为函数的参数值显示不同的指标。 右侧的输出端口给选指标 （**准确性**在此种情况下） 根据最佳的模型相对应的培训的模型。  

![验证数据集](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

您可以看到确切的参数选择进行可视化处理的右输出端口。 这种模型后，可使用或实施了 web 服务中评分测试集另存为培训模型。

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
