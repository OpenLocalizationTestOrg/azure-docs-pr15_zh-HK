<properties
    pageTitle="如何使用 Android 的移动应用程序客户端库"
    description="如何使用 Android 客户端 SDK Azure 移动应用程序。"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>如何对移动应用程序中使用 Android 的客户端库

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

本指南介绍了如何使用 Android 的客户端 SDK 的移动应用程序实现的常见情形中，如︰

- 查询数据 （插入、 更新和删除）。
- 身份验证。
- 处理错误。
- 自定义客户端。 

此外那样深入到大多数移动应用程序中使用的常见客户端代码。

本指南重点介绍客户端 Android SDK。  若要了解有关服务器端 Sdk 的移动应用程序，请参阅[使用.NET 后端 SDK] [10]或[如何使用 Node.js 后端 SDK][11]。

## <a name="reference-documentation"></a>参考文档

您可以找到[Javadocs API 参考][12]在 GitHub 上 Android 的客户端库。

## <a name="supported-platforms"></a>支持的平台

Azure 移动应用程序的 Android SDK 支持 API 级别 19 至 24 (KitKat 通过 Nougat)。  

"服务器流程"身份验证提供的用户界面使用 web 视图。 如果该设备不能提供一个 web 视图用户界面，然后其他身份验证方法需要产品的范围之外。  此 SDK 不适用于监视类型或同样受限制的设备。

## <a name="setup-and-prerequisites"></a>安装和系统必备组件

完成[移动应用程序快速入门](app-service-mobile-android-get-started.md)教程。  此任务可以确保开发 Azure 的移动应用程序的所有前提条件已得到满足。  快速入门还可帮助您配置您的帐户，并创建您的第一个移动应用程序后端。

如果您决定不完成快速入门教程，请完成以下任务︰

- [创建移动应用程序端][ 13] Android 应用程序中使用。
- 在 Android Studio 中，[更新 Gradle 生成文件](#gradle-build)。
- [启用 internet 权限](#enable-internet)。

###<a name="gradle-build"></a>更新 Gradle 生成文件

更改两个**build.gradle**文件︰

1. 将此代码添加到*buildscript*标记中的*项目*级别的**build.gradle**文件︰

        buildscript {
            repositories {
                jcenter()
            }
        }

2. 将此代码添加到*模块的应用程序*级别的**build.gradle**文件内部*依赖项*标记︰

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    目前最新版本是 3.1.0。 列出了受支持的版本[在此处][14]。

###<a name="enable-internet"></a>启用 internet 权限

若要访问 Azure，您的应用程序必须启用了 INTERNET 权限。 如果尚未启用，请向**AndroidManifest.xml**文件添加以下代码行︰

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>基础知识深入探究

本节讨论一些适用于使用 Azure 移动应用程序快速入门应用程序中的代码。  

###<a name="data-object"></a>定义客户端数据类

若要访问 SQL Azure 表中的数据，定义到移动应用程序的后端中的表对应的客户端数据类。 本主题中的示例假定一个名为**ToDoItem**，具有以下列的表︰

- 标识
- 文本
- 完成

相应的输入端的对象︰

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

该代码驻留在名为**ToDoItem.java**的文件中。

如果 SQL Azure 表中包含多个列，您将向此类添加相应的字段。  例如，如果将 DTO （数据传输对象） 有整数优先级列中，然后您可以添加该字段，以及其 getter 和 setter 方法︰

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

若要了解如何在您的移动应用程序后端中创建附加的表，请参阅[如何︰ 定义一个表格控制器][ 15] （.NET 后端） 或[定义表使用动态架构][ 16] （Node.js 后端）。 对 Node.js 后端，您还可以使用**简单的表**设置[Azure 门户]中。

###<a name="create-client"></a>如何︰ 创建客户端环境

此代码将创建**MobileServiceClient**对象，用于访问您移动应用程序的后端。 代码将放`onCreate`**活动**类为**主要**操作和**启动程序**类别在*AndroidManifest.xml*中指定的方法。 在快速启动代码中，将它放在**ToDoActivity.java**文件中。

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

在此代码中，将`MobileAppUrl`的移动应用程序端 url，可找到的[Azure 门户]中为您的移动应用程序后端刀片式服务器。 对于这行代码进行编译，您还需要添加下面的**导入**语句︰

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>如何︰ 创建表引用

查询或修改在后端的数据的最简便方法是使用*类型化编程模型*，因为 Java 是一种强类型的语言。 此模型提供了无缝的 JSON 序列化和反序列化使用[gson] [3]库的后端 SQL Azure 中发送客户端对象与表之间的数据时。

若要访问一个表，请首先创建[MobileServiceTable] [8]通过调用对象**可获取**方法[MobileServiceClient][9]。  此方法具有两个重载︰

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

在以下代码中， **mClient**是 MobileServiceClient 对象的引用。  其中的类名称和表名称是相同的和一个用于快速入门中使用第一个重载︰

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

不同于类名称的表名称时使用第二个重载︰ 第一个参数是表名。

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>如何︰ 将数据绑定到用户界面

数据绑定涉及三个组件︰

- 数据源
- 屏幕布局
- 将这两者联系在一起的适配器。

在我们的示例代码中，我们从返回数据移动应用程序的 SQL Azure 表**ToDoItem**到一个数组。 此活动是为数据应用程序的常见模式。  通常，数据库查询返回客户端获取列表或数组中的行的集合。 在此示例中，该数组为数据源。

该代码指定设备定义所显示的数据的视图的屏幕布局。  这两个被装订在一起的适配器，此代码中是一种扩展的**ArrayAdapter&lt;ToDoItem&gt;**类。

#### <a name="layout"></a>如何︰ 定义布局

由几个代码段的 XML 代码定义布局。 下面的代码提供现有的布局，代表我们希望与我们的服务器数据填充**列表视图**。

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

在前面的代码中，*列表项*属性指定的布局列表中的单个行的 id。 此代码指定复选框和它相关联的文本，并获取一个实例化一次，每个列表中的项。 此布局不显示**id**字段中，并且更复杂的布局可以指定附加字段中显示。 此代码是在**row_list_to_do.xml**文件中。

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>如何︰ 定义适配器

由于我们的视图的数据源是数组的**ToDoItem**，我们子类从我们适配器**ArrayAdapter&lt;ToDoItem&gt;**类。 该子类产生的每个**ToDoItem**使用**row_list_to_do**布局视图。

在我们的代码中，我们定义下面的类是一种扩展的**ArrayAdapter&lt;E&gt;**类︰

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

重写该适配器**getView**方法。 例如︰

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

我们创建此类的实例在我们活动中，如下所示︰

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

ToDoItemAdapter 构造函数的第二个参数是对布局的引用。 我们现在可以实例化**列表视图**，并将适配器分配给**列表视图**。

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>API 结构

移动应用程序表操作和自定义的 API 调用是异步的。 [未来]和[AsyncTask]对象涉及查询、 插入、 更新和删除操作的异步方法，请使用 使用将来容易地执行多个操作在后台线程上的，而无需处理多个嵌套的回调。

查看示例 Android 快速入门从[Azure 的门户]项目中的**ToDoActivity.java**文件。

#### <a name="use-adapter"></a>如何︰ 使用适配器

现在您可以使用数据绑定。 下面的代码演示如何获取表中的项并使用本地适配器填充将退回的物料。

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

随时修改**ToDoItem**表调用该适配器。 在记录的基础上完成后的修改，因为您处理而不是集合的单个行。 当插入一项时，调用**add**方法在适配器上;删除时，调用**remove**方法。

##<a name="querying"></a>如何︰ 通过您的移动应用程序后端查询数据

本节介绍如何将查询发送到移动应用程序的后端，其中包括以下任务︰

- [返回所有项]
- [筛选返回的数据]
- [返回的数据的排序]
- [返回在页中的数据]
- [选择特定列]
- [合并查询方法](#chaining)

### <a name="showAll"></a>如何︰ 返回表中的所有项

下面的查询返回**ToDoItem**表中的所有项。

    List<ToDoItem> results = mToDoTable.execute().get();

*结果*变量返回通过为列表查询结果集。

### <a name="filtering"></a>如何︰ 筛选器未返回数据

执行以下查询返回所有项从**ToDoItem**表其中**完全**等于**false**。

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable**是我们先前创建的移动服务表引用。

定义使用**其中**方法调用上表引用的筛选器。 **其中**方法后面跟有一个方法，指定逻辑谓词**字段**方法。 可能的谓词方法包括**eq** （等于）、**内布拉斯加州**（不等于）、 **gt** （大于）、 **ge** （大于或等于）、 **lt** （小于）， **le** （小于或等于）。 这些方法使您可以为特定值的数字和字符串字段进行比较。

您可以对日期进行筛选。 下面的方法让您比较的日期部分的完整日期字段︰**年**、**月**、**日**、**小时**、**分钟**和**第二**。 下面的示例添加一个筛选器项的*到期日期*等同于 2013年。

    mToDoTable.where().year("due").eq(2013).execute().get();

下列方法支持复杂的筛选器字符串字段︰ **startsWith**、 **endsWith**、 **concat**、**子字符串**、 **indexOf**、**替换**、 **toLower**、 **toUpper**，**裁切**，和**长度**。 下面的示例筛选表行*文本*列中开始"PRI0"。

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

数字字段支持下列运算符方法︰**添加**、**子**、 **mul**， **div**、 **mod**，**地板**，**天花板**，和**圆**。 下面的示例筛选表行，**持续时间**为偶数。

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

您可以使用这些逻辑方法组合谓词︰**和**，**或**并**不**。 下面的示例组合前面示例中的两个。

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

组和嵌套逻辑运算符︰

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

有关更详细的讨论和筛选的示例，请参阅[浏览 Android 的客户端查询模型的丰富程度](http://hashtagfail.com/post/46493261719/mobile-services-android-querying)。

### <a name="sorting"></a>如何︰ 排序返回数据

下面的代码返回**ToDoItems**表中的所有项目都已通过的*文本*字段按升序排序。 *mToDoTable*是对先前创建的后端表的引用︰

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

**OrderBy**方法的第一个参数是字段的一个字符串作为排序依据的名称相同。 第二个参数使用**QueryOrder**枚举指定是否以升序或降序排序。  如果您正在筛选使用***的***方法，***其中***方法必须调用***orderBy***方法之前。

### <a name="paging"></a>如何︰ 返回在页中的数据

第一个示例演示如何从表中选择的前五项。 查询返回的**ToDoItems**表中的项。 **mToDoTable**是对先前创建的后端表的引用︰

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


这里是一个查询，将跳过的第一次 5 个项目，然后将返回以下五个︰

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>如何︰ 选择特定列

下面的代码演示如何返回的**ToDoItems**，表中的所有项目，但仅显示**完成**和**文本**字段。 **mToDoTable**是我们先前创建的后端表的引用。

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

选择函数的参数是要返回的表的列的字符串名称。

**选择**方法需要按照**位置**和**排序**等方法。 它可以跟像**顶部**的分页方法。

### <a name="chaining"></a>如何︰ 将查询方法

可以连接后端表的查询中使用的方法。 链接查询方法允许您选择特定列的筛选行进行排序和分页。 您可以创建复杂的逻辑筛选器。
每个查询方法中返回查询对象。 若要结束的一系列方法，并实际运行查询，**执行**方法调用 例如︰

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

必须按以下顺序链接的查询方法︰

1. （**其中**） 筛选方法。
2. 排序 (**排序依据**) 方法。
3. （**选择**） 的选择方法。
4. 分页 （**跳过**和**顶部**） 方法。

##<a name="inserting"></a>如何︰ 将数据插入到后端

实例化*ToDoItem*类的一个实例并设置其属性。

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

然后使用**insert()**插入的对象︰

    ToDoItem entity = mToDoTable.insert(item).get();

返回的实体匹配数据插入到后端表，包含 ID，并在后端上设置的任何值。

移动应用程序表需要名为**id**的主键列。 默认情况下，此列是一个字符串。 ID 列的默认值是一个 GUID。  您可以提供其他唯一值，例如电子邮件地址或用户名。 当插入记录未提供一个字符串的 ID 值时后, 端生成新的 GUID。

ID 值的字符串具有以下优点︰

+ 而对数据库进行一次往返行程，可以生成 Id。
+ 记录是容易合并不同的表或数据库中。
+ ID 值更好地集成应用程序的逻辑。

字符串 ID 值为脱机同步支持是**必需的**。

##<a name="updating"></a>如何︰ 更新移动应用程序中的数据

要更新表中的数据，请将新对象传递给**update （）**方法。

    mToDoTable.update(item).get();

在此示例中，*项*是在*ToDoItem*表中，已对其进行一些更改的行的引用。
具有相同**id**的行被更新。

##<a name="deleting"></a>如何︰ 删除移动应用程序中的数据

下面的代码演示如何通过指定的数据对象从表格中删除数据。

    mToDoTable.delete(item);

您可以通过指定要删除的行的**id**字段中删除项目。

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>如何︰ 查找特定的项

使用**lookUp()**方法来查找具有特定**id**字段的项︰

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>如何︰ 使用非类型化数据

非类型化编程模型可以精确控制 JSON 序列化。  有一些常见的情况，您可能希望使用非类型化的编程模型。 例如，如果后端表包含很多列，并且您只需要引用列的子集。  类型化的模型要求您在您的数据类中定义移动应用程序表的所有列。  

大多数数据访问 API 调用都与类型化编程调用类似。 主要的区别是，在非类型化的模型中调用**MobileServiceJsonTable**对象，而不是**MobileServiceTable**对象上的方法。

### <a name="json_instance"></a>如何︰ 创建非类型化表的实例

您开始通过获取表引用，类似于类型化的模型，但在本例中是**MobileServicesJsonTable**对象。 通过调用获取引用**可获取**客户端的实例上的方法︰

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

一旦您已创建的**MobileServiceJsonTable**实例，具有几乎相同的 API 可用作与类型化编程模型。 在某些情况下，方法采用了一个非类型化的参数，而不是类型化的参数。

### <a name="json_insert"></a>如何︰ 插入到非类型化的表

下面的代码演示如何执行插入。 第一步是创建[JsonObject][1]， [gson]的[3]库。

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

然后，使用**insert()**表中插入非类型化的对象。

    mJsonToDoTable.insert(jsonItem).get();

如果您需要获取插入的对象的 ID，使用**getAsJsonPrimitive()**方法。

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>如何︰ 从类型化的表中删除

下面的代码演示如何删除一个实例，在这种情况下，*插入*前一示例中创建的**JsonObject**的同一个实例。 代码是类型化的情况下，与相同但由于引用**JsonObject**，该方法具有不同的签名。

         mToDoTable.delete(jsonItem);

您还可以直接通过其 ID 删除实例︰

         mToDoTable.delete(ID);

### <a name="json_get"></a>如何︰ 从类型化的表返回的所有行

下面的代码演示如何检索整个表。 使用 JSON 表，因为您可以有选择地检索只有某些表的列。

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

一组相同的筛选，筛选和分页方法所提供的类型化的模型都可用于非类型化的模型。

##<a name="custom-api"></a>如何︰ 调用一个自定义的 API

自定义的 API 使您能够定义自定义终结点公开服务器功能的不映射到插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括读取和设置 HTTP 消息头定义非 JSON 消息正文格式。

从 Android 的客户端，您可以调用**invokeApi**方法调用的自定义的 API 端点。 下面的示例演示如何调用一个名为**completeAll**，它返回一个名为**MarkAllResult**的集合类的 API 端点。

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

在客户端，将 POST 请求发送到新的自定义 API 调用**invokeApi**方法。 由自定义的 API 返回的结果将显示在消息对话框中，如任何错误。 其他版本的**invokeApi**使您可选择在请求正文中发送对象、 指定的 HTTP 方法，和发送请求的查询参数。 它还提供了**invokeApi**的非类型化的版本。

##<a name="authentication"></a>如何︰ 添加到您的应用程序的身份验证

教程已经详细说明如何添加这些功能。

应用程序服务支持使用各种外部身份提供程序的[应用程序的用户身份验证](app-service-mobile-android-get-started-users.md)︰ Facebook、 Google、 Microsoft 帐户、 Twitter 和 Azure 活动目录。 可以对表来限制对特定操作只有已通过身份验证的用户设置权限。 您还可以使用经过身份验证的用户的身份在您的后端实现授权规则。

支持两种身份验证流程︰**服务器**和**客户端**流。 服务器流提供最简单的身份验证体验，因为它依赖于标识提供程序 web 界面。  实现服务器流量身份验证所不需任何其他 Sdk。 服务器流量身份验证不会提供深度集成到移动设备，并且建议仅证明概念方案。

因为它依赖于标识提供程序提供的 Sdk 客户端流允许使用特定于设备的功能，如单一登录更深度的集成。  例如，您可以将 Facebook SDK 集成到移动应用程序。  移动客户端交换到 Facebook 的应用程序，并确认您登录之前换回您的移动应用程序。

要启用身份验证，在您的应用程序需要四个步骤︰

- 标识提供程序注册您的应用程序进行身份验证。
- 配置您的应用程序服务的后端。
- 限制为仅对后端应用程序服务的身份验证的用户的表权限。
- 将验证代码添加到您的应用程序。

可以对表来限制对特定操作只有已通过身份验证的用户设置权限。 您可以使用经过身份验证的用户的 SID 来修改请求。  详细信息，请参阅[开始使用身份验证]和 SDK HOWTO 服务器文档。

### <a name="caching"></a>如何︰ 将验证代码添加到您的应用程序

下面的代码将启动服务器流登录进程使用 Google 提供程序︰

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

从**MobileServiceUser**使用**getUserId**方法获得已登录的用户的 ID。 有关如何使用将来调用异步登录 Api 的示例，请参阅[开始使用身份验证]。

### <a name="caching"></a>如何︰ 缓存身份验证令牌

缓存的身份验证令牌要求您设备上的本地存储的用户标识和身份验证令牌。 下次启动该应用程序，检查缓存中，并且是否存在这些值，您可以跳过过程中的日志，rehydrate 包含此数据的客户机。 但是这些数据是敏感的并应存储在电话被盗的情况下，为了安全加密。

您可以看到如何向缓存身份验证令牌[缓存身份验证令牌]中的完整示例[7]。

当您尝试使用已过期的令牌时，您将收到的*401 未经授权*响应。 您可以处理使用筛选器的身份验证错误。  筛选器截获到后端应用程序服务请求。 筛选器代码测试为 401 响应，登录过程中，会触发，然后恢复生成 401 的请求。 

## <a name="adal"></a>如何︰ 使用活动目录身份验证库的用户进行身份验证

活动目录身份验证库 (ADAL) 可用于登录到应用程序使用 Azure 活动目录的用户。 使用客户端流登录名通常要优于使用`loginAsync()`作为它的方法提供了更多本机用户体验感觉和允许进行更多自定义。

1. [如何配置应用程序服务 Active Directory 登录](app-service-mobile-how-to-configure-active-directory-authentication.md)教程配置您移动应用程序端的 AAD 签到。 请务必完成注册本机客户端应用程序的可选步骤。

2. 安装 ADAL 通过修改 build.gradle 文件，包括了以下定义︰

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. 将下面的代码添加到应用程序中，进行以下替换︰

* 组织资源调配您的应用程序的名称替换**插入的主管机构-这里**。 格式应为 https://login.windows.net/contoso.onmicrosoft.com。 此值可以从 [Azure 经典门户中] 将 Azure Active Directory 中的域选项卡。

* **插入资源 ID 这里**替换为您的移动应用程序后端的客户端 ID。 从门户下**Azure 活动目录设置**的**高级**选项卡，您可以获得的客户端 ID。

* 从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。

* 替换为您的网站_/.auth/login/done_的终结点，使用 HTTPS 方案**插入重定向 URI 这里**。 此值应与_https://contoso.azurewebsites.net/.auth/login/done_类似。

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>如何︰ 将推式通知添加到您的应用程序

您可以[阅读有关][ 6] ，它描述如何 Microsoft Azure 通知集线器支持各种各样的推式通知。  在[本教程中][5]，每次插入记录的所有设备发送推式通知。

## <a name="how-to-add-offline-sync-to-your-app"></a>如何︰ 添加到您的应用程序的脱机同步

快速入门教程包含实现脱机同步的代码。 对于前面带注释的代码如下︰

    // Offline Sync

通过取消注释下面的代码行，您可以实现脱机同步，并且您可以添加其他移动应用程序的代码类似的代码。

##<a name="customizing"></a>如何︰ 自定义客户端

有几种方法供您自定义客户端的默认行为。

### <a name="headers"></a>如何︰ 自定义请求标头

配置**ServiceFilter**要为每个请求中添加自定义的 HTTP 标头︰

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>如何︰ 自定义序列化

客户机假定表名称、 列名称和数据类型后端所有匹配完全在客户端定义的数据对象。 可以有任意数量的原因为什么可能不匹配的服务器和客户端的名称。 在方案中，您可能想要执行以下类型的自定义项︰

- 应用程序服务表中的列名称与您在客户端中使用的名称不匹配。
- 使用具有不同的名称比类映射到客户端应用程序服务表。
- 打开自动属性大小写。
- 将复杂的属性添加到对象中。

### <a name="columns"></a>如何︰ 将不同的客户端和服务器名称映射

假设您的 Java 客户端代码使用标准 Java 样式名称为**ToDoItem**的对象属性，如下面的属性︰

- 中期
- mText
- mComplete
- mDuration

客户端名称序列化为 JSON 名称匹配的服务器上的**ToDoItem**表的列的名称。 下面的代码使用[gson] [3]库进行批注属性︰

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>如何︰ 映射客户端和后端之间的不同的表名

使用[getTable()]的重写客户端表名称映射到不同的移动服务表名称[4]方法︰

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>如何︰ 自动化列名称映射

您可以指定适用于每个列使用[gson]的转换策略[3]API。 Android 的客户端库使用[gson] [3]在后台进行序列化 Java 对象到 JSON 数据之前数据发送到 Azure 应用程序服务。  下面的代码使用**setFieldNamingStrategy()**方法来制定战略。 本示例将删除初始字符 ("m")，然后使用小写的下一个字符，对于每个字段名称。 例如，它会变成"mId""id"。

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

在使用**MobileServiceClient**之前，必须执行此代码。

### <a name="complex"></a>如何︰ 将对象或数组的属性存储到表中

到目前为止，我们的示例中序列化已涉及基元类型，如整数和字符串。  基元类型轻松序列化为 JSON。  如果我们想要添加没有自动序列化的复杂对象到 JSON，我们需要提供的 JSON 序列化方法。  若要查看如何提供自定义的 JSON 序列化的示例，请查看博客文章[使用 Android 手机服务客户端中的 gson 库的自定义序列化][2]。

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[返回所有项]: #showAll
[筛选返回的数据]: #filtering
[返回的数据的排序]: #sorting
[返回在页中的数据]: #paging
[选择特定列]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure 门户]: https://portal.azure.com
[开始使用身份验证]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[未来]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html