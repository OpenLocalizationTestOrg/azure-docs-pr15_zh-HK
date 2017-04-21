
1. 在解决方案视图 （或在 Visual Studio 中的**解决方案资源管理器**），用鼠标右键单击**组件**文件夹**获取更多的组件...**搜索**Google 云消息传递客户端**组件和，将其添加到项目。

2. 打开 ToDoActivity.cs 项目文件，然后添加以下使用类的语句︰

        using Gcm.Client;

3. 在**ToDoActivity**类中，添加以下新的代码︰ 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    这使您可以访问推入处理程序服务过程中的移动客户端实例。

4.  **MobileServiceClient**创建后， **OnCreate**方法中，添加以下代码︰

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

**ToDoActivity**您现在准备添加推式通知。