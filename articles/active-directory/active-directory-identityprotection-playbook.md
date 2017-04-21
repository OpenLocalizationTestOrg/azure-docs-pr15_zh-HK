<properties
    pageTitle="Azure 的活动目录标识保护行动手册 |Microsoft Azure"
    description="了解如何 Azure AD 身份信息保护使您能够限制攻击者的能力，利用破坏的身份或设备和安全标识或以前怀疑或已知受到破坏的设备。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure 的活动目录标识保护行动手册 

本操作手册可帮助您︰

- 通过模拟风险事件和漏洞填充标识保护环境中的数据
- 建立基于风险的条件访问策略和测试这些策略的影响


## <a name="simulating-risk-events"></a>模拟的风险事件

本节提供您与步骤模拟以下风险事件类型︰

- 从匿名登录的 IP 地址 （简单）
- 不熟悉的位置 （中等） 的登录
- 不可能旅行到典型位置 （困难）

以安全的方式，不能模拟其他风险事件。


### <a name="sign-ins-from-anonymous-ip-addresses"></a>从匿名登录的 IP 地址

这种风险事件类型标识用户已成功登录从已被标识为匿名代理服务器 IP 地址的 IP 地址。 使用这些代理服务器时要隐藏其设备的 IP 地址并可能用于恶意目的的人员。

**以模拟登录从一个匿名的 IP，请执行以下步骤**︰

1.  下载[或浏览器](https://www.torproject.org/projects/torbrowser.html.en)。
2.  使用或浏览器，定位到[https://myapps.microsoft.com](https://myapps.microsoft.com)。   
3.  输入您希望**号接匿名 IP 地址从**报表中显示的帐户的凭据。

签入将显示标识保护操控板上的在 5 分钟之内。 


###<a name="sign-ins-from-unfamiliar-locations"></a>来自不熟悉的位置登录

不熟悉的位置风险是认为过去的登录位置的实时登录评估机制 (IP、 纬度 / 经度和 ASN) 来确定新的 / 不熟悉的位置。 系统存储以前 IPs，纬度 / 经度和 Asn 的用户，并认为这些都是熟悉的位置。 如果登录的位置与任何现有的熟悉位置都不匹配，则被视为不熟悉登录的位置。

Azure 的 Active Directory 身份保护︰  

 - 具有初始学习期限为 14 天，在此期间则不会标记为不熟悉的位置的任何新位置。
 - 将忽略登录从熟悉的设备和位置，那些在地理位置上靠近现有的熟悉位置。

要模拟不熟悉的位置，您必须登录的位置和该帐户尚未签署中从之前的设备。 


**以模拟的登录不熟悉的位置，请执行以下步骤**︰

1.  选择具有最少 14 天登录历史记录的帐户。 

2.  执行两种操作︰
    
    一。 当使用 VPN，定位到[https://myapps.microsoft.com](https://myapps.microsoft.com) ，输入想要模拟的风险事件的帐户的凭据。

    b。 请让同仁在不同的位置要使用该帐户的凭据 （不推荐）。

签入将显示标识保护操控板上的在 5 分钟之内。
 
### <a name="impossible-travel-to-atypical-location"></a>不可能旅行到典型的位置
模拟不可能旅行条件是困难的因为该算法使用机器学习滤出如不可能旅行从熟悉的设备或从目录中的其他用户使用的 Vpn 登录的误判。 此外，算法需要 3 到 14 天内的登录历史记录用户生成的风险事件开始之前。

**模拟不可能旅行到典型的位置，请执行以下步骤**︰

1.  使用标准浏览器，定位到[https://myapps.microsoft.com](https://myapps.microsoft.com)。  

2.  请输入您想要生成不可能旅行风险事件的帐户的凭据。

3.  更改您的用户代理。 您可以从开发人员工具更改用户在 Internet Explorer 中的代理或更改用户代理在 Firefox 或使用用户代理切换器加载项的镶边。

4.  更改您的 IP 地址。 您可以通过使用 VPN 或加载项，或在 Azure 中不同的数据中心中的新机向上旋转更改您的 IP 地址。

5.  登录到[https://myapps.microsoft.com](https://myapps.microsoft.com)像以前一样使用相同的凭据和以前登录之后的几分钟内。

签入将显示标识保护仪表板中 2-4 小时内。<br>
由于复杂的机器学习模型所涉及的没有它将不获得挑选的机会。<br> 您可能想要复制多个 Azure 的广告帐户的这些步骤。


## <a name="simulating-vulnerabilities"></a>模拟的漏洞 
漏洞是坏参与者可以利用 Azure AD 环境中的弱点。 目前 3 种类型的漏洞都出来后在 Azure AD 身份保护利用 Azure 广告的其他功能。 这些漏洞后，将显示标识保护操控板上的自动设置这些功能。

-   Azure AD[多因素身份验证吗？](../multi-factor-authentication/multi-factor-authentication.md)
-   AD azure[云应用程序发现](active-directory-cloudappdiscovery-whatis.md)。
-   Azure AD[特权的身份管理](active-directory-privileged-identity-management-configure.md)。 



##<a name="user-compromise-risk"></a>用户泄露风险

**若要测试用户危害风险，请执行以下步骤**︰

1.  登录到具有全局管理员凭据的[https://portal.azure.com](https://portal.azure.com)为您的租户。

2.  导航到**身份保护**。 

3.  上主的**Azure AD 身份保护**刀片式服务器，单击**设置**。 

4.  在**门户网站设置**刀片式服务器下**安全规则**，, 单击**用户危害风险**。 

5.  **风险提供登录**刀片式服务器，将关闭，**启用规则**，然后单击**保存**设置。

6.  对于给定的用户帐户，模拟不熟悉的位置或匿名 IP 风险事件。 这会将该用户的用户风险级别提升至**中等**。

7.  请等待几分钟，并验证您的用户的用户级别是**媒体**。

8.  转到**门户网站设置**刀片式服务器。

9.  在**用户危害风险**刀片式服务器，**启用规则**，请在下选择**上**。 

10. 选择下列选项之一︰

    一。 若要阻止，请选择下**阻止登录** **** 。

    b。 要强制使用安全的密码更改，选择**需要多因素身份验证**下的**媒体**。

13. 单击**保存**。

14. 现在可以通过登录用户使用提升的权限的风险等级进行测试基于风险的条件访问。 如果用户风险为中等，具体取决于您的策略配置，您登录或者被阻止或被强制更改密码。 
<br><br>
![操作手册](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>登录的风险

 
**若要测试风险中的一个符号，请执行以下步骤︰**

1.  登录到具有全局管理员凭据的[https://portal.azure.com](https://portal.azure.com)为您的租户。

2.  导航到**身份保护**。

3.  上主的**Azure AD 身份保护**刀片式服务器，单击**设置**。 

4.  **门户网站设置**刀片式服务器下**安全规则**，, 单击**签名的风险**。

5.  **风险提供登录**刀片式服务器，请选择**上****启用规则**下。 

7.  选择下列选项之一︰

    一。 若要阻止，请选择下**阻止登录****中**

    b。 要强制使用安全的密码更改，选择**需要多因素身份验证**下的**媒体**。

8.  若要阻止，请下块登录选择媒体。

9.  要实施多因素身份验证，请选择**需要多因素身份验证**下的**媒体**。

10. 单击**保存**。

11. 现在可以通过模拟对不熟悉的位置或匿名 IP 风险事件，因为它们是两个**中等**风险事件进行测试基于风险的条件访问。

<br>
![操作手册](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>请参见

 - [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md)
