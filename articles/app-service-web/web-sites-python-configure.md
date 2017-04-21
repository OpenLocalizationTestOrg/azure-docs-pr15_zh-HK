<properties 
    pageTitle="使用 Azure 应用程序服务 Web 应用程序配置 Python" 
    description="本教程介绍了创作和 Azure 应用程序服务 Web 应用程序上配置基本 Web 服务器网关接口 (WSGI) 符合 Python 应用程序的选项。" 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>使用 Azure 应用程序服务 Web 应用程序配置 Python

本教程介绍了创作和[Azure 应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)上配置的基本 Web 服务器网关接口 (WSGI) 符合 Python 应用程序的选项。

它描述了 Git 部署，如虚拟环境和使用 requirements.txt 软件包安装的其他功能。


## <a name="bottle-django-or-flask"></a>瓶子，Django 或 Flask 吗？

Azure 市场包含模板的瓶子，Django 和 Flask 框架。 如果您正在开发的第一个 web 应用程序在 Azure 应用程序服务，或者您还不熟悉 Git，我们建议您按照其中一个这些教程，其中包括用于构建使用 Git 部署 Windows 或者 Mac 从库中运行的应用程序的分步指导︰

- [创建 web 应用程序使用的瓶子](web-sites-python-create-deploy-bottle-app.md)
- [Django 使用创建 web 应用程序](web-sites-python-create-deploy-django-app.md)
- [用 Flask 创建 web 应用程序](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>在 Azure 门户网站上的 web 应用程序创建

本教程假定现有 Azure 订阅并对 Azure 门户的访问。

如果您没有对现有的 web 应用程序，您可以创建一个从[Azure 门户](https://portal.azure.com)。  单击新建按钮的左上角，然后单击**Web + 移动** > **Web 应用程序**。

## <a name="git-publishing"></a>Git 发布

通过在[本地 Git 部署到 Azure 应用程序服务](app-service-deploy-local-git.md)的说明配置新创建的 web 应用程序的 Git 发布。 本教程使用 Git 来创建、 管理和将我们 Python 的 web 应用程序发布到 Azure 应用程序服务。

一旦设置 Git 发布，Git 存储库并将其与您的 web 应用程序关联。 存储库的 URL 将显示，从今以后可以用于将数据从本地开发环境推到云。 发布应用程序通过 Git，请确保同时安装 git 中获取客户端并使用以将您的 web 应用程序内容推送到 Azure 应用程序服务中提供的说明。


## <a name="application-overview"></a>应用程序概述

在下一节中，将创建以下文件。 应将它们放在根目录下的 Git 存储库。

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI 处理程序

WSGI 是 Python 标准所描述的[PEP 3333](http://www.python.org/dev/peps/pep-3333/)定义 web 服务器和 Python 之间的接口。 它提供了一个标准化的接口编写各种 web 应用程序和使用 Python 的框架。 常用的 Python web 框架目前使用 WSGI。 Azure 应用程序服务 Web 应用程序提供的任何这类框架; 支持此外，高级的用户可以甚至创作他们自己只要自定义处理程序遵循 WSGI 规范准则。

下面是一个示例的`app.py`，它定义一个自定义处理程序︰

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

您可以运行此应用程序与本地`python app.py`，然后浏览到`http://localhost:5555`在您的 web 浏览器中。


## <a name="virtual-environment"></a>虚拟环境

虽然上面的示例应用程序不需要任何外部的包，则很可能您的应用程序将需要一些。

若要帮助管理外部包的依赖关系，Azure Git 部署支持虚拟环境的创建。

Azure 存储库的根目录中，检测到 requirements.txt 时, 自动创建一个虚拟环境，名为`env`。 这只发生在第一次部署，或选定的 Python 之后任何部署期间运行库已更改。

您可能需要创建虚拟环境进行开发、 本地但不将其包括在您的 Git 存储库中。


## <a name="package-management"></a>包管理

在 requirements.txt 中列出的包将在虚拟环境中使用 pip 自动安装。 在每个部署中，将发生这种情况，但如果软件包已安装，pip 会跳过安装。

示例`requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python 版本

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

示例`runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

您将需要创建一个 web.config 文件，以指定服务器应如何处理请求。

请注意，如果您有存储库，其中 x.y 匹配选定的 Python 运行时，然后 Azure 将自动复制适当的文件作为 web.config 中的 web.x.y.config 文件。

下面的 web.config 示例中依赖于虚拟环境代理脚本，在下一节中描述。  在该示例中使用的 WSGI 处理工作`app.py`以上。

示例`web.config`的 Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


示例`web.config`的 Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


静态文件将处理由 web 服务器直接，而无需经过 Python 代码，以提高性能。

在上面的示例中，磁盘上的静态文件的位置应匹配的 URL 中的位置。 这意味着对于请求`http://pythonapp.azurewebsites.net/static/site.css`可在磁盘上的文件`\static\site.css`。

`WSGI_ALT_VIRTUALENV_HANDLER`是您在其中指定 WSGI 处理程序。 在上面的示例中，它具有`app.wsgi_app`因为处理程序是名为的函数`wsgi_app`在`app.py`的根文件夹中。

`PYTHONPATH`可以进行自定义，但如果通过在 requirements.txt 中指定它们，在虚拟环境中安装所有依赖项，则无须对其进行更改。


## <a name="virtual-environment-proxy"></a>虚拟环境代理

使用以下脚本来检索 WSGI 处理、 激活的虚拟环境和日志错误。 它旨在作为一般并不修改的情况下使用。

内容的`ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>自定义 Git 部署

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>故障排除的包的安装

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>故障诊断-虚拟环境

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅[Python 开发中心](/develop/python/)。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)





 
