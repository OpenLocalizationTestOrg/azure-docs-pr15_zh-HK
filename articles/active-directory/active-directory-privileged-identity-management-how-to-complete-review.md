<properties
   pageTitle="如何完成访问审查 |Microsoft Azure"
   description="在 Azure AD 特权身份管理以启动访问审阅后，学习如何完成它并查看结果"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>如何完成在 Azure AD 特权身份管理的访问审核


特权的角色管理员可以查看访问权限一次[安全检查已启动](active-directory-privileged-identity-management-how-to-start-security-review.md)。 Azure AD 特权身份管理 (PIM) 将自动发送电子邮件提示用户查看他们的访问权限。 如果用户没有收到一封电子邮件，您可以发送这些说明如何[进行安全检查](active-directory-privileged-identity-management-how-to-perform-security-review.md)。

安全审查周期已结束，或所有用户已都完成其自我考核后，按照管理评审和查看结果这篇文章中的步骤操作。

## <a name="manage-security-reviews"></a>管理安全审核

1. 转到[Azure 门户网站](https://portal.azure.com/)并选择您的仪表板上的**Azure AD 特权身份管理**应用程序。
2. 选择仪表板的**访问权限检查**部分。
3. 选择您想要管理的访问检查。

访问检查的细节刀片式服务器上有编号选项可用于管理审阅。

![PIM 访问查看按钮的屏幕抓图][1]

### <a name="remind"></a>提醒

如果访问检查设置，以便用户查看自己，**提醒**按钮发送通知。 

### <a name="stop"></a>停止

所有访问评审都具有结束日期，但您可以使用**停止**按钮提前完成了它。 如果任何用户还没有通过这次评审，他们也不能向后停止审查。 已停止后，无法重新启动审查。

### <a name="apply"></a>应用

访问检查完毕后，也因为到达结束日期或手动，停止其**应用**按钮可实现评审的结果。 如果评审中，用户的访问被拒绝，则将删除其角色分配的步骤。  

### <a name="export"></a>导出

如果您想手动应用安全检查的结果，您可以导出评审。 **导出**按钮将开始下载一个 CSV 文件。 您可以在 Excel 或其他程序，打开 CSV 文件中的结果。

### <a name="delete"></a>删除

如果您不感兴趣的任何进一步的检查，请将其删除。 **删除**按钮删除 PIM 应用程序审查。

> [AZURE.IMPORTANT] 您不会得到警告，删除操作发生之前，因此请确保您要删除该审阅。


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
