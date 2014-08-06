## Status

[SINCE: Orbeon Forms 4.7]

This documentation is preliminary as of 2014-07-22.

## Rationale

The intent is to allow Java (and other Java Virtual Machine (JVM)-based languages) applications to easily embed forms produced with Form Builder within other pages.

## Configuration

You deploy Form Runner in a separate web app, which can be located in the same servlet container as your web app or in a separate or even remote servlet container.

Your own web app does the following:

- include `orbeon-embedding.jar` under `WEB-INF/lib`
- setup a filter in `WEB-INF/lib`
- call the embedding API when producing a page

SLF4J is also required for logging. If your application already uses SLF4J, you don't need to do anything. Otherwise, you must add slf4j-api.jar to your application's `WEB-INF/lib` folder.

*OPTIONAL: In addition, if you want to actually configure logging for the embedding library, you must add a logging adapter for SLF4j and the appropriate configuration file, for example for log4j.*

This is a typical filter configuration:

```xml
<!-- Declare and configure the embedding filter -->
<filter>
    <filter-name>orbeon-form-runner-filter</filter-name>
    <filter-class>org.orbeon.oxf.fr.embedding.servlet.ServletFilter</filter-class>
    <init-param>
        <param-name>form-runner-url</param-name>
        <param-value>http://localhost:8080/orbeon</param-value>
    </init-param>
    <init-param>
        <param-name>orbeon-prefix</param-name>
        <param-value>/orbeon</param-value>
    </init-param>
</filter>
<!-- Any JSP resource is processed by the filter -->
<filter-mapping>
    <filter-name>orbeon-form-runner-filter</filter-name>
    <url-pattern>*.jsp</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
<!-- This ensures that Orbeon resources are proxied appropriately -->
<filter-mapping>
    <filter-name>orbeon-form-runner-filter</filter-name>
    <url-pattern>/orbeon/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

And here is an example of embedding a form from a JSP page:

```jsp
<%@ page
    pageEncoding="utf-8"
    contentType="text/html; charset=UTF-8"
    import="org.orbeon.oxf.fr.embedding.servlet.API" %>
<!DOCTYPE HTML>
<html>
<body>
    <%
        API.embedFormJava(
            request,
            out,
            "orbeon",
            "bookshelf",
            "new",
            null,
            null
        );
    %>
</body>
</html>
```

## How it works

The embedding implementation:

- makes an HTTP or HTTPs request to Form Runner to retrieve the HTML to embed when you call the API
- appropriately rewrites URLs in the HTML returned by Form Runner
- associates session cookies
- proxies requests for resources, Ajax calls and file uploads to Form Runner

## Limitations

- navigation between pages, such as the Form Runner Edit and Review pages, is not supported
- embedding Form Builder is currently not supported
- embedding multiple forms is known to work in most cases, but has at least one known issue with repeated grid menus