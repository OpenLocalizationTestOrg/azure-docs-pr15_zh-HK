<properties 
    pageTitle="使用地理空间数据在 Azure DocumentDB |Microsoft Azure" 
    description="了解如何创建、 索引和查询使用 Azure DocumentDB 的空间对象。" 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>使用地理空间数据在 Azure DocumentDB

本文是对[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)中的地理空间功能的介绍。 之后阅读本文，您将能够回答以下问题︰

- 如何在 Azure DocumentDB 存储空间数据？
- 如何查询在 SQL 和 LINQ 的 Azure DocumentDB 中的地理空间数据？
- 如何启用或禁用在 DocumentDB 的空间索引？

请此[Github 项目](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs)有关代码示例，参阅。

## <a name="introduction-to-spatial-data"></a>空间数据简介

空间数据描述的位置和形状的空间中的对象。 在大多数应用程序，这些对应上了地球，即地理空间数据的对象。 空间数据可用于表示一个人、 一个地方感兴趣或一个城市或湖泊的边界的位置。 常见使用案例通常涉及近似查询，例如，"查找所有咖啡店附近我当前所在的位置"。 

### <a name="geojson"></a>GeoJSON
DocumentDB 支持索引和[GeoJSON 规范](http://geojson.org/geojson-spec.html)表示地理空间数据的查询。 GeoJSON 数据结构始终是有效的 JSON 对象，以便他们可以存储和查询而无需任何专用的工具或库使用 DocumentDB。 DocumentDB Sdk 提供帮助器类和方便地处理空间数据的方法。 

### <a name="points-linestrings-and-polygons"></a>点、 linestrings 和多边形
一个**点**表示空间中的单个位置。 在地理空间数据点表示的确切位置，可能是从杂货店、 展台、 一辆汽车或一个城市的街道地址。  在 GeoJSON （和 DocumentDB），它使用其坐标对或经度和纬度表示一个点。 下面是示例 JSON 的点。

**DocumentDB 中的点**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] GeoJSON 规范指定经度第一和纬度第二。 像其他映射应用程序中，经度和纬度角，以度为单位表示。 经度值从子午线测量和介于-180 和 180.0 度和纬度值距离赤道并且-90.0 之间和 90.0 度。 
>
> DocumentDB 解释坐标表示每个 WGS 84 参考系统。 请参阅下面的坐标参考系统有关的更多详细信息。

此事件可以在用户配置文件包含位置数据的此示例中所示的 DocumentDB 文档中嵌入︰

**使用配置文件存储在 DocumentDB 中的位置**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

除了点，GeoJSON 还支持 LineStrings 和多边形。 **LineStrings**表示一系列的空间，并将它们连接的线段中的两个或更多点。 在地理空间数据中，linestrings 通常用于表示高速公路或河。 **多边形**是构成闭合的 LineString 边界相连的点。 多边形通常用于表示像湖泊自然形态或政治的管辖区，如城市和状态。 这里是多边形的 DocumentDB 中的一个示例。 

**在 DocumentDB 中的多边形**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] GeoJSON 规范要求，对于有效的多边形，提供的最后一个坐标对应该首先，需要创建一个闭合的形状相同。
>
>必须以逆时针顺序指定多边形内的点。 按顺时针顺序指定多边形表示在该地区的反。

除了点 LineString，多边形，GeoJSON 还指定如何对分组多个地理空间位置，以及如何将任意属性与作为一项**功能**的地理位置相关联的表示形式。 由于这些对象都是有效的 JSON，他们可以全部存储，并在 DocumentDB 中处理。 但是 DocumentDB 仅支持自动索引的点。

### <a name="coordinate-reference-systems"></a>坐标参考系统

由于地球的形状是不规则的在许多坐标参考系统 (CRS)，每个都有它们自己的参考框架和度量单位表示的地理空间数据的坐标。 例如，"国家网格的大不列颠"是参照系统是非常准确的英国，但不是外它。 

使用最流行的 CRS 目前是[WGS 84](http://earth-info.nga.mil/GandG/wgs84/)世界测量系统。 GPS 设备和许多映射服务包括 Google 地图和 Bing 地图 Api 使用 WGS 84。 DocumentDB 支持索引和查询使用 WGS 84 CRS 只将地理空间数据。 

## <a name="creating-documents-with-spatial-data"></a>空间数据创建文档
创建包含 GeoJSON 值的文档时，它们会自动建立索引的空间索引根据集合中的索引策略。 如果您正在使用类似 Python 或 Node.js 动态类型的语言在 DocumentDB SDK，您必须创建有效的 GeoJSON。

**地理空间数据的 Node.js 创建文档**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

如果您正在使用的.NET 或 Java） Sdk，您可以使用新点和多边形的 Microsoft.Azure.Documents.Spatial 命名空间中的类将位置信息嵌入在应用程序对象。 这些类可帮助简化的序列化和反序列化的空间数据为 GeoJSON。

**使用地理空间数据在.NET 中创建文档**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

如果没有的纬度和经度信息，但有的物理地址或地点名称，如城市或国家/地区，您可以使用 geocoding 服务如 Bing 地图 REST 服务来查找实际坐标。 了解更多关于 Bing Maps geocoding[在此处](https://msdn.microsoft.com/library/ff701713.aspx)。

## <a name="querying-spatial-types"></a>空间的查询类型

现在，我们已经看如何将地理空间数据，让我们看看如何查询此使用 DocumentDB 使用 SQL 和 LINQ 的数据。

### <a name="spatial-sql-built-in-functions"></a>空间 SQL 的内置函数
DocumentDB 支持地理空间查询以下开放地理空间联盟 (OGC) 内置函数。 SQL 语言中的内置函数的完整集合的详细信息，请参阅[查询 DocumentDB](documentdb-sql-query.md)。

<table>
<tr>
  <td><strong>使用</strong></td>
  <td><strong>说明</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE （point_expr，point_expr）</td>
  <td>返回两个 GeoJSON 点表达式之间的距离。</td>
</tr>
<tr>
  <td>ST_WITHIN （point_expr，polygon_expr）</td>
  <td>返回指示第一个参数中指定的 GeoJSON 点是否位于第二个参数中的 GeoJSON 多边形内的布尔表达式。</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>返回一个布尔值，该值指示指定的 GeoJSON 点或多边形表达式是否有效。</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>返回一个 JSON 值，包含一个布尔值并指定的 GeoJSON 点或多边形表达式是否有效，如果无效，此外作为字符串值的原因。</td>
</tr>
</table>

空间的函数可用于执行对空间数据的近似查询。 例如，下面是一个查询，返回不超过 30 公里的 ST_DISTANCE 内置函数使用指定位置的所有系列文档。 

**查询**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**结果**

    [{
      "id": "WakefieldFamily"
    }]

如果包括空间索引在索引策略，然后"距离查询"提供有效地通过索引。 对空间索引的详细信息，请参阅下面的部分。 如果没有为指定的路径对空间索引，仍然可以通过指定执行空间查询`x-ms-documentdb-query-enable-scan`请求标头值设置为"true"。 在.NET 中，这可以通过将可选的**FeedOptions**参数传递给查询[EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery)设置为 true。 

ST_WITHIN 可用于检查是否点位于多边形。 多边形常用来表示如 zip 代码、 状态边界或自然形态的边界。 再次如果包括空间索引在索引策略，然后"内"查询提供有效地通过索引。 

ST_WITHIN 中的多边形参数可以包含单声响铃，即多边形不能包含在它们的孔。 

**查询**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**结果**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] 类似于在 DocumentDB 查询，如果既参数格式不正确或无效，则它的计算结果为**不明确**的和计算的文档，可以跳过从查询结果中指定的位置值的方式不匹配类型作品。 如果查询未不返回任何结果，请运行 ST_ISVALIDDETAILED 到调试 spatail 类型无效的原因。     

DocumentDB 还支持执行反向查询，即您可以多边形或 DocumentDB，中行的索引，然后查询包含指定的点的区域。 此模式通常在物流中用于标识例如当卡车进入或离开指定的区域。 

**查询**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**结果**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID 和 ST_ISVALIDDETAILED 可以用来检查空间对象是否有效。 例如，以下查询检查点的有效性不足的区域的纬度值 (-132.8)。 ST_ISVALID 返回的只是一个布尔值，ST_ISVALIDDETAILED 返回布尔值和一个字符串，包含为何被认为无效的原因。

**查询**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**结果**

    [{
      "$1": false
    }]

这些函数也可用于验证的多边形。 例如，在这里我们使用 ST_ISVALIDDETAILED 来验证未闭合的多边形。 

**查询**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**结果**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ 查询.NET SDK 中

DocumentDB.NET SDK 还提供存根方法`Distance()`，`Within()`在 LINQ 表达式中使用。 DocumentDB LINQ 提供程序转换为等效的 SQL 内置函数调用这些方法调用 (ST_DISTANCE 和 ST_WITHIN 分别)。 

下面是其"位置"值是指定的 30 公里半径内点使用 LINQ 的 DocumentDB 集合中找到的所有文档的 LINQ 查询的一个示例。

**LINQ 查询的距离**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

同样，这里是寻找其"位置"位于每个多边形的框中指定的所有文档的查询。 

**LINQ 查询中**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


现在，我们已经看如何查询使用 LINQ 和 SQL 的文档，看看如何配置 DocumentDB 空间索引。

## <a name="indexing"></a>索引

如我们所述[架构不可知索引使用 Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)白皮书中，我们设计了 DocumentDB 的数据库引擎可真正架构不可知和第一类为提供支持 JSON。 DocumentDB 写优化的数据库引擎本身了解 GeoJSON 标准中所表示的空间数据 （点、 多边形和行）。

简而言之，几何投影到二维平面测量坐标则逐渐划分为使用**quadtree**的单元格。 这些单元格映射到一维**希尔伯特空间填充曲线**，保持点的区域内的单元格的位置。 此外当位置数据进行索引，经历此过程称为**镶嵌**，即所有的单元格相交位置标识，并将其存储为 DocumentDB 索引中的键。 在查询时，如点和多边形的参数还镶嵌中提取相应的单元格 ID 范围，则用于从索引检索数据。

如果您指定包含空间索引的索引策略 / * （所有路径），然后在集合中找到的所有点将被都索引以便有效空间查询 （ST_WITHIN 和 ST_DISTANCE）。 空间索引不具有精度值，并始终使用缺省精度值。

>[AZURE.NOTE] DocumentDB 支持的点、 多边形和 LineStrings 自动索引

下面的 JSON 段显示与已启用的空间索引的索引创建策略，即索引进行空间查询在文档中找到任何 GeoJSON 点。 如果您要修改的索引策略使用 Azure 门户，您可以指定要启用空间索引的集合索引策略下 JSON。

**集合索引策略 JSON 与空间点和多边形启用**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

这里正在演示如何创建一个空间索引打开包含点的所有路径的.NET 代码段。 

**创建空间索引的集合**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

并且这里是如何修改现有的收藏集以利用空间索引存储在文档中的任意点上。

**修改现有集合了空间索引**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] 如果格式不正确或无效的位置在文档中的 GeoJSON 值，然后它将被索引，以进行空间查询。 您可以验证在使用 ST_ISVALID 和 ST_ISVALIDDETAILED 的位置值。
>
> 如果集合定义中包含分区键，则不报告索引转换进度。 

## <a name="next-steps"></a>下一步行动
现在，他们已经了解到有关如何开始使用地理空间支持在 DocumentDB 中，您可以︰

- 在开始编码[在 Github 上的地理空间.NET 代码示例](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)使用
- 获取在[DocumentDB 查询运动场](http://www.documentdb.com/sql/demo#geospatial)查询与地理空间上的手
- 了解更多关于[DocumentDB 查询](documentdb-sql-query.md)
- 了解更多关于[DocumentDB 索引策略](documentdb-indexing-policies.md)
