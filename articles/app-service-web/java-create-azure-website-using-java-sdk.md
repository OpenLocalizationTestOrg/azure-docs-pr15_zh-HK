<properties 
    pageTitle="在 Azure 应用程序服务的 Java 使用 Azure SDK 中创建 Web 应用程序" 
    description="了解如何在 Azure 应用程序服务使用 Java Azure SDK 以编程方式创建一个 Web 应用程序。" 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>在 Azure 应用程序服务的 Java 使用 Azure SDK 中创建 Web 应用程序

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>概述

本演练演示如何创建 Azure SDK 在[Azure 应用程序服务][]，创建一个 Web 应用程序的 Java 应用程序，然后部署到该应用程序。 它由两部分组成︰

- 第 1 部分演示如何构建的 Java 应用程序创建 web 应用程序。
- 第二部分演示如何创建简单的 JSP"Hello World"将代码部署到应用程序服务的应用程序，然后使用 FTP 客户端。


## <a name="prerequisites"></a>系统必备组件

### <a name="software-installations"></a>软件安装

在这篇文章中的 AzureWebDemo 应用程序代码编写使用 Azure Java SDK 0.7.0，您可以安装使用[Web 平台安装程序][](WebPI)。 此外，请确保使用[Eclipse 的 Azure Toolkit][]的最新版本。 安装 SDK 后，通过运行**Maven 存储库**中，**更新索引**更新您的 Eclipse 项目中的依赖关系，然后重新添加最新版本的**依赖项**窗口中的每个文件包。 您可以通过单击验证在 Eclipse 中已安装的软件的版本**帮助 > 安装详细信息**;您应该至少具有以下版本︰

- 对于 Java 0.7.0.20150309 Microsoft Azure 库包
- Eclipse IDE 的 Java EE 开发 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>创建和配置在 Azure 的云资源

在开始此过程之前，您需要有有效的 Azure 订阅设置的默认活动目录 (AD) 在 Azure 上。


### <a name="create-an-active-directory-ad-in-azure"></a>在 Azure 创建活动目录 (AD)

如果您已经没有 Azure 订购的活动目录 (AD)，登录到[Azure 的传统门户网站][]与您的 Microsoft 帐户。 如果您有多个订阅，单击**订阅**，选择要用于此项目的订阅的默认目录。 然后单击**应用**以切换到该订阅视图。

1. 从左侧菜单中选择**活动目录**。 **新单击 > 目录 > 创建自定义**。

2. 在**添加目录**中，选择**创建新目录**。

3. 在**名称**框中，输入目录名称。

4. 在**域**中输入域名称。 这是一个基本的域名包含默认情况下，与您的目录;它具有形式`<domain_name>.onmicrosoft.com`。 您可以命名它基于目录名称或您所拥有的另一个域名。 稍后，您可以添加您的组织已经使用的另一个域名。

5. 在**国家或区域**中，选择您的区域设置。

在广告上的详细信息，请参阅[什么是 Azure 的广告目录][]吗？


### <a name="create-a-management-certificate-for-azure"></a>为 Azure 创建管理证书

Azure SDK 的 Java 使用管理证书进行身份验证使用 Azure 的预订。 这些是 X.509 v3 证书用于身份验证的客户端应用程序服务管理 API 用于代表预订所有者管理预订资源。

在此过程中的代码使用自签名的证书进行身份验证使用 Azure。 对于此过程，您需要创建一个证书并将其上载到[Azure 的传统门户网站][]事先。 这包括以下步骤︰

- 生成表示您的客户证书 PFX 文件并将其保存在本地。
- 从 PFX 文件中生成的管理证书 （CER 文件）。
- 将 CER 文件上载到 Azure 订购。
- PFX 文件转换 JKS，，因为 Java 使用该格式以使用证书进行身份验证。
- 编写应用程序的身份验证代码，它指的是本地 JKS 文件。

当您完成此过程时，CER 证书将驻留在 Azure 订购和 JKS 证书将驻留在您的本地驱动器上。 有关管理证书的详细信息，请参阅[创建并上载 Azure 管理证书][]。


#### <a name="create-a-certificate"></a>创建证书

若要创建您自己的自签名的证书，打开一个命令控制台，在您的操作系统上，运行以下命令。

> **注意︰** 运行此命令的计算机必须安装了 JDK。 此外，通往 keytool 取决于 JDK 安装在其中的位置。 有关详细信息，请参见 Java 联机文档中的[密钥和证书管理工具 (keytool)][] 。

要创建的.pfx 文件︰

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

若要创建此.cer 文件︰

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

其中︰

- `<java-install-dir>`是 Java 的安装目录的路径。
- `<keystore-id>`为密钥库的条目标识符 (例如， `AzureRemoteAccess`)。
- `<cert-store-dir>`是您要在其中存储证书的目录的路径 (例如`C:/Certificates`)。
- `<cert-file-name>`证书文件的名称 (例如`AzureWebDemoCert`)。
- `<password>`是选择保护证书; 密码它必须是长度至少为 6 个字符。 您可以输入任何密码，尽管不建议这样做。
- `<dname>`要与别名相关联的 X.500 可分辨名称，用作中的自签名证书的颁发者和主题字段。

有关详细信息，请参阅[创建并上载 Azure 管理证书][]。


#### <a name="upload-the-certificate"></a>将上载证书

要上载到 Azure 的自签名的证书，转到了经典的门户，在**设置**页，然后单击**管理证书**选项卡。 单击页面底部的**上载**并导航到您创建的 CER 文件的位置。


#### <a name="convert-the-pfx-file-into-jks"></a>PFX 文件转换 JKS

（以管理员身份运行），请在 Windows 命令提示符 cd 目录包含证书并运行以下命令，其中`<java-install-dir>`是在您的计算机安装 Java 的目录︰

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. 出现提示时，输入目标密钥库口令;这将是 JKS 文件的密码。

2. 出现提示时，输入源密钥库口令;这是您指定的 PFX 文件密码。

两个密码不需要是相同的。 您可以输入任何密码，尽管不建议这样做。


## <a name="build-a-web-app-creation-application"></a>构建 Web 应用程序创建应用程序

### <a name="create-the-eclipse-workspace-and-maven-project"></a>创建 Eclipse 工作区和 Maven 项目

在这一节中创建工作区和 web 应用程序的创建应用程序，名为 AzureWebDemo 的 Maven 项目。

1. 创建一个新的 Maven 项目。 单击**文件 > 新建 > Maven 项目**。 在**新的 Maven 项目**中，选择**创建一个简单的项目**，并**使用默认工作区位置**。

2. 在**新的 Maven 项目**的第二页上，指定以下项︰

    - 组 ID:`com.<username>.azure.webdemo`
    - 项目 ID: AzureWebDemo
    - 版本︰ 0.0.1-SNAPSHOT
    - 包装︰ jar
    - 名称︰ AzureWebDemo

    单击**完成**。

3. 在项目资源管理器中打开新的项目 pom.xml 文件。 选择**依赖项**选项卡。 因为这是一个新的项目，尚未列出不包。

4. 打开 Maven 存储库视图。 **单击窗口 > 显示视图 > 其他 > Maven > Maven 存储库**，然后单击**确定**。 **Maven 存储库**视图将显示在 IDE 的底部。

5. 打开**全局资料库**、**中央**存储库中，用鼠标右键单击并选择**重新生成索引**。

    ![][1]
    
    此步骤可能需要几分钟时间，具体取决于您的连接速度。 当重新生成索引时，您应该看到**中央**Maven 存储库中的 Microsoft Azure 包。

6. 在**依赖项**中，单击**添加**。 **输入组 id。.**在输入`azure-management`。 选择为基础的管理和应用程序服务 Web 应用程序管理包︰

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **注意︰**如果要更新依赖项新版本发布后，您需要重新添加每个在此列表中的依赖项。
    > 单击**添加**，然后选择每个依赖项之后，它将显示与**依存关系**列表中的新版本号。

单击**确定**。 Azure 的包然后出现在**依赖项**列表中。


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>编写 Java 代码来调用 Azure SDK 来创建一个 Web 应用程序

接下来，编写 Java 的 Azure SDK 来创建应用程序服务 web 应用程序中调用 Api 的代码。

1. 创建包含主入口点代码的 Java 类。 在项目资源管理器中，右键单击项目节点，然后选择**新建 > 类**。

2. 在**新的 Java 类**中，类命名`WebCreator`，并检查**公共静态 void 主**复选框。 选择应如下所示︰

    ![][2]

3. 单击**完成**。 WebCreator.java 文件出现在项目资源管理器中。


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>调用的 Azure 的 API 来创建应用程序服务 Web 应用程序


#### <a name="add-necessary-imports"></a>添加所需的导入

在 WebCreator.java 中，添加下面的导入;这些导入提供对使用 Azure Api 管理库中的类的访问︰

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>定义的主入口点类

由于 AzureWebDemo 应用程序的目的是创建一个应用程序服务 Web 应用程序，此应用程序命名的主类`WebAppCreator`。 此类提供调用 Azure 服务管理 API 来创建 web 应用程序的主入口点代码。

添加对 web 应用程序和 webspace 下面的参数定义。 您需要提供您自己的 Azure 的订阅 ID 和证书信息。

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

其中︰

- `<subscription-id>`是您要在其中创建资源的 Azure 的订阅 ID。
- `<certificate-store-path>`是的路径和文件名 JKS 文件在您的本地证书存储区目录。 例如， `C:/Certificates/CertificateName.jks` linux 和`C:\Certificates\CertificateName.jks`的窗口。
- `<certificate-password>`是您在创建 JKS 证书时指定的密码。
- `webAppName`可以是任何名称选择;此过程使用名称`WebDemoWebApp`。 完整的域名是`webAppName`与`domainName`追加，因此在这种情况下的全域是`webdemowebapp.azurewebsites.net`。
- `domainName`如上所示，应指定。
- `webSpaceName`应在[WebSpaceNames][]类中定义的值之一。
- `appServicePlanName`如上所示，应指定。

> **注意︰**每次运行此应用程序时，您需要更改的值`webAppName`和`appServicePlanName`（或删除在 Azure 门户 web 应用程序） 运行一次应用程序之前。 否则，执行将失败，因为在 Azure 上已存在相同的资源。


#### <a name="define-the-web-creation-method"></a>定义 web 创建方法

接下来，定义一个方法来创建 web 应用程序。 这种方法， `createWebApp`，指定的 web 应用程序和 webspace 的参数。 它还创建并配置应用程序服务 Web 应用程序管理客户端，它由[WebSiteManagementClient][]对象。 管理客户端将创建 Web 应用程序的关键。 它提供了 rest 风格 web 服务允许应用程序通过调用服务管理 API 来管理 web 应用程序 （执行操作，如创建、 更新和删除）。

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

将输出指示成功或失败，响应的 HTTP 状态代码，并如果成功，将输出创建的 web 应用程序的名称。


#### <a name="define-the-main-method"></a>定义的 main （） 方法

提供调用 createWebApp() 来创建 web 应用程序的 main （） 方法代码。

最后，调用`createWebApp`从`main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>运行应用程序并验证 web 应用程序创建

若要验证您的应用程序运行，请单击**运行 > 运行**。 当应用程序完成运行时，您应看到下面的输出在 Eclipse 控制台︰

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

登录到 Azure 的传统门户网站，请单击**Web 应用程序**。 在几分钟内，新的 web 应用程序应显示在 Web 应用程序列表。


## <a name="deploying-an-application-to-the-web-app"></a>应用程序部署到 Web 应用程序

您运行 AzureWebDemo 并创建新的 web 应用程序之后，登录到经典的门户，单击**Web 应用程序**，和**Web 应用程序**列表中选择**WebDemoWebApp** 。 在 web 应用程序的仪表板页面中，单击**浏览**(或单击 URL `webdemowebapp.azurewebsites.net`) 以导航到它。 您将看到空占位符页面，因为没有内容已发布到 web 应用程序尚未。

接下来您将创建一个"Hello World"应用程序并将其部署到 web 应用程序。


### <a name="create-a-jsp-hello-world-application"></a>创建一个 JSP Hello World 的应用程序

#### <a name="create-the-application"></a>创建应用程序

为了演示如何部署到 web 应用程序，下面的过程演示如何创建一个简单的"Hello World"Java 应用程序并将其上载到您的应用程序创建的应用程序服务 Web 应用程序

1. 单击**文件 > 新建 > 动态 Web 项目**。 将它命名为`JSPHello`。 您不需要更改此对话框中的任何其他设置。 单击**完成**。

    ![][3]

2. 在项目资源管理器中展开的**JSPHello**项目，用鼠标右键单击**网站**，然后单击**新建 > JSP 文件**。 在新的 JSP 文件对话框中，命名新文件`index.jsp`。 单击**下一步**。

3. 在**选择 JSP 模板**对话框中，选择**新的 JSP 文件 (html)** ，然后单击**完成**。

4. 在 index.jsp，添加下面的代码`<head>`和`<body>`标记的部分︰

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>在本地主机中运行的 Hello World 应用程序

在运行此应用程序之前，您需要配置几个属性。

1. 右击**JSPHello**项目并选择**属性**。

2. 在**属性**对话框︰ 选择**Java 构建路径**，选择**订单和导出**选项卡，检查**JRE 系统库**中，然后单击**向上**以将其移动到列表的顶部。

    ![][4]

3. 此外会在**属性**对话框︰ 选择**目标运行时**，单击**新建**。

4. 在**新服务器运行时环境**对话框中，选择如**Apache Tomcat 7.0 版**的服务器，然后单击**下一步**。 在**Tomcat 服务器**对话框中，设置**名**为`Apache Tomcat v7.0`，并将**Tomcat 安装目录**设置为要使用 Tomcat 服务器的版本的安装的目录。

    ![][5]

    单击**完成**。

5. 然后返回到**属性**对话框的**目标运行库**页。 选择**Apache Tomcat 7.0 版**，然后单击**确定**。

    ![][6]

6. 在 Eclipse**运行**菜单中，单击**运行**。 在**运行方式**对话框中，选中**在服务器上运行**。 在**在服务器上运行**对话框中，选择**Tomcat 7.0 版的服务器**︰

    ![][7]

    单击**完成**。

7. 当应用程序运行时，您应该看到在 Eclipse 中的本地主机窗口中显示的**JSPHello**页面 (`http://localhost:8080/JSPHello/`)，显示下列消息︰

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>将应用程序导出为战争

将 web 项目文件导出为 web 档案 （战争） 文件，以便您可以将其部署到 web 应用程序。 以下 web 项目文件驻留在网站文件夹中︰

    META-INF
    WEB-INF
    index.jsp

1. 用鼠标右键单击网站文件夹并选择**导出**。

2. 在**选择导出**对话框中，单击**网站 > WAR**文件，然后单击**下一步**。

3. 在**战争导出**对话框中，选择当前项目中的 src 目录，包括末尾的 WAR 文件的名称。 例如︰

    `<project-path>/JSPHello/src/JSPHello.war`

有关部署 WAR 文件的详细信息，请参阅[添加到 Azure 应用程序服务 Web 应用程序的 Java 应用程序](web-sites-java-add-app.md)。


### <a name="deploying-the-hello-world-application-using-ftp"></a>部署使用 FTP Hello World 应用程序

选择第三方 FTP 客户端，若要发布的应用程序。 本过程描述了两个选项︰ Kudu 控制台内置到 Azure。和 FileZilla，具有方便的图形化用户界面的流行工具。

> **注意︰**Eclipse 的 Azure Toolkit 支持部署到存储帐户和云服务，但当前不支持对 web 应用程序的部署。 您可以部署于存储帐户和云服务使用 Azure 部署项目所述[创建 Eclipse 中的 Azure 的 Hello World 应用程序](http://msdn.microsoft.com/library/azure/hh690944.aspx)，但不适用于 web 应用程序。 使用 FTP 或 GitHub 等其他方法将文件传输到您的 web 应用程序。

> **注意︰**我们不建议使用 FTP 通过 Windows 命令提示符 （命令行 FTP.EXE 实用程序与 Windows 附带）。 FTP 客户端使用活动 FTP，如 FTP.EXE，通常无法通过防火墙工作。 活动 FTP 指定内部的基于 LAN 的地址，到该 FTP 服务器可能将不能连接。

为应用程序服务 web 应用程序使用 FTP 部署的详细信息，请参阅下列主题︰

- [使用 FTP 实用程序部署](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>设置部署凭据

请确保您运行**AzureWebDemo**应用程序创建 web 应用程序。 会将文件转移到此位置。

1. 登录到经典的门户，单击**Web 应用程序**。 确保**WebDemoWebApp**出现在列表中的 web 应用程序，并确保其正在运行。 单击**WebDemoWebApp**以打开其**仪表板**页。

2. 在**仪表板**页下**快速粗略地看**，单击**设置部署凭据**（如果已部署的凭据，这读取**重置您部署的凭据**）。

    部署的凭据会与 Microsoft 帐户相关联。 您需要指定用户名和密码，您可以使用部署使用 Git 和 FTP。 您可以使用这些凭据将部署到任何 web 应用程序中与您的 Microsoft 帐户相关联的所有 Azure 订阅。 提供在对话框的 Git 和 FTP 部署凭据并记录的用户名和密码以备将来使用。


#### <a name="get-ftp-connection-information"></a>获取 FTP 连接信息

若要使用 FTP 部署到新创建的 web 应用程序的应用程序文件，您需要获取连接信息。 有两种方法来获取连接的信息。 一种方法是访问 web 应用程序的**仪表板**页面;另一种方法是下载 web 应用程序的发布配置文件。 发布配置文件是一个 XML 文件，在 Azure 应用程序服务 web 应用程序提供 FTP 主机名称和登录凭据之类的信息。 您可以使用此用户名和密码以将部署到任何 web 应用程序中使用 Azure 帐户，不只有这一关联的所有订阅。

从[Azure 门户网站][]中的 web 应用程序的刀片式服务器获取 FTP 连接信息︰

1. **精要**在找到并复制**FTP 主机名称**。 这是类似于一个 URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`。

2. **精要**在找到并**复制/部署 FTP 用户名**。 这将使用窗体的*webappname\deployment 用户名*;例如`WebDemoWebApp\deployer77`。

从发布配置文件获取 FTP 连接信息︰

1. 在 web 应用程序的刀片式服务器，单击**获取发布配置文件**。 这将下载.publishsettings 文件到您的本地驱动器。

2. 在 XML 编辑器或文本编辑器中打开.publishsettings 文件并查找`<publishProfile>`元素，其中包含`publishMethod="FTP"`。 其外观类似如下︰

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. 请注意，web 应用程序的`publishProfile`设置映射到的 FileZilla 站点管理器设置，如下所示︰

- `publishUrl`为**FTP 主机名称**，**主机**中设置的值相同。
- `publishMethod="FTP"`意味着将**协议**设置为**FTP 的文件传输协议**，并对**使用普通 FTP**进行**加密**。
- `userName`和`userPWD`被用于实际的用户名和密码值重置部署凭据时指定快捷键。 `userName`等同于**部署 FTP 用户 /**。 它们将映射到**用户**和**密码**在 FileZilla 中。
- `ftpPassiveMode="True"`表示 FTP 站点使用被动 FTP 传输;在**传送设置**选项卡上，选择**被动**。


#### <a name="configure-the-web-app-to-host-a-java-application"></a>配置 Web 应用程序来承载一个 Java 应用程序

在发布应用程序之前，您需要更改几项配置设置，以便使 web 应用程序可以承载一个 Java 应用程序。

1. 在经典的门户中，转到 web 应用程序的**仪表板**页面，单击**配置**。 在**配置**页中，指定下列设置。

2. 在**Java 版本**的默认设置是**关闭**;选择的 Java 版本应用程序的目标;例如 1.7.0_51。 执行此操作后，还要确保**Web 容器**将 Tomcat 服务器的版本。

3. 在**默认的文档**中，添加 index.jsp 并将其移动到列表的顶部。 （web 应用程序的默认文件是 hostingstart.html）。

4. 单击**保存**。


#### <a name="publish-your-application-using-kudu"></a>发布您的应用程序使用 Kudu

发布应用程序的一种方法是使用内置到 Azure Kudu 调试控制台。 Kudu 已知为稳定并且与应用程序服务 Web 应用程序和 Tomcat 服务器一致。 您可以通过浏览到以下形式的 URL 访问 web 应用程序的控制台︰

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. 在此过程中，Kudu 控制台位于下面的 URL;浏览至该位置︰

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. 从顶部的菜单中，选择**调试控制台 > CMD**。

3. 在控制台命令行定位到`/site/wwwroot`(或单击`site`，然后`wwwroot`目录视图中的页面的顶部):

    `cd /site/wwwroot`

4. 指定**Java 版本**后，Tomcat 服务器应创建 webapps 目录。 在控制台命令行定位到 webapps 目录︰

    `mkdir webapps`

    `cd webapps`

5. 将从 JSPHello.war`<project-path>/JSPHello/src/`并将其放置到 Kudu 目录视图下`/site/wwwroot/webapps`。 不要拖动它到"拖动此处上载和压缩"区域中，因为 Tomcat 将将其解压缩。

  ![][8]

在第一个 JSPHello.war 区域中会出现目录本身︰

  ![][9]

在短的时间 （可能不超过 5 分钟） Tomcat 服务器将到 JSPHello 目录中解包解压缩 WAR 文件。 单击根目录是否有 index.jsp 解压缩并复制到此处。 如果是这样，导航回 webapps 目录以查看是否已创建解包的 JSPHello 目录。 如果看不到这些项目，请等待并重复。

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>发布您的应用程序使用 FileZilla （可选）

可用于发布应用程序的另一个工具是 FileZilla，使用方便的图形化用户界面受欢迎第三方 FTP 客户端。 您可以从下载并安装 FileZilla [http://filezilla-project.org/](http://filezilla-project.org/)如果您没有它。 使用客户端的详细信息，请参阅[FileZilla 文档](https://wiki.filezilla-project.org/Documentation)和此博客上[FTP 客户端-4 部分︰ FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx)。

1. 在 FileZilla，单击**文件 > 站点管理器**。
2. 在**站点管理器**对话框中，单击**新站点**。 一个新的空白 FTP 网站将出现在**选择项**提示您提供一个名称。 在此过程中，其命名为`AzureWebDemo-FTP`。

    在**常规**选项卡上指定下列设置︰
    - **主机︰**输入从仪表板中复制的**FTP 主机名称**。
    - **端口︰**（让此项保留为空，因为这是被动转移，服务器将确定要使用的端口。
    - **协议︰**FTP 文件传输协议
    - **加密︰**使用普通 FTP
    - **登录类型︰**普通
    - **用户︰**输入部署 / FTP 复制仪表板的用户。 这是完整的 FTP 用户名具有窗体*webappname\username*。
    - **密码︰**输入设置部署凭据时所指定的密码。

    在**传送设置**选项卡，选择**被动**。

3. 单击**连接**。 如果成功，FileZilla 的控制台将显示`Status: Connected`消息和问题`LIST`命令来列出目录内容。

4. 在**本地**的站点面板中，选择源目录中的 JSPHello.war 文件所在;路径将类似如下︰

    `<project-path>/JSPHello/src/`

5. 在**远程**站点面板中，选择目标文件夹。 将部署 WAR 文件为`webapps`的 web 应用程序根目录下的目录。 定位到`/site/wwwroot`，用鼠标右键单击`wwwroot`，然后选择**创建的目录**。 命名目录`webapps`，然后输入该目录。

6. 转移到 JSPHello.war `/site/wwwroot/webapps`。 **本地**文件列表中选择 JSPHello.war、 右键单击它并选择**上载**。 您应看到它在`/site/wwwroot/webapps`。

7. 将 JSPHello.war 复制到 webapps 目录后，Tomcat 服务器将自动打开包装 （解压缩） WAR 文件中的文件。 虽然 Tomcat 服务器开始几乎立即打开包装，它可能会花费较长时间 （可能是小时） 在 FTP 客户端中显示的文件。


#### <a name="run-the-hello-world-application-on-the-web-app"></a>在 Web 应用程序上运行的 Hello World 应用程序

1. 您上载的 WAR 文件并验证 Tomcat 服务器创建了解压缩后`JSPHello`目录中，浏览到`http://webdemowebapp.azurewebsites.net/JSPHello`以运行该应用程序。

    > **注意︰**如果您单击**浏览**从传统的门户网站，您可能会收到该默认网页，说:"此基于 Java 的 web 应用程序已成功创建。" 您可能需要刷新网页以查看应用程序的输出而不是默认网页。

2. 当应用程序运行时，您应看到下面的输出与 web 页︰

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>清理 Azure 的资源

此过程创建的应用程序服务 web 应用程序。 只要它存在，将记入该资源。 除非您打算继续使用 web 应用程序，用于测试或开发时，应考虑停止或将其删除。 已停止 web 应用程序仍将会对较小的费用，但您可以重新启动它在任何时候。 删除 web 应用程序会删除所有已上载到它的数据。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure 应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web 平台安装程序]: http://go.microsoft.com/fwlink/?LinkID=252838
[日蚀式的 azure Toolkit]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure 的传统门户网站]: https://manage.windowsazure.com
[什么是 Azure 的广告目录]: http://technet.microsoft.com/library/jj573650.aspx
[创建并上载的 Azure 管理证书]: ../cloud-services/cloud-services-certs-create.md
[密钥和证书管理工具 (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure 门户]: https://portal.azure.com
