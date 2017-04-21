<properties
    pageTitle="Azure AD 连接同步︰ 同步服务管理器用户界面 |Microsoft Azure"
    description="理解 Azure AD 连接在同步服务管理器的操作选项卡。"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD 连接同步︰ 同步服务管理器

[操作](active-directory-aadconnectsync-service-manager-ui-operations.md) | [连接器](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [元节设计器](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [元搜索](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

操作选项卡显示的最近操作的结果。 此选项卡上是要理解和解决问题的关键。

## <a name="understand-the-information-visible-in-the-operations-tab"></a>了解操作选项卡中显示的信息
上半部分中慢性顺序显示所有运行。 默认情况下，操作日志保留过去七天内，有关的信息，但与[计划程序](active-directory-aadconnectsync-feature-scheduler.md)可以更改此设置。 您想要查找未显示成功状态的任何运行。 您可以更改通过单击标题来排序。

**状态**列是最重要的信息，并显示为运行最严重的问题。 这里是最常见的优先级顺序研究状态的摘要 (其中 * 指出几个可能的错误字符串)。

状态 | 注释
--- | ---
已停止-* | 无法完成运行。 例如，如果远程系统已关闭，无法联系。
停止错误限制 | 有 5000 多个错误。 由于大量的错误自动停止运行。
已完成-\*-错误 | 运行已完成，但有错误 (少于 5000)，应进行调查。
已完成-\*-警告 | 运行已完成，但某些数据并不是处于预期的状态。 如果存在错误，然后此消息通常是只是一种症状。 直到您已经解决了的错误，您不应该调查警告。
成功 | 没有问题。

当您选择了一行时，底部将更新以显示该运行的详细信息。 到最左边的底部，您可能必须说**步骤 #**列表。 如果您的目录林中每个域都由某一步骤中有多个域，才会显示此列表。 在**分区**的标题下找不到的域名。 在**同步统计信息**，可以找到有关的已处理的更改数的详细信息。 您可以单击链接以获取更改的对象的列表。 如果您有对象有错误，这些错误会显示在**同步错误**。

## <a name="troubleshoot-errors-in-operations-tab"></a>在操作选项卡中的错误的疑难解答
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
如果有错误，这两个对象中的错误，错误本身都提供了详细信息的链接。

首先单击 （图片中的**同步--错误的函数-触发规则**），错误字符串。 首先显示该对象的概述。 若要查看实际的错误，请单击按钮**堆栈跟踪**。 此跟踪提供了错误的调试级别信息。

**提示︰**可以在**调用堆栈信息**框中单击鼠标右键，选择**全选**，和**复制**。 然后可以复制到堆栈，并在自己喜爱的编辑器 （如记事本） 中查看错误。

- 如果错误是从**SyncRulesEngine**，然后调用堆栈信息首先对所有属性的列表对象。 向下滚动直到可以看到标题**内部异常 = >**。  
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
后面的行显示错误。 如上图所示的错误是从创建自定义同步规则 Fabrikam。

如果错误本身不会提供足够的信息，然后就可以看一看数据本身。 您可以单击的对象标识符和[遵循对象和系统中的数据](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system)的链接。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
