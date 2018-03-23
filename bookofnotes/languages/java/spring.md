# Spring


## to study

```java
// could use this for realtime add/remove listeners?
@Autowired KafkaListenerEndpointRegistry registry;
```

## Gradle config

### Plugin

_I always add the plugin and the devtools_

Using **buildscript** method to add plugins we can control which repository to use
```groovy
buildscript {
    repositories { jcenter() }
	
    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:1.5.10.RELEASE"
    }
}

apply plugin: 'org.springframework.boot'
// use java
apply plugin: 'java'
// or war
apply plugin: 'war'
```

Using the **plugins** method
```groovy
plugins {
    id 'org.springframework.boot' version '1.5.10.RELEASE'
    id 'java' // use java
    id 'war'  // or war
}
```

### Config
```groovy
// add this so 'bootRun' will check for changes on src/main/resources
bootRun {
    addResources = true
}


dependencies {
    // add this to use devtools, very usefull for development
    compile 'org.springframework.boot:spring-boot-devtools'

    // for web development
    compile 'org.springframework.boot:spring-boot-starter-web'

    // websockets
    compile 'org.springframework.boot:spring-boot-starter-websocket'
    
    // kafka
    compile 'org.springframework.kafka:spring-kafka'

}

// using undertow instead of tomcat: 1-disable tomcat
configurations {
    compile.exclude module: "spring-boot-starter-tomcat"
}

// using undertow instead of tomcat: 2-depend on undertow
dependencies {
    compile 'org.springframework.boot:spring-boot-starter-undertow'
}

```

## Main

```java
// basic
@SpringBootApplication
public class SomeApplication {
    public static void main(String[] args) {
		SpringApplication.run(SomeApplication.class, args);
	}
}

// scan for beans on packages not under this
@SpringBootApplication(scanBasePackages = {"some.other.package"})

// better mode
@SpringBootApplication
public class SomeApplication {
    public static void main(String[] args) {
		new SpringApplicationBuilder(DemoApplication.class)
            // configure banner mode
            .bannerMode(Mode.OFF)

            // set one or more properties, this will change the config file name
            .properties("spring.config.name=demoapplication",

                        // this usefull when running under tomcat, put config files in tomcat/conf
                        "spring.config.location=classpath:/,classpath:/config/,file:./,file:./config/,file:./conf/")

            // disable builtin servlet container
            .web(false)

            // activate some profiles
            .profiles("profile1", "profile2")

            // run the app
            .run(args);
	}
}

// initialize on a servlet container (not the embedded one)
// can use the same main as previous mode to support both modes
@SpringBootApplication
public class SomeApplication extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(SomeApplication.class)
                          // parameters same as main
                          .properties("...")
                          .profiles("...")
                          ;
    }

    public static void main(String[] args) {
        // same as the previous main
    }
}


```

## Components

```java

@Component
public class SomeComponent {

    // Injects the bean automatically
    @Autowired
    AnotherComponent anotherComponent;

    // without autowired
    private AnotherBean anotherBean;

    // can inject using setter
    @Autowired
    public void setAnotherBean(AnotherBean anotherBean) {
        this.anotherBean = anotherBean;
    }

    // or can inject if there is only one constructor which requires parameters
    // if more than one constructor, requires Autowired annotation
    // also: can mix autowired members and constructors
    public SomeComponent(AnotherBean anotherBean) {
        this.anotherBean = anotherBean;
    }

    // this also works.
    @Autowired
    public void anyNameGoes(AnotherBean anotherBean) {
        this.anotherBean = anotherBean;
    }

    @PostConstruct
    public void initialize() {
        // this will run after constructor and injection
    }

    @PreDestroy
    public void destroy() {
        // ...
    }

}

```

## Controllers

```java

// fix @PathVariable missing extension on controllers
public class X extends WebMvcConfigurerAdapter {
    @Override configurePathMatch(PathMatchConfigurer matcher)
    {
        matcher.setUseRegisteredSuffixPatternMatch(true);
    }
}

```
