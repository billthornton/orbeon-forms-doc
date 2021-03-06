# Persistence configuration properties



## Levels of configuration

Storage for the persistence hierarchy can be configured at multiple levels:

* Globally
* For each _application_
    * The application is referred to with an _app name._
* For each _form definition_ within an application
    * The form definition is referred to with a _form name._
* For each _resource type_ (form definition vs. form data)
    * The resource type is referred to with an identifier:
        * _form_: form definition as XHTML,  with optional associated resources (images, PDF template, and other attachments)
        * _data_: form data as XML, with optional associated attachments

This allow you for example to store certain form definition on disk, while storing the associated data, as filled-out by users, in one or more databases.

## Persistence layer configuration

Orbeon Forms 4.0 introduces a level of indirection in the persistence layer configuration: you map the parameters *app*, *form*, and *form definition or data* to a _provider_, and then map that provider to its REST persistence API. This is configured via properties starting with `oxf.fr.persistence.provider`.

By default, eXist is configured for all apps and forms, including form definitions and form data. This is done by associating the `exist` provider:

```xml
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*"
    value="exist"/>
```

In each such provider mapping, there are 3 configurable pieces of information. They represent, in this order:

1. A string to specify the Form Runner application name, like `orbeon` or `acme`.
2. A Form Runner form name, like `registration` or `address`.
3. Whether the configuration regards form data (`data`), or the form configuration files (`form`).

Each of those can be a wildcard (`*`). Wildcards allow you to setup Orbeon Forms to use different persistence providers for different app, form, and resource  type combinations.

The properties are interpreted hierarchically and you may specify the configuration more or less specifically:

Property                                            | Explanation
----------------------------------------------------|-------------------------------------------------------
`oxf.fr.persistence.provider.*.*.*`                 | form definitions and form data for all applications
`oxf.fr.persistence.provider.*.*.data`              | form data for all applications
`oxf.fr.persistence.provider.*.*.form`              | form definitions for all applications
`oxf.fr.persistence.provider.orbeon.*.form`         | form definitions for all forms in application "orbeon"
`oxf.fr.persistence.provider.orbeon.*.data`         | form data for all forms in  application "orbeon"
`oxf.fr.persistence.provider.orbeon.contact.*`      | form definitions and data for "orbeon/contact"
`oxf.fr.persistence.provider.orbeon.contact.form`   | form definitions for "orbeon/contact"
`oxf.fr.persistence.provider.orbeon.contact.data`   | form data for "orbeon/contact"

_NOTE: This means that you can't name a persistence provider `provider`._

For example some built-in demo forms as well as Form Builder load their form definitions directly from the Orbeon Forms web application. This is done by using more specific properties (without wildcards):

```xml
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.orbeon.builder.form"
    value="resource"/>
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.orbeon.dmv-14.form"
    value="resource"/>
```

One thing you might notice is that, as far as configuring persistence, you treat Form Builder itself just like another form, with app name `orbeon` and form name `builder`.

If you want to change the default provider to Oracle, and since a provider named `oracle` is predefined, the following configuration will do just that:

```xml
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*"
    value="oracle"/>
```

Because wildcards are used, this property does not override the configuration for the built-in demo forms as well as Form Builder! This is desirable, because the Form Builder implementation itself is not usually something you want to store somewhere else.

In the case of Oracle, the wildcards also allow you to setup Orbeon Forms to use different schemas for different app, form, and resource type combinations. For instance you could store everything related to an `hr` app in one Oracle database schema and everything related to another `finance` app in another Oracle database schema. For more on this, see how to setup the persistence layer for [multiple schemas](../../form-runner/persistence/relational-db.md).

Each provider supports standard properties, as follows:

