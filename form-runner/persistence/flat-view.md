# Flat view

## Usage

### Rationale

If you're using Oracle, SQL Server [SINCE Orbeon Forms 2016.2], or DB2 [SINCE Orbeon Forms 4.7], or PostgreSQL [SINCE Orbeon Forms 4.8], when you deploy a form created in Form Builder, Orbeon Forms can create a form-specific view of your data, with one column for each form field.

### Property to enable

You enable this feature by setting the relevant property listed below to `true`.

Database   | Property
---------- | -----------------------------------------------
Oracle     | `oxf.fr.persistence.oracle.create-flat-view`
SQL Server | `oxf.fr.persistence.sqlserver.create-flat-view`
DB2        | `oxf.fr.persistence.db2.create-flat-view`
PostgreSQL | `oxf.fr.persistence.postgresql.create-flat-view`

For instance, if using  Oracle, you set:

```xml
<property
    as="xs:boolean"
    name="oxf.fr.persistence.oracle.create-flat-view"
    value="true"/>
```

## Generated names

### View name

When you enable this property, upon publishing a form, the persistence layer creates a view specific to that form. The name of the view is based on your app and form name, and has the form:

```
orbeon_f_#{app}_#{form}
```
 
For instance, if your app is `hr` and you form is `expense`, then the view is named `orbeon_f_hr_expense`. If upon publishing, there is already a view with that name, the persistence layer deletes it before recreating a new view.

### Metadata column names

The view always has the following metadata columns, with information copied from the equivalent columns in `orbeon_form_data`:

- [UP TO Orbeon Forms 4.3]
    - `metadata_document_id`
    - `metadata_created`
    - `metadata_last_modified`
    - `metadata_username`
- [SINCE Orbeon Forms 4.4]
    - `metadata_document_id`
    - `metadata_created`
    - `metadata_last_modified_time`
    - `metadata_last_modified_by`

Note that there is no `metadata_draft` column, as drafts are not included the view. (Before 4.7 they were, incorrectly, see [issue 1870](https://github.com/orbeon/orbeon-forms/issues/1870).)

### Data column names

In addition to those columns, you have one column per form field, and each column is named by combining the section name with the control name. On some databases, like Oracle, columns names are limited to 30 characters, so the persistence layer truncates column names. It also converts dashes to underscores, removes any non alphanumeric character except inner underscores, and converts the name to uppercase (so it can be used in queries without quotes).

#### With Orbeon Forms 4.5 and newer

Orbeon Forms 4.5 introduces a new truncation algorithm so names are not cut short unnecessarily, and A numerical *suffix* is used instead for those columns which would introduce duplicates.

Examples:

Section name             | Control name                                 | Column name
------------------------ | -------------------------------------------- | --------------------------------
`personal-information`   | `first-name`                                 | `PERSONAL_INFORMATIO_FIRST_NAME`
                         | `last-name`                                  | `PERSONAL_INFORMATION_LAST_NAME`
                         | `address`	                                | `PERSONAL_INFORMATION_ADDRESS`
`company`                | `name`                                       | `COMPANY_NAME`
                         | `industry`                                   | `COMPANY_INDUSTRY`
`section-with-long-name` | `my-control-with-a-pretty-long-name`	        | `SECTION_WITH_L_MY_CONTROL_WITH`
                         | `my-control-with-a-pretty-long-name-too`     | `SECTION_WITH_L_MY_CONTROL_WIT1`
                         | `my-control-with-a-pretty-long-name-really`	| `SECTION_WITH_L_MY_CONTROL_WIT2`


#### With Orbeon Forms 4.4 and earlier

The section name is truncated to 14 characters, the control name to 15 characters, and both are combined with an underscore in between. In the vast majority of the cases, this will result in distinct and recognizable column names. In cases where two or more columns would end up having the same name or conflict with one of the metadata column, the persistence layer adds a number prefix of the form `001_`, `002_`, `003_`… to each column to make it unique. If this happens, you might want to change your section and/or control names to have more recognizable column names.

Examples:

Section name           | Control name | Column name
---------------------- | ------------ | ---------------------------
`personal-information` | `first-name` | `PERSONAL_INFOR_FIRST_NAME`
                       | `last-name`  | `PERSONAL_INFOR_LAST_NAME`
                       | `address`    | `PERSONAL_INFOR_ADDRESS`
`company`              | `name`       | `COMPANY_NAME`
                       | `industry`   | `COMPANY_INDUSTRY`

## Limitations

- Repeated grids and repeated sections are not supported, see [issue 1069](https://github.com/orbeon/orbeon-forms/issues/1069).
- [SINCE Orbeon Forms 2016.1] Orbeon Forms handles fields inside nested sections and nested section templates. Such fields used to be ignored with Orbeon Forms 4.10 and earlier.
