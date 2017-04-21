<properties
    pageTitle="Azure 的活动目录 B2C︰ 用户界面 (UI) 自定义 |Microsoft Azure"
    description="在用户界面 (UI) 的自定义功能在 Azure 活动目录 B2C 上一个主题"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure 的活动目录 B2C︰ 自定义 Azure AD B2C 用户界面 (UI)

用户体验是面向消费者的应用程序中极其重要的。 它是很好的应用程序之间的一个很好的活动，和真正参与的活动只是使用者之间的差异。 Azure 的活动目录 (AD Azure) B2C 允许您自定义使用者注册，登录 （*请参阅下面的注释*），编辑的配置文件和密码重置与像素完美控件的网页。

> [AZURE.NOTE]
目前，本地帐户登录页面，其 accompaning 密码重置网页，可以自定义验证的电子邮件，只能通过使用该[公司品牌特征](../active-directory/active-directory-add-company-branding.md)并不是由本文中介绍的机制。

在本文中，您将了解︰

- 网页用户界面 (UI) 的自定义功能。
- 一种工具，可帮助您测试使用我们的示例的内容的页面 UI 自定义功能。
- 在页的每个类型的核心用户界面元素。
- 在行使这项功能时的最佳做法。

## <a name="the-page-ui-customization-feature"></a>页的用户界面自定义功能

页的用户界面自定义功能，您可以自定义外观和感觉的使用者注册，登录，密码重置和配置文件编辑页 （通过配置[策略](active-directory-b2c-reference-policies.md)）。 当您的应用程序和页面之间的导航由 Azure AD B2C，使用者将有无缝体验。

与其他服务在用户界面选项有限或通过 Api 仅有不同 Azure AD B2C 使用页自定义 UI 的现代 （和较简单的） 方法。

以下是它的工作原理︰ Azure AD B2C 在使用者的浏览器中运行的代码，并使用现代方法调用[跨原始资源共享 (CORS)](http://www.w3.org/TR/cors/)从您在策略中指定的 URL 加载内容。 您可以指定不同于其他页的 Url。 该代码合并从 Azure AD B2C 的 UI 元素的内容是加载自您的 URL，并向消费者显示网页。 所有您需要做的是︰

1. 创建格式良好的 HTML5 内容与`<div id="api"></div>`（必须是一个空元素） 的元素位于某处`<body>`。 此元素标记，Azure AD B2C 内容插入的位置。
2. （与允许 CORS) 主持的 HTTPS 终结点上的内容。
3. 在插入 Azure AD B2C 的用户界面元素的样式。

## <a name="test-out-the-ui-customization-feature"></a>用户界面自定义项功能测试

如果您想要试用的用户界面自定义功能使用我们的示例 HTML 和 CSS 的内容，我们已经给您提供[一个简单的帮助器工具](active-directory-b2c-reference-ui-customization-helper-tool.md)，上载和 Azure Blob 存储节点上配置示例内容。

> [AZURE.NOTE]
可以承载任何地方您的 UI 内容︰ 在 web 服务器上，Cdn，AWS S3，文件共享系统，等等。只要公开可用的 HTTPS 终结点承载内容时 （与允许 CORS)，现在就可以进行练习了。 我们使用 Azure Blob 存储仅用于说明目的。

### <a name="the-most-basic-html-content-for-testing"></a>最基本的 HTML 内容进行测试

下面是最基本的 HTML 内容，可以使用它来测试这一功能。 使用同一个帮助工具较早提供要上载并配置 Azure Blob 存储此内容。 然后，您可以验证基本、 非风格的按钮和每一页上的窗体字段显示且功能正常。

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>核心用户界面元素中每种类型的页面

在下面的章节中，您会发现属于 HTML5 片段 Azure AD B2C 将合并到`<div id="api"></div>`元素位于您的内容。 **不要在 HTML 5 内容中插入这些片段。** Azure AD B2C 服务在运行时插入它们。 使用这些示例来设计您自己的样式表。

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure B2C 广告内容插入到"标识提供程序选择页"

此页包含用户可以选择从注册或登录过程的标识提供程序的列表。 这些都是如 Facebook 和 Google + 社交标识提供程序，或是本地帐户 （基于电子邮件地址或用户名称）。

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure B2C 广告内容插入到"本地帐户注册页"

此页包含用户具有本地帐户的基础的电子邮件地址或用户名注册时填写注册表单。 窗体可以包含不同的输入的控件，如文本输入的框、 密码输入框、 单选按钮、 单选下拉列表框中和多项选择复选框。

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure B2C 广告内容页面中插入""社会帐户注册"

本页包含使用者必须使用如 Facebook 或 Google + 社交身份提供程序从现有的帐户注册时填写注册表单。 此页是类似于本地帐户注册页 （在上一节中所示） 密码输入字段除外。

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure B2C 广告内容页面中插入"统一注册或登录"

此页处理同时注册和登录的使用者，可使用如 Facebook、 Google +，或本地帐户的社会身份提供程序的用户。

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure B2C 广告内容插入到"多因素身份验证"页

在此页上，用户可以在注册或登录过程验证 （使用文本或语音） 它们的电话号码。

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure B2C 广告内容页面中插入""错误"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>在构建您自己的内容时要记住的事情

如果您打算使用的页 UI 自定义功能，审查了下列最佳做法︰

- 不复制 Azure AD B2C 的默认内容，并尝试对其进行修改。 最好能够生成您从零开始的 HTML5 内容和默认内容用作参考。
- 您提供的样式表将不得不重写默认样式表，我们将添加到这些网页中所有页中 （除错误页中） 提供的登录、 注册和配置文件编辑策略，<head>片段。 由注册或登录和密码重置策略和所有策略上的错误页的所有页面，您都需要提供所有的样式自己。
- 出于安全原因，我们不允许您在您的内容中包含任何 JavaScript。 您所需要的大部分应该提供开箱即用。 否则，使用[用户语音](http://feedback.azure.com/forums/169401-azure-active-directory)申请新的功能。
- 受支持的浏览器版本︰
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 （有限的）
    - Internet Explorer 8 （有限的）
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
