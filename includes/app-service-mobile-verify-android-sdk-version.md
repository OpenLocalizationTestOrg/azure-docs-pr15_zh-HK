由于不断开发，在 Android Studio 安装 Android SDK 版本可能不匹配的版本在代码中。 在本教程中引用 Android SDK 是版本 23，在书写时的最新。 新的 SDK 版本中出现，而我们建议使用最新版本可能会增加版本号。

两个版本不匹配的症状有︰

1. 生成或重新生成该项目时，可能会收到 Gradle 错误消息，如"**未能找到目标 Google Inc.:Google APIs:n**"。

2. 标准的 Android 对象中应解决的代码根据`import`语句可能会生成错误消息。

如果其中一种出现，Android SDK 安装在 Android Studio 的版本可能不匹配 SDK 下载的项目的目标。  若要验证的版本，请进行以下更改︰


1. 在 Android Studio 中，单击**工具** => **Android** => **SDK 管理器**。 如果您没有安装最新版本的 SDK 平台，然后单击安装。 记下版本号。

2. 在项目资源管理器选项卡下**Gradle 脚本**，打开的文件**build.gradle (modeule︰ 应用程序)**。 确保为最近安装的 SDK 版本设置了**compileSdkVersion**和**buildToolsVersion** 。 标记可能类似于︰
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Android Studio 项目资源管理器中右键单击项目节点，选择**属性**并在左边列中选择**Android**。 确保**项目生成目标**设置为同一 SDK 版本为**targetSdkVersion**。

4. 在 Android Studio 中，清单文件不再用于指定目标 SDK 和最低 SDK 版本，与 Eclipse 的情况不同。
