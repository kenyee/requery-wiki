###Overview
The kotlin module provides an alternate query and storage interface that takes advantage of Kotlin specific language features. These include property references, infix functions, operator functions and more.

To use these features use `KotlinEntityDataStore` instead of `EntityDataStore`. 

```kotlin
val configuration = KotlinConfiguration(dataSource = dataSource, model = Models.DEFAULT)
val data = KotlinEntityDataStore(configuration)
```

###Query:
```kotlin
data.invoke {
    val result = select(Person::class) where (Person::name eq "Bob") limit 5
    val first = result().first()
}
```

###Entities:

In Kotlin entity definitions can be created from interface classes with properties or from immutable data classes.
Example: (note the get prefix on annotations, note the `Persistable` marker interface is required to use property references in queries)
```kotlin
@Entity
interface Person : Persistable {

    @get:Key
    @get:Generated
    var id: Int
    var name: String
    var email: String
    var birthday: Date
    var age: Int

    @get:ForeignKey
    @get:OneToOne
    var address: Address

    @get:ManyToMany
    val groups: Set<Group>

    var about: String

    @get:Column(unique = true)
    var uuid: UUID
    var homepage: URL
}
```

###Setup

1. Add the apt gradle plug-in to your build as described [here](https://github.com/requery/requery/wiki/Gradle-&-Annotation-processing)
2. Add the requery dependencies to your app build.gradle file:

```gradle
dependencies {
    compile 'io.requery:requery:<latestVersion>'
    compile 'io.requery:requery-kotlin:<latestVersion>'
    compile 'io.requery:requery-android:<latestVersion>' // optional for android support
    apt 'io.requery:requery-processor:<latestVersion>' 
}
```
