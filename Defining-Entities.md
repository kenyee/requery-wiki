### Entities

requery works by using abstract classes or interfaces to define the structure of an entity. These classes should be marked with the `@Entity` annotation. Classes marked with this annotation will have a corresponding Entity class generated for them by the requery annotation processor. This generated class will inherit from the original class, and be used as the proxy class to store the state of the object. This is how requery avoids reflection.

Example:

```java
@Entity
abstract class AbstractPerson {

    @Key @Generated
    int id;

    String name;
    String email;
    Date birthday;

    @OneToMany  
    Set<Phone> phoneNumbers;
}
```

This will generate a `Person.java` file in the same package as the source class with the required entity attributes and state.

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

This will generate a file called `PersonEntity.java` which contains the required attributes and implements the Person interface.

The name of the generated file can be customized by specifying the `name()` property on the `Entity` annotation. Whether to use abstract classes or interfaces is up to the user, requery is not opinionated about how those classes are defined. Use whatever best fits your use case.

### Models

A [EntityModel](http://requery.github.io/javadoc/io/requery/meta/EntityModel.html) is a grouping of `Entity` types belonging to a specific database or schema. A file called Models.java is automatically generated that contains model definitions based on the entity classes you have defined. Use this model to create storage objects for working the model through a database. As described [here](https://github.com/requery/requery/wiki/Using-the-EntityStore-interface).

### Generated class names

This table illustrates the default class names when using [@Entity](http://requery.github.io/javadoc/io/requery/Entity.html) on your domain classes. You can always specify the desired class name in the annotation. However below are the defaults for hypothetical 'User' entity when this isn't specified.

your class name       |  class type |  generated name
----------------------|-------------|----------------
AbstractUser          |  class      |  User    
BaseUser              |  class      |  User
User                  |  interface  |  UserEntity      
IUser                 |  interface  |  User   