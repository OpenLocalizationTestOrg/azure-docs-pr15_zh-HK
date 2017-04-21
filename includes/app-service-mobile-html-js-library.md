##<a name="create-client"></a>创建客户端连接

通过创建创建客户端连接`WindowsAzure.MobileServiceClient`对象。  更换`appUrl`与您的移动应用程序的 URL。

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>使用表格

来访问或更新数据，创建对后端表的引用。 更换`tableName`您的表的名称

```
var table = client.getTable(tableName);
```

一旦表引用，您可以进一步使用您的表︰

* [查询表](#querying)
  * [筛选数据](#table-filter)
  * [数据分页](#table-paging)
  * [对数据进行排序](#sorting-data)
* [插入数据](#inserting)
* [修改数据](#modifying)
* [删除数据](#deleting)

###<a name="querying"></a>如何︰ 查询表引用

一旦表引用，可用于服务器上的数据查询。  "LINQ 类似"语言中进行查询。
要从表格返回所有数据，请使用以下方法︰

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

成功函数的结果。   不要使用`for (var i in results)`的成功，将循环访问结果中包含的信息可正常时其他查询功能 (如`.includeTotalCount()`) 使用。

有关查询语法的详细信息，请参阅 [查询文档对象]。

####<a name="table-filter"></a>在服务器上筛选数据

您可以使用`where`上表引用的子句︰

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

您还可以使用筛选器对象的函数。  在这种情况下`this`变量分配给当前对象进行筛选。  以下是功能上等效于前面的示例︰

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>数据分页

利用 take() 和 skip() 的方法。  例如，如果您想要将表拆分为 100 行的记录︰

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

`.includeTotalCount()`方法用于将一个 totalCount 字段添加到结果对象。  TotalCount 字段充满如果没有分页使用返回的记录的总数。

然后可用页变量和用户界面的某些按钮来提供页面列表;使用 loadPage() 加载每个页面的新记录。  应实施某种形式的速度访问的记录被加载到缓存。


####<a name="sorting-data"></a>如何︰ 返回的数据进行排序

使用.orderBy() 或.orderByDescending() 查询方法︰

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

有关查询对象的详细信息，请参阅 [查询文档对象]。

###<a name="inserting"></a>如何︰ 将数据插入

使用适当的日期创建一个 JavaScript 对象，并以异步方式调用 table.insert():

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

成功插入，插入的项返回与所需的同步操作的附加字段。  您应使用此信息，以便以后的更新更新自己的缓存。

请注意 Azure 移动应用程序 Node.js 服务器 SDK 动向支持动态架构。
对于动态架构，实时，使您可以向表中添加列，只是通过指定在插入或更新操作更新表的架构。  我们建议，动态架构之前关闭移动到生产应用程序。

###<a name="modifying"></a>如何︰ 修改数据

类似于.insert() 方法，您应该创建一个更新的对象，然后调用.update()。  更新对象必须包含要更新的记录的 ID-这获取读取记录时或调用.insert() 时。

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>如何︰ 删除数据

调用.del() 方法来删除一条记录。  传递的对象引用中的 ID:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
