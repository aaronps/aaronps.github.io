# Java

* `SocketTimeoutException` is a subclass of `InterruptedIOException`, take into
account you might not have interrupted it yourself.

```java
// Base64 (java8)
import java.util.Base64;
Base64.getEncoder().encode();
Base64.getDecoder().decode();

// Base64 and FastJSON library:
//	FastJSON will automatically encode and decode base64 when using 'getBytes'
//	and put(byte[]). This may depend on settings.

// join 2 arrays, java8 style
final String[] joinedArray = Stream.concat( Stream.of(array1),
											Stream.of(array2))
								   .toArray(String[]::new);

```

## Servlet

```java
// Add "/*" if you want to use req.getPathInfo() which will return:
//   null   -> if not using "/*" or if request is to "/something"
//   "/"    -> if using "/*" and request is to "/something/"
//   "/XXX" -> if using "/*" and request is to "/something/XXX" 
// Note: cannot use "/something*"
@WebServlet("/something/*")
public class MyServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
        
        // to set servlet response content type
        resp.setContentType("text/plain");

        // write something on the response with "out"
        final PrintWriter out = resp.getWriter();
        
        out.printf("PathInfo: %s\n", req.getPathInfo());
        
        // HttpServletRequest.getHeader() is case insensitive
        out.printf( "Insensitive header example: [%s] and [%s]\n",
                    req.getHeader("Host"),
                    req.getHeader("host"));
        
        // use setStatus to manually change the response code
        resp.setStatus(200);
    }

}
```

## JSP & el

```jsp
<%-- jsp comments (does not appear on rendered page) --%>
<%-- trimDirectiveWhitespaces: remove extra whitespace from output --%>
<%@ page contentType="text/html" pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>
<%
    // you might not like it, but you can do this
    final String aName = "SomeName";
    request.setAttribute("someAttribute", "Yes, some attribute");
%>
<!doctype html>
<html>
    <head>
        <title>JSP</title>
    </head>
    <body>
        <pre>
Old ways: Hello "<%= aName %>"

EL ways:
Print some attribute? &rarr; "${someAttribute}"
Context Path: "${pageContext.request.contextPath}"
Codition: ${empty param.x ? "you didn't set 'x' parameter" : param.x }
        </pre>
    </body>
</html>
```

## Tomcat

notes:
* `CATALINA_HOME` refers to the path to tomcat, the folder uncompressed from the zip files.
* `CATALINA_BASE` refers to the folder of the *active configuration*.
* Tomcat sessions are saved in work folder

beware:
* setting `antiResourceLocking="true"` on the context may inhibit reloading of JSP

tomee:
* Some non JavaEE jars might be scanned for EJB and other things which may
  fail to load or report errors on TomEE. To avoid this, you need to exclude
  those files from scanning, add `/WEB-INF/exclusions.list`, each line is a jar
  name prefix to skip.

## Spring

### configuration

`application.properties`

* http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html
* It can be application.yml too
* It can be next to the jar file
* It can be on src/resources/ but will be added to the generated jar file, use to set defaults, better use resources/config, which is valid too
* It can be put on the root folder of the project ( maybe where gradle is run ) so it will be picked by bootRun but not included on the final jar.
* to start with a different config file use parameter: --spring.config.name=someothername, it will add the .properties or .yml by itself, there is also spring.config.location to set the search path
	
### Notes

* Log files will autorotate when 10Mb and keep 
* To disable console output: logging.pattern.console= (nothing here)
* to disable spring banner:
	* application.properties: spring.main.banner-mode=off
	* application.yml: spring.main.banner-mode: 'off'
* If you use jackson, remove it from dependencies as it is added by springboot.

Run with different configuration file name:

	add parameter `--spring.config.name=newname`


```java
@SpringBootApplication
public class YourApplication {

	public static void main(String[] args) {
		new SpringApplicationBuilder(YourApplication.class)
            .properties("spring.config.name=yourconfigname")
            .run(args);
	}

}
```

### bootRun

1. Use apply plugin 'java' instead of war to generate executables
2. Use src/main/resources/{static,resources} for static files
3. To be able to reload resources on dev, config on build.gradle:	
```groovy
bootRun {
    addResources = true
}
```
4. run: `gradle bootRun`

### Run as a service

In `build.gradle`:

```groovy
springBoot {
	executable = true
}
```

Now it can run as normal executable, System V init script and systemd (with extra file):

```ini
[Unit]
Description=Your service description
After=syslog.target

[Service]
User=running-user
ExecStart=path/to/your/file
SuccessExitStatus=143
StandardOutput=null

[Install]
WantedBy=multi-user.target
```

### web

_initial notes_

From documentation: static files read from: `/static` or `/public` or `/resources` or `/META-INF/resources` in class path

So: if package as jar, can put them on `src/main/resources/desired_name_from_before`, but after testing,
`src/main/webapp` takes precedence if it exists, this seems irrelevant of using jar or war packaging.

regarding gradle::

If using java plugin, the project doesn't have a webpages area (at least on netbeans)


#### Using JSP

_springboot recommends to avoid jsp_

To avoid problems and due to some limitations:

	* Use tomcat (the default)
	* use *war* packaging
	* in gradle: `dependencies { providedRuntime 'org.apache.tomcat.embed:tomcat-embed-jasper' }`
	* Configure the viewResolver prefix and suffix


If also desires to use **jstl**:

	* in gradle: `dependencies { compile 'javax.servlet:jstl:1.2' }`
	* Configure viewResolver to use `org.springframework.web.servlet.view.JstlView.class`

### Old Spring

To be able to use the autoconfiguration it might be needed to add a line to beans.xml:
```xml
    <context:component-scan base-package="org.example.package"/>
```

```java
@RestController
@RequestMapping("/path")
public class RestExample {

	@GetMapping
	public X getX() {
		return new X();
	}
	
	@PutMapping(consumes = "application/json")
	public X putX(@RequestBody X newX) {
		return newX;
	}
}
```

## mybatis

```java
public interface SomeMapper {
    // use java constants from annotations, may need review
    @Select("select xxx from yyy where zzz = ${@full.java.class.or.enum@value}")
    List<String> listSomething();
}
```
