<properties
    pageTitle="将推式通知添加到您的 Xamarin.Forms 应用程序 |Microsoft Azure"
    description="了解如何使用 Azure 服务发送给您的 Xamarin.Forms 应用程序的多平台推式通知。"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>向您的 Xamarin.Forms 应用程序添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>概述

在本教程中，您将添加的推送通知发送到所有的项目源自[Xamarin.Forms 快速启动](app-service-mobile-xamarin-forms-get-started.md)，以便推式通知发送给所有跨平台客户机每次插入记录。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

##<a name="prerequisites"></a>系统必备组件

* 对于 iOS 中，您将需要一个[苹果开发商计划成员资格](https://developer.apple.com/programs/ios/)和物理 iOS 设备因为[iOS 模拟器不支持推式通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)。

##<a name="configure-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>更新发送推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>（可选）配置和运行 Android 的项目

完成此节可启用推式通知 Xamarin.Forms Droid 项目为 Android。


###<a name="enable-firebase-cloud-messaging-fcm"></a>启用消息传递 (FCM) 的 Firebase 云

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>配置发送推式请求使用 FCM 的移动应用程序后端

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Android 的项目中添加推式通知

与用 FCM 配置后端，我们可以添加组件和代码到客户端以使用 FCM，注册注册 Azure 通知集线器通过移动应用程序的后端，推式通知和接收通知。

1. 在**Droid**项目中，用鼠标右键单击**组件**文件夹**获取更多组件...**搜索**Google 云消息传递客户端**组件和，将其添加到项目中。 此组件支持 Xamarin Android 项目推式通知。


2. 打开 MainActivity.cs 项目文件，然后添加以下 using 语句，在文件的顶部︰

        using Gcm.Client;

3. 在对**LoadApplication**的调用之后**OnCreate**方法中添加以下代码︰

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. 添加一个新的**CreateAndShowDialog**帮助器方法，如下︰

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. 将以下代码添加到**MainActivity**类︰

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    这揭示当前的**MainActivity**实例，这样我们可以在主 UI 线程上执行。

6. 初始化`instance`、 变量，如下所示的**OnCreate**方法开头。

        // Set the current instance of MainActivity.
        instance = this;

2. 将新的类文件添加到名为的**Droid**项目`GcmService.cs`，并确保下面的**using**语句出现在文件的顶部︰

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. 在顶部的文件，添加以下权限请求之后**using**语句, 和**命名空间**声明之前。

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. 将下面的类定义添加到命名空间。 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]**< PROJECT_NUMBER >**替换为您前面提到的项目编号。   

11. 空的**GcmService**类替换为以下代码，使用新的广播的接收器︰

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. 将以下代码添加到**OnRegistered**事件处理程序重写并实现**注册**方法的**GcmService**类。

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. 添加下面的代码实现**活动消息**︰ 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    这处理传入通知并将其发送到通知管理器将显示。

14. **GcmServiceBase**还需要实现**OnUnRegistered**和**OnError**处理程序方法，您可以执行此操作，如下所示︰

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

现在，您将在 Android 设备或仿真程序上运行的应用程序准备好测试推式通知。

###<a name="test-push-notifications-in-your-android-app"></a>Android 应用程序中测试推式通知

仅当在模拟器上测试时前, 两个步骤是必需的。

1. 请确保您正在部署到或调试上了 Google Api 集作为目标，如下所示在 Android 虚拟设备 (AVD) 管理器的虚拟设备。

2. 通过单击**应用程序**到 Android 设备添加 Google 帐户 > **设置** > **添加帐户**，然后按照提示使用添加设备连接到现有的 Google 帐户创建一个新。

1. 在 Visual Studio 或 Xamarin Studio 中，右击该**Droid**项目，然后单击**设为启动项目**。

2. 按**运行**按钮以生成项目，然后启动应用程序，Android 设备或仿真程序上。

3. 在应用程序中，键入任务，然后单击加号 (**+**) 图标。

4. 验证在添加项时接收的通知。


##<a name="optional-configure-and-run-the-ios-project"></a>（可选）配置和运行 iOS 项目

本部分是用于运行 iOS 设备 Xamarin iOS 项目。 如果您不使用 iOS 设备，您可以跳过本节。

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>配置通知中心 APNS

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

