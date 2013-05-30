## Main function library documentation

The function library is [documented here](http://wiki.orbeon.com/forms/doc/developer-guide/xforms-xpath-functions). New functions are documented below.

## Extension XForms functions

### xxf:r()

The purpose of this function is to automatically resolve resources by name given the current language and an XForms instance containing localized resources.

```ruby
xxf:r($resource-name as xs:string) as xs:string
xxf:r($resource-name as xs:string, $instance-name as xs:string) as xs:string
```

- `$resource-name`: resource path of the form `foo.bar.baz`. The path is relative to the `resource` element corresponding to the current language in the resources instance.
- `$instance-name`: name of the resources instance. If omitted, search `orbeon-resources` and then `fr-form-resources`.

The function:

- determines the current language based on `xml:lang`attribute in scope where the function is in use
-  resolves the closest relevant resources instance
  - specified instance name if present
  - `orbeon-resources` or `fr-form-resources` (for Form Runner compatibility) if absent
- uses the resource name specified to find a resource located in the resources instance, relative to the `resource` element with the matching language

Example:

```xml
<xf:instance id="orbeon-resources" xxf:readonly="true">
    <resources>
        <resource xml:lang="en"><buttons><download>Download</download></buttons></resource>
        <resource xml:lang="fr"><buttons><download>Télécharger</download></buttons></resource>
    </resources>
</xf:instance>

<xf:label value="xxf:r('buttons.download')"/>
```

### xxf:get-request-method()

Return the current HTTP method.

```ruby
xxf:get-request-method() as xs:string
```

Return the HTTP method of the current request, such as `GET`, `POST`, etc.

### xxf:get-portlet-mode()

Return the portlet mode.

```ruby
xxf:get-portlet-mode() as xs:string
```

If running within a portlet context, return the portlet mode (e.g. 'view', 'edit'), otherwise return the empty sequence.

### xxf:get-window-state()

Return the portlet window state.

```ruby
xxf:get-window-state() as xs:string
```

If running within a portlet context, return the window state (e.g. 'normal', 'minimized', 'maximized'), otherwise return the empty sequence.
