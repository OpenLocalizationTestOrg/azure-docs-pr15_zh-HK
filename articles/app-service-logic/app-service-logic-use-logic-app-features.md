<properties 
    pageTitle="使用应用程序逻辑功能 |Microsoft Azure" 
    description="了解如何使用逻辑应用程序的高级的功能。" 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>使用逻辑应用程序功能

在[前面的主题](app-service-logic-create-a-logic-app.md)中，您将创建第一个逻辑应用程序。 现在我们将向您展示如何构建一个更加完整的过程，使用应用程序服务的逻辑应用程序。 本主题介绍以下新的应用程序逻辑概念︰

- 条件逻辑，它仅在某些条件满足时执行操作。
- 要编辑现有的逻辑应用程序的代码视图。
- 启动工作流的选项。

完成本主题之前，您应该完成[创建新的逻辑应用程序](app-service-logic-create-a-logic-app.md)中的步骤。 在[Azure 的门户]中，浏览到您的逻辑应用程序并单击要编辑的逻辑应用程序定义的摘要中的**触发器和操作**。

## <a name="reference-material"></a>参考资料

您可能会发现以下文档很有用︰

- [管理和运行时 REST Api](https://msdn.microsoft.com/library/azure/mt643787.aspx) -其中包括如何直接调用逻辑应用程序
- [语言参考](https://msdn.microsoft.com/library/azure/mt643789.aspx)的所有受支持的函数/表达式的完整列表
- [触发器和操作类型](https://msdn.microsoft.com/library/azure/mt643939.aspx)的操作的不同类型和它们所采取的输入
- [应用程序服务概述](../app-service/app-service-value-prop-what-is.md)-选择要生成的解决方案组件的描述

## <a name="adding-conditional-logic"></a>添加条件逻辑

尽管原始流的工作方式，有一些可以改进的领域。 


### <a name="conditional"></a>条件
此逻辑应用程序可能会导致您收到大量电子邮件。 以下步骤添加逻辑，以确保在 tweet 来自人与一定数量的用户只收到的电子邮件。 

1. 单击加号并查找 Twitter*获取用户*的操作。

2. 从触发器来获取有关 Twitter 用户的信息传递**的 Tweeted**字段中。

    ![获取用户](./media/app-service-logic-use-logic-app-features/getuser.png)

3. 再次单击加号，但这一次选择**添加条件**

4. 在第一个框中，单击**获取用户**要查找的**用户计数**字段下面的**...** 。

5. 在下拉列表中选择**大于**

6. 在第二个框中键入您希望用户拥有的用户数。

    ![条件](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  最后，拖放电子邮件框中的**如果是**框中。 这将意味着满足随动计数时，将仅有电子邮件。

## <a name="repeating-over-a-list-with-foreach"></a>重复使用 forEach 列表

在 forEach 循环指定要重复上一个操作的数组。 如果它不是一个数组流无法正常工作。 举一个例子，如果您有 action1 输出数组的邮件，并且您想要发送的每封邮件，您可以包括此 forEach 语句操作的属性中︰ forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>使用代码视图中编辑逻辑应用程序

除了设计器中，可以直接编辑代码，如下所示定义逻辑的应用程序。 

1. 单击命令栏中的**代码视图**按钮。 

    这将打开显示您刚编辑过的定义完全编辑器。

    ![代码视图](./media/app-service-logic-use-logic-app-features/codeview.png)

    通过使用文本编辑器，您可以复制和粘贴操作在同一个逻辑应用程序或逻辑应用程序之间的任何数。 可以方便地添加或从该定义中，删除整节，也可以与其他人共享定义。

2. 在代码视图中进行更改后，只需单击**保存**。 

### <a name="parameters"></a>参数
有逻辑的应用程序只能在代码视图中的某些功能。 其中的一个示例是参数。 参数方便地重复使用整个逻辑应用程序的值。 例如，如果您希望在多个操作中的使用的电子邮件地址，您应定义它作为参数。

下列更新您现有的逻辑应用程序参数使用的查询词。

1. 在代码视图中，找到`parameters : {}`对象并插入以下主题对象︰

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. 滚动到`twitterconnector`操作，找到的查询，并将其替换为`#@{parameters('topic')}`。
    您可以使用**concat**函数联接在一起两个或多个字符串，例如︰`@concat('#',parameters('topic'))`与上述相同。 
 
参数是拉出值可能会更改太多的好办法。 当您需要重写在不同环境中的参数时，它们将非常有用。 有关如何重写基于环境参数的详细信息，请参阅我们的[REST API 文档](https://msdn.microsoft.com/library/mt643787.aspx)。

现在，当您单击**保存**，每小时可以有五个以上 retweets 传递到称为**tweets**中您收存箱文件夹中所有新 tweets。

若要了解有关逻辑应用程序定义的详细信息，请参阅[作者逻辑应用程序定义](app-service-logic-author-definitions.md)。

## <a name="starting-a-logic-app-workflow"></a>启动逻辑应用程序流
有几个不同的选项来启动您的逻辑应用程序中定义的工作流。 工作流可以始终启动按需在[Azure 的门户]。

### <a name="recurrence-triggers"></a>重复执行触发器
您指定的时间间隔运行定期触发器。 当触发器条件逻辑时，触发器将确定需要运行该工作流。 触发器表明它应该运行通过返回`200`状态代码。 当它并不需要运行时，它返回`202`状态代码。

### <a name="callback-using-rest-apis"></a>使用 REST Api 的回调
服务可以调用来启动工作流的逻辑应用程序终结点。 [为可调用的终结点的逻辑应用程序](app-service-logic-connector-http.md)的详细信息，请参见 若要启动这种逻辑应用程序请求，请单击命令栏上的**立即运行**按钮。 

<!-- Shared links -->
[Azure 门户]: https://portal.azure.com 