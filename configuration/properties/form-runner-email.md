# Email configuration properties

<!-- toc -->

## Connection to the SMTP server

The following properties control the connection to the SMTP server:

- `host`: required SMTP host name
- `port`: optional SMTP port override. If not specified, the defaults are:
    * plain SMTP: 25
    * TLS: 587
    * SSL: 465
- `encryption`:
    * blank: none (plain SMTP)
    * `tls`: use TLS
    * `ssl`: use SSL
- `username`: SMTP username (required if TLS or SSL is used, optional otherwise)
- `credentials`: SMTP password

```xml
<property
    as="xs:string"
    name="oxf.fr.email.smtp.host.*.*"
    value="my.outgoing.smtp.server.org"/>

<property
    as="xs:string"
    name="oxf.fr.email.smtp.port.*.*"
    value="587"/>

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
```

## Email addresses properties

- `from`: sender email address(es) appearing in the email sent
- `to`: recipient email address(es) of the email sent
- `cc`:
    - SINCE Orbeon Forms 2017.1
    - carbon copy recipient email address(es) of the email sent
- `bcc`:
    - SINCE Orbeon Forms 2017.1
    - blind carbon copy email address(es) of the email sent

List of emails are space- or comma- separated.

```xml
<property
    as="xs:string"
    name="oxf.fr.email.from.*.*"
    value="john@example.org"/>

<property
    as="xs:string"
    name="oxf.fr.email.to.*.*"
    value="mary@example.org,nancy@example.org"/>
    
<property
    as="xs:string"
    name="oxf.fr.email.cc.*.*"
    value="mary@example.org,nancy@example.org"/>

<property
    as="xs:string"
    name="oxf.fr.email.bcc.*.*"
    value="mary@example.org,nancy@example.org"/>
```

## Attachment properties

- `attach-pdf`: whether the PDF representation is attached to the email
- `attach-tiff`: whether the TIFF representation is attached to the email
- `attach-xml`:  whether the XML data is attached to the email

```xml
<property
    as="xs:boolean"
    name="oxf.fr.email.attach-pdf.*.*"
    value="true"/>

<property
    as="xs:boolean"
    name="oxf.fr.email.attach-tiff.*.*"
    value="true"/>

<property
    as="xs:boolean"
    name="oxf.fr.email.attach-xml.*.*"
    value="true"/>
```

[SINCE Orbeon Forms 2016.1]

The following property controls whether file and image form attachments are attached to the email.

- `all`: all form attachments are included (this is the default)
- `none`: no form attachments is included
- `selected`: only form attachments selected in the Form Builder with "Include as Email Attachment" are included
    
```xml
<property
    as="xs:string"
    name="oxf.fr.email.attach-files.*.*"
    value="all"/>
```
    
[SINCE Orbeon Forms 2018.1]

The following properties control the name of the PDF, TIFF and XML attachments:

- `oxf.fr.email.pdf.filename`:
    - filename of the PDF attachment, when present
- `oxf.fr.email.tiff.filename`
    - filename of the TIFF attachment, when present
- `oxf.fr.email.xml.filename`
    - filename of the XML attachment, when present
    
The property contains an XPath expression which generates the filename. The expression runs in the context of the current
form data but does *not* have a access to controls. Only a limited set of Form Runner XPath functions can be used, in
particular:

- `fr:form-title()`
- `fr:app-name()`
- `fr:form-name()`
- `fr:form-version()`
- `fr:document-id()`
- `fr:mode()`
- `fr:is-readonly-mode()`
- `fr:is-design-time()`

*NOTE: Control values must be extracted by searching for element values within the XML document. In the future, we hope
to provide a function for that purpose.* 

```xml
<property as="xs:string" name="oxf.fr.email.pdf.filename.*.*">
    concat(
        fr:form-title(),
        ' - ',
        //case-id,
        '.pdf'
    )
</property>

<property as="xs:string" name="oxf.fr.email.tiff.filename.*.*">
    concat(
        fr:form-title(),
        ' - ',
        //case-id,
        '.tiff'
    )

<property as="xs:string" name="oxf.fr.email.xml.filename.*.*">
    concat(
        fr:form-title(),
        ' - ',
        //case-id,
        '.xml'
    )
</property>
```
    
## Email subject and body

```xml
<property 
    as="xs:string"
    name="oxf.fr.resource.*.*.en.email.subject"
    value="Here is your confirmation: "/>

<property 
    as="xs:string"
    name="oxf.fr.resource.*.*.en.email.body"
    value="Hi, here is an email from Orbeon Forms!"/>
```