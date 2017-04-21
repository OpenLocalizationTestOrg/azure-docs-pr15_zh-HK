<properties
    pageTitle="Azure 堆栈的常见问题 |Microsoft Azure"
    description="Azure 堆栈的常见问题。"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Azure 堆栈的常见问题

## <a name="deployment"></a>部署

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>我需要我的数据磁盘格式化之前启动或重新启动的安装吗？

磁盘应以原始格式。 如果您重新安装操作系统，您可能需要检查是否依然存在，旧的存储池，并使用以下步骤删除︰

1. 打开服务器管理器。
2. 选择存储池。
3. 查看是否列出一个存储池。
4. 请右键单击**存储池**列出并启用读 / 写。
5. 用鼠标右键单击**虚拟硬盘**（左下的角），并选择删除。
6. 右键单击**存储池**，然后单击删除。
7. 再次启动 Azure 堆栈脚本，然后验证磁盘验证通过。

或者，可以使用以下脚本︰

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>可以为存储池 POC 安装中使用 SSD 的所有磁盘？

在此版本中不支持此配置。  有关详细信息，请参阅[要求指南](azure-stack-deploy.md)的详细信息。

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>可以为 Microsoft Azure 堆栈 POC 使用 NVMe 数据磁盘？

尽管存储空间直接支持 NVMe 磁盘，Azure 堆栈仅为存储空间直接支持了可能的驱动器类型和可能的组合的一个子集。 

### <a name="how-can-i-reinstall-azure-stack"></a>如何重新安装 Azure 堆栈？
您可以按照[重新部署指南](azure-stack-redeploy.md)中的步骤。  

## <a name="tenant"></a>租户

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>可以将自定义映像部署作为一个租户？

是的就像在 Azure，租户可以上载 Azure 堆栈中的图像除了使用服务管理器的图像。 有关概述，请参阅[添加虚拟机映像](azure-stack-add-vm-image.md)。 

## <a name="testing"></a>测试

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>可以使用嵌套虚拟化测试 Microsoft Azure 堆栈 POC 吗？

不支持嵌套的虚拟化或经 Azure 堆栈技术预览 2。

## <a name="virtual-machines"></a>虚拟机

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Azure 堆栈是否支持虚拟机的动态磁盘？

Azure 堆栈不支持动态磁盘。

## <a name="next-steps"></a>下一步行动

[故障排除](azure-stack-troubleshooting.md)
