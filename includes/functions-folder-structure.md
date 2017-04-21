
所有给定的函数的应用程序中的函数的代码存在于包含主机配置文件的根文件夹和一个或多个的子文件夹，其中每个包含一个单独的函数，如下面的示例中所示的代码

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json*文件包含某些特定于运行时的配置，位于函数的应用程序的根文件夹中。 有关可用设置的详细说明，请参阅[host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json)在 WebJobs.Script 资料库 wiki。

每个函数都有一个文件夹，其中包含一个或多个代码文件、 function.json 配置和其他依赖项。