<properties
  pageTitle="为您的 web 应用程序有效地使用 DevOps 环境"
  description="了解如何部署插槽用于设置和管理多个应用程序的开发环境"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>对于 web 应用程序有效地使用 DevOps 环境

本文介绍您可以设置和管理 web 应用程序部署多个版本的应用程序开发、 暂存，QA 和生产等。 每个版本的应用程序可以被认为是开发环境的部署过程中的具体需要。 例如 QA 环境可以使用您的开发人员的团队，以将更改推送到生产环境之前测试该应用程序的质量。
设置多个开发环境可以是具有挑战性的任务，以跟踪、 管理资源 （计算 web 应用程序、 数据库、 缓存等） 和在环境中部署代码所需。

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>设置非生产环境 （开发、 QA 阶段）
生产 web 应用程序启动并运行之后下, 一步是创建一个非生产环境。 若要使用部署插槽请确保在**标准**或**高级**应用程序服务计划模式运行。 部署插槽均为实际的 web 应用程序使用它们自己的主机名。 Web 应用程序内容和配置元素可以部署两个插槽，包括生产插槽之间交换。 部署应用程序的部署槽具有以下优点︰

1. 您可以换生产插槽前验证临时部署插槽中的 web 应用程序更改。
2. 首次部署 web 应用程序到插槽和换成生产以确保插槽的所有实例都取暖被交换到生产环境之前。 在部署 web 应用程序时，这消除了停机时间。 通信流重定向是紧密相连的并没有请求丢弃由于交换操作。 这整个工作流可以通过配置[自动交换](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app)不需要预先交换验证时自动执行。
3. 换用后，用以前分阶段的 web 应用程序现在有以前的生产 web 应用程序。 如果换到生产插槽的更改未按预期的那样，您可以执行相同的交换，立即以获取"最后一个已知完好的 web 应用程序"返回。

若要设置一个临时部署插槽，请参阅[设置临时在 Azure 应用程序服务 web 应用程序的环境](web-sites-staged-publishing.md)。 每一种环境应包括它自己的资源，集示例如果您的 web 应用程序使用数据库然后生产和临时的 web 应用程序应该使用不同的数据库。 添加临时开发环境资源，如数据库、 存储或缓存设置临时开发环境。

## <a name="examples-of-using-multiple-development-environments"></a>使用多个开发环境的示例

任何项目，应按照源代码管理使用至少两种环境，开发和生产环境，但当使用内容管理系统，等我们可能会遇到应用程序框架发出应用程序不支持这种情况下，开箱即用。 这适用于某些常用的框架下讨论。 使用如 CMS/框架时可以想出很多问题的

1. 如何将其缩小分割为不同的环境
2. 哪些文件可以更改并希望影响框架版本更新
3. 如何管理每个环境配置
4. 如何管理模块/插件版本更新、 核心框架版本更新

有许多方法，可以设置多个环境项目，下面的示例是一个只有一个此类方法各自的应用程序。

### <a name="wordpress"></a>WordPress
在本节中，您将学习如何设置使用的 WordPress 的插槽的部署工作流。 像大多数的 CMS 解决方案的 WordPress 不支持与开箱即用的多个开发环境的工作。 应用程序服务 Web 应用程序有一些更加轻松地将存储在您的代码之外的配置设置的功能。

创建一个临时的插槽之前, 设置应用程序代码以支持多个环境。 若要在您需要编辑的 WordPress 支持多个环境`wp-config.php`在您的本地开发 web 应用程序在文件的开头添加以下代码。 这将允许您的应用程序选择正确的配置，根据选定的环境。

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

创建名为的 web 应用程序根目录下的文件夹`config`并添加第二个文件︰ `wp-config.azure.php` ，`wp-config.local.php`分别代表 azure 和本地环境。

复制下列文本中的`wp-config.local.php`:

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

