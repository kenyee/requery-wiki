## Converters

[Converters](http://requery.github.io/javadoc/io/requery/Converter.html) are a powerful way to map special custom types into persisted equivalents. requery provides a number of [builtin](http://requery.github.io/javadoc/io/requery/converter/package-frame.html) converters.

Defining a converter is simple, here we'll look at the implementation of the built in URIConverter:

```java
public class URIConverter implements Converter<URI, String> {

    @Override
    public Class<URI> mappedType() {
        return URI.class;
    }

    @Override
    public Class<String> persistedType() {
        return String.class;
    }
...
```

First extend the Converter interface. The Converter interface has two generic arguments. The first is the type to be converted (URI) and second is the converted persisted form (String). Override mappedType and persistedType to provide the class references explicitly since these are erased by java at compile time. 

Next provide the conversion methods:

```
    @Override
    public String convertToPersisted(URI value) {
        return value == null ? null : value.toString();
    }

    @Override
    public URI convertToMapped(Class<? extends URI> type, String value) {
        return value == null ? null : URI.create(value);
    }
```

That's it. Then you can use the converter class on an entity on the attribute level:

```java
interface Person {
...
    @Convert(URIConverter.class)
    URI getHomePage();
...
}
```

or you can also override the [Mapping](http://requery.github.io/javadoc/io/requery/sql/Mapping.html) interface and provide this converter as a default for all entities.