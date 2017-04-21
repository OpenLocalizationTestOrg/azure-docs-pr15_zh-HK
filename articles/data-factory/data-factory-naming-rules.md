<properties 
    pageTitle="数据工厂的命名规则 |Microsoft Azure" 
    description="描述数据工厂实体的命名规则。" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure 数据工厂的命名规则 
下表提供了对数据工厂项目的命名规则。



名称 | 名称的唯一性 | 验证检查
:--- | :-------------- | :----------------
数据工厂 | 在 Microsoft Azure 唯一。 名称不区分大小写，即 MyDF 和 mydf 是指相同的数据工厂。 |<ul><li>每个数据工厂被依赖于一个 Azure 的订阅。</li><li>对象名称必须以字母或数字开头，可以包含字母、 数字和短划线 （-） 字符。</li><li>每个短划线 （-） 字符必须立即前面和后面可以按字母或数字。 容器名称中不允许连续的短划线。</li><li>名称可以是 3-63 个字符。</li></ul>
链接的服务/表/管线 | 与数据工厂中的唯一。 名称不区分大小写。 | <ul><li>表名称中的字符的最大数量︰ 260。</li><li>对象名称必须以字母数字或下划线 (_) 开头。</li><li>不允许以下字符:"。"，"+"、"?""/"、"<"，">"，"*"，"%"、"&"":"，"\\"</li></ul>
资源组 | 在 Microsoft Azure 唯一。 名称不区分大小写。 | <ul><li>最大字符数︰ 1000年。</li><li>名称可以包含字母、 数字和下列字符:"-"，"_"、""和"。"。</li></ul>