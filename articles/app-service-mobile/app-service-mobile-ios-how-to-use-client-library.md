<properties
    pageTitle="如何使用 iOS SDK 的 Azure 移动应用程序"
    description="如何使用 iOS SDK 的 Azure 移动应用程序"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>如何使用 iOS Azure 移动应用程序的客户端库

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

本指南教您可以执行常见的方案，使用最新的[Azure 移动应用程序 iOS SDK][1]。 如果您是新手 Azure 移动应用程序，第一个完成[Azure 移动应用程序快速开始]创建后端，创建一个表，并下载预构建的 iOS Xcode 项目。 在本指南中，我们将专注于客户端的 iOS SDK。 若要了解有关为后端服务器端 SDK 的详细信息，请参阅服务器 SDK Howto。

## <a name="reference-documentation"></a>参考文档

IOS 客户端 SDK 的参考文档的位置如下︰ [Azure 移动应用程序客户端引用 iOS][2]。

## <a name="supported-platforms"></a>支持的平台

IOS SDK 的 iOS 版本 8.0 或更高版本支持目标 C 项目、 Swift 2.2 项目和 Swift 2.3 项目。

"服务器流程"身份验证提供的用户界面使用 web 视图。  如果该设备不是产品的能够提供一个 web 视图用户界面中，然后另一种身份验证方法是产品的必需的是产品的范围以外。  
此 SDK 不因此适合监视类型或同样受限制的设备。

##<a name="Setup"></a>安装和系统必备组件

本指南假定您已使用的表创建后端。 本指南假定表中这些教程中的表相同的架构。 本指南还假设，在代码中引用`MicrosoftAzureMobile.framework`和导入`MicrosoftAzureMobile/MicrosoftAzureMobile.h`。

##<a name="create-client"></a>如何︰ 创建客户机

若要访问项目中的 Azure 移动应用程序端，创建`MSClient`。 更换`AppUrl`使用的应用程序的 URL。 您可以保留`gatewayURLString`和`applicationKey`为空。 如果您设置了用于身份验证的网关，则填充`gatewayURLString`具有网关的 URL。

**目标 C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>如何︰ 创建表引用

来访问或更新数据，创建对后端表的引用。 更换`TodoItem`您的表的名称

**目标 C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>如何︰ 查询数据

若要创建一个数据库查询，查询`MSTable`对象。 下面的查询获取的所有项目`TodoItem`，并记录每个项的文本。

**目标 C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>如何︰ 筛选器未返回数据

要筛选结果，有很多可用的选项。

若要使用谓词筛选，使用`NSPredicate`， `readWithPredicate`。 下列筛选器返回的数据来查找不完整 Todo 项。

**目标 C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>如何︰ 使用 MSQuery

若要执行复杂的查询 （包括排序和分页），创建`MSQuery`对象，直接地或通过使用谓词︰

