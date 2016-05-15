### Query examples:

Note these examples are based on the following entities:

```java
@Entity
public class AbstractPerson implements Serializable {

    @Key @Generated
    int id;
    String name;
    String email;
    Date birthday;
    @Nullable
    int age;

    @ForeignKey
    @OneToOne
    Address address;

    @OneToMany
    MutableResult<Phone> phoneNumbers;
 }

@Entity
public class AbstractPhone implements Serializable {

    @Key @Generated
    int id;
    String phoneNumber;

    @ManyToOne
    Person owner;
}
```

This is a very small set of examples of some basic queries that could be performed on the above: Note that depending on the query type the following types of data can be returned 1) a row count result (integer) 2) A entity object e.g. Person 3) A `Tuple` which is a result row with values readable by index or key.

Counting the number of people entries in the table:

```java
int count = data.count(Person.class).get().value();
```

Counting people with a given name:

```java
int count = data.count(Person.class).where(Person.NAME.eq("Bob Jones")).get().value()
```

Finding a unique set of names for all people:

```java
data.select(Person.NAME).distinct().get()
```

Finding people over a certain age:

```java
data.select(Person.class).where(Person.AGE.gt(21)).get();
```

Finding people between a certain age:

```java
data.select(Person.class).where(Person.AGE.between(20, 40)).get();
```

Finding people whose name is not a given set of names:

```java
data.select(Person.class).where(Person.NAME.notIn(Arrays.asList("Bob Jones", "Alice Jones"))).get()
```

Finding people with several where clauses anded together:

```java
data.select(Person.class).where(Person.AGE.gt(20).and(Person.AGE.lt(30)).and(Person.NAME.like("Bob%"))).get();
```

Find phone numbers belonging to a particular person:

```java
data.select(Phone.class).where(Phone.OWNER.eq(person)).get()
```

###Result

[Result](http://requery.github.io/javadoc/io/requery/query/Result.html) is a powerful container class which you can use to consume the results of query. Below are some of the common operations you can use on this type:

-`iterator()` returns a java iterator of the result set

-`first()` retrieves the first element of the query, throwing an exception if none exists

-`firstOrNull()` retrieves the first element of the query, returning null if none exists

-`firstOr(E defaultValue)` retrieves the first element of the query, returning the given default value if none exist

-`toList()` retrieves all the elements of the result as a unmodifiable list

-`collect(C collection)` retrieves all the elements of the result into the given collection

-`toObservable()` converts the query into a RxJava Observable sequence

-`stream()` converts the query into a Java 8 stream