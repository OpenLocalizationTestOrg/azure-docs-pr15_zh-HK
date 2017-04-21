<properties
    pageTitle="入门的 azure AD iOS |Microsoft Azure"
    description="如何构建的 iOS 应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>将 Azure AD 集成到 iOS 应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD 提供活动目录身份验证库或 ADAL，iOS 客户端需要访问受保护的资源的。  在现实生活中的 ADAL 的唯一目的是为了方便您的应用程序才能访问令牌。  以展示它是多么简单，此处我们将构建一个目标 C 待办事项列表的应用程序的︰

-   获取访问令牌用于调用 Azure 广告图形 API 使用[OAuth 2.0 身份验证协议](https://msdn.microsoft.com/library/azure/dn645545.aspx)。
-   搜索具有指定别名的用户目录。

若要生成完整的工作应用程序，您需要︰

2. 向应用程序注册 Azure 的广告。
3. 安装和配置 ADAL。
5. 使用 ADAL 从 Azure AD 获取令牌。

若要开始，[下载应用程序骨架](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip)或[下载已完成的示例](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)。  您还将需要 Azure AD 租户可以在其中创建用户和注册应用程序。  如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

> [AZURE.TIP] 请尝试我们新[开发人员门户](https://identity.microsoft.com/Docs/iOS)可帮助您在几分钟内使用 Azure 活动目录获取启动并运行预览 ！  开发人员门户将引导您完成注册应用程序并将 Azure AD 集成到您的代码的过程。  完成后，您可以在一个简单的应用程序可以验证您租户和后端中的用户能够接受的标记和执行验证。 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1.确定您重定向 URI 将 iOS 的是什么*

为了安全地启动应用程序中某些 SSO 方案我们要求您以特定格式创建**重定向 URI** 。 重定向 URI 用于确保令牌返回到正确的应用程序，它们要求。

重定向 URI 的 iOS 格式是︰

```
<app-scheme>://<bundle-id>
```

-   **aap 方案**-这 XCode 项目中注册。 它是其他应用程序可以呼叫您。 您可以查找在 Info.plist-> URL 类型-> URL 标识符。 如果没有一个或多个配置，则应创建一个。
-   **捆绑包 id** -这是在"标识"下找到包标识符联合国中 XCode 项目的设置。

此快速入门代码的一个例子是︰ ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2.注册的 DirectorySearcher 应用程序*
若要使您的应用程序获取令牌，您首先需要将其注册在 Azure AD 租户，并授予它 Azure 广告图形 API 的访问权︰

-   登录到 Azure 的管理门户
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   请按照提示进行操作，创建新的**本机客户端应用程序**。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   **重定向 Uri**是方案和字符串组合 Azure 广告将用于返回标记的响应。  基于上述信息的应用程序输入一个特定的值。
-   完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要此值在下一节中，所以将其复制从**配置**选项卡。
- 此外在**配置**选项卡上，查找"权限与其他应用程序"部分。  "Azure Active Directory"应用程序中，将添加**访问您组织的目录**权限**委派权限**下。  这将使您的应用程序用户在图形 API 中查询。

## <a name="3-install--configure-adal"></a>*3.安装和配置 ADAL*
现在，您已经在 Azure 广告应用程序，可以安装 ADAL 和编写标识相关代码。  为了使 ADAL 能与 Azure AD 间的通信，您需要为其提供一些有关您的应用程序的注册信息。
-   通过将 ADAL 添加到使用 Cocapods 的 DirectorySearcher 项目的开始。

```
$ vi Podfile
```
将以下内容添加到此 podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

现在加载使用 cocoapods podfile。 这将创建新 XCode 工作区会加载。

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   在快速启动项目中，打开 plist 文件`settings.plist`。  替换为要反映到 Azure 门户输入的值的节中的元素的值。  它使用 ADAL 时，您的代码将引用这些值。
    -   `tenant`是您 Azure AD 租户，例如 contoso.onmicrosoft.com 的域
    -   `clientId`是来自门户应用程序的客户机 Id。
    -   `redirectUri`是重定向 url 在门户中注册。

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4.使用 ADAL 来获得 AAD 的标记*
ADAL 背后的基本原则是，每当您的应用程序需要一个访问令牌，它只需调用 completionBlock `+(void) getToken : `，和 ADAL 将完成其余工作。  

-   在`QuickStart`项目中打开`GraphAPICaller.m`并找到`// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.`靠近顶端的注释。  这是您在其中传递 ADAL 通过 CompletionBlock Azure 广告与通信并告诉它如何缓存标记的坐标。

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- 现在，我们需要使用该标记来搜索用户的关系图中。 发现`// TODO: implement SearchUsersList`commentThis 方法使到 Azure 广告图形 API 对查询对其 UPN 开头给定的搜索条件的用户的 GET 请求。  但为了查询图形 API，您需要包括在 access_token`Authorization`标头的请求-这是 ADAL 的进入。

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                     // We can grab the top most JSON node to get our graph data.
                     NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                     // Don't be thrown off by the key name being "value". It really is the name of the
                     // first node. :-)

                     //each object is a key value pair
                     NSDictionary *keyValuePairs;
                     NSMutableArray* Users = [[NSMutableArray alloc]init];

                     for(int i =0; i < graphDataArray.count; i++)
                     {
                         keyValuePairs = [graphDataArray objectAtIndex:i];

                         User *s = [[User alloc]init];
                         s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                         s.name =[keyValuePairs valueForKey:@"givenName"];

                         [Users addObject:s];
                     }

                     completionBlock(Users, nil);
                 }
                 else
                 {
                     completionBlock(nil, error);
                 }

             }];
         }
     }];

}

```
- 当您的应用程序通过调用请求令牌`getToken(...)`，ADAL 会返回一个标记而不要求用户提供凭据。  如果 ADAL 确定用户需要登录以获取令牌，它将显示登录对话框，收集用户的凭据，并返回在成功的身份验证令牌。  如果 ADAL 不能出于任何原因返回标记，则将引发`AdalException`。
- 请注意，`AuthenticationResult`对象包含`tokenCacheStoreItem`可以用来收集您的应用程序可能需要的信息的对象。  在快速入门，`tokenCacheStoreItem`用来确定 authenitcation 是否已发生。


## <a name="step-5-build-and-run-the-application"></a>第 5 步︰ 生成并运行应用程序



祝贺您 ！ 现在有有效的 iOS 应用程序的已验证用户身份，安全地调用 Web Api 使用 OAuth 2.0 的功能，并获取有关用户的基本信息。  如果还没有的话，现在是时间来填充某些用户与您租户。  运行您快速启动的应用程序，并使用一个这些用户登录。  搜索其他用户基于其 UPN。  关闭应用程序，然后重新运行它。  注意到该用户的会话将保持不变。

ADAL 可以轻松地将所有这些公共标识功能合并到您的应用程序。  它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。  您真正需要知道的只是一个 API 调用， `getToken`。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)。  

## <a name="additional-scenarios"></a>其他方案
您可以立即将移动到其他方案。  您可能想要尝试︰

- [安全的 Node.JS Web API 加装了 Azure 的广告](active-directory-devquickstarts-webapi-nodejs.md)
- 了解[如何启用跨应用程序上使用 ADAL 的 iOS 的 SSO](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
