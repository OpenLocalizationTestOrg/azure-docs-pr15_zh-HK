<properties 
    pageTitle="监视的依赖关系、 异常和 Java web 应用程序中执行时间" 
    description="扩展网站 Java 应用程序的见解的监视" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>监视的依赖关系、 异常和 Java web 应用程序中执行时间

*在预览是应用程序的见解。*

如果您必须[检测 Java web 应用程序与应用程序的见解][java]，您可以使用 Java 代理获得更深入的洞察力，无需进行任何代码更改︰


* **依赖项︰**有关调用您的应用程序对其他组件，包括数据︰
 * 通过 HttpClient、 OkHttp 和 RestTemplate （弹簧） 进行**REST 调用**。
 * Jedis 客户端通过**redis**调用。 如果通话时间超过 10 条，代理还提取调用参数。
 * **[JDBC 调用](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**的 MySQL，SQL Server，PostgreSQL、 SQLite，Oracle 数据库或 Apache Derby 数据库。 支持调用"executeBatch"。 对于 MySQL 和 PostgreSQL，通话时间超过 10 条，如果代理报告查询计划。 
* **捕获异常︰**有关异常处理的代码的数据。
* **方法的执行时间︰**要执行的特定方法的时间有关的数据。

若要使用 Java 代理，则它在服务器上安装。 Web 应用程序必须检测[应用程序理解 Java SDK][java]。

## <a name="install-the-application-insights-agent-for-java"></a>安装 Java 的应用程序理解代理

1. 在计算机上运行您的 Java 服务器，[下载代理](https://aka.ms/aijavasdk)。
2. 编辑应用程序服务器的启动脚本，然后添加以下 JVM:

    `javaagent:`*代理程序 JAR 文件的完整路径*

    例如，在 Tomcat 的 Linux 计算机上︰

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. 重新启动应用程序服务器。

## <a name="configure-the-agent"></a>配置代理

创建名为`AI-Agent.xml`，并将其作为代理 JAR 文件相同的文件夹中。

设置的 xml 文件的内容。 编辑下面的示例包含或省略功能所需。 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

您必须启用报告异常和各个方法的方法计时。

默认情况下，`reportExecutionTime`为 true 和`reportCaughtExceptions`为假。

## <a name="view-the-data"></a>查看数据

在见解应用程序资源中，聚合远程相关性和方法执行时间出现[在性能拼下][metrics]。 

打开[搜索]以搜索各个实例的相关性、 异常和方法的报告，[diagnostic]。 

[Diagnosing 依赖项问题的了解](app-insights-dependencies.md#diagnosis)。



## <a name="questions-problems"></a>问题？ 有问题吗？

* 没有数据？ [设置防火墙例外](app-insights-ip-addresses.md)
* [Java 的疑难解答](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
