<properties
   pageTitle="Azure 数据目录支持的数据源 |Microsoft Azure"
   description="当前支持的数据源的说明。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure 数据目录支持的数据源

Azure 数据目录的用户可以发布元数据中使用一个公共的 API，只需单击一-一次注册工具，或通过直接到数据目录中手动输入信息的 web 门户。 以下栅格总结今天，目录和发布功能为每个支持的所有源。  此外列出了每个源可以启动从"打开在"门户网站经验的外部数据工具。 在文章的第二个网格都有每个数据源连接属性的详细技术规范。


## <a name="list-of-supported-data-sources"></a>支持的数据源的列表

<table>

    <tr>
       <td><b>数据源对象</b></td>
       <td><b>API</b></td>
       <td><b>手动输入</b></td>
       <td><b>注册工具</b></td>
       <td><b>打开工具</b></td>
       <td><b>备注</b></td>
    </tr>

    <tr>
      <td>Azure 数据湖存储区目录</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure 数据湖存储文件</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure 存储 Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure 存储目录</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure 存储表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS 目录</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS 文件</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>配置单元表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>配置单元视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL 表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL 视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle 数据库表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle 数据库视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>其他 （通用资产）</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL 数据仓库表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中，PowerBI，SQL Server 数据工具</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL 数据仓库视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中，PowerBI，SQL Server 数据工具</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services 维度</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services 度量值</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services 表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中 PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server 申报服务报告</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>浏览器</font></td>
      <td><font size=2>本机模式服务器。 不支持 SharePoint 模式。</font></td>
    </tr>

    <tr>
      <td>SQL Server 表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中，PowerBI，SQL Server 数据工具</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server 视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel 中，PowerBI，SQL Server 数据工具</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata 表</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata 视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana 视图</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>计算视图和分析视图不支持属性视图。</font></td>
    </tr>

    <tr>
      <td>Db2 表</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Db2 视图</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>文件系统文件</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ftp 目录</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ftp 文件</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http 的报告</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http 的终点</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http 文件</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Odata 实体集</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Odata 函数</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql 表</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql 视图</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana 视图</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> 销售对象</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Sharepoint 列表 </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

如果您需要其他来源的支持，提交使用[Azure 数据目录论坛](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)的功能请求。


