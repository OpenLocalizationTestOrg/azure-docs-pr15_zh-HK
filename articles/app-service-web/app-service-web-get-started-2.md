<properties
    pageTitle="第一个 web 应用程序中添加功能"
    description="在几分钟内将超酷功能添加到您的第一个 web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>第一个 web 应用程序中添加功能

在[部署到 Azure 中五分钟第一个 web 应用程序](app-service-web-get-started.md)，则示例 web 应用程序部署到[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)。 在本文中，将快速于已部署的 web 应用程序添加一些强大的功能。 几分钟后，您将能够︰

- 强制执行为您的用户身份验证
- 自动缩放您的应用程序
- 在您的应用程序的性能上接收通知

无论您在以前的文章中部署的示例应用程序，用户可以根据教程中。

在本教程中的三个活动是仅当您将您的 web 应用程序放入应用程序服务时，获得许多有用的功能的一些示例。 许多功能都可以在**自由**层 （这是您的第一个 web 应用程序运行在上），并且可以使用您试用的信用要求较高的定价层的功能。 放心，您的 web 应用程序仍保留在**自由**层除非显式将其更改为不同的定价层。

>[AZURE.NOTE] 在**自由**层，只允许一个共享的资源配额的 VM 实例中运行使用 Azure CLI 创建 web 应用程序。 与**自由**层获取的内容的详细信息，请参阅[应用程序的限制](../azure-subscription-service-limits.md#app-service-limits)。

## <a name="authenticate-your-users"></a>对用户进行身份验证

现在，让我们看看将身份验证添加到您的应用程序 （在[应用程序服务的身份验证/授权](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)进一步阅读） 是多么容易。

1. 在您的应用程序门户刀片，其刚刚打开，单击**设置** > **身份验证 / 授权**。  
    ![通过身份验证的设置刀片式服务器](./media/app-service-web-get-started/aad-login-settings.png)

2. **在**要启用身份验证，请单击。  

4. 在**身份验证提供程序**，请单击**Azure 活动目录**。  
    ![验证-选择 Azure 的广告](./media/app-service-web-get-started/aad-login-config.png)

5. 在**Azure 活动目录设置**刀片式服务器，单击**速成**，然后单击**确定**。 创建新的默认设置 Azure AD 应用程序在默认目录中的。  
 ![验证-快速配置](./media/app-service-web-get-started/aad-login-express.png)

6. 单击**保存**。  
    ![通过身份验证的配置存储](./media/app-service-web-get-started/aad-login-save.png)

    成功更改后，您将看到通知铃声变为绿色，以及友好的消息。

7. 重新在您的应用程序门户刀片式服务器，请单击**URL**链接 （或**浏览**菜单栏中）。 以下链接是一个 HTTP 地址。  
    ![验证-浏览到的 URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    但一旦它在新选项卡中打开应用程序时，URL 框几次的重定向和 HTTPS 地址与您的应用程序完成。 什么您会看到您已经登录到 Azure 的订阅，并且您要自动在应用程序中进行身份验证。  
    ![验证-登录](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    因此，如果您现在在不同的浏览器中打开一个未经身份验证的会话，您将看到一个登录屏幕时定位到同一 URL。  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   如果所永远不会做任何操作 Azure Active Directory，您的默认目录可能不具有任何 Azure AD 用户。 在这种情况下，可能仅有的 Microsoft 帐户是 Azure 订阅。 这就是为什么您自动登录到该应用程序在同一个浏览器前面。
   可以使用该同一 Microsoft 帐户尝试登录到此的登录页。

祝贺您，您正在于 web 应用程序所有通信进行身份都验证。

您可能已经注意到在**身份验证 / 授权**刀片，可以执行更多操作，如︰

- 实现社会的登录
- 启用多个登录选项
- 当人们第一次导航到您的应用程序更改的默认行为

应用程序服务提供了一个交钥匙解决方案，为一些常见的身份验证需求，因此不需要提供您自己的验证逻辑。
有关详细信息，请参阅[应用程序服务的身份验证/授权](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)。

## <a name="scale-your-app-automatically-based-on-demand"></a>扩展您的应用程序会自动根据需求

下一步，让自动缩放您的应用程序，以便它将自动调整它的容量来响应用户需求 （进一步阅读[在 Azure 应用程序向上扩展](web-sites-scale.md)和[扩展实例计数，手动或自动](../monitoring-and-diagnostics/insights-how-to-scale.md)）。

简言之，缩放您的 web 应用程序在两个方面︰

- [向上扩展](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)︰ 获取更多的 CPU、 内存、 磁盘空间和额外功能，如专用的虚拟机、 自定义的域和暂存槽、 自动缩放，和更多的证书。 通过更改您的应用程序所属的应用程序服务计划定价层来扩大规模。
- [向外的扩展](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)︰ 增加了 VM 实例运行您的应用程序。
您可以扩展到多达 50 实例，具体取决于您的定价层。

闲话，让我们设置自动缩放。

1. 首先，让我们扩大以启用自动缩放。 在您的应用程序门户刀片式服务器，请单击**设置** > **向上 （应用程序服务计划） 的扩展**。  
    ![扩大的刀片式服务器设置](./media/app-service-web-get-started/scale-up-settings.png)

2. 向下滚动并选择**S1 标准**层，最底层支持自动缩放 （屏幕抓图中圆圈），然后单击**选择**。  
    ![向上扩展-选择层](./media/app-service-web-get-started/scale-up-select.png)

    在完成向上扩展。

    >[AZURE.IMPORTANT] 这一层消耗您免费试用的信用。 如果您有一个按使用付费帐户，它将导致对您的帐户的费用。

3. 接下来，我们可以配置自动缩放。 在您的应用程序门户刀片式服务器，请单击**设置** > **向外扩展 （应用程序服务计划）**。  
    ![扩大规模的刀片式服务器设置](./media/app-service-web-get-started/scale-out-settings.png)

4. 将**按比例**更改为**CPU 百分比**。 下拉列表下方的滑块相应地更新。 然后，定义**实例**范围为**1**和**2**以及**目标区域** **40**和**80**之间。 请通过在框中键入或移动滑块。  
 ![向外扩展 — 配置自动缩放](./media/app-service-web-get-started/scale-out-configure.png)

    基于这种配置，您的应用程序将自动缩放出时 CPU 利用率超过 80%，可以扩展在 CPU 利用率低于 40%时。

5. 在菜单栏中，单击**保存**。

祝贺您，您的应用程序被自动缩放。

您可能已经注意到，可以执行更多操作，如**缩放设置**刀片式服务器中︰

- 手动缩放到特定的实例数
- 其他性能指标，如内存百分比或磁盘队列来缩放
- 性能规则被触发时，自定义缩放行为
- 按照日程安排自动缩放比例
- 未来事件的设置自动缩放行为

扩展您的应用程序的详细信息，请参阅[在 Azure 应用程序扩展](../app-service-web/web-sites-scale.md)。 横向扩展的详细信息，请参阅[手动或自动缩放实例计数](../monitoring-and-diagnostics/insights-how-to-scale.md)。

## <a name="receive-alerts-for-your-app"></a>接收到通知您的应用程序

现在，您的应用程序是自动缩放，在到达最大实例数 (2) 时，会发生什么情况，需利用率 （80%) 以上的 CPU 是？
您可以设置警报 （进一步阅读时[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)），因此，您可以进一步扩展向上/出您的应用程序，例如告诉您这种情况。 让我们快速设置此方案中的警报。

1. 在您的应用程序门户刀片式服务器，请单击**工具** > **的警报**。  
    ![预警-刀片式服务器设置](./media/app-service-web-get-started/alert-settings.png)

2. 单击**添加通知**。 然后，在**资源**框中，选择结束**(serverfarms)**的资源。 这就是您的应用程序服务的计划。  
    ![预警-添加为应用程序服务计划的通知](./media/app-service-web-get-started/alert-add.png)

3. 指定**名称**为`CPU Maxed`，**规格****阈值**作为**CPU 百分比**，以及`90`，然后选择**电子邮件所有者、 参与者和读者**，然后单击**确定**。   
 ![预警-配置警报](./media/app-service-web-get-started/alert-configure.png)

    当 Azure 完成创建警报时，您将看到它**通知**刀片式服务器中。  
    ![预警-完成视图](./media/app-service-web-get-started/alert-done.png)

祝贺您，您将立即获得警报。

此警报设置检查 CPU 利用率每隔五分钟。 如果该数字超过 90%，您将收到电子邮件警报，以及被授权的任何人。 若要查看有权接收通知的人，请返回到您的应用程序门户刀片并单击**访问**按钮。  
![谁得到通知，请参阅](./media/app-service-web-get-started/alert-rbac.png)

您应该看到**预订管理员**已经是应用程序的**所有者**。 此组将包括如果你 Azure 订阅 （例如您的订购试用期） 的帐户管理员。 Azure 的基于角色的访问控制的详细信息，请参阅[Azure Role-Based 访问控制](../active-directory/role-based-access-control-configure.md)。

> [AZURE.NOTE] 预警规则是 Azure 的功能。 有关详细信息，请参阅[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)。

## <a name="next-steps"></a>下一步行动

可配置的通知方式，您可能已经注意到一组丰富的**工具**刀片式服务器中的工具。 在这里，您可以解决问题，监控性能测试是否存在漏洞、 管理资源、 交互使用 VM 控制台时，和添加有用的扩展。 我们邀请您单击每个这些工具来发现的简单但功能强大的工具，在您的指尖。

了解如何做更多与您已部署的应用程序。 以下是部分列表︰

- [购买和配置自定义域名](custom-dns-web-site-buydomains-web-app.md)的购买您的 web 应用程序而不是吸引域 *。 azurewebsites.net 域。 或使用已有的域。
- [设置临时环境](web-sites-staged-publishing.md)-在投入生产之前将您的应用程序部署到一个临时的 URL。 自信地更新您的实时 web 应用程序。 设置有多个部署槽精密 DevOps 解决方案。
- [设置连续部署](app-service-continuous-deployment.md)-集成的应用程序部署到您的源代码管理系统。 每个提交部署到 Azure。
- [访问本地资源](web-sites-hybrid-connection-get-started.md)的访问权限的现有内部数据库或 CRM 系统。
- [备份您的应用程序](web-sites-backup.md)-设置后并还原您的 web 应用程序。 准备的意外故障并从中恢复。
- [启用诊断日志](web-sites-enable-diagnostic-log.md)-从 Azure 或应用程序跟踪读取 IIS 日志。 在流中读取、 下载它们，或它们交钥匙分析导入[应用程序的见解](../application-insights/app-insights-overview.md)。
- [扫描您的应用程序的漏洞](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
扫描 web 应用程序使用提供的[锡箔安全](https://www.tinfoilsecurity.com/)服务的现代威胁。
- [后台作业运行](../azure-functions/functions-overview.md)的运行作业用于数据处理、 报告等。
- [了解应用程序服务的工作方式](../app-service/app-service-how-works-readme.md)
