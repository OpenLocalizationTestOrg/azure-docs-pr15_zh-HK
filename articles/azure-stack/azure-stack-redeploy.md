<properties
    pageTitle="重新部署 Azure 堆栈 |Microsoft Azure"
    description="重新部署 Azure 的堆栈。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>重新部署 Azure 堆栈

重新部署 Azure 堆栈，您必须从头如下所述。

## <a name="steps-to-redeploy-azure-stack"></a>重新部署 Azure 堆栈的步骤

1. 重新启动主机到原始操作系统 （裸机安装）。 这不是启动菜单中的默认设置，因此您必须使用 KVM 或本地控制台在重新启动过程中将其选中 （在安装期间，您可以命名"引导的 VHD"操作系统到"AzureStack TP2"，这将有助于确定哪些操作系统是这种做法）。

    您不需要删除现有的启动项目 （新支持脚本"PrepareBootFromVHD.ps1"负责为您。）

2. 如果您没有 KVM，或者想要在重新引导之前选择启动操作系统︰
    
    1. 查找脚本.\BootMenuNoKVM.ps1。 此文件是使用与该版本一起提供的其他支持脚本可用。
    
    2. 使用提升的权限运行脚本。 选择原始主机操作系统的名称。 这将到原始主机操作系统启动主机，而无需 KVM 访问。
    
    3. 脚本后系统将要求您确认重新启动。

    - 如果有其他用户登录，则此命令将失败。

    - 请只需运行下面的命令︰ 重新启动计算机的强制 
 
3. 删除以前的部署过程中使用的 CloudBuilder.vhdx 文件。

    您不需要从以前的 TP2 部署中删除现有的存储池。 部署脚本检测到并清理现有，然后创建新。

5. 重新部署了从复制一份新的 CloudBuilder.vhdx，引导到它，等等。

## <a name="next-steps"></a>下一步行动

[连接到 Azure 堆栈](azure-stack-connect-azure-stack.md)
