> [Wiki](Home) ▸ Contributors ▸ [Test Plan](./Contributors-:-Test-Plan)

- see also [`acme/library` with 1 service](./Contributors-:-Test-Plan-:-Section-Templates)
- create HTTP service AND database service
    - database service
        - db
            - use MySQL on RDS (`jdbc:mysql://mysql.c4pgtxbv1cuq.us-east-1.rds.amazonaws.com:3306/orbeon?useUnicode=true&amp;characterEncoding=UTF8`)
            - set datasource in `server.xml`
            - create test table + data row if doesn't exist (can use IntelliJ Database tools)
        - start with sample form and scenario from [#1230][2]
        - sets service values on request
        - sets control values on response
        - set itemset values on response 
            - `/*/*`
            - `concat(first, ' ', last)`
            - `id`
    - HTTP service
        - using echo service is ok
            - POST to /fr/service/custom/orbeon/echo
        - test
            - call service upon form load and set control value upon response
            - same with button activation
            - same but set service values on request from control
            - set itemset values on response, e.g. use:

            ```xml
            <items>
                <item label="Foo" value="foo"/>
                <item label="Bar" value="bar"/>
            </items>
            ```

[2]: https://github.com/orbeon/orbeon-forms/issues/1230