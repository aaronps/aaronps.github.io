# Java

* Http servlet request get header is case insensitive
* Tomcat sessions are saved in work folder

## Servlets

`@WebServlet("/something/and/something/*")`
--> needs to have the /* at end if you want to get the path info

`request.getPathInfo()` -> null or "/" for base, then the rest for the thing

toPrint
`HttpServletResponse.getWriter().print()`

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
	* application.properties: spring.main,banner-mode=off
	* application.yml: spring.main.banner-mode: 'off'

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

### Old Spring

To be able to use the autoconfiguration it might be needed to add a line to beans.xml:
```xml
    <context:component-scan base-package="org.example.package"/>
```


Rest example
```java
@RestController
@RequestMapping("/path")
public class Name {
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




