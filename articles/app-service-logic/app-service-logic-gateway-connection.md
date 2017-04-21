<properties
   pageTitle="逻辑应用程序部署的数据网关连接 |Microsoft Azure"
   description="有关如何创建连接到内部数据网关从逻辑应用程序信息。"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>用于逻辑的应用程序连接到内部数据网关

受支持的逻辑应用程序连接器允许您配置通过内部数据网关访问内部数据连接。  以下步骤将指导您完成如何安装和配置内部数据网关来处理逻辑的应用程序。

## <a name="prerequisites"></a>系统必备组件

* 必须使用某一工作或学校中 Azure 将内部数据网关与您的帐户 （基于 Azure Active Directory 帐户） 相关联的电子邮件地址
    * 如果您使用的是 Microsoft 帐户 (例如@outlook.com, @live.com) Azure 帐户可用于创建某一工作或学校的电子邮件地址，按照[这里的步骤](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] 是有限制的当前的内部网关安装时使用电源双注册帐户时才能完成。  在此期间，请注册任何帐户具有"电源双向免费"才能成功完成安装。

* 必须有内部数据网关[安装在本地计算机上](app-service-logic-gateway-install.md)。
* 网关必须未有已申报的另一个内部部署 Azure 数据网关 （[声称发生下面的步骤 2 创建](#2-create-an-azure-on-premises-data-gateway-resource)）-安装只能关联到一个网关资源。

## <a name="installing-and-configuring-the-connection"></a>安装和配置连接

### <a name="1-install-the-on-premises-data-gateway"></a>1.安装内部数据网关

[在这篇文章中](app-service-logic-gateway-install.md)找不到安装内部数据网关信息。  必须在本地计算机上安装网关可以继续使用其余的步骤。

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2.创建一个内部部署 Azure 数据网关资源

安装完成后，您必须将 Azure 订购与内部数据网关相关联。

1. 登录到 Azure 使用相同的工作或学校的电子邮件地址所使用的网关安装过程
1. 单击**新建**资源按钮
1. 搜索并选择**内部数据网关**
1. 填写相应信息以您的帐户-包括选择适当的**安装名称**相关联的网关

    ![内部数据网关连接][1]
1. 单击**创建**按钮创建资源

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3.在设计器中创建的逻辑应用程序连接

Azure 订购过程的内部数据网关实例关联，您可以创建连接到它从逻辑应用程序内。

1. 打开逻辑应用程序，然后选择支持 （撰写本文时，SQL Server） 的内部连接的连接器
1. **通过内部数据网关连接**选中的复选框

    ![逻辑应用程序设计器网关创建][2]
1. 选择**网关**连接到并完成所需的任何其他连接信息
1. 单击**创建**以创建连接

现在应将连接成功配置逻辑应用程序中使用。  

## <a name="next-steps"></a>下一步行动
- [常见的示例和应用场景的逻辑应用程序](app-service-logic-examples-and-scenarios.md)
- [企业集成功能](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG