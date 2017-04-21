<properties
    pageTitle="本地 Git 部署到 Azure 应用程序服务"
    description="了解如何启用本地 Git 部署到 Azure 应用程序服务。"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>本地 Git 部署到 Azure 应用程序服务

本教程展示了如何从本地计算机上的一个 Git 存储库中将您的应用程序部署到[Azure 应用程序服务]。 应用程序服务在[Azure 门户]支持**本地 Git**部署选项使用这种方法。  
在创建应用程序服务应用程序使用与所述[此处](app-service-web-get-started.md) [Azure 命令行界面]时，许多在这篇文章中所述的 Git 命令都会自动执行。

## <a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要︰

- Git。 您可以下载安装二进制[此处](http://www.git-scm.com/downloads)。  
- Git 的基本知识。
- 一种 Microsoft Azure 帐户。 如果您没有帐户，您可以[注册免费试用版](https://azure.microsoft.com/pricing/free-trial)或[激活您的 Visual Studio 的订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的起始应用程序的应用程序服务中。 没有信用卡，所需;没有承诺。  

## <a name="Step1"></a>步骤 1︰ 创建一个本地存储库

执行以下任务来创建一个新的 Git 存储库。

1. 启动一个命令行工具，如**GitBash** (Windows) 或**狂欢**（Unix 外壳）。 在 OS X 系统上可以访问命令行通过**终端**应用程序。

2. 导航到要部署的内容会所在的目录。

3. 使用下面的命令来初始化新的 Git 存储库︰

        git init

## <a name="Step2"></a>第 2 步︰ 提交您的内容

应用程序服务支持各种编程语言创建的应用程序。 

1. 如果存储库已包含内容跳过这点，并着手点下面的 2。 如果您的存储库中不包括的内容只是用填充静态.html 文件，如下所示︰ 

    - 使用文本编辑器，创建一个新文件，名为**index.html** Git 存储库中的根本
    - 添加下面的文本内容为 index.html 文件并将其另存为︰ *Hello Git ！*
        
2. 从命令行，请验证您已在您的 Git 存储库的根目录下。 然后，使用下面的命令将文件添加到存储库︰

        git add -A 

4. 接下来，将更改提交到存储库中使用下面的命令︰

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>第 3 步︰ 使应用程序服务的应用程序存储库

执行以下步骤来使您的应用程序服务的应用程序的 Git 存储库。

1. 登录到[Azure 的门户]。

2. 在您的应用程序服务的应用程序的刀片式服务器，请单击**设置 > 部署源代码**。 单击**选择源**，然后单击**本地 Git 存储库**，然后单击**确定**。  

    ![本地 Git 存储库](./media/app-service-deploy-local-git/local_git_selection.png)

3. 如果这是您第一次设置 Azure 中的存储库，您需要为其创建登录凭据。 您将使用它们从本地 Git 存储库登录到 Azure 存储库和强制更改。 从您的应用程序的刀片式服务器，请单击**设置 > 部署凭据**，然后配置部署用户名和密码。 当您完成时，单击**保存**。

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>步骤 4︰ 部署您的项目

使用以下步骤将您的应用程序发布到本地 Git 使用的应用程序服务。

1. 在您的应用程序在 Azure 门户的刀片式服务器，请单击**设置 > 属性** **Git URL**。

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Git URL**是从您的本地存储库中将部署到远程引用。 在以下步骤中，您将使用此 URL。

2. 使用命令行，请验证您是本地 Git 存储库的根目录中。

3. 使用`git remote`添加远程引用中列出的**Git URL**从第 1 步。 看起来类似于以下命令︰

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] **远程**命令添加到远程资源库中的已命名的引用。 在此示例中，它创建的引用名 azure 为您的 web 应用程序的存储库。

4. 将您的内容推送到使用新**azure**远程您刚才创建的应用程序服务。

        git push azure master

    系统将提示您重置 Azure 门户中的部署凭据时以前创建的密码。 输入的密码 （请注意，Gitbash 不会回显到控制台的星号键入您的密码）。 
       
5. 返回到您的应用程序在 Azure 门户。 您最新推的日志条目应显示在**部署**刀片式服务器。 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. 单击顶部的应用程序的刀片式服务器以验证内容已部署的**浏览**按钮。 
    
## <a name="Step5"></a>故障排除

以下是错误或使用 Git 发布到 Azure 中的应用程序服务的应用程序时通常会遇到的问题︰

****

**症状**︰ 无法访问 [siteURL]: 无法连接到 [scmAddress]

**原因**︰ 如果应用程序未启动并且正在运行，则可能发生此错误。

**解决方法**︰ 在 Azure 门户中启动该应用程序。 Git 部署将无法工作，除非该应用程序正在运行。 


****

**症状**︰ 无法解析主机主机名

**原因**︰ 如果输入时创建 azure 远程地址信息不正确，则可能发生此错误。

**解决方法**︰ 使用`git remote -v`命令来列出所有远程数据库，以及相关联的 URL。 验证 azure 远程的 URL 正确。 如果需要请删除并重新创建该远程使用正确的 URL。

****

**症状**︰ 没有引用常用和无指定;不执行任何操作。 也许您应该指定如母版的一个分支。

**原因**︰ 如果您没有指定分支执行 git 推入操作，并没有设置使用 Git 的 push.default 值时，会出现此错误。

**解析**︰ 推再次执行操作，指定主分支。 例如︰

    git push azure master

****

**症状**︰ src refspec [branchname] 任何不匹配。

**原因**︰ 如果您试图将推送到非主分支上 azure 远程，会出现此错误。

**解析**︰ 推再次执行操作，指定主分支。 例如︰

    git push azure master

****

**症状**︰ 错误的更改提交到远程资源库中，但不是会更新 web 应用程序。

**原因**︰ 如果您正在部署包含的 package.json 文件中指定所需的附加模块 Node.js 应用程序，则可能发生此错误。

**解析**︰ 其他消息包含 npm ERR ！ 应记录此错误之前，可以提供附加的上下文失败。 下面是已知的原因此错误和相应 npm ERR ！ 消息：

* **格式不正确的 package.json 文件**︰ npm 错误 ！ 无法读取的依赖关系。

* **不具有 Windows 二进制分配的本机模块**︰

    * npm 错误 ！ \`cmd"/c""节点 gyp 重建"\` 1 失败

        OR

    * npm 错误 ！ [modulename@version]预安装︰\`进行 | |gmake\`


## <a name="additional-resources"></a>其他资源

* [Git 文档](http://git-scm.com/documentation)
* [项目 Kudu 文档](https://github.com/projectkudu/kudu/wiki)
* [连续部署到 Azure 应用程序服务](app-service-continuous-deployment.md)
* [如何使用 Azure PowerShell](../powershell-install-configure.md)
* [如何使用 Azure 命令行界面](../xplat-cli-install.md)

[Azure 应用程序服务]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure 门户]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure 的命令行界面]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
