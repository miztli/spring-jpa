# Spring-JPA

Spring Hibernate/JPA samples project

### Batching inserts/updates

##### Enable hibernate batching

```properties
spring.jpa.properties.hibernate.jdbc.batch_size=100
```

##### Batch inserts for single table

- _IMPORTANT:_ When we persist an entity, Hibernate stores it in the persistence context (Aka: first-level cache). E.g. If we store 1000 entities in one transaction, then well have 1000 instances on memory, which can be a potential cause of OutOfMemoryException.
- Use `EntityManager.flush()` triggers a transaction synchronization. This makes hibernate send queries to the DB. It naturally happens at the end of a transaction if this method is not called.
- Use `EntityManagaer.clear` to cleat the persistence context (remove from memory).

##### Batch inserts for multiple tables

- Hibernate will prepare one batch per entity, if several entities are saved during the transaction, then it will prepare different batches whenever entity changes.

Allow multiple entities batches config:
`spring.jpa.properties.hibernate.order_inserts=true`

- _IMPORTANT:_ Key generation strategy is relevant for enabling batch inserts. Hibernate will silently disable batch inserts for `GenerationType.IDENTITY` identifier generator strategy.

### Batch update

Group update statements together and send them in one go

```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.inserts=true
spring.jpa.properties.hibernate.order_updates=true
spring.jpa.properties.hibernate.batch_versioned_data=true
```

### Logging

##### To standard output (not recommended approach)

```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

##### Via loggers
```properties
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
logging.level.org.hibernate.stat=DEBUG
logging.level.org.hibernate.engine.internal=INFO
```

##### When using JDBCTemplate
```properties
logging.level.org.springframework.jdbc.core.JdbcTemplate=DEBUG
logging.level.org.springframework.jdbc.core.StatementCreatorUtils=TRACE
```


### Resources

- [jpa-hibernate-batch-insert-update](https://www.baeldung.com/jpa-hibernate-batch-insert-update)