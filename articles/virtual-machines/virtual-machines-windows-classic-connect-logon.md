<properties
    pageTitle="登录到经典的 Azure VM |Microsoft Azure"
    description="使用 Azure 的传统门户网站登录到 Windows 虚拟机使用传统部署模型创建。"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>登录到 Windows 虚拟机使用 Azure 的经典门户

在 Azure 的经典门户中，使用**连接**按钮以启动远程桌面会话，并登录到 Windows 虚拟机上。

若要连接到 Linux 虚拟机吗？ 请参阅[如何登录到运行 Linux 的虚拟机](virtual-machines-linux-mac-create-ssh-keys.md)。

了解如何[执行这些步骤，使用新的 Azure 门户](virtual-machines-windows-connect-logon.md)。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>视频的演练

在本教程中，下面是视频演练中的步骤。 它还介绍了端点和公用和专用端口用于连接到 Windows Azure 中 VM。

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>连接到虚拟机

1. 登录到 Azure 的传统门户网站。

2. 单击**虚拟机**，然后选择虚拟机。

3. 在页面底部的命令栏中，单击**连接**。

    ![登录到虚拟机](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] 如果**连接**按钮不可用，请参阅本文末尾的故障排除提示。

## <a name="log-on-to-the-virtual-machine"></a>登录到虚拟机

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>下一步行动

-   如果**连接**按钮处于非活动状态，或者您有其他问题的远程桌面连接，请尝试重置配置。 从虚拟机面板下**快速粗略地看**，单击**重置远程配置**。
-   您的密码的问题，请尝试重置它。 虚拟机面板中，从下**快速粗略地看**，单击**重设密码**。

如果这些技巧不起作用或不需要的内容，请参阅[解决远程桌面连接到基于 Windows Azure 虚拟机](virtual-machines-windows-troubleshoot-rdp-connection.md)。 本文将指导您在诊断和解决常见的问题。


