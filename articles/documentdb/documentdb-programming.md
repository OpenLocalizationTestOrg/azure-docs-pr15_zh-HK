<properties 
    pageTitle="DocumentDB 编程︰ 存储过程、 数据库触发器和 Udf |Microsoft Azure" 
    description="了解如何使用 DocumentDB 在 JavaScript 编写的存储的过程、 数据库触发器和用户定义函数 (Udf)。 获取数据库编程技巧等。" 
    keywords="数据库触发器、 存储的过程、 存储的过程、 数据库程序、 存储过程、 documentdb、 azure、 Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB 服务器端编程︰ 存储过程、 数据库触发器和 Udf

了解如何 Azure DocumentDB 语言集成、 事务执行的 JavaScript 使开发人员能够编写**存储过程**、**触发器**和**用户定义函数 (Udf)**本身在 JavaScript 中。 这使您可以编写可和直接在数据库存储分区上执行的数据库程序应用程序逻辑 

我们建议开始通过观看下面的视频，其中 Andrew 先生提供 DocumentDB 的服务器端数据库的编程模型的简要介绍。 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

然后，返回到本文中，您学习以下问题的答案︰  

- 如何编写存储的过程、 触发器或使用 JavaScript 的 UDF？
- DocumentDB 能酸有什么保证？
- 交易记录在 DocumentDB 中如何工作？
- 什么是预触发和后触发以及如何写？
- 如何注册并通过使用 HTTP 以 rest 风格的方式执行存储的过程、 触发器或 UDF？
- DocumentDB Sdk 都可用来创建和执行存储过程、 触发器和 Udf？

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>存储的过程和 UDF 编程简介

*"作为现代天 T SQL JavaScript"*此方法释放应用程序开发人员不再需要类型系统不匹配和对象关系映射技术的复杂性。 它还具有多个可被利用来生成丰富的应用程序的固有优点︰  

-   **的过程逻辑︰**作为高级编程语言，JavaScript 提供丰富且熟悉界面来表达业务逻辑。 您可以执行复杂的操作更近的数据序列。

-   **原子事务︰**DocumentDB 保证，在单个存储过程内执行数据库操作和触发器都是原子。 这样，可以组合在单个批处理中的相关的操作，以便所有这些成功或其中任何一个成功的应用。 

-   **的性能︰**对大量的类似惰性具体化的 JSON 文档中缓冲池，并使其可用的按需执行代码的优化使事实 JSON 本质上的 Javascript 语言类型系统映射和也是在 DocumentDB 中的存储的基本单位。 有与运输业务逻辑与数据库相关联的多个性能优势︰
    -   批处理 – 开发人员可以进行分组操作，如插入和批量进行提交。 极大地减少成本的网络通信延迟和存储开销，以便创建单独的交易记录。 
    -   预编译 – DocumentDB 预编译的存储的过程、 触发器和用户定义函数 (Udf)，以避免每次调用的 JavaScript 编译成本。 生成过程的逻辑的字节代码的开销被分摊为最小值。
    -   先后顺序 – 许多操作需要一个副作用 （"触发器"），其中可能涉及到执行一个或多个辅助存储操作。 除了原子性，这是更多的性能，当移动到服务器上。 
-   **封装︰**存储的过程可用于组合在一个位置中的业务逻辑。 这样做有两个优点︰
    -   它将添加一个抽象层之上的原始数据，使数据架构师要发展他们独立于数据的应用程序。 当数据架构灵活，由于脆弱的假设可能需要融入该应用程序，如果他们要直接处理数据，这是特别有用。  
    -   这种抽象形式允许企业通过优化从脚本访问保证数据的安全。  

通过[REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)、 [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)和[客户端 Sdk](documentdb-sdk-dotnet.md)中包括.NET，Node.js 和 JavaScript 的多平台支持的创建和执行数据库触发器、 存储的过程和自定义的查询运算符。

**本教程使用[Q 承诺与 Node.js SDK](http://azure.github.io/azure-documentdb-node-q/)**说明语法和用法的存储的过程、 触发器和 Udf。   

## <a name="stored-procedures"></a>存储的过程

### <a name="example-write-a-simple-stored-procedure"></a>示例︰ 编写一个简单的存储的过程 
我们先简单的存储过程返回的"Hello World"响应。

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


存储的过程注册每个集合，而且可以对任何文档和附件存在于该集合进行操作。 下面的代码段演示如何使用集合注册 helloWorld 存储过程。 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


存储的过程注册后，我们可以执行它根据该集合，并读取客户端在回发的结果。 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


该上下文对象提供权可以在 DocumentDB 存储执行的所有操作请求和响应对象的访问。 在这种情况下，我们可以使用响应对象来设置已发送回客户端的响应的正文。 有关详细信息，请参阅[DocumentDB JavaScript 服务器 SDK 文档](http://azure.github.io/azure-documentdb-js-server/)。  

让我们扩展本示例并添加多个数据库的存储过程相关的功能。 存储的过程可以创建、 更新、 读取、 查询和删除文档和集合内的附件。    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>示例︰ 编写一个存储的过程来创建文档 
下一步的代码段演示如何使用上下文对象与 DocumentDB 资源进行交互。

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


此存储的过程将作为输入 documentToCreate，将在当前集合中创建的文档的正文。 这样的操作是异步的取决于 JavaScript 函数回调。 回调函数具有两个参数，在种情况下，此操作失败，错误对象和所创建的对象。 在回调中，用户可以处理的异常或引发错误。 如果未提供回调，并且没有错误，DocumentDB 运行时将引发错误。   

在上面的示例中，回调引发一个错误，如果该操作失败。 否则，它将创建的文档的 id 设置为对客户端的响应的正文。 下面是如何使用输入参数执行此存储的过程。

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
注意，此存储过程可以修改文档正文数组作为输入并创建它们都在同一个存储的过程的执行而不是单独创建每个多个网络请求。 这可以用于 DocumentDB （在本教程后面部分讨论） 为实现高效批量导入程序。   

所述的示例演示了如何使用存储的过程。 在本教程后面部分，我们将介绍触发器和用户定义函数 (Udf)。

## <a name="database-program-transactions"></a>数据库程序事务
在典型的数据库中的事务可以定义为一系列作为单个逻辑工作单元执行的操作。 每个事务提供**酸可以保证**。 酸是已知的首字母缩写，代表四种属性的原子性、 一致性、 隔离性和持久性。  

简言之，原子可以保证在事务内执行的所有工作，被都视为一个单元其中任一全部致力或无。 一致性可确保数据始终处于良好的内部状态跨事务。 两个事务不会相互干扰--通常，大多数商业系统提供多个可以使用的隔离级别的应用程序需要基于可保证隔离。 耐久性可确保始终会存在数据库中提交的任何更改。   

在 DocumentDB，JavaScript 被承载数据库相同的内存空间。 因此，请求中的存储过程和触发器在同一数据库会话的作用域中执行。 这使 DocumentDB 以保证 ACID 所有属于一个存储过程/触发器的操作。 请考虑下面的存储的过程定义︰

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

此存储的过程使用一个游戏应用程序在单个操作中的两个运动员之间的贸易项目中的交易记录。 该存储的过程将尝试读取每个对应玩家 Id 作为参数传入的两个文档。 如果找到两个播放机的文档，然后存储的过程更新这些文档来换自己的项目。 如果途中遇到任何错误，则会引发一个隐式中止该事务的 JavaScript 异常。

如果注册到该存储过程对照单分区集合中，则该事务的作用范围是集合中的所有 docuemnts。 如果对集合进行分区，则存储的过程执行单个分区键的事务作用域中。 每个存储过程的执行则必须包括交易记录必须在其下运行的范围对应的分区键值。 有关详细信息，请参阅[DocumentDB 分区](documentdb-partition-data.md)。

### <a name="commit-and-rollback"></a>提交和回滚
交易记录已深和本机集成到 DocumentDB 的 JavaScript 编程模型。 在 JavaScript 函数内, 所有操作自动都换下一个事务。 如果 JavaScript 完成后没有任何异常，被提交到数据库的操作。 实际上，在关系数据库中的"开始交易"和"提交事务处理"语句是在 DocumentDB 隐式的。  
 
如果脚本从传播任何异常，DocumentDB 的 JavaScript 运行时将会回滚整个事务。 在前面的示例中所示，引发异常是实际上等同于 DocumentDB 中的"回滚事务"。
 
### <a name="data-consistency"></a>数据一致性
在 DocumentDB 集合的主副本始终执行存储的过程和触发器。 这样可确保从内部读取存储过程提供强一致性。 使用用户定义函数的查询可以执行的主要或次要的任何副本，但是我们保证以满足要求的一致性等级，通过选择适当的复制副本。

## <a name="bounded-execution"></a>有限的执行
DocumentDB 的所有操作必须在指定的服务器都完成请求超时持续时间。 此限制也适用于 JavaScript 函数 （存储的过程、 触发器和用户定义的函数） 中。 如果操作未完成与该时间限制，则回滚事务。 JavaScript 函数必须在时间限制内完成或实现一个延续基于模型以批处理/继续执行。  

为了简化开发存储的过程和触发器来处理时间限制，在集合对象 （创建、 读取、 替换和删除文档和附件） 下的所有函数都返回一个布尔值，该值代表是否将完成该操作。 如果此值为 false，则表明时限即将到期，该过程必须总结执行。  第一次未接受的存储操作操作排队的之前保证如果此存储的过程在时间内完成并不排任何更多的请求完成。  

JavaScript 函数还在资源消耗上包围起来。 DocumentDB 保留每个集合基于数据库帐户已设置大小的吞吐量。 吞吐量是根据标准化的 CPU、 内存和 IO 消耗称为请求单位或 RUs 单位来表示的。 JavaScript 函数可能会占用大量的 RUs 短的时间，并可能得到速率限制如果达到该集合的限制。 此外可能隔离资源密集型存储的过程以确保基本的数据库操作的可用性。  

### <a name="example-bulk-importing-data-into-a-database-program"></a>示例︰ 批量数据导入数据库程序
下面是存储过程写入到批量导入的文档集合的示例。 注意存储的过程通过检查的布尔值来处理有限的执行的方式从 createDocument，返回的值，然后使用插入到每个存储过程的调用中的文档数来跟踪和批间恢复进度。

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>数据库触发器
### <a name="database-pre-triggers"></a>数据库预触发器
DocumentDB 提供了执行或文档上的操作触发的触发器。 例如，您可以指定预触发器时创建的文档 – 之前创建文档时，将运行此预触发器。 以下是如何使用预触发器来验证正在创建文档的属性的示例︰

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


并相应的 Node.js 客户端注册代码触发器︰

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


预触发器不能有任何输入的参数。 请求对象可以用于操作和操作相关联的请求消息。 在这里，预触发正在运行使用的一个文档，创建并请求邮件正文中包含该文档以 JSON 格式创建。   

当触发器都注册后时，用户可以指定它使用可以运行的操作。 使用 TriggerOperation.Create，这意味着以下不允许创建此触发器。

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>数据库后触发器
如预触发器，后期触发器与文档执行操作和不采取任何输入的参数。 他们运行**后**操作已完成，并有权向客户端发送响应消息。   

下面的示例演示在操作后的触发器︰

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


下面的示例中所示，可以注册触发器。

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


此触发器元数据文档中查询和更新其有关新创建的文档的详细信息。  

值得注意的一件事是 DocumentDB 中的触发器在**事务**执行。 此后期触发器运行作为原始文档的创建同一事务的一部分。 因此，如果我们后期的触发器 （说如果我们现在不能更新元数据文档） 从引发异常，则整个事务将失败，并且会回滚。 将创建的文档，并将返回异常。  

##<a id="udf"></a>用户定义的函数
用户定义函数 (Udf) 用来扩展 DocumentDB SQL 查询语言语法和实现自定义业务逻辑。 仅从可调用内查询。 他们不具有访问权限的上下文对象，并打算用作计算纯 JavaScript。 因此，Udf 可以运行在 DocumentDB 服务的辅助副本。  
 
下面的示例创建 UDF 计算所得税根据各种收入支架，率，然后将其使用嵌套查询，以查找所有人支付税款超过 20000 美元。

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


UDF 随后可以使用类似下面的示例中的查询中︰

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript 语言集成查询 API
除了颁发使用 DocumentDB 的 SQL 语法的查询，服务器端 SDK 允许您执行优化的查询使用 JavaScript 界面无需了解 SQL 的流畅。 JavaScript 查询 API 允许您以编程方式构建查询，通过将谓词函数传递到可链接的函数调用，为 ECMAScript5 的数组 built-ins 和 lodash 类似的流行 JavaScript 库熟悉语法。 通过有效地使用 DocumentDB 的索引执行的 JavaScript 运行时分析查询。

> [AZURE.NOTE]`__` （双下划线） 是一个别名为`getContext().getCollection()`。
> <br/>
> 换句话说，您可以使用`__`或`getContext().getCollection()`访问 JavaScript 查询 API。

受支持的功能包括︰
<ul>
<li>
<b>chain()...。值 ([回调] [，选项])</b>
<ul>
<li>
必须终止的链式的调用开头使用 value （）。
</li>
</ul>
</li>
<li>
<b>筛选器 (predicateFunction [，选项] [，回调])</b>
<ul>
<li>
筛选器使用的谓词函数，它返回到结果集中筛选输入/输出输入文档以真/假的输入。 此行为类似于 SQL WHERE 子句。
</li>
</ul>
</li>
<li>
<b>映射 (transformationFunction [，选项] [，回调])</b>
<ul>
<li>
适用于给定转换函数将每个输入的项映射到一个 JavaScript 对象或值的投影。 此行为类似于 SQL 中的 SELECT 子句。
</li>
</ul>
</li>
<li>
<b>弦乐器 ([属性] [，选项] [，回调])</b>
<ul>
<li>
这是地图，它从每个输入项中提取单个属性的值的快捷方式。
</li>
</ul>
</li>
<li>
<b>平面化 ([isShallow] [，选项] [，回调])</b>
<ul>
<li>
将组合在一起，并合并到单个阵列中的每个输入项数组。 此行为类似于 LINQ 中的 SelectMany。
</li>
</ul>
</li>
<li>
<b>排序方式 ([谓语] [，选项] [，回调])</b>
<ul>
<li>
通过排序升序排序使用给定的谓词的输入的文档流中的文档来生成一组新的文档。 此行为类似于 SQL 中的 ORDER BY 子句。
</li>
</ul>
</li>
<li>
<b>sortByDescending ([谓语] [，选项] [，回调])</b>
<ul>
<li>
通过排序降序排序使用给定的谓词的输入的文档流中的文档来生成一组新的文档。 此行为类似于 SQL 中的 x DESC ORDER BY 子句。
</li>
</ul>
</li>
</ul>


当包含谓词和/或选择器函数内，下面的 JavaScript 构造获取自动优化在 DocumentDB 索引上直接运行︰

* 简单的运算符: = +-* / %|^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= ||&amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* 原义字符，其中包括文字对象: {}
* var 返回

下面的 JavaScript 构造不获取适合 DocumentDB 索引︰

* 控制流 (例如如果您，虽然)
* 函数调用

有关详细信息，请参阅我们的[服务器端 JSDocs](http://azure.github.io/azure-documentdb-js-server/)。

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>示例︰ 编写存储的过程使用 JavaScript 查询 API

下面的代码示例是如何可以在存储过程的上下文中使用 JavaScript 查询 API 的示例。 该存储的过程将插入文档中，通过输入参数，给出并更新元数据文档，使用`__.filter()`方法，使用 minSize、 maxSize 和 totalSize 根据输入的文档的大小属性。

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>Javascript 作弊表的 SQL
下表列出了各种 SQL 查询和相应的 JavaScript 查询。

如与 SQL 查询、 文档属性键 (例如`doc.id`) 是区分大小写。

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>JavaScript 查询 API</th>
<th>详细信息</th>
</tr>
<tr>
<td>
<pre>
选择 * 从文档
</pre>
</td>
<td>
<pre>
__.map(function(doc) {返回文档;});
</pre>
</td>
<td>在所有的文档 （与延续标记分页） 作为结果是。</td>
</tr>
<tr>
<td>
<pre>
选择 docs.id、 docs.message 为消息，docs.actions 从文档
</pre>
</td>
<td>
<pre>
__.map(function(doc) {返回 {id: doc.id、 消息︰ doc.message、 操作︰ doc.actions};});
</pre>
</td>
<td>项目 id、 消息 （化名为 msg） 和所有文档中的操作。</td>
</tr>
<tr>
<td>
<pre>
选择 * 从文档位置 docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.filter(function(doc) {返回 doc.id ==="X998_Y998";});
</pre>
</td>
<td>文档使用谓词的查询︰ id ="X998_Y998"。</td>
</tr>
<tr>
<td>
<pre>
选择 * 从文档位置 ARRAY_CONTAINS(docs.标记，123）
</pre>
</td>
<td>
<pre>
__.filter(function(x) {返回 x.Tags & & x.Tags.indexOf(123) >-1;});
</pre>
</td>
<td>对于具有标记属性和标记文档的查询是一个数组，包含值 123。</td>
</tr>
<tr>
<td>
<pre>
选择 docs.id，docs.message 作为消息源文档的位置 docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain().filter(function(doc) {返回 doc.id ==="X998_Y998";}).map(function(doc) {返回 {id: doc.id、 消息︰ doc.message};}).value();
</pre>
</td>
<td>查询的谓词、 文档 id ="X998_Y998"，然后投影的 id 和消息 （化名为 msg）。</td>
</tr>
<tr>
<td>
<pre>
选择的值从文档加入标记在文档的标记。ORDER BY docs._ts 标记
</pre>
</td>
<td>
<pre>
__.chain().filter(function(doc) {返回文档。标记 & & Array.isArray 技术支持 (doc。标记）;}）.sortBy(function(doc) {返回 doc._ts;}).pluck("Tags").flatten().value()
</pre>
</td>
<td>为具有数组属性，标签的文档筛选器和排序由 _ts 时间戳系统属性，然后项目生成的文档 + 展平标记数组。</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>运行库支持
[DocumentDB JavaScript 服务器端 SDK](http://azure.github.io/azure-documentdb-js-server/)提供作为标准化的[ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)主流的 JavaScript 语言功能的最大支持。

### <a name="security"></a>安全
JavaScript 的存储过程和触发器的沙盒，这样一个脚本的效果不要泄漏到另而无需经过快照事务隔离级别的数据库。 运行时环境被放入池中，但每次运行后清理上下文。 因此能够保证相互安全的任何非预期的副作用。

### <a name="pre-compilation"></a>预编译
存储的过程、 触发器和 Udf 则是隐式预编译为字节代码格式以在每个脚本调用时避免编译成本。 这可确保存储过程的调用的速度快，具有很低的需求量。

## <a name="client-sdk-support"></a>客户端 SDK 支持
除了[Node.js](documentdb-sdk-node.md)客户端，DocumentDB 还支持[.NET](documentdb-sdk-dotnet.md)、 [Java](documentdb-sdk-java.md)、 [JavaScript](http://azure.github.io/azure-documentdb-js/)和[Python 的 Sdk](documentdb-sdk-python.md)。 存储的过程、 触发器和 Udf 可以创建和执行任何这些 Sdk 也使用了。 下面的示例演示如何创建和执行存储的过程使用.NET 客户端。 请注意如何传递到存储过程作为 JSON 和朗读.NET 类型。

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


此示例演示如何使用[.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx)来创建预触发器，并使用启用触发器创建文档。 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


然后，下面的示例演示如何创建用户定义函数 (UDF) 和在[DocumentDB 的 SQL 查询](documentdb-sql-query.md)中使用它。

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API，
所有 DocumentDB 操作可以都执行 rest 风格的方式。 存储的过程、 触发器和用户定义的函数可以使用 HTTP POST 注册下集合。 下面是如何注册存储的过程的一个示例︰

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


存储的过程都由与正文包含要创建的存储的过程执行 POST 请求 URI dbs/testdb/colls/testColl/存储过程根据注册。 触发器和 Udf 可以注册同样分别发出公告针对 /triggers 和 /udfs。
此存储过程可以然后执行通过发出 POST 请求对其资源的链接︰

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


在这里，在请求正文中传递给存储过程的输入。 请注意，输入作为 JSON 数组输入参数的传递。 存储的过程采用第一个输入作为响应正文的文档。 我们收到的响应是，如下所示︰

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


与不同的存储过程，触发器不能直接执行。 而它们都执行对文档的操作的一部分。 我们可以指定触发器，以便运行与使用 HTTP 标头的请求。 以下为创建文档的请求。

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


这里与请求运行预触发器 x-ms-documentdb-pre-trigger-include 的标题中指定。 相应地，给出任何后期触发器 x-ms-documentdb-post-trigger-include 标头中。 请注意，两个前和后期触发器可以指定给定请求。

## <a name="sample-code"></a>示例代码

我们[Github 存储库](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples)中，您可以找到更多的服务器端代码示例 （包括[批量删除](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)和[更新](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)）。

要共享超存储的过程？ 请向我们发送一个请求请求 ！ 

## <a name="next-steps"></a>下一步行动

一个或多个存储的过程、 触发器和用户定义的函数创建后，您可以加载它们并 Azure 门户使用脚本资源管理器中查看它们。 有关详细信息，请参阅[查看存储过程、 触发器和用户定义的函数使用 DocumentDB 脚本资源管理器](documentdb-view-scripts.md)。

您可能还发现以下引用和资源在您的路径，以了解更多关于 DocumentDB 服务器端编程非常有用︰

- [Azure DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON 类型系统](http://www.json.org/js.html) 
- [安全且可移植的数据库可扩展性](http://dl.acm.org/citation.cfm?id=276339) 
- [面向服务的数据库体系结构](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [在 Microsoft SQL server.NET 运行库宿主](http://dl.acm.org/citation.cfm?id=1007669)
