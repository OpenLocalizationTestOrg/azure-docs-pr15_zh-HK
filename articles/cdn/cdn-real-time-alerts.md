<properties
    pageTitle="Azure CDN 实时警报 |Microsoft Azure"
    description="在 Microsoft Azure CDN 的实时警报。 实时警报提供了有关性能的 CDN 配置文件中的终结点的通知。"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>在 Microsoft Azure CDN 的实时警报

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>概述

本文介绍了 Microsoft Azure CDN 中的实时警报。 此功能提供实时通知有关性能的 CDN 配置文件中的终结点。  您可以设置电子邮件或基于 HTTP 的警报︰

* 带宽
* 状态代码
* 高速缓存状态
* 连接

## <a name="creating-a-real-time-alert"></a>创建一个实时警报

1. 在[Azure 门户网站](https://portal.azure.com)，浏览到您 CDN 的配置文件。

    ![CDN 配置文件刀片式服务器](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. 从 CDN 配置文件刀片式服务器，请单击**管理**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    CDN 管理门户打开。

3. 悬停在**分析**选项卡，然后悬停在**实时统计**飞出。  请单击**实时警报**。

    ![CDN 管理门户](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    将显示现有的警报配置 （如果有） 的列表。

4. 单击**添加通知**按钮。

    ![添加警报按钮](./media/cdn-real-time-alerts/cdn-add-alert.png)

    显示窗体用于创建新的警报。

    ![新通知窗体](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. 如果您希望此警报处于活动状态时单击**保存**，，选中**启用通知**复选框。

6. 在**名称**字段中输入您的通知的描述性名称。

7. 在**媒体类型**下拉列表中，选择**HTTP 大型对象**。

    ![与 HTTP 大型对象选择的媒体类型](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] 您必须选择**HTTP 大对象**作为**媒体类型**。  其他选项不使用**Verizon 从 Azure CDN**的。  不选择**HTTP 大型对象**将导致您永远不会被触发的通知。

8. 创建一个**表达式**来监视通过选择**指标**、**运算符**和**触发器值**。

    - 为**指标**，选择要监视的条件的类型。  **Mbps 带宽**是大量在兆位 / 秒的带宽使用情况。  **总连接**数是我们边缘服务器并发 HTTP 连接的数量。  有关的各种缓存状态和状态代码定义，请参阅[Azure CDN 缓存状态代码](https://msdn.microsoft.com/library/mt759237.aspx)和[Azure CDN HTTP 状态代码](https://msdn.microsoft.com/library/mt759238.aspx)
    - **运算符**是建立度量和触发器值之间关系的数学运算符。
    - **触发器值**是发送通知之前必须满足的临界值。

    在下面的示例中，我已创建的表达式表示希望 404 状态码数大于 25 时得到通知。

    ![实时警报示例表达式](./media/cdn-real-time-alerts/cdn-expression.png)

9. 为**间隔**，输入频率想要计算的表达式。

10. 在**通知在**下拉列表中，选择当您想要的表达式为真时得到通知。
    
    - **启动条件**指示第一次检测到指定的条件时，将发送通知。
    - **结束条件**表示不再检测到指定的条件时，将发送通知。 我们监视系统的网络检测到指定的条件出现后只可触发此通知。
    - **连续**指示，通知将发送每次网络监视系统检测到指定的条件。 请记住，网络监视系统将只检查一次每个间隔所指定的条件。
    - **条件 Start 和 End**指示，通知将发送第一次检测到指定的条件时，再一次当条件不能检测到。

11. 如果您想要接收电子邮件通知，请检查**通过电子邮件通知**复选框。  

    ![通过电子邮件形式通知](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    在**到**字段中，输入您要通知发送的电子邮件地址。 **主题**和**正文**，您可以保留默认值，或您可以自定义使用**可用的关键字**列表来在发送邮件时动态地插入警报数据的消息。

    > [AZURE.NOTE] 通过单击**测试通知**按钮，但仅在已保存警报的配置后，您可以测试电子邮件通知。

12. 如果您希望通知被发送到 web 服务器，请检查**通过 HTTP 发送通知**复选框。

    ![通过 HTTP Post 表单通知](./media/cdn-real-time-alerts/cdn-notify-http.png)

    在**Url**字段中，输入您要的 HTTP 消息发布的 URL。 在**标题**文本框中，输入在请求中发送的 HTTP 标头。  对于**体**中，您可以自定义使用**可用的关键字**列表来在发送邮件时动态地插入警报数据的消息。  **标头**和**正文**默认为类似于 XML 有效负载下的示例。

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] 通过单击**测试通知**按钮，但仅在已保存警报的配置后，您可以测试 HTTP 发送通知。

13. 单击**保存**按钮以保存配置警报。  如果在步骤 5 中选中**启用通知**，通知现活动。

## <a name="next-steps"></a>下一步行动

- [在 Azure CDN 的实时统计](cdn-real-time-stats.md)分析
- 使用[高级的 HTTP 报告](cdn-advanced-http-reports.md)深入
- 分析[使用模式](cdn-analyze-usage-patterns.md)

