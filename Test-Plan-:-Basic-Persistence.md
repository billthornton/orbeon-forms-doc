> [Wiki](Home) ▸ Contributors ▸ [Test Plan](./Contributors-:-Test-Plan)

[[fjsjfksldf]]

We do the following just for eXist and DB2, as automated tests already test most of this, and the code running for DB2 is almost identical to the code running for other relational databases.

1. Setup: in `properties-local.xml`, add:

    ```xml
    <property as="xs:string" name="oxf.fr.persistence.provider.db2.*.*"   value="db2"/>
    <property as="xs:string" name="oxf.fr.persistence.provider.exist.*.*" value="exist"/>
    ```
2. Create same form in 2 apps: `exist/a`, `db2/a`.
    - Use Duplicate button in FB Summary
    - Then change app name
3. pages
    - FB: create form, publish
    - FR: check it shows on http://localhost:8080/orbeon/fr/
    - FR: create new form, review, back to edit (#1643)
    - FR: enter data, save
    - FR: check it shows in the summary page
4. attachments
    - FB: attach static image to form
    - FB: add file attachment field
    - FB: save and publish
        - DB2: be aware of #1409
    - FR: deployed form loads image
    - FR: attach file, save, edit
5. search
    - FB: check summary/search field, save and deploy
    - FR: create new form data, see in summary
    - FR: search free-text and structured
    - FR: delete data in summary page works
6. duplicate
    - FR: Summary: Duplicate button works
        - data for latest form
        - older data
7. versioning
    - create form db2/a
        - input field A + file or image attachment
        - publish
        - go to new page
        - enter value a in field A, attach file
        - review and back to edit
        - save
    - edit the form definition
        - rename field in B
        - publish
        - go to new page
        - enter value b in field B, attach file
        - review and back to edit
        - save
    - /fr/service/(mysql|oracle|db2)/a/schema: schema with B is produced
    - /fr/service/(mysql|oracle|db2)/a/schema?form-version=1: schema with A is produced
    - go to the summary page, click on first row (created last), check field B/value b and attachment show
    - go to the summary page, click on second row (created first), check field A/value a and attachment show
    - Form Builder Publish dialog options (new in 4.6)
        - with persistence layer which supports versioning (mysql)
            - if mysql/a form has never been published
                - no options and no messages are shown
                - latest version shows "-"
                - publish message says version 1 was created
            - if mysql/a form has been published
                - latest version shows correct number
                - option to create new version or overwrite (check version numbers)
                - switch option shows different message
                - publish message says which version was created/updated
        - with persistence layer which doesn't support versioning (exist)
            - latest version line doesn't show
            - if no exist/a form has been published
                - no options and no messages are shown
            - if exist/a form has been published
                - no options are shown
                - message about overwrite
            - publish message says version 1 was updated
8. with all persistence layers active
    - go to /fr/
    - check that form definitions from all persistence layers show

  [#1409]: https://github.com/orbeon/orbeon-forms/issues/1409