**目标 C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`您可以控制多个查询行为。

* 指定结果的顺序
* 限制返回的字段
* 限制返回的记录数
* 在响应中指定总计数
* 在请求中指定的自定义查询字符串参数
* 应用附加的函数

执行`MSQuery`查询通过调用`readWithCompletion`对象上。

## <a name="sorting"></a>如何︰ 使用 MSQuery 的数据进行排序

若要排序结果，让我们看一个示例。 若要按升序排序的字段的文本，然后按完成按降序排序，请调用`MSQuery`如下所示︰

**目标 C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>如何︰ 限制字段并展开使用 MSQuery 查询字符串参数

若要限制在一个查询中返回的字段，请在**selectFields**属性中指定字段的名称。 本示例返回文字和已完成的字段︰

**目标 C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

若要在服务器的请求中包括附加查询字符串参数 （例如，因为自定义的服务器端脚本使用它们），填充`query.parameters`如下所示︰

**目标 C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>如何︰ 配置页面大小

Azure 的移动应用程序，使用页面大小控制每次从后端的表中提取的记录数。 对`pull`数据然后将批处理根据此页面大小，直到有更多的记录中提取的数据。

也可以配置使用**MSPullSettings** ，如下所示的页面大小。 缺省的页面大小为 50，并且下面的示例将它更改为 3。

您可以配置不同的页面尺寸性能方面的原因。 如果您有大量的小型数据记录，较高的页面大小减少了服务器往返过程数。 

此设置控制仅在客户端上的页面大小。 如果客户端要求的较大的页面大小不是移动应用程序的后端支持，页面大小上限为后端配置为支持的最大值。 

此设置也不是_字节大小_的数据记录的_编号_。

如果增加客户端页面大小，[也应增加服务器上的页面大小](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)。

**目标 C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>如何︰ 将数据插入

若要插入一个新的表行，创建`NSDictionary`和调用`table insert`。 如果启用了[动态架构]，Azure 应用程序服务移动后端会自动生成新的列基于`NSDictionary`。

如果`id`未提供后端会自动生成一个新的唯一 id。 提供您自己`id`若要使用电子邮件地址、 用户名或您自己的自定义值作为 id。 提供您自己的 ID 可能会减轻联接和面向业务的数据库的逻辑。

`result`包含已插入的新项。 这取决于您服务器的逻辑，它可能有其他的或修改的数据与内容传递给服务器。

**目标 C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>如何︰ 修改数据

若要更新现有的行，请修改项和调用`update`:

**目标 C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

或者，提供将行 ID 和已更新的字段︰

**目标 C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

至少，`id`进行更新时，必须设置属性。

##<a name="deleting"></a>如何︰ 删除数据

若要删除某个项，请调用`delete`的项︰

**目标 C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

此外，通过提供的行 ID 删除︰

**目标 C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

至少，`id`进行删除时，必须设置属性。

##<a name="customapi"></a>如何︰ 调用自定义的 API

利用自定义的 API，就可以公开任何后端功能。 它不需要映射到表操作。 不仅您获得更好地控制邮件，您甚至可以读取/设置标题并更改响应正文格式。 若要了解如何在后端上创建一个自定义的 API，读取[自定义的 Api](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

若要自定义 API 调用时，调用`MSClient.invokeAPI`。 请求和响应的内容将被视为 JSON。 若要使用其他媒体类型，[使用其他重载的`invokeAPI`] [ 5]。  若要使`GET`请求而不是`POST`请求，设置参数`HTTPMethod`到`"GET"`和参数`body`到`nil`（因为 GET 请求中没有邮件正文。）如果您自定义的 API 支持其他 HTTP 谓词，请更改`HTTPMethod`适当。

**目标 C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>如何︰ 注册推模板发送跨平台通知

若要注册模板，将与您在您的客户端应用程序中的**client.push registerDeviceToken**方法传递模板。

**目标 C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

模板类型 NSDictionary 的并且可以包含多个模板中的以下格式︰

**目标 C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

所有标记都会都去除安全的请求。  要将标记添加到安装或安装中的模板，请参阅[使用.NET 后端服务器 SDK Azure 移动应用程序的][4]。  若要发送通知使用这些已注册的模板，使用[通知集线器 Api][3]。

##<a name="errors"></a>如何︰ 处理错误

Azure 应用程序服务移动后端调用时，完成块包含`NSError`参数。 发生错误时，则此参数为非零。 在代码中，您应该检查此参数并处理错误，根据需要如前面的代码段所示。

文件[`<WindowsAzureMobileServices/MSError.h>`][6]定义的常量`MSErrorResponseKey`， `MSErrorRequestKey`，和`MSErrorServerItemKey`。 若要获取更多与此错误相关的数据︰

**目标 C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

此外，该文件定义常量为每个错误代码︰

**目标 C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>如何︰ 使用活动目录身份验证库的用户进行身份验证

活动目录身份验证库 (ADAL) 可用于登录到应用程序使用 Azure 活动目录的用户。 使用标识提供程序 SDK 客户端流量进行身份验证要优于使用`loginWithProvider:completion:`方法。  客户端流量身份验证提供了更多本机用户体验感觉并进行其他自定义。

1. [如何配置应用程序服务 Active Directory 登录]配置您移动应用程序端的 AAD 登录[7]教程。 请务必完成注册本机客户端应用程序的可选步骤。 对于 iOS 中，我们建议，重定向 URI 是窗体的`<app-scheme>://<bundle-id>`。 有关详细信息，请参阅[ADAL iOS 快速入门][8]。

2. 安装使用 Cocoapods 的 ADAL。 编辑您要包含下面的定义，使用 Xcode 项目的名称替换**您项目**的 Podfile:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   并从盒︰

        pod 'ADALiOS'

3. 使用终端，运行`pod install`目录中包含您的项目，然后打开生成的 Xcode 区 （非项目）。

