大部分时间身份验证错误是由于不正确或不一致的配置设置。 以下是要检查的事项有关的一些具体建议。

* 请确保您不会错过任何地方**保存**按钮。 这是方便，和结果是需要查看正确的值在门户页面上，但他们实际上在 Azure 的环境或广告 Azure 应用程序中尚未保存。
* 对于在 Azure 门户的**应用程序设置**刀片式服务器中配置的设置，请确保输入设置时，选择正确的 API 应用程序或 web 应用程序。  还要确保所做设置所输入**的应用程序设置**，并不是**连接字符串**，作为两个部分的格式是类似。
* JavaScript 的前端的身份验证，下载清单再次以验证`oauth2AllowImplicitFlow`已成功更改为`true`。
* 确认您配置的 Url 的任何地方使用 HTTPS:

    * 在项目代码中
    * 在 CORS
    * 在 Azure 环境每个 API 的应用程序和 web 应用程序的应用程序设置
    * 在 Azure AD 应用程序设置。
    
    请注意，是否您从门户复制 API 的应用程序的 URL，它通常有`http://`，您必须手动将其更改为`https://`。

* 确保代码做任何更改已成功部署。 例如，在多项目解决方案中则可以更改项目的代码和不小心选择一种其他打算部署更改时。
* 请确保，您要转到 HTTPS Url 在浏览器中，不是 HTTP Url。 默认情况下，Visual Studio 将创建发布配置文件与 HTTP Url，并且打开部署项目之后在浏览器中。
* 对于于 JavaScript 前端的身份验证，确保 CORS 上的 JavaScript 代码调用 API 应用程序已正确配置。 如果有有关问题是否 CORS 相关的疑问，请尝试"*"作为允许的原始 URL。 
* JavaScript 的前端，用于打开浏览器的开发人员工具控制台选项卡以获得错误的详细信息，并检查网络上的 HTTP 请求。 但是，控制台错误消息可能会引起误解。 如果您收到一条消息，指示 CORS 错误，真正的问题可能是身份验证。 您可以检查，是否这种情况通过暂时暂时禁用身份验证使用运行该应用程序。
* 对于.NET API 的应用程序，请确保您正在获取尽可能多的信息在错误消息中尽可能通过设置[customErrors 模式设置为 Off](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview)。
* 对于.NET API 的应用程序，启动[远程调试会话](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)，并检查值的变量的传递给代码，使用 ADAL 来获取载体令牌或代码检查索赔针对预期的服务主体 id。 请注意，您的代码可领取来自许多不同来源，配置值，这样就可以找到这样的奇异的事情发生。 例如，如果您键入了错误`ida:ClientId`与`ida:ClientID`代码配置环境设置 Azure 应用程序服务时，可能会收到`ida:ClientId`从 Web.config 文件，忽略 Azure 应用程序服务设置希望的值。 
* 如果一切不正常的 Internet Explorer 窗口中工作，现有日志项可能影响;请尝试 InPrivate 并尝试镶边或 Firefox。
