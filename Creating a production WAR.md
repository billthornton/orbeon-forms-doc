## Rationale

The standard Orbeon Forms WAR comes with demo apps and forms. For production, you can safely remove some of that from the WAR file.

## What can be removed

For most deployments, the following can be removed:

- `xforms-jsp`: demo JSP files
- `WEB-INF/resources/apps`: demo apps
- `WEB-INF/resources/forms/orbeon/controls`: some demo forms resources
- `WEB-INF/resources/forms/orbeon/dmv-14`: some demo forms resources
- `orbeon-cli.jar` and `commons-cli-1_0.jar`: for command-line XPL
- `exist-data`: embedded eXist XML database (setup an external database instead)

## Removing Form Builder

Form Builder is packaged as a separate JAR file:

`WEB-INF/lib/orbeon-form-builder.jar`

If you don't need Form Builder in an installation, you can simply remove that JAR file.

