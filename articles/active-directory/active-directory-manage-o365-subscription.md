<properties
   pageTitle="为 Azure 中订购 Office 365 管理目录 |Microsoft Azure"
   description="使用 Azure Active Directory 和 Azure 的经典门户 Office 365 订阅目录进行管理"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>为 Azure 中订购 Office 365 管理目录

本文介绍如何管理对于 Office 365 的订阅，使用 Azure 的经典门户创建一个目录。 您必须服务管理员或订阅了 Azure 的联管理员登录到 Azure 的传统门户网站。 如果您还没有 Azure 的订阅，可以立即注册为[免费试用 30 天](https://azure.microsoft.com/trial/get-started-active-directory/)和第一个云解决方案部署在 5 分钟内，使用此链接。 请务必使用您用于登录到 Office 365 的工作或学校科目。

Azure 的订阅后，您可以登录到 Azure 的传统门户网站并访问 Azure 服务。 单击要管理验证 Office 365 提供用户位于同一目录的 Active Directory 扩展。

如果已经订阅了 Azure，来管理其他目录的过程也是简单明了的。 例如，迈克尔 • 史密斯可能有 Contoso.com Office 365 的订阅。 他也有他通过他的 Microsoft 帐户，签约了 Azure 订阅msmith@hotmail.com。 在这种情况下，他管理着两个目录。

  订阅 |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  显示名称 |  Contoso  |     默认 Azure 活动目录 (AD Azure) 目录
  域名称  |  contoso.com  | msmithhotmail.onmicrosoft.com

他想要管理 Contoso 目录中的用户标识，而他登录到 Azure 使用 Microsoft 帐户，以便他可以启用 Azure 的广告功能，如多因素身份验证。 下面的关系图来帮助说明此过程。

![图中两个独立的目录管理](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

在这种情况下，这两个目录是相互独立。

## <a name="to-manage-two-independent-directories"></a>若要管理两个独立的目录
为了使迈克尔 • 史密斯，他登录到作为 Azure 时管理这两个目录msmith@hotmail.com,他必须完成以下步骤︰

> [AZURE.NOTE]
> 仅当用户登录到 Microsoft 帐户时，可以完成这些步骤。 如果用户登录到某一工作或学校科目，选择**使用现有目录**不可用。 工作或学校的科目只需要经过其主目录 （即，目录的工作或学校科目存储位置，同时业务或学校拥有）。

1.  登录到[Azure 的传统门户网站](https://manage.windowsazure.com)为msmith@hotmail.com。
2.  单击**新建** > **应用程序服务** > **Active Directory** > **目录** > **自定义创建**。
3.  单击使用现有目录并选择**我已准备好要现在注销**复选框。
4.  作为 Contoso.onmicrosoft.com 的全局管理员登录到 Azure 的传统门户网站 (例如， msmith@contoso.com)。
5.  当系统提示**Azure 使用 Contoso 目录？**，单击**继续**。
6.  单击**立即注销**。
7.  登录到作为 Azure 的传统门户网站msmith@hotmail.com。 Contoso 目录和默认目录显示在 Active Directory 扩展。

在完成以上步骤后msmith@hotmail.com是 Contoso 目录中的全局管理员。

## <a name="to-administer-resources-as-the-global-admin"></a>以全局管理员身份管理资源
现在让我们假设 Jane Doe 需要管理的 Azure 订阅与相关联的网站和数据库资源msmith@hotmail.com。 她可以做到这一点之前，迈克尔 • 史密斯需要完成以下附加步骤︰

1.  登录到[Azure 的传统门户网站](https://manage.windowsazure.com)使用 Azure 订阅的服务管理员帐户 (在此示例中， msmith@hotmail.com)。
2.  将订阅传送到 Contoso 目录︰ 单击**设置** > **订阅**> 选择订购 >**编辑目录**> 选择**Contoso (Contoso.com)**。 作为传输、 任何工作或学校的一部分删除订阅的联管理员帐户。
3.  作为联管理员订阅添加 Jane Doe︰ 单击**设置** > **管理员**> 选择订购 >**添加**> 类型**JohnDoe@Contoso.com**。

## <a name="next-steps"></a>下一步行动
有关订阅和目录之间的关系的详细信息，请参阅[如何订阅是与目录关联](active-directory-how-subscriptions-associated-directory.md)。
