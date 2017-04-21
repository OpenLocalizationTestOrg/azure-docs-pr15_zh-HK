<properties
    pageTitle="重写默认 HTTP 行为中使用规则引擎的 Azure CDN |Microsoft Azure"
    description="规则引擎允许您自定义的 HTTP 请求如阻止某些类型的内容传递的处理通过 Azure CDN，定义缓存策略，并修改 HTTP 标头。"
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
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>重写默认 HTTP 行为使用规则引擎

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>概述

规则引擎允许您自定义的 HTTP 请求的处理方式，如阻止某些类型的内容的交付、 定义缓存策略，以及修改 HTTP 标头。  本教程将说明创建一个规则来将更改 CDN 资源的缓存行为。  还有视频内容可在"[请参阅](#see-also)"部分中。

## <a name="tutorial"></a>教程

1. 从 CDN 配置文件刀片式服务器，请单击**管理**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-rules-engine/cdn-manage-btn.png)

    CDN 管理门户打开。

2. 单击**大 HTTP**选项卡上，跟**规则引擎**。

    将显示一个新的规则的选项。

    ![CDN 新规则选项](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] 多个规则中列出的顺序会影响如何处理它们。 后续的规则可能会覆盖前面的规则所指定的操作。
    
3. 输入的名称在**名称 / 描述**文本框。

4. 标识的请求的规则将应用于的类型。  默认情况下，**始终**匹配条件处于选中状态。  您将**始终**用于本教程，所以让选择。

    ![CDN 匹配条件](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] 有许多类型的匹配条件下拉列表中可用。  点击左侧的匹配条件的蓝色信息图标将说明中详细信息的当前所选的条件。
    >
    >匹配条件的详细信息的完整列表，请参阅[规则引擎匹配条件和功能的详细信息](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0)。

5.  单击**+**按钮**功能**来添加新功能。  在左侧的下拉列表，选择**强制内部的最大期限**。  在出现的文本框中，输入**300**。  将保留剩余的默认值。

    ![CDN 功能](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] 作为匹配条件，单击蓝色的信息图标左侧的新功能将显示有关此功能的详细信息。  对于**强制内部的最大期限**，我们重资产的**缓存控制**和**过期**标头控件时 CDN 边缘节点将会刷新，从原点资产。  我们的示例中 300 秒意味着 CDN 边缘节点将缓存 5 分钟，然后刷新该资产从其原点的资产。
    >
    >详细的功能的完整列表，请参阅[规则引擎匹配条件和功能的详细信息](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)。

6.  单击**添加**按钮以保存新的规则。  现在，新规则正在等待批准。 后已获批准，状态将变为从**挂起 XML** **有效的 XML**。

    >[AZURE.IMPORTANT] 规则的更改可能需要 90 分钟才能通过 CDN 传播。

## <a name="see-also"></a>请参见
* [Azure 星期五︰ Azure CDN 的强大的新高级功能](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/)（视频）
* [规则引擎匹配条件和功能的详细信息](https://msdn.microsoft.com/library/mt757336.aspx)
