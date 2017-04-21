<properties
    pageTitle="在 Azure 应用程序服务创建一个 PHP MySQL web 应用程序和部署使用 Git"
    description="演示如何创建一个 PHP web 应用程序将数据存储在 MySQL 和使用 Git 部署到 Azure 的教程。"
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>在 Azure 应用程序服务创建一个 PHP MySQL web 应用程序和部署使用 Git

本教程介绍如何创建一个 PHP MySQL web 应用程序以及如何将其部署到[应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)使用 Git。 您将使用[PHP][install-php]，MySQL 命令行工具 (属于[MySQL][install-mysql])，和[Git] [install-git]安装在您的计算机上。 在本教程中的说明进行操作的后面可以在任何操作系统，包括 Windows、 Mac 和 Linux 上。 在完成本指南，您必须在 Azure 上运行 PHP/MySQL web 应用程序。

您将了解︰

* 如何创建 web 应用程序和 MySQL 数据库使用[Azure 门户网站][management-portal]。 由于默认情况下[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)中启用了 PHP，什么特别的事情需要运行您的 PHP 代码。
* 如何发布和重新发布您的应用程序使用 Git 的 Azure。
* 如何启用自动作曲家作曲者扩展任务在每个`git push`。

通过遵循本教程中，您将生成 PHP 中一个简单的注册 web 应用程序。 将 Web 应用程序中承载的应用程序。 下面是完整的应用程序的屏幕快照︰

![Azure 的 PHP 网站][running-app]

## <a name="set-up-the-development-environment"></a>设置开发环境

本教程假设您拥有[PHP][install-php]，MySQL 命令行工具 (属于[MySQL][install-mysql])，和[Git] [install-git]安装在您的计算机上。

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>创建 web 应用程序和设置 Git 发布

请按照下列步骤来创建一个 web 应用程序和 MySQL 数据库︰

1. 登录到[Azure 的门户网站][management-portal]。
2. 单击**新建**图标。

3. 单击**查看所有****市场**旁。 

4. 请单击**Web + 移动**，然后**Web 应用程序 + MySQL**。 然后，单击**创建**。

4. 输入资源组的有效名称。

    ![设置资源组的名称][resource-group]

5. 输入您的新 web 应用程序的值。

    ![创建 web 应用程序][new-web-app]

6. 输入新的数据库，其中包括同意的法律条款的值。

    ![创建新的 MySQL 数据库][new-mysql-db]

7. 创建 web 应用程序后，您将看到新的 web 应用程序刀片式服务器。

7. 在**设置****连续部署**，依次单击_配置所需的设置_。

    ![设置 Git 发布][setup-publishing]

8. 为源选择**本地 Git 存储库**。

    ![设置 Git 存储库][setup-repository]


9. 为了让 Git 发布，您必须提供用户名和密码。 记下的用户名和密码创建。 （如果您已经设置了一个 Git 存储库之前，将会跳过此步骤。）

    ![创建发布的凭据][credentials]


## <a name="get-remote-mysql-connection-information"></a>获取远程 MySQL 连接信息

若要连接到 MySQL 数据库的 Web 应用程序，您将在运行时需要的连接信息。 若要获取 MySQL 连接信息，请执行以下步骤︰

1. 从资源组中，单击数据库:

    ![选择数据库][select-database]

2. 从数据库**设置**，选择**属性**。

    ![选择属性][select-properties]

2. 请记下的值`Database`， `Host`， `User Id`，和`Password`。

    ![注释属性][note-properties]

## <a name="build-and-test-your-app-locally"></a>生成和测试您的应用程序本地

既然您已创建一个 web 应用程序，您可以开发本地应用程序，然后测试完成后对其进行部署。

注册应用程序是一个简单的 PHP 应用程序，允许您注册为事件提供您的姓名和电子邮件地址。 以前登记的信息将显示在表中。 注册信息存储在一个 MySQL 数据库中。 该应用程序包含一个文件 （使用下面的复制/粘贴代码）︰

