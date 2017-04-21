<properties 
    pageTitle="云代理服务的应用程序发现注册表设置 |Microsoft Azure" 
    description="本主题的目的是为您提供您需要执行在运行云应用程序探测代理的计算机上设置所需的端口的步骤。" 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>云应用程序发现注册表设置代理服务。

默认情况下，云应用程序探测代理配置为只使用端口 80 或 443。 如果计划在环境中使用的代理服务器使用自定义端口 （80 和 443 都不） 安装云应用程序发现，您需要配置您的代理程序以使用此端口。 配置基于注册表项。


本主题的目的是为您提供您需要执行在运行云应用程序探测代理的计算机上设置所需的端口的步骤。



**若要修改运行云应用程序探测代理的计算机所使用的端口，请执行以下步骤︰**


1. 启动注册表编辑器。 <br> ![运行](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. 导航到或创建下面的注册表项︰ <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud 应用程序 Discovery\Endpoint** 

3. 创建新的**多字符串**值称为**端口**。 ![新增功能](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. 若要打开**编辑多字符串**对话框中，双击的端口值。


5. 在值数据文本框中，键入下列值并添加您的组织使用的所有自定义端口︰ <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![编辑多字符串](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. 单击**确定**以关闭**编辑多字符串**对话框。



**其他资源**


* [如何在我的组织内发现未批准的云应用程序使用](active-directory-cloudappdiscovery-whatis.md) 


