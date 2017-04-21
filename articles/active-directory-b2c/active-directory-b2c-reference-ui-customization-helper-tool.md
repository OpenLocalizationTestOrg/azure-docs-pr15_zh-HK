<properties
    pageTitle="Azure 的活动目录 B2C︰ 页面 UI 自定义帮助器工具 |Microsoft Azure"
    description="用于演示页用户界面自定义项功能在 Azure 活动目录 B2C 的帮助器工具"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure 的活动目录 B2C︰ 帮助器使用一种工具来演示页用户界面 (UI) 的自定义功能

本文是在 Azure 活动目录 (AD Azure) B2C 的[主用户界面自定义项项目](active-directory-b2c-reference-ui-customization.md)助理。 以下步骤描述了如何通过我们提供的示例 HTML 和 CSS 内容练习页 UI 自定义功能。

## <a name="get-an-azure-ad-b2c-tenant"></a>获取 Azure AD B2C 租户

您可以自定义任何操作之前，您需要[获取 Azure AD B2C 租户](active-directory-b2c-get-started.md)如果您尚没有。

## <a name="create-a-sign-up-or-sign-in-policy"></a>创建注册或登录策略

我们提供了示例内容可用于[注册或登录策略](active-directory-b2c-reference-policies.md)中的 customze 两个页面︰[统一登录页面](active-directory-b2c-reference-ui-customization.md)和[自我肯定的属性页](active-directory-b2c-reference-ui-customization.md)。 当[创建注册或登录策略](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy)，作为**标识提供程序**中添加本地帐户 （电子邮件地址）、 Facebook、 Google 和微软。 这些都是我们的示例 HTML 内容将接受的唯一 IDPs。  如果您愿意，还可以添加这些 IDPs 的子集。

## <a name="register-an-application"></a>注册应用程序

您需要[注册一个应用程序](active-directory-b2c-app-registration.md)中您可以用于执行您的策略的 B2C 租户。 注册后您的应用程序，您有以下几种选择可用于实际运行您注册策略︰

