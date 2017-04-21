<properties 
    pageTitle="如何配置云服务 （门户） |Microsoft Azure" 
    description="了解如何配置在 Azure 的云服务。 了解如何更新云服务配置并配置远程访问角色实例。 这些示例使用 Azure 的门户。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>如何配置云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-configure-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-configure.md)

可以在 Azure 门户配置为云服务最常用的设置。 或者，如果您想直接更新您的配置文件，下载更新，然后上载已更新的文件并更新云服务使用配置更改服务配置文件。 两种方法配置更新被推送到所有角色实例。

您还可以管理您的云服务角色或远程桌面到它们的实例。

Azure 只可以配置更新期间确保 99.95%的服务可用性，如果必须为每个角色至少两个角色实例。 这样一个虚拟机来处理客户端请求，而其他正在更新。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。

## <a name="change-a-cloud-service"></a>更改一个云服务

打开后[Azure 门户](https://portal.azure.com/)，请导航到云服务。 从这里您可以管理它的许多方面。 

![设置页面](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**设置**或**所有设置**链接将打开**设置**刀片式服务器，更改**属性**、 更改**配置**、 管理**证书**，设置**预警规则**，和管理人员有权访问此云服务**用户**。

![Azure 的云服务设置刀片式服务器](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>云服务使用的操作系统不能更改使用**Azure 门户**，您只能更改此设置通过[Azure 的传统门户网站](http://manage.windowsazure.com/)。 这将详细说明[此处](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file)。

## <a name="monitoring"></a>监视

向云服务，您可以添加通知。 单击**设置** > **警报规则** > **添加通知**。 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

从这里您可以设置警报。 与**Mertic**下拉列表框中，可以设置以下类型的数据的警报。

- 磁盘读取
- 磁盘写入
- 在网络
- 网络出
- CPU 百分比 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>配置监视从跃平铺

而不是使用**设置** > **预警规则**，您可以单击其中一个**云服务**刀片式服务器的**监视**部分中的指标。

![云服务监视](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

从这里您可以自定义图表使用的拼贴，或添加预警规则。


## <a name="reboot-reimage-or-remote-desktop"></a>重新启动，重新映像或远程桌面

这一次无法配置远程桌面使用**Azure 的门户**。 但是，您可以设置它通过[Azure 的传统门户网站](cloud-services-role-enable-remote-desktop.md)， [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)，或通过[Visual Studio](../vs-azure-tools-remote-desktop-roles.md)。 

首先，单击的云服务实例。

![云服务实例](./media/cloud-services-how-to-configure-portal/cs-instance.png)

从刀片式服务器，打开 uou 可以启动远程桌面连接、 远程重启实例，或远程重新映像 （从开始一个全新的图像） 实例。

![云服务实例按钮](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>重新配置您的.cscfg

您可能需要重新配置您[服务配置 (cscfg)](cloud-services-model-and-package.md#cscfg)文件通过云服务。 首先，您需要下载.cscfg 文件，修改它，然后将其上传。

1. 单击**设置**图标或**所有设置**链接以打开**设置**刀片式服务器。

    ![设置页面](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. 单击**配置**项。

    ![刀片式服务器配置](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. 单击**下载**按钮。

    ![下载](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. 更新服务配置文件后，上载并应用配置更新︰

    ![将上载](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. 选择.cscfg 文件，然后单击**确定**。

            
## <a name="next-steps"></a>下一步行动

* 了解如何[部署云服务](cloud-services-how-to-create-deploy-portal.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name-portal.md)。
* [管理您的云服务](cloud-services-how-to-manage-portal.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate-portal.md)。