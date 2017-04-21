<properties
    pageTitle="将云服务集成使用 Azure CDN |Microsoft Azure"
    description="教您如何部署云服务提供者集成 Azure CDN 终结点内容的教程"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>与 Azure CDN 集成云服务

云服务可以集成在一起 Azure CDN，提供云服务的位置的任何内容。 这种方法为您提供了以下优点︰

- 易于部署和更新的图像、 脚本和云服务的项目目录中的样式表
- 方便地升级您的云服务，例如 jQuery 或引导版本中的 NuGet 程序包
- 管理 Web 应用程序和提供 CDN 服务内容全部来自同一个 Visual Studio 界面
- 对于 Web 应用程序和您提供 CDN 服务的内容统一的部署工作流
- 将 ASP.NET 捆绑和缩小与 Azure CDN 相集成

## <a name="what-you-will-learn"></a>您将了解 ##

在本教程中，您将学习如何︰

-   [云服务相结合的 Azure CDN 终结点，并从 Azure CDN 提供 Web 页中的静态内容](#deploy)
-   [在云服务中配置静态内容的缓存的设置](#caching)
-   [提供通过 Azure CDN 的控制器操作中的内容](#controller)
-   [服务捆绑在一起，同时保留脚本调试体验在 Visual Studio 中的缩减到 Azure CDN 的内容](#bundling)
-   [配置备用脚本和 CSS Azure CDN 脱机时](#fallback)

## <a name="what-you-will-build"></a>您将生成 ##

将部署默认 ASP.NET MVC 模板使用云服务 Web 角色、 添加代码，以提供从集成的 Azure CDN，如图像、 控制器操作结果和默认的 JavaScript 和 CSS 文件的内容和编写代码来配置包提供的 CDN 处于脱机状态的备用机制。

## <a name="what-you-will-need"></a>您将需要 ##

本教程中必须满足以下先决条件︰

-   [Microsoft Azure 帐户](/account/)活动
-   [Azure](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) sdk 的 Visual Studio 2015 年

> [AZURE.NOTE] Azure 帐户来完成本教程，您需要︰
> + 您可以[免费开设 Azure 帐户](/pricing/free-trial/)-获取积分可用于试验 Azure 服务付费，并且即使他们习惯之后最多可以保留该帐户并使用释放 Azure 服务，例如网站。
> + 您可以[激活 MSDN 订户权益](/pricing/member-offers/msdn-benefits-details/)-您 MSDN 订阅提供了积分可用于付费的 Azure 服务每月。

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>云服务部署 ##

在本节中，将默认 Visual Studio 2015 ASP.NET MVC 应用程序模板部署到云服务 Web 角色，并将集成与新的 CDN 终结点。 请按照以下说明操作︰

1. 在 Visual Studio 2015，创建新的 Azure 的云服务从菜单栏转到**文件 > 新建 > 项目 > 云 > Azure 云服务**。 为它指定一个名称，然后单击**确定**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. 选择**ASP.NET Web 角色**，然后单击**>**按钮。 单击确定。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. **MVC**中选择并单击**确定**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. 现在，将此 Web 角色发布到 Azure 的云服务。 用鼠标右键单击该云服务项目并选择**发布**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. 如果您有尚未签署到 Microsoft Azure，请单击**添加帐户...**下拉列表，单击**添加帐户**菜单项。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. 在登录页中，使用用来激活您的 Azure 帐户 Microsoft 帐户进行登录。
7. 一旦您登录后，单击**下一步**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. 假定您没有创建一个云服务或存储帐户，Visual Studio 将帮助您创建两个。 在**创建云服务和帐户**对话框中，键入所需的服务名称并选择所需的区域。 然后，单击**创建**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. 在发布设置页中，验证配置，然后单击**发布**。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] 云服务的发布过程需要很长时间。 启用 Web 部署所有角色选项可能会使调试更快云服务通过提供对 Web 角色 （但临时） 快速更新。 有关此选项的详细信息，请参阅[发布云服务使用 Azure 工具](http://msdn.microsoft.com/library/ff683672.aspx)。

    当**Microsoft Azure 活动日志**显示，发布状态为**已完成**时，您将创建与此云服务集成在一起的 CDN 终结点。

    >[AZURE.WARNING] 如果发布后部署的云服务显示错误屏幕的错误，它可能是因为您已经部署了云服务使用[来宾操作系统不包含.NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates)。  您可以通过[部署.NET 4.5.2 作为启动任务](../cloud-services/cloud-services-dotnet-install-dotnet.md)来变通解决此问题。

## <a name="create-a-new-cdn-profile"></a>创建新的 CDN 配置文件

CDN 配置文件是一套 CDN 的终结点。  每个配置文件包含一个或多个 CDN 终结点。  您可能希望使用多个配置文件来组织您的 internet 域，web 应用程序或一些其他条件的 CDN 终结点。

> [AZURE.TIP] 如果您已经有您想要使用在本教程的 CDN 配置文件，继续执行[创建一个新的 CDN 终结点](#create-a-new-cdn-endpoint)。

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>创建新的 CDN 终结点

**若要创建新存储帐户的 CDN 终结点**

1. 在[Azure 管理门户](https://portal.azure.com)中，导航到您 CDN 的配置文件。  您可能已经固定其到上一步中的仪表板。  如果您没有，您可以发现它通过单击**浏览**，然后**CDN 的配置文件**，然后单击要添加到配置文件。

    CDN 配置文件刀片式服务器出现。

    ![CDN 的配置文件][cdn-profile-settings]

2. 单击**添加终结点**按钮。

    ![添加终结点按钮][cdn-new-endpoint-button]

    此时将显示**添加终结点**刀片式服务器。

    ![添加终结点刀片式服务器][cdn-add-endpoint]

3. 输入此 CDN 终结点的**名称**。  此名称将用于访问您在域的缓存的资源`<EndpointName>.azureedge.net`。

4. 在**源类型**下拉列表中，选择*云服务*。  

5. 在**源主机名**下拉列表，选择云服务。

6. 让**原始路径**、**原始主机标头**和**原始协议/端口**的默认值。  您必须指定至少一个协议 （HTTP 或 HTTPS）。

7. 单击**添加**按钮来创建新的终结点。

8. 创建终结点后，它的终结点配置文件列表中显示。 列表视图显示用于访问缓存的内容，以及原始域的 URL。

    ![CDN 终结点][cdn-endpoint-success]

    > [AZURE.NOTE] 终结点不立即可供使用。  它可能需要最多 90 分钟时间注册通过 CDN 网络传播。 通过 CDN 可用内容之前，请立即使用 CDN 的域名用户可能会收到状态代码 404。

## <a name="test-the-cdn-endpoint"></a>测试 CDN 终结点

**已完成**的发布状态时，打开一个浏览器窗口并定位到**http://<cdnName>*.azureedge.net/Content/bootstrap.css**。 在我的安装程序，此 URL 为︰

    http://camservice.azureedge.net/Content/bootstrap.css

该值对应于以下原始 URL CDN 终结点︰

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

当您导航到**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**，具体情况取决于您的浏览器，您将被提示要下载或打开来自您已发布的 Web 应用程序 bootstrap.css。

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

同样，您可以访问在任何公共可访问的 URL * *http://*&lt;站点 >*.cloudapp.net/**，直接从 CDN 端点。 例如︰

-   从 /Script 路径.js 文件
-   /Content 中的任何内容文件路径
-   任何控制器/操作
-   如果您的 CDN 终结点，任何与查询字符串的 URL 上启用了查询字符串

事实上，上述配置后，可以承载整个云服务从**http://*&lt;cdnName >*.azureedge.net/**。如果我定位到**http://camservice.azureedge.net/ * *，将操作结果从家庭中的索引。

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

但是，这并不意味着它一直是一个好主意 （或通常是个好主意） 为整个云服务通过 Azure CDN 提供服务。 下面是一些注意事项︰

-   此方法需要整个网站是公开的因为 Azure CDN 不能提供任何私人内容服务这一次。
-   如果由于某种原因，CDN 终结点离线，是否安排的维护时间或用户错误，整个云服务离线除非可以将客户重定向到原始 URL * *http://*&lt;站点 >*.cloudapp.net/**。
-   即使使用自定义缓存控制设置 （请参阅[配置缓存在云服务中的静态文件的选项](#caching)），CDN 终结点无法提高性能的高动态内容。 如果您尝试作为显示上面，请注意，花费至少 5 秒钟，第一次，这是一个相当简单的页加载默认主页 CDN 端点从加载主页。 想象一下会发生什么情况的客户端体验到如果该页包含必须更新每分钟的动态内容。 提供动态内容从 CDN 终结点需要短缓存过期时间，这相当于 CDN 终结点上的频繁缓存未命中数。 这会影响您的云服务的性能，并违背了 CDN。

另一种是确定要在云服务中基于案例的情况从 Azure CDN 服务的内容。 为此，您已经看到如何从 CDN 终结点访问单独的内容文件。 我将介绍如何在[提供内容从控制器操作通过 Azure CDN](#controller)提供 CDN 端点通过一个特定的控制器操作。

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>在云服务中配置静态文件的缓存选项 ##

Azure CDN 中云服务的集成，可以指定要如何 CDN 终结点以缓存的静态内容。 若要这样做，请从 Web 角色项目 (例如 WebRole1) 中打开*Web.config*并添加`<staticContent>`元素为`<system.webServer>`。 下面的 XML 配置的缓存，在 3 天后过期。  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

一旦这样做，在云服务中的所有静态文件将遵循相同的规则在 CDN 缓存中。 缓存设置的更精确地控制，将*Web.config*文件添加到文件夹中并添加设置存在。 例如，将*Web.config*文件添加到*\Content*文件夹和内容替换为下面的 XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

此设置会导致为缓存 15 天的*\Content*文件夹中的所有静态文件。

有关如何配置`<clientCache>`元素，请参阅[客户端缓存&lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache)。

在[服务于通过 Azure CDN 的控制器操作中的内容](#controller)，我还会显示您如何在 CDN 缓存配置控制器操作结果的缓存设置。

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>提供通过 Azure CDN 的控制器操作中的内容 ##

您将与 Azure CDN 集成云服务 Web 角色，时，相对容易地提供从通过 Azure CDN 的控制器操作的内容。 除提供云服务直接通过 Azure CDN （上面的说明）， [Maarten Balliauw](https://twitter.com/maartenballiauw)也演示如何用有趣的在[网上与 Azure CDN 减少延迟](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)的 MemeGenerator 控制器。 这里将只需复制它。

假设您想要生成基于这样一个年轻 Chuck Norris 图像 （照片由[亚伦光](http://www.flickr.com/photos/alan-light/218493788/)） memes 云服务中︰

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

有一个简单的`Index`允许用户指定在图像中，superlatives 的动作然后生成 meme 一旦它们过帐到的操作。 由于 Chuck Norris，希望此页后，可以成为全球流行的大幅变化。 这是使用 Azure CDN 服务半动态内容的一个很好示例。

请按照上述步骤以安装此控制器操作︰

1. 在*\Controllers*文件夹中，创建一个名为*MemeGeneratorController.cs*的新.cs 文件和内容替换为以下代码。 请务必使用 CDN 名称替换突出显示的部分。  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. 在默认的右击`Index()`操作并选择**添加视图**。

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  接受以下设置，然后单击**添加**。

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. 打开新的*Views\MemeGenerator\Index.cshtml*和内容替换为以下简单的 HTML 提交 superlatives:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. 重新发布的云服务，然后定位到**http://*&lt;站点 >*.cloudapp.net/MemeGenerator/Index** 浏览器中的。

当您提交表单值为`/MemeGenerator/Index`，`Index_Post`操作方法返回链接到`Show`与各自的输入标识符的操作方法。 当单击该链接时，您到达下面的代码所示︰  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

如果连接本地调试程序，您将得到与本地重定向的常规调试经验。 如果它正在运行在云服务中，然后它将重定向到︰

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

该值对应于以下原始 URL CDN 端点处︰

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


然后，您可以使用`OutputCacheAttribute`特性`Generate`方法，以指定如何操作结果应缓存，这将允许在 Azure CDN。 下面的代码中指定 1 小时 （3600 秒） 的缓存过期。

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

同样，您可以用任何控制器操作中的内容在云服务通过您 Azure CDN，与所需的缓存选项。

接下来，我将介绍如何提供捆绑和缩减脚本和 CSS 通过 Azure CDN。

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>将 ASP.NET 捆绑和缩小与 Azure CDN 相集成 ##

脚本和 CSS 样式表不经常更改，而且 Azure CDN 缓存的最佳候选对象。 服务于整个 Web 角色通过 Azure CDN 是最简单的办法将捆绑和缩小与 Azure CDN 相集成。 但是，因为您可能不想这样做，我将介绍了您如何去做，而如保留的 ASP.NET 捆绑和缩小，所需的 develper 经验︰

-   很好的调试模式的经验
-   简化的部署
-   立即更新客户端的脚本/CSS 版本升级
-   CDN 终结点失败时回退机制
-   最小化代码修改

在**WebRole1**项目中[集成 Azure CDN 终结点与 Web 页从 Azure CDN 中您 Azure 网站，并服务于静态内容](#deploy)创建，打开*App_Start\BundleConfig.cs* ，看一看`bundles.Add()`方法调用。

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

第一个`bundles.Add()`语句在虚拟目录中添加脚本包`~/bundles/jquery`。 然后，打开*Views\Shared\_Layout.cshtml*查看脚本包标记呈现方式。 您应该能够找到 Razor 代码的以下行︰

    @Scripts.Render("~/bundles/jquery")

在 Azure Web 角色中运行此 Razor 代码时，它会呈现`<script>`与以下内容类似的脚本包的标记︰

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

但是，运行时在 Visual Studio 中通过键入`F5`，它会分别呈现每个包中的脚本文件 （在上面的例子中，只有一个脚本文件是软件包中）︰

    <script src="/Scripts/jquery-1.10.2.js"></script>

这使您能够调试您的开发环境中的 JavaScript 代码，同时减少并发客户端连接 （绑定） 和改进文件下载在生产环境中的性能 （缩小）。 它是一个重要的功能，以保留 Azure CDN 集成。 此外，呈现的包已包含自动生成的版本字符串，因为您要复制的功能，因此只要通过 NuGet jQuery 版本更新时，它可以更新客户端尽快。

对集成 ASP.NET 捆绑和缩小使用 CDN 的终结点执行下列步骤。

1. 早在*App_Start\BundleConfig.cs*，修改`bundles.Add()`使用不同[包构造函数](http://msdn.microsoft.com/library/jj646464.aspx)，一个指定 CDN 地址的方法。 若要执行此操作，请替换`RegisterBundles`方法定义为以下代码︰  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    一定要替换`<yourCDNName>`Azure CDN 的名称。

    您正在设置中简要描述， `bundles.UseCdn = true` ，并添加到每个包的巧尽心思构建的 CDN URL。 例如，在代码中第一个构造函数︰

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    是相同的︰

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    此构造函数指示 ASP.NET 捆绑，并缩小呈现本地调试时的各个脚本文件，但使用指定的 CDN 地址访问问题脚本。 但是，请注意这个巧尽心思构建的 CDN url 的两个重要特征︰

    -   此 CDN URL 的原点是`http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`，这是实际的脚本包在云服务中的虚拟目录。
    -   由于您正在使用 CDN 构造函数，使用捆绑包的 CDN 脚本标记不再包含呈现 URL 中的自动生成的版本字符串。 每次修改脚本包时强制在 Azure CDN 缓存未命中，则必须手动生成一个唯一的版本的字符串。 在同一时间，这个唯一的版本字符串必须保持不变以最大化在 Azure CDN 缓存命中后部署包的部署生命周期中。
    -   查询字符串 v = < W.X.Y.Z > 拉从*Properties\AssemblyInfo.cs* Web 角色项目中的。 您可以包括每次您发布到 Azure 递增程序集版本的部署工作流。 或者，您只可以修改*Properties\AssemblyInfo.cs*项目每次生成时，使用通配符时自动递增的版本字符串中 ' *'。 例如︰

            [assembly: AssemblyVersion("1.0.0.*")]

        任何其他策略来优化生成部署的整个生命周期的唯一字符串将这里工作。

3. 重新发布云服务和访问主页。

4. 查看网页的 HTML 代码。 您应该能够看到 CDN URL 呈现，每次将更改重新发布到云服务的唯一版本字符串。 例如︰  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. 在 Visual Studio 中，调试 Visual Studio 中的云服务通过键入`F5`。，

6. 查看网页的 HTML 代码。 您仍可看到每个单独呈现，以便您可以具有一致出现在 Visual Studio 中调试的脚本文件。  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>CDN Url 的回退机制 ##

当 Azure CDN 端点由于某种原因失败时，您希望您 Web 页后，可以有足够的智能来访问原始 Web 服务器作为加载 JavaScript 或引导的备用选项。 很严重，会丢失图像在您的网站由于 CDN 不可用，但要失去您的脚本和样式表提供的关键页面功能严重得多。

[包](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx)类包含一个名为[CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) ，您可以配置备用机制 CDN 失败的属性。 若要使用此属性，请按照以下步骤操作︰

1. 在您的 Web 角色项目，打开*App_Start\BundleConfig.cs*，在每个[包的构造函数](http://msdn.microsoft.com/library/jj646464.aspx)中添加 CDN URL 位置，并以下突出显示的更改，将回退机制添加到缺省包︰  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    当`CdnFallbackExpression`是不为 null，脚本将注入到 HTML 测试是否成功加载包时，如果不是，直接从原始 Web 服务器访问此捆绑包。 此属性必须设置为测试是否正确加载相应的 CDN 捆绑的 JavaScript 表达式。 根据内容不同需要测试每个绑定的表达式。 对于上述的默认包︰

    -   `window.jquery`在 jquery 的 {版本}.js 中定义
    -   `$.validator`在 jquery.validate.js 中定义
    -   `window.Modernizr`在 modernizer-{版本}.js 中定义
    -   `$.fn.modal`在 bootstrap.js 中定义

    您可能已经注意到我没有设置为 CdnFallbackExpression`~/Cointent/css`捆绑。 这是因为目前没有注入[System.Web.Optimization 中的错误](https://aspnetoptimization.codeplex.com/workitem/104)`<script>`而不是预期的备用 css 标记`<link>`标记。

    没有，但是，由[某成员咨询组](https://github.com/EmberConsultingGroup)提供了良好[样式包备用](https://github.com/EmberConsultingGroup/StyleBundleFallback)。

2. 若要在 css 中使用的替代方法，称为*StyleBundleExtensions.cs*，Web 角色项目的*App_Start*文件夹中创建新的.cs 文件并[从 GitHub 代码](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs)替换其内容。

4. 在*App_Start\StyleFundleExtensions.cs*，重命名为您的 Web 角色名称 (例如**WebRole1**) 的命名空间。

3. 回到`App_Start\BundleConfig.cs`和修改最后`bundles.Add`语句与下面突出显示的代码︰  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    此新的扩展方法使用了相同的思想将在 HTML 中要检查的 DOM 脚本匹配的类名称、 规则名称和规则定义 CSS 捆绑和回退到原始 Web 服务器中，如果未能找到匹配的值。

4. 重新发布云服务和访问主页。

5. 查看网页的 HTML 代码。 您应该发现注入的脚本类似于以下︰    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    请注意，插入的脚本使用 CSS 捆绑包仍包含从四处游荡的残像`CdnFallbackExpression`属性行中︰

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    但由于第一部分 | |表达式将始终返回 true （在正上方的行），document.write() 函数将永远不会运行。

## <a name="more-information"></a>详细信息 ##
- [Azure 内容传递网络 (CDN) 的概述](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [使用 Azure CDN](cdn-create-new-endpoint.md)
- [ASP.NET 捆绑和缩小](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
