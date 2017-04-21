<properties
    pageTitle="Azure AD，2.0 版 iOS 应用程序 |Microsoft Azure"
    description="如何构建 iOS 应用程序的签名中有两个人的 Microsoft 帐户的用户，通过使用第三方库的工作或学校科目。"
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>添加登录到 iOS 应用程序图形 API 使用 v2.0 终结点与使用第三方库

Microsoft 身份平台使用开放标准，如 OAuth2 和 OpenID 连接。 开发人员可以使用任何他们想要与我们的服务集成在一起的库。 为了帮助开发人员与其他库使用我们的平台，我们编写了这个例子来演示如何配置以连接到 Microsoft 身份平台的第三方库的几个演练。 大多数实现[RFC6749 OAuth2 规范](https://tools.ietf.org/html/rfc6749)的库可以连接到 Microsoft 身份平台。

使用本演练中创建的应用程序，用户可以登录到他们的组织，然后搜索其他人在其组织中使用图形 API。

如果您是新手，OAuth2 还是 OpenID 连接，此示例配置大部分可能没有意义给您。 我们建议您阅读[2.0 版协议的 OAuth 2.0 授权代码流](active-directory-v2-protocols-oauth-code.md)为背景。


> [AZURE.NOTE]
    我们的平台在 OAuth2 或连接 OpenID 标准，如条件接收和 Intune 策略管理，具有表达式的某些功能需要您使用我们开源 Microsoft Azure 标识库。

V2.0 终结点不支持所有 Azure Active Directory 方案和功能。

> [AZURE.NOTE]
    要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="download-code-from-github"></a>从 GitHub 上下载的代码
本教程中的代码[在 GitHub 上](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip)或克隆主干︰

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

您也可以下载示例并立即开始︰

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>注册应用程序
在[应用程序注册门户](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，创建新的应用程序或按照如何[注册使用 2.0 版的终结点的应用程序](active-directory-v2-app-registration.md)的详细的步骤。  请确保︰

- 复制**应用程序 Id** ，因为您很快将需要分配给您的应用程序。
- 添加您的应用程序的**移动**平台。
- 从门户复制**重定向 URI** 。 您必须使用默认值为`urn:ietf:wg:oauth:2.0:oob`。


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>下载第三方 NXOAuth2 库并创建工作区

对于本演练，您将使用从 GitHub，是 Mac OS X 和 Io （Cocoa 和触摸 Cocoa） OAuth2 库 OAuth2Client。 此库基于 OAuth2 规范草案 10。 它实现的本机应用程序配置文件，并支持用户的授权终结点。 这些是您将需要与 Microsoft 身份平台集成的所有事情。

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>通过使用 CocoaPods 将库添加到项目

CocoaPods 是 Xcode 项目依赖关系管理器。 它自动管理以前的安装步骤。

```
$ vi Podfile
```
1. 将以下内容添加到此 podfile:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. 通过使用 CocoaPods 加载 podfile。 这将创建新 Xcode 工作区将会加载。

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>了解项目的结构

为我们的项目在骨架中设置以下结构︰

- 母版视图中，使用 UPN 搜索
- 有关所选用户数据详细信息视图
- 登录视图，用户可以登录到应用程序以查询关系图

我们将移动到在骨架中的各种文件以添加身份验证。 可视化代码，代码的其他部分与身份无关，但为您提供。

## <a name="set-up-the-settingsplst-file-in-the-library"></a>设置的 settings.plst 文件库中

-   在快速启动项目中，打开`settings.plist`文件。 替换为要反映在 Azure 门户中使用的值的节中的元素的值。 只要它使用活动目录身份验证库，您的代码将引用这些值。
    -   `clientId`是从门户中复制的应用程序的客户端 ID。
    -   `redirectUri`是门户网站提供的重定向 URL。

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>设置在您 LoginViewController NXOAuth2Client 库

NXOAuth2Client 库需要设置一些值。 完成该任务后，您可以使用已获得的令牌调用图形 API。 因为`LoginView`将会调用任何时候我们要进行身份验证，所以应该放到该文件中的配置值。

- 让我们添加一些值`LoginViewController.m`文件来设置身份验证和授权的上下文。 有关这些值的详细信息代码后。

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

让我们看一下有关代码的详细信息。

第一个字符串为`scopes`。  `User.Read`值允许您读取登录的用户的基本配置文件。

您可以了解有关[Microsoft Graph 权限](https://graph.microsoft.io/docs/authorization/permission_scopes)范围的可用作用域的详细信息。

为`authURL`， `loginURL`， `bhh`，和`tokenURL`，您应使用前面提供的值。 如果您使用开源 Microsoft Azure 标识库，我们提取此数据向下您使用我们的元数据终结点。 我们所做的辛勤工作为您提取这些值。

`keychain`值是 NXOAuth2Client 库将使用创建的钥匙链，以存储您标记的容器。 如果您想要获得单一登录 (SSO) 跨多个应用程序，可以在每个应用程序指定相同的钥匙链和请求该钥匙链使用 Xcode 权利。 苹果文档中对此进行了说明。

其余的这些值所需使用的库并创建可执行的上下文的值的位置。

### <a name="create-a-url-cache"></a>创建一个 URL 缓存

在`(void)viewDidLoad()`，它总是调用加载视图后，下面的代码将供我们使用的缓存。

添加以下代码︰

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>创建用于登录 web 视图

Web 视图可以提示用户输入 SMS 短消息等其他因素 （如果配置），或向用户返回错误消息。 您将在此处设置 web 视图并且然后稍后编写代码来处理身份服务从 web 视图中将发生的回调。

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>重写以处理身份验证的 web 视图方法

来告诉 web 视图当用户需要登录，如前面所述，将会发生什么情况，您可以粘贴下面的代码。

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>编写代码以处理 OAuth2 请求的结果

下面的代码将处理从 web 视图返回 redirectURL。 如果身份验证不成功，该代码将再试一次。 同时，库将提供可以在控制台中看到，也可以异步处理的错误。

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>设置了 OAuth 上下文 （称为帐户存储）

在此处您可以调用`-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]`为每个服务所需的应用程序能够访问共享的帐户存储上。 科目类型是 string，它作为一种标识用于某个服务。 要访问图形 API，因为代码引用了其作为`"myGraphService"`。 然后设置将告诉您使用标记的任何内容更改时观察者。 获取标记后，则返回用户回`masterView`。



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>设置母版视图来搜索和显示图形 API 的用户

本演练中，范畴是在网格中显示返回的数据的主-视图-控制器 (MVC) 应用程序，很多在线教程介绍了如何创建一个。 所有这些代码是在骨架的文件中。 但是，您需要处理此 MVC 应用程序中的几种方法︰

* 当用户键入的内容在搜索字段中的截距
* 回 MasterView 提供的数据的对象，因此它可以在网格中显示结果

我们将执行那些下面。

### <a name="add-a-check-to-see-if-youre-logged-in"></a>检查以确定是否要记录中添加

应用程序很少如果用户没有登录，因此很智能，检查是否已经存在一个令牌缓存中。 否则，您将重定向到登录用户登录视图。 如果您还记得，视图加载时执行的操作的最佳方法是使用`viewDidLoad()`苹果为我们提供的方法。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>接收数据时，更新表视图

当图形 API 返回数据时，您需要显示的数据。 为简单起见，下面是可更新的表的所有代码。 只可以在 MVC 样板代码中粘贴正确的值。

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>提供一种方法来调用图形 API，当有人在搜索字段中键入

当用户在框中键入搜索时，您需要到图形 API 撞的。 `GraphAPICaller`类，您将生成以下代码中，将查找功能分开的演示文稿中。 现在，让我们来编写源图形 api 的任何搜索字符的代码。 我们这样做是通过提供一种方法称为`lookupInGraph`，接受我们想要搜索的字符串。

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>编写一个 Helper 类访问图形 API

这是我们的应用程序的核心。 而其余的苹果从默认的 MVC 模式中插入代码，这里您编写代码来查询作为用户类型的关系图，然后返回该数据。 以下是相关代码，和它后面的详细的说明。

### <a name="create-a-new-objective-c-header-file"></a>创建一个新的目标 C 头文件

作为文件名`GraphAPICaller.h`，并添加下面的代码。

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

这里您看到的指定的方法接受一个字符串并返回 completionBlock。 该 completionBlock，您可能已经猜到了，将更新表对象提供实时为用户搜索填充的数据。


### <a name="create-a-new-objective-c-file"></a>创建一个新的目标 C 文件

作为文件名`GraphAPICaller.m`，并添加下面的方法。

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


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

```

让我们通过这种方法在细节中。

此代码的核心是在`NXOAuth2Request`，方法采用参数，该方法已经定义在 settings.plist 文件中。

第一步是构建适当的图形 API 调用。 因为您正在调用`/users`，将其附加到版本以及图形 API 资源指定的。 要将这些外部设置文件中放，因为这些会随着 API 的发展意义。


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

接下来，您需要指定您还向图形 API 调用提供的参数。 这一点*非常重要*，不要放置参数资源端点中因为它所有非 URI 一致字符在运行时清理。 必须在参数中提供所有查询代码。

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

您可能会注意到这将调用`convertParamsToDictionary`您以前没有编写的方法。 让我们不要在文件末尾的现在︰

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
接下来，我们将使用`NXOAuth2Request`方法以从 API 以 JSON 格式返回数据。

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

最后，让我们看一下如何将数据返回到 MasterViewController。 数据返回序列化和反序列化并加载到一个对象，可以使用 MainViewController 中所需要。 为此目的，该骨架已`User.m/h`创建用户对象的文件。 填充该用户对象关系图中的信息。

```objc
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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>运行示例

如果您使用框架或连同本演练后面应该现在运行您的应用程序。 启动模拟器，单击**登录**以使用此应用程序。

## <a name="get-security-updates-for-our-product"></a>我们的产品中获得安全更新

我们鼓励您能够访问[安全技术中心](https://technet.microsoft.com/security/dd252948)和安全通报警报订阅时出现安全事件的通知。
