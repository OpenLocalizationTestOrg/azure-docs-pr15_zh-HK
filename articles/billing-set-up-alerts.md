<properties
    pageTitle="设置计费通知 Microsoft Azure 订阅 |Microsoft Azure"
    description="描述如何您可以将通知设置 Azure 账单上以避免计费尽在掌握之中。"
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>设置 Microsoft Azure 订阅付费通知

您是否担心花费 Azure 订阅每月的多少？ 如果你订阅了 Azure 的帐户管理员，可以使用 Azure 计费通知服务来创建自定义记帐警报可帮助您监视和管理 Azure 帐户付费活动。

此服务是预览服务，因此您所要做的第一件事是符号组成。 [预览功能页](https://account.windowsazure.com/PreviewFeatures)在 Azure 帐户管理门户，请不要启用此功能，请访问。

## <a name="set-the-alert-threshold-and-email-recipients"></a>设置警报的阈值和电子邮件收件人

接收订阅付费服务已被打开的电子邮件确认后，请访问帐户门户中的[订阅页](https://account.windowsazure.com/Subscriptions)。 单击要监视的订阅，然后单击**警报**。

![][Image1]

接下来，单击**添加警报**以创建您的第一个-您可以设置五个计费通知每个订阅，与不同的阈值和最多两个电子邮件收件人的每个通知的总数。

![][Image2]

添加警报时，您为其指定一个唯一的名称、 选择花费的阈值，并选择都将发送通知的电子邮件地址。 当设置阈值，可以从**警报的**列表中选择**总帐单**或**货币的信用**。 帐单总计，在订阅花费超过阈值时发送警报。 货币信用，货币信用限额以下时，会发出一个警报。 货币信用通常应用到免费试用版和订阅 MSDN 帐户相关联。

![][Image3]

Azure 支持任何电子邮件地址，但并不验证您的电子邮件地址有效，所以仔细检查输入错误。

## <a name="check-on-your-alerts"></a>检查您的通知

设置警报后，帐户中心列出它们，并显示多少更设置了。 对于每个警报，您可以看到的日期和发送的时间，不管是警报总帐单或货币的信用，您设置的限制。 日期和时间格式是 24 小时通用协调时间 (UTC)，日期是 yyyy-月-日格式。 单击在列表中进行编辑，警报的加号或者单击垃圾桶可以将其删除。

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
