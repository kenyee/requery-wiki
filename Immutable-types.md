You can combine requery annotations (and even some JPA annotations) for use with immutable types. However there are some limitations to consider when using immutable types. 

Limitations to consider:

* Immutable types cannot contain relational links (however you can have foreign keys)
* Immutable types must be buildable via a builder class or static create method

Example using Google [AutoValue](https://github.com/google/auto)

Defining a person mapping:

```java
@AutoValue
@Entity
public abstract class Person implements Serializable {

    @AutoValue.Builder
    public static abstract class Builder {
        public abstract Builder setId(int id);
        public abstract Builder setName(String name);
        public abstract Builder setBirthday(Date date);
        public abstract Builder setAge(int age);
        public abstract Person build();
    }

    public static Builder builder() {
        return new AutoValue_Person.Builder().setId(-1);
    }

    @Id @GeneratedValue
    public abstract int getId();

    public abstract String getName();
    public abstract Date getBirthday();
    public abstract int getAge();
}
```

Inserting a person:

```java
data.insert(Person.class)
            .value(PersonEntity.NAME, name)
            .value(PersonEntity.AGE, random.nextInt(50))
            .value(PersonEntity.BIRTHDAY, calendar.getTime())
            .get().first().get(PersonEntity.ID)
```

Retrieving via query:

```java
Result<Person> result = data.select(Person.class).get();
```