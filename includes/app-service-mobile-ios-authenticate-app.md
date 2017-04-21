**目标 C**: 

1. 在您的 Mac，打开 Xcode _QSTodoListViewController.m_并添加下面的方法。 如果您没有为您标识提供程序使用 Google， _google_更改为_microsoftaccount_，_使用 twitter_， _facebook_或_windowsazureactivedirectory_ 。 如果您使用 Facebook，[您需要对您的应用程序中的白名单 Facebook 域](https://developers.facebook.com/docs/ios/ios9#whitelist)。

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. 更换`[self refresh]`在`viewDidLoad`在_QSTodoListViewController.m_与下列︰

            [self loginAndGetData];

3. 请按_运行_以启动该应用程序，然后再登录。 当您登录时，您应该能够查看 Todo 列表并进行更新。

**Swift**:

1. 在您的 Mac，打开 Xcode _ToDoTableViewController.swift_并添加下面的方法。 如果您没有为您标识提供程序使用 Google， _google_更改为_microsoftaccount_，_使用 twitter_， _facebook_或_windowsazureactivedirectory_ 。 如果您使用 Facebook，[您需要对您的应用程序中的白名单 Facebook 域](https://developers.facebook.com/docs/ios/ios9#whitelist)。
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. 删除行`self.refreshControl?.beginRefreshing()`和`self.onRefresh(self.refreshControl)`结尾的`viewDidLoad()`在_ToDoTableViewController.swift_中。 添加对的调用`loginAndGetData()`在其位置︰

            loginAndGetData()

3. 请按_运行_以启动该应用程序，然后再登录。 当您登录时，您应该能够查看 Todo 列表并进行更新。