Property                                                                             | Explanation
-------------------------------------------------------------------------------------|-------------------------------------------------------
[`oxf.fr.persistence.[provider].uri`](#property_uri)                                 | specifies the location, via HTTP, of the provider implementation.
[`oxf.fr.persistence.[provider].active`](#property_active)                           | specifies whether the provider is active
[`oxf.fr.persistence.[provider].autosave`](#property_autosave)                       | specifies whether [autosave](../../form-runner/persistence/autosave.md) is supported.
[`oxf.fr.persistence.[provider].permissions`](#property_permissions)                 | specifies whether user/group permissions are supported
[`oxf.fr.persistence.[provider].versioning`](#property_versioning)                   | specifies whether versioning is supported
[`oxf.fr.persistence.[provider].data-format-version`](#property_data-format-version) | specifies the data format version used in the database

### <a name="property_uri"></a> `uri` property

The `uri` property specifies the location, via HTTP, of the provider implementation.

```xml
<property
    as="xs:anyURI"
    name="oxf.fr.persistence.[provider].uri"
    value="[URI to reach the provider]"/>
```

### <a name="property_active"></a> `active` property

[SINCE Orbeon Forms 4.4]

The `active` property specifies whether the provider is active.

This is used as follows (confirmed for Orbeon Forms 4.4 to 4.10):

- If active, the Form Runner Home Page queries the persistence implementation to obtain the list of published forms and enable administrative operations.
- See issue [#2327](https://github.com/orbeon/orbeon-forms/issues/2327).

```xml
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].active"
    value="[true|false]"/>
```
### <a name="property_autosave"></a>`autosave` property

[SINCE Orbeon Forms 4.4]

The `autosave` property specifies whether [autosave](../../form-runner/persistence/autosave.md) is supported.

This is used as follows (confirmed for Orbeon Forms 4.4 to 4.10):

- If `true`, Form Runner enables autosave if the other [conditions](../../form-runner/persistence/autosave.md) are met. Otherwise, Form Runner will not attempt to enable autosave.

```xml
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].autosave"
    value="[true|false]"/>
```

### <a name="property_permissions"></a> `permissions` property

[SINCE Orbeon Forms 4.4]

The `permissions` property specifies whether user/group permissions are supported.

If `true`, Form Runner assumes that permissions are supported by the provider implementation.

This is used as follows (confirmed for Orbeon Forms 4.4 to 4.10):

- The Summary Page sends a 403 if the user doesn't have access based on role, and the persistence provider is known
  not to support permissions.

```xml
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].permissions"
    value="[true|false]"/>
```

### <a name="property_versioning"></a> `versioning` property

[SINCE Orbeon Forms 4.5]

The `versioning` property specifies whether versioning is supported.

If `true`, Form Runner assumes that versioning is supported by the provider implementation.

This is used as follows (confirmed for Orbeon Forms 4.5 to 4.10):

- At form publishing time, Form Builder proposes the option to overwrite the existing published form definition, or to create a new version.

```xml
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].versioning"
    value="[true|false]"/>
```

### <a name="property_data-format-version"></a> `data-format-version` property

[SINCE Orbeon Forms 2017.1]

The `data-format-version` property specifies which data format version is in the database.

Allowed values:

- `4.0.0`
- `4.8.0`

The values must match exactly.

The default is `4.0.0` for backward compatibility.

This property must be changed very carefully. All form data in the database for a given provider must be in the same format and it is not possible, at this point, to change the value of this property if there is existing data in the database.

```xml
<property 
    as="xs:string"
    name="oxf.fr.persistence.*.data-format-version"
    value="4.8.0"/>
```

_NOTE: As of Orbeon Forms 2017.1, the `oxf.fr.detail.new.service.enable` property always assumes data in `4.0.0` format even if the property above is set to a different value._

## Multiple databases of the same type

Say you have two MySQL databases (or a single MySQL database with different schemas and users configured) and you would like to configure Orbeon Forms to store form definitions and form data to one or the other of the databases. Assume the following JDBC datasources:

- `mysql_foo`
- `mysql_bar` 

You associate each datasource with a Form Runner *persistence provider* with the following properties:
 
```
<property as="xs:anyURI"  name="oxf.fr.persistence.mysql_foo.uri"         value="/fr/service/mysql"/>
<property as="xs:string"  name="oxf.fr.persistence.mysql_foo.datasource"  value="mysql_foo"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.autosave"    value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.permissions" value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.versioning"  value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.active"      value="true"/>

<property as="xs:anyURI"  name="oxf.fr.persistence.mysql_bar.uri"         value="/fr/service/mysql"/>
<property as="xs:string"  name="oxf.fr.persistence.mysql_bar.datasource"  value="mysql_bar"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.autosave"    value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.permissions" value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.versioning"  value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.active"      value="true"/>
```

Then if you would like:

- form definitions and form data for the Form Runner app `foo` to go to `mysql_foo` 
- form definitions and form data for the Form Runner app `bar` to go to `mysql_bar`
 
Add:

```
<property as="xs:string"  name="oxf.fr.persistence.provider.foo.*.*"      value="mysql_foo"/>
<property as="xs:string"  name="oxf.fr.persistence.provider.bar.*.*"      value="mysql_bar"/>
```

## Configuration properties for specific persistence providers

Each provider may have specific configuration properties. For the latest settings, see
[`properties-form-runner.xml`](https://github.com/orbeon/orbeon-forms/blob/master/src/main/resources/config/properties-form-runner.xml#L17).

## See also

- [Form Runner persistence API](../../form-runner/api/persistence/README.md)
