# JPA Relationship Lab 3

## Instruction

You are given the following entity relationship model:

![Owner cat many to many relationship diagram](https://curriculum-content.s3.amazonaws.com/6036/java-mod-5-jpa-lab/jpa_lab3_erd.png)

There is a many-to-many relationship between `Owner` and `Cat`.
An owner has many cats and each cat may have many owners. 

The lab repository has the required dependencies defined in the `pom.xml` file
and the database configuration is defined in the `persistence.xml` file.

Open the `Owner` class.  Notice it has a field for a list of cats.

```java
private List<Cat> cats = new ArrayList<>();
```

Open the `Cat` class.  It has a field for a list of owners.

```java
private List<Owner> owners = new ArrayList<>();
```

The `Owner` and `Cat` classes are POJOs (plain old Java objects).
Neither class contains JPA annotations (@Entity, @Id, etc).

NOTE: Since `Owner` and `Cat` both contain a list of instances of the other class, it is important
to avoid an infinite cycle of method calls if you have IntelliJ generate the `toString()` method.
The default `toString()` generated by IntelliJ would include a call to the `toString()` method
for each object in the list, which would in turn call the `toString()` method for each
object in the other list, etc.  We avoid this infinite cycle of method calls
by removing the lists from the `toString()` methods generated by IntelliJ.

1. You will use the database named `jpalab_db` that you created from the previous lab.  
   Check `persistence.xml` to make sure the `hibernate.hbm2ddl.auto` property is set to `create`.
2. Edit the `Owner` class:
   - Add the `@Entity` annotation to the class.
   - Add the `@Id` and `@GeneratedValue` annotations to the `id` field to make it an auto-incremented primary key.
   - Add the  `@ManytoMany` annotation to the `cats` field.
3. Edit the `Cat` class:
    - Add the `@Entity` annotation to the class.
    - Add the `@Id` and `@GeneratedValue` annotations to the `id` field to make it an auto-incremented primary key.
    - Add the  `@ManyToMany` annotation to the `owners` field.  Set the `mappedBy` property to `"cats"`.
   
4. Run the `JpaCreate.main` method.  The code should create tables `OWNER`, `CAT`, and the join table `OWNER_CAT`.
5. Use the **pgAdmin** query tool to query the tables.

`SELECT * FROM OWNER;`

| ID  | NAME |
|-----|------|
| 1   | Jia  |
| 2   | Yuri |
| 3   | Luka |

`SELECT * FROM CAT;`

| ID  | AGE | BREED              | NAME            |
|-----|-----|--------------------|-----------------|
| 4   | 5   | Tabby              | Meowie          |
| 5   | 1   | Birman             | Whiskers        |
| 6   | 10  | American Shorthair | Freddy Purrcury |


`SELECT * FROM OWNER_CAT;`

| OWNERS_ID | CATS_ID |
|-----------|---------|


The `OWNER_CAT` join table is empty because `JpaCreate` has not set up any associations.

6. Edit `JpaCreate` to add associations between owners and cats.  
   The first two owners own the first two cats, while the third owner owns the third cat.
   Establish the associations by calling the `addCat` method on the owner.
7. Run the `JpaCreate.main` method to create and populate all three tables.
8. Use the **pgAdmin** query tool to query the tables.

`SELECT * FROM OWNER;`

| ID  | NAME |
|-----|------|
| 1   | Jia  |
| 2   | Yuri |
| 3   | Luka |

`SELECT * FROM CAT;`

| ID  | AGE | BREED              | NAME            |
|-----|-----|--------------------|-----------------|
| 4   | 5   | Tabby              | Meowie          |
| 5   | 1   | Birman             | Whiskers        |
| 6   | 10  | American Shorthair | Freddy Purrcury |


`SELECT * FROM OWNER_CAT;`

| OWNERS_ID | CATS_ID |
|-----------|---------|
| 1         | 4       | 
| 1         | 5       |
| 2         | 4       |
| 2         | 5       |
| 3         | 6       |

9. Edit `persistence.xml` to set `hibernate.hbm2ddl.auto` property is set to `none`.
10. Edit `JpaRead` to find and print the data for owner with id 1, along with their cats. 
    - Replace `null` on the line `Owner owner = null` to call the entity manager's `find` method to get the owner with id 1.
    - Replace `null` on the line `List<Cat> cats = null;` to call the `getCats` method to get the owner's cats.
11. Run `JpaRead` to find and print the owner data. 
12. Look between the Hibernate output to confirm the output from the two print statements:

```text
...
Owner{id=1, name='Jia'}
...
[Cat{id=4, name='Meowie', age=5, breed='Tabby'}, Cat{id=5, name='Whiskers', age=1, breed='Birman'}]
```

