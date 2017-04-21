<properties 
    pageTitle="研究的 Java 跟踪日志将在应用程序的见解" 
    description="在应用程序的见解的搜索 Log4J 或 Logback 跟踪" 
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
    ms.date="07/12/2016" 
    ms.author="awills"/>

# <a name="explore-java-trace-logs-in-application-insights"></a>研究的 Java 跟踪日志将在应用程序的见解

如果您使用的 Logback 或 Log4J （1.2 版或 2.0 版） 进行跟踪，您可以跟踪日志自动发送到应用程序您可以在这里浏览和搜索他们的见解。

安装[的 Java 应用程序的见解 SDK][java]，如果您已经没有这样的。


## <a name="add-logging-libraries-to-your-project"></a>向项目中添加日志记录库

*选择适当的方式为您的项目。*

#### <a name="if-youre-using-maven"></a>如果您正在使用 Maven...

如果已将项目设置为生成使用 Maven，合并到 pom.xml 文件中一个以下代码段的代码。

然后刷新项目依赖项，以获取下载的二进制文件。

*Logback*

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>

*Log4J v2.0*

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>

*Log4J v1.2*

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>

#### <a name="if-youre-using-gradle"></a>如果您使用的 Gradle...

如果您的项目已设置要用于生成 Gradle，添加到以下命令行之一`dependencies`build.gradle 文件中的组︰

然后刷新项目依赖项，以获取下载的二进制文件。

**Logback**

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'

**Log4J v2.0**

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'

**Log4J v1.2**

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'

#### <a name="otherwise-"></a>否则为...

下载并解压缩适当 appender，然后向项目中添加相应的库︰


记录器 | 下载 | 库
----|----|----
Logback|[与 Logback appender SDK](https://aka.ms/xt62a4)|--logback applicationinsights 日志记录
Log4J v2.0|[与 Log4J v2 appender SDK](https://aka.ms/qypznq)|--log4j2 applicationinsights 日志记录 
Log4j v1.2|[与 Log4J v1.2 appender SDK](https://aka.ms/ky9cbo)|--log4j1_2 applicationinsights 日志记录 



## <a name="add-the-appender-to-your-logging-framework"></a>为日志记录框架添加 appender

要开始获取跟踪，合并到 Log4J 或 Logback 配置文件中的代码的相关代码段︰ 

*Logback*

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>


*Log4J v2.0*


    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>


*Log4J v1.2*

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>

应用程序的见解 appenders 可以引用通过任何已配置的记录器，并不一定是根记录器 （如上面的代码示例中所示）。

## <a name="explore-your-traces-in-the-application-insights-portal"></a>浏览应用程序的见解门户中您跟踪

既然您已配置您的项目以将跟踪发送到应用程序的见解，可以查看和搜索这些跟踪信息在[搜索]中的应用程序理解门户网站[diagnostic]刀片式服务器。

![在应用程序的见解门户中，打开搜索](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>下一步行动

[诊断搜索][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md

 
