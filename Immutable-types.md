You can combine requery annotations (and even JPA annotations) for use with immutable types. However there are some limitations to consider when using immutable types:

* Immutable types cannot contain relational links (however you can have foreign keys)
* Immutable types cannot have lazy loading or change tracking
* Immutable types must have a static create method or be buildable via a builder class

Example using Google [AutoValue](https://github.com/google/auto) defining a mapping:

```java
@AutoValue
@Entity
public abstract class Person {

    @AutoValue.Builder
    static abstract class Builder {
        abstract Builder setId(int id);
        abstract Builder setName(String name);
        abstract Builder setBirthday(Date date);
        abstract Builder setAge(int age);
        abstract Person build();
    }

    static Builder builder() {
        return new AutoValue_Person.Builder().setId(-1);
    }

    @Id @GeneratedValue
    public abstract int getId();

    public abstract String getName();
    public abstract Date getBirthday();
    public abstract int getAge();
}
```

The processor will generate a `PersonType` class contain the attributes of `Person` which is used to construct and create instances of `Person` by the library when needed. 