<properties 
    pageTitle="如何配置云服务 （传统门户） |Microsoft Azure" 
    description="了解如何配置在 Azure 的云服务。 了解如何更新云服务配置并配置远程访问角色实例。" 
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

可以在 Azure 的传统门户配置为云服务最常用的设置。 或者，如果您想直接更新您的配置文件，下载更新，然后上载已更新的文件并更新云服务使用配置更改服务配置文件。 两种方法配置更新被推送到所有角色实例。

Azure 的传统门户网站还允许您为[启用远程桌面连接的 Azure 云服务中的角色](cloud-services-role-enable-remote-desktop.md)

Azure 只可以配置更新期间确保 99.95%的服务可用性，如果必须为每个角色至少两个角色实例。 这样一个虚拟机来处理客户端请求，而其他正在更新。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。

## <a name="change-a-cloud-service"></a>更改一个云服务

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，单击**云服务**，单击云服务的名称，然后单击**配置**。

    ![配置页](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    在**配置**页中，可以配置监视、 更新角色设置，并选择的来宾操作系统和角色实例的家族。 

2. 在**监视**，设置监视级别详细或最低限度，并配置所需的详细的监控诊断连接字符串。

3. 对于服务角色 （按角色），您可以更新下列设置︰
    
    >**设置**  
    >修改服务配置 (.cscfg) 文件中的*ConfigurationSettings*元素中指定的其他配置设置的值。
    >
    >**证书**  
    >更改角色的 SSL 加密中正在使用的证书指纹。 若要更改一个证书，必须首先将新的证书 （在**证书**上） 上载。 然后，更新证书字符串显示在角色设置中的指纹。

4. 在**操作系统**中，可以更改系列操作系统或版本对于角色实例，或选择**自动**启用自动更新最新的操作系统版本。 操作系统设置应用于 web 角色和辅助的角色，但并不影响虚拟机。

    在部署上所有角色实例，, 安装了最新的操作系统版本和操作系统默认情况下自动更新。 
    
    如果您需要为您的云服务可以运行在不同的操作系统版本在代码中的兼容性要求，您可以选择一个系列操作系统和版本。 当您选择特定的操作系统版本时，在云服务的操作系统自动更新都将被挂起。 您将需要确保操作系统接收更新。
    
    如果解决所有您的应用程序具有最新的操作系统版本的兼容性问题，可以通过将操作系统版本设置为**自动**启用自动操作系统更新。 
    
    ![操作系统设置](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. 若要保存配置设置，并将它们推送到角色实例，请单击**保存**。 （单击**放弃**若要取消所做的更改）。更改设置后，**保存**并**放弃**将添加到命令栏。

## <a name="update-a-cloud-service-configuration-file"></a>云服务配置文件更新

1. 下载使用当前配置的云服务配置文件 (.cscfg)。 在云服务**配置**页中，单击**下载**。 然后单击**保存**，或者单击**另存为**保存该文件。

2. 更新服务配置文件后，上载并应用配置更新︰

    1. 在**配置**页中，单击**上载**。
    
        ![将配置上载](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. 在**配置文件**中，使用**浏览**，选择该更新的.cscfg 文件。
    
    3. 如果云服务包含只能有一个实例的任何角色，选择**应用于配置，即使一个或多个角色中包含的单个实例**复选框，以启用的角色，若要继续配置更新。
    
        除非您定义的每个角色至少两个实例，Azure 无法保证至少达到 99.95%的云服务服务配置更新期间的可用性。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。
    
    4. 单击**确定**（复选标记）。 


## <a name="next-steps"></a>下一步行动

* 了解如何[部署云服务](cloud-services-how-to-create-deploy.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name.md)。
* [管理您的云服务](cloud-services-how-to-manage.md)。
* [启用远程桌面连接的 Azure 的云服务中的角色](cloud-services-role-enable-remote-desktop.md)
* 配置[ssl 证书](cloud-services-configure-ssl-certificate.md)。
