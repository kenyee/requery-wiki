One of more powerful features is being able to generate SQL tables from your code. This can be easily done in code through the [SchemaModifier](http://requery.github.io/javadoc/io/requery/sql/SchemaModifier.html) class.

Example:

```java
EntityModel model = Models.DEFAULT;
dataSource = ...; // create a Datasource for your JDBC driver
new SchemaModifier(dataSource, model).createTables(TableCreationMode.DROP_CREATE);
```

There are various TableCreationMode options:
* CREATE - create the tables, the tables must not already exist
* DROP_CREATE - drop all tables and create them newly (ideal for testing)
* CREATE_NOT_EXISTS - create tables if they don't exist (requires IF EXISTS support)
