<properties
    pageTitle="如何选择机器学习算法 |Microsoft Azure"
    description="如何在群集、 分类或回归试验中选择用于监察和监督学习 Azure 机器学习算法。"
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>如何选择 Microsoft Azure 机器学习算法

回答问题"什么机器学习算法应使用吗？" 始终是"其依赖"。 这取决于大小、 质量和数据的性质。 它取决于您想要做的答案。 这取决于如何为您正在使用的计算机的指令翻译算法的数学。 它取决于您有多少时间。 即使最有经验的数据科学家无法知道哪种算法将在它们之前的性能最佳。

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>机器学习算法内容便笺

**Microsoft Azure 机器学习算法内容便笺**将帮助您选择适当的机器学习算法从 Azure 机器学习算法的库您预测分析的解决方案。
本文讲解如何使用它。

> [AZURE.NOTE] 要下载内容便笺，并按照这篇文章，请转到[机器学习算法对 Microsoft Azure 机器学习 Studio 的便笺](machine-learning-algorithm-cheat-sheet.md)。

此便笺具有非常特殊的听众记住︰ 开始数据科学家与本科级机器学习，尝试在 Azure 机器学习 Studio 中开始选择算法。 这意味着，它使得一些泛化和 oversimplifications，但它将指引您安全的方向。 这也意味着，有很多未在此处列出的算法。 随着 Azure 机器学习为包含一套更完整的可用的方法，我们将添加它们。

这些建议都是已编译的反馈和从大量的数据科学家和机器学习专家提示。 我们没有达成一致意见的所有内容，但我试着到大致共识协调我们的观点。 大部分的不一致的语句开头"它取决于..."

### <a name="how-to-use-the-cheat-sheet"></a>如何使用便笺

阅读作为图表上的路径和算法标签"的*&lt;路径标签&gt;*使用*&lt;算法&gt;*。" 例如，对于*速度*使用*两个类物流回归*。 有时多分支将应用。
有时他们会再合适不过。 他们被应因此不必担心它的确切的经验法则建议。
我提到了与上述的几种数据科学家找到最好的算法只确保方法是尝试所有这些。