4. 将下面的代码添加到应用程序中，根据所使用的语言。 在每个，使这些替换︰

    * 组织资源调配您的应用程序的名称替换**插入的主管机构-这里**。 格式应为 https://login.windows.net/contoso.onmicrosoft.com。 此值可以从 [Azure 经典门户中] 将 Azure Active Directory 中的域选项卡。
    * **插入资源 ID 这里**替换为您的移动应用程序后端的客户端 ID。 从门户下**Azure 活动目录设置**的**高级**选项卡，您可以获得的客户端 ID。
    * 从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。
    * 替换为您的网站_/.auth/login/done_的终结点，使用 HTTPS 方案**插入重定向 URI 这里**。 此值应与_https://contoso.azurewebsites.net/.auth/login/done_类似。

**目标 C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>如何︰ 使用 Facebook SDK 的 iOS 的用户进行身份验证

对于 iOS Facebook SDK 可用于登录到您的应用程序使用 Facebook 的用户。  使用身份验证的客户端流要优于使用`loginWithProvider:completion:`方法。  客户端流量身份验证提供了更多本机用户体验感觉和允许进行更多自定义。

1. [如何配置登录 Facebook 的应用程序服务]配置为 Facebook 登录您移动应用程序后的端[9]教程。

2. 按照[iOS 的入门的 Facebook SDK]安装 iOS 的 Facebook SDK[10]文档。 而不是创建一个应用程序，您可以添加您的现有注册的 iOS 平台。 

3. Facebook 的文档中包含一些目标 C 代码在应用程序委托。 如果您使用的**Swift**，可以使用下面的翻译为 AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. 除了添加`FBSDKCoreKit.framework`到项目中，还添加引用`FBSDKLoginKit.framework`以相同的方式。 

4. 将下面的代码添加到应用程序中，根据所使用的语言。 

**目标 C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>如何︰ 使用 Twitter，结构与 iOS 的用户进行身份验证

IOS 的结构可用于登录到您的应用程序使用 Twitter 的用户。 客户端流量身份验证要优于使用`loginWithProvider:completion:`方法，因为它提供了更多本机的用户体验感觉，并允许其他自定义项。

1. [如何配置应用程序服务的 Twitter 登录](app-service-mobile-how-to-configure-twitter-authentication.md)教程配置为 Twitter 签入您移动应用程序后的端。

2. 通过执行下面的[结构为 iOS 的入门]文档以及设置 TwitterKit，向项目中添加结构。

    > [AZURE.NOTE] 默认情况下，结构为您创建一个 Twitter 的应用程序。 您可以避免通过注册使用者密钥和用户机密前面使用下面的代码段创建创建的应用程序。  另外，您可以替换[结构仪表板]中显示的值提供给应用程序服务的使用者密钥和使用者密码值。 如果您选择此选项，请务必设置回调 URL 为占位符值，例如`https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`。

    如果您选择使用以前创建的机密，给您的应用程序的代理添加以下代码︰
    
    **目标 C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. 将下面的代码添加到应用程序中，根据所使用的语言。 

**目标 C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>如何︰ iOS 签到 sdk Google 的用户进行身份验证

您可以使用 SDK iOS 的 Google 登录到应用程序中使用的 Google 帐户登录的用户。  Google 最近宣布他们 OAuth 的安全策略的更改。  这些策略更改将在将来需要使用 Google SDK。

1. [如何配置应用程序服务的 Google 登录](app-service-mobile-how-to-configure-google-authentication.md)教程配置为 Google 登录您移动应用程序后的端。

2. 通过[Google 登录为 iOS 的开始集成](https://developers.google.com/identity/sign-in/ios/start-integrating)文档安装 Google SDK 的 iOS。 您可以跳过"身份验证与后端服务器"一节。

3. 将以下内容添加到您的代理`signIn:didSignInForUser:withError:`方法，根据所使用的语言。

**目标 C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. 请确保您还将添加到以下`application:didFinishLaunchingWithOptions:`在您的应用程序代理，替换相同的 ID，用于将应用程序服务配置为在步骤 1 中的"SERVER_CLIENT_ID"。

**目标 C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. 将下面的代码添加到应用程序中实现 UIViewController`GIDSignInUIDelegate`协议，根据所使用的语言。  再次，登录之前注销，尽管您不需要再次输入您的凭据，您看到一个许可对话框。  过期的会话令牌时，才调用此方法。
 
 **目标 C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure 的移动应用程序快速启动]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[动态架构]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[结构控制板]: https://www.fabric.io/home
[IOS 的入门的结构]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
