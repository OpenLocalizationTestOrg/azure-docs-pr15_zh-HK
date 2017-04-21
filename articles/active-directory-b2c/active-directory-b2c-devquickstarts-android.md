<properties
    pageTitle="Azure 的活动目录 B2C︰ 调用 web API 从 Android 应用程序 |Microsoft Azure"
    description="这篇文章将介绍如何创建待办事项列表 Android 使用 OAuth 2.0 载体标记调用 Node.js web API 的应用程序。 Android 的应用程序和 web API 使用 Azure 活动目录 B2C 用户进行身份验证并管理用户的身份。"
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C︰ 从 Android 应用程序调用 web API

> [AZURE.WARNING] 本教程需要专门去 B2C 的 ADAL Android 使用某些重要更新。  我们将发布在下个星期，在 Android 应用程序中使用 Azure AD B2C 的最新说明，我们建议存放到那时。  但是，如果您只是想要尝试出事情，随意继续下面文章。



通过使用 Azure 活动目录 (AD Azure) B2C，可以添加强大的自助服务身份标识管理功能，对 Android 的应用程序和 web Api 中简单的几步。 这篇文章将介绍如何创建"待办事项列表"Android 使用 OAuth 2.0 载体标记调用 Node.js web API 的应用程序。 Android 的应用程序和 web API 使用 Azure AD B2C 用户进行身份验证并管理用户的身份。

本快速入门要求网站 Azure 广告与 B2C 工作完全由受保护的 API。 我们已经构建了一个用于.NET，Node.js 供您使用。 本演练假定 Node.js web API 示例配置。 有关详细信息，请参阅[Azure AD B2C 网站的 API，Node.js 教程](active-directory-b2c-devquickstarts-api-node.md)。

对于需要访问受保护的资源的 Android 客户端，Azure 广告提供了活动目录身份验证库 (ADAL)。 ADAL 的目的是为了方便您的应用程序才能访问令牌。 以展示它是多么容易，本指南中我们将构建 Android 的待办事项列表的应用程序中︰

- 获取调用使用[OAuth 2.0 身份验证协议](https://msdn.microsoft.com/library/azure/dn645545.aspx)的待办事项列表 API 的访问令牌。
- 获取用户的待办事项列表。
- 用户出的迹象。

> [AZURE.NOTE] 本文不包括通过使用 Azure AD B2C 如何登录、 注册、 实现和配置文件管理。 它着重于如何对用户进行验证后调用 web Api。 如果尚未启动，首先应具有[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)了解 Azure AD B2C 的基础知识。

## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。 目录是您的用户、 应用程序、 组和更多的所有的容器。 如果您还没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)本指南在继续进行。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序进行安全通讯的 Azure 的广告信息。 应用程序和 web API 由表示单个**应用程序 ID**在这种情况下，因为它们构成了一个逻辑的应用程序。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。 请务必︰

- 包含**web 应用程序**/**web API**应用程序中。
- 输入`urn:ietf:wg:oauth:2.0:oob`作为**回复 URL**。 它是该代码示例的默认 URL。
- 创建应用程序的**应用程序密码**并将其复制。 您将稍后需要它。 请注意，此值必须是[转义 XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape)之前使用它。
- 复制**应用程序 ID**分配给您的应用程序。 您还需要这以后。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此应用程序包含三个标识体验︰ 注册，登录，并使用 Facebook 的登录。  您需要创建的每个类型，一种策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。 创建您的三个策略时，请务必︰

- 在您注册策略选择的**显示名称**和注册的其他属性。
- 在每个策略选择的**显示名称**和**对象 ID**的应用程序声明。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 它应具有前缀`b2c_1_`。  稍后，您将需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

创建三个策略后，就可以生成您的应用程序。

请注意，本文不介绍如何使用您刚才创建的策略。 若要了解有关在 Azure AD B2C 策略如何工作，开始与[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)。

## <a name="download-the-code"></a>下载的代码

[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android)此教程的代码。 当您生成示例，您可以[下载.zip 文件的主干项目](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **您需要下载完成本教程的主干。** 由于实施全功能的 Android 应用程序的复杂性，骨架有用户体验将运行的代码完成本教程之后。 这是为开发人员节省时间的措施。 UX 代码不是 germane 到如何添加 B2C 到 Android 应用程序的主题。

已完成应用程序同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip)或在`complete`的同一存储库中的分支。

若要使用 Maven 构建，您可以使用`pom.xml`最高级别。

  1. 请按照本文[先决条件部分设置为 Android 的 Maven](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)。
  2. 设置 SDK 21 一个仿真程序。
  3. 转到克隆 repo 的位置的根文件夹。
  4. 运行命令`mvn clean install`。
  5. 将目录更改为快速入门示例`cd samples\hello`。
  6. 运行命令`mvn android:deploy android:run`。

您应该会看到启动该应用程序。 输入测试用户凭据来试一试。

Java 归档文件 (JAR) 文件包将 Android 存档 (AAR) 包旁边还提交。

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>下载 Android ADAL，并将其添加到 Android Studio 工作区

有关如何使用此库 Android 项目中的选项有︰

* 源代码可用于导入到 Eclipse 中，并链接到您的应用程序库。
* 如果您使用 Android 的工作室，可以使用 AAR 包的格式，并引用二进制文件。

### <a name="option-1-binaries-via-gradle-recommended"></a>选项 1︰ 二进制文件通过 Gradle （推荐）

您可以从 Maven 中央 repo 获取二进制文件。 AAR 包可以包含在 Android Studio 中项目 (例如，在`build.gradle`) 这种方式︰

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>选项 2: AAR Maven 通过

如果您使用`m2e`在 Eclipse 插件，您可以指定中的依赖关系您`pom.xml`文件︰

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>选项 3︰ 通过 Git 源 （最后的手段）

若要获取 SDK 通过 Git 的源代码，请输入︰

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

使用分支**收敛。**

## <a name="set-up-your-configuration-file"></a>设置您的配置文件

使用您设置的配置在 B2C 门户配置 Android 项目中较早。

打开`helpes/Constants.java`，并填写下面的值︰

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`︰ 将传递给要回当用户向服务器请求的服务器作用域登录。 B2C 的预览中，您将传递`client_id`。 但是，这应改为`read scopes`在将来。 出现这种情况时，将更新此文档。
- `ADDITIONAL_SCOPES`︰ 要使用您的应用程序其他作用域。 他们会在将来使用。
- `CLIENT_ID`︰ 你从门户应用程序 ID。
- `REDIRECT_URL`︰ 预期的标记必须回发的位置重定向。
- `EXTRA_QP`︰ 任何额外的内容要传递给该服务器的 URL 编码格式。
- `FB_POLICY`︰ 您正在调用的策略。 这是最重要的部分，此预排。
- `EMAIL_SIGNIN_POLICY`︰ 您正在调用的策略。 这是最重要的部分，此预排。
- `EMAIL_SIGNUP_POLICY`︰ 您正在调用的策略。 这是最重要的部分，此预排。

## <a name="add-references-to-android-adal-to-your-project"></a>向项目中添加对 Android ADAL 的引用

> [AZURE.NOTE]  Android 的 ADAL 使用基于意图的模型要调用的身份验证。 方法"铺设"应用程序进行工作。 此完整的示例，并对 Android，所有 ADAL 如何管理方法和它们之间传递信息的中心。

首先，告诉 Android 的应用程序中，包括要使用的方式布局。 这些方法将在本教程后面部分中详细说明。

更新您的项目`AndroidManifest.xml`文件以包含所有您的方法︰

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

如您所见，您定义五个活动。 您将使用所有这些。

- `AuthenticationActivity`︰ 这来源于 ADAL，并提供登录 web 视图。
- `LoginActivity`︰ 这会显示您的登录策略和按钮为每个策略。
- `SettingsActivity`︰ 您可以使用它可以改变在运行时的应用程序设置。
- `AddTaskActivity`︰ 您可以使用此向通过 Azure 广告保护您 REST API 添加任务。
- `ToDoActivity`︰ 这是显示任务的主要活动。

## <a name="create-the-sign-in-activity"></a>创建登录活动

创建主活动，并称它为`LoginActivity`。

创建名为的文件`LoginActivity.java`。

您需要初始化该活动并添加一些按钮将控制您的 UI。 这是熟悉您，假如您编写了 Android 代码之前。

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
现在，您已创建按钮，调用您`ToDoActivity`（它需要一个标记时，请调用 ADAL） 的意图。 它们作为参考和额外的参数使用您的活动执行此操作。 此额外的参数传递`intent.putExtra()`方法。 您定义`"thePolicy"`通过使用在中指定`Constants.java`。 这说明意图要在身份验证过程中调用的策略。

## <a name="create-the-settings-activity"></a>创建设置活动

这是填充设置 UI 活动。

创建名为的文件`SettingsActivity.java`的简单创建、 读取、 更新和删除 (CRUD) 操作。

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>创建添加任务活动

您可以使用此活动将任务添加到 REST API 端点。

创建名为的文件`AddTaskActivity.java`和编写以下代码。

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>创建待办事项列表中的活动

这是最重要的活动。 您可以使用它以从 Azure AD 策略，获取令牌，然后使用该标记来调用任务 REST API 服务器。

创建名为的文件`ToDoActivity.java`和编写以下代码。 （调用会在后面解释。）

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 您可能已经注意到这依赖尚未尚未写入的方法。 它们包括`updateLoggedInUser()`， `clearSessionCookie()`，和`getTasks()`。 您将编写本指南下文中的那些。 您可以放心忽略目前的 Android Studio 中的错误。

参数的说明︰

  - `SCOPES`︰ 所需，想要请求的访问权限的范围。 B2C 预览，这等同于`client_id`，但这需要在以后更改。
  - `POLICY`︰ 当您想要对用户进行身份验证策略。
  - `CLIENT_ID`︰ 需要、 来自 Azure 广告门户。
  - `redirectUri`︰ 可以设置为您的包名称。 不需要为提供`acquireToken`调用。
  - `getUserInfo()`: 以查看用户是否已在缓存中的方法。 该参数还介绍了如何对用户发出提示，如果找不到该用户或用户的访问令牌无效。 此方法将在本指南后面写入。
  - `PromptBehavior.always`︰ 有助于索要凭据以跳过的缓存和 cookie。
  - `Callback`︰ 在授权码交换标记之后调用。 它将有一个对象， `AuthenticationResult`，其中包含访问令牌、 到期日期和 ID 标记信息。

> [AZURE.NOTE]  Microsoft Intune 公司门户应用程序提供代理程序组件，并可能在用户的设备上安装它。 该应用程序在设备上的所有应用程序提供单一登录 (SSO) 访问。 开发人员应准备允许 Intune。 如果在身份验证器中创建一个用户帐户，Android 的 ADAL 将使用代理帐户。 若要使用代理程序，开发人员需要注册一个特殊`redirectUri`的代理使用。 `redirectUri`采用 msauth://packagename/Base64UrlencodedSignature 格式。 您可以获得`redirectUri`为您的应用程序通过使用脚本`brokerRedirectPrint.ps1`或使用 API 调用`mContext.getBrokerRedirectUri()`。 从 Google 播放存储您的签名证书与签名。

 通过使用，您可以跳过代理用户︰

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] 为了减少此 B2C 快速入门的复杂性，我们已选择跳过代理程序在我们的示例。

接下来，创建获取令牌在身份验证任务 API 调用单独的帮助器方法。

在同一个`ToDoActivity.java`文件，编写以下代码。

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

接口中添加方法，将"设置"和"获取" `AuthenticationResult` （其中有您的令牌） 到全球`Constants`。 尽管`ToDoActivity.java`使用`sResult`在其流程中，您需要添加这些方法。 如果不这样做，其他活动也都没有对令牌进行工作访问 (例如，添加中的任务`AddTaskActivity.java`)。

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>创建一个方法以返回用户标识符

对于 Android 表示的用户在窗体中的 ADAL`UserIdentifier`对象。 它管理用户。 可以使用对象确定是否正在使用相同的用户在您调用。 通过使用此信息，您可以依赖于缓存中，而使新呼叫的服务器。 若要简化此过程，我们创建了`getUserInfo()`返回的方法`UserIdentifier`。 您可以使用它与`acquireToken()`。 我们还创建了`getUniqueId()`返回的 ID 的方法`UserIdentifier`在缓存中。

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>编写的帮助器方法

接下来，编写一些帮助器方法，以帮助您清除 cookie 并提供`AuthenticationCallback`。 这些方法只不过用于进行样本，以确保在调用时要保持干净状态您`ToDo`活动。

在同一文件中名为`ToDoActivity.java`，编写以下代码。

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>调用 API 的任务

您准备好抓住标记的活动后，您可以编写您的 API 来访问任务的服务器。

`getTasks`提供一个数组，表示您的服务器中的任务。

开始使用`getTasks`。

在同一文件中名为`ToDoActivity.java`，编写以下代码。

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

此外编写将初始化表上第一次运行的方法。

在同一文件中名为`ToDoActivity.java`，编写以下代码。

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

您可以查看此代码需要一些其他的方法来完成其工作。 编写这些下一步。

### <a name="create-an-endpoint-url-generator"></a>创建终结点的 URL 生成器

您需要生成您将连接到的端点 URL。 这样做在同一个类文件中。

**在同一文件中**名为`ToDoActivity.java`，编写以下代码。

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


注意到在下一节所述的代码请求中添加访问令牌。

## <a name="write-some-ux-methods"></a>写一些用户体验的方法

Android 需要处理一些回调以运行该应用程序。 这些都是`createAndShowDialog`， `onResume()`。 这是熟悉您，假如您编写了 Android 代码之前。

在同一文件中名为`ToDoActivity.java`，编写以下代码。

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

下一步，管理对话框回调。

在同一文件中名为`ToDoActivity.java`，编写以下代码。

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

您现在应该有`ToDoActivity.java`编译的文件。 此外应在此时编译整个项目。

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，构建并运行应用程序，Android Studio 或 Eclipse 中。 该应用程序在每次登录或注册。 创建已登录的用户的任务。 注销并重新登录以另一个用户，然后创建该用户的任务。

请注意，任务存储每个用户的 api，因为 API 的访问令牌，它接收从提取用户的身份。

为了便于参考，[以.zip 文件格式提供](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip)完整的示例。 您还可以从 GitHub 克隆它︰

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>重要信息


### <a name="encryption"></a>加密

ADAL 加密令牌和存储到`SharedPreferences`，默认情况。 您可以查看`StorageHelper`类的详细信息，请参阅。 Android 推出**4.3(API18) 的 AndroidKeyStore**安全存储私钥。 ADAL 使用的 API18 及以上。 如果您想要更低的 SDK 版本中使用 ADAL，您需要提供机密密钥在`AuthenticationSettings.INSTANCE.setSecretKey`。

### <a name="session-cookies-in-web-view"></a>在 web 视图中的会话 cookie

关闭应用程序后，android 的 web 视图不会清除会话 cookie。 可以使用下面的代码示例来处理这种情况。
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[了解更多有关 cookie](http://developer.android.com/reference/android/webkit/CookieSyncManager.html)。
