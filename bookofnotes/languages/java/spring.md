# Spring


## to study

```java
// could use this for realtime add/remove listeners?
@Autowired KafkaListenerEndpointRegistry registry;
```

## Gradle config

```groovy

//-----------------------------
// using old buildscript method
// this method would be preffered if you want to use repo mirrors
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

//--------------------------------------------
// or new method which uses plugins.gradle.org
plugins {
    id 'org.springframework.boot' version '1.5.10.RELEASE'
    id 'java' // use java
    id 'war'  // or war
}
//--------------------------------------------

// add this so 'bootRun' will check for changes on src/main/resources
bootRun {
    addResources = true
}


dependencies {
    compile 'org.springframework.boot:spring-boot-devtools'
	compile 'org.springframework.boot:spring-boot-starter-web'
}



```