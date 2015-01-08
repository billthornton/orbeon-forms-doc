> [[Home]] ▸ Contributors ▸ [[Test Plan|Contributors ~ Test Plan]]

## DB2 DDL

Do the following just with DB2; there is no need to test this with Oracle, MySQL, and SQL Server as this is done by the unit tests. Before each test, run the `drop table` statements below.

1. Run the [4.3 DDL] and [4.3 to 4.4 DDL].
2. Run the [4.4 DDL] and [4.4 to 4.6 DDL].
3. Run the [4.6 DDL].

```sql
drop table orbeon_form_definition ;
drop table orbeon_form_definition_attach ;
drop table orbeon_form_data ;
drop table orbeon_form_data_attach ;
```

## Oracle and DB2 Flat View

- Enable the flat view option, adding:

    ```xml
    <property 
        as="xs:boolean"
        name="oxf.fr.persistence.db2.create-flat-view" 
        value="true"/>
    ```
- Remove existing view if any: `drop view orbeon_f_db2_a ;`
- Create a new form from [this source](https://gist.github.com/avernet/ff343c6a5e6c3be077d2), which has the sections and controls named as in the table in the [[flat view documentation|Form-Runner-~-Persistence-~-Flat-View]]
  - rename app name to `db2`
  - publish, check that a view with the appropriate column names is created.

    ```sql
    SELECT * FROM orbeon_f_db2_a ;
    ```

  [4.3 DDL]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/persistence/relational/ddl/db2-4_3.sql
  [4.3 to 4.4 DDL]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/persistence/relational/ddl/db2-4_3-to-4_4.sql
  [4.4 DDL]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/persistence/relational/ddl/db2-4_4.sql
  [4.4 to 4.6 DDL]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/persistence/relational/ddl/db2-4_4-to-4_6.sql
  [4.6 DDL]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/persistence/relational/ddl/db2-4_6.sql