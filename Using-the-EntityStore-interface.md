The `EntityStore` interface provides the basic API for insert/update/delete operations as well querying using the builtin DSL for SQL.

##EntityDataStore

`EntityDataStore` provides a concrete implementation of `EntityStore` for use with SQL databases. This provides a fundamental blocking API for all basic operations.

Example of instantiating the EntityDataStore:

```java
DataSource dataSource = ...
EntityModel model = Models.DEFAULT;
Configuration configuration = new ConfigurationBuilder(dataSource, model)
            .useDefaultLogging()
            .setEntityCache(new EntityCacheBuilder(model)
                .useReferenceCache(true)
                .useSerializableCache(true)
                .useCacheManager(cacheManager)
                .build())
            .build();
        data = new EntityDataStore<>(configuration);
```

`EntityDataStore` takes a `Configuration` as a parameter. The best way to create a `Configuration` is through `ConfigurationBuilder`. `ConfigurationBuilder` requires at a minimum a `DataSource` (access to a JDBC connection) and a `EntityModel`. The EntityModel definition is generated automatically for you in the same package as your entities in a file called Models.java.

#Async wrappers

In addition to `EntityDataStore` there are two supported non-blocking API's for basic operations.

##RxJava

`SingleEntityStore` represents all basic operations as Rx Single classes. A single is like an observable but only returns a single result.

Creating a SingleEntityStore:

```java
SingleEntityStore dataStore = RxSupport.toReactiveStore(new EntityDataStore<Persistable>(configuration));
```

##Java 8

For Java 8 a `CompletableEntityStore` class is provided which returns all basic operations as Java 8 `CompletionStage` objects.

Creating a CompletionStageEntityStore:

```java
CompletableEntityStore dataStore = new CompletableEntityStore<>(new EntityDataStore<Persistable>(configuration));
```