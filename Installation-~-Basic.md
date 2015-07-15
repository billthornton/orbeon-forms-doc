> [[Home]] ▸ [[Installation]]

## Software requirements

Orbeon Forms 4 runs on any platform that supports:

* Java 6, 7 (recommended), or 8
* a Servlet 2.5 container or greater (such as [Apache Tomcat][1] 6, 7 (recommended) or greater)

## Hardware requirements

Orbeon Forms is best installed on hardware with:

* a reasonably fast CPU, e.g. as of early 2011:
    * Intel Core i7 or better (desktop-grade)
    * Intel Xeon (server-grade)
    * As of 2015, we don't recommend AMD CPUs, which tend to be 2-4 times slower than Intel CPUs per core.
* at least 1.5 GB of available RAM

## Java virtual machine configuration

Configure the Java VM with:

* `-Xmx` option for dedicated Java heap memory:
    * on a development machine: at least 512 MB of Java heap: `-Xmx512m`
    * on a production machine: at least 1 GB of Java heap: `-Xmx1024m`
* ` -XX:MaxPermSize` for "permgen" space:
    * use at least: `-XX:MaxPermSize=256m`

## License installation (Orbeon Forms PE only)

* If you are running Orbeon Forms CE, you don't need to install a license file.
* If you are running Orbeon Forms PE:
    * complete the steps for your application server below
    * you can obtain a full licence from Orbeon, or get a [trial license](http://demo.orbeon.com/orbeon/fr/orbeon/register/new)
    * before starting your servlet container, copy your license file under the Orbeon Forms WAR file as:
    ```
    WEB-INF/resources/config/license.xml
    ```

With Orbeon Forms 4.1 and newer, you can also place license.xml file under the user's home directory. For example, on Unix systems:

```
~/.orbeon/license.xml
```

Orbeon Forms first searches for the license file within the WAR, and if not found attempts to find it under the home directory.

The benefit of this approach is that you don't have to find where the WAR file is deployed in your container, or to uncompress and recompress the WAR file with the license.

_NOTE:  Orbeon Forms uses Java's `System.getProperty("user.home")` to identify the user's home directory.__  This corresponds to the user running the servlet container and not necessarily to the user of the developer or system administrator._

## Logging configuration

This step is optional.

Orbeon Forms has a logging configuration file under WEB-INF/resources/config/log4j.xml. By default, logging information is output to a file path relative to the directory where you start your application server.

```xml
<appender name="SingleFileAppender" class="org.apache.log4j.FileAppender">
    <param name="File" value="../logs/orbeon.log"/>
    <param name="Append" value="false" />
    <param name="Encoding" value="UTF-8"/>
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d{ISO8601} %-5p %c{1} %x - %m%n"/>
    </layout>
</appender>
```

You can change this by modifying the file parameter. Notes that on Windows, you must use forward slashes:

```xml
<appender name="SingleFileAppender" class="org.apache.log4j.FileAppender">
    <param name="File" value="C:/My Path/To/Logs/orbeon.log"/>
    <param name="Append" value="false" />
    <param name="Encoding" value="UTF-8"/>
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d{ISO8601} %-5p %c{1} %x - %m%n"/>
    </layout>
</appender>
```

The benefit of changing this configuration is that you know exactly where the file is stored. This can be really handy when trying to troubleshoot issues.

## Specific steps for your container / app server

- [[Tomcat|Installation ~ Tomcat]]
- [[JBoss|Installation ~ JBoss]]
- [[WebLogic|Installation ~ WebLogic]]
- [[WebSphere|Installation ~ WebSphere]]
- [[GlassFish|Installation ~ GlassFish]]
