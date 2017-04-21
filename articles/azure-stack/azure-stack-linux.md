<properties
    pageTitle="在 Azure 的堆栈上的 Linux 客人 |Microsoft Azure"
    description="了解如何在 Azure 堆栈上创建基于 Linux 的虚拟机。"
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>部署在 Azure 堆栈上的 Linux 虚拟机

通过 Azure 堆栈市场添加基于 Linux 的图像，您可以部署在 Azure 堆栈 POC 的 Linux 虚拟机。 几个 Linux 供应商已经提供了可以添加到 Azure 堆栈 POC 的图像或您可以构建自己的。

## <a name="download-an-image"></a>下载图像

 1. 下载并提取 Azure 的堆栈兼容图像从下面的链接，或准备自己︰
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. 提取图像 VHD 如果需要和[市场上向其中添加映像](azure-stack-add-vm-image.md)。 请确保`OSType`参数设置为`Linux`。
 
 3. 已经将图像添加到市场上后，创建市场项目，您可以部署 Linux 虚拟机。
  
## <a name="prepare-your-own-image"></a>准备您自己的图像

1. 准备自己的 Linux 图像使用下列指令之一︰
 - [基于 centOS 的分配](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [红帽企业 Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. 下载并安装[Azure Linux 代理](https://github.com/Azure/WALinuxAgent/)

    Azure Linux 代理 2.1.3 版本或更高，则需要提供 Linux VM Azure 堆栈上。 许多已经上面列出的分配在其存储库包含此版本或更高为包的代理 (通常称为`WALinuxAgent`或`walinuxagent`)。 但是，是否小于 2.1.3 的 Azure 代理包版本 （如 2.0.18 或更低），然后您必须手动安装代理。 可以确定已安装的版本，软件包名称中或通过运行`/usr/sbin/waagent -version`在虚拟机上。

    按照下面的说明手动-安装 Azure Linux 代理

 - 首先，从[Github](https://github.com/Azure/WALinuxAgent/releases)，下载最新的 Azure Linux 代理示例︰

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - 解包 Azure 的代理︰

            # tar -vzxf v2.2.0.tar.gz

 - 安装 python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - 安装 Azure 的代理︰

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    系统使用 Python 2.x 和 Python 3.x 版安装通过并行可能需要运行以下命令︰

        # sudo python3 setup.py install --register-service

    有关详细信息，请参阅 Azure Linux 代理程序[自述文件](https://github.com/Azure/WALinuxAgent/blob/master/README.md)。

3. [添加到市场上的图像](azure-stack-add-vm-image.md)。 请确保`OSType`参数设置为`Linux`。

4. 已经将图像添加到市场上后，创建市场项目，您可以部署 Linux 虚拟机。

## <a name="next-steps"></a>下一步行动

[Azure 堆栈的常见问题](azure-stack-faq.md)