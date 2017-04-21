<properties 
    pageTitle="在 Azure API 管理策略 |Microsoft Azure" 
    description="了解如何创建、 编辑和配置 API 管理策略。" 
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


#<a name="policies-in-azure-api-management"></a>在 Azure API 管理策略

在 Azure API 管理中，策略是允许发布者更改通过配置 API 的行为系统具有强大的功能。 策略是在请求按顺序执行的语句的集合或一个 API 的响应。 常用语句包含 XML 格式转换为 JSON 并调用速率限制来限制来电从开发人员的数量。 许多更多的策略是提供开箱即用。

请参阅[策略引用][]的策略声明和其设置的完整列表。

内部网关位于 API 使用者和托管的 API 之间的应用策略。 网关接收所有请求，并通常将信息转发出去与基础 API 不变。 但是策略可以将更改应用到的请求入站和出站响应。

策略表达式可作为特性值或文本值放到任何 API 管理策略，除非该策略指定，否则。 某些策略，如[控制流][]和[将变量设置][]策略基于策略表达式。 有关详细信息，请参阅[高级策略][]和[策略表达式][]。

## <a name="scopes"></a>如何配置策略
全局范围内或在[产品][]、 [API][]或[操作][]的作用域，则可以配置策略。 要配置策略，请导航到策略编辑器中在发布服务器上的门户。

![策略菜单][policies-menu]

策略编辑器包含三个主要部分︰ 策略作用域 （上图）、 策略定义的策略进行编辑 （左） 和语句列表 （右图）︰

![策略编辑器][policies-editor]

要开始配置策略必须先选择该策略应该应用的范围。 在**初学者**下面的屏幕快照中选择产品。 请注意，策略名称旁边的方形符号表示已经这个级别应用策略。

![作用域][policies-scope]

由于已应用策略，配置所示定义视图。

![配置][policies-configure]

该策略将显示只读起初。 若要编辑定义，请单击**配置策略**操作。

![编辑][policies-edit]

策略定义为简单的 XML 文档描述的入站和出站的语句序列。 可以在定义窗口中直接编辑 XML。 向右提供的语句列表和语句适用于当前作用域已启用并且突出显示;在上面的抓图中**限制调用速率**语句所示。

单击启用的语句将定义视图中的光标位置处添加相应的 XML。 

>[AZURE.NOTE] 如果未启用您要添加的策略，请确保您处于正确的范围内为该策略。 每个策略语句用于某些范围和策略部分中。 要查看策略部分和策略的作用域，检查该策略[策略引用][]中**使用**部分。

策略语句的完整列表以及它们的设置位于[策略参考][]。

例如，若要添加新的语句将传入的请求限制为指定的 IP 地址，将光标放的内容中只是`inbound`的 XML 元素，然后单击**限制调用方 IPs**语句。

![限制策略][policies-restrict]

此操作将添加到 XML 代码段`inbound`指南提供有关如何配置语句的元素。

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

若要限制入站的请求并接受不仅仅来自 1.2.3.4 的 IP 地址，如下所示修改 XML:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![保存][policies-save]

当完成配置语句的策略，单击**保存**和所做的更改将立即传播到 API 管理关。

##<a name="sections"></a>了解策略配置

策略是一系列的请求和响应的顺序执行的语句。 配置相应地划分为`inbound`， `backend`， `outbound`，和`on-error`章节中配置如下所示。

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

如果在处理请求期间出现错误，在任何剩余步骤`inbound`， `backend`，或`outbound`节将被跳过并执行跳转到的语句`on-error`部分。 通过在策略语句放入`on-error`部分，您可以通过查看错误`context.LastError`属性，检查和自定义错误响应使用`set-body`策略，并配置出错时，会发生什么情况。 有内置的步骤和策略语句的处理过程中可能出现的错误的错误代码。 有关详细信息，请参阅[API 管理策略中的错误处理](https://msdn.microsoft.com/library/azure/mt629506.aspx)。

由于可以在不同级别 （全球，产品、 api 和操作） 指定策略配置提供了有助于您指定的策略定义语句相对于父策略执行的顺序。 

按以下顺序计算策略的作用域。

1. 全局作用域
2. 产品范围
3. API 范围
4. 操作作用域

其中的语句进行计算的位置根据`base`元素中，如果存在。

例如，如果您在全局级别和策略配置 API 有一个策略，然后使用该特定 API 时这两个策略将应用。 API 管理可以通过基本元素的组合的策略语句的确定性订购。 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

在上面的示例策略定义`cross-domain`语句将执行任何更高的策略，这将轮流，跟之前`find-and-replace`策略。

如果策略语句中两次出现相同的策略，将应用最近评估的策略。 可以使用此重写在较高的范围内定义的策略。 在策略编辑器中当前作用域中的策略，请单击**重新计算所选范围的有效策略**。

请注意，全球政策有没有父级的策略并使用`<base>`中它的元素不起作用。 

## <a name="next-steps"></a>下一步行动

签出以下策略表达式上的视频。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[策略引用]: api-management-policy-reference.md
[产品]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[操作]: api-management-howto-add-operations.md

[高级的策略]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[控制流]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[设置变量]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[策略表达式]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
