<properties 
   pageTitle="调试 U SQL 作业 |Microsoft Azure" 
   description="了解如何调试使用 Visual Studio 的 U SQL 失败的顶点。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>调试 C# 代码中 U SQL 数据湖分析作业 

了解如何使用 Azure 数据湖 Visual Studio 工具来调试失败的 U SQL 作业由于用户代码中的 bug。 

Visual Studio 工具允许您从已编译的代码和所需的顶点数据群集来跟踪和调试失败的作业。

大数据系统通常提供扩展性模型中的通过语言，如 Java、 C#、 Python 等。许多这些系统提供有限运行时调试很难进行调试自定义代码中的运行时错误的信息。 最新的 Visual Studio 工具附带了一项称为"失败顶点调试"功能。 使用此功能，您可以将运行时数据从下载 Azure 到本地工作站，以便您可以调试故障自定义 C# 代码中使用同一个运行时和精确的输入的数据从云。  问题得到解决后，您可以重新运行修改后的代码在 Azure 从工具。

此功能的视频演示，请参见[调试 Azure 数据湖分析中的自定义代码](https://mix.office.com/watch/1bt17ibztohcb)。

>[AZURE.NOTE] Visual Studio 可能会挂起或失败如果没有以下两个 windows 升级︰ [Microsoft Visual C++ 2015年可再发行组件更新 2](https://www.microsoft.com/download/details.aspx?id=51682)、[通用 C 运行时的窗口](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0)。


##<a name="prerequisites"></a>系统必备组件
-   已通过[入门](data-lake-analytics-data-lake-tools-get-started.md)文章。

## <a name="create-and-configure-debug-projects"></a>创建和配置调试项目

在数据湖 Visual Studio 工具打开失败的作业时，您将收到预警。 错误详细的信息将显示在错误选项卡和黄色警告栏窗口的顶部。 

![Azure 数据湖分析 U SQL 调试 visual studio 下载顶点](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**若要下载顶点并创建调试解决方案**

1.  在 Visual Studio 中打开失败的 U SQL 作业。
2.  单击**下载**以下载所有必需的资源和输入的流。 如果下载失败，请单击**重试**。
3.  下载完成创建本地调试项目后，请单击**打开**。 将创建空项目称为**LocalVertexHost**称为**VertexDebug**的新 Visual Studio 解决方案。

如果 U SQL 代码 (Script.usql.cs) 中使用用户定义的运算符，则必须用用户定义的运算符代码，创建一个类库 C# 项目，包括 VertexDebug 解决方案中的项目。

如果您对数据湖分析数据库注册.dll 程序集，必须将这些程序集的源代码添加 VertexDebug 解决方案。
 
如果您对数据湖分析数据库创建单独 C# 类库为 U SQL 代码和注册的.dll 程序集，您需要添加到 VertexDebug 解决方案程序集的版本 C# 项目。

在极少数情况下，U SQL 代码 (Script.usql.cs) 文件中的原始解决方案中使用用户定义的运算符。 如果您想要使其正常运行，您需要创建 C# 库包含的源代码并将程序集名称更改为在群集中注册。 您可以通过检查获得群集中运行的脚本在群集中注册的程序集名称。 可以通过打开 U SQL 作业来执行此操作，然后单击作业面板中的"脚本"。 

**若要配置解决方案**

1.  从解决方案资源管理器中，右击刚刚创建的 C# 项目，然后单击**属性**。
2.  设置为工作目录的路径的 LocalVertexHost 项目的输出路径。 您可以通过 LocalVertexHost 属性的 LocalVertexHost 项目工作目录路径。
3.  将.pdb 文件放入 LocalVertexHost 项目工作目录中，以便生成 C# 项目，或者您可以手动将.pdb 文件复制到此文件夹。
4.  在**异常设置**，请检查公共语言运行时异常︰

![Azure 数据湖分析 U SQL 调试 visual studio 设置](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>调试工作

已通过下载顶点创建调试解决方案和配置环境之后，您可以开始调试 U SQL 代码。

1.  从解决方案资源管理器中，右击刚刚创建的**LocalVertexHost**项目，指向**调试**，，然后单击**启动新实例**。 LocalVertexHost 必须设置为启动项目。 它可以忽略第一次，可能会看到以下消息。 它可能需要一分钟的时间才能到调试屏幕。
 
    ![Azure 数据湖分析 U SQL 调试 visual studio 警告](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  使用 Visual Studio 根据调试体验 （监视、 变量等） 来解决问题。 
5.  在确定问题后，修复代码，然后重新测试，直到所有问题都得到解决之前重新生成 C# 项目。 在调试后已成功完成，输出窗口中显示下面的消息 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>重新提交作业

U SQL 代码调试完成后，您可以重新提交失败的作业。

1. 注册新的.dll 程序集对 ADLA 数据库。

    1.  从服务器资源管理器/云资源管理器数据湖 Visual Studio 工具中，展开**数据库**节点 
    2.  右击程序集注册程序集。 
    3.  注册新的.dll 程序集，对 ADLA 数据库。
 
2.  或将您的 C# 代码复制到 script.usql.cs-C# 代码隐藏文件。
3.  重新提交您的工作。

##<a name="next-steps"></a>下一步行动

- [有关 Azure 数据湖分析 U SQL 语言入门教程︰](data-lake-analytics-u-sql-get-started.md)
- [教程︰ 开发数据湖工具使用 Visual Studio 的 U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)
- [开发 Azure 数据湖分析作业的 U SQL 用户定义运算符](data-lake-analytics-u-sql-develop-user-defined-operators.md)

