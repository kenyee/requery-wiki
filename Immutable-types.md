You can combine requery annotations (and even some JPA annotations) for use with immutable types. However there are some limitations to consider when using immutable types:

* Immutable types cannot contain relational links (however you can have foreign keys)
* Immutable types cannot have lazy loading or change tracking
* Immutable types must have a static create method or be buildable via a builder class

Example using Google [AutoValue](https://github.com/google/auto) defining a mapping:

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