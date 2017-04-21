<properties
    pageTitle="在 Eclipse 中的 Azure 部署中启用远程访问"
    description="了解如何启用对 Eclipse 使用 Azure Toolkit Azure 部署的远程访问。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>在 Eclipse 中的 Azure 部署中启用远程访问

为了帮助解决您的部署，可能会启用并使用远程访问连接到承载您的部署的虚拟机。 远程访问功能依赖于远程桌面协议 (RDP)。 发布到 Azure，或如果您使用 Windows 操作系统使用 Eclipse，才能发布到 Azure，您可以配置远程访问后，您可以配置远程访问您的部署。 注意，您将需要远程桌面客户端才能连接到您的部署的虚拟机在 Azure 中是与您的操作系统兼容。

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>如何启用远程访问，部署到 Azure 之前

> [AZURE.NOTE] 要启用远程访问，部署到 Azure 应用程序之前，您需要在 Windows 上运行 Eclipse。

下图显示对话框用来启用远程访问的**远程访问**属性。

![][ic719494]

有两种方法可以显示**远程访问**属性对话框︰

* 单击**发布到 Azure**对话框的**远程访问**部分中的**高级**链接。
* 打开 Azure 项目的**属性**对话框。

创建新的 Azure 的部署项目时，项目不会按默认值启用远程访问。 但是，可以很容易地通过**发布到 Azure**对话框中指定的用户名和密码启用远程访问。 使用 X.509 证书来加密远程访问密码。 如果您不使用提供您自己的证书，加密依赖于随 Azure 的 Eclipse 插件提供一个自签名证书。 自签名的证书是 Azure 项目，存储公钥证书文件 (SampleRemoteAccessPublic.cer)，而且作为一个个人信息交换 (PFX) 证书文件 (SampleRemoteAccessPrivate.pfx) 的**证书**文件夹中。 后者包含私钥的证书，并具有一个默认密码，**密码 1**。 但是，因为该密码是人尽皆知，默认证书应仅用于学习，而不被用于生产部署。 因此而不出于学习目的，当您想要启用您的部署的远程会话您应该单击**发布到 Azure**对话框以指定您自己的证书中的**高级**链接。 请注意，您需要将证书 PFX 版本上传到托管的服务在 Azure 管理门户，以便该 Azure 可以解密的用户密码。

本教程的其余部分显示了如何启用远程访问禁用远程访问最初创建的 Azure 的部署项目。 出于本教程的需要，我们将创建一个新的自签名的证书，以及其.pfx 文件将具有所选的密码。 您还可以使用证书颁发机构颁发的证书。

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>如何启用远程访问，部署到 Azure 之后

若要启用远程访问，部署到 Azure 之后，使用以下步骤︰

1. 登录到使用 Azure 帐户 Azure 的管理门户
1. 在**云**服务列表中，选择部署的云服务
1. 在云服务 web 页中，单击**配置**链接
1. 在配置页的底部，单击**远程**链接
1. 在弹出对话框中将显示︰
    * 指定您要为其启用远程访问的角色
    * 单击以选中**启用远程桌面**复选框
    * 指定用户名和密码要使用远程访问
    * 选择要使用的证书
1. 单击**确定** 

您将看到一条消息提示您配置更改正在进行，这可能需要几分钟才能完成。 完成配置更改后，请按照**远程登录**部分本文内下文中。
    
## <a name="how-to-enable-remote-access-in-your-package"></a>如何在您的程序包中启用远程访问

1. 在 Eclipse 的项目资源管理器窗格中，右击 Azure 项目，然后单击**属性**。

1. 在**属性**对话框中，展开**Azure**的左窗格中，单击**远程访问**。

1. 在**远程访问权限**对话框中，请确保已**启用要接受与这些登录凭据的远程桌面连接的所有角色**。

1. 指定远程桌面连接的用户名称。

1. 指定并确认用户的密码。 当您建立一个远程桌面连接时，将使用在此对话框中设置的用户名称和密码值。 （请注意，这是从 PFX 密码单独的密码）。

1. 指定的用户帐户的到期日期。

1. 单击**新建**以创建新的自签名的证书。 （或者，可以分别通过**工作区**或**文件系统**的按钮，您的工作区或文件系统中选择一个证书但出于本教程的目的，我们将创建一个新的证书）。

    * 在**新证书**对话框中，指定并确认将用到 PFX 文件密码。

    * 接受**名称 (CN)**，为提供的值或使用自定义名称。

    * 指定将保存新的证书，以.cer 形式的路径和文件名称。 对于这一步和下一步，您可以使用 Azure 项目中，**证书**文件夹，但您可以自由地选择另一个位置。 出于本教程的需要，我们将使用**c:\mycert\mycert.cer**。 （创建的**c:\mycert**文件夹之前，或如果需要使用一个现有文件夹）。

    * 指定保存新证书及相关私钥，以.pfx 形式的路径和文件名称。 出于本教程的需要，我们将使用**c:\mycert\mycert.pfx**。 **新证书**对话框看起来应该类似于下面 （如果您没有使用**c:\mycert**更新文件夹路径）︰

        ![][ic712275]

    * 单击**确定**以关闭**新证书**对话框。

1. **远程访问**对话框应类似于以下形式︰</p>

    ![][ic719495]

1. 单击**确定**以关闭**远程访问**对话框。
    
重建与设置为部署云生成您的应用程序。

## <a name="to-log-in-remotely"></a>远程登录

准备好角色实例后，您可以远程登录到承载您的应用程序虚拟机。

* 如果正在使用 Eclipse 在 Windows 并且您选择了**在启动远程桌面部署**在您部署到 Azure 期间，部署启动时将显示远程桌面连接登录屏幕。 当系统提示您输入用户名和密码时，请输入您的远程用户指定的值并将能够登录。

* 另一种远程登录是通过<a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure 管理门户</a>︰

    * 在 Azure 管理门户网站**云服务**视图中，单击您的云服务**实例**的特定实例，请单击和，然后单击**连接**按钮。 **连接**按钮显示为以下命令栏中︰

        ![][ic659273]

    * 单击**连接**按钮后，将提示您打开 RDP 文件。 打开该文件并按照提示进行操作。 （您可能还将此文件保存到本地计算机，并双击，它就可以远程登录到您的虚拟机，而无需首先转管理门户，然后运行该文件。）

    * 当系统提示您输入用户名和密码时，请输入您的远程用户指定的值并将能够登录。

> [AZURE.NOTE] 如果您是在非 Windows 的操作系统上，您需要使用远程桌面客户端与您的操作系统，然后按照使用您下载的 RDP 文件中的设置配置该客户端的步骤。

## <a name="see-also"></a>请参见

[日蚀式的 azure Toolkit][]

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

[日蚀式安装 Azure Toolkit][] 

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
