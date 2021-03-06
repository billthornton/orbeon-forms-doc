# CRUD



## Basics

When using `GET` and `PUT` to deal with resources, the body of HTTP requests contains the resource to handle.

- `GET`
    - The request body is empty and response body contains resource.
    - [SINCE Orbeon Forms 4.5] The `Orbeon-Operations` response header lists the operations that the user can perform on the data (see [Supporting permissions in your persistence API implementation](http://blog.orbeon.com/2013/10/supporting-permissions-in-your.html)).
    - [SINCE Orbeon Forms 4.5] If the implementation of the persistence API supports form versioning, and the request is for a form definition, the `Orbeon-Form-Definition-Version` request header tells which version of the form definition is requested.
- `PUT`
    - The request body contains the resource to store.
    - The `Content-Type` must contain the content-type of the resource to store. For XML in particular, pass `application/xml`.
    - The request response is empty.
    - If your implementation of the persistence API supports form versioning, then when storing a new document, you need to tell the persistence what version of the form definition was used to create this data, so the same version can then be used to subsequently edit the data. This is done by specifying the version in the `Orbeon-Form-Definition-Version` header.
- `DELETE`
    - Both request and response bodies are empty.

When the resource is an XML file (e.g. form.xhtml, data.xml), the persistence layer must return an appropriate XML content-type: `application/xml`, or `application/xhtml+xml`.

## Examples using curl

The following examples use the [curl](https://curl.haxx.se/) command-line utility. They are indented on multiple lines for clarity but in practice each command must be written on a single line.

The service must be open for these examples to work.
See [Authorization of pages and services](../../../xml-platform/controller/authorization-of-pages-and-services.md).

The general format of the a path to access form data is:

```
/fr/service/persistence/crud/$app/$form/(data|draft)/$doc/data.xml
```

So with the following assumptions:

- app name (`$app`): `acme`
- form name (`$form`): `order`
- document id (`$doc`): `fc4c32532e8d35a2d0b84e2cf076bb070e9c1e8e`

then you can `PUT` form data as follows:

```
curl
  -v
  -k
  -d @mydata.xml
  -H "Content-Type: application/xml"
  -H "Orbeon-Form-Definition-Version: 1"
  -X PUT
  http://localhost:8080/orbeon/fr/service/persistence/crud/acme/order/data/fc4c32532e8d35a2d0b84e2cf076bb070e9c1e8e/data.xml
```

The following retrieves the data (the `GET` method is used implicitly by curl):

```
curl 
  -v 
  -k 
  -H "Orbeon-Form-Definition-Version: 1" 
  http://localhost:8080/orbeon/fr/service/persistence/crud/acme/order/data/fc4c32532e8d35a2d0b84e2cf076bb070e9c1e8e/data.xml
```

The same API is used by Form Builder to store in-progress (unpublished) form definitions under the `orbeon/builder` app/form names. So with the following assumptions:

- app name (`$app`): `orbeon`
- form name (`$form`): `builder`
- document id (`$doc`): `7b55c9d6f9b058376293e61d9f0d4442e379f717`

then you can `PUT` a form definition so that it's available to Form Builder:

```
curl
  -v
  -k
  -d @mydata.xml
  -H "Content-Type: application/xml"
  -H "Orbeon-Form-Definition-Version: 1"
  -X PUT
  http://localhost:8080/orbeon/fr/service/persistence/crud/orbeon/builder/data/7b55c9d6f9b058376293e61d9f0d4442e379f717/data.xml
```

The following retrieves the form definition (the `GET` method is used implicitly):

```
curl 
  -v 
  -k 
  -H "Orbeon-Form-Definition-Version: 1" 
  http://localhost:8080/orbeon/fr/service/persistence/crud/orbeon/builder/data/7b55c9d6f9b058376293e61d9f0d4442e379f717/data.xml
```

## URL parameters

### Supported by the persistence implementation

[SINCE Orbeon Forms 2017.1]

- `nonrelevant`
    - when using GET for data only
    - values
        - `remove`: remove all XML data elements with attribute `fr:relevant="false"`
        - `keep`: [SINCE Orbeon Forms 2018.1] do not remove XML data elements
        - missing: do not remove XML data elements
        
_NOTE: This property only operates properly on data stored with Orbeon Forms 2017.1 and newer, as only Orbeon Forms 2017.1 and newer stores data with the `fr:relevant="false"` annotation._

### Passed by Form Runner

[SINCE Orbeon Forms 4.2]

The following URL parameters are set by Form Runner:

- `valid`
    - passed when data is stored using PUT only
    - values
        - `true`: data sent satisfies validation rules
        - `false`: data sent does not satisfy validation rules
    
This allows a persistence implementation to store this information if desired.

Example:

```xml
http://example.org/orbeon/fr/service/persistence/crud/
    orbeon/
    bookshelf/
    data/891ce63e59c17348f6fda273afe28c2b/data.xml?
    valid=true
```

## Publishing from Form Builder

When you create, edit, or read form definition with Form Builder, form definitions are stored as Form Builder _data_. That is, they are stored under:

`/crud/orbeon/builder/data/[FORM_DATA_ID]`

On the other hand, when Form Builder _publishes_ a form definition, it stores it where Form Runner will find it, that is under:

```
/crud/[APPLICATION_NAME]/[FORM_NAME]/form
```

For example, this is what happens when saving and publishing a form definition `acme/order` with a single attachment:

Save:

- PUT /crud/orbeon/builder/data/7b55c9d6f9b058376293e61d9f0d4442e379f717/a29fd47011b2957ef44a62d92995adfdbae03fa9.bin
- PUT /crud/orbeon/builder/data/7b55c9d6f9b058376293e61d9f0d4442e379f717/data.xml

Publish:

- PUT /crud/acme/order/form/a29fd47011b2957ef44a62d92995adfdbae03fa9.bin
- PUT /crud/acme/order/form/form.xhtml?document=7b55c9d6f9b058376293e61d9f0d4442e379f717

[SINCE Orbeon Forms 4.6]

When Form Builder publishes a form definition, if versioning is supported by the target persistence layer, it passes a `Orbeon-Form-Definition-Version` header with values:

- `next`: to indicate that the form definition must be published under the next available version
- or a specific version number: to indicate that the form definition must replace the given version

## Deleting all data for an existing form

To remove all instances of form data, issue a DELETE to:

```
/crud/[APPLICATION_NAME]/[FORM_NAME]/data/
```
