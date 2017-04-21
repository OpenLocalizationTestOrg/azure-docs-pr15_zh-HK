<properties
 pageTitle="IoT 集线器设备管理概述 |Microsoft Azure"
 description="本文概述在 Azure IoT 集线器的设备管理︰ 企业设备生命周期、 重新启动、 出厂重置、 固件更新、 配置、 设备 twins、 查询、 作业"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Azure IoT 集线器设备管理 （预览） 概述

## <a name="introduction"></a>介绍

Azure IoT 集线器提供的功能和扩展性模型，从而使设备和后端开发人员构建功能强大 IoT 设备管理解决方案。 IoT 设备范围有限的传感器和单一用途的微控制器，到功能强大的网关的路由组的设备通信。  此外，用例和要求 IoT 运算符有显著变化跨行业。  尽管这一变化，Azure IoT 集线器设备管理提供了功能、 模式和代码库能够满足广泛的设备和最终用户。

创建一个成功的企业 IoT 解决方案为运算符如何处理他们的设备集合的日常管理提供一种战略的一个重要组成部分。 IoT 运算符要求简单和可靠的工具和应用程序，使他们能够专注于他们的工作更具战略意义方面。 这篇文章提供了︰

- Azure IoT 集线器设备管理方法的简要概述。
- 公共设备管理原则的说明。
- 在设备生命周期的说明。
- 概述公共设备管理模式。

## <a name="iot-device-management-principles"></a>IoT 设备管理原则

IoT 有一套独特的设备管理面临的挑战，每一种企业级的解决方案必须解决下列原则︰

![Azure IoT 集线器设备管理原则图形][img-dm_principles]

- **规模和自动化**︰ IoT 解决方案需要简单的工具，可以自动执行常规任务，并使管理数百万台设备相对较小的操作人员。 日常的运营商希望远程处理设备操作、 批量，并只收到通知当出现问题时，需要直接关注。

- **开放性和兼容性**︰ IoT 设备生态系统是非常多样化。 管理工具必须进行定制，以适应多种设备类、 平台和协议。 运算符必须能够支持多种类型的设备，从受限程度最强的嵌入式单处理芯片，为功能强大且功能齐全的计算机。

- **上下文感知**︰ IoT 环境是动态的、 不断变化。 服务的可靠性至关重要。 SLA 维护窗口、 网络和电源状态、 使用条件和设备地理位置以确保该维护停机时间不会影响关键业务操作或创建危险状态必须处理设备管理操作。

- **服务很多角色**︰ 唯一的工作流和流程 IoT 操作角色的支持是至关重要的。 操作人员必须与给定的约束条件的内部 IT 部门 harmoniously 合作。  他们还必须找到可持续方式向主管和其他业务管理角色的曲面实时设备操作信息。

## <a name="iot-device-lifecycle"></a>IoT 设备生命周期

没有一套共有的所有企业 IoT 项目一般设备管理阶段。 在 Azure IoT 有 IoT 设备生命周期内的五个阶段︰

![五个 Azure IoT 设备生命周期阶段︰ 规划、 资源调配、 配置、 监视、 报废][img-device_lifecycle]

在以下五个阶段中，有几个设备操作员要求提供完整的解决方案应满足的︰

- **计划**︰ 启用创建设备元数据方案，使他们能够方便、 准确地查询，并针对一组设备进行批量管理操作的运算符。 设备两路输出可用于存储此设备元数据标记和属性的形式。

    *进一步阅读*︰[开始使用设备 twins][lnk-twins-getstarted]，[了解设备 twins][lnk-twins-devguide]，[如何使用两路输出属性][lnk-twin-properties]

- **规定**︰ 安全地调配 IoT 集线器到新设备并启用操作员立即了解设备的功能。  使用 IoT 集线器设备注册表创建灵活的设备标识和凭据，并使用作业批量执行此操作。 生成设备来报告他们的能力和条件通过设备双子星中的设备属性。

    *进一步阅读*︰[管理设备标识][lnk-identity-registry]，[批量管理设备标识的][lnk-bulk-identity]，[如何使用两路输出属性][lnk-twin-properties]

- **配置**︰ 促进批量配置更改和对设备的固件更新，同时可以保持健康和安全。 通过使用所需的属性或直接方法和广播的作业，批量执行这些设备管理操作。

    *进一步阅读*︰[直接使用][lnk-c2d-methods]，[在设备上的直接方法调用][lnk-methods-devguide]，[如何使用两路输出属性][lnk-twin-properties]，[计划和广播的作业][lnk-jobs]，[多台设备上的计划作业][lnk-jobs-devguide]

- **显示器**︰ 监视整个设备集合健康，正在进行的操作的状态，并向可能需要其注意的问题向操作员发出警报。  应用设备两路输出允许报告实时运行条件和状态的更新操作的设备。 生成功能强大的仪表板使用设备双子星查询报告该曲面的最直接的问题。

    *进一步阅读*︰[如何使用同步属性][lnk-twin-properties]， [twins 和作业的查询语言][lnk-query-language]

- **退废**︰ 替换或出现故障后退役设备，升级周期，或在服务生命周期的结束。  使用设备双子星维护设备信息的物理设备是否正被替换，或者如果要停存档。 用于安全地收回设备标识和凭据 IoT 集线器设备注册表。

    *进一步阅读*︰[如何使用同步属性][lnk-twin-properties]，[管理设备标识][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT 集线器设备管理模式

IoT 中心启用下面的一组设备管理模式。  [设备管理教程][lnk-get-started]显示详细如何扩展这些模式，以适应具体的情况和如何来设计这些核心模板所基于的新模式。

- **重新启动**-后端应用程序会通知设备通过直接的方法，它已开始重新启动。  该设备使用设备两路输出报告属性来更新该设备的重新启动状态。

    ![Azure IoT 集线器设备管理重新启动模式图形][img-reboot_pattern]

- **工厂重置**-后端应用程序通知设备通过直接的方法，它具有启动出厂重置。  该设备使用设备双子星要更新工厂的报告的属性重置的设备的状态。

    ![Azure IoT 集线器设备管理工厂重置模式图形][img-facreset_pattern]

- **配置**的后端应用程序使用设备两路输出所需属性来配置设备上运行的软件。  该设备使用设备两路输出报告属性来更新该设备的配置状态。

    ![Azure IoT 集线器设备管理配置模式图形][img-config_pattern]

- **固件更新**-后端应用程序会通知设备通过直接的方法，它具有启动固件更新。  设备启动多步骤的过程，要下载固件包，请应用固件包，并最终重新连接到 IoT 中心服务。  在整个多步过程，该设备使用设备两路输出报告要更新的进度和状态的设备的属性。

    ![Azure IoT 集线器设备管理固件更新模式图形][img-fwupdate_pattern]

- **报告进度和状态**的应用程序后端运行设备双子星查询，跨一组设备的状态和在这些设备上运行的操作的进度报告。

    ![报告进度和状态模式图形的 azure IoT 集线器设备管理][img-report_progress_pattern]

## <a name="next-steps"></a>下一步行动

您可以使用功能、 模式和 Azure IoT 集线器设备管理提供了，创建 IoT 满足企业 IoT 运算符中每个设备生命周期阶段中的应用程序的代码库。

要继续学习 Azure IoT 集线器设备管理功能的信息，请参阅[开始使用 Azure IoT 集线器设备管理][lnk-get-started]教程。

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md