设置安全参数上面可以帮助防止您的 web 应用程序被黑客攻击，因此，使用唯一的值。 如果您需要生成上面提到的安全密钥的字符串，您可以转到自动生成器以创建新的键/值使用此 [链接] (https://api.wordpress.org/secret-key/1.1/salt)

复制下面的代码在`wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>使用相对路径
最后一项任务是配置 WordPress app 使用相对路径。 WordPress 数据库中存储的 URL 的信息。 这使得移动内容从一个环境到另一个更困难，因为您需要更新数据库，每当您从本地移动到舞台或舞台到生产环境。 若要减少可导致与部署数据库，每当您从一个环境部署到另一个问题的风险使用[根目录相对链接插件](https://wordpress.org/plugins/root-relative-urls/)可以使用 WordPress 管理员仪表板安装或手动从[此处](https://downloads.wordpress.org/plugin/root-relative-urls.zip)下载它。


添加对以下项您`wp-config.php`文件，然后`That's all, stop editing!`注释︰

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

激活该插件通过`Plugins`在 WordPress 管理员仪表板中的菜单。 将固定链接的 WordPress 的应用程序设置保存。

#### <a name="the-final-wp-configphp-file"></a>最终`wp-config.php`文件
WordPress 核心的任何更新将不会影响您`wp-config.php`， `wp-config.azure.php` ，`wp-config.local.php`文件。 最终该如何`wp-config.php`文件将如下所示

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>设置临时环境
假设您已在 Azure 网站，登录到[Azure 管理预览门户网站](http://portal.azure.com)上运行一个 WordPress web 应用程序，然后转到 WordPress 的 web 应用程序。 如果应用程序不是您可以创建一个从市场。 若要了解更多信息，[请单击此处](web-sites-php-web-site-gallery.md)。
单击设置-> 部署插槽-> 添加以创建部署插槽与名称阶段。部署插槽是另一个 web 应用程序共享相同的资源上面创建主 web 应用程序。

![创建阶段部署插槽](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

添加另一个 MySQL 数据库，说`wordpress-stage-db`给资源组`wordpressapp-group`。

 ![MySQL 数据库添加到资源组](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

更新您的阶段部署槽，以指向新创建的数据库的连接字符串`wordpress-stage-db`。 注意您的生产 web 应用程序中，`wordpressprodapp`和临时的 web 应用程序`wordpressprodapp-stage`必须指向不同的数据库。

#### <a name="configure-environment-specific-app-settings"></a>配置特定于环境的应用程序设置
开发人员可以用与调用的应用程序设置一个 web 应用程序关联的配置信息的一部分 Azure 存储键-值字符串对。 在运行时，应用程序服务 Web 应用程序自动检索这些值并将它们提供给您的 web 应用程序中运行的代码。 从安全角度来看是好的一面受益后敏感信息如数据库连接字符串的密码永远不会显示为文件中的明文形式如`wp-config.php`。

因为它包括文件更改和数据库更改的 WordPress 的应用程序执行时，下面的定义此过程很有用︰
- WordPress 版本升级
- 添加新的或编辑或升级插件
- 添加新的或编辑或升级主题

配置应用程序的设置︰

- 数据库信息
- 打开/关闭 WordPress 日志记录
- WordPress 安全设置

![Wordpress 的 web 应用程序的应用程序设置](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

确保生产 web 应用程序和舞台插槽中添加下面的应用程序设置。 请注意，生产 web 应用程序和临时 web 应用程序使用不同的数据库。
取消选中除 WP_ENV 之外的所有设置参数**槽设置**复选框。 这将交换您的 web 应用程序，以及文件内容和数据库的配置。 **槽设置**为**选中**，如果 web 应用程序的应用程序设置和连接字符串配置将不会移动跨环境时执行替换操作，因此如果数据库的任何更改都存在这不会中断生产 web 应用程序。

部署阶段的 web 应用程序和数据库使用 WebMatrix 或工具，如 FTP、 Git 或 PhpMyAdmin 您所选择的本地开发环境的 web 应用程序。

![WordPress 的 web 应用程序的 web 发布列表对话框](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

浏览并测试您的临时 web 应用程序。 考虑到其中的 web 应用程序的主题是要更新的情况下，这是临时的 web 应用程序。

![在换用插槽前浏览临时 web 应用程序](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 如果所有看上去不错，请单击临时 web 应用程序要将内容移到生产环境中的**交换**按钮。 在这种情况下你在每个**交换**操作期间跨环境交换使 web 应用程序和数据库。

![WordPress 的交换预览更改](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >如果有情况︰ 您需要只有推送文件 （不是数据库的更新），然后**检查****槽设置**所有数据库相关的*应用程序设置*和 Azure 预览门户中的 web 应用程序设置刀片式服务器中的*连接字符串设置*进行交换之前。 在此案例 DB_NAME、 DB_HOST、 DB_PASSWORD、 DB_USER，默认连接字符串设置不应出现在预览更改进行**交换**时。 在这一次，当您完成**交换**操作 WordPress web 应用程序必须更新的文件**只**。

在做之前交换，这是生产 WordPress web 应用程序![生产 web 应用程序之前换用插槽](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

交换操作后，您的生产 web 应用程序已更新主题。

![换用插槽后生产 web 应用程序](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

在的情况下**回滚**，在需要时您可以转到生产 web 应用程序设置并单击**交换**按钮可供换用的 web 应用程序和数据库从生产到临时的插槽上。 需要记住的重要一点是，如果数据库更改包含在任意给定时间一个**交换**操作，然后重新部署到临时 web 应用程序部署数据库所需的下一个时间更改为临时 web 应用程序可能是以前的生产数据库或阶段数据库的当前数据库。

#### <a name="summary"></a>摘要
通用化的任何应用程序与数据库的过程

1. 在您的本地环境上安装应用程序
2. 包括环境特定的配置 (本地和 Azure Web 应用程序)
3. 设置您的应用程序服务 Web 应用程序 – 组配、 生产环境
4. 如果您已在 Azure 上运行的生产应用程序，同步到本地和暂存环境生产内容 （文件/代码 + 数据库）。
5. 开发您的应用程序在您的本地环境
6. 将内容从生产到临时和开发环境下维护或锁定的模式和同步数据库生产 web 应用程序
7. 部署到临时环境和测试
8. 部署到生产环境
9. 重复步骤 4 到 6

### <a name="umbraco"></a>Umbraco
在本节中，您将学习 Umbraco CMS 如何使用自定义模块来跨多 DevOps 环境中部署。 本示例将向您提供不同的方法来管理多个开发环境。

[Umbraco CMS](http://umbraco.com/)是 popular.NET CMS 许多开发人员使用的解决方案，这些解决方案提供了[Courier2](http://umbraco.com/products/more-add-ons/courier-2)模块，从开发到生产环境转移到部署之一。 您可以轻松创建使用 Visual Studio 或 WebMatrix Umbraco CMS web 应用程序的本地开发环境。

1. Umbraco web 应用程序创建 Visual Studio，[请单击此处](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)。
2. 若要创建 web 应用程序 Umbraco WebMatrix，[请单击此处](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix)。

始终记住去`install`文件夹在您的应用程序下，永远不会将其上载到舞台或生产 web 应用程序。 对于本教程，我将使用 WebMatrix

#### <a name="set-up-a-staging-environment"></a>设置测试环境
- 正如前面提到的 Umbraco CMS web 应用程序中，假设您已有的 Umbraco CMS web 应用程序和运行，请创建部署插槽。 如果不是您可以创建一个从市场。

- 更新为指向新创建的数据库， **umbraco-阶段-db**您阶段部署插槽的连接字符串。 您的生产 web 应用程序 (umbraositecms-1) 和临时 web 应用程序 （umbracositecms-1 的舞台）**必须**指向不同的数据库。

![更新过渡与新临时数据库的 web 应用程序的连接的字符串](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- 单击**获取发布设置**的部署槽**阶段**。 这将下载发布设置存储文件的 Visual Studio 或 Web 矩阵来发布您的应用程序从本地开发 web 应用程序到 Azure 的 web 应用程序所需的所有信息。

 ![获取发布临时的 web 应用程序的设置](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- **WebMatrix**或**Visual Studio**中打开您的本地开发 web 应用程序。 在本教程中我使用 Web 矩阵，首先您需要导入您临时的 web 应用程序的发布设置文件

![导入使用 Web 矩阵的 Umbraco 的发布设置](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- 查看在对话框中更改并将您的本地 web 应用程序部署到 Azure 的 web 应用程序， *umbracositecms-1 的阶段*。 直接向临时 web 应用程序部署的文件时将忽略中的任何文件`~/app_data/TEMP/`文件夹，当这些第一阶段的 web 应用程序时将重新生成开始。 此外应省略`~/app_data/umbraco.config`，为此，文件将重新生成。

![查看发布 web 矩阵中的更改](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- 之后成功发布到调试 web 应用程序的 Umbraco 本地 web 应用程序，浏览到您的临时 web 应用程序并运行一些测试，以排除任何问题。

#### <a name="set-up-courier2-deployment-module"></a>设置 Courier2 部署模块
用[Courier2](http://umbraco.com/products/more-add-ons/courier-2)模块可以推送的内容、 样式表、 开发模块，多以简单临时的 web 应用程序从右击到生产 web 应用程序更多的麻烦免费部署和减少破坏生产 web 应用程序在部署更新时的风险。
为域为 Courier2 购买许可证`*.azurewebsites.net`和您自定义的域 (比如 http://abc.com) 一旦购买了许可证，将下载的许可证 (。LIC 文件） 中`bin`文件夹。

![将许可证文件放在 bin 文件夹下](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

从[此处](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/)下载 Courier2 软件包。 登录阶段 web 应用程序，说 http://umbracocms-site-stage.azurewebsites.net/umbraco，然后单击**开发**菜单和选择**包**。 单击**安装**本地包

![Umbraco 包安装程序](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

使用安装程序的 courier2 包上传。

![上载的快递服务模块的包](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

要配置您需要更新您的 web 应用程序的**配置**文件夹下的 courier.config 文件。

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

在下， `<repositories>`，输入生产站点 URL 和用户信息。 如果您正在使用默认的 Umbraco 成员资格提供程序，然后添加中的管理用户 ID<user>部分。 如果您正在使用自定义的 Umbraco 成员资格提供程序，使用`<login>`，`<password>`到 Courier2 模块知道如何连接到生产站点。 有关详细信息，查看有关快递服务模块的[文档](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation)。

同样，快递服务模块安装在您的生产站点上并将其配置指向舞台 web 应用程序在其各自的 courier.config 文件如下所示

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

请单击 Courier2 Umbraco CMS web 应用程序的仪表板选项卡，然后选择位置。 您应该看到存储库名称，如中所述`courier.config`。在您的生产和调试 web 应用程序上执行此操作。

![查看目标 web 应用程序存储库](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

现在让我们将部署到生产站点从暂存站点的某些内容。 转到内容选择现有网页，或创建一个新页。 我将从我在页面的标题更改为**入门 – 新**的 web 应用程序中选择一个现有网页，现在单击**保存并发布**。

![更改页面的标题和发布](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

现在，选择修改的页上，然后*右键单击*以查看所有选项。 **快递服务**以查看部署对话框上单击。 单击以启动部署的**部署**

![快递服务模块部署对话框](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

查看所做更改，然后单击继续。

![快递服务模块部署对话框查看更改](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

部署日志显示部署是否成功。

 ![快递服务模块中查看部署日志](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

浏览您的生产 web 应用程序以查看所做的更改都会。

 ![浏览生产的 web 应用程序](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

若要了解有关如何使用快递服务的详细信息，请查看的文档。

#### <a name="how-to-upgrade-umbraco-cms-version"></a>如何升级 Umbraco CMS 版本

快递服务不有助于使用来自 Umbraco CMS 的一个版本升级到另一个部署。 当 Umbraco CMS 版本升级，则必须使用您的自定义模块或第三方模块和 Umbraco 核心库检查的不兼容性。 作为一种最佳做法

1. 始终在执行升级之前备份您的 web 应用程序和数据库。 在 Azure Web 应用程序中，您可以设置自动备份您的网站使用备份功能并还原您的网站需要使用恢复功能。 有关详细信息，请参阅[如何备份您的 web 应用程序](web-sites-backup.md)以及[如何将您的 web 应用程序的恢复](web-sites-restore.md)。

2. 检查是否正在使用第三方软件包与您要升级到的版本兼容。 在包的下载页面，查看与 Umbraco CMS 版本的项目兼容。

有关如何升级 web 应用程序本地的更多详细信息，为提到[这里](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general)中遵循的准则。

一旦升级本地开发站点，将更改发布到临时 web 应用程序。 测试您的应用程序，所有看起来很好，如果使用到**交换**网站转移到生产 web 应用程序**交换**按钮。 当执行**替换**操作，您可以查看所做的更改将影响 web 应用程序的配置中。 使用此**交换**操作时，我们要换的 web 应用程序和数据库。 这意味着，交换生产 web 应用程序将指向 umbraco 的阶段-db 数据库，并临时 web 应用程序都将指向 umbraco-生产-db 数据库之后。

![用于部署 Umbraco CMS 换预览](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

换用的 web 应用程序和数据库的优点︰
1. 为您提供了回滚到早期版本的 web 应用程序与另一个**换**，如果有任何应用程序问题的能力。
2. 升级需要部署文件和从临时生产 web 应用程序和数据库的 web 应用程序的数据库。 有许多事都部署文件和数据库时，会出现错误。 使用的插槽**交换**功能，我们可以减少升级期间的停机时间，并减少部署更改时，可能发生的故障的风险。
3. 使您能够执行**A / B 测试**使用[生产环境中的测试](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)功能

此示例演示了平台的灵活性可以生成自定义模块类似于 Umbraco 货运公司模块来管理环境中的部署。

## <a name="references"></a>引用
[敏捷软件开发与 Azure 应用程序服务](app-service-agile-software-development.md)

[设置临时在 Azure 应用程序服务 web 应用程序的环境](web-sites-staged-publishing.md)

[如何阻止网站访问非生产部署插槽](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
