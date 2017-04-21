<properties 
    pageTitle="如何委派用户注册和产品预订" 
    description="了解如何将委托给第三方在 Azure API 管理用户注册和产品订阅。" 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>如何委派用户注册和产品预订

委托可用于处理开发人员签名的中/sign-高和订阅的产品而不是使用内置的功能，开发人员门户中使用您现有的网站。 这使您的网站拥有用户数据和自定义的方式执行这些步骤的验证。

## <a name="delegate-signin-up"></a>委派开发人员登录和注册

委托开发人员登录和注册到您现有的网站，您需要在您的站点，它就像从 API 管理开发人员门户网站发起的任何此类请求的入口点上创建特殊委托终结点。

最后的工作流将如下所示︰

1. 开发人员单击位于 API 管理开发人员门户的登录或注册链接
2. 浏览器被重定向到委托终结点
3. 委托终结点返回重定向到或者显示要求用户登录或注册的用户界面
4. 成功，用户将被重定向回开始从他们的 API 管理开发人员门户页


首先，让我们第一个设置 API 管理路由请求通过委托终结点。 在发布服务器 API 管理门户中，在**安全**上单击，然后单击**委派**选项卡。 单击复选框以启用签入和注册代理。

![委派页][api-management-delegation-signin-up]

* 决定什么特殊委托终结点的 URL 并**委派端点 URL**字段中输入它。 

* **委派身份验证密钥**字段中输入用于计算签名对您进行验证，以确保请求确实来自 Azure API 管理提供一个秘密。 您可以单击**生成**按钮具有 API Managemnet 为您随机生成一个密钥。

现在，您需要创建**委托终结点**。 它具有要执行的操作的数量︰

1. 收到请求采用以下形式︰

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {源页的 URL} 和盐 = {字符串} & sig = {字符串}*

    登录 / 注册情况的查询参数︰
    - **操作**︰ 确定哪种类型的委派请求它-它在这种情况下只能是**登录**
    - **returnUrl**︰ 用户登录或注册的链接单击的位置的页面的 URL
    - **盐**︰ 特殊盐字符串用于计算安全哈希
    - **签名**︰ 使用与自己进行比较计算的安全哈希计算哈希

2. 请验证该请求来自 Azure API 管理 （可选，但强烈建议执行的安全）

    * 计算 HMAC SHA512 哈希，基于**returnUrl**和**盐**的查询参数 （[下面提供的示例代码]） 的字符串︰
        > HMAC （**salt** + \n + **returnUrl**）
         
    * 将上述计算的哈希与**sig**查询参数的值进行比较。 如果两个哈希值匹配，则转到下一步，否则拒绝该请求。

2. 验证接收到的注册/登录上的请求︰**操作**查询参数将设置为"**登录**"。

3. 向用户显示用户界面，以登录或注册

4. 如果用户是签名上必须在 API 管理中为其创建一个对应的帐户。 与 API 管理 REST API[创建用户]。 当执行此操作，请确保将用户 ID 设置为相同的处于用户存储或一个 ID，您可以跟踪。

5. 当用户成功通过身份验证︰

    * 通过 API 管理 REST API[请求单点登录 (SSO) 标记]

    * 将 returnUrl 查询参数追加到您收到来自上面的 API 调用 SSO URL:
        > 例如 https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * 将用户重定向到上面生成的 URL

除了**登录**操作，您还可以通过执行前面的步骤并使用下列操作之一执行帐户管理。

-   **更改密码**
-   **ChangeProfile**
-   **CloseAccount**

您必须通过以下查询参数进行帐户管理操作。

-   **操作**︰ 确定哪种类型的委派请求 （更改密码、 ChangeProfile，或 CloseAccount） 是
-   **用户 Id**︰ 管理帐户的用户 id
-   **盐**︰ 特殊盐字符串用于计算安全哈希
-   **签名**︰ 使用与自己进行比较计算的安全哈希计算哈希

## <a name="delegate-product-subscription"></a>委派产品订购

委派产品订购工作方式类似于委派用户登录/幅。 最后的工作流将如下所示︰

1. 开发人员 API 管理开发人员门户中选择产品，并单击订阅按钮
2. 浏览器被重定向到委托终结点
3. 终结点委托执行所需的产品预订步骤-这取决于您，并且可能会重定向到另一个页请求提出附加的问题，或只存储的信息并不需要用户执行任何操作的付费信息


若要启用的功能，在**委派**页上单击**代理产品订购**。

然后，确保该委托终结点执行下列操作︰


1. 收到请求采用以下形式︰

    > *http://www.yourwebsite.com/apimdelegation?operation= {操作} & 产品 Id = {产品订阅} & 用户 Id = {发出请求的用户} 和盐 = {字符串} & sig = {字符串}*

    产品预订情况的查询参数︰
    - **操作**︰ 确定它是哪种类型的委派请求。 为产品的订阅请求有效的选项是︰
        - "订阅": 请求订阅与特定产品的用户提供标识 （参见下文）
        - "取消订阅": 取消从产品用户的请求
        - "续订": requst 续订的订阅 （例如可能即将过期）
    - **产品 id**︰ 用户请求订阅的产品 ID
    - **用户 Id**︰ 其发出请求的用户的 ID
    - **盐**︰ 特殊盐字符串用于计算安全哈希
    - **签名**︰ 使用与自己进行比较计算的安全哈希计算哈希


2. 请验证该请求来自 Azure API 管理 （可选，但强烈建议执行的安全）

    * 计算 HMAC SHA512 根据**产品 Id**、**用户 Id**和**盐**的查询参数的字符串︰
        > HMAC （**salt** + \n +**产品 id** + \n +**用户 Id**）
         
    * 将上述计算的哈希与**sig**查询参数的值进行比较。 如果两个哈希值匹配，则转到下一步，否则拒绝该请求。
    
3. 执行基于请求**操作**-例如开帐单、 等其他问题中的操作的任何的类型产品预订处理。

4. 成功预订到在您这一边的产品用户，在 API 管理产品用户订阅通过[调用 REST API，为产品的订阅]。

## <a name="delegate-example-code"></a>的示例代码 ##

这些代码示例显示如何执行*委派验证密钥*，它在发布者门户，委派屏幕中的设置创建 HMAC 然后用来验证签名，证明传递 returnUrl 的有效性。 相同的代码适合稍作修改，与用户 Id 与产品 id。

**C# 代码以生成哈希的 returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**用于生成哈希的 returnUrl 的 NodeJS 代码**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>下一步行动

委派的详细信息，请参见下面的视频。

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[单点登录 (SSO) 令牌的请求]: http://go.microsoft.com/fwlink/?LinkId=507409
[创建用户]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[REST API，呼吁产品订购]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[下面提供的示例代码]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 