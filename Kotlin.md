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

###Overview
The kotlin module provides an alternate query and storage interface that takes advantage of Kotlin specific language features. These include property references, infix functions, operator functions and more.

To use these features use `KotlinEntityDataStore` instead of `EntityDataStore`. You can create an instance of  `KotlinEntityDataStore` using an KotlinConfiguration (or any configuration) instance.

###Entity definition

In Kotlin entity definitions can be created from interface classes with properties or from immutable data classes.

Example: (note the get prefix on annotations, note the `Persistable` is required to use property references)
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

    @get:ManyToMany(mappedBy = "members")
    val groups: Set<Group>

    var about: String

    @get:Column(unique = true)
    var uuid: UUID
    var homepage: URL
}
```

###Queries:
```kotlin
val configuration = KotlinConfiguration(dataSource = dataSource, model = Models.DEFAULT)
instance = KotlinEntityDataStore(configuration)
data.invoke {
    val result = data.select(Person::id, Person::name) limit 1
    val first = result().first()
}
```
