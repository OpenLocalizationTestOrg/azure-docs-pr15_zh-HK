<properties
pageTitle="MailChimp |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 MailChimp 是一种 SaaS 服务，使公司能够管理并自动执行电子邮件市场营销活动，包括发送市场营销邮件、 自动的消息和目标市场活动。"
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>开始使用 MailChimp 连接器

MailChimp 是一种 SaaS 服务，使公司能够管理并自动执行电子邮件市场营销活动，包括发送市场营销邮件、 自动的消息和目标市场活动。


>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

MailChimp 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。

 MailChimp 连接器具有以下操作和/或可用触发器︰

### <a name="mailchimp-actions"></a>MailChimp 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|创建新市场活动基于市场活动类型时，收件人列表和市场活动设置 （主题行、 标题、 from_name 和 reply_to）|
|[newlist](connectors-create-api-mailchimp.md#newlist)|您的 MailChimp 帐户中创建新的列表|
|[addmember](connectors-create-api-mailchimp.md#addmember)|添加或更新列表成员|
|[removemember](connectors-create-api-mailchimp.md#removemember)|从列表中删除成员。|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|更新特定列表成员的信息|
### <a name="mailchimp-triggers"></a>MailChimp 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|当将成员添加到列表|已将新成员添加到列表时，将触发工作流|
|当创建新的列表|创建新列表时，将触发工作流|


## <a name="create-a-connection-to-mailchimp"></a>创建到 MailChimp 的连接
要使用 MailChimp 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 MailChimp 的凭据|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-mailchimp"></a>MailChimp 的引用
适用于版本︰ 1.0

## <a name="newcampaign"></a>newcampaign
新市场︰ 创建新市场活动基于市场活动类型时，收件人列表和市场活动设置 （主题行、 标题、 from_name 和 reply_to）

```POST: /campaigns```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|newCampaignRequest| |是的|正文|无|要用新的市场活动请求参数正文中发送的 Json 对象|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="newlist"></a>newlist
新列表︰ 您的 MailChimp 帐户中创建新的列表

```POST: /lists```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|newListRequest| |是的|正文|无|要用新的市场活动请求参数正文中发送的 Json 对象|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="addmember"></a>addmember
向列表中添加成员︰ 添加或更新列表成员

```POST: /lists/{list_id}/members```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|字符串|是的|路径|无|唯一的 id 列表|
|newMemberInList| |是的|正文|无|要用新的成员信息正文中发送的 Json 对象|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="removemember"></a>removemember
从列表中移除成员︰ 从列表中删除成员。

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|字符串|是的|路径|无|唯一的 id 列表|
|member_email|字符串|是的|路径|无|要删除的成员的电子邮件地址|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="updatemember"></a>updatemember
更新成员信息︰ 更新特定列表成员的信息

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|字符串|是的|路径|无|唯一的 id 列表|
|member_email|字符串|是的|路径|无|成员更新的唯一的电子邮件地址|
|updateMemberInListRequest| |是的|正文|无|要用更新的成员信息正文中发送的 Json 对象|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
当将成员添加到列表︰ 已将新成员添加到列表时，将触发工作流

```GET: /trigger/lists/{list_id}/members```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|字符串|是的|路径|无|唯一的 id 列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="oncreatelist"></a>OnCreateList
当创建新的列表︰ 创建新列表时，将触发工作流

```GET: /trigger/lists```

此调用没有参数
#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义

### <a name="newcampaignrequest"></a>NewCampaignRequest


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|类型|字符串|是的 |
|收件人|未定义|是的 |
|设置|未定义|是的 |
|variate_settings|未定义|不 |
|跟踪|未定义|不 |
|rss_opts|未定义|不 |
|social_card|未定义|不 |



### <a name="recipient"></a>收件人


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|字符串|是的 |
|segment_opts|未定义|不 |



### <a name="settings"></a>设置


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|subject_line|字符串|是的 |
|标题|字符串|不 |
|from_name|字符串|是的 |
|reply_to|字符串|是的 |
|use_conversation|布尔值|不 |
|to_name|字符串|不 |
|folder_id|整数|不 |
|进行身份验证|布尔值|不 |
|auto_footer|布尔值|不 |
|inline_css|布尔值|不 |
|auto_tweet|布尔值|不 |
|auto_fb_post|数组|不 |
|fb_comments|布尔值|不 |



### <a name="variatesettings"></a>Variate_Settings


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|winner_criteria|字符串|不 |
|wait_time|整数|不 |
|test_size|整数|不 |
|subject_lines|数组|不 |
|send_times|数组|不 |
|from_names|数组|不 |
|reply_to_addresses|数组|不 |



### <a name="tracking"></a>跟踪


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|将打开|布尔值|不 |
|html_clicks|布尔值|不 |
|text_clicks|布尔值|不 |
|goal_tracking|布尔值|不 |
|ecomm360|布尔值|不 |
|google_analytics|字符串|不 |
|clicktale|字符串|不 |
|销售队伍|未定义|不 |
|highrise|未定义|不 |
|马蹄形|未定义|不 |



### <a name="rssopts"></a>RSS_Opts


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|feed_url|字符串|不 |
|频率|字符串|不 |
|constrain_rss_img|字符串|不 |
|日程安排|未定义|不 |



### <a name="socialcard"></a>Social_Card


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|image_url|字符串|不 |
|说明|字符串|不 |
|标题|字符串|不 |



### <a name="segmentopts"></a>Segment_Opts


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|saved_segment_id|整数|不 |
|匹配项|字符串|不 |



### <a name="salesforce"></a>销售队伍


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|市场活动|布尔值|不 |
|备注|布尔值|不 |



### <a name="highrise"></a>Highrise


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|市场活动|布尔值|不 |
|备注|布尔值|不 |



### <a name="capsule"></a>马蹄形


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|备注|布尔值|不 |



### <a name="schedule"></a>日程安排


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|小时|整数|不 |
|daily_send|未定义|不 |
|weekly_send_day|字符串|不 |
|monthly_send_date|编号|不 |



### <a name="dailysend"></a>Daily_Send


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|星期日|布尔值|不 |
|星期一|布尔值|不 |
|（星期二)|布尔值|不 |
|（星期三)|布尔值|不 |
|（星期四)|布尔值|不 |
|星期五|布尔值|不 |
|星期六|布尔值|不 |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|类型|字符串|不 |
|create_time|字符串|不 |
|archive_url|字符串|不 |
|状态|字符串|不 |
|emails_sent|整数|不 |
|send_time|字符串|不 |
|content_type|字符串|不 |
|收件人|数组|不 |
|设置|未定义|不 |
|variate_settings|未定义|不 |
|跟踪|未定义|不 |
|rss_opts|未定义|不 |
|ab_split_opts|未定义|不 |
|social_card|未定义|不 |
|report_summary|未定义|不 |
|delivery_status|未定义|不 |
|_links|数组|不 |



### <a name="absplitopts"></a>AB_Split_Opts


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|split_test|字符串|不 |
|pick_winner|字符串|不 |
|wait_units|字符串|不 |
|wait_time|整数|不 |
|split_size|整数|不 |
|from_name_a|字符串|不 |
|from_name_b|字符串|不 |
|reply_email_a|字符串|不 |
|reply_email_b|字符串|不 |
|subject_a|字符串|不 |
|subject_b|字符串|不 |
|send_time_a|字符串|不 |
|send_time_b|字符串|不 |
|send_time_winner|字符串|不 |



### <a name="reportsummary"></a>Report_Summary


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|将打开|整数|不 |
|unique_opens|整数|不 |
|open_rate|编号|不 |
|单击|整数|不 |
|subscriber_clicks|编号|不 |
|click_rate|编号|不 |



### <a name="deliverystatus"></a>Delivery_Status


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|启用|布尔值|不 |
|can_cancel|布尔值|不 |
|状态|字符串|不 |
|emails_sent|整数|不 |
|emails_canceled|整数|不 |



### <a name="link"></a>链接


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|rel|字符串|不 |
|href|字符串|不 |
|方法|字符串|不 |
|targetSchema|字符串|不 |
|架构|字符串|不 |



### <a name="newlistrequest"></a>NewListRequest


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|是的 |
|联系人|未定义|是的 |
|permission_reminder|字符串|是的 |
|use_archive_bar|布尔值|不 |
|campaign_defaults|未定义|是的 |
|notify_on_subscribe|字符串|不 |
|notify_on_unsubscribe|字符串|不 |
|email_type_option|布尔值|是的 |
|可见性|字符串|不 |



### <a name="contact"></a>联系人


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|公司|字符串|是的 |
|地址 1|字符串|是的 |
|地址 2|字符串|不 |
|市/县|字符串|是的 |
|状态|字符串|是的 |
|压缩|字符串|是的 |
|国家/地区|字符串|是的 |
|电话|字符串|是的 |



### <a name="campaigndefaults"></a>Campaign_Defaults


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|from_name|字符串|是的 |
|from_email|字符串|是的 |
|主题|字符串|不 |
|语言|字符串|是的 |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|是的 |
|名称|字符串|是的 |
|联系人|未定义|是的 |
|permission_reminder|字符串|是的 |
|use_archive_bar|布尔值|不 |
|campaign_defaults|未定义|是的 |
|notify_on_subscribe|字符串|不 |
|notify_on_unsubscribe|字符串|不 |
|date_created|字符串|不 |
|list_rating|整数|不 |
|email_type_option|布尔值|是的 |
|subscribe_url_short|字符串|不 |
|subscribe_url_long|字符串|不 |
|beamer_address|字符串|不 |
|可见性|字符串|不 |
|模块|数组|不 |
|统计数据|未定义|不 |
|_links|数组|不 |



### <a name="stats"></a>统计数据


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|member_count|整数|不 |
|unsubscribe_count|整数|不 |
|cleaned_count|整数|不 |
|member_count_since_send|整数|不 |
|unsubscribe_count_since_send|整数|不 |
|cleaned_count_since_send|整数|不 |
|campaign_count|整数|不 |
|campaign_last_sent|整数|不 |
|merge_field_count|整数|不 |
|avg_sub_rate|编号|不 |
|avg_unsub_rate|编号|不 |
|target_sub_rate|编号|不 |
|open_rate|编号|不 |
|click_rate|编号|不 |
|last_sub_date|字符串|不 |
|last_unsub_date|字符串|不 |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|列表|数组|不 |
|total_items|整数|不 |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|email_type|字符串|不 |
|状态|字符串|是的 |
|merge_fields|未定义|不 |
|兴趣爱好|字符串|不 |
|语言|字符串|不 |
|vip|布尔值|不 |
|位置|未定义|不 |
|电子邮件地址|字符串|是的 |



### <a name="firstandlastname"></a>FirstAndLastName


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|FNAME|字符串|不 |
|LNAME|字符串|不 |



### <a name="location"></a>位置


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|纬度|编号|不 |
|经度|编号|不 |



### <a name="memberresponsemodel"></a>MemberResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|电子邮件地址|字符串|不 |
|unique_email_id|字符串|不 |
|email_type|字符串|不 |
|状态|字符串|不 |
|merge_fields|未定义|不 |
|兴趣爱好|字符串|不 |
|统计数据|未定义|不 |
|ip_signup|字符串|不 |
|timestamp_signup|字符串|不 |
|ip_opt|字符串|不 |
|timestamp_opt|字符串|不 |
|member_rating|整数|不 |
|last_changed|字符串|不 |
|语言|字符串|不 |
|vip|布尔值|不 |
|email_client|字符串|不 |
|位置|未定义|不 |
|last_note|未定义|不 |
|list_id|字符串|不 |
|_links|数组|不 |



### <a name="lastnote"></a>Last_Note


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|note_id|整数|不 |
|created_at|字符串|不 |
|created_by|字符串|不 |
|请注意|字符串|不 |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|成员|数组|不 |
|list_id|字符串|不 |
|total_items|整数|不 |



### <a name="object"></a>对象






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|电子邮件地址|字符串|不 |
|email_type|字符串|不 |
|状态|字符串|是的 |
|merge_fields|未定义|不 |
|兴趣爱好|字符串|不 |
|语言|字符串|不 |
|vip|布尔值|不 |
|位置|未定义|不 |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|成员|数组|不 |
|list_id|字符串|不 |
|total_items|整数|不 |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|是的 |
|电子邮件地址|字符串|是的 |
|unique_email_id|字符串|不 |
|email_type|字符串|不 |
|状态|字符串|不 |
|merge_fields|未定义|是的 |
|兴趣爱好|字符串|不 |
|统计数据|未定义|不 |
|ip_signup|字符串|不 |
|timestamp_signup|字符串|不 |
|ip_opt|字符串|不 |
|timestamp_opt|字符串|不 |
|member_rating|整数|不 |
|last_changed|字符串|不 |
|语言|字符串|不 |
|vip|布尔值|不 |
|email_client|字符串|不 |
|位置|未定义|不 |
|last_note|未定义|不 |
|list_id|字符串|不 |
|_links|数组|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)
