<properties
   pageTitle="在 Azure 创建一个 Analysis Services 服务器 |Microsoft Azure"
   description="了解如何创建在 Azure Analysis Services 服务器实例。"
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>创建一个 Analysis Services 服务器
这篇文章将引导您完成创建新的 Analysis Services 服务器资源在 Azure 订购。

## <a name="before-you-begin"></a>在开始之前
若要开始，您需要︰

- **Azure 订阅**︰ 就诊[Azure 免费试用版](https://azure.microsoft.com/offers/ms-azr-0044p/)来创建一个帐户。
- **资源组**︰ 使用已有的资源组或[新建一个](../azure-resource-manager/resource-group-overview.md)。

> [AZURE.NOTE] 创建一个 Analysis Services 的服务器可能会导致一种新的收费服务。 若要了解详细信息，请参阅 Analysis Services 定价。

## <a name="create-an-analysis-services-server"></a>创建一个 Analysis Services 服务器

1. 登录到[Azure 的门户](https://portal.azure.com)。

2. 单击**新建 +** > **智能 + 分析** > **Analysis Services**。

3. 在**Analysis Services**刀片式服务器，填写必填字段，，然后按**创建**。

    ![创建服务器](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **服务器名称**︰ 键入用来引用服务器的唯一名称。

    - **订阅**︰ 选择的订购此服务器对账单。

    - **资源组**︰ 这些都是旨在帮助您管理 Azure 的资源集合的容器。 若要了解详细信息，请参阅[资源组](../resource-group-overview.md)。

    - **位置**︰ 此 Azure 数据中心位置存放服务器。 选择最接近您最大的用户群的位置。

    - **定价层**︰ 选择定价层。 表格式建模为 100 GB 支持。 以后始终可以更改您的定价层。

4. 单击**创建**。

创建通常时间不超过一分钟的时间;通常只需几秒钟。 如果选择**添加到门户网站**，请导航到您的门户网站，以查看您的新服务器。 或导航到**更多的服务** > **Analysis Services**查看如果您的服务器已准备就绪。 如果没有显示刷新列表。

 ![仪表板](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>下一步行动
一旦您已经创建了您的服务器，就可以向其[部署模型](analysis-services-deploy.md)通过 SSDT 或通过 SSMS。

如果与内部数据源连接到您的服务器部署模型，您需要在网络中的计算机上安装的[内部数据网关](analysis-services-gateway.md)。
