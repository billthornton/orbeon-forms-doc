> [[Home]] ▸ [[Form Runner|Form Runner]]

## Integration points

Form Builder and Form Runner integrate with other systems through the following means:

- __Plain URLs__
    - Through URLS you access Form Runner and Form Builder's pages
    - The URLs can be accessed simply by using hyperlinks or redirects from other applications.
- __A [[configurable persistence API|Form Runner ~ APIs ~ Persistence]]__
    - The API is based on REST (that is, through HTTP).
    - It provides CRUD, search, and metadata operations.
- __External user management__
    - This allows you to connect Orbeon Forms to a directory of users with associated roles.
- __HTTP services__
    - via the [[HTTP Service Editor|Form Builder ~ HTTP Services]]
    - via properties to load initial XML data
    - via processes to [[submit data|Form Runner ~ Buttons and Processes#send]]
- __[[Embedding|Form-Runner ~ Embedding]]__
    - with the [[Server-side embedding API|Form Runner ~ APIs ~ Server side Embedding]]
    - with the [[Form Runner proxy portlet|Form Runner ~ Portal ~ Liferay Proxy Portlet Guide]]
    - with the [[Form Runner full portlet|Form Runner ~ Portal ~ Full Portlet Guide]]

The persistence API can be implemented either within Orbeon Forms (like for example the built-in eXist persistence layer), or within an external system.

### Form Runner URLs

Form Runner and Form builder attempt to use friendly URLs.

The following URL patterns are followed:

* Summary page for a given form definition:
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/summary`
* New empty form data:
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/new`
* Edit existing form data:
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/edit/[DOCUMENT_ID]`
* Read-only HTML view:
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/view/[DOCUMENT_ID]`
* Read-only PDF view:
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/pdf/[DOCUMENT_ID]`
* Read-only TIFF view: [SINCE Orbeon Forms 4.11]
    `/fr/[APPLICATION_NAME]/[FORM_NAME]/tiff/[DOCUMENT_ID]`

See also [[Form Builder Integration|Form Builder ~ Integration]].

_NOTE: All paths above are relative to the deployment context, e.g the actual URLs start with http://localhost:8080/orbeon/fr/..._

### Persistence API

The persistence API is used to store and retrieve *form definitions* and *form data*.

Orbeon Forms ships out of the box with support for a number of databases (see [[Database Support|Orbeon Forms Features ~ Database Support]]). Integrators can provide persistence via the [[Form Runner persistence API|Form Runner ~ APIs ~ Persistence]].

### External user management

See [[Access Control|Form Runner ~ Access Control]].

### XML representation of form data

See [[Data Format|Form Runner ~ Data Format]]

_NOTE: Non-repeated grids do not create containing elements._

## See also

- [[Form Builder Integration|Form Builder ~ Integration]]
- [[Form Runner Embedding|Form-Runner ~ Embedding]]
- [[Form Runner persistence API|Form Runner ~ APIs ~ Persistence]]
- [[Access Control|Form Runner ~ Access Control]].
- [[Data Format|Form Runner ~ Data Format]]