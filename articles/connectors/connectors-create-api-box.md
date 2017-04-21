<properties
    pageTitle="将框连接器添加到您的逻辑应用程序 |Microsoft Azure"
    description="使用 REST API 的参数框中连接器的概述"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>入门框连接器
连接到框中并创建文件、 删除文件等。 

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。

使用框中，您可以︰

- 构建业务流从框中获取的数据。 
- 创建或更新文件时，请使用触发器。
- 使用复制文件、 删除文件等操作。 这些操作获得的响应，并使输出可用于其他操作。 例如，在中更改文件时，可以将该文件发送并通过电子邮件发送它使用 Office 365。

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作
框中包括下面的触发器和操作。

| 触发器 | 操作|
| --- | --- |
|<ul><li>当创建一个文件</li><li>文件被修改时</li></ul> | <ul><li>创建文件</li><li>当创建一个文件</li><li>复制文件</li><li>删除文件</li><li>提取存档文件夹</li><li>获取使用 id 文件内容</li><li>获取使用路径的文件内容</li><li>获取使用 id 文件元数据</li><li>获取使用路径的文件元数据</li><li>更新文件</li><li>文件被修改时</li></ul>

所有连接器都支持 JSON 和 XML 格式的数据。

## <a name="create-a-connection-to-box"></a>创建框连接
当此连接器添加到您的逻辑应用程序时，您必须授权逻辑应用程序连接到您的机器。

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

在创建连接后，输入框属性。 本主题中的**其余部分 API 参考**描述了这些属性。

>[AZURE.TIP] 在其他逻辑应用程序中，您可以使用此同一框连接。

## <a name="swagger-rest-api-reference"></a>Swagger REST API 参考
适用于版本︰ 1.0。

### <a name="create-file"></a>创建文件
将文件上载到框中。  
```POST: /datasets/default/files```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|采用文件夹路径|字符串|是的|查询|无 |要将文件上载到框中的文件夹路径|
|名称|字符串|是的|查询|无 |若要在中创建的文件的名称|
|正文|string(binary) |是的|正文|无 |该文件将上载到框中的内容|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="when-a-file-is-created"></a>当创建一个文件
在框中的文件夹中创建一个新文件时，将触发流程。  
```GET: /datasets/default/triggers/onnewfile```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|文件夹 Id|字符串|是的|查询|无 |框中的文件夹的唯一标识符|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="copy-file"></a>复制文件
将文件复制到框中。  
```POST: /datasets/default/copyFile```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|来源|字符串|是的|查询|无 |源代码文件的 Url|
|目标|字符串|是的|查询| 无|目标文件路径框中，包括目标文件名|
|重写|布尔值|不|查询| 无|如果覆盖目标文件设置为 true|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="delete-file"></a>删除文件
从框中删除一个文件。  
```DELETE: /datasets/default/files/{id}```


| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无 |要从框中删除的文件的唯一标识符|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="extract-archive-to-folder"></a>提取存档文件夹
归档文件提取到一个文件夹中 (示例︰.zip)。  
```POST: /datasets/default/extractFolderV2```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|来源|字符串|是的|查询| |存档文件路径|
|目标|字符串|是的|查询| |提取存档内容框中的路径|
|重写|布尔值|不|查询| |如果覆盖目标文件设置为 true|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-file-content-using-id"></a>获取使用 id 文件内容
从框中使用 id 检索文件的内容。  
```GET: /datasets/default/files/{id}/content```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无 |框中的文件的唯一标识符|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-file-content-using-path"></a>获取使用路径的文件内容
从框中使用路径中检索文件的内容。  
```GET: /datasets/default/GetFileContentByPath```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|路径|字符串|是的|查询|无 |唯一路径框中的文件|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-file-metadata-using-id"></a>获取使用 id 文件元数据
从框中使用文件 id 检索文件元数据。  
```GET: /datasets/default/files/{id}```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径| 无|框中的文件的唯一标识符|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-file-metadata-using-path"></a>获取使用路径的文件元数据
从框中使用路径中检索文件元数据。  
```GET: /datasets/default/GetFileByPath```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|路径|字符串|是的|查询|无 |唯一路径框中的文件|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="update-file"></a>更新文件
更新中的文件。  
```PUT: /datasets/default/files/{id}```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径| 无|要更新中的文件的唯一标识符|
|正文|string(binary) |是的|正文|无 |要更新中的文件的内容|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="when-a-file-is-modified"></a>文件被修改时
在框中的文件夹修改文件时触发流程。  
```GET: /datasets/default/triggers/onupdatedfile```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|文件夹 Id|字符串|是的|查询|无 |框中的文件夹的唯一标识符|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|属性名称 | 数据类型 | 必填|
|---|---|---|
|表格格式|未定义|不|
|blob|未定义|不|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|属性名称 | 数据类型 |必填|
|---|---|---|
|来源|字符串|不|
|显示名称|字符串|不|
|urlEncoding|字符串|不|
|tableDisplayName|字符串|不|
|tablePluralName|字符串|不|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|属性名称 | 数据类型 |必填|
|---|---|---|
|来源|字符串|不|
|显示名称|字符串|不|
|urlEncoding|字符串|不|

#### <a name="blobmetadata"></a>BlobMetadata

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|名称|字符串|不|
|显示名称|字符串|不|
|路径|字符串|不|
|上次更改时间|字符串|不|
|大小|整数|不|
|媒体类型|字符串|不|
|IsFolder|布尔值|不|
|ETag|字符串|不|
|FileLocator|字符串|不|

## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。
