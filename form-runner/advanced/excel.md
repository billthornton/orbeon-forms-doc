# Excel Import



## Availability

This is an Orbeon Forms PE feature.

## What it is

This feature allows you to import batches of data from a source Excel spreadsheet to a deployed Form Runner form.

## How it works

It's pretty simple!

1. You start the import from the Form Runner import page, accessible from the summary page

    ![](../images/excel-import-summary.png)

2. You select the Excel 2007 file to upload and import

    ![](../images/excel-import-select.png)

3. Form Runner validates the Excel file and give you the option to add to existing data for the given form, or remove all existing data first

    ![](../images/excel-import-validate.png)

4. Form Runner imports valid data from the Excel file

    ![](../images/excel-import-import.png)

_NOTE: Only the Excel 2007 .xlsx format (Office Open XML) is supported. The older, .xls format is not supported._

## Enabling the import button

You enable the import button on the Summary page by adding the import token to the `oxf.fr.summary.buttons.*.*` property. Here for the Orbeon Contact form:

```xml
<property
    as="xs:string"
    name="oxf.fr.summary.buttons.orbeon.contact"
    value="new import edit print pdf delete"/>
```

## Mapping between form controls and spreadsheet

A given Excel file contains data for a single Orbeon Forms form.

The spreadsheet must follow this format:

- only the first sheet is considered
- the first row is a special header row, where each cell contains an identifier that matches a control name in the given form
- each subsequent row contains data for a new instance of form data

In your form, you create controls with names that match the names in the first row (header row) of the Excel document.

Here is an example spreadsheet for the sample Orbeon Contact form:

![]![](../images/excel-import-sheet.png)

_NOTE: Only characters allowed in XML names are allowed as control names in Form Builder. In case your Excel header row requires names with non-XML characters (Form Builder will tell you the name is not allowed), simply replace them by "_" in Form Builder._

## Allowing invalid data

[SINCE Orbeon Forms 2017.2]

By default, invalid data is skipped during import.

You can enable the optional import of invalid data with the following property:

```xml
<property
    as="xs:boolean"
    name="oxf.fr.import.allow-invalid-documents.*.*"
    value="true"/>
``` 

By default, it is set to `false` and the user is not provided with an option to skip invalid data.

When set to `true`, the user is provided with an option to skip invalid data at the time of import:

![](../images/excel-import-validate-allow-invalid.png)


## Limitations

The import functionality does not support importing data into repeated grids.
