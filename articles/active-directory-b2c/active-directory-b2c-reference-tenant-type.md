<properties
    pageTitle="Azure 的活动目录 B2C︰ 生产规模与预览 B2C 承租人 |Microsoft Azure"
    description="在 Azure 活动目录 B2C 租户的类型上一个主题"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure 的活动目录 B2C︰ 生产规模与预览 B2C 承租人

如果您打算在 Azure 活动目录 (AD Azure) B2C 上写入一个生产应用程序，需要能够确定您拥有合适的租户"类型"上投入。 若要了解您遵循下列步骤以[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)在 Azure 的门户网站上，而且**租户类型**下找到。

## <a name="summary"></a>摘要

Azure AD B2C 支持上**生产规模**B2C 承租人在北美洲的生产应用程序。

| 组织类型 | 国家/地区 | 通常可用？ |
| ----------- | -------------- | --------------------- |
| **生产规模租户** | 北美国家/地区 | 是的 |
| **生产规模租户** | 除北美地区以外的所有国家/地区 | 不 |
| **预览租户** | 所有国家/地区 | 不 |

> [AZURE.NOTE]
在几个国家或地区 （员工） 的 Azure AD 承租人有 （对消费者） 的 azure AD B2C 承租人是当前不可用。 阅读以下有关更多详细信息。

## <a name="production-scale-b2c-tenant-in-north-america"></a>在北美的生产规模 B2C 租户

如果[创建 B2C 租户](active-directory-b2c-get-started.md)在北美地区，即在以下国家或地区之一︰ 美国状态、 加拿大、 哥斯达黎加、 多米尼加共和国、 萨尔瓦多、 危地马拉、 墨西哥、 巴拿马、 波多黎各和特立尼达和多巴哥，特立尼达和多巴哥和 B2C Admin UI 上的**租户类型**说**生产规模**，您租户可用于生产应用程序。

> [AZURE.NOTE]
生产规模承租人它们可以扩展到数百数以百万计的消费者每个租户的身份。

![生产规模租户的屏幕抓图](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>任何国家/地区中的预览 B2C 租户

如果 Azure AD B2C 预览期间创建了 B2C 租户，则很可能您**键入租户**说**预览租户**。 如果出现这种情况，您必须使用您的租户仅用于开发和测试的目的，并不用于生产应用程序。

> [AZURE.IMPORTANT]
没有从预览 B2C 租户到生产规模 B2C 租户的迁移路径。 请注意，存在一些已知问题时删除预览 B2C 租户并重新创建具有相同域名生产规模 B2C 租户。 您必须使用不同的域名创建生产规模 B2C 租户。

![预览租户的屏幕抓图](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>北美以外地区的生产规模 B2C 租户

Azure AD B2C，目前的上市北美以外地区。 但是您可以创建和生产规模的承租人，用于开发和测试目的，下列国家或地区之一︰ 阿尔及利亚、 奥地利、 阿塞拜疆、 巴林、 白俄罗斯、 比利时、 保加利亚、 克罗地亚、 塞浦路斯、 捷克共和国、 丹麦、 埃及、 爱沙尼亚、 芬兰、 法国、 德国、 希腊、 匈牙利、 冰岛、 爱尔兰、 以色列、 意大利、 约旦、 哈萨克斯坦、 肯尼亚、 科威特、 Lativa，黎巴嫩、 列支敦士登、 Lituania，卢森堡、 马其顿马其顿、 马耳他、 黑山、 摩洛哥、 荷兰、 尼日利亚、 挪威阿曼、 巴基斯坦、 波兰、 葡萄牙、 卡塔尔、 罗马尼亚、 俄罗斯、 沙特阿拉伯、 塞尔维亚、 斯洛伐克、 斯洛文尼亚、 南非、 西班牙、 瑞典、 瑞士、 突尼斯、 土耳其、 乌克兰，美国阿拉伯联合酋长国和大不列颠及北爱尔兰联合王国。

一旦 Azure AD B2C 宣布公众在上述国家或地区，您可以继续使用这些生产规模承租人和投入与您的生产应用程序不会丢失任何数据。

## <a name="availability-of-b2c-tenants"></a>B2C 承租人的可用性

B2C 承租人处于当前不可用的下列国家或地区︰ 阿富汗、 阿根廷、 澳大利亚、 巴西、 智利、 哥伦比亚、 厄瓜多尔、 中国香港特别行政区、 印度、 印度尼西亚、 伊拉克、 日本、 韩国、 马来西亚、 新西兰、 巴拉圭、 秘鲁、 菲律宾、 新加坡、 斯里兰卡、 台湾、 泰国、 乌拉圭和委内瑞拉。 我们计划以便将它们包含在将来。
