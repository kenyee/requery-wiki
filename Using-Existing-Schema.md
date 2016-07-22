Using Existing Schema
=====================

You frequently have an existing project with an existing database schema that you want to add an ORM to.  Requery normally will generate tables and column names for you depending on how define your entity classes, but you can explicitly name them to fit your existing schema.

### Naming Attributes
The @Table and @Column annotations can also be specified with a name attribute and Requery will use this for naming the tables.  E.g., here's how you'd name a table "Directions":

```java
@Table(name="contacts")
```

### Many-To-Many Tables

In an SQL database, you need a many to many table to map one set of objects to another.  In order to tell Requery about this, don't set up an @Entity for the mapping table.  Instead, each entity field that references the other should have a @ManyToMany annotation.  However, only one of these entities mapping fields should have a @JunctionTable annotation.  This field would look like this:

```java
    @JunctionTable(name="ContactAddress", columns = {
            @Column(name = "contactId", foreignKey =
            @ForeignKey(references = Contact.class, referencedColumn = "cid")),
            @Column(name = "addressId", foreignKey =
            @ForeignKey(references = Address.class, referencedColumn = "aid")) })
    @ManyToMany
    Set<Address> getAddresses();
```

The example above would be used if your mapping table is named "ContactAddress" and has two columns named "contactId" and "addressId".  These columns are foreign keys for the respective tables where the columns are also named "cid" and "aId", respectively.  It's important that you don't reference the Requery generated *Entity.classes as well.

### View vs. Detail Entities
You commonly would want to minimize the number of fields pulled into your entity objects when displaying data in a view.  Display of the entity in a detail view would included more fields.  You can do this by declaring multiple entities that reference the same table but the entities will have different names (e.g., ContactView and Contact).

### Migration

Note that if you add new columns to these entities, you'll have to handle the migration manually because you're referencing fields directly.