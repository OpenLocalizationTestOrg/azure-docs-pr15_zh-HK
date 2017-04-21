<properties
    pageTitle="Azure AD 连接︰ 启用写回设备 |Microsoft Azure"
    description="本文详细介绍了如何启用使用 Azure AD 连接的设备写回"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD 连接︰ 启用设备写回

>[AZURE.NOTE] 对 Azure AD 特优的订阅是必需的设备写回。

以下文档提供有关如何启用设备写回功能在 Azure AD 连接的信息。 在下列情况中使用设备写回︰

- 启用 ADFS 的设备所基于的条件访问 (2012 R2 或更高版本) 受保护的应用程序 （信赖方信任）。

这提供了更高的安全性和对应用程序的访问权限只授予受信任设备的保证。 条件的访问权限的详细信息，请参阅[管理风险条件访问](active-directory-conditional-access.md)并[设置内部条件访问使用 Azure 活动目录的设备注册](https://msdn.microsoft.com/library/azure/dn788908.aspx)。

>[AZURE.IMPORTANT]
<li>设备必须位于同一个林中的用户。 由于设备必须重新写入到单个林，此功能当前不支持使用多个用户目录林部署。</li>
<li>只有一个设备注册配置对象可以添加到内部部署 Active Directory 目录林。 此功能不兼容与内部部署 Active Directory 同步到多个 Azure 的广告目录位置的拓扑结构。</li>

## <a name="part-1-install-azure-ad-connect"></a>第 1 部分︰ 安装 Azure AD 连接
1. 安装使用自定义的 Azure AD 连接或快速设置。 Microsoft 建议开始所有用户和组都同步成功之前启用写回设备。

## <a name="part-2-prepare-active-directory"></a>第 2 部分︰ 准备活动目录
使用以下步骤来准备使用设备写回。

1.  从装有 Azure AD 连接的计算机，在提升模式下启动 PowerShell。

2.  如果未安装活动目录 PowerShell 模块，安装，请使用下面的命令︰

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. 如果未安装 Azure 活动目录 PowerShell 模块，然后下载并安装它从[Azure 活动目录模块用于 Windows PowerShell （64-位版本）](http://go.microsoft.com/fwlink/p/?linkid=236297)。 此组件具有登录的助手，Azure AD 连接与安装的依赖项。

4.  具有企业管理员凭据，运行以下命令，然后退出 PowerShell。

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

由于需要更改配置命名空间，则需要企业管理员凭据。 域管理员将没有足够的权限。

![启用设备写回 Powershell](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

说明︰

- 如果他们已不存在，创建并配置新的容器和对象在 CN = 设备注册配置，CN = 服务，CN = 配置，[林 dn]。
- 如果他们已不存在，创建并配置新的容器和对象在 CN = RegisteredDevices，[域的 dn]。 将在该容器中创建设备对象。
- 在 Azure AD 接口帐户，可管理您的活动目录的设备上设置必要的权限。
- 只需要运行一个目录林，即使 Azure AD 连接安装在多个目录林。

参数︰

- 域名︰ 在其中创建设备对象 Active Directory 域。 注意︰ 将在单个域中创建一个给定的活动目录林的所有设备。
- AdConnectorAccount: Active Directory 帐户的 Azure AD 连接将用来管理目录中的对象。 这是 Azure AD 连接同步用于连接到 AD 的帐户。 如果您安装了使用快速设置，则前缀为 MSOL_ 的帐户。

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>在 Azure AD 连接的第 3 部分︰ 启用设备写回
使用以下过程可以在 Azure AD 连接的设备写回。

1.  再次运行安装向导。 从附加任务页中选择**自定义同步选项**，然后单击**下一步**。
![自定义安装自定义的同步选项](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  在可选功能页中，设备写回不再将灰显。 请注意，如果 Azure AD 连接准备步骤已完成的设备写回将出灰的可选功能页中。 检查设备回写的框，然后单击**下一步**。 如果该复选框仍处于禁用状态，请参阅[疑难解答一节](#the-writeback-checkbox-is-still-disabled)。
![自定义安装设备写回可选功能](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  在写回页中，您将看到默认设备写回林提供的域。
![自定义安装设备写回目标目录林](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  完成安装向导，其中含有任何其他配置更改。 如果需要请参阅[Azure AD 连接的自定义安装。](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>启用条件访问
可以[设置内部条件访问使用 Azure 活动目录设备注册](https://msdn.microsoft.com/library/azure/dn788908.aspx)中获得来实现此方案的详细的说明。

## <a name="verify-devices-are-synchronized-to-active-directory"></a>确认设备与 Active Directory 同步
设备写回应该现在工作正常。 请注意，它可能会花 3 小时到 AD 要写回的设备对象。  要验证正确地同步您的设备，请执行以下同步规则完成后︰

1.  启动 Active Directory 管理中心。
2.  展开 RegisteredDevices，在联盟的域中。
![活动目录管理中心注册设备](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  列出当前已注册的设备。
![活动目录管理中心注册设备列表](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>故障排除

### <a name="the-writeback-checkbox-is-still-disabled"></a>回写复选框仍处于禁用状态
如果未启用设备写回的复选框，则即使您按照上面的步骤，以下步骤将指导您完成安装向导将验证框处于启用状态之前。

第一件事第一︰

- 请确保至少一个目录林中的 Windows 服务器 2012R2。 设备对象类型必须存在。
- 如果尚未运行安装向导，然后更改将不会检测。 在这种情况下，完成安装向导，然后重新运行它。
- 请确保您在初始化脚本中提供的帐户是真正正确的用户使用 Active Directory 连接器。 若要验证这一点，请执行以下步骤︰
    - 从开始菜单打开**同步服务**。
    - 打开**连接器**选项卡。
    - 找到与 Active Directory 域服务类型的连接器，然后选择它。
    - 在**操作**中，选择**属性**。
    - 转**到活动目录林连接**到。 请验证域名和用户指定此屏幕匹配向脚本提供的帐户。
![同步服务管理器中的连接器帐户](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

验证在 Active Directory 中的配置︰
- 验证设备注册服务位于下面的位置 (CN = DeviceRegistrationService，CN = 设备注册服务，CN = 设备注册配置，CN = 服务，CN = 配置) 配置命名上下文。

![解决问题，配置命名空间中的 DeviceRegistrationService](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- 验证有只有一个配置对象通过搜索的配置命名空间。 如果有多个，删除重复。

![排查、 搜索重复对象](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- 对设备注册服务的对象确保属性 msDS DeviceLocation 存在并且具有值。 查找此位置，请确保它是存在与对象类型 msDS-DeviceContainer。

![解决，msDS DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![解决，RegisteredDevices 对象类](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- 验证使用 Active Directory 连接器的帐户有注册设备容器通过上一步找到所需的权限。 这是此容器的预期的权限︰

![解决问题，请验证容器上的权限](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Active Directory 验证帐户是否有权限在 CN = 设备注册配置，CN = 服务，CN = 配置对象。

![解决问题，请验证设备注册配置权限](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>其他信息
- [条件接收与风险管理](active-directory-conditional-access.md)
- [设置内部条件访问使用 Azure 活动目录设备注册](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
