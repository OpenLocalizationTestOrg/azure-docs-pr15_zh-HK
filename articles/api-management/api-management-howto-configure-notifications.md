<properties 
    pageTitle="如何在 Azure API 管理中配置通知和电子邮件模板" 
    description="了解如何配置通知和电子邮件模板在 Azure API 管理。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>如何在 Azure API 管理中配置通知和电子邮件模板

API 管理提供配置特定的事件通知，并配置用于与管理员和开发人员的 API 管理实例进行通信的电子邮件模板的功能。 本主题演示如何配置为可用的事件通知，并概述配置用于这些事件的电子邮件模板。

## <a name="publisher-notifications"></a>配置发布服务器通知

要配置通知，请单击**管理**API 管理服务的 Azure 传统门户。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

单击在左侧以查看可用的通知**API 管理**菜单中的**通知**。

![发布服务器上的通知][api-management-publisher-notifications]

下面几个事件可以配置为通知。

-   **订阅请求 （需要批准）**的用户与指定的电子邮件收件人将收到有关 API 需要批准的产品的预订请求的电子邮件通知。
-   **新订阅**的用户与指定的电子邮件收件人将收到有关新的 API 产品订阅电子邮件通知。
-   在新的应用程序提交到应用程序库时，**应用程序库请求**-收件人指定的电子邮件和用户将收到电子邮件通知。
-   **密件抄送**的收件人指定的电子邮件和用户会收到电子邮件密件副本的所有电子邮件发送给开发人员。
-   **新问题或批注**-收件人指定的电子邮件和用户会收到电子邮件通知，当新的问题或评论提交的开发人员门户。
-   在关闭帐户时，**关闭帐户邮件**的收件人指定的电子邮件，用户将收到电子邮件通知。
-   **Approaching 的订阅配额限制**-以下电子邮件收件人和用户将收到电子邮件通知，当接近使用率配额获取订阅使用情况。

对于每个事件中，您可以指定电子邮件收件人使用的电子邮件地址文本框或从列表中选择用户。

若要指定接收通知的电子邮件地址，请在电子邮件地址文本框中输入。 如果您有多个电子邮件地址，则将它们分开使用逗号。

![通知收件人][api-management-email-addresses]

若要指定被通知的用户，单击**添加收件人**，选中要通知的用户旁边的框，单击**确定**。

>请注意，只有管理员才显示在列表中。

配置后通知收件人，请单击**保存**将更新的通知收件人。

>如果离开**发布通知**选项卡发布门户警告如果那里未保存的更改。

## <a name="email-templates"></a>配置电子邮件模板

API 管理提供了进行管理和使用服务过程中发送的电子邮件的电子邮件模板。 提供了以下的电子邮件模板。

-   应用程序库提交批准
-   开发人员告别信
-   开发人员接近通知的配额限制
-   邀请用户
-   向问题添加新注释
-   接收到的新问题
-   激活新订阅
-   续订的订阅确认
-   订阅请求拒绝
-   接收到的订阅请求

可以修改这些模板所需。

查看和配置 API 管理实例的电子邮件模板，从**API 管理**菜单的左侧，单击**通知**，并选择**电子邮件模板**选项卡。

![电子邮件模板][api-management-email-templates]

若要查看或修改特定模板，**模板**下拉列表中选择它。

![电子邮件模板列表][api-management-email-templates-list]

每个电子邮件模板已经以纯文本的主题和正文定义 HTML 格式。 可以自定义每个项所需。

![电子邮件模板编辑器][api-management-email-template]

**参数**列表包含的参数的列表的主题或正文中插入，将发送电子邮件时替换为指定的值。 若要插入参数，将光标放在要使用参数来转，存放，单击参数名称左侧的箭头。

单击**预览**或**发送测试**以查看如何将查看电子邮件或将其发送测试电子邮件。

>注意参数与实际值在预览或发送一个测试时不会替换。
>
>若要将更改保存到电子邮件模板，请单击**保存**，或取消所做的更改单击**取消**。



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance