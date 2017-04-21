<properties
    pageTitle="连接到您的 Xamarin.Forms 应用程序中的 Azure 存储"
    description="通过连接到 Azure blob 存储为 todo 列表 Xamarin.Forms 移动应用程序中添加图像"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>连接到您的 Xamarin.Forms 应用程序中的 Azure 存储

## <a name="overview"></a>概述

在 Azure 移动应用程序客户端和服务器 SDK 支持 CRUD 操作对 /tables 终结点使用的结构化数据的脱机同步。 通常，此数据一直存储在数据库或类似的商店，并且这些数据存储区通常不能有效地存储大型二进制数据。 此外，某些应用程序中有相关的数据存储在其他位置 （如 blob 存储，文件共享），而且很有用的功能是能够创建 /tables 终结点中的记录与其他数据之间的关联。

本主题演示如何将对图像的支持添加到移动应用程序的 todo 列表快速入门。 您必须先完成本教程[创建一个 Xamarin.Forms 应用程序]。

在本教程中，您将创建存储帐户并将连接字符串添加到您的移动应用程序后端。 然后将添加新的移动应用程序类型继承新`StorageController<T>`到服务器项目。

>[AZURE.TIP] 本教程已[伴随示例](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/)可用，可以将其部署到 Azure 帐户。 

## <a name="prerequisites"></a>系统必备组件

* 完成[创建 Xamarin.Forms 应用程序]教程，其中列出了其他系统必备组件。 本文使用该教程中已完成的应用程序。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](https://tryappservice.azure.com/?appServiceName=mobile)。 那里，您立即可以应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

## <a name="create-a-storage-account"></a>创建存储帐户。

1. 通过遵循本教程[创建一个 Azure 存储帐户]创建存储帐户。 

2. 在 Azure 的门户中，定位到新创建的存储帐户并单击**项**图标。 复制**主连接字符串**。

3. 导航到您的移动应用程序后端。 在**所有设置**下 -> **应用程序设置** -> **的连接字符串**，创建一个新键名为`MS_AzureStorageAccountConnectionString`，并使用从您的存储帐户复制值。 为键类型使用**自定义**。

## <a name="add-a-storage-controller-to-the-server"></a>向服务器添加一个存储控制器

您需要为您的服务器项目，将 Azure 存储对 SAS 令牌的请求作出响应，以及返回到一个记录的对应的文件的列表中添加新的控制器︰

- [服务器项目中添加一个存储控制器](#add-controller-code)
- [存储控制器通过注册的路由](#routes-registered)
- [客户端和服务器通信](#client-communication)

###<a name="add-controller-code"></a>服务器项目中添加一个存储控制器

1. 在 Visual Studio 中，打开您的.NET 服务器项目。 添加 Nuget 程序包[Microsoft.Azure.Mobile.Server.Files]。 请务必选择**包括预发布版**。

2. 在 Visual Studio 中，打开您的.NET 服务器项目。 用鼠标右键单击**控制器**文件夹并选择**添加** -> **控制器** -> **Web API 2 控制器-空**。 该控制器命名为`TodoItemStorageController`。

3. 添加以下使用语句︰

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. 更改基类`StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. 将下列方法添加到该类︰

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. 更新 Web API 配置来设置路由选择特性。 在**Startup.MobileApp.cs**中，添加以下行`ConfigureMobileApp()`方法之后的定义,`config`变量︰

        config.MapHttpAttributeRoutes();

7. 将服务器项目发布到您的移动应用程序后端。

###<a name="routes-registered"></a>存储控制器通过注册的路由

新`TodoItemStorageController`公开它管理的记录下两子资源︰

- StorageToken

    + HTTP POST︰ 创建存储标记
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET︰ 检索与该记录关联的文件的列表
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP 删除︰ 删除指定的文件资源标识符中的文件
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>客户端和服务器通信

请注意， `TodoItemStorageController` does*不*具有用于上载或下载一个 blob 的路由。 这是因为移动客户端与 blob 存储*直接*交互才能执行这些操作之后第一个获得 SAS 令牌 （共享访问签名） 安全地访问特定的斑点或容器。 这是重要的体系结构设计，否则为访问存储将受到的可伸缩性和可用性移动的后端。 相反，通过直接连接到 Azure 存储，移动客户端可以充分利用其功能，如自动分区和地理分布。

共享的访问签名提供委派的访问您的存储帐户中的资源。 这意味着您可以授予客户端而无需共享您的帐户访问键指定期限内的时间，并具有一组指定的权限，限制对您的存储帐户中的对象的权限。 若要了解详细信息，请参阅[了解共享访问签名]。

下图显示了客户端和服务器的交互。 之前上传文件，客户端从服务请求 SAS 令牌。 该服务使用存储连接字符串生成新的 SAS，随后返回到客户端。 SAS 受时间的限制，并将权限限制为只是一个特定的文件或容器。 移动客户端然后使用此 SAS 和 Azure 存储客户端 SDK 的文件上载到 blob 存储。

![请求的 SAS 标记](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>更新您的客户端应用程序添加图像支持

在 Visual Studio 或 Xamarin Studio 中打开 Xamarin.Forms 快速启动项目。 Nuget 程序包的安装和更新的可移植的库项目的 iOS，Android 和 Windows 客户端项目︰

- [添加 Nuget 程序包](#add-nuget)
- [添加 IPlatform 接口](#add-iplatform)
- [添加 FileHelper 类](#add-filehelper)
- [添加文件的同步处理程序](#file-sync-handler)
- [更新 TodoItemManager](#update-todoitemmanager)
- [添加详细信息视图](#add-details-view)
- [更新主视图](#update-main-view)
- [Android 的项目更新](#update-android)， [iOS 项目](#update-ios)， [Windows 项目](#update-windows)

>[AZURE.NOTE] 本教程只包含有关 Android、 iOS 和 Windows 应用商店平台，没有 Windows Phone 的说明。

###<a name="add-nuget"></a>添加 Nuget 程序包

用鼠标右键单击该解决方案，然后选择**管理 Nuget 程序包的解决方案**。 将下面的 Nuget 程序包添加到解决方案中的**所有**项目。 请务必检查**包括预发布版**。

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

为方便起见，此示例使用[PCLStorage]库中，但它不必需的 Azure 移动应用程序客户端 SDK。

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>添加 IPlatform 接口

创建新的接口`IPlatform`主可移植的库项目中。 这遵循[Xamarin.Forms DependencyService]模式的权限特定于平台的类在运行时加载。 以后，您将每个客户端项目中添加特定于平台的实现。

1. 添加以下使用语句︰

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. 实现替换以下内容︰

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>添加 FileHelper 类

1. 创建一个新类`FileHelper`主可移植的库项目中。 添加以下使用语句︰

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. 添加类定义︰

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>添加文件的同步处理程序

创建一个新类`TodoItemFileSyncHandler`主可移植的库项目中。 此类包含从 Azure SDK 来添加或删除文件时通知代码回调。

Azure 移动客户端 SDK 实际上不存储任何文件数据︰ 客户端 SDK 调用实现的`IFileSyncHandler`顺序又决定是否以及如何将文件存储在本地设备上。

1. 添加以下使用语句︰

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. 用以下内容替换类定义︰ 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>更新 TodoItemManager

1. 在**TodoItemManager.cs**，请取消注释该行`#define OFFLINE_SYNC_ENABLED`。

2. 在**TodoItemManager.cs**中，添加以下使用语句︰

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. 在构造函数中的`TodoItemManager`，添加以下调用后的`DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. 在构造函数中，将替换为对的调用`InitializeAsync`以下。 这将确保在本地存储区中的更改记录有回调。 文件同步功能使用这些回调来触发文件同步处理程序。

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. 在`SyncAsync()`，添加以下调用后的`PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. 将以下方法添加`TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>添加详细信息视图

在本节中，您将添加一个 todo 项新详细信息视图。 当用户选择一个 todo 项，并允许新的图像添加到项目时，将创建视图。

1. 将**TodoItemImage**的新类添加到具有以下实现可移植类库项目︰

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. 编辑**程序**。 替换的初始化`MainPage`如下︰
    
        MainPage = new NavigationPage(new TodoList());

3. 在**程序**中，添加以下属性︰

        public static object UIContext { get; set; }

4. 右击可移植的库项目，然后选择**添加** -> **新项** -> **跨平台** -> **窗体 Xaml 页**。 命名视图`TodoItemDetailsView`。

5. 打开**TodoItemDetailsView.xaml** ，用以下内容替换内容页的正文︰

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. 编辑**TodoItemDetailsView.xaml.cs** ，并添加以下使用语句︰

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. 实现替换`TodoItemDetailsView`如下︰

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>更新主视图 

更新主视图时选择了 todo 项打开详细信息视图。

在**TodoList.xaml.cs**的实现替换`OnSelected`如下︰

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Android 的项目更新

Android 的项目，包括代码下载文件，并使用照相机捕获新映像中添加特定于平台的代码。 

此代码使用 Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/)权限特定于平台的类在运行时加载。

1. **Xamarin.Mobile**组件添加到 Android 项目。

2. 添加一个新类`DroidPlatform`具有以下实现。 "YourNamespace"替换为您的项目的主命名空间。

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. 编辑**MainActivity.cs**。 在`OnCreate`，添加以下调用之前`LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>IOS 项目更新

将特定于平台的代码添加到 iOS 项目。

1. **Xamarin.Mobile**组件添加到 iOS 项目。

2. 添加一个新类`TouchPlatform`具有以下实现。 "YourNamespace"替换为您的项目的主命名空间。

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. 编辑**AppDelegate.cs** ，并取消对的调用`SQLitePCL.CurrentPlatform.Init()`。

###<a name="update-windows"></a>更新 Windows 项目

1. 安装 Visual Studio 扩展[SQLite 的 Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919)。 有关详细信息，请参阅[启用脱机同步您的 Windows 应用程序](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)的教程。 

2. 编辑**Package.appxmanifest** ，并检查**网络摄像机**功能。

3. 添加一个新类`WindowsStorePlatform`具有以下实现。 "YourNamespace"替换为您的项目的主命名空间。

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>摘要

本文介绍了如何使用新的文件支持 Azure 移动客户端和服务器 SDK 中使用 Azure 存储。 

- 创建存储帐户并添加到您的移动应用程序后端的连接字符串。 只有后端具有 Azure 存储的关键︰ 移动客户端需要访问 Azure 存储时请求的 SAS 标记 （共享访问签名）。 若要了解有关 SAS Azure 存储中的令牌的详细信息，请参阅[了解共享访问签名]。

- 创建子类的控制器`StorageController`以处理 SAS 令牌请求并获得与记录相关联的文件。 默认情况下，文件是与记录关联容器名称; 中通过的记录 ID可以通过指定的一种实现自定义行为`IContainerNameResolver`。 也可以自定义 SAS 令牌策略。

- Azure 移动客户端 SDK 并不存储实际存储文件中的任何数据。 相反，客户端 SDK 调用您`IFileSyncHandler`，然后决定如何 （和如果） 文件存储在本地设备上。 同步处理程序注册，如下所示︰

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`Azure 移动客户端 SDK 需要 （例如，作为上载过程的一部分） 的文件数据时调用。 这使您能够管理方式 （和如果） 文件存储在本地设备上并将该信息在需要时返回。

      + `IFileSyncHandler.ProcessFileSynchronizationAction`调用文件同步流的一部分。 以便您可以确定您的应用程序应如何处理该事件 （例如在创建或更新时自动下载文件，当该文件被从服务器上删除，删除的文件从本地设备） 提供的文件引用和 FileSynchronizationAction 枚举值。

- A`MobileServiceFile`可用于以在线或离线模式，通过使用`IMobileServiceTable`或`IMobileServiceSyncTable`，分别。 在脱机的情况下，当应用程序调用发生上载`PushFileChangesAsync`。 这会导致脱机操作队列以进行处理;对于每个文件操作，Azure 移动客户端 SDK 将调用`GetDataSource`方法在`IFileSyncHandler`实例检索为上载该文件的内容。

- 要检索的项的文件，请调用 GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>`实例。 此方法返回提供与数据项相关联的文件的列表。 (注︰ 这是*本地*操作，将返回上次同步时，可根据对象状态的文件。 若要从服务器获取更新的文件列表，则应启动同步操作第一次。）

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- 文件同步功能来检索客户端接收推送或抽取操作的一部分的记录在本地存储上使用记录的更改通知。 这通过打开本地和服务器通知同步上下文使用`StoreTrackingOptions`参数。 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + 其他存储跟踪选项是可用的例如仅本地或只用于服务器的通知。 您可以添加或拥有自定义回调使用`EventManager`属性`IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- 也可以添加或从记录中移除文件，直接修改 blob 存储，因为关联通过命名约定。 然而，在这种情况下您应该始终**更新的记录时间戳，如果修改了关联的 blob**。 Azure 移动客户端 SDK 始终更新记录时添加或删除文件。 

    这一要求的原因是一些移动的客户端已经将本地存储区中有记录。 当这些客户端执行增量拉时，都不会返回该记录，客户端将不会查询新的相关文件。 若要避免此问题，建议在执行任何不使用 Azure 移动客户端 SDK 的 blob 存储更改时，更新记录的时间戳。

- 客户端项目使用的[Xamarin.Forms DependencyService]模式在运行时加载权限特定于平台的类。 在此示例中，我们定义了一个接口`IPlatform`在每个特定于平台的项目中实施。

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[创建一个 Xamarin.Forms 应用程序]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[了解共享访问签名]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[创建一个 Azure 存储帐户]:  ../storage/storage-create-storage-account.md#create-a-storage-account
