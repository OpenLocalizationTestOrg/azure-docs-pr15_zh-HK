<properties
 pageTitle="获取工具 (Ubuntu 16.04) |Microsoft Azure"
 description="下载并安装 Ubuntu 的必要工具和软件的第一个示例应用程序为您 Pi。"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="12-get-the-tools-ubuntu-1604"></a>1.2 获取工具 (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 将做什么

开发工具和您 Raspberry Pi 3 的第一个示例应用程序的软件下载。 如果您遇到任何问题，寻求解决方案的[故障排除页](iot-hub-raspberry-pi-kit-node-troubleshooting.md)。

## <a name="122-what-you-will-learn"></a>1.2.2 将学到的

在本节中，您将了解︰

- 如何安装 Git，Node.js
  - [Git](https://git-scm.com)是分布式开放源代码版本控制系统。 这一课的示例应用程序存储在 Git 中。
  - [Node.js](https://nodejs.org/en/)是丰富包生态系统与 JavaScript 运行。
- 如何使用 NPM 安装其他 Node.js 开发工具。
  - 要求的最低版本的 Node.js 是 4.5 LTS。
  - [NPM](https://www.npmjs.com)是 Node.js 包经理之一

## <a name="123-what-do-you-need"></a>1.2.3 什么需要

- 互联网的连接来下载的开发工具和软件
- 运行 Ubuntu 16.04 或更高版本的计算机， 

## <a name="124-install-git-nodejs-and-npm"></a>1.2.4 安装 Git，Node.js 和 NPM

使用键盘快捷方式`Ctrl + Alt + T`打开一个终端窗口并运行以下命令︰

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 安装其他 Node.js 开发工具

[Gulp.js](http://gulpjs.com)用于自动执行到您 Pi 的示例应用程序的部署。 此外可以使用[设备-发现-cli](https://github.com/Azure/device-discovery-cli)有关 IoT 设备检索网络信息。

安装`gulp`和`device-discovery-cli`通过终端窗口中运行以下命令︰

```bash
sudo npm install -g device-discovery-cli gulp
```

如果您遇到问题在 Ubuntu 上安装 Node.js 和这些其他的开发工具，请参阅[故障排除指南](iot-hub-raspberry-pi-kit-node-troubleshooting.md)有关的常见问题的解决方案。

## <a name="126-install-visual-studio-code"></a>1.2.6 安装 Visual Studio 代码

[下载](https://code.visualstudio.com/docs/setup/linux)并安装 Visual Studio 代码。 Visual Studio 代码是用于 Windows、 Linux 和 macOS 轻量，但功能强大的源代码编辑器。 稍后在本教程中使用此编辑器进行编辑的示例代码。

## <a name="127-summary"></a>1.2.7 摘要

您已经安装了所需的开发工具和软件的第一个示例应用程序。 在下一部分中，您创建、 部署和运行示例应用程序基于您 Pi。

## <a name="next-steps"></a>下一步行动

[1.3 创建和部署闪烁示例应用程序](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)