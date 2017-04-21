<properties
    pageTitle="在应用程序逻辑中添加 Microsoft 在线翻译 |Microsoft Azure"
    description="使用 REST API 的参数的 Microsoft 转换器接头的概述"
    services=""
    suite=""
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

# <a name="get-started-with-the-microsoft-translator-connector"></a>开始使用 Microsoft 转换器接头
连接到 Microsoft 在线翻译翻译文本、 检测一种语言，等等。 使用 Microsoft 转换器，您可以︰ 

- 建立基于获得 Microsoft 在线翻译的数据业务流。 
- 使用动作翻译文本，检测一种语言，等等。 这些操作获得的响应，并使输出可用于其他操作。 例如，在收存箱中创建新文件时，可以翻译成其他语言使用 Microsoft 在线翻译文件中的文本。

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作
Microsoft 在线翻译包括以下操作。 没有触发器。

触发器 | 操作
--- | ---
无 | <ul><li>检测语言</li><li>文本到语音转换</li><li>翻译文本</li><li>获取语言</li><li>获取语音语言</li></ul>

所有连接器都支持 JSON 和 XML 格式的数据。


## <a name="create-a-connection-to-microsoft-translator"></a>创建一个连接到 Microsoft 在线翻译

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST API 参考
适用于版本︰ 1.0。

### <a name="detect-language"></a>检测语言    
检测到的源语言给定文本。  
```GET: /Detect```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|查询|字符串|是的|查询|无 |将识别的语言的文本|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="text-to-speech"></a>文本到语音转换    
将给定的文本转换成语音波形格式的音频流。  
```GET: /Speak```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|查询|字符串|是的|查询|无 |要转换文本|
|语言|字符串|是的|查询|无 |语言代码以生成语音 (示例: en-我们的)|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="translate-text"></a>翻译文本    
翻译为指定语言使用 Microsoft 在线翻译的文本。  
```GET: /Translate```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|查询|字符串|是的|查询|无 |要翻译的文本|
|languageTo|字符串|是的|查询| 无|目标语言代码 (示例: fr)|
|languageFrom|字符串|不|查询|无 |源代码的语言;如果未提供，则 Microsoft 在线翻译将尝试自动检测。 (示例︰ en)|
|类别|字符串|不|查询|常规 |翻译类别 (默认: 常规)|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-languages"></a>获取语言    
检索所有 Microsoft 转换器支持的语言。  
```GET: /TranslatableLanguages```

没有此调用的参数。 

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


### <a name="get-speech-languages"></a>获取语音语言    
检索可用语音合成的语言。  
```GET: /SpeakLanguages``` 

没有此调用的参数。

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|

## <a name="object-definitions"></a>对象定义

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>语言︰ Microsoft 在线翻译翻译语言的语言模型

|属性名称 | 数据类型 | 必填|
|---|---|---|
|代码|字符串|不|
|名称|字符串|不|


## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

返回到[列表中的 Api](apis-list.md)。


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