接下来您将 Xamarin Studio 或 Visual Studio 中配置的 iOS 项目设置。

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>添加到您的 iOS 应用程序的推式通知

1. 在**iOS**项目中，打开 AppDelegate.cs 将以下**using**语句添加到代码文件的顶部。

        using Newtonsoft.Json.Linq;

4. 在**AppDelegate**类中，添加**RegisteredForRemoteNotifications**事件注册通知重写︰

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. 在**AppDelegate**中，添加以下重写**DidReceivedRemoteNotification**事件处理程序︰

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    该应用程序正在运行时，此方法处理传入通知。

2. 在**AppDelegate**类中， **FinishedLaunching**方法中添加以下代码︰ 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    这将使支持远程通知和请求推入注册。

现在，您的应用程序将更新以支持推送通知。

####<a name="test-push-notifications-in-your-ios-app"></a>IOS 应用程序中测试推式通知

1. 右键单击 iOS 项目，然后单击**设为 StartPp 项目**。

2. 在 Visual Studio 生成项目并在 iOS 设备上，启动该应用程序按**f5 键**的**运行**按钮，然后单击**确定**以接受推式通知。

    > [AZURE.NOTE] 从您的应用程序，您必须显式地接受推式通知。 此请求仅发生第一次应用程序运行。

3. 在应用程序中，键入任务，然后单击加号 (**+**) 图标。

4. 请验证通知收到后，然后单击**确定**以关闭通知。


##<a name="optional-configure-and-run-the-windows-projects"></a>（可选）配置和运行 Windows 项目

本部分是运行 Xamarin.Forms WinApp 和 WinPhone81 项目的 Windows 设备。 这些步骤还支持通用 Windows 平台 (UWP) 项目。 如果您不使用 Windows 设备，您可以跳过本节。


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>与 WNS 注册您的 Windows 应用程序的推式通知

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>为 WNS 配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>向您的 Windows 应用程序中添加推式通知

1. 在 Visual Studio 中，Windows 项目中打开**App.xaml.cs**并添加下面的**using**语句。

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    更换`<your_TodoItemManager_portable_class_namespace>`便携式项目包含的命名空间与`TodoItemManager`类。
 

2. 在 App.xaml.cs 中，添加下面的**InitNotificationsAsync**方法︰ 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    此方法获取推送通知通道并注册接收通知中心来自模板的通知模板。 支持*messageParam*的模板通知将发送到此客户端。

3. 在 App.xaml.cs 中，通过添加更新**OnLaunched**事件处理程序方法定义`async`修饰符，然后在方法的末尾添加下面这行代码︰ 

        await InitNotificationsAsync();

    这可以确保在创建或刷新每次启动该应用程序推送通知注册。 务必要执行此操作以保证 WNS 推通道始终处于活动状态。  

4. 在解决方案资源管理器中的 Visual Studio，打开**Package.appxmanifest**文件并设置**祝酒能够**为**是**在**通知**下。

5. 生成应用程序并验证不有任何错误。  您客户端应用程序应立即注册移动应用程序的后端模板通知。 为您的解决方案中的每个 Windows 项目重复本部分。


####<a name="test-push-notifications-in-your-windows-app"></a>在您的 Windows 应用程序中测试推式通知

1. 在 Visual Studio 中，右击 Windows 项目，然后单击**设为启动项目**。

2. 按**运行**按钮以生成项目，然后启动应用程序。

3. 在应用程序中，为新的 todoitem，键入一个名称，然后单击加号 (**+**) 图标来添加它。

4. 验证在添加项目时，收到通知。

##<a name="next-steps"></a>下一步行动

了解有关推式通知的详细信息︰

* [推式通知问题诊断](../notification-hubs/notification-hubs-push-notification-fixer.md)  
有通知可能会被丢弃或没有停止在设备上的各种原因。 本主题演示如何分析和计算出的推式通知失败的根本原因。 

请考虑继续下面的教程之一︰

* [添加到您的应用程序的身份验证](app-service-mobile-xamarin-forms-get-started-users.md)  
了解如何标识提供程序与应用程序的用户进行身份验证。

* [使您的应用程序的脱机同步](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

