Only a subset of the JPA annotations are supported. These are the annotations that are supported:

- Basic
- Cacheable
- Column
- Embeddable
- Embedded
- Entity
- Enumerated
- GeneratedValue
- Id
- JoinColumn
- JoinTable
- ManyToMany
- ManyToOne
- MappedSuperclass
- OneToMany
- OneToOne
- PostLoad
- PostPersist
- PostRemove
- PostUpdate
- PrePersist
- PreRemove
- PreUpdate
- Table
- Transient
- Version

There is no support for embedded types. Unique/index constraints must be placed on the field/method level. Advanced JPA features are not yet supported such as embedded types, mapping via @JoinTable to secondary tables to define relationships (outside of ManyToMany).