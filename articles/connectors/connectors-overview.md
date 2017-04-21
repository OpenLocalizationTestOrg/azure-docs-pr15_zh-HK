<properties
    pageTitle="逻辑应用程序连接器概述 |Microsoft Azure"
    description="可以在逻辑应用程序中使用的连接器的概述"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>逻辑应用程序中使用连接器

连接器在服务、 协议和平台提供了快速访问事件、 数据和操作。  逻辑应用程序支持的接口的完整列表可以[被找到这里](apis-list.md)。  连接器可以用作触发器或操作中的逻辑的应用程序，并且可能需要配置的*连接*使用 (例如︰ 授权访问或以您的名义发布的 Twitter 帐户)。

## <a name="basics"></a>基础知识

连接器是托管的服务，您可以访问作为逻辑的应用程序与动态，Azure，销售队伍，[和更多](apis-list.md)像其他服务进行集成的一部分。  它们是部署和 Microsoft，使您能够生成您集成的工作流与比例、 吞吐量和注意的安全管理。  可以通过搜索并选择连接器操作或**显示 Microsoft 托管 Api**在触发器逻辑应用程序添加一个连接符。

![操作菜单中选择触发器][1]

每个连接器操作或触发器将具有属性来配置其的设置。  您可以单击信息按钮可了解更多有关行动，或引用其文档[以了解详细信息](apis-list.md)。

如果您想要与某个服务或不是又一个连接器，也可以扩展逻辑应用程序通过[自定义连接线](../app-service-logic/app-service-logic-create-api-app.md)或只需调用到服务直接通过例如 HTTP 协议的 API 集成。

## <a name="triggers"></a>触发器

一些连接器有触发器，这意味着该连接器中的事件将触发逻辑应用程序并作为触发器的一部分传入的任何数据。  触发器始终是逻辑应用程序中的第一步。  常见的触发器包括诸如之类的操作︰
 
 * 定期的每小时运行一次
 * 当接收到的 HTTP 请求时
 * 当将某项添加到队列
 * 当收到一封电子邮件
 
一些触发器将触发事件发生到逻辑的应用程序，通知通过即时，其他人还需要配置上的逻辑应用程序检查 （最多每隔 15 秒） 的事件服务的时间间隔重复出现的间隔。  

一旦收到事件时，将会触发逻辑应用程序运行并启动工作流中的操作。  您还能够访问的任何数据从整个工作流 （例如开启新的 tweet 触发器将传入 tweet 运行） 的触发器。

## <a name="actions"></a>操作

大多数连接器有一个或多个可以作为工作流的一部分执行的操作。  操作是从触发器触发运行后发生的任何步骤。  若要添加操作单击**新步骤**按钮和连接器搜索想要使用。  一旦选定 （和之后配置可能需要的任何[连接](#connections)），您将看到可以配置操作卡。  您可以通过单击任何标记为输出，从前面的步骤中选择数据或输入所需的任何其他配置。

![配置连接器操作][2]

## <a name="connections"></a>连接

大多数的连接器都需要您要配置的*连接*，然后才能使用该连接器。  *连接*的访问接口所需的任何登录或连接配置。  连接器的使用 OAuth，创建连接是指登录可以加密和 Azure 的密钥存储区中安全地存储您的访问令牌 （如 Office 365、 销售队伍或 GitHub） 服务。  （如 FTP 和 SQL） 其他连接器都需要包含与服务器地址、 用户名和密码配置的连接。  此外加密和安全地存储这些连接配置详细信息。  连接将能够，只要服务允许访问服务。  对 Azure 活动目录 OAuth 连接 （如 Office 365 和动态） 我们可以继续无限期地刷新访问令牌。  其他服务可能对限制多长时间我们可以使用一个标记没有被刷新。  一般情况下某些操作，如更改密码将导致所有访问令牌都无效。  

可以查看和管理在 Azure 中通过单击**浏览**并选择**API 连接**的连接。  从 API 连接资源可以查看、 编辑、 更新，或重新创建任何连接的用户授权。

## <a name="next-steps"></a>下一步行动

- [创建您的第一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [了解常见用法和逻辑的应用程序的示例](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [企业集成触发器和操作入门](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png