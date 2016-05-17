Android support is at the core of this project. requery is the most feature complete ORM available for Android that is also performant. The requery-android project provides classes specific to using Android's SQLite database within the requery core library as well as useful UI adapters/classes. requery works on Android API level 15 and later.

###Quickstart

Requirements:
 - Android Studio 1.5 or later
 - Java JDK 8

1. Add the apt gradle plug-in to your build as described [here](https://github.com/requery/requery/wiki/Gradle-&-Annotation-processing)
2. Add the requery dependencies to your app build.gradle file:

```gradle
dependencies {
    compile 'io.requery:requery:<latestVersion>'
    compile 'io.requery:requery-android:<latestVersion>' // for android
    apt 'io.requery:requery-processor:<latestVersion>'   // use an APT plugin
}
```
Replace `<latestVersion>` with latest released version.

###DatabaseSource

`DatabaseSource` is the Android specific connection source for use with [EntityDataStore](http://requery.github.io/javadoc/io/requery/sql/EntityDataStore.html) which is the core entity management API of requery. `DatabaseSource` extends [SQLiteOpenHelper](http://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) and using the requery API automatically creates tables from the EntityModel provided. It also provides basic upgrade support by adding missing columns and tables on a upgrade. Every time you change the model be sure to increment the schemaVersion passed to `DatabaseSource`. If you need to perform more complex migrations just override onUpgrade and perform the necessary migration steps.

###[SQLCipher](https://github.com/sqlcipher/android-database-sqlcipher)

SQLCipher is a popular extension to SQLite that encrypts the entire database. You can use requery with SQLCipher by using `io.requery.android.sqlcipher.SqlCipherDatabaseSource` instead of `DatabaseSource` when creating the store and providing a database password. Also be sure to include the SQLCipher dependency in your gradle file
```gradle
compile 'net.zetetic:android-database-sqlcipher:<version>'
```

###SQLite support library

See the [project page](https://github.com/requery/sqlite-android) for more information. This is an embedded version of SQLite you can include with your application instead of the default system version. To use it include the SQLite library dependency and use `io.requery.android.sqlitex.SqlitexDatabaseSource` instead of `DatabaseSource` when creating the database store.

###Databinding

You can take advantage of the new databinding library from Google in your entities. Simply extend [Observable](http://developer.android.com/reference/android/databinding/Observable.html) and provide [@Bindable](http://developer.android.com/reference/android/databinding/Bindable.html) on bindable properties.

```java
@Entity
@BindingResource(Binding.BR_CLASS) // requery annotation
public interface Person extends Observable, Parcelable, Persistable {

    @Key @Generated
    int getId();

    @Bindable // android databinding annotation
    String getName();
...
```

Note here the @BindingResource is used to provide the class name of the generated BR class. The BR class is needed to generate change notifications. Unfortunately this class is not automatically discoverable by the library so it must be specified.

With binding enabled you can automatically update your UI when the underlying database entity is changed or updated.

###Adapters

A typical use of a query [Result](http://requery.github.io/javadoc/io/requery/query/Result.html) instance is to use it's items to populate a list. For this `QueryRecyclerAdapter` can be used to populate a RecyclerView with query result data.

###Async operations

For async operations we recommend using [RxJava](https://github.com/ReactiveX/RxJava). requery provides a complete API that encapsulates all common database operations (insert/update/delete/refresh) using the Rx [Single](http://reactivex.io/documentation/single.html) API. See [here](https://github.com/requery/requery/wiki/Using-the-EntityStore-interface#rxjava) for more information.

###Example project

See the [requery-android/example](https://github.com/requery/requery/tree/master/requery-android/example) project for a working example.