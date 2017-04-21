<properties 
    pageTitle="身份验证检测过程中出现错误" 
    description="活动目录连接向导检测到不兼容的身份验证类型" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>身份验证检测过程中出现错误

同时检测以前的身份验证代码，该向导检测到不兼容的身份验证类型。   

##<a name="what-is-being-checked"></a>正在检查什么？

**注意︰**为了正确检测项目中的以前的身份验证代码，必须生成项目。  如果您遇到此错误并且没有以前的身份验证代码在您的项目中，重新生成并再试一次。

###<a name="project-types"></a>项目类型

向导正在检查哪种类型的项目正在开发以便它可以将项目插入到适当的身份验证逻辑。  如果没有从派生的任何控制器`ApiController`项目，该项目将被视为 WebAPI 项目。  如果仅从派生的控制器`MVC.Controller`项目，该项目将被视为 MVC 项目。  向导不支持任何其他操作。  WebForms 项目当前不支持。

###<a name="compatible-authentication-code"></a>兼容的身份验证代码

该向导还检查先前使用该向导配置或兼容向导使用身份验证设置。  所有设置是否存在，认为是可重入的情况下，向导将打开和显示设置。  如果仅存在时的一些设置，它被视为错误的情况。

在 MVC 项目中，向导会检查任何下列设置，从上次使用该向导的结果︰

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

此外，该向导检查以下任一设置在 Web API 项目中，这导致从以前使用向导︰

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>不兼容的身份验证代码

最后，向导会尝试检测已配置与以前版本的 Visual Studio 版本的验证代码。 如果您收到此错误，则表示您的项目包含不兼容的身份验证类型。 向导检测到以下类型的身份验证从 Visual Studio 的早期版本︰

* Windows 身份验证 
* 每个用户帐户 
* 组织的帐户 
 

在 MVC 项目中检测 Windows 身份验证，向导将查找`authentication` **web.config**文件中的元素。

<pre>
    &lt;配置&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;身份验证模式 ="Windows"/&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

为了检测 Windows 身份验证在 Web API 项目，向导将查找`IISExpressWindowsAuthentication`项目的**.csproj**文件中的元素︰

<pre>
    &lt;项目&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;启用&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/项目        &gt;
</pre>

来检测单个的用户帐户身份验证，向导将查找包元素从**Packages.config**文件。

<pre>
    &lt;包&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework 打包"版本 ="2.1.0"targetFramework ="net45"/&gt;</span>
    &lt;/包&gt;
</pre>

若要检测组织帐户身份验证的旧形式，向导查找下一个元素从**web.config**:

<pre>
    &lt;配置&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;添加键 = 值"ida︰ 领域"="***"/&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

若要更改身份验证类型，请删除不兼容的身份验证类型，再次运行该向导。

有关详细信息，请参阅[Azure AD 身份验证方案](active-directory-authentication-scenarios.md)。