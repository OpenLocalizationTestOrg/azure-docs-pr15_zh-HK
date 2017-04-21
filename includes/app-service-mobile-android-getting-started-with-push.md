1. 在**应用程序**项目中，打开文件`AndroidManifest.xml`。 在接下来的两个步骤中的代码，将_`**my_app_package**`_app 包装箱中的为您的项目的名称，它的价值是`package`的特性`manifest`标记。

2. 在现有之后添加以下新权限`uses-permission`元素︰

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. 添加以下代码后的`application`开始标记︰

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. 打开的文件*ToDoActivity.java*，并添加下面的导入语句︰

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. 将下面的私有变量添加到类︰ 替换_`<PROJECT_NUMBER>`_与 Google 到您在前面步骤中的应用程序分配的项目编号︰

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. 将*MobileServiceClient*的定义为**公共静态**，从**私有**更改，它就像这样︰

        public static MobileServiceClient mClient;

7. 接下来，我们需要添加一个新的类来处理通知。 在项目资源管理器中，打开**src** => **主** => **java**节点，用鼠标右键单击包节点︰ 单击**新建**，然后单击**Java 类**。

8. **名称**类型中`MyHandler`，然后单击**确定**。


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. 在 MyHandler 文件中，替换的类声明

        public class MyHandler extends NotificationsHandler {


10. 添加为下面的导入语句`MyHandler`类︰

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. 接下来，添加到此成员`MyHandler`类︰

        public static final int NOTIFICATION_ID = 1;


12. 在`MyHandler`类中，添加以下代码以重写**onRegistered**方法，使用移动服务通知中心注册您的设备。

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. 在`MyHandler`类中，添加以下代码以重写**onReceive**方法，使它收到时显示通知。

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. 后在 TodoActivity.java 文件中，更新注册的通知处理程序类的*ToDoActivity*类的**onCreate**方法。 请确保将此代码添加*MobileServiceClient*实例化之后。


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    现在，您的应用程序将更新以支持推送通知。
