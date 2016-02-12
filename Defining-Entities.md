Requery works by using abstract classes or interfaces to define the structure of an entity. These classes should be marked with the @Entity annotation. Classes marked with this annotation will have a corresponding Entity class generated for them by the requery annotation processor. This generated class will inherit from the original class, and be used as the proxy class to store the state of the object. This is how requery avoids reflection.

Example:

```java
@Entity
abstract class AbstractPerson {

    @Key @Generated
    int id;

    String name;

    @OneToMany  
    Set<Phone> phoneNumbers;

    String email;
}
```

This will generate a Person.java file in the same package as the source class with the required entity attributes and state.

Alternatively you can also define the entity as an interface:

```java
@Entity
public interface Person {

    @Key @Generated
    int getId();

    String getName();
    String getEmail();
    Date getBirthday();
    @OneToMany
    Result<Phone> getPhoneNumbers();

}
```

This will generate a file called PersonEntity.java which contains the required attributes and implements the Person interface.

The name of the generated file can be customized by specifying the name() property on the Entity annotation. Whether to use abstract classes or interfaces is up to the user, requery is not opinionated about how those classes are defined. Use whatever best fits your use case.