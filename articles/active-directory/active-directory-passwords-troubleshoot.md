<properties
    pageTitle="故障排除︰ Azure AD 密码管理 |Microsoft Azure"
    description="Azure AD 密码管理，包括重置、 更改、 写回、 注册、 常见故障排除步骤和哪些信息要包括何时寻求帮助。"
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>如何解决密码管理

> [AZURE.IMPORTANT] **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。

如果有密码管理的问题，我们愿意随时提供帮助。 您可能会遇到的大多数问题可以解决与几个简单的故障排除步骤您可以读取有关下面来解决您的部署︰

* [**您需要帮助时要包括的信息**](#information-to-include-when-you-need-help)
* [**在 Azure 管理门户中的密码管理配置问题**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**在 Azure 管理门户中的密码管理报告的问题**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**问题使用密码重置注册门户**](#troubleshoot-the-password-reset-registration-portal)
* [**问题的密码重新设置门户网站**](#troubleshoot-the-password-reset-portal)
* [**用密码写回的问题**](#troubleshoot-password-writeback)
  - [密码写回事件日志错误代码](#password-writeback-event-log-error-codes)
  - [写回密码连接问题](#troubleshoot-password-writeback-connectivity)

如果您已尝试下面的故障排除步骤，仍遇到问题，可以在[Azure 广告论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)上发布问题或与支持部门联系，一旦我们可以我们来看一看您的问题。

## <a name="information-to-include-when-you-need-help"></a>您需要帮助时要包括的信息

如果不能解决您的问题与下面的指南，您可以与我们的支持工程师。 当与他们联系时，建议包括以下信息︰

 - **常规错误说明**– 什么确切的错误消息未用户请参阅？  如果有任何错误消息，说明您注意到详细的意外的行为。
 - **页面**– 哪些页面是您上时您所看到的错误 （包括 URL）？
 - **日期 / 时间 / 时区**– 以前的精确日期和时间看到错误 （包括时区）？
 - **支持代码**– 支持代码生成时用户将看到错误 （以找到此、 重现该错误，然后单击位于屏幕底部的支持代码链接并发送所生成的 GUID 的支持工程师） 的内容。
   - 如果您没有在底部支持代码的页上，按下 f12 键和搜索的 SID 和 CID 和那些两个结果发送给技术支持工程师。

    ![][001]

 - **用户 ID** – （例如看到错误的用户 ID 是什么user@contoso.com)?
 - **有关用户的信息**– 被联合的用户、 密码哈希同步，仅云吗？  用户没有分配 AAD 津贴或 AAD 基本许可证？
 - **应用程序事件日志**– 如果您正在使用的密码写回和错误是在内部部署基础结构，请压缩从 Azure AD 连接服务器应用程序事件日志的一个副本和与您的请求一起发送。

包括此信息将帮助我们尽可能快地解决您的问题。


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>对 Azure 管理门户中的密码重置配置进行疑难解答
如果您遇到的错误配置的密码重置时，您可能能够通过执行下列疑难解答步骤来解决它︰

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>错误情况下</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>用户看到什么错误？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解决方案</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>我看不到 Azure 管理门户中的<strong>配置</strong>选项卡下的<strong>用户密码重置策略</strong>部分</p>
            </td>
            <td>
              <p>在 Azure 管理门户中的<strong>配置</strong>选项卡上看不到<strong>用户密码重置策略</strong>部分。</p>
            </td>
            <td>
              <p>如果您没有执行此操作的管理员分配 AAD 津贴或 AAD 基本许可证，这可能发生。 </p>
              <p>若要进行纠正，AAD 津贴或 AAD 基本许可证给管理员帐户问题通过导航到<strong>许可证</strong>选项卡，然后重试。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>看不到任何文档中描述的配置选项，<strong>用户密码重置策略</strong>部分下。</p>
            </td>
            <td>
              <p><strong>用户密码重置策略</strong>部分是可见的但在其下面显示的唯一标志是<strong>为重新设置密码的用户启用</strong>的标志。</p>
            </td>
            <td>
              <p>切换<strong>用户启用密码重置</strong>标志时，将显示用户界面的其余部分<strong>是。</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>我看不到一个特定的配置选项。</p>
            </td>
            <td>
              <p>例如，我看不<strong>数天之前用户必须确认其联系人数据</strong>选项时滚动了<strong>用户密码重置策略</strong>部分 （或相同问题的其他示例）。</p>
            </td>
            <td>
              <p>许多用户界面元素处于隐藏状态，直到需要。 尝试启用页上的所有选项，如果您想要查看。</p>
              <p>有关所有可用的控件的详细信息，请参阅<a href="active-directory-passwords-customize.md#password-management-behavior">密码管理行为</a>。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>我看不到<strong>回写到内部的密码</strong>配置选项</p>
            </td>
            <td>
              <p>Azure 管理门户中的<strong>配置</strong>选项卡下看不到了<strong>回写到内部的密码</strong>选项</p>
            </td>
            <td>
              <p>如果您已下载 Azure AD 连接并配置密码写回，此选项才可见。 完成这些操作后，该选项会出现，允许您启用或禁用写回从云。</p>
              <p>如何执行此操作的详细信息，请参阅<a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">在 Azure AD 连接中启用密码写回</a>。</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>在 Azure 管理门户中的密码管理报表疑难解答
如果使用密码管理报表时遇到错误，您可能能够通过执行下列疑难解答步骤来解决它︰

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>错误情况下</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>用户看到什么错误？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解决方案</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>我看不到任何密码管理报告</p>
            </td>
            <td>
              <p><strong>密码重置活动</strong>和<strong>重设密码注册的活动</strong>的报告是不可见的<strong>活动日志</strong>报表在<strong>报表</strong>选项卡下。</p>
            </td>
            <td>
              <p>如果您没有执行此操作的管理员分配 AAD 津贴或 AAD 基本许可证，这可能发生。 </p>
              <p>若要进行纠正，AAD 津贴或 AAD 基本许可证给管理员帐户问题通过导航到<strong>许可证</strong>选项卡，然后重试。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户登记显示多次</p>
            </td>
            <td>
              <p>当用户注册备用电子邮件、 移动电话和安全问题时，他们每个显示为单独的行，而不是一行。</p>
            </td>
            <td>
              <p>目前，当用户注册时，我们不能假定它们将注册在注册页面上的所有内容。 因此，我们当前记录每条单独的注册为一个单独的事件的数据。</p>
              <p>如果您想要此数据聚合，可以下载报告并打开数据与数据透视表在 excel 具有更大的灵活性。</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>密码重置注册门户的疑难解答
如果注册用户在密码重置时遇到一个错误，您可能能够通过执行下列疑难解答步骤来解决它︰

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>错误情况下</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>用户看到什么错误？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解决方案</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>密码重置为不启用目录</p>
            </td>
            <td>
              <p>您的管理员不启用您要使用此功能。</p>
            </td>
            <td>
              <p>切换<strong>用户启用密码重置</strong>标志为<strong>是</strong>并在 Azure 管理门户目录配置选项卡上单击<strong>保存</strong>。 您必须具有 Azure AD 津贴或基本许可证分配给管理员执行此操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户不具有分配 AAD 津贴或 AAD 基本许可证</p>
            </td>
            <td>
              <p>您的管理员不启用您要使用此功能。</p>
            </td>
            <td>
              <p>将 Azure AD 津贴或 Azure AD 基本许可证分配给 Azure 管理门户中的<strong>许可证</strong>选项卡下的用户。 您必须具有 Azure AD 津贴或基本许可证分配给管理员执行此操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>处理请求时出错</p>
            </td>
            <td>
              <p>用户将看到错误消息︰</p>
              <p>

              </p>
              <p>处理请求时出错 </p>
              <p>在尝试重置密码。</p>
            </td>
            <td>
              <p>这由很多问题，但此错误通常由任一服务中断或配置问题不能解决的。 </p>
              <p>如果您看到此错误，它将影响您的业务，请与支持部门联系，我们将帮助您尽快。 请参阅<a href="#information-to-include-when-you-need-help">信息包括在您需要帮助</a>，则应提供与支持工程师，以帮助快速的解决方法，请参阅。</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>密码重置门户的疑难解答
如果您遇到的错误，在重置用户的密码时，您可能能够通过执行下列疑难解答步骤来解决它︰

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>错误情况下</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>用户看到什么错误？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解决方案</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>密码重置为不启用目录</p>
            </td>
            <td>
              <p>您的帐户未启用密码重置</p>
              <p>很抱歉，您的管理员没有设置您的帐户以用于此服务。 </p>
              <p>

              </p>
              <p>如果您愿意，我们可以与您的组织中管理员联系来重置您的密码。</p>
            </td>
            <td>
              <p>切换<strong>用户启用密码重置</strong>标志为<strong>是</strong>并在 Azure 管理门户目录配置选项卡上单击<strong>保存</strong>。 您必须具有 Azure AD 津贴或基本许可证分配给管理员执行此操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户不具有分配 AAD 津贴或 AAD 基本许可证</p>
            </td>
            <td>
              <p>虽然我们不能自动重置非管理员帐户密码，我们可以联系到您的组织的管理人员为您代劳。</p>
            </td>
            <td>
              <p>将 Azure AD 津贴或 Azure AD 基本许可证分配给 Azure 管理门户中的<strong>许可证</strong>选项卡下的用户。 您必须具有 Azure AD 津贴或基本许可证分配给管理员执行此操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>目录启用密码重置，但用户已丢失或存在格式错误的身份验证信息</p>
            </td>
            <td>
              <p>您的帐户未启用密码重置</p>
              <p>很抱歉，您的管理员没有设置您的帐户以用于此服务。 </p>
              <p>

              </p>
              <p>如果您愿意，我们可以与您的组织中管理员联系来重置您的密码。</p>
            </td>
            <td>
              <p>请确保该用户具有正确联系人数据文件中的目录之前。 有关如何配置身份验证信息的目录中，以便用户不会看到此错误的信息，请参阅<a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">数据使用的密码重置</a>。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>目录启用密码重置，但用户只有一条联系人数据文件上设置策略需要两个验证步骤</p>
            </td>
            <td>
              <p>您的帐户未启用密码重置</p>
              <p>很抱歉，您的管理员没有设置您的帐户以用于此服务。 </p>
              <p>

              </p>
              <p>如果您愿意，我们可以与您的组织中管理员联系来重置您的密码。</p>
            </td>
            <td>
              <p>在继续操作前请确保该用户具有至少两个正确配置联系方法 （例如，移动电话和办公电话）。 有关如何配置身份验证信息的目录中，以便用户不会看到此错误的信息，请参阅<a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">数据使用的密码重置</a>。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>目录已启用密码重置，并正确配置用户，但用户将无法连接到 </p>
            </td>
            <td>
              <p>天哪 ！  我们与您联系时遇到意外的错误。</p>
            </td>
            <td>
              <p>这可能是一个临时服务错误的结果或配置错误的我们无法正确检测到联系人数据。 如果用户等待 10 秒钟，再试一次，出现"联系您的管理员联系"链接。 再次单击重试将重新派发该调用，而单击"联系您的管理员"将向用户、 密码或请求密码重置为该用户帐户执行 （按优先级顺序） 的全局管理员发送表单的电子邮件。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户永远不会接收密码重置 SMS 或电话联络</p>
            </td>
            <td>
              <p>用户单击"文本我"或"请呼叫我"，然后永远收不到任何东西。</p>
            </td>
            <td>
              <p>这可能是出现在目录中的格式不正确的电话号码。 请确保电话号码的格式是"+ ccc xxxyyyzzzzXeeee"。 若要了解有关格式电话使用密码重置的数字请参见<a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">数据使用的密码重置</a>。</p>
              <p>如果您需要扩展路由到当前用户，请注意，密码重置不支持扩展，即使您指定一个目录 （它们去除之前调用将被调度） 中。 尝试使用不带扩展名，数字或将扩展集成到您的 PBX 中的电话号码。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户永远不会接收密码重置电子邮件</p>
            </td>
            <td>
              <p>用户单击"电子邮件我"，然后永远收不到任何东西。</p>
            </td>
            <td>
              <p>为解决此问题的最常见原因是，垃圾邮件筛选器拒绝该消息。 请检查您的垃圾邮件，电子邮件的垃圾邮件，或删除项目文件夹。</p>
              <p>另外，请确保您正在检查正确的电子邮件消息...，很多人具有非常相似的电子邮件地址并结束检查错误的邮件收件箱。 如果这两个选项都不起作用，也可能是在目录中的电子邮件地址不正确，请检查以确保电子邮件地址正确，然后重试。 若要了解有关格式的电子邮件地址重新设置密码的使用请参阅<a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">哪些数据使用的密码重置</a>。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>我已经设置密码重置策略，但当管理员帐户使用密码重置，该策略不会应用</p>
            </td>
            <td>
              <p>重置其密码的管理员帐户请参阅启用密码重置、 电子邮件和移动电话，无论何种策略设置在<strong>用户密码重置策略</strong>部分<strong>配置</strong>选项卡下的选项相同。</p>
            </td>
            <td>
              <p>在<strong>配置</strong>选项卡的<strong>用户密码重置策略</strong>部分配置的选项仅适用于您的组织中的最终用户。</p>
              <p>Microsoft 管理并控制管理员重设密码策略以确保最高的安全级别</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>用户阻止试图在一天内多次重置密码</p>
            </td>
            <td>
              <p>用户将看到一条错误指出︰</p>
              <p>

              </p>
              <p>请使用另一种方法。</p>
              <p>您尝试验证您的帐户在最后 1 小时次数过多。 出于安全原因，您需要等待 24 小时之前您可以再试一次。 </p>
              <p>如果您愿意，我们可以与您的组织中管理员联系来重置您的密码。</p>
            </td>
            <td>
              <p>我们到阻止用户尝试重置其密码，在较短时间内多次从实现的自动调节机制。 这种情况发生时︰</p>
              <ol class="ordered">
                <li>
用户尝试验证电话号码在 1 小时内 5 次。<br\><br\></li>
                <li>
用户尝试在一小时内 5 次使用入口的安全问题。<br\><br\></li>
                <li>
用户尝试在一小时内 5 次置相同的用户帐户的密码。<br\><br\></li>
              </ol>
              <p>若要解决此问题，指示用户等待 24 小时后的最后一次尝试，并且用户将能够重置其密码。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>他或她的电话号码进行验证时，用户将看到错误</p>
            </td>
            <td>
              <p>尝试验证电话使用作为身份验证方法，用户将看到一条说明︰</p>
              <p>

              </p>
              <p>指定了不正确的电话号码。</p>
            </td>
            <td>
              <p>输入的电话号码与文件上的电话号码不匹配时，将发生此错误。</p>
              <p>请确保用户输入完整的电话号码，包括区域和国家/地区的代码中，当尝试使用密码重置基于电话的方法。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>处理请求时出错</p>
            </td>
            <td>
              <p>用户将看到错误消息︰</p>
              <p>

              </p>
              <p>处理请求时出错 </p>
              <p>在尝试重置密码。</p>
            </td>
            <td>
              <p>这由很多问题，但此错误通常由任一服务中断或配置问题不能解决的。 </p>
              <p>如果您看到此错误，它将影响您的业务，请与支持部门联系，我们将帮助您尽快。 请参阅<a href="#information-to-include-when-you-need-help">信息包括在您需要帮助</a>，则应提供与支持工程师，以帮助快速的解决方法，请参阅。</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>解决密码写回
如果您遇到错误时启用、 禁用或使用密码写回，您可能能够通过执行下列疑难解答步骤来解决它︰

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>错误情况下</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>用户看到什么错误？</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解决方案</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>常规服务和启动故障</p>
            </td>
            <td>
              <p>在 Azure AD 连接计算机的应用程序事件日志中的错误 6800 场所，密码重置服务不启动。</p>
              <p>

              </p>
              <p>服务，联合或密码哈希后同步的用户不能重置其密码。</p>
            </td>
            <td>
              <p>当启用密码写回时，同步引擎将调用写回库执行 （服务） 的配置与云服务服务。 在服务期间或密码写回启动 WCF 终结点时遇到的任何错误将导致您 Azure AD 连接的计算机的事件日志中的事件日志中的错误。</p>
              <p>期间 ADSync 服务重新启动，如果配置写回，WCF 终结点将开始。 但是，如果启动的终结点失败，我们将只是记录事件 6800，让同步服务启动。 此事件表示终结点不启动密码写回。 按组件将表明为什么终结点无法启动 PasswordResetService 生成事件日志事件 (6800) 以及事件日志条目的详细信息。 查看这些事件日志错误，然后尝试重新启动 Azure 的 AD 连接，如果密码写回仍不能正常工作。 如果问题仍然存在，请禁用并重新启用密码写回。</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>当用户尝试重置密码或解锁帐户密码写启用的时则操作将失败。 此外，您会看到在 Azure AD 连接事件日志包含事件:"同步引擎返回错误 hr = 800700CE，邮件 = 文件名或扩展名太长"解除锁定操作发生后。
                            </p>
            </td>
            <td>
              <p>如果必须升级旧版本的 Azure AD 连接或目录同步，这会发生。 升级到旧版本的 Azure AD 连接设置 Azure 广告管理代理帐户 （较新版本将设置 127 的字符长度的密码） 254 个字符的密码。 这种长密码用于 AD 接口导入和导出操作，但解除锁定操作不支持。
                            </p>
            </td>
            <td>
              <p>[查找 Active Directory 帐户](active-directory-aadconnect-accounts-permissions.md#active-directory-account)Azure AD 连接和重置密码中包含不能超过 127 个字符。 然后从开始菜单打开**同步服务**。 定位到**连接器**并查找**Active Directory 连接器**。 选择它并单击**属性**。 导航到**凭据**页并输入新的密码。 请选择**确定**关闭该页面。
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>在 Azure AD 连接安装过程中配置写回错误。</p>
            </td>
            <td>
              <p>在 Azure AD 连接安装过程的最后一步，您可以看到一个错误，指示无法配置密码写回。</p>
              <p>

              </p>
              <p>Azure 的 AD 连接应用程序事件日志中包含错误 32009 型"错误获取身份验证令牌"。</p>
            </td>
            <td>
              <p>在以下两种情况下会发生此错误︰</p>
              <ul>
                <li class="unordered">
您已指定开始 Azure AD 连接安装过程中指定的全局管理员帐户的密码不正确。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
您试图使用 Azure AD 连接安装过程的开始处指定的全局管理员帐户的联盟的用户。<br\><br\></li>
              </ul>
              <p>若要修复此错误，请确保您没有为全局管理员使用联合的帐户指定的 Azure AD 连接安装过程中，开头和指定的密码不正确。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>在 Azure AD 连接安装过程中配置写回错误。</p>
            </td>
            <td>
              <p>Azure AD 连接的计算机的事件日志包含错误 32002 引发的 PasswordResetService。</p>
              <p>

              </p>
              <p>该错误显示为:"错误连接到 ServiceBus，令牌提供程序不能提供安全令牌..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>此错误的根本原因是内部环境中运行的密码重置服务不能够连接到服务总线端点在云中。 通过阻止对特定端口或 web 地址的出站连接的防火墙规则通常通常导致此错误。</p>
              <p>

              </p>
              <p>请确保您的防火墙允许为下列的出站连接︰</p>
              <ul>
                <li class="unordered">
通过 TCP 443 (HTTPS) 的所有通信<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
对出站连接 <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>一旦您已更新了这些规则，重新启动 Azure AD 连接的计算机和密码写回应该重新开始工作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>密码写回终结点上的 prem 无法访问</p>
            </td>
            <td>
              <p>之后的一些时间，联合或密码哈希的工作同步的用户不能重置其密码。</p>
            </td>
            <td>
              <p>在某些罕见的情况下，密码写回服务可能无法重新启动时重新启动 Azure AD 连接。 在这些情况下，第一次，检查是否是在 prem.启用的密码写回 这可以使用 Azure AD 连接向导或 powershell （请参阅 Howto 上面一节）。如果该功能似乎已启用，请尝试启用或禁用功能再次通过用户界面或 PowerShell。 请参阅"步骤 2︰ 启用目录同步计算机上的密码写回&amp;配置防火墙规则"<a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">如何启用/禁用密码写回</a>有关如何执行此操作的详细信息。</p>
              <p>

              </p>
              <p>如果这不起作用，请尝试完全卸载并重新安装 Azure AD 连接。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>权限错误</p>
            </td>
            <td>
              <p>联合或密码哈希同步获得的尝试之后，重置其密码请参见错误提交时出现服务问题表示的密码的用户。</p>
              <p>

              </p>
              <p>除此之外，在密码重置操作，可能会看到有关管理代理的错误被拒绝访问您在本地事件日志中。</p>
            </td>
            <td>
              <p>如果您在事件日志中查看这些错误，请确认 （即已在向导中指定在配置的时间） AD 帐户密码写回所需的权限。</p>
              <p>

              </p>
              <p>请注意，一旦被授予此权限，可能需要 1 小时 trickle 下通过 sdprop 后台任务的 DC 上的权限。 </p>
              <p>密码重设工作，权限需要加盖在正在重置其密码的用户对象的安全描述符。 此权限显示在用户对象上，直到密码重置将继续由于访问被拒绝而失败。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>从 Azure AD 连接配置向导配置密码写回时的错误 </p>
            </td>
            <td>
              <p>"无法找到 MA"向导中启用/禁用密码写回时的错误</p>
            </td>
            <td>
              <p>Azure AD 连接的清单在以下情况中的发行版中没有一个已知的错误︰</p>
              <ol class="ordered">
                <li>
配置为租户 abc.com （已验证域） 使用凭据设置 Azure AD 连接。 这将导致 AAD 连接器名称"abc.com – AAD"正在创建。<br\><br\></li>
                <li>
然后您再更改 AAD 凭据设置 （使用旧同步用户界面） 连接器 （注意是但不同的域名相同的租户）。 <br\><br\></li>
                <li>
现在，您尝试启用/禁用密码写回。 该向导将构造使用凭据设置，为"abc.onmicrosoft.com – AAD"的连接器的名称并传递给密码写回 cmdlet。 这将失败，因为没有使用该名称创建的接头。<br\><br\></li>
              </ol>
              <p>已在我们最新版本中得到解决。 如果较早版本，一种解决办法是使用 powershell cmdlet 以启用/禁用该功能。 请参阅"步骤 2︰ 启用目录同步计算机上的密码写回&amp;配置防火墙规则"<a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">如何启用/禁用密码写回</a>有关如何执行此操作的详细信息。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>无法重置密码中特殊的组，如域管理员的用户 / 企业管理员等。</p>
            </td>
            <td>
              <p>联合或密码哈希同步获得的受保护组的一部分，并尝试之后重置其密码请参见错误提交时出现服务问题表示的密码的用户。</p>
            </td>
            <td>
              <p>在 Active Directory 中的特权的用户使用 AdminSDHolder 进行保护。 <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">Http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a>有关更多详细信息，请参阅。 </p>
              <p>

              </p>
              <p>这意味着在这些对象上的安全描述符定期检查匹配一个中指定的 AdminSDHolder 并被重置，如果它们是不同的。 因此对这类用户不 trickle 密码写回所需的附加权限。 这可能会导致不工作这样的用户的密码写回。因此，我们不支持管理密码的这些组中的用户因为它中断的 AD 安全模型。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>找不到与对象重置操作失败</p>
            </td>
            <td>
              <p>联合或密码哈希同步获得的尝试之后，重置其密码请参见错误提交时出现服务问题表示的密码的用户。</p>
              <p>

              </p>
              <p>除此之外，在密码重置操作，您可能会看到错误从 Azure AD 连接服务指示"找不到对象"错误事件日志中。</p>
            </td>
            <td>
              <p>此错误通常表明同步引擎是找不到链接的 MV AAD 连接器空间中的用户对象或 AD 的连接器空间对象。 </p>
              <p>

              </p>
              <p>若要解决此问题，请确保用户确实同步从上 prem 到通过 Azure AD 连接的当前实例的 AAD 和检查连接器空间和 MV 中对象的状态。 请确认 AD CS 对象是通过"Microsoft.InfromADUserAccountEnabled.xxx"规则的 MV 对象的连接器。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>重置操作将失败，有多个匹配找到 eror</p>
            </td>
            <td>
              <p>联合或密码哈希同步获得的尝试之后，重置其密码请参见错误提交时出现服务问题表示的密码的用户。</p>
              <p>

              </p>
              <p>除此之外，在密码重置操作，您可能会看到错误从 Azure AD 连接服务指示"找到多个 maches"错误事件日志中。</p>
            </td>
            <td>
              <p>这表明，同步引擎检测到 MV 对象是连接到多个 AD CS 对象，通过"Microsoft.InfromADUserAccountEnabled.xxx"。 这意味着用户在多个目录林都启用的帐户。 </p>
              <p>

              </p>
              <p>目前这种情况下不是支持密码写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>由于配置错误而失败的密码操作。</p>
            </td>
            <td>
              <p>由于配置错误而失败的密码操作。 应用程序事件日志中包含文本的 Azure AD 连接错误 6329: 0x8023061f （操作失败，因为此管理代理没有启用密码同步。）</p>
            </td>
            <td>
              <p>出现这种情况是如果 Azure AD 连接配置更改为添加&nbsp;新的 AD 林 （或以删除并重新添加现有的目录林中）<strong>后</strong>的密码写回功能已启用。 这种新添加的目录林中的用户的密码操作将会失败。 若要解决此问题，请禁用并完成目录林配置更改之后，重新启用密码写回功能。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>由用户更改写回正常，用户可通过重置密码，但写回密码或重置为管理员用户失败。</p>
            </td>
            <td>
              <p>尝试重置代表从 Azure 管理门户用户的密码，当您看到一条消息指出:"在您的内部环境中运行的密码重置服务不支持重置用户密码的管理员。 请升级到最新版本的 Azure AD 连接来解决这个问题。"</p>
            </td>
            <td>
              <p>同步引擎的版本不支持特定的密码写回操作使用时，将发生这种情况。 写回、 密码更改写回和从 Azure 管理门户管理员启动密码重置回写，Azure AD 连接 1.0.0419.0911 支持所有密码管理操作以后的版本中，包括密码重置。&nbsp; 目录同步版本高于 1.0.6862 支持密码重置回写。 若要解决此问题，我们强烈建议您安装最新版本的 Azure AD 连接或 Azure 活动目录连接 （有关详细信息，请参阅<a href="active-directory-aadconnect">目录集成工具</a>） 来解决此问题，并充分利用您的组织中的密码写回。</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>密码写回事件日志错误代码
最佳做法是在用密码写回的问题进行故障排除时是检查您 Azure AD 连接的计算机上的应用程序事件日志。 此事件日志将包含密码写回感兴趣的两个来源的事件。 PasswordResetService 源将描述操作和相关的密码写回操作的问题。 ADSync 源将描述操作和与 AD 环境中设置密码相关的问题。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>代码</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>名称 / 消息</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>来源</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>说明</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>扣︰ MMS(4924) 0x80230619 –"限制可以防止密码被指定到当前更改。</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>尝试设置密码不符合密码期限、 历史记录、 复杂性，或域的筛选要求的本地目录的密码写回服务时，将发生此事件。</p>
              <ul>
                <li class="unordered">
如果有密码最短使用期限，最近更改了该时间段的时间内的密码，您将无法再次更改密码，直到它到达您的域中指定的生存期。 出于测试目的，最小年龄应该设置为 0。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码历史要求启用，则您必须选择在上一次的 N 次，未使用过的密码其中 N 是密码历史记录设置。 如果您选择在最后一个 N 次使用过的密码，然后将这种情况下看到失败。 出于测试目的，历史应该设置为 0。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码复杂性要求，它们将用户尝试更改或重设密码时实施。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码筛选器启用，并且用户选择的密码未满足筛选条件，然后重置或更改操作将会失败。<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>同步引擎返回错误 hr = 80230402，邮件 = 尝试获取一个对象，该对象失败，因为与同一个锚点的重复的项</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>当多个域中启用了相同的用户 id 时，将发生此事件。  例如，如果您正在同步帐户/资源目录林，并具有相同的用户 id 存在并在每个已启用，则可能发生此错误。  </p>
              <p>如果您正在使用一个非唯一的定位特性 （如别名或 UPN） 和两个用户共享同一 anchor 属性，也可发生此错误。</p>
              <p>若要解决此问题，请确保您没有任何重复的用户在您的域中，每个用户使用一个独特的定位属性。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示内部服务检测到密码重置请求联合或密码哈希同步获得的来自云中的用户。 此事件是在每个密码的第一个事件重置写回操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，用户在密码重置操作过程中选择一个新密码，我们确定该密码满足公司密码要求，并且该密码已成功写入回本地广告环境。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示用户选择的密码，并且密码到内部部署环境中，成功到达，但当我们试图在本地的广告环境中设置密码时，出现错误。 发生这种情况的原因有多种︰</p>
              <ul>
                <li class="unordered">
用户的密码不符合时代、 历史记录、 复杂性，或筛选器的域的要求。 请尝试全新的密码，若要解决此问题。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
马萨诸塞州服务帐户没有相应的权限才能设置用户帐户的新密码。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
用户帐户是受保护组中，如域或企业管理员，不允许密码设置操作。<br\><br\></li>
              </ul>
              <p>请参阅<a href="#troubleshoot-password-writeback">疑难解答密码写回</a>，以了解更多关于哪些其他 situtions 可能导致此错误。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件指示我们在贵组织的密码写回 web 服务启动服务，并启用与 Azure AD 连接的密码写回会发生。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示服务过程成功，并且密码写回功能已准备好使用。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明内部服务检测到联盟的密码更改请求或密码哈希同步获得的来自云中的用户。 此事件是在每个密码更改写回操作中的第一个事件。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，用户在密码更改操作过程中选择一个新密码，我们确定该密码满足公司密码要求，并且该密码已成功写入回本地广告环境。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示用户选择的密码，并且密码到内部部署环境中，成功到达，但当我们试图在本地的广告环境中设置密码时，出现错误。 发生这种情况的原因有多种︰</p>
              <ul>
                <li class="unordered">
用户的密码不符合时代、 历史记录、 复杂性，或筛选器的域的要求。 请尝试全新的密码，若要解决此问题。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
马萨诸塞州服务帐户没有相应的权限才能设置用户帐户的新密码。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
用户帐户是受保护组中，如域或企业管理员，不允许密码设置操作。<br\><br\></li>
              </ul>
              <p>请参阅<a href="#troubleshoot-password-writeback">疑难解答密码写回</a>，以了解更多关于哪些其他情况下可能导致此错误。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>内部服务检测到密码重置请求联合或密码哈希同步获得的来自管理员代表用户的用户。 此事件是在每个管理员启动密码重置回写操作中的第一个事件。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>管理管理员启动密码重置操作期间选择新的密码，我们确定该密码满足公司密码要求，并且该密码已成功写入回本地广告环境。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>该管理员代表用户选择密码和密码成功到达到内部部署环境中，但当我们试图在本地的广告环境中设置密码时，出现错误。 发生这种情况的原因有多种︰</p>
              <ul>
                <li class="unordered">
用户的密码不符合时代、 历史记录、 复杂性，或筛选器的域的要求。 请尝试全新的密码，若要解决此问题。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
马萨诸塞州服务帐户没有相应的权限才能设置用户帐户的新密码。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
用户帐户是受保护组中，如域或企业管理员，不允许密码设置操作。<br\><br\></li>
              </ul>
              <p>请参阅<a href="#troubleshoot-password-writeback">疑难解答密码写回</a>，以了解更多关于哪些其他 situtions 可能导致此错误。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件会发生如果您禁用使用 Azure AD 连接的密码写回，表示我们在贵组织的密码写回 web 服务启动 offboarding。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明 offboarding 过程成功，并已成功禁用密码写回功能。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明 offboarding 过程没有成功。 这可能是由于配置过程中，指定的云或本地管理员帐户的权限错误，或者因为您试图禁用密码写回时使用联合的云全局管理员。 若要解决此问题，请检查您的管理权限，您不使用任何配置的密码写回功能时联合帐户。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示密码写回服务已成功启动，并已准备好接受来自云的密码管理请求。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示密码写回服务已停止，并从云中的任何密码管理请求不会成功。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示我们成功检索到 Azure AD 连接安装期间指定以启动 offboarding 或服务进程全局管理员授权标记。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示我们已成功创建了密码加密密钥用于加密密码从云被发送到您的内部环境。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示密码管理操作时出现未知的错误。 查看事件详细信息中的异常文本。 如果您有问题，请尝试禁用并重新启用密码写回。 如果这不起作用，包括事件日志和跟踪 id 指定的副本与支持工程师的内幕。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>服务错误 </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件指示没有错误连接到云密码重置服务，并且通常发生在内部服务无法连接到密码重置 web 服务。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示连接到租户的服务总线实例时出错。 这可能是因为您内部环境中阻止出站连接。 请检查您的防火墙，以确保您通过 TCP 443 和向<a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>，允许连接并再试一次。 如果仍有问题，请尝试禁用并重新启用密码写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示传递给我们的 web 服务 API 的输入无效。 再次尝试该操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示已解密的密码，云从到达时出错。 这可能是因为云服务和本地环境之间的解密密钥不匹配。 若要解决此问题，禁用并重新启用您的内部环境中的密码写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>期间服务，我们将特定于租户的信息保存在您的内部环境中的配置文件。 此事件指示保存此文件时出错或，那里启动该服务时在错误读取文件。 若要解决此问题，请尝试禁用并重新启用密码写回强制重写此配置文件。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>不建议使用-此事件 Azure AD 连接中不存在，只有最早期生成的目录同步支持写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>在服务，我们将发送数据从云到内部密码重置服务。 然后该数据写入到内存中文件之前发送到同步服务在磁盘上安全地存储这些信息中。 此事件指示编写或更新内存中的数据的问题。 若要解决此问题，请尝试禁用并重新启用密码写回强制重写此配置。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示我们从密码重置 web 服务收到无效的响应。 若要解决此问题，请尝试禁用并重新启用密码写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，我们无法获得授权令牌 Azure AD 连接安装过程中指定的全局管理员帐户。 此错误可能由错误的用户名或密码的全局管理员帐户指定或联合是因为指定的全局管理员帐户。 若要解决此问题，请重新运行配置使用正确的用户名和密码，并确保管理员托管 (仅云或密码同步) 帐户。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示在生成密码时出现错误的加密密钥或解密密码发送过来的云服务。 此错误可能表示您的环境的问题。 查看事件日志以了解详细信息并解决此问题的详细信息。 您还可以尝试禁用和重新启用密码写回服务来解决此问题。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，内部服务无法正确通信与密码重置 web 服务启动服务进程。 这可能是由于防火墙规则或您组织中获取身份验证令牌的问题。 若要解决此问题，请确保您对 TCP 443 和 TCP 9350 9354 或<a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>，未阻止的出站连接和不联合使用到板载 AAD admin 帐户。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>不建议使用-此事件 Azure AD 连接中不存在，只有最早期生成的目录同步支持写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，内部服务无法正确通信与密码重置 web 服务启动 offboarding 过程。 这可能是由于防火墙规则或获得授权标记为您的租户的问题。 若要解决此问题，确保您对 443 或<a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>，未阻止的出站连接不联合 AAD 管理员帐户使用到离职。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明，我们不得不重新尝试连接到租户的服务总线实例。 正常情况下，这不应该值得关注，但如果您看到此事件很多时候，考虑检查您的网络连接到服务总线，尤其是高延迟或低带宽的连接。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>为了监视密码写回服务的运行状况，我们将发送心跳信号数据与我们的密码重置 web 服务每个 5 分钟。 此事件指示时将此健康信息发送回给云 web 服务时发生错误。 此健康信息不包含 OII 或 PII 的数据，并且是纯粹的心跳和基本服务的统计信息，以便我们可以提供在云中的服务状态信息。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明广告由返回的未知的错误，请检查 Azure AD 连接服务器事件日志中的事件 ADSync 来源有关此错误的详细信息。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示尝试重置或更改密码的用户未在本地目录中找到。 用户已删除的内部部署时但在云中，不可能发生这种情况或是否有同步的问题。 检查同步日志，以及最后几个同步运行的详细信息的详细信息。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>在密码重置或更改请求源自云时，我们使用 Azure AD 连接的安装过程中指定的群锚点以确定如何将该请求链接返回到您的内部环境中的用户。 此事件表示我们与同一个云定位特性内部目录中找到两个用户。 检查同步日志，以及最后几个同步运行的详细信息的详细信息。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件指示管理代理服务帐户问题设置新的密码帐户上没有适当的权限。 确保 MA 目录林中的用户帐户具有在树林中的所有对象重置和更改密码权限。  有关如何执行到此问题，请参阅<a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">步骤 4︰ 设置适当的 Active Directory 权限</a>。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示我们尝试重置或更改在场所已禁用帐户的密码。 启用此帐户，然后再次尝试该操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>事件指示我们尝试重置或更改在的场所已被锁定的帐户的密码。 用户尝试更改或重置密码操作，多次在短时间内，可能会出现锁定。 解除锁定该帐户，然后再次尝试该操作。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表示用户指定当前密码不正确，当执行密码更改操作。 指定正确的当前密码，然后重试。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>尝试设置密码不符合密码期限、 历史记录、 复杂性，或域的筛选要求的本地目录的密码写回服务时，将发生此事件。</p>
              <ul>
                <li class="unordered">
如果有密码最短使用期限，最近更改了该时间段的时间内的密码，您将无法再次更改密码，直到它到达您的域中指定的生存期。 出于测试目的，最小年龄应该设置为 0。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码历史要求启用，则您必须选择在上一次的 N 次，未使用过的密码其中 N 是密码历史记录设置。 如果您选择在最后一个 N 次使用过的密码，然后将这种情况下看到失败。 出于测试目的，历史应该设置为 0。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码复杂性要求，它们将用户尝试更改或重设密码时实施。<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
如果有密码筛选器启用，并且用户选择的密码未满足筛选条件，然后重置或更改操作将会失败。<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>此事件表明存在密码重新添加到您的内部目录配置问题由于 Active Directory 问题写入。 检查来自 ADSync 服务上发生了什么错误详细信息的消息的 Azure AD 连接计算机的应用程序事件日志。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>不建议使用-此事件 Azure AD 连接中不存在，只有最早期生成的目录同步支持写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>不建议使用-此事件 Azure AD 连接中不存在，只有最早期生成的目录同步支持写回。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>不建议使用-此事件 Azure AD 连接中不存在，只有最早期生成的目录同步支持写回。</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>写回密码连接疑难解答

如果您遇到的用密码写回组件的 Azure AD 连接的服务中断，下面是一些快速步骤可以解决此问题︰

 - [重新启动 Azure AD 连接同步服务](#restart-the-azure-AD-Connect-sync-service)
 - [禁用并重新启用密码写回功能](#disable-and-re-enable-the-password-writeback-feature)
 - [安装最新版本，Azure AD 连接](#install-the-latest-azure-ad-connect-release)
 - [解决密码写回](#troubleshoot-password-writeback)

一般情况下，我们建议您就可以恢复您的服务以最快的方式上面的顺序执行这些步骤。

### <a name="restart-the-azure-ad-connect-sync-service"></a>重新启动 Azure AD 连接同步服务
重新启动 Azure AD 连接同步服务可以帮助解决连接问题或与该服务的其他暂时的问题。

 1. 作为管理员，单击运行**Azure AD 连接**的服务器上的**开始**。
 2. 在搜索框中键入**"服务"** ，然后按**Enter**。
 3. 查找**Microsoft Azure AD 连接**项。
 4. 用鼠标右键单击服务项，单击**重新启动**，等待该操作完成。

    ![][002]

这些步骤将重新建立与云服务的连接，并解决您可能遇到的任何中断。  如果重新启动同步服务不能解决您的问题，我们建议您尝试禁用并重新启用密码写回功能作为下一步。

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>禁用并重新启用密码写回功能
禁用和重新启用密码写回功能可以帮助解决连接问题。

 1. 作为管理员，打开**Azure AD 连接配置向导**。
 2. 在**对 Azure AD 连接**对话框中，输入您的**Azure AD 全局管理员凭据**
 3. 在**连接到 AD DS**对话框中，输入您的**AD 域服务管理员凭据**。
 4. **唯一地标识您的用户**在对话框上，单击**下一步**按钮。
 5. 在**可选功能**对话框中，取消选中**密码回写**复选框。

    ![][003]

 6. 单击**下一步**通过剩余的对话框页面不需要改变直到到达**即可开始配置**页面。
 7. 确保配置页显示**密码回写选项为禁用状态**，然后单击绿色的**配置**按钮以提交您的更改。
 8. 在**已完成**对话框中，取消选择**立即同步**选项，然后单击**完成**以关闭向导。
 9. 重新打开**Azure AD 连接配置向导**。
 10.    **重复步骤 2-8**，除了确保您**选中密码回写选项****可选功能**屏幕重新启用该服务。

    ![][004]

这些步骤将重新建立与我们的云服务的连接，并解决您可能遇到的任何中断。

如果禁用和重新启用密码写回功能不能解决您的问题，我们建议您尝试重新安装 Azure AD 连接，为下一步。

### <a name="install-the-latest-azure-ad-connect-release"></a>安装最新版本，Azure AD 连接
重新安装 Azure AD 连接包将解决任何配置问题，这可能会影响您能够连接到云服务或管理本地 AD 环境中的密码。
我们建议，只有在尝试上面所述的前两个步骤之后执行此步骤。

 1. 下载最新版本的 Azure AD 连接[此处](active-directory-aadconnect.md#install-azure-ad-connect)。
 2. 由于您已经安装了 Azure AD 连接，只需要执行原位升级以更新到最新版本的 Azure AD 连接安装。
 3. 执行下载的程序包，请按照屏幕说明进行操作以更新您 Azure AD 连接的计算机。  没有更多的手动步骤是必需的除非您自定义的框同步规则，在这种情况下，您应该**备份这些文件，然后继续进行升级，然后手动重新部署其在完成后**。

这些步骤将重新建立与我们的云服务的连接，并解决您可能遇到的任何中断。

如果安装最新版本的 Azure AD 连接的服务器不能解决您的问题，我们建议在安装最新的同步 QFE 之后，作为最后一步尝试禁用并重新启用密码写回。

如果它不能解决您的问题，我们建议您看一下[解决密码写回](#troubleshoot-password-writeback)和[Azure AD 密码管理常见问题解答](active-directory-passwords-faq.md)查看如果您的问题可能会那里讨论。


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>链接到密码重置文档
下面是 Azure AD 密码重置文档页的所有链接︰

* **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。
* [**它的工作原理**](active-directory-passwords-how-it-works.md)的了解有关的六个不同部分的服务和每个站点都
* [**入门**](active-directory-passwords-getting-started.md)-了解如何允许用户重置并更改其云或内部密码
* [**自定义**](active-directory-passwords-customize.md)-了解如何自定义的外观和感觉和行为与您的组织需要的服务
* [**最佳做法**](active-directory-passwords-best-practices.md)-了解如何快速部署和有效地管理您的组织中的密码
* [**深入**](active-directory-passwords-get-insights.md)的了解我们集成的报告功能
* [**常见问题解答**](active-directory-passwords-faq.md)-获取常见问题的答案
* [**了解更多**](active-directory-passwords-learn-more.md)-转到服务的工作方式的技术细节的深



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