* **index.php**︰ 显示注册和包含注册人信息表的窗体。

要生成并在本地运行应用程序，请按照下面的步骤。 注意这些步骤假定您有 PHP 和 MySQL 命令行工具 （MySQL 的一部分） 在您的本地计算机上设置，并启用了[MySQL 的 PDO 扩展][pdo-mysql]。

1. 连接到远程 MySQL 服务器，使用的值为`Data Source`， `User Id`， `Password`，和`Database`您检索到更早版本︰

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. MySQL 命令提示符将显示︰

        mysql>

3. 在下面的示例粘贴`CREATE TABLE`命令创建`registration_tbl`数据库中的表︰

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. 在本地应用程序文件夹的根目录创建**index.php**文件。

5. 在文本编辑器或 IDE 中打开**index.php**文件中添加以下代码，和完成所需的更改标记为`//TODO:`的注释。


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  在终端转到应用程序文件夹，并键入下面的命令︰

        php -S localhost:8000

现在，您可以浏览到**http://localhost:8000 /**测试应用程序。


## <a name="publish-your-app"></a>发布您的应用程序

在测试您的应用程序本地后，可以将其发布到 Web 应用程序使用 Git 中。 初始化本地 Git 存储库，并将发布应用程序。

> [AZURE.NOTE]
> 这些是显示在最上面的 Git 发布部分将创建一个 web 应用程序和设置末尾 Azure 门户中的相同步骤。

1. （可选） 如果您已经忘记或者 Git 远程 repostitory URL 放错了地方，导航到 Azure 门户的 web 应用程序属性。

1. 打开 GitBash (或终端，Git 是否在您`PATH`)，将目录更改到根目录下的应用程序，并运行下面的命令︰

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    您将被提示输入您在前面创建的密码。

    ![通过 Git 的初始推到 Azure][git-initial-push]

2. 浏览到**http://[site name].azurewebsites.net/index.php**开始使用应用程序 （此信息将存储在您的帐户的仪表板）︰

    ![Azure 的 PHP 网站][running-app]

发布您的应用程序后，可以开始对其进行更改，并使用 Git 发布它们。

## <a name="publish-changes-to-your-app"></a>将更改发布到您的应用程序

若要将更改发布到您的应用程序，请执行以下步骤︰

1. 为您的应用程序本地进行的更改。
2. 打开 GitBash (或终端，it Git 是您`PATH`)，将目录更改到您的应用程序的根目录下运行以下命令︰

        git add .
        git commit -m "comment describing changes"
        git push azure master

    您将被提示输入您在前面创建的密码。

    ![通过 Git 推到 Azure 的站点更改][git-change-push]

3. 浏览到**http://[site name].azurewebsites.net/index.php**若要查看您的应用程序和您所做的任何更改︰

    ![Azure 的 PHP 网站][running-app]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>启用与作曲家扩展编辑器自动化

默认情况下，应用程序服务中的 git 部署过程不执行任何操作与 composer.json，如果您有一个在您的 PHP 项目。 您可以启用 composer.json 处理期间`git push`通过启用编辑器扩展。

1. 在您的 PHP web app 的刀片在[Azure 门户][management-portal]，单击**工具** > **扩展**。

    ![作曲者扩展插件设置][composer-extension-settings]

2. 单击**添加**，然后单击**编辑器**。

    ![添加编辑器扩展][composer-extension-add]
    
3. 单击**确定**以接受法律条款。 再次单击**确定**以添加该扩展。

    **已安装扩展**刀片式服务器现在将显示编辑器扩展。  
    ![作曲者扩展视图][composer-extension-view]
    
4. 现在，执行`git add`， `git commit`，和`git push`喜欢上一节中。 您现在将看到作曲家安装在 composer.json 中定义的依赖项。

    ![作曲者扩展成功][composer-extension-success]

## <a name="next-steps"></a>下一步行动

有关详细信息，请参见[PHP 开发人员中心](/develop/php/)。

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
