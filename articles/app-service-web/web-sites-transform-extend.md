<properties
    pageTitle="高级配置和扩展的 azure 应用程序服务 web 应用程序"
    description="若要转换 Azure 应用程序服务 web 应用程序中的 ApplicationHost.config 文件，并添加私有扩展来启用自定义管理操作中使用 XML 文档 Transformation(XDT) 声明。"
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>高级配置和扩展的 azure 应用程序服务 web 应用程序

通过使用[XML 文档转换](http://msdn.microsoft.com/library/dd465326.aspx)(XDT) 声明，您可以在 Azure 应用程序服务 web 应用程序中转换的[ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)文件。 您可以使用 XDT 声明添加私有扩展来启用自定义 web 应用程序管理操作。 本文包括的示例 PHP 管理器 web 应用程序扩展，使您能够通过一个 web 界面的 PHP 设置管理。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>通过 ApplicationHost.config 的高级的配置
Web 应用程序配置应用程序服务平台提供灵活性和控制。 虽然标准 IIS ApplicationHost.config 配置文件不是可用于在应用程序服务中直接编辑，该平台还支持声明性的基于 XML 文档转换 (XDT) 的 ApplicationHost.config 变换模型。

利用此转换功能，请与 XDT 内容和根目录下的网站 (d:\home\site) 在[Kudu 控制台](https://github.com/projectkudu/kudu/wiki/Kudu-console)的位置创建 ApplicationHost.xdt 文件。 您可能需要重新启动 Web 应用程序的更改才会生效。

下面的 applicationHost.xdt 示例演示了如何使用 PHP 5.4 一个 web 应用程序中添加新的自定义的环境变量。

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


在 FTP 根目录下 LogFiles\Transform 提供了日志文件转换状态和详细信息。

有关其他示例，请参见[https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)。

**请注意**<br />
下的模块的列表中的元素`system.webServer`不能删除或重新排序，但可能会添加到列表中。


##<a id="extend"></a>扩展您的 web 应用程序

###<a id="overview"></a>专用 web 应用程序扩展概述

应用程序服务支持 web 应用程序扩展为管理操作的扩展性点。 事实上，一些应用程序服务平台的功能实现为预安装的扩展。 无法修改预装的平台扩展，同时可以创建并配置 web 应用程序专用的扩展。 此功能还依赖于 XDT 声明。 创建一个专用的 web 应用程序扩展的关键步骤如下所示︰

1. Web 应用程序扩展**内容**︰ 创建应用程序服务所支持的任何 web 应用程序
2. Web 应用程序扩展**声明**︰ 创建 ApplicationHost.xdt 文件
3. Web 应用程序扩展**部署**︰ 在 SiteExtensions 文件夹下放置内容`root`

为使 web 应用程序的内部链接应指向的 ApplicationHost.xdt 文件中指定的应用程序路径的相对路径。 对 ApplicationHost.xdt 文件的任何更改都需要 web 应用程序回收。

**注意**︰ 这些关键元素的其他信息可在[https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions)。

包括详细的示例来说明如何创建和启用专用 web 应用程序扩展。 下面的 PHP 管理器示例的源代码可以从[https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager)下载。

###<a id="SiteSample"></a>Web 应用程序扩展示例︰ PHP 管理器

PHP 管理器是一个允许 web 应用程序管理员可以轻松地查看和配置它们的 PHP 设置使用 web 界面，而无需直接修改 PHP 的.ini 文件的 web 应用程序扩展。 Php 的公用配置文件包括 php.ini 文件位于下面的程序文件和。 位于您的 web 应用程序的根文件夹中的 user.ini 文件。 由于 php.ini 文件无法直接编辑应用程序服务平台上，使用 PHP 管理器扩展。 user.ini 文件，使设置的改动生效。

####<a id="PHPwebapp"></a>PHP 管理器的 web 应用程序

以下是 PHP 经理部署的主页︰

![TransformSitePHPUI][TransformSitePHPUI]

如您所见，一个 web 应用程序扩展是常规的 web 应用程序一样，但与另一个 ApplicationHost.xdt 文件放置在 web 应用程序 （关于 ApplicationHost.xdt 文件的更多详细信息可在本文下一节） 的根文件夹中。

使用 Visual Studio ASP.NET MVC 4 Web 应用程序模板创建 PHP 管理器扩展。 从解决方案资源管理器中的下列视图显示 PHP 管理器扩展结构。

![TransformSiteSolEx][TransformSiteSolEx]

所需的文件 I/O 的唯一特殊逻辑是指示 wwwroot 目录的 web 应用程序所在的位置。 如下面的代码示例所示，环境变量"主页"指示 web 应用程序的根路径，并可以通过附加"site\wwwroot"构造 wwwroot 路径︰

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


目录路径后，您可以使用常规文件 I/O 操作来读取和写入文件。

与 web 应用程序扩展的注意的一个点将处理的内部链接。  如果必须为绝对路径提供为您的 web 应用程序的内部链接 HTML 文件中的任何链接，您必须确保这些链接的前面附有您扩展您的根的名称。 这需要因为现在是您的扩展根"/`[your-extension-name]`/"而不是只是"/"，因此任何内部链接必须相应地进行更新。 例如，假设您的代码包括以下的链接︰

`"<a href="/Home/Settings">PHP Settings</a>"`

链接 web 应用程序扩展的一部分时，该链接必须为以下形式︰

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

可以通过以下任一方式来解决这一要求使用仅在相对路径中 web 应用程序中，或在 ASP.NET 应用程序中，使用`@Html.ActionLink`为您创建相应的链接的方法。

####<a id="XDT"></a>ApplicationHost.xdt 文件

您的 web 应用程序扩展的代码转下 %HOME%\SiteExtensions\[您扩展名称]。 我们将把它扩展根。  

ApplicationHost.config 文件中注册您的 web 应用程序扩展，您需要将扩展根中名为 ApplicationHost.xdt 的文件。 ApplicationHost.xdt 文件的内容应如下所示︰

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

选择作为扩展名称的名称应作为扩展根文件夹中具有相同的名称。

此操作将添加到新的应用程序路径的`system.applicationHost`在 SCM 网站下的网站列表。 SCM 站点是具有特定访问凭据的站点管理结束点。 它具有 URL `https://[your-site-name].scm.azurewebsites.net`。  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web 应用程序扩展部署

若要安装您的 web 应用程序扩展，可以使用 FTP 复制到 web 应用程序的所有文件`\SiteExtensions\[your-extension-name]`您要在其上安装扩展的 web 应用程序的文件夹。  请确保将 ApplicationHost.xdt 文件复制到此位置也。 请重新启动您的 web 应用程序以启用扩展。

您应该能够看到在 web 应用程序扩展︰

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

请注意，该 URL 就像 URL 的 web 应用程序，不同之处在于，它使用 HTTPS，包含".scm"。

可以通过使用密钥添加应用程序设置开发和调查期间禁用 web 应用程序的所有专用 （未预先安装的） 扩展`WEBSITE_PRIVATE_EXTENSIONS`，值为`0`。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
