<properties
    pageTitle="入门的 azure AD Android |Microsoft Azure"
    description="如何构建的 Android 应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>将 Azure AD 集成到一个 Android 的应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

如果要开发桌面应用程序，Azure 的广告使简单和直接地来使用自己的 Active Directory 帐户您的用户进行身份验证。  它还使应用程序可以安全地使用任何 web Azure 的广告，例如，Office 365 Api 或 Azure API 由受保护的 API。

对于需要访问受保护的资源的 Android 客户端，Azure 广告提供了活动目录身份验证库或 ADAL。  在现实生活中的 ADAL 的唯一目的是为了方便您的应用程序才能访问令牌。  以展示它是多么简单，此处我们将构建 Android 的待办事项列表应用程序的︰

-   获取访问令牌用于调用一个待办事项列表 API 使用[OAuth 2.0 身份验证协议](https://msdn.microsoft.com/library/azure/dn645545.aspx)。
-   获取用户的待办事项列表
-   标记出的用户。

若要开始，您将需要 Azure AD 租户可以在其中创建用户和注册应用程序。  如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

> [AZURE.TIP] 请尝试我们新[开发人员门户](https://identity.microsoft.com/Docs/Android)可帮助您在几分钟内使用 Azure 活动目录获取启动并运行预览 ！  开发人员门户将引导您完成注册应用程序并将 Azure AD 集成到您的代码的过程。  完成后，您可以在一个简单的应用程序可以验证您租户和后端中的用户能够接受的标记和执行验证。 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>步骤 1︰ 下载并运行 Node.js REST API TODO 示例服务器

此示例是写入专门针对 Microsoft Azure Active Directory 为构建单个租户待办事项 REST API，我们现有的示例工作。 这是用于快速入门的先决条件。

有关如何对此进行设置的信息，请访问我们现有的示例︰

* [Microsoft Azure Active Directory 示例 REST API 服务 Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>步骤 2︰ 注册您的 Microsoft Azure AD 租户 Web API

**我正在做什么呢？**

*Microsoft 活动目录支持添加两种类型的应用程序。Web 服务提供给用户和应用程序 （或者在 web 或在设备上运行的应用程序） 访问这些 Web Api 的 Api。在这一步注册 Web API 在本地运行测试此示例。通常此 Web API 会提供所需的应用程序来访问的功能的 REST 服务。Microsoft Azure Active Directory 可以防止任何终结点 ！*

*这里我们假定要注册 TODO REST API 引用上面，但是这适用于希望 Azure Active Directory 来保护任何 Web API。*

与 Microsoft Azure AD 注册 Web API 的步骤

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2. 单击左侧 nav.中的活动目录
3. 单击您想要注册的示例应用程序目录租户。
4. 单击应用程序选项卡。
5. 在抽屉里，单击添加。
6. 单击"添加我的公司正在开发的应用程序"。
7. 输入友好名称的应用程序，例如"TodoListService"，选择"Web 应用程序和/或 Web API"，然后单击下一步。
8. 登录 url，输入的基 URL 的示例中，默认为`https://localhost:8080`。
9. 对于应用程序 ID URI，输入`https://<your_tenant_name>/TodoListService`，并替换`<your_tenant_name>`Azure AD 租户的名称。  单击确定以完成注册。
10. 虽然仍然在 Azure 的门户中，单击您的应用程序的配置选项卡。
11. **查找客户端 ID 值并复制它放在一边**，您将需要这以后配置您的应用程序时。

## <a name="step-3-register-the-sample-android-native-client-application"></a>第 3 步︰ 注册示例 Android 的本机客户端应用程序

注册 web 应用程序是第一步。 接下来，您将需要 Azure Active Directory 告诉您的应用程序。 这允许应用程序与刚刚注册的 Web API 进行通信

**我正在做什么呢？**  

*如上所述，Microsoft Azure 活动目录支持添加两种类型的应用程序。Web 服务提供给用户和应用程序 （或者在 web 或在设备上运行的应用程序） 访问这些 Web Api 的 Api。在这一步注册此示例中的应用程序。必须在此应用程序能够访问请求 Web API 刚刚注册的顺序进行的。Azure 的 Active Directory 将拒绝甚至允许您的应用程序除非它注册提出用于登录 ！这是模式的安全机制的一部分。*

*这里我们假定您在登记本，上面提到的示例应用程序，但这适用于任何正在开发的应用程序。*

**在一个租户，为什么使应用程序和 Web API？**

*您可能已经猜到了，您可以构建访问从另一个租户在 Azure Active Directory 中注册的外部 API 的应用程序。如果您这样做时，将提示您的客户同意的 api 应用程序中使用。好的部分是，iOS 的活动目录身份验证库保管此同意为您 ！当我们进入到更高级的功能，您将看到这是从 Azure 和办公室以及其他服务提供商访问 Microsoft Api 套所需的工作的重要组成部分。现在，因为您注册您的 Web API 和应用程序相同的租户则看同意任何提示。如果您开发的应用程序只是为自己公司使用，这通常是这种情况。*

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2. 单击左侧 nav.中的活动目录
3. 单击您想要注册的示例应用程序目录租户。
4. 单击应用程序选项卡。
5. 在抽屉里，单击添加。
6. 单击"添加我的公司正在开发的应用程序"。
7. 输入的友好名称的应用程序，例如"TodoListClient-Android"，选择"本机客户端应用程序"，然后单击下一步。
8. 对于重定向 URI 中，输入`http://TodoListClient`。  单击完成。
9. 单击应用程序的配置选项卡。
10. 查找客户端 ID 值并复制它放在一边，这以后需要配置您的应用程序时。
11. 在"权限与其他应用程序"，单击"添加应用程序"。  在"显示"下拉列表中，选择"其他"，请单击上面的复选标记。  找到并单击 TodoListService，并单击底部复选标记以将应用程序添加。  从"授予权限"下拉列表中，选择"访问 TodoListService"并保存配置。



若要生成使用 Maven，可以使用 pom.xml 顶层

  * 克隆到您选择的目录中此 repo:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * 按照[Prerequests](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)部分来设置您的 android 的 maven 的步骤
  * 安装 SDK 19 仿真程序
  * 转到克隆 repo 的位置的根文件夹
  * 运行该命令︰ mvn 干净安装
  * 将目录更改为快速入门示例︰ cd samples\hello
  * 运行该命令︰ mvn android︰ 部署 android︰ 运行
  * 您应该可以看到应用程序启动
  * 输入测试用户凭据尝试 ！

Jar 包也会提交 aar 包的旁边。

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>步骤 4︰ 下载 Android ADAL，并将其添加到您的 Eclipse 工作区

我们已使之便于您有多个选项可用于使用此库 Android 项目中︰

* 可以使用源代码来将此库导入到 Eclipse 并链接到您的应用程序。
* 如果使用 Android Studio，您可以使用*aar*包格式并引用二进制文件。

####<a name="option-1-source-zip"></a>选项 1︰ 源 Zip

若要下载的源代码的副本，请在页面的右侧单击"下载 ZIP"或单击[此处](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz)。

####<a name="option-2-source-via-git"></a>选项 2︰ 通过 Git 的源

若要获取 SDK 通过 git 的源代码只需键入︰

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>选项 3︰ 通过 Gradle 的二进制文件

您可以从 Maven 中央 repo 获取二进制文件。 AAR 包可以包含在 AndroidStudio 中的项目的如下所示︰

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>通过 Maven 的第 4 种选择︰ aar

如果您在 Eclipse 中使用 m2e 插件，您可以在 pom.xml 文件中指定依赖项︰

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>选项 5: jar 包库文件夹内
可以从 maven repo 获取 jar 文件，并放到*库*文件夹，您的项目中。 您需要复制到项目以及所需的资源，因为 jar 包不包括它们。


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>步骤 5︰ 向项目中添加对 Android ADAL 的引用


2. 添加到项目的引用并将其指定为 Android 库。 如果您不确定如何执行此操作，[单击此处获得详细信息] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. 添加用于在调试您的项目设置的项目依赖项

4. 更新项目的 AndroidManifest.xml 文件，包括︰

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. 在您的主活动创建 AuthenticationContext 的实例。 此调用的详细信息不在讨论范围本自述文件，但您可以通过看[Android 的本机客户端示例](https://github.com/AzureADSamples/NativeClient-Android)获取一个不错的起点。 下面是一个示例︰

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * 注︰ mContext 是活动中的一个字段

8. 复制此代码块来处理 AuthenticationActivity 结束后用户输入凭据并获得授权码︰

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. 若要请求的标记，您定义回调

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. 最后，要求使用回叫的标记︰

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

参数的说明︰

  * 资源是必需的是您试图访问的资源。
  * 客户机 Id 是必需的来自于 AzureAD 门户。
  * 作为您的软件包名称，可以设置 redirectUri。 不需要为 acquireToken 调用提供。
  * PromptBehavior 帮助索要凭据缓存和 cookie，请跳过。
  * 授权码交换标记后，会调用回调。

  回调将使用 AuthenticationResult 的 accesstoken 对象，到期日期，和 idtoken 信息。

可选︰ **acquireTokenSilent**

您可以调用**acquireTokenSilent**来处理缓存，并标记刷新。 它提供了同样的同步版本。 它接受作为参数的用户 id。

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **经纪人**︰ Microsoft Intune 公司门户应用程序将提供代理程序组件。 ADAL 将的使用此身份验证器和开发人员在创建代理帐户，如果有一个用户帐户，选择不跳过它。 开发人员可以跳过代理用户︰

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 开发者需要注册代理使用特殊的 redirectUri。 RedirectUri 是 msauth://packagename/Base64UrlencodedSignature 的格式。 您可以为您的应用程序使用"brokerRedirectPrint.ps1"的脚本获取您 redirecturi 或使用 API 调用 mContext.getBrokerRedirectUri。 签名与签名证书相关。

 当前代理模型是一个用户。 AuthenticationContext 提供了 API 方法来获取代理用户。

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 如果帐户有效，将返回代理用户。

 您的应用程序清单应该有权使用 AccountManager 帐户︰ http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


使用本演练中，您应该有您需要成功地将与 Azure Active Directory 集成。 此工作的更多示例，请访问 AzureADSamples / 在 GitHub 上的存储库。

## <a name="important-information"></a>重要信息

### <a name="customization"></a>自定义项

库项目资源可以覆盖您应用程序的资源。 当您的应用程序在生成时，将发生这种情况。 鉴于此，可以自定义身份验证活动布局所需的方法。 您需要确保保留该 ADAL uses(Webview) 的控件的 id。

### <a name="broker"></a>代理程序

代理程序组件将使用 Microsoft Intune 公司门户应用程序传送。 将帐户管理器中创建帐户。 科目类型是"com.microsoft.workaccount"。 它只允许一个 SSO 帐户。 完成设备挑战某个应用程序后，它将创建此用户 SSO cookie。

### <a name="authority-url-and-adfs"></a>颁发机构 Url 和 ADF

ADFS 无法识别作为生产 STS，因此您需要打开的实例发现，在 AuthenticationContext 构造函数传递 false。

颁发机构 url 需要 STS 实例和租户名称︰ https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>正在查询缓存项

ADAL 提供了在 SharedPreferences 中使用一些简单的缓存的默认缓存查询功能。 您可以从与 AuthenticationContext 获得当前高速缓存︰
```Java
 ITokenCacheStore cache = mContext.getCache();
```
如果您希望自定义它，还可以提供您的缓存实现。
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL 提供了选项指定提示的行为。 如果刷新令牌无效，并且需要用户凭据时，PromptBehavior.Auto 会弹出 UI。 PromptBehavior.Always 将跳过缓存利用率，总是显示用户界面。

### <a name="silent-token-request-from-cache-and-refresh"></a>从缓存和刷新的静默令牌请求

此方法不使用用户界面 pop up 和不需要的活动。 如果可用的缓存中，它将返回标记。 如果令牌已过期，则它将尝试刷新它。 如果刷新令牌过期或失败，它将返回 AuthenticationException。

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

您还可以使用此方法时调用同步。 您可以设置回拨空，或者使用 acquireTokenSilentSync。

### <a name="diagnostics"></a>诊断程序

以下是信息的用于诊断问题的主要来源︰

+ 例外情况
+ 日志
+ 网络跟踪

另外，请注意相关 Id 是诊断程序库中的核心。 您可以设置您的相关性 Id 在每次请求都如果您想要将相关联的 ADAL 请求与您的代码中的其他操作。 如果不设置相关性 id，则 ADAL 将生成随机一个 and 所有日志消息和网络调用将使用相关性 id 标记。 自生成 id 在每次请求的更改。

#### <a name="exceptions"></a>例外情况

这显然是第一个诊断程序。 我们尽可能提供有用的错误消息。 如果您发现了一个无帮助意义的请提交问题和让我们知道。 此外请提供设备信息，如模型和 SDK #。

#### <a name="logs"></a>日志

您可以配置库生成日志消息可以用来帮助诊断问题。 配置日志记录时，进行下面的调用来配置 ADAL 将使用到抓手关闭每条日志消息生成的回调。


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
消息可以写入自定义的日志文件，如下所示。 遗憾的是，没有标准从设备获取日志的方法。 有一些可以帮助您与此的服务。 可以也创造自己，例如将文件发送到服务器。

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>日志记录级别

+ Error(Exceptions)
+ Warn(Warning)
+ 信息 （信息目的）
+ 详细 （详细信息）

设置日志级别如下︰
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 所有日志消息被都发送到 logcat 以及任何自定义日志回调。
您可以记录到文件窗体 logcat 所示的 belog 为︰

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Adb 传送的命令有关的更多示例︰ https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>网络跟踪

可以使用各种工具来捕获 ADAL 生成的 HTTP 通讯。  这是最有用的如果您熟悉 OAuth 协议或者需要诊断的信息提供给 Microsoft 或其他支持渠道。

Fiddler 是最简单的 HTTP 跟踪工具。  使用下面的链接可设置它到正确记录 ADAL 的网络流量。  才会有意义，就需要配置 fiddler 或任何其他工具如 Charles 记录未加密的 SSL 通信量。  注意︰ 这种方法生成的跟踪可能包含高度特权的访问令牌、 用户名和密码等信息。  如果您使用的生产帐户，并与第三方共享这些跟踪信息。  如果您需要提供跟踪到人，才能得到支持，再现具有用户名和密码，您不介意共享的临时帐户的问题。

+ [设置为 Android Fiddler](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [对于 ADAL 配置 Fiddler 规则](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>模式对话框
没有活动的 acquireToken 方法支持对话框提示。

### <a name="encryption"></a>加密

ADAL 加密令牌和存储在 SharedPreferences 中，默认情况。 您可以查看详细信息，请参阅 StorageHelper 类。 Android 推出 AndroidKeyStore 4.3(API18) 安全存储私钥。 ADAL 使用的 API18 及以上。 如果您想要使用较低的 SDK 版本的 ADAL，您需要提供机密密钥在 AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Oauth2 持有者面临的挑战

AuthenticationParameters 类提供功能即可 authorization_uri Oauth2 持有者面临的挑战。

### <a name="session-cookies-in-webview"></a>在 web 视图中的会话 cookie

关闭应用程序后，android 的 web 视图不会清除会话 cookie。 可以使用下面的代码示例来处理这种情况︰
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
更多有关 cookie: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>资源重写

ADAL 库包括下列两个 ProgressDialog 消息的英文字符串。

如果将需要本地化的字符串，应用程序应覆盖它们。

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM 对话框
Adal 版本 1.1.0 支持 NTLM 对话框，通过 WebViewClient onReceivedHttpAuthRequest 事件处理。 可以自定义对话框布局和字符串。

### <a name="cross-app-sso"></a>跨应用程序的 SSO
了解[如何启用跨应用程序上使用 ADAL 的 Android 的 SSO](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
