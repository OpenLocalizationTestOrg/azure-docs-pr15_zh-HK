<properties
    pageTitle="Web 应用程序在 Azure 堆栈-已知的问题和疑难解答 |Microsoft Azure"
    description="部署在 Azure 堆栈中的 Web 应用程序的详细的指导"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web 应用程序资源提供程序的已知的问题和疑难解答

> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

如果在部署或使用 Azure 堆栈上的 Web 应用程序时遇到问题请参考下面的指南。

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure 堆栈 Web 应用程序不可用的门户

![Azure 堆栈 Web 应用程序创建新的 Web 应用程序][1]

之后如果您找不到 Web + 移动刀片式服务器，请尝试以下，您已完成[注册 Azure 堆栈 Web 应用程序提供商](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm)︰
* **门户的注销**和**关闭您的浏览器**，然后在**新浏览器窗口登录到门户网站**，然后重试。

如果您仍然看不到的 web 和移动刀片式服务器，请尝试以下︰

1.  **Hyper-V 管理器**的**Azure 堆栈主机**上找到的**xRPVM**和虚拟机**连接**。
2.  打开**管理员命令提示符**并运行**IISRESET**
3.  返回到**ClientVM**和打开**新浏览器窗口**，请**登录到门户网站**，然后重试。

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>在新的资源组中创建 Web 应用程序时，部署失败

在第一个预览的 Web 应用程序，必须为 Web 应用程序资源的提供程序 （**AppService 本地**） 同一资源组中创建**所有 Web 应用程序**。  这是一个已知的问题，将在未来的预览中予以解决。

## <a name="other-issues"></a>其他问题

如果您遇到其他 Azure 堆栈上的 Web 应用程序的问题请张贴在 [Azure 堆栈论坛] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) 在我们小组的成员正在监视公告。


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
