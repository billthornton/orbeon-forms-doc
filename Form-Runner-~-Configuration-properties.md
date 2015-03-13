## Default values

For the latest configuration, see [properties-form-runner.xml](https://github.com/orbeon/orbeon-forms/blob/master/src/resources-packaged/config/properties-form-runner.xml).

## Form Runner properties documented elsewhere

* [[Custom Model Logic|Form-Runner-~-Custom-Model-Logic]]
    * `oxf.fr.detail.model.custom`
* [[Wizard View|Form Runner ~ Wizard View]]
    * `oxf.fr.detail.view.appearance`
* [[Autosave|Form Runner ~ Autosave]]
    * `oxf.fr.detail.autosave-delay`
    * `oxf.fr.persistence.*.autosave`
* [[Configuration Properties ~ Persistence|Form Runner ~ Configuration properties ~ Persistence]]
    * `oxf.fr.persistence.provider`
    * `oxf.fr.persistence.[provider].uri`
    * `oxf.fr.persistence.[provider].active`
    * `oxf.fr.persistence.[provider].autosave`
    * `oxf.fr.persistence.[provider].permissions`
    * `oxf.fr.persistence.[provider].versioning`
* [[Form Runner Access Control|https://github.com/orbeon/orbeon-forms/wiki/Form-Runner-~-Access-Control-~-Setup]]
    * `oxf.fr.support-owner-group`
    * `oxf.fr.authentication.method`
    * `oxf.fr.authentication.container.roles`
    * `oxf.fr.authentication.container.roles.split`
    * `oxf.fr.authentication.header.username`
    * `oxf.fr.authentication.header.group`
    * `oxf.fr.authentication.header.roles`
    * `oxf.fr.authentication.header.roles.split`
    * `oxf.fr.authentication.header.roles.property-name`

## General

### Default language

```xml
<property 
    as="xs:string" 
    name="oxf.fr.default-language.*.*" 
    value="en">
```

Form Runner's default language.


### Available languages

[SINCE 2012-06-19]

For a given form, you can filter which languages are available in the language selector with a space-separated list of language codes:

```xml
<property
  as="xs:string"
  name="oxf.fr.available-languages.*.*"
  value="en fr"/>
```

The language selector by default shows all languages available in the form definition.

When this property is specified, only the intersection of the languages is shown in the selector. For example:

* Example 1
    * form languages: `en fr jp`
    * property: `en fr`
    * resulting languages: `en fr`
* Example 2
    * form languages: `en fr jp`
    * property: `en jp kr`
    * resulting languages: `en jp`

If the property is blank or contains the wildcard `*`, all the form languages are available.

[SINCE 2012-07-18 / Orbeon Forms 4.3]

For pages that don't involve a specific form, like the Form Runner home page, the following property controls the available languages:

```xml
<property
  as="xs:string"
  name="oxf.fr.available-languages"
  value="en fr"/>
```

## Summary page

### Summary page size

```xml
<property 
    as="xs:integer" 
    name="oxf.fr.summary.page-size.*.*" 
    value="10"/>
```

Number of rows shown in the summary page.

### Created and Last Modified columns

By default, the summary page shows a Created and Last Modified columns:

![](images/fr-summary-created-last-modified.png)

You can remove either one of those columns by setting the value appropriate property to `false`:

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.summary.show-created.*.*"        
    value="true"/>
    
<property 
    as="xs:boolean" 
    name="oxf.fr.summary.show-last-modified.*.*"  
    value="true"/>
```

### Buttons on the summary page

```xml
<property
    as="xs:string"
    name="oxf.fr.summary.buttons.*.*"
    value="home review pdf delete duplicate new"/>
```

The property configures which buttons are included on the summary page, and in what order they are shown. Possible buttons are:

* `home`
    * Label: "Home"
    * Action: Navigate to the Form Runner Home page.
* review
    * Label: "Review"
    * Action: Navigate to the detail page in "view" mode to review the selected form data.
* `pdf`
    * Label: "PDF"
    * Action: Create a PDF file for the selected form data.
* `delete`
    * Label: "Delete"
    * Action: Delete the selected form data.
* `import`
    * Label: "Import"
    * Action: Import data via the [Import page][8].
* `duplicate` [SINCE Orbeon Forms 4.5] 
    * Label: "Duplicate"
    * Action: Duplicate the selected form data (or form definition on the Form Builder Summary page), including attachments.
    * Usage: Select one or more checkboxes and press the "Duplicate" button. When the operation completes the Summary page refreshes with the new duplicated form data.
* `new`
    * Label: "New"
    * Action: Navigate to the detail page in "new" mode to create new form data.

## Detail page

### Show table of contents

```xml
<property 
    as="xs:integer" 
    name="oxf.fr.detail.toc.*.*" 
    value="0"/>

```

If the number of table of contents entries are greater than this value, then show the Table of Contents at the top of the form. Can be omitted or set to -1 to never show the TOC.

### Position of error summary

```xml
<property 
    as="xs:string" 
    name="oxf.fr.detail.error-summary.*.*" 
    value="bottom"/>
```

Where to place the error summary: `top`, `bottom`, `both`, or `none`.

### Buttons on the detail page

```xml
<property 
    as="xs:string"  
    name="oxf.fr.detail.buttons.*.*" 
    value="close clear print pdf save submit"/>
```

The property configures which buttons are included on the detail page, and in what order they are shown. For more information, see Form Runner: Buttons and Processes.

### Buttons in detail page custom views

[SINCE 2013-05-03 / Orbeon Forms 4.2]

When using the wizard view (or other detail page custom views), the following property allows specifying which buttons are presented inside the view, as opposed to the bottom of the page:

```xml
<property
    as="xs:string"
    name="oxf.fr.detail.buttons.inner.*.*"
    value="wizard-prev wizard-next"/>
```

The example above places the wizard's Prev and Next buttons just under the current wizard section:

![](images/fr-wizard-buttons.png)

For more information, see Form Runner: Buttons and Processes.

### Display hints inline

This property set whether the control hints are shown inline, rather than as tool-tips. The default is `true`.

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.hints.inline.*.*" 
    value="true"/>
```

### PDF

#### Custom PDF filename

[SINCE Orbeon Forms 4.9]

The following property dynamically controls the name of the PDF file produced on the detail page. By default, if the property value is blank, the PDF filename is a random id assigned to the current form session.

```xml
<property
    as="xs:string"
    name="oxf.fr.detail.pdf.filename.*.*"
    value=""/>
```

The value of the property, if not empty, is an XPath expression which runs in the context of the root element of the XML document containing form data. The trimmed string value of the result of the expression is used to determine the filename.

Example:

```xml
<property
    as="xs:string"
    name="oxf.fr.detail.pdf.filename.*.*"
    value="//customer-id"/>
```

If the form contains a `customer-id` field, the PDF filename will be the value of that field followed by `.pdf`. If the field is blank, the default, random id filename is used, as if the property had not been specified.

#### Hyperlinks in automatic mode

[SINCE Orbeon Forms 4.6]

The following property controls whether hyperlinks are enabled in the generated PDF. By default, they are enabled:

```xml
<property
    as="xs:boolean"
    name="oxf.fr.detail.pdf.hyperlinks.*.*"
    value="true"/>
```

When set to `true`:

* HTTP and HTTPS URLs in input field and text areas are automatically hyperlinked.
* Hyperlinks in rich text controls are preserved.
* Hyperlinks in the rest of the form, if any, are preserved.

When set to `false`:

* HTTP and HTTPS URLs in input field and text areas are not hyperlinked, but placeholders are added.
* Hyperlinks in rich text controls are removed and placeholders are left.
* Hyperlinks in the rest of the form, if any, are removed and placeholders are left.
* Placeholders consist of an HTML `<a>` without an `href` attribute. This helps with CSS styling.

For example, the default style for hyperlinks only highlights and underlines `<a>` elements with an `href` attribute:

```css
a[href] {
    text-decoration: underline;
    &:link, &:visited {
        color: @linkColor !important;
    }
}
```

#### Barcode

The following property specifies whether a barcode must be included on PDF files.

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.pdf.barcode.*.*" 
    value="false">
```

#### Font embedding in automatic mode

[SINCE 2011-09-27]

These properties allow specifying fonts to embed in PDF files. The `oxf.fr.pdf.font.path` property ends with an identifier for the font (here `vera`). It specifies the path to the font file. Optionally, the oxf.fr.pdf.font.family property ending with the same identifier (here `vera`) allows overriding the font family.

```xml
<property 
    as="xs:string" 
    name="oxf.fr.pdf.font.path.vera"   
    value="/path/to/font.ttf"/>
    
<property 
    as="xs:string" 
    name="oxf.fr.pdf.font.family.vera" 
    value="Arial"/>
```

As of Orbeon Forms 4.x, to change the main font, you must map to the Helvetica Neue font. For example;

```xml
<property
    as="xs:string"
    name="oxf.fr.pdf.font.path.my-font"
    value="/path/to/font.ttf"/>

<property
    as="xs:string"
    name="oxf.fr.pdf.font.family.my-font"
    value="Helvetica Neue"/>
```

#### Font embedding in template mode

[SINCE 2012-06-16]

In template mode, fonts can be specified to provide glyphs which are not present in the PDF's original font(s). Several fonts can be specified, separated by spaces:

```xml
<property
    as="xs:string"
    name="oxf.fr.pdf.template.font.paths"
    value="/path/to/font1.ttf /path/to/font2.ttf"/>
```

#### Disabling the PDF button when form is invalid

[BEFORE 2013-03-13]

With version 4.0 and earlier, the PDF button is always disabled if invalid data is present in the form.

[SINCE 2013-03-13]

The PDF button is always enabled, allowing users to generate a PDF for the current form, even if some data in the form is invalid. If instead, you wish to disable the PDF button when the form is invalid, set the following property to `true` (it is set to `false` by default):

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.pdf.disable-if-invalid.*.*" 
    value="false"/>
```

### Captcha

[SINCE 2011-06-30]

reCAPTCHA support.

[SINCE 2012-05-19]

SimpleCaptcha support.

If you are creating a public form, you might want to add a captcha to avoid spam. You can do so by enabling the _captcha_ feature, which you do by adding the following property to your `properties-local.xml`:

```xml
<property 
    as="xs:string" 
    name="oxf.fr.detail.captcha.*.*" 
    value="reCAPTCHA"/>
```

You can set this property to either `reCAPTCHA` or `SimpleCaptcha`, depending on the captcha implementation you want to use (also see: [Which captcha is right for you][10]).  Setting it to blank (empty string), won't show a captcha, which is the default. Instead of stars (`*`) in the name of the first property, use specific app/form names for the captcha to only show on certain forms.

If using the reCAPTCHA, also add the following properties to specify your reCAPTCHA public and private keys. You can get those by [signing up for reCAPTCHA][11].

```xml
<property 
    as="xs:string"  
    name="oxf.xforms.xbl.fr.recaptcha.public-key"  
    value="..."/>
    
<property 
    as="xs:string"  
    name="oxf.xforms.xbl.fr.recaptcha.private-key" 
    value="..."/>
```

With those properties in place, your forms will show a captcha as illustrated by the following screenshot.

![](images/fr-recaptcha.png)

[LIMITATION] The Form Runner captcha uses the [captcha XBL components][13], which doesn't support the noscript mode. Hence,  enabling this feature will have no effect in noscript mode.

### Initial instance

When creating a new form (for instance going to the URL `http://localhost:8080/orbeon/fr/orbeon/bookshelf/new`), the initial form instance can come from 3 different places:

1. The initial instance provided in the form can be used.
2. The Base64-encoded XML documented POSTed to the "new form" URI can be used.
3. A service can be called to get the initial instance.

#### Initial instance posted to the New Form page

The instance provided in the form is used by default and the POSTed XML document is used if there actually is an XML document being POSTed.

The document can be POSTed in two ways:

1. As a direct POST of the XML document
2. As an HTML form POST parameter called fr-form-data

For #2, this behaves as if a browser was submitting an HTML form that looks like the following, with the value of the `fr-form-data` request parameter being the Base64-encoded XML document.:

```xml
<form method="post" action="/path/to/new">
    <input type="hidden" name="fr-form-data" value="Base64-encoded XML"/>
</form>
```

[SINCE Orbeon Forms 4.8]

The format of the instance data follows the Orbeon Forms 4.0.0 format by default. You can change this behavior to POST data in the latest internal format by specifying the `data-format-version=edge` request parameter. This is useful if you obtained the data from, for example, a send action using [`format-version = "edge"`][14].

Use the authorization mechanism for services (see [Authorization of pages and services](http://wiki.orbeon.com/forms/doc/developer-guide/page-flow-controller/authorization)), to enable submitting initial instances to the new page:

* Your external application must provide credentials (e.g. BASIC authorization, a secret token, etc.) when POSTing to Form Runner.
* Your authorizer service must validate those credentials.

#### Initial instance from service

With the following properties, you can configure Form Runner to call a service instead of using the default instance provided as part of the form:

```xml
<property
    as="xs:boolean"
    name="oxf.fr.detail.new.service.enable.*.*"
    value="false"/>

<property
    as="xs:string"
    name="oxf.fr.detail.new.service.uri.*.*"
    value="/fr/service/custom/my-app/new"/>
```

Set the first property above to `true` to enable this behavior and have the second property point to your service.

The following property defines a space-separated list of request parameters to be passed to the service. Say the new page was invoke with request parameters `foo=42` and `bar=84`, if you set the value of this property to `foo bar`, these two request parameters will be passed along as request parameters to the service. The request parameters can either get to the new page in a POST or GET request. The service is always called with a GET, consequently request parameters will be passed on the URI.

```xml
<property
    as="xs:string"
    name="oxf.fr.detail.new.service.passing-request-parameters.*.*"
    value=""/>
```

*NOTE: Enabling `oxf.fr.detail.new.service.enable` doesn't change the behavior with regard to POSTed instance: even if you are calling a service to get the initial instance, the POSTed instance will be used when a document is POSTed to the corresponding "new form" page.*

## View page

### Buttons on the view page

You configure which buttons are shown on the view page with the following property:

```xml
<property 
    as="xs:string"  
    name="oxf.fr.detail.buttons.view.*.*" 
    value="back workflow-edit pdf"/>
```

You can use all the buttons available on the detail page. In addition, the following buttons apply:

* `workflow-edit`
    * Label: "Edit"
    * Action: Navigate back to the detail page in "edit" mode.

### Show Orbeon Forms version

[UNTIL Orbeon Forms 4.6, use `oxf.show-version` starting Orbeon Forms 4.6.1]

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.version.*.*"
    value="true"/>
```

Whether to show the Orbeon Forms version at the bottom.

### Default logo

```xml
<property
    as="xs:anyURI"
    name="oxf.fr.default-logo.uri.*.*"
    value="/apps/fr/style/orbeon-logo-trimmed-transparent-42.png"/>
```

With this property, you can set the default logo URI. This logo appears on the summary and detail pages for a given form. You can omit (or comment out) this property or set its value to `""` (empty string) if you don't want a default logo at all.

### Adding your own CSS

#### Adding your own CSS files (Orbeon Forms 4)

[SINCE 2012-12-12]

1. Place your CSS file(s) under one of the following recommended locations:
    * `WEB-INF/resources/forms/assets`: CSS for all forms
    * `WEB-INF/resources/forms/APP/assets`: CSS for app name APP
    * `WEB-INF/resources/forms/APP/FORM/assets`: CSS for app name APP and form name FORM
2. Define the `oxf.fr.css.custom.uri.*.*` property to point to the file(s) you added.

```xml
<property
    as="xs:string"
    name="oxf.fr.css.custom.uri.*.*"
    value="/config/acme.css"/>
```

You can add more than one file, and just separate the paths by whitespace in the property.

### Authoring CSS

1. **Disable the minimal and combined resources**. When working on your CSS, you might want to temporarily set the following properties in your `properties-local.xml`, which  will disable the combined and minimized resources, so the files and line numbers you see in your browser correspond to what you have on disk.

    ```xml
    <property 
        as="xs:boolean" 
        name="oxf.xforms.minimal-resources" 
        value="false"/>
    
    <property 
        as="xs:boolean" 
        name="oxf.xforms.combine-resources" 
        value="false"/>
    ```
2. **Know which class names to use in your CSS selectors**. We strongly recommend you use the [Chrome Dev Tools][15] or [Firebug][16] to check which classes are generated by Orbeon Forms. Look specifically for classes that start with `fr-`. Once you have your CSS working with Chrome and/or Firefox, to test it on IE, you'll need to enable minimal resources, as IE is [unable to loads more than 31 CSS files][17].

 **Use case** |  **Sample CSS** |  **Description**
-----|-----|-----
 Change the width of a column |  `.fr-grid-invoice .fr-grid-col-1 { width: 40px }` | 1. In Form Builder, you can name grids (for now [only repeated grids can be named][18]). When doing so, the table element corresponding to your grid gets a `fr-grid-my-name` class, where `my-name` is the name you choose for the grid. In the example, the name was `invoice`. 2. Each column gets a class `fr-grid-col-1`, `fr-grid-col-2` and so on, starting with the number 1.

### Adding your own JavaScript

[SINCE Orbeon Forms 4.4]

1. Place your JavaScript file(s) under one of the following recommended locations:
    * `WEB-INF/resources/forms/assets`: scripts for all forms
    * `WEB-INF/resources/forms/APP/assets`: scripts for app name APP
    * `WEB-INF/resources/forms/APP/FORM/assets`: scripts for app name APP and form name FORM
2. Define the `oxf.fr.js.custom.uri.*.*` property to point to the file(s) you added.

```xml
<property
    as="xs:string"
    name="oxf.fr.js.custom.uri.*.*"
    value="/forms/acme/assets/acme.js forms/acme/sales/assets/acme-sales.js"/>
```

You can add more than one file, and just separate the paths by whitespace in the property.

### Overriding resources

In some cases, it might make sense to change some of the resources provided out of the box by Form Runner. For instance, the detail page can have a submit button, which in English has a label "Submit". For your application, another label might make more sense, for instance "Send". To override Form Runner resources, you define properties with a name that has the following structure:

1. The name start with `oxf.fr.resource`.
2. Followed by the name of the application and form name for which you want to redefine the resource. You can use `*` for either if you want the redefinition to apply to all the applications or all the forms. For instance: `*.*`, or `my-app.my-form`.
3. The 2-letter code for the language for which you want to override the resource. For instance: `en`.
4. A dot-separated path corresponding to the path of the resource you want to override as defined by Form Runner [`resources.xml`][19].
5. Resources are aggressively caches, so you need to restart your application server (or redeploy the web app) after changing a property that overrides resources.

For instance, to change the label of the submit button to be "Send" in English for all applications and forms, write:

```xml
<property
    as="xs:string"
    name="oxf.fr.resource.*.*.en.detail.buttons.send"
    value='&lt;i class="icon-arrow-right"/&gt; Send'/>
```

### Email settings

These properties control email sending in Form Runner:

```xml
<property
    as="xs:string"
    name="oxf.fr.email.smtp.host.*.*"
    value="my.outgoing.smtp.server.org"/>

<!-- SINCE 2012-07-23 / Orbeon Forms 4.0 -->
<property
    as="xs:string"
    name="oxf.fr.email.smtp.port.*.*"
    value="587"/>

<!-- SINCE 2012-07-23 / Orbeon Forms 4.0 -->
<property
    as="xs:string"
    name="oxf.fr.email.smtp.encryption.*.*"
    value="tls"/>

<property
    as="xs:string"
    name="oxf.fr.email.smtp.username.*.*"
    value="jdoe"/>

<property
    as="xs:string"
    name="oxf.fr.email.smtp.credentials.*.*"
    value="secret"/>

<property
    as="xs:string"
    name="oxf.fr.email.from.*.*"
    value="john@example.org"/>

<property
    as="xs:string"
    name="oxf.fr.email.to.*.*"
    value="mary@example.org,nancy@example.org"/>

<property
    as="xs:boolean"
    name="oxf.fr.email.attach-pdf.*.*"
    value="true"/>

<property
    as="xs:boolean"
    name="oxf.fr.email.attach-xml.*.*"
    value="true"/>
```

* The following properties control the connection to the SMTP server.
    * `host`: required SMTP host name
    * `port`: [SINCE 2012-07-23] optional SMTP port override. If not specified, the defaults are:
        * plain SMTP: 25
        * TLS: 587
        * SSL: 465
    * `encryption`: [SINCE 2012-07-23] 
        * blank: none (plain SMTP)
        * `tls`: use TLS
        * `ssl`: use SSL
    * `username`: SMTP username (required if TLS or SSL is used, optional otherwise)
    * `credentials`: SMTP password
* The `from` property specifies the sender email address(es) appearing in the email sent.
* The `to` property specifies the recipient email address(es) of the email sent.
* The `attach-pdf` and `attach-xml` properties control whether PDF and/or XML versions of the form data are attached to the email.

## Noscript and Ajax modes

### Show Noscript link

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.noscript-link.*.*" 
    value="true"/>
```

Whether to show the link to the noscript/full version. 

### Noscript: use table layout

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.noscript.table.*.*" 
    value="true"/>
```

Whether forms in noscript mode are allowed to use a layout based on tables. If false, no tables are used. WYSIWYG is lost, but the form may be more accessible. Default is true.

Noscript: allow sections to collapse

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.noscript.section.collapse.*.*" 
    value="true"/>
```

Whether sections in noscript mode are allowed to collapse. A value of false may make sections more accessible and less confusing to screen reader users. Default is true.

### Ajax mode: allow sections to collapse

```xml
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.ajax.section.collapse.*.*" 
    value="true"/>
```

Whether sections in Ajax mode are allowed to collapse. Default is true.

### Ajax mode: allow sections to animate

```xml
<property
    as="xs:boolean" 
    name="oxf.fr.detail.ajax.section.animate.*.*" 
    value="true"/>
```

Whether sections in Ajax mode are allowed to open and close with an animation. Default is true. A value of false is more efficient with slow browsers / large forms.

[6]: https://sites.google.com/a/orbeon.com/forms/doc/developer-guide/configuration-properties/configuration-properties-base
[8]: https://sites.google.com/a/orbeon.com/forms/doc/developer-guide/form-runner/importing-data
[10]: http://wiki.orbeon.com/forms/doc/developer-guide/xbl-components/recaptcha#TOC-Which-captcha-is-right-for-you
[11]: https://www.google.com/recaptcha/admin/create
[13]: https://sites.google.com/a/orbeon.com/forms/doc/developer-guide/xbl-components/recaptcha
[14]: https://github.com/orbeon/orbeon-forms/wiki/Form-Runner-~-Buttons-and-Processes#send
[15]: https://developers.google.com/chrome-developer-tools/docs/overview
[16]: http://getfirebug.com/
[17]: http://wiki.orbeon.com/forms/doc/contributor-guide/browser#TOC-IE-limit-of-31-CSS-files
[18]: https://github.com/orbeon/orbeon-forms/issues/635
[19]: https://github.com/orbeon/orbeon-forms/blob/master/src/resources/apps/fr/i18n/resources.xml
