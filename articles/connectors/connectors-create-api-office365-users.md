<properties
    pageTitle="在逻辑的应用程序中添加 Office 365 用户接口 |Microsoft Azure"
    description="Office 365 用户连接器使用 REST API 的参数的概述"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>开始使用 Office 365 用户连接器

连接到 Office 365 用户获取配置文件、 搜索用户和更多。 

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。

使用 Office 365 的用户，您可以︰

- 建立基于获得 Office 365 用户的数据业务流。 
- 获得直接报告的使用操作获取管理器的用户配置文件，等等。 这些操作获得的响应，并使输出可用于其他操作。 例如，获取人的直接下属，然后利用此信息并更新 SQL Azure 数据库。 

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

Office 365 用户连接器具有以下操作可用。 没有触发器。

| 触发器 | 操作|
| --- | --- |
|无 | <ul><li>获取管理器</li><li>获取我的个人资料</li><li>获取直接报告</li><li>获取用户配置文件</li><li>搜索用户</li></ul>|

所有连接器都支持 JSON 和 XML 格式的数据。 


## <a name="create-a-connection-to-office-365-users"></a>创建 Office 365 用户的连接

当此连接器添加到您的逻辑应用程序时，您必须登录到您的 Office 365 用户帐户并允许逻辑应用程序连接到您的帐户。

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

在创建连接后，您输入的 Office 365 用户属性，如用户 id。 本主题中的**其余部分 API 参考**描述了这些属性。

>[AZURE.TIP] 在其他逻辑应用程序中，您可以使用此相同的 Office 365 用户连接。


## <a name="office-365-users-rest-api-reference"></a>Office 365 REST API，用户参考
适用于版本︰ 1.0。

### <a name="get-my-profile"></a>获取我的个人资料 
检索当前用户的配置文件。  
```GET: /users/me``` 

没有此调用的参数。

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="get-user-profile"></a>获取用户配置文件 
检索一个特定的用户配置文件。  
```GET: /users/{userId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|用户 Id|字符串|是的|路径|无|用户主体名称或电子邮件 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="get-manager"></a>获取管理器 
检索用户配置文件管理器的指定用户。  
```GET: /users/{userId}/manager``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|用户 Id|字符串|是的|路径|无|用户主体名称或电子邮件 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|



### <a name="get-direct-reports"></a>获取直接报告 
获取直接报告。  
```GET: /users/{userId}/directReports``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|用户 Id|字符串|是的|路径|无|用户主体名称或电子邮件 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|



### <a name="search-for-users"></a>搜索用户 
检索搜索结果的用户配置文件。  
```GET: /users``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|术语|字符串|不|查询|无|搜索字符串 (适用于︰ 显示名称、 名字、 姓氏、 邮件、 邮件昵称和用户主体名称)|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|



## <a name="object-definitions"></a>对象定义

#### <a name="user-user-model-class"></a>用户︰ 用户模型类别

|属性名称 | 数据类型 |必填
|---|---|---|
|显示名称|字符串|不|
|GivenName|字符串|不|
|姓氏|字符串|不|
|邮件|字符串|不|
|MailNickname|字符串|不|
|TelephoneNumber|字符串|不|
|AccountEnabled|布尔值|不|
|标识|字符串|是的
|范围内|字符串|不|
|部门|字符串|不|
|职务|字符串|不|
|mobilePhone|字符串|不|


## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

返回到[列表中的 Api](apis-list.md)。

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
