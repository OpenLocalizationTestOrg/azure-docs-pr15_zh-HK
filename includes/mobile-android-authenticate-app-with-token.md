
前面的示例显示一个标准登录，这需要客户端联系身份标识提供程序和后端 Azure 服务每次启动该应用程序。 此方法不仅是低效的您可以运行到与使用状况相关的问题应许多客户尝试启动您的应用程序在同一时间。 更好的方法是 Azure 服务返回的授权令牌缓存并尝试使用基于提供程序的登录之前，使用此第一。 

>[AZURE.NOTE]您可以缓存颁发由后端 Azure 服务的用户无论使用的客户端管理或托管服务的身份验证令牌。 本教程使用托管服务的身份验证。


1. 打开 ToDoActivity.java 文件，然后添加下面的导入语句︰

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. 添加对下列成员`ToDoActivity`类。

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. 在 ToDoActivity.java 文件中，添加的以下定义`cacheUserToken`方法。
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    此方法标记为私有的首选项文件中存储用户 id 和标记。 这应该保护对缓存的访问，以便沙盒处理以用于应用程序首选项是在设备上的其他应用程序不具有访问令牌。 但是，如果有人获得访问设备，则可能，它们可能访问令牌缓存通过其他途径。 

    >[AZURE.NOTE]如果令牌访问您的数据被视为高度敏感的人可能会获得对设备的访问，可以进一步保护加密令牌。 但是，超出了本教程的并取决于您的安全要求的范围将是完全安全的解决方案。


4. 在 ToDoActivity.java 文件中，添加的以下定义`loadUserTokenCache`方法。

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. 在*ToDoActivity.java*文件中，替换`authenticate`用下面的方法使用令牌缓存的方法。 如果您想要使用 Google 以外的帐户，请更改登录提供程序。

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. 生成应用程序并测试验证使用有效的帐户。 执行至少两次。 在第一次运行，您应该收到提示登录和创建令牌缓存。 在此之后，每次运行将尝试加载身份验证的令牌缓存，您应该不需要登录。



