<properties
    pageTitle="了解详细信息︰ Azure AD 密码管理 |Microsoft Azure"
    description="Azure AD 密码管理，包括密码写回的工作原理，密码写回安全、 如何重新设置密码的门户的工作方式，以及哪些数据使用的密码重置的高级的主题。"
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>了解更多有关密码管理

> [AZURE.IMPORTANT] **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。

如果您已经部署了密码管理，或者只希望多了解些具体功能的部署之前的工作方式，本部分将为您提供服务的技术概念很好地概括的技术。 我们将介绍以下内容︰

* [**密码写回概述**](#password-writeback-overview)
  - [密码写回的工作原理](#how-password-writeback-works)
  - [方案支持的密码写回](#scenarios-supported-for-password-writeback)
  - [密码写回安全模型](#password-writeback-security-model)
* [**密码重新门户的工作有什么设置？**](#how-does-the-password-reset-portal-work)
  - [密码重置使用什么数据？](#what-data-is-used-by-password-reset)
  - [如何访问密码重置为您的用户数据](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>密码写回概述
密码写回是启用和 Azure 活动目录津贴的当前订阅服务器使用[Azure 活动目录连接](active-directory-aadconnect.md)组件。 有关详细信息，请参阅[Azure 活动目录版本](active-directory-editions.md)。

写回密码允许您配置您的云租户写给您的密码部署 Active Directory。  该项目则无无需设置和管理一个复杂的内部部署自助服务密码重置解决方案，并提供简便的基于云的方法为您的用户重置其内部密码，无论他们身在何处。  请继续阅读一些密码写回的主要功能︰

- **零延迟反馈。**  密码写回是同步操作。  如果不符合策略或者不能够重置或由于某种原因更改他们的密码，将立即通知用户。
- **重置密码，使用户使用 AD FS 或其他联合身份验证技术的支持。**  用密码写回，只要联合的用户帐户会同步到您的 Azure AD 租户，他们将能够管理其内部云从 AD 密码。
- **重设密码的用户使用密码哈希同步的支持。** 密码重置服务检测到密码哈希同步中启用了同步的用户帐户时，我们重新设置此帐户的内部和云密码同时。
- **从访问权限面板和 Office 365 更改密码的支持。**  当联合或密码同步用户来更改其过期或未过期的密码，我们将写这些密码回本地广告环境。
- **支持写回密码，当管理员重置它们从**[**Azure 的管理门户**](https://manage.windowsazure.com)。  只要在[Azure 管理门户](https://manage.windowsazure.com)，如果该用户联合或密码同步的用户的密码将管理员重置，我们将设置管理员选择本地广告，也的密码。  这是当前不支持在办公室管理门户。
- **强制使用您内部 AD 密码策略。**  当用户重置其密码时，我们确保它满足您在部署之前将它提交到该目录的广告策略。  这包括历史、 复杂性、 年龄、 密码筛选器和任何其他已定义在您的本地广告的密码限制。
- **不需要任何入站的防火墙规则。**  写回密码为基础的通信通道，意味着您不需要打开任何使用该功能的防火墙上的入站的端口使用 Azure 服务总线中继。
- **不支持您的内部部署 Active Directory 中的受保护组中的用户帐户。** 有关受保护组的详细信息，请参阅[受保护的帐户和组在 Active Directory 中](https://technet.microsoft.com/library/dn535499.aspx)。

### <a name="how-password-writeback-works"></a>密码写回的工作原理
密码回写有三个主要组件︰

- 密码重置云服务 （这也集成到 Azure 广告的密码更改页）
- 特定于租户的 Azure 服务总线中继
- 在 prem 密码重置的终结点

它们结合在一起所述如下图︰

  ![][001]

当同步联合或密码哈希将用户方面重置或更改他或她的密码，在云中，将发生以下情况︰

1.  我们要检查哪种类型的密码的用户都有。  如果我们看到在场所管理密码，我们确保写回服务已启动并正在运行。  如果是这样，我们让用户执行操作时，如果不是，我们让用户不能在此处重置其密码。
2.  接下来，用户通过适当的身份验证关口，达到重置密码屏幕。
3.  用户选择一个新密码，并确认它。
4.  在单击提交，我们使用写回安装过程中创建一个对称密钥加密的明文密码。
5.  加密密码之后, 我们将其包括在负载通过 HTTPS 通道发送给您组织特定的服务总线中继 （即我们还为您设置回写安装过程中）。  受此中继您的内部部署安装知道随机生成密码。
6.  一旦消息到达服务总线，密码重置终结点将自动苏醒，并发现它具有挂起的重置请求。
7.  该服务然后寻找用户问题通过云定位属性。  为取得成功，该查阅用户对象必须存在于广告连接器空间、 必须链接到相应的 MV 对象，和它必须链接到相应的 AAD 连接器对象。 最后，为了使同步来查找此用户帐户，MV AD 连接器对象之间的链接必须具有同步规则`Microsoft.InfromADUserAccountEnabled.xxx`的链接。  这需要因为同步引擎时打来电话，从云中，使用 cloudAnchor 属性来查找 AAD 的连接器空间对象，然后回 MV 对象，遵循该链接，然后回 AD 对象跟随该链接。 因为有可能为同一用户的多个 AD 对象 （多林），同步引擎将依靠`Microsoft.InfromADUserAccountEnabled.xxx`链接来选取正确的一个。
8.  一旦找到的用户帐户，我们尝试重置密码直接在合适的 AD 林。
9.  如果密码设置操作是成功的我们会告诉的用户已修改自己的密码，他们可以继续他们顺利。
10. 如果操作失败时设置了密码，我们将错误返回给用户，让他们再试一次。  操作可能失败，因为服务已关闭，因为他们所选的密码不符合组织的策略，因为我们找不到用户在本地的广告，或任何数目的原因。  我们很多这种情况下有特定的消息，告诉用户可以做些什么来解决问题。

### <a name="scenarios-supported-for-password-writeback"></a>方案支持的密码写回
下表描述了我们的同步功能的版本支持哪些方案。  一般情况下，强烈建议您安装最新版本的[Azure AD 连接](active-directory-aadconnect.md#install-azure-ad-connect)，如果您想要使用的密码写回。

  ![][002]

### <a name="password-writeback-security-model"></a>密码写回安全模型
密码写回是一种高度安全、 可靠的服务。  为了确保您的信息受到保护，我们让 4 分层的安全模型，如下所述。

- **租户特定服务总线中继**– 设置服务时，我们设置了永远不会有权访问 Microsoft 随机生成强密码保护的特定于租户的服务总线中继。
- **向下强加密的密码加密密钥锁定**– 创建服务总线中继后，我们创建强对称密钥的加密密码，随着在线路上说，我们使用。  此项仅在您的公司机密存储在云中，这很大程度是锁定和审核，就像在目录中的任何密码生存之地。
- **行业标准 TLS** — 时，密码重置或更改操作发生在云环境中，我们会明文密码，并使用您的公钥加密它。  我们然后放置，到通过使用您的服务总线中继到微软 SSL 证书加密通道发送 HTTPS 消息。  到服务总线的消息到达后，prem 上代理唤醒向服务总线使用以前生成的强密码进行身份验证、 领取在加密的邮件、 使用私有密钥生成，然后再尝试通过 AD DS SetPassword API 将密码设置将其解密。  此步骤在云中是什么使我们能够强制广告上 prem 密码策略 （复杂性、 年龄、 历史记录、 筛选等）。
- **邮件过期策略**— 最后，如果由于某种原因搁置的消息在服务总线因为上 prem 服务已关闭，则将超时并删除几分钟后，这样才能提高安全性更进一步。

## <a name="how-does-the-password-reset-portal-work"></a>密码重新门户的工作有什么设置？
当用户导航到密码重设的门户网站，启动工作流以确定该用户帐户是否有效，哪些用户所属的那个用户的密码托管的并且无论是否已授权用户使用功能的组织。  通读下面的步骤来了解有关密码重置页背后的逻辑。

1.  单击用户不能访问您的帐户链接或直接变成[https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com)。
2.  用户输入的用户 id，并通过 captcha。
3.  Azure AD 验证用户是否可以使用此功能执行以下操作︰
    - 检查用户具有启用此功能并分配一个 Azure 的广告许可证。
        - 如果用户没有启用此功能或分配的许可证，则要求用户与他或她要重置其密码的管理员联系。
    - 检查用户有权限的挑战在他或她的帐户，由于管理员策略中定义的数据。
        - 如果策略要求只能有一个挑战，它被保证用户具有适当的数据定义为至少由管理员策略启用的挑战之一。
          - 如果没有配置该用户，然后建议用户要与他或她的管理员联系，以重置其密码。
        - 如果策略要求两个难题，它被保证用户具有适当的数据定义为至少两个管理员策略启用的挑战。
          - 如果没有配置该用户，然后该用户是我们建议与他或她的管理员联系，以重置其密码。
    - 检查用户的密码托管在场所 （联合或密码哈希同步了）。
       - 如果部署回写并且在场所管理用户的密码，然后允许用户继续进行身份验证和重置其密码。
       - 如果没有部署回写在场所管理用户的密码，然后将要求用户与他或她要重置其密码的管理员联系。
4.  如果确定该用户能够成功地重置其密码，然后用户指导完成重置过程。

了解更多有关如何部署在密码写回[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)。

### <a name="what-data-is-used-by-password-reset"></a>密码重置使用什么数据？
下表概要列出此数据在何处以及如何使用密码重置期间，旨在帮助您确定适合您的组织的身份验证选项。 该表还显示任何格式要求的情况下您代表从输入的路径不验证这些数据的用户提供数据。

> [AZURE.NOTE] 办公室电话由于用户目前不可以编辑目录中的此属性出现在注册门户。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>联系人的方法名称</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure 的 Active Directory 数据元素</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>使用 / 可设置位置吗？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>格式要求</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>办公室电话</p>
            </td>
            <td>
              <p>电话号码</p>
              <p>如范围内设置 MsolUserJWarner@contoso.com的电话号码"x 1234 + 1 1234567890"</p>
            </td>
            <td>
              <p>在使用︰</p>
              <p>密码重置门户</p>
              <p>从可设置︰</p>
              <p>电话号码是可从 PowerShell、 目录同步，Azure 管理门户和办公室管理门户设置</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (例如 + 1 1234567890)</p>
              <ul>
                <li class="unordered">
必须提供国家/地区代码<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须提供一个区域代码 （如果适用）<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须有提供一个 + 在前面的国家/地区代码<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须有国家/地区代码和编号的其余部分之间的空间<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
不支持扩展的如果您有任何指定的扩展名，我们将它从数之前删除调度电话呼叫。<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>移动电话</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OR</p>
              <p>MobilePhone</p>
              <p>（电话存在的数据是否使用的身份验证，否则这会回到移动电话字段）。</p>
              <p>如范围内设置 MsolUser JWarner@contoso.com -MobilePhone"x 1234 + 1 1234567890"</p>
            </td>
            <td>
              <p>在使用︰</p>
              <p>密码重置门户</p>
              <p>注册门户</p>
              <p>从可设置︰ </p>
              <p>AuthenticationPhone 可设定在密码重置注册门户或 MFA 注册门户。</p>
              <p>MobilePhone 是可从 PowerShell、 目录同步，Azure 管理门户和办公室管理门户设置</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (例如 + 1 1234567890)</p>
              <ul>
                <li class="unordered">
必须提供国家/地区代码。<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须提供一个区域代码 （如果适用）。<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须有提供一个 + 在前面的国家/地区代码。<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
必须有国家/地区代码和编号的其余部分之间的空间。<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
不支持扩展的如果您有任何指定的扩展，我们不必理会时调度电话呼叫。<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>备选电子邮件</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OR</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>（如果存在数据使用的电子邮件身份验证，否则这会回到备用电子邮件字段）。</p>
              <p>注意︰ 在目录中的字符串数组的形式指定备用电子邮件字段。  我们使用此数组中的第一个条目。</p>
              <p>如范围内设置 MsolUser JWarner@contoso.com -AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>在使用︰</p>
              <p>密码重置门户</p>
              <p>注册门户</p>
              <p>从可设置︰ </p>
              <p>AuthenticationEmail 可设定在密码重置注册门户或 MFA 注册门户。</p>
              <p>AlternateEmail 是可从 PowerShell，Azure 管理门户和办公室管理门户设置</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>或甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
电子邮件应按照为每个标准格式。<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
支持 Unicode 的电子邮件。<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>安全问题和解答</p>
            </td>
            <td>
              <p>不能直接在目录中修改。</p>
            </td>
            <td>
              <p>在使用︰</p>
              <p>密码重置门户</p>
              <p>注册门户 </p>
              <p>从可设置︰ </p>
              <p>设置安全问题的唯一办法是通过 Azure 管理门户。</p>
              <p>设置给定用户的安全问题的答案的唯一方法是通过注册门户。</p>
            </td>
            <td>
              <p>安全问题有最大值为 200 个字符和 3 个字符的最小值</p>
              <p>答案有 40 个字符的最大值和最小的 3 个字符</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>如何访问密码重置为您的用户数据
####<a name="data-settable-via-synchronization"></a>数据可通过同步设置
以下字段可以从内部进行同步︰

* 移动电话
* 办公室电话

####<a name="data-settable-with-azure-ad-powershell"></a>可使用 Azure AD PowerShell 设置数据
下列字段是使用 Azure AD PowerShell 和图形 API 可访问︰

* 备选电子邮件
* 移动电话
* 办公室电话
* 验证电话
* 验证电子邮件

####<a name="data-settable-with-registration-ui-only"></a>只有注册用户界面可设置数据
以下字段才通过 SSPR 注册用户界面 (https://aka.ms/ssprsetup):

* 安全问题和解答

####<a name="what-happens-when-a-user-registers"></a>当用户注册时，会发生什么情况？
当用户注册时，注册页面将**始终**设置以下字段︰

* 验证电话
* 验证电子邮件
* 安全问题和解答

如果您的**移动电话**或**其他电子邮件**提供了值，用户可以立即使用重置其密码，即使他们还没有为此服务注册。  此外，用户将在第一次注册时，将显示这些值，并根据自己的意愿对其进行修改。  但是，他们成功登记后，这些值将保留在**身份验证电话**和**身份验证电子邮件**字段中，分别。

这可以是一种解除阻止大量用户，同时仍允许用户验证此信息通过注册过程使用 SSPR。

####<a name="setting-password-reset-data-with-powershell"></a>设置密码重置使用 PowerShell 的数据
您可以设置与 Azure AD PowerShell 以下字段的值。

* 备选电子邮件
* 移动电话
* 办公室电话

若要开始，请先[下载](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)并安装 Azure AD PowerShell 模块。  一旦您已安装，您可以按照下面的步骤来配置每个字段。

#####<a name="alternate-email"></a>备选电子邮件
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>移动电话
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>办公室电话
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>阅读密码重置使用 PowerShell 的数据
您可以阅读使用 Azure AD PowerShell 以下字段的值。

* 备选电子邮件
* 移动电话
* 办公室电话
* 验证电话
* 验证电子邮件

若要开始，请先[下载](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)并安装 Azure AD PowerShell 模块。  一旦您已安装，您可以按照下面的步骤来配置每个字段。

#####<a name="alternate-email"></a>备选电子邮件
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>移动电话
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>办公室电话
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>验证电话
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>验证电子邮件
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>链接到密码重置文档
下面是 Azure AD 密码重置文档页的所有链接︰

* **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。
* [**它的工作原理**](active-directory-passwords-how-it-works.md)的了解有关的六个不同部分的服务和每个站点都
* [**入门**](active-directory-passwords-getting-started.md)-了解如何允许用户重置并更改其云或内部密码
* [**自定义**](active-directory-passwords-customize.md)-了解如何自定义的外观和感觉和行为与您的组织需要的服务
* [**最佳做法**](active-directory-passwords-best-practices.md)-了解如何快速部署和有效地管理您的组织中的密码
* [**深入**](active-directory-passwords-get-insights.md)的了解我们集成的报告功能
* [**常见问题解答**](active-directory-passwords-faq.md)-获取常见问题的答案
* [**故障排除**](active-directory-passwords-troubleshoot.md)-了解如何快速排查服务问题



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
