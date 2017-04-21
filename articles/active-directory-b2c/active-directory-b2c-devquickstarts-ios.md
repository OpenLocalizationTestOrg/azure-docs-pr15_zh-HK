<properties
    pageTitle="Azure 的活动目录 B2C︰ 调用 web API 中使用第三方库的 iOS 应用程序 |Microsoft Azure"
    description="这篇文章将说明如何创建 iOS 待办事项列表应用程序通过使用 OAuth 2.0 载体标记使用第三方库调用 Node.js web API"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< 标签"活动目录 b2c"ms.service= ms.workload="identity"ms.tgt_pltfrm="na"ms.devlang="objectivec"ms.topic="英雄文章"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C︰ 使用第三方库的 iOS 应用程序从调用 web API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Microsoft 身份平台使用开放标准，如 OAuth2 和 OpenID 连接。 这使开发人员能够利用他们想要与我们的服务集成在一起的任何库。 以帮助开发人员在使用其他库中使用我们的平台我们编写类似于 demonstate 的几个演练如何配置连接到 Microsoft 身份平台的第三方库。 实现[RFC6749 OAuth2 规范](https://tools.ietf.org/html/rfc6749)的大多数库将能够连接到 Microsoft 身份平台。


如果您是新手 OAuth2 或 OpenID 连接此示例配置大部分可能没有多大意义给您。 我们建议您看一下简要[概述我们已在此处列出的协议](active-directory-b2c-reference-protocols.md)。

> [AZURE.NOTE]
    我们的平台在这些标准中，如条件接收和 Intune 策略管理，具有表达式的某些功能需要您使用 Microsoft Azure 标识库我们开放源代码。 
   
不是所有的 Azure Active Directory 方案和功能支持的 B2C 平台。  若要确定是否应使用的 B2C 平台，了解[B2C 的限制](active-directory-b2c-limitations.md)。


## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。 目录是您的用户、 应用程序、 组和更多的所有的容器。 如果您没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)继续。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序进行安全通讯的 Azure 的广告信息。 应用程序和 web API 由表示单个**应用程序 ID**在这种情况下，因为它们构成了一个逻辑的应用程序。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。 请务必︰

- 包含在应用程序的**移动设备**。
- 复制**应用程序 ID**分配给您的应用程序。 您还需要这以后。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此应用程序包含一个标识体验︰ 组合的签名和注册。 您需要创建的每个类型，此策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。 在创建策略时，请务必︰

- 在策略中选择的**显示名称**和注册属性。
- 在每个策略选择的**显示名称**和**对象 ID**的应用程序声明。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 它应具有前缀`b2c_1_`。  稍后，您将需要的策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

创建策略之后，就可以生成您的应用程序。


## <a name="download-the-code"></a>下载的代码

本教程中的代码[在 GitHub 上](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)维护。  要继续下去，则可以[下载应用程序作为.zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip） 或克隆它︰

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

或只是下载完整的代码，并立即开始︰ 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>下载第三方库 nxoauth2 并启动工作区

在本演练中，我们将使用从 GitHub，Mac OS X 与 iOS (Cocoa & Cocoa 触摸) OAuth2 库 OAuth2Client。 此库基于 OAuth2 规范草案 10。 它实现的本机应用程序配置文件，并支持最终用户授权的终结点。 这些是我们都需要用到与 Microsoft 身份平台集成顺序的所有事情。

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>将库添加到您的项目使用 CocoaPods

CocoaPods 是 Xcode 项目依赖关系管理器。 它自动管理上面的安装步骤。

```
$ vi Podfile
```
将以下内容添加到此 podfile:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

现在加载使用 cocoapods podfile。 这将创建新 XCode 工作区会加载。

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>项目的结构

我们必须为我们的项目在骨架中设置了以下结构︰

* **母版视图中**任务窗格
* 有关所选任务的数据**添加任务视图**
* 一个**登录视图**，允许用户登录到该应用程序。

我们将对各种文件在项目中添加身份验证跳中。 如可视化的代码的代码的其他部分不是 germane 为标识，为您提供。

## <a name="create-the-settingsplist-file-for-your-application"></a>创建`settings.plist`应用程序文件

很容易地配置应用程序，如果我们有一个集中的位置来放我们的配置值。 它还可以帮助您了解每个设置应用程序中的作用。 我们将利用*属性列表*作为提供给应用程序的这些值的方式。

* 创建/打开`settings.plist`文件下`Supporting Files`在您的应用程序工作区

* 输入下面的值 （我们将穿过它们详细地很快）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

让我们在这些细节中。


为`authURL`， `loginURL`， `bhh`，`tokenURL`您会发现您需要填写您组织的名称。 这是您已分配给您的 B2C 租户的组织名称。 例如， `kidventusb2c.onmicrosoft.com`。如果您使用 Microsoft Azure 标识库我们开放源代码我们将使用我们的元数据终结点向下拉动此数据。 我们所做的辛勤工作为您提取这些值。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

`keychain`值是 NXOAuth2Client 库将使用创建的钥匙链，以存储您标记的容器。 如果想要获得跨多个应用程序的 SSO 您可以在每个应用程序指定相同的钥匙链，以及请求该钥匙链使用 XCode entitements。 这是苹果文档中介绍的。

`<policy name>`每个 URL 的末尾是您想将上面创建的策略的位置。 该应用程序将调用这些策略，具体取决于流。

`taskAPI`是我们将为您 B2C 的令牌与 REST 端点添加任务或查询现有任务。 这已专门为此示例已设置。 您不需要更改该示例才能正常工作。

其余的这些值所需使用的库和只需创建要执行的上下文的值的位置。

现在，我们已经`settings.plist`创建的文件，我们需要的代码，以读取它。

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>设置应用程序数据类阅读我们的设置

让我们做一个简单的文件，只需分析我们`settngs.plist`我们上面创建并对任何类进行将来这些设置可用的文件。 因为我们不想创建新副本的数据每次类要求，我们将使用单一实例模式并只返回每次发出请求时设置时创建的同一个实例

* 创建`AppData.h`文件︰

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* 创建`AppData.m`文件︰

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

现在，我们可以轻松地在我们的数据通过只需调用`  AppData *data = [AppData getInstance];`在任何我们类为您将看到下面。



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>设置在您 AppDelegate NXOAuth2Client 库

NXOAuthClient 库需要设置一些值。 完成后，您可以使用 aquired 来调用 REST API 的令牌。 我们知道，由于`AppDelegate`的随时我们加载该应用程序将被调用它，我们放在我们配置值对该文件的意义。
* 打开`AppDelegate.m`文件

* 我们将稍后使用某些头文件中导入。

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* 添加`setupOAuth2AccountStore`AppDelegate 中的方法

我们需要创建 AccountStore，然后向它提供的数据，我们只是从阅读中`settings.plist`文件。

有的一些您应该知道的关于 B2C 此时服务，该服务将使此代码更易于理解︰


1. Azure AD B2C 使用*策略*提供的查询参数您请求提供服务。 这使得 Azure 活动目录作为应用程序的独立服务。 为了提供这些额外查询我们需要提供的参数`kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:`方法与我们自定义的策略参数。 

2. Azure AD B2C 与其他 OAuth2 服务器使用在很大程度的范围相同的方式。 但是由于使用的 B2C 是有关验证以访问资源的用户尽可能某些范围是绝对必需的流才能正常工作。 这是`openid`范围。 我们会自动提供的 Sdk 的 Microsoft 身份`openid`为您以便您不会看到我们 SDK 配置中的作用域。 由于我们使用的第三方库，但是，我们需要指定此作用域。

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
接下来，请确保您在 AppDelegate 下调用它`didFinishLaunchingWithOptions:`方法。 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>创建`LoginViewController`类，我们将使用它来处理身份验证请求

我们使用 web 视图的帐户登录。 这使得我们可以提示用户输入 SMS 短消息等其他因素 （如果配置） 或错误消息返回给用户。 这里我们将向上 web 视图，然后以后编写代码来处理从 Microsoft 标识服务 web 视图中将发生的回调。

* 创建`LoginViewController.h`类

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

我们将创建下面这些方法。

> [AZURE.NOTE] 
    请确保您绑定`loginView`为您演示图板在实际 web 视图。 否则不会有时间进行身份验证时可以弹出 web 视图。

* 创建`LoginViewController.m`类

* 添加一些变量来执行状态，因为我们的身份验证

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* 重写以处理身份验证的 web 视图方法

我们需要告诉我们希望当用户需要登录上面所述的行为的 web 视图。 您只可以剪切并粘贴下面的代码。

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* 编写代码以处理 OAuth2 请求的结果

我们需要将处理来自 web 视图返回 redirectURL 的代码。 如果它还不能成功，我们会再试一次。 同时库将提供您可以在控制台中查看或处理 asyncronously 错误。 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* 设置通知工厂。

相同的方法中，我们创建`AppDelegate`之上，但这一次我们将添加一些`NSNotification`s 告诉我们什么发生在我们的服务。 我们建立了观察者，告诉我们任何内容发生更改时使用标记。 我们一旦我们获得令牌返回用户回`masterView`。



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* 添加代码来处理用户只要登录本机启动请求

让我们创建每次我们有一个请求进行身份验证时将调用的方法。 这将是实际创建 web 视图的方法

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* 最后，让我们调用所有这些方法之上每次我们编写`LoginViewController`加载。 我们通过执行此操作将添加到这些方法我们`viewDidLoad`方法苹果让我们

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

您现在已完成创建我们将与我们为登录的应用程序进行交互的主要方式。 我们已经签署之后，我们需要使用我们收到了我们标记。 对此，我们将创建一些帮助器代码将调用 REST Api，我们使用此库。


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>创建`GraphAPICaller`类来处理我们的请求到 REST API

我们有每次加载我们的应用程序时加载的配置。 现在，我们需要进行一些处理它，一旦我们有一个令牌。 

* 创建`GraphAPICaller.h`文件

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

您看到此代码中，我们将创建两个方法︰ 一个可以从 API，另一个 api 添加任务的任务。

现在，我们已经建立了我们的接口，让我们添加实际的实现︰

* 创建`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，构建并运行 Xcode app。 注册或登录到该应用程序，并创建为已登录的用户的任务。 退出并重新登录以另一个用户，并创建该用户的任务。

请注意，任务存储每个用户的 api，因为 API 从它收到的访问令牌中提取用户的身份。


## <a name="next-steps"></a>下一步行动

您现在可以移动到更高级的 B2C 主题。 您可以尝试︰

[从 Node.js web 应用程序中调用 Node.js web API]()

[自定义用户体验 B2C 应用程序]()
