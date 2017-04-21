<properties 
    pageTitle="颁发者名称和颁发者密钥在 BizTalk 服务 |Microsoft Azure" 
    description="了解如何检索服务总线或在 BizTalk 服务的访问控制 (ACS) 的颁发者名称和颁发者的密钥。 MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk 服务︰ 颁发者名称和颁发者的密钥

Azure BizTalk 服务使用服务总线颁发者名称和颁发者密钥和访问控制颁发者名称和颁发者的密钥。 具体来说︰

任务 | 哪些颁发者名称和颁发者的密钥
--- | ---
从 Visual Studio 将应用程序部署 | 访问控制颁发者名称和颁发者的密钥
配置 Azure BizTalk 服务门户 | 访问控制颁发者名称和颁发者的密钥
使用 Visual Studio 中的 BizTalk 适配器服务创建 LOB 继电器 | 服务总线颁发者名称和颁发者的密钥

本主题列出的步骤操作，以检索颁发者名称和颁发者的密钥。 

## <a name="access-control-issuer-name-and-issuer-key"></a>访问控制颁发者名称和颁发者的密钥
访问控制颁发者名称和颁发者的密钥将用于以下方面︰

- 在 Visual Studio 中创建 Azure BizTalk 服务应用程序︰ 若要成功部署到 Azure 的 BizTalk 服务应用程序在 Visual Studio 中的，您输入的访问控制颁发者名称和颁发者的密钥。 
- Azure 的 BizTalk 服务门户︰ 当您创建的 BizTalk 服务并打开 BizTalk 服务门户时，您的访问控制颁发者名称和颁发者的密钥是自动注册为您的部署使用相同的访问控制值。

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>复制和粘贴的访问控制颁发者名称和颁发者的密钥

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BizTalk 服务**。
3. 选择您的 BizTalk 服务。 
4. 在任务栏中选择**连接信息**。 访问控制 Namespace，默认颁发者 （颁发者名称），并默认密钥 （颁发密钥） 列出可以复制和粘贴。  

总结︰  
颁发者名称 = 默认颁发者  
颁发者的密钥 = 默认键


您还可以选择**打开 ACS 管理门户**来获取访问控制值︰

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**BizTalk 服务**。
3. 选择您的 BizTalk 服务。
4. 选择连接信息按钮，然后选择**打开 ACS 管理门户**。
5. 在门户在**服务设置**下，选择**服务标识**。 这将显示您服务的身份，这是访问控制颁发者名称值。 选择您的服务标识链接查看密码，颁发者的密钥值。 可以复制其值。<br/><br/>
例如，在**服务标识**，可以看到"所有者"。 "所有者"是您访问控制颁发者的名称。 当您单击"所有者"链接时，您将看到**密码**。 当您单击"密码"链接时，您会看到值。 此密码的值是您访问控制颁发者的密钥。  

总结︰  
颁发者名称 = 服务标识名称  
颁发者的密钥密码值 =

在左侧的导航窗格中，您还可以选择要检索的访问控制值的**Active Directory** 。 

> [AZURE.IMPORTANT]创建访问控制 Namespace 后使用**Active Directory**，服务标识**无法**自动创建。 当您配置 BizTalk 服务、 访问控制 Namespace，服务标识名为"所有者"（颁发者名称）、 密码 （颁发者键） 和对称密钥自动创建的。<br /> 
[如何︰ 配置服务标识使用 ACS 管理服务](http://go.microsoft.com/fwlink/p/?LinkID=303942)提供访问控制服务标识的详细信息。


## <a name="service-bus-issuer-name-and-issuer-key"></a>服务总线颁发者名称和颁发者的密钥
服务总线颁发者名称和颁发者的密钥使用 BizTalk 适配器服务。 在 BizTalk 服务项目在 Visual Studio 中，您可以使用 BizTalk 适配器服务连接到内部的业务线 (LOB) 系统。 若要连接，创建 LOB 中继并输入您的 LOB 系统详细信息。 当执行此操作，也可以输入服务总线颁发者名称和颁发者的密钥。

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>要检索的服务总线颁发者名称和颁发者的密钥

1. 登录到[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)。
2. 在左侧的导航窗格中，选择**服务总线**。
3. 选择您的命名空间。 在任务栏中，选择**连接信息**。 这将显示**默认的颁发者**（颁发者名称） 和**默认的键**（颁发者的密钥）。 可以复制其值。  

总结︰  
颁发者名称 = 默认颁发者  
颁发者的密钥 = 默认键

## <a name="next"></a>下一步
Azure BizTalk 服务的其他主题︰

-  [安装 SDK 的 Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [教程︰ Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>请参见
-  [如何︰ 使用 ACS 管理服务配置服务标识](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk 服务︰ 开发人员、 基本、 标准和高级版图表](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk 服务︰ 资源调配使用 Azure 经典门户](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk 服务︰ 资源调配的状态图表](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk 服务︰ 备份和恢复](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk 服务︰ 限制](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