下面是从[Cortana 智能库](http://gallery.cortanaintelligence.com/)实验，尝试对相同数据的多种算法和比较结果的一个示例︰[比较多类分类器︰ 字母识别](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92)。

>[AZURE.TIP] 要下载并打印关系图提供的机器学习 Studio 功能的概述，请参阅[Azure 机器学习 Studio 功能的概述图](machine-learning-studio-overview-diagram.md)。

## <a name="flavors-of-machine-learning"></a>口味的机器学习

### <a name="supervised"></a>管理

监察的学习算法进行预测根据一组示例。 例如，历史股价可用于危险推测未来的价格。 用于培训每个示例都标有感兴趣的值 — — 在这种情况下，股票的价格。 监察的学习算法查找这些值标签中的模式。 它可以使用任何可能相关的信息 — — 一周、 季节、 公司的财务数据、 行业类型、 存在中断的 geopolicitical 事件的一天 — — 和每种算法寻找不同类型的模式。 该算法已找到的最佳模式后它可以它使用该模式来进行标记的测试数据的预测 — — 明天的价格。

这是流行的和有用的机器学习类型。 有一个例外，所有在 Azure 机器学习模块的监督学习算法。 有几种特定类型监察学习表示在 Azure 机器学习︰ 分类、 回归和异常检测。

* **分类**。 如果正在使用数据来预测某一类别，监察的学习也被称为分类。 指定图像作为 '猫' 或 '狗' 的图片时，这种情况。 当只有两种选择，这被称为**两类**或**二项式的分类**。 当有更多类别，为预测三月疯狂的 ncaa 分区联赛获胜时，此问题被称为**多类分类**。

* **回归分析**。 当正在预测值，与股票价格一样时，监察的教育称为回归。

* **异常检测**。 有时目标是确定并不只是常用的数据点。 例如，在欺诈检测任何非常罕见的信用卡支出模式是可疑。 因此很多可能的变化形式也很少有人，不难了解什么欺诈活动的训练示例如下所示。 异常检测所采用的方法就是了解什么正常活动如下所示 （使用历史记录非欺诈性交易记录），并标识任何明显不同。

### <a name="unsupervised"></a>无人监管

监督学习，在数据点具有与其关联的标签。 相反，非监督的学习算法的目标是组织某些方面的数据，或描述其结构。 这可能意味着将其分组到群集或查找不同的方法来看复杂的数据，以使其显示简单或更有组织性。

### <a name="reinforcement-learning"></a>加强学习

加强学习，在算法获取响应每个数据点选择的操作。 学习算法还收到奖励信号短时间以后，指示不好决定。
该算法基于此，为了获得最高奖励修改其战略。 目前没有强化学习在 Azure 机器学习算法模块。 强化学习是的机器人，其中某一点的时间的传感器读数集是一个数据点，并且算法必须选择自动机的下一步操作中常见。 它是自然适应物联网的应用程序。

## <a name="considerations-when-choosing-an-algorithm"></a>选择哪种算法时的注意事项

### <a name="accuracy"></a>准确性

获取最准确的答案可能并非总是必要。
有时近似是足够的具体取决于您想要使用它的。 如果是这种情况，您可能能够减少处理时间大大粘连与更多的近似方法。 更多的近似方法的另一个优点是它们自然倾向避免[overfitting](https://youtu.be/DQWI1kvmwRg)。

### <a name="training-time"></a>培训时间

数分钟或几小时内需要培训模型算法之间有很大不同。 培训时间通常紧密的准确性 — — 一个通常伴随其他。 此外，某些算法在数据点的数量比其他人更敏感。
当时间有限它会推动算法，算法的选择，尤其是在数据集很大时。

### <a name="linearity"></a>线性

很多机器学习算法进行线性的使用。 线性分类算法假定类可以隔开的直线 （或其更高维度模拟）。 这些包括物流的回归分析和支持向量机 （如在 Azure 机器学习中实现）。
线性回归算法假定数据趋势按照一条直线。 这些假设并不坏，对于某些问题，但在其他人他们降低准确度。

![非线性类 bounday][1]

***非线性的类边界****-依赖的线性分类算法会导致较低精度*

![用非线性趋势的数据][2]

***用非线性趋势的数据****-使用线性回归方法将生成可比所需更大错误*

尽管他们危险，线性算法是作为攻击第一行非常受欢迎。 他们往往是通过算法简单和快速训练。

### <a name="number-of-parameters"></a>参数数目

参数是数据科学家获取设置算法时打开旋钮。 它们是影响算法的行为，例如错误容限或迭代次数或选项变体算法的处理方式之间的数字。 培训时间和该算法的准确性有时会对获取正确的设置非常敏感。 通常情况下，使用大量参数的算法需要大多数的试验和错误，要找到好的组合。

或者，在自动尝试在您选择的任意粒度的所有参数组合的 Azure 机器学习是[参数扫描](machine-learning-algorithm-parameters-optimize.md)模块。 虽然确保了跨区卷的参数空间的好方法，培训模型所需的时间将与参数的数目大幅增加。

有利因素是，通常具有多个参数指示算法，具有更大的灵活性。 它通常可以获得很好的准确性。 提供您可以找到参数设置的适当组合。

### <a name="number-of-features"></a>许多功能

对于某些类型的数据，一些功能可能会非常大与数目的数据点进行比较。 这通常是与遗传科或文本数据的情况。 大量的功能可以陷入困境一些学习算法，使 unfeasibly 长时间的培训。 支持向量机是特别适合于这种情况下 （见下文）。

### <a name="special-cases"></a>特殊情况

一些学习算法进行特定假设的结构数据或所需的结果。 如果您能找到最适合您的需要，它可以为您提供更有用的结果，更准确的预报或更快的训练时间。

|**算法**|**准确性**|**培训时间**|**线性**|**参数**|**备注**|
|---|:---:|:---:|:---:|:---:|---|
|**两类分类**| | | | | |
|[物流的回归](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[诊断目录林](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[决策时蛙鸣](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|较低的内存占用量|
|[提升的决策树](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|较大的内存占用量|
|[神经网络](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[其他自定义是可能的](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[平均的 perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[支持向量机](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|适合于大型的功能集|
|[本地深层支持向量机](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|适合于大型的功能集|
|[Bayes' 点机](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**多类分类**| | | | | |
|[物流的回归](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[诊断目录林](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[决策时蛙鸣](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|较低的内存占用量|
|[神经网络](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[其他自定义是可能的](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[一个 v-全部](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|查看所选的两类方法的属性|
|**回归**| | | | | |
|[线性](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[线性贝叶斯](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[诊断目录林](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[提升的决策树](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|较大的内存占用量|
|[快速的林 quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|分配，而不是点的预测|
|[神经网络](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[其他自定义是可能的](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[泊松](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|从技术上讲日志线性。 对于预测计数|
|[序号](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|对于预测秩排序|
|**异常检测**| | | | | |
|[支持向量机](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|尤其适用于大型的功能集|
|[基于 PCA 的异常检测](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-方法](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|聚集算法|


**算法的属性︰**

**●** -显示了出色的准确度、 快速的培训时间和线性的使用

**○** -显示良好的准确性和中等培训时间

## <a name="algorithm-notes"></a>算法的笔记

### <a name="linear-regression"></a>线性回归

前面已提到，[线性回归分析](https://msdn.microsoft.com/library/azure/dn905978.aspx)对数据集适合的行 （或平面或 hyperplane）。 健将，简单又快捷，但它可能会对某些问题过于简单化。
查看此处以[线性回归教程](machine-learning-linear-regression-in-azure.md)。

![与线性趋势的数据][3]

***与线性趋势的数据***

### <a name="logistic-regression"></a>物流的回归

虽然它容易引起混淆的名称中包括回归，物流的回归是实际用于[两类](https://msdn.microsoft.com/library/azure/dn905994.aspx)和[multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx)分类的强大工具。 它是快速而简单。 事实是，它使用的 '-形状的曲线，而不是一条直线就自然不适用于将数据划分为组。 物流回归提供线性的类边界，因此当您使用它，请确保线性近似是可以忍受。

![对两类数据只是一个功能的物流回归][4]

***对两类数据只对一个功能与物流回归****-类边界是尽可能靠近这两个类只是物流的曲线点*

### <a name="trees-forests-and-jungles"></a>树、 林和 jungles

诊断目录林 （[回归](https://msdn.microsoft.com/library/azure/dn905862.aspx)、[两个类](https://msdn.microsoft.com/library/azure/dn906008.aspx)和[multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)）、 决策 jungles （[两类](https://msdn.microsoft.com/library/azure/dn905976.aspx)和[multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)） 和提升的决策树 （[回归](https://msdn.microsoft.com/library/azure/dn905801.aspx)和[两类](https://msdn.microsoft.com/library/azure/dn906025.aspx)） 均基于决策树基础机器学习的概念。 有许多变体决策树，但它们都执行相同的操作 — — 为大部分相同的标签区域细分的功能空间。 这可能是类别的值的区域或常量，具体取决于您执行的值的分类或回归的一致。

![诊断树细分功能空间][5]

***诊断树为大致统一的值的区域细分功能空间***

因为功能空间可以细分成任意小的区域，很容易想象划分不够精细，每个地区有一个数据点 — — 极端的过度臃肿。 为了避免这种情况，大量树的构造与特殊数学小心树不相关联。 这个"决策林"的平均值是避免过度臃肿的树。 诊断目录林可以使用大量的内存。 判定 jungles 是一个 variant 类型，会消耗更少的内存为代价稍长的训练时间。

提升的决策树通过限制他们可以细分的次数及如何几个数据点可以用每个区域中避免过度臃肿。 算法构造树，其中每个学习以弥补由树前留下的错误序列。 其结果是非常精确的学员往往会使用大量内存。 有关完整的技术说明，敬请[Friedman 的原纸](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf)。

[快速的林 quantile 回归](https://msdn.microsoft.com/library/azure/dn913093.aspx)是一种想要知道不只在一个地区，而且还以 quantiles 的形式及其分布的数据的典型 （中间） 值的特殊情况下的决策树。

### <a name="neural-networks-and-perceptrons"></a>神经网络和 perceptrons

神经网络是具创意大脑的学习算法，其中包括[multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx)、[两个类](https://msdn.microsoft.com/library/azure/dn905947.aspx)和[回归](https://msdn.microsoft.com/library/azure/dn905924.aspx)问题。 他们有无限多，但内 Azure 机器学习的神经网络是所有定向非循环图的形式。 这意味着，输入的功能传递向前 （从不后退） 通过一系列的图层之前被转换为输出。 在每个层，输入是各种组合的加权、 求和，而传递到下一层。 这种组合的简单的计算结果能够了解复杂的类边界和数据趋势似乎是变魔术一样。 这种多层网络执行"深度学习"，刺激了这么多的技术报告和科幻小说。

此高性能并不是免费，但。 神经网络可以需要很长的时间来训练，特别是对于大型的数据集有很多的功能。 他们也有更多的参数，比大多数算法，这意味着参数扫描极大地扩展了训练时间。
并为那些想要[指定自己的网络结构](http://go.microsoft.com/fwlink/?LinkId=402867)的 overachievers，可能是 inexhaustible。

<a name="boundaries-learned-by-neural-networks6"></a>![通过神经网络学习的边界][6]
---------------------------

***通过神经网络学习的边界可以是复杂和不规则***

[两类平均 perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)是神经网络答案不断飚升的训练时间。 它使用给出线性类边界的网络结构。 几乎基元按照今天的标准，但它有悠久的历史的可靠地工作，是足够小，能够快速学习。

### <a name="svms"></a>SVMs

支持向量机 (SVMs) 找到尽可能宽的边距为分隔的类边界。 当不能明显分开的两个类时，算法发现他们可以最佳边界。 当写入 Azure 机器学习中，[两类 SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx)这是直条线。 （在 SVM 的角度说，它使用线性内核。）因为它使此线性近似，就能够非常快地运行。 其中它确实非常出色是功能重要数据，如文本或基因组。 在这些情况下 SVMs 都能够单独类的速度更快、 比大多数其他算法，另外还要求了适当数量的内存更少过度臃肿。

![支持向量机类边界][7]

***一个典型的支持向量机类边界最大化两类分开的边距***

微软研究院的另一种产品，[两个类的本地深层 SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx)是保留大部分的线性版本的速度和内存效率的 SVM 的非线性变形。 它非常适合于案例中的线性方法不能提供足够准确的答案。 开发人员将保持其快速通过将问题分解成大量小型线性 SVM 问题。 阅读有关如何关闭此技巧拉入的详细信息的[完整说明](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf)。

使用非线性 SVMs 巧妙地扩展，[一类 SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx)绘制紧密地概述了整个数据集的边界。 它可用于异常检测。 任何新的数据点，远远超出该边界是足够罕见，是值得注意。

### <a name="bayesian-methods"></a>贝叶斯方法

贝叶斯方法具有非常理想的质量︰ 它们将避免过度臃肿。 他们这样做，从而预先分发有关的可能答案的一些假设。 这种方法的另一个副产品是，它们具有很少的参数。 Azure 的机器学习的分类 （[两类 Bayes 点机](https://msdn.microsoft.com/library/azure/dn905930.aspx)） 和回归 （[贝叶斯线性回归](https://msdn.microsoft.com/library/azure/dn906022.aspx)） 这两种贝叶斯算法。
请注意，这些假设的数据可以拆分或适合用一条直线。

上一个历史说明，Bayes' 点机是在微软研究院开发的。 他们有背后一些极其美丽理论工作。 感兴趣的学生被定向到[原始文章中 JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf)和[富有远见的 Chris 大伟博客](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx)。

### <a name="specialized-algorithms"></a>专门的算法

如果您有一个非常具体的目标也许可以的运气真好。 在 Azure 机器学习集合中还有专门负责排名预测 （[序号回归](https://msdn.microsoft.com/library/azure/dn906029.aspx)） 中的算法，计算预测 （[泊松回归](https://msdn.microsoft.com/library/azure/dn905988.aspx)） 和异常检测 （一个基于[主体的组件分析](https://msdn.microsoft.com/library/azure/dn913102.aspx)和基于[支持向量机](https://msdn.microsoft.com/library/azure/dn913103.aspx)的 s）。
还有一个独立群集算法以及 （[K 表示](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)）。

![基于 PCA 的异常检测][8]

***基于 PCA 的异常检测****-绝大多数数据点偏离极大地分布可疑落所谓的分布;*

![使用 K 表示分组的数据集][9]

***数据集分为 5 群集使用 K 方法***

此外，还有 ensemble[一个 v 全部 multiclass 分类器](https://msdn.microsoft.com/library/azure/dn905887.aspx)，其 N 类分类问题分成 N-1 两类分类问题。 由两类分类器用于确定的准确性、 培训时间和线性属性。

![两类分类器组合在一起构成一个三级分类器][10]

***对两类分类器组合在一起形成的三级分类器***

Azure 机器学习还包括一个功能强大的机器学习框架，标题下的[Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf)访问。
VW 分别分类在这里，因为它可以学习分类和回归问题，甚至可以了解从部分标记数据。 您可以将其配置为使用任何一种学习算法、 损失函数和优化算法的数字。 它被可从地面向上高效、 并行，且速度非常快。 它可以处理很少明显的工作的几大功能集。
开始，由微软研究院自己 John Langford 牵头，VW 是股票汽车算法的一个字段的公式某一项。 不是每个问题会使 VW，但如果您这样做，它可能值得在其界面上攀登了学习时间。 它同时也是以多种语言编写[独立的开源的代码](https://github.com/JohnLangford/vowpal_wabbit)。


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