- 构建一个快速启动的应用程序的"入门"一节中列出的 Azure AD B2C[注册并登录您的应用程序的使用者](active-directory-b2c-overview.md#getting-started)。
- 使用预先构建的[Azure AD B2C 运动场](https://aadb2cplayground.azurewebsites.net)应用程序。 如果您选择使用运动场，您必须在您使用**重定向 URI**的 B2C 租户中注册应用程序`https://aadb2cplayground.azurewebsites.net/`。
- 在[Azure 门户](https://portal.azure.com/)策略使用**立即运行**按钮。

## <a name="customize-your-policy"></a>自定义您的策略

若要自定义外观和感觉您的策略，您需要首先创建 HTML 和 CSS 文件使用 Azure AD B2C 的具体约定。 然后可以将您的静态内容上载到可公开获得的位置以便 Azure AD B2C 可以访问它。 这可能是您自己的专用的 web 服务器、 Azure Blob 存储、 Azure 内容传递网络或任何其他静态资源承载的提供程序。 唯一的要求是您的内容通过 HTTPS 是可用和可以访问使用 CORS。 一旦已经公开您的静态内容在网站上，您可以编辑您指向该位置，并向客户提供该内容的策略。 [主用户界面自定义文章](active-directory-b2c-reference-ui-customization.md)详细介绍了 Azure AD B2C 自定义功能的工作原理。

出于本教程中，我们已经创建一些示例内容并位于 Azure Blob 存储。 示例内容是公司的非常基本的自定义主题中我们虚构，"Wingtip Toys"。 若要尝试在您自己的策略，请执行以下步骤︰

1. 登录到您的租户在[Azure 的门户网站](https://portal.azure.com/)上，定位到 B2C 功能刀片式服务器。
2. 单击**注册或签入策略**，然后单击您的策略 (例如，"b2c\_1\_号\_上\_号\_中")。
3. 单击**页用户界面自定义项**，然后**统一注册或登录页**。
4. 切换为**是****使用自定义页**开关。 在**自定义页面的 URI**字段中，输入`https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`。 单击**确定**。
5. 单击**本地帐户注册页**。 切换为**是****使用自定义模板**开关。 在**自定义页面的 URI**字段中，输入`https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`。
5. **社会帐户注册页**重复相同的步骤。
 单击**确定**两次以关闭 UI 自定义薄片。
6. 单击**保存**。

现在，您可以尝试您的自定义策略。 如果您希望，但也只是可以单击策略刀片式服务器中的**立即运行**命令，可以使用您自己的应用程序或 Azure AD B2C 运动场。 在下拉框中选择您的应用程序，然后选择适当的重定向 URI。 单击**立即运行**按钮。 将打开新的浏览器选项卡，您可以通过注册应用程序的位置中的新内容的用户体验来运行 ！

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>将示例内容上载到 Azure Blob 存储

如果您想要使用 Azure Blob 存储来承载您的网页内容，您可以创建存储帐户并使用我们 B2C 帮助器工具将文件上传。

### <a name="create-a-storage-account"></a>创建存储帐户。

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**新建 +** > **数据 + 存储** > **存储帐户**。 您将需要创建 Azure Blob 存储帐户的 Azure 订阅。 您可以注册[Azure 网站](https://azure.microsoft.com/pricing/free-trial/)上的免费试用版。
3. 提供存储帐户 (例如，"contoso") 的**名称**并选择适当选择**定价层**、**资源组**和**订阅**。 请确保您已检**附到 Startboard**选项。 单击**创建**。
4. 请返回到 Startboard 并单击刚才创建的存储帐户。
5. 在**摘要**部分中，单击**容器**，然后单击**+ 添加**。
6. 容器 (例如，"b2c") 提供的**名称**并选择**Blob**作为**访问类型**。 单击**确定**。
7. 您创建的容器将**Blob**刀片式服务器上的列表中显示。 记下该 URL 的容器;例如，它看起来应该类似`https://contoso.blob.core.windows.net/b2c`。 关闭**Blob**刀片式服务器。
8. 存储帐户刀片式服务器，请单击**键**并记下**存储帐户名称**和**访问主键**字段的值。

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**新建 +** > **数据 + 存储** > **存储帐户**。 您将需要创建 Azure Blob 存储帐户的 Azure 订阅。 您可以注册[Azure 网站](https://azure.microsoft.com/pricing/free-trial/)上的免费试用版。
3. 选择**帐户类型**， **Blob 存储**和保留为默认值的其他值。  如果您愿意，可以编辑资源组和位置。  单击**创建**。
4. 请返回到 Startboard 并单击刚才创建的存储帐户。
5. 在**摘要**部分中，单击**+ 容器**。
6. 容器 (例如，"b2c") 提供的**名称**并选择**Blob**作为**访问类型**。 单击**确定**。
7. 打开容器**属性**中，并记下该 URL 的容器;例如，它看起来应该类似`https://contoso.blob.core.windows.net/b2c`。 关闭刀片式服务器容器。
8. 存储帐户刀片式服务器，请单击**键图标**和记下**存储帐户名称**和**访问主键**字段的值。

> [AZURE.NOTE]
    **主键访问**是重要的安全凭据。

### <a name="download-the-helper-tool-and-sample-files"></a>下载帮助程序工具和示例文件

可以下载[Azure Blob 存储帮助工具和示例文件以.zip 文件格式](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip)或从 GitHub 克隆它︰

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

该存储库包含`sample_templates\wingtip`目录，其中包含示例 HTML、 CSS 和图像。 这些模板引用 Azure Blob 存储帐户，您需要编辑 HTML 文件。 打开`unified.html`和`selfasserted.html`和替换的任何实例`https://localhost`与您在前面的步骤中记下您自己容器的 URL。 必须使用 HTML 文件的绝对路径，因为在这种情况下，HTML 将提供由 Azure 的广告，在域下`https://login.microsoftonline.com`。

### <a name="upload-the-sample-files"></a>上载示例文件

在同一存储库中，解压缩`B2CAzureStorageClient.zip`，然后运行`B2CAzureStorageClient.exe`文件中。 这个程序只需将上载到您的存储帐户，您指定的目录中的所有文件并启用 CORS 访问这些文件。 如果您按照上述步骤操作，HTML 和 CSS 文件将立即指向您的存储帐户。 请注意您的存储帐户的名称，前面的部分`blob.core.windows.net`;例如， `contoso`。 您可以验证是否正确地已尝试访问的上载内容`https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html`浏览器上。 此外可以使用[http://test-cors.org/](http://test-cors.org/)来确保内容现已启用的 CORS。 (寻找"XHR 状态︰ 200"的结果。)

### <a name="customize-your-policy-again"></a>自定义您的策略重新

现在，您已上载示例内容到自己的存储帐户，您必须编辑注册策略引用它。 这次使用自己的存储帐户 Url 重复上面， ["自定义您的策略"](#customize-your-policy)一节中的步骤。 例如的位置您`unified.html`文件将`<url-of-your-container>/wingtip/unified.html`。

现在您可以使用**立即运行**按钮或您自己的应用程序若要再次执行您的策略。 结果看起来几乎完全相同，则使用相同示例 HTML 和 CSS 中这两种情况。 但是，您的策略现在正在引用的 Azure Blob 存储实例，您可以随意编辑和上载的文件再次为您请。 有关自定义的 HTML 和 CSS 的详细信息，请参阅[主 UI 自定义文章](active-directory-b2c-reference-ui-customization.md)。
