<properties
   pageTitle="Azure AD 连接︰ 在预览功能 |Microsoft Azure"
   description="本主题介绍在 Azure AD 连接在预览中的更多详细信息功能。"
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
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>有关在预览功能的更多详细信息
本主题介绍如何在预览中当前使用的功能。

## <a name="group-writeback"></a>组的写回
可选功能中的组回写选项将允许您写回**Office 365 组**到一个目录林向具有安装 Exchange。 这是一组在云中始终掌握了它。 如果您有交换场所，然后您可以将写回到这些组内部因此具有内部 Exchange 邮箱的用户可以发送和接收电子邮件，这些用户组。

有关 Office 365 组以及如何使用它们的详细信息可以找到[这里](http://aka.ms/O365g)。

此组将用内部的通讯组表示 AD DS。 内部部署 Exchange 服务器必须在 Exchange 2013 累积更新 8 （发布的 3 月 2015年） 或交换 2016 能够识别这个新的组类型。

**在预览的过程的说明**

- 地址簿属性当前不填充在预览中。 如果没有此特性，组不会显示在 GAL 中。 填充此属性的最简单方法是使用 Exchange PowerShell cmdlet `update-recipient`。
- 仅林与交换架构是有效目标组。 如果检测不到任何 Exchange，组写回将无法启用。
- 目前支持的只有单个目录林 Exchange 组织部署。 如果您有多个 Exchange 组织内部，您将需要这些组出现在您的其他目录林内部 GALSync 解决方案。
- 组的写回功能当前不处理安全组或通讯组。

>[AZURE.NOTE] 对 Azure AD 特优的订阅是必需的组写回。

## <a name="user-writeback"></a>用户写回
> [AZURE.IMPORTANT] 已删除的用户写回预览功能在 Azure AD 连接 8 月 2015年更新。 如果您启用了它，您应禁用该功能。

## <a name="next-steps"></a>下一步行动
继续[的 Azure AD 连接的自定义安装](./connect/active-directory-aadconnect-get-started-custom.md)。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
