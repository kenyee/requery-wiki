##Android

Android support is at the core of this project. The requery-android project provides classes specific to using Android's SQLite database within the requery core library as well as useful UI adapters/classes for working with database entities in an Android application. requery works on Android API level 15 and later.

###Databinding

You can also take advantage of the new databinding library from Google in your entities. Simply extend [Observable](http://developer.android.com/reference/android/databinding/Observable.html) and provide [@Bindable](http://developer.android.com/reference/android/databinding/Bindable.html) on bindable properties.

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

See the [requery-android/example](https://github.com/requery/requery/tree/master/requery-android/example) project for more information and a working example.