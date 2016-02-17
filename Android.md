##Android

Android support is at the core of this project. requery is the most feature complete ORM available for Android that is also performant. The requery-android project provides classes specific to using Android's SQLite database within the requery core library as well as useful UI adapters/classes. requery works on Android API level 15 and later.

###DatabaseSource

`DatabaseSource` is the Android specific connection source for use with [EntityDataStore](http://requery.github.io/javadoc/io/requery/sql/EntityDataStore.html) which is the core entity management API of requery. `DatabaseSource` extends [SQLiteOpenHelper](http://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) and using the requery API automatically creates tables from the EntityModel provided. It also provides basic upgrade support by adding missing columns and tables on a upgrade. Every time you change the model be sure to increment the schemaVersion passed to `DatabaseSource`. If you need to perform more complex migrations just override onUpgrade and perform the necessary migration steps.

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

###ParcelConverter

ParcelConverter is a builtin [Converter](http://requery.github.io/javadoc/io/requery/Converter.html) that allows you to easily store Android's Parcelable objects into a SQLite BLOB column. Here's an example of storing an Android Parcelable Location object in an Entity:

First define the converter:

```java
 public class LocationConverter<Location> extends ParcelConverter<Location> {
     public LocationConverter() {
         super(Location.class, Location.CREATOR);
     }
 }
```
Then apply to it your entity:
```java
@Entity
public interface GeoInfo {
    @Key int getId();
    @Converter(LocationConverter.class)
    Location getLocation(); // stored as a SQLite BLOB
}
```

###Example project

See the [requery-android/example](https://github.com/requery/requery/tree/master/requery-android/example) project for a working example.