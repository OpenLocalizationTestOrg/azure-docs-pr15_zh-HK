<properties
    pageTitle="组策略和每台 MDM 设置 |Microsoft Azure"
    description="提供有关组策略和移动设备的信息应在企业所拥有的设备中使用的管理 (MDM) 设置。 这些策略应用到用户的整个设备。"
    services="active-directory"
    keywords="组策略和企业状态漫游、 企业状态漫游和 windows 云的 MDM 设置是什么"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>组策略和每台 MDM 设置

只能对企业拥有的设备中使用这些组策略和移动设备管理 (MDM) 设置因为这些策略应用到用户的整个设备。 应用 MDM 策略来禁用个人设置同步，用户负责设备将带来负面影响该设备的使用。 此外，设备上的其他用户帐户也将受该策略。

想要管理的个人 （非托管） 设备漫游的企业可以使用 Azure 门户网站启用或禁用漫游，而不是使用组策略或 mdm。
下表描述了可用的策略设置。

## <a name="mdm-settings"></a>MDM 设置
MDM 策略设置应用于 Windows 10 和 Windows 10 移动。  Windows 10 移动支持 Microsoft 帐户基于漫游通过用户的 OneDrive 帐户的存在。  请"设备和终结点"的部分，以基于 Azure AD 同步支持哪些设备的详细信息，参阅。

| 名称                               | 说明                                                          |
|------------------------------------|----------------------------------------------------------------------|
| 允许 Microsoft 客户连接 | 允许用户在设备上使用 Microsoft 帐户进行身份验证 |
| 允许我设置的同步             | 允许用户在漫游 Windows 设置和应用程序数据;禁用此策略将禁用同步，以及移动设备上的备份                  |

## <a name="group-policy-settings"></a>组策略设置
组策略设置适用于 Windows 10 设备加入到 Active Directory 域中。 该表还包括传统设置，似乎管理同步设置，但不适用于企业状态漫游的 Windows 10，这与不使用说明中指出。

| 名称                                | 说明 |
|-------------------------------------|-------------|
| 帐户︰ 块 Microsoft 帐户  |通过此策略设置可防止用户在此计算机上添加新的 Microsoft 帐户|
| 同步                         |可防止用户漫游 Windows 设置和应用程序数据|
| 同步进行个性化设置             |禁用主题组的同步|
| 不同步浏览器设置        |禁用 Internet Explorer 组的同步|
| 不同步密码               |禁用密码组的同步|
| 不会同步其他 Windows 设置  |禁用其他窗口设置组的同步|
| 不同步桌面个性化设置 |请不要使用;不起作用|
| 在计量连接上同步  |禁用漫游在计量等手机 3g 连接，|
| 不同步应用程序                    |请不要使用;不起作用|
|不同步的应用程序设置             |禁用的应用程序数据漫游|
|不同步启动设置           |请不要使用;不起作用|


## <a name="related-topics"></a>相关的主题
- [企业状态漫游概述](active-directory-windows-enterprise-state-roaming-overview.md)
- [启用漫游在 Azure Active Directory 中的企业状态](active-directory-windows-enterprise-state-roaming-enable.md)
- [设置和数据漫游的常见问题解答](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10 漫游设置参考](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
