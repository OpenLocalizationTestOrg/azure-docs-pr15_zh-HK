<properties
    pageTitle="使用 Azure CDN CORS |Microsoft Azure"
    description="了解如何使用 Azure 内容传递网络 (CDN) 到与跨原始资源共享 (CORS)。"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>使用 Azure CDN CORS     

## <a name="what-is-cors"></a>CORS 是什么？

（跨原始资源共享） 的 CORS 是 HTTP 功能，可以将 web 应用程序运行在一个域访问另一个域中的资源。 为了减少跨站点脚本攻击的可能性，所有现代 web 浏览器实现称为[同源策略](http://www.w3.org/Security/wiki/Same_Origin_Policy)的安全限制。  这样，从另一个域中调用 Api 的网页。  CORS 提供一种安全的方法，以允许一个域 （原始域） 另一个域中调用的 Api。
 
## <a name="how-it-works"></a>它的工作原理
1.  浏览器发送选项请求用**原始**HTTP 标头。 此标头的值是在父页面提供服务的域。 当从 https://www.contoso.com 页试图访问 fabrikam.com 域中的用户数据时，以下请求标头将发送给 fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  服务器会用以下响应︰
    - 在其响应指示允许哪些来源站点**访问控件的允许的源**标题。 例如︰
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - 如果该服务器不允许跨来源请求错误页
    - 通配符允许所有域**访问控制-允许的源**标头︰
        
        `Access-Control-Allow-Origin: *`
 
对于复杂的 HTTP 请求，没有"印前检查"请求完成的第一个以确定其是否具有权限，然后发送整个请求。
 
## <a name="wildcard-or-single-origin-scenarios"></a>通配符或单一来源的方案

任何附加的配置，**访问控件的允许的源**标头设置为通配符 （*） 或单个来源时将自动不工作 CORS Azure CDN 上。  CDN 缓存第一次响应和后续请求将使用相同的标题。
 
如果请求已做出的 cdn 之前 CORS 设置在您的来源，您需要清除终结点内容重新加载与**访问控制-允许的源**标题内容上的内容。
 
## <a name="multiple-origin-scenarios"></a>多个源方案

如果您需要允许特定列表中的来源要允许 CORS 的事情就变稍微复杂一些。 CDN 缓存第一个 CORS 原点的**访问控件的允许的源**标题时，将出现此问题。  当不同的 CORS 起点后续请求，CDN 将提供缓存的**访问控制-允许的源**标题不匹配。  有几种方法来更正此问题。
 
### <a name="azure-cdn-premium-from-verizon"></a>从 Verizon 的 azure CDN 津贴

要启用此功能的最佳方法是使用**Verizon 从 Azure CDN 特优**，它提供了一些高级的功能。 
 
您需要创建[一条规则](cdn-rules-engine.md)来检查请求的**原始**页眉。  如果它是有效的原点，您的规则将请求中提供的原点设置**访问控制-允许的源**标题。  如果不允许**原始**标题中指定的来源，您的规则应忽略**访问控件的允许的源**标头，这会导致浏览器以拒绝该请求。 
 
有两种方法执行此操作的规则引擎。  在这两种情况下，从该文件的原始服务器的**访问控制-允许的源**标题完全被忽略，CDN 的规则引擎完全管理允许的 CORS 来源。

#### <a name="one-regular-expression-with-all-valid-origins"></a>所有有效的起源与一个正则表达式
 
在这种情况下，您将创建包含所有您想要允许起源的正则表达式︰ 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **从 Verizon 的 azure CDN**为其正则表达式引擎使用[Perl 兼容的正则表达式](http://pcre.org/)。  可以使用[正则表达式 101](https://regex101.com/)类工具来验证正则表达式。  请注意，"/"字符在正则表达式无效，并且不需要对其进行转义，但是，转义字符被认为是最佳做法，应由某些正则表达式验证程序。

如果正则表达式匹配，您的规则将替换**访问控件的允许的源**标头 （如果有的话） 从原点以发送请求的来源。  您还可以添加其他 CORS 头文件，如**访问控制-允许的方法**。

![使用正则表达式规则示例](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>每个来源的请求标头规则。

而不是正则表达式，您可以创建每个来源的您想要允许使用**请求标头通配符**[匹配条件](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)的单独规则。 如使用正则表达式方法，单独的规则引擎设置 CORS 头。 
  
![没有正则表达式规则示例](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] 在上面的示例，使用通配符 * 通知规则引擎以匹配 HTTP 和 HTTPS。
 
### <a name="azure-cdn-standard"></a>Azure CDN 标准

在 Azure CDN 标准配置文件，以便您无需使用通配符原点的多个来源的唯一机制是使用[查询字符串缓存](cdn-query-string.md)。  您需要启用查询字符串设置为 CDN 终结点，然后从允许的每个域的请求，使用唯一的查询字符串。 这样做将导致缓存单独为每个唯一的查询字符串对象的 CDN。 这种方法不是理想的但是，因为它会导致缓存在 CDN 上同一文件的多个副本。  