<br>
<br>
## <a name="data-source-reference-specification"></a>数据源引用规范
> [AZURE.NOTE] "DSL 结构"列在下列表格仅列表连接属性的"地址"属性包所使用的 Azure 数据目录 （例如"地址"属性包可以包含其他的 Azure 数据目录仍然存在，但不使用的数据源的连接属性。）
<table>
    <tr>
       <td><b>源类型</b></td>
       <td><b>资产类型</b></td>
       <td><b>对象类型</b></td>
       <td><b>DSL 结构<b></td>
    </tr>
    <tr>
      <td>Azure 数据湖存储区</td>
      <td>容器</td>
      <td>数据湖</td>
      <td>
        <font size=2>协议︰ webhdfs <br>身份验证: {基本，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Azure 数据湖存储区</td>
      <td>表</td>
      <td>目录文件</td>
      <td>
        <font size=2>协议︰ webhdfs <br>身份验证: {基本，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Azure 存储</td>
      <td>容器</td>
      <td>容器</td>
      <td>
        <font size=2>协议︰ azure blob <br>身份验证: {azure 快捷键} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;帐户 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;容器</font>
      </td>
    </tr>
    <tr>
      <td>Azure 存储</td>
      <td>表</td>
      <td>Blob 目录</td>
      <td>
        <font size=2>协议︰ azure blob <br>身份验证: {azure 快捷键} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;帐户 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;容器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名称</font>
      </td>
    </tr>
    <tr>
      <td>Azure 存储</td>
      <td>容器</td>
      <td>容器</td>
      <td>
        <font size=2>协议︰ azure 表 <br>身份验证: {azure 快捷键} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;帐户</font>
      </td>
    </tr>
    <tr>
      <td>Azure 存储</td>
      <td>表</td>
      <td>表</td>
      <td>
        <font size=2>协议︰ azure 表 <br>身份验证: {azure 快捷键} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;帐户 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名称</font>
      </td>
    </tr>
    <tr>
      <td>宇宙</td>
      <td>容器</td>
      <td>虚拟群集</td>
      <td>
        <font size=2>协议︰ 宇宙 <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>宇宙</td>
      <td>表</td>
      <td>流，流集视图</td>
      <td>
        <font size=2>协议︰ 宇宙 <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>容器</td>
      <td>网站</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>报告</td>
      <td>报告，仪表板</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ db2 <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ db2 <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构</font>
      </td>
    </tr>
    <tr>
      <td>文件系统</td>
      <td>表</td>
      <td>文件</td>
      <td>
        <font size=2>协议︰ 文件 <br>身份验证: {无，基本的 windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;路径</font>
      </td>
    </tr>
    <tr>
      <td>Ftp</td>
      <td>表</td>
      <td>目录文件</td>
      <td>
        <font size=2>协议︰ ftp <br>身份验证: {无，基本的 windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop 分布式文件系统</td>
      <td>容器</td>
      <td>群集</td>
      <td>
        <font size=2>协议︰ webhdfs <br>身份验证: {基本，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop 分布式文件系统</td>
      <td>表</td>
      <td>目录文件</td>
      <td>
        <font size=2>协议︰ webhdfs <br>身份验证: {基本，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>配置单元</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ 配置单元 <br>身份验证: {hdinsight，基本，用户名，无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>配置单元</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ 配置单元 <br>身份验证: {hdinsight，基本，用户名，无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>容器</td>
      <td>网站</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>报告</td>
      <td>报告，仪表板</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>表</td>
      <td>终点文件</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ mysql <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ mysql <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>Odata</td>
      <td>容器</td>
      <td>实体容器</td>
      <td>
        <font size=2>协议︰ odata <br>身份验证: {无，基本的 windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>Odata</td>
      <td>表</td>
      <td>实体集函数</td>
      <td>
        <font size=2>协议︰ odata <br>身份验证: {无，基本的 windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;资源</font>
      </td>
    </tr>
    <tr>
      <td>Oracle 数据库</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ oracle <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>Oracle 数据库</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ oracle <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ postgresql <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ postgresql <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>双电源</td>
      <td>容器</td>
      <td>网站</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>双电源</td>
      <td>报告</td>
      <td>报告，仪表板</td>
      <td>
        <font size=2>协议︰ http <br>身份验证: {无，基本的 windows，oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>电源查询</td>
      <td>表</td>
      <td>数据混合应用程序</td>
      <td>
        <font size=2>协议︰ 电源查询 <br>身份验证: {oauth} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>销售队伍</td>
      <td>表</td>
      <td>对象</td>
      <td>
        <font size=2>协议︰ 队伍 com <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;类 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;项目名称</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>容器</td>
      <td>服务器</td>
      <td>
        <font size=2>协议︰ sap hana sql <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>表</td>
      <td>视图</td>
      <td>
        <font size=2>协议︰ sap hana sql <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>表</td>
      <td>列表</td>
      <td>
        <font size=2>协议︰ sharepoint 列表 <br>身份验证: {基本，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url</font>
      </td>
    </tr>
    <tr>
      <td>SQL 数据仓库</td>
      <td>命令</td>
      <td>存储的过程</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL 数据仓库</td>
      <td>TableValuedFunction</td>
      <td>表值函数</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL 数据仓库</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>SQL 数据仓库</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>命令</td>
      <td>存储的过程</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>表值函数</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ tds <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;架构 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 多维</td>
      <td>容器</td>
      <td>模型</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 多维</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 多维</td>
      <td>度量值</td>
      <td>度量值</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型︰ {0} 度量值}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 多维</td>
      <td>表</td>
      <td>维度</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型︰ {0} 维度}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 表格</td>
      <td>容器</td>
      <td>模型</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 表格</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 表格</td>
      <td>度量值</td>
      <td>度量值</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型︰ {0} 度量值}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services 表格</td>
      <td>表</td>
      <td>表</td>
      <td>
        <font size=2>协议︰ 分析服务 <br>身份验证: {windows、 基本、 匿名、 无} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象类型: {表}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server 报表服务</td>
      <td>容器</td>
      <td>服务器</td>
      <td>
        <font size=2>协议︰ 报告服务 <br>身份验证: {windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版本: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server 报表服务</td>
      <td>报告</td>
      <td>报告</td>
      <td>
        <font size=2>协议︰ 报告服务 <br>身份验证: {windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;路径 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版本: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>容器</td>
      <td>数据库</td>
      <td>
        <font size=2>协议︰ teradata <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>表</td>
      <td>表、 视图</td>
      <td>
        <font size=2>协议︰ teradata <br>身份验证: {协议，windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据库 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server 主数据服务</td>
      <td>容器</td>
      <td>模型</td>
      <td>
        <font size="2">协议︰ mssql mds <br>身份验证: {windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版本</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server 主数据服务</td>
      <td>表</td>
      <td>实体</td>
      <td>
        <font size="2">协议︰ mssql mds <br>身份验证: {windows} <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模型 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版本 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;实体</font>
      </td>
    </tr>
    <tr>
      <td>其他 （而不是以上）</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>协议︰ 一般资产 <br>地址︰ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </td>
    </tr>
</table>
