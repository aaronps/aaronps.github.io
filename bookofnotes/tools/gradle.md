# gradle

## Install and configure

1. Download gradle-${version}-bin.zip.
2. Uncompress somewhere, add its "bin" folder to path
3. Create `~/.gradle/init.gradle` with following contents for China

```groovy
allprojects {

	// flysnow.org is not good, recomend to not use wrapper and download the binary manually.
    // task wrapper(type: Wrapper) {
    //    distributionUrl = "http://mirrors.flysnow.org/gradle/gradle-${gradleVersion}-bin.zip"
    // }
    
    task showRepositories {
        doLast {
            println repositories.collect { "${it.name} = ${it.url}"  }
        }
    }
    
    repositories {
        all { ArtifactRepository repo ->
        
            if ( (repo.name == "BintrayJCenter") && (repo.url.toString() == "https://jcenter.bintray.com/") ) {
                repo.url = "http://maven.aliyun.com/nexus/content/repositories/jcenter/"
                println "Using Aliyun jcenter mirror: ${repo.url}"
            }
            
            if ( (repo.name == "MavenRepo") && (repo.url.toString() == "https://repo1.maven.org/maven2/") ) {
                repo.url = "http://maven.aliyun.com/nexus/content/repositories/central/"
                println "Using Aliyun maven mirror: ${repo.url}"
            }
        }
    }  
}
```

### Create project

    gradle init --type <type>

Where type might be
* basic
* groovy-application
* groovy-library
* java-application
* java-library
* pom
* scala-library

### Init without wrapper

By default, `gradle init` depends on wrapper, if you dont' want it, either delete
wrapper files after create, modify init task to not depend on wrapper or create 
a new tasks of type InitBuild, for example on the global configuration file:

	task initSimple(type: InitBuild) {}

This task will work exactly as init but without the wrapper.

### Initialize wrapper

As 2017-12-02 it seems mirrors.flysnow.org might be down, maybe avoid the wrapper altogether.

    gradle wrapper --gradle-distribution-url http://mirrors.flysnow.org/gradle/gradle-4.1-bin.zip
	- or -
	gradle wrapper

### add a repository

	repositories { jcenter() }

### add dependencies

* For 'java-library':

	dependencies { testImplementation 'junit:junit:4.12' }

* For 'java':

	dependencies { testCompile'junit:junit:4.12' }

### basic test file

	import static org.junit.Assert.*;
	import org.junit.Test;
	public class SomeTest {
		// it is important to be public!
		@Test public void firstTest() {
			// use the Assert.* funs
			fail("some message");
		}
	}

note, @Test has a timeout=milliseconds

### simple custom tasks

```groovy
task release {
	group war.group
	description 'Some special build task'

	dependsOn buildFrontend
	dependsOn war
}
```

### war ignore files in other folders than webapp

```groovy
war {
	// ...

	rootSpec.exclude '**/*.some.thing'
}
```

### war versioned name

```groovy
war {
	// ...
	archiveName "${baseName}##${version}.${extension}"
}
```

## SVN, GIT, Import to IDE

Only need to commit:

* src/
* gradle/
* build.gradle
* gradlew
* gradlew.bat
* settings.gradle

Then import as gradle project.

Can generate project files for eclipse:

	apply plugin: 'eclipse'

then run task 'eclipse'.

Can generate project files for intellij-idea:

	apply plugin: 'idea'

run task 'idea'.

## Notes

**If your jdk version is different with the global jre version, SET JDK_HOME**

`src/main/resources` will be added verbatim to the jar file.

To tell it to download the sources, apply plugin eclipse, then generate the eclipse files, it will download the sources automatically, later you can delete the eclipse files but the sources will be picked by netbeans

### Gradle with some other directory structure

	sourceSets {
		main {
			java {
				srcDirs = [ 'src' ]
			}
		}
	}

### Repositories with maven mirror

    repositories {
        maven {
            url "http://maven.aliyun.com/nexus/content/repositories/central/" 
        }
    }

### using local repository (ivy)

_note: it can be on global `init.gradle`_

	repositories {
		ivy {
			name "somereponame"
			url "file:///path/to/repo"
		}
	}

### push to repo

	version = "0.1.2"
	group = "com.aaronps"
	
	artifacts {
		archives jar
	}

	uploadArchives {
		repositories {
			add project.repositories.somereponame
		}
	}

### wrapper mirror for China

_note: mirrors.flysnow.org might not work_

edit `gradle/wrapper/gradle-wrapper.properties`

    distributionUrl=http://mirrors.flysnow.org/gradle/gradle-3.5-bin.zip

(need to check) If the import of project or download shows errors, go to $HOME/.gradle and try to remove wrapper

### To include other projects as dependency

`settings.gradle`

	include ':ref-project-name'
	project(':ref-project-name').projectDir = file('path/to/project')

`build.gradle`

	dependencies {
		compile project(':ref-project-name')
	}

### To show the Test output

	test {
	    dependsOn cleanTest
	    testLogging.showStandardStreams = true
	}
	
	tasks.withType(Test) {
		systemProperty 'java.util.logging.SimpleFormatter.format', '%1$tY-%1$tm-%1$td %1$tH:%1$tM:%1$tS %4$s | %2$s: %5$s%6$s%n'
	}

### war

There is providedCompile and providedRuntime for dependencies, these will be added to the war if
using bootRepackage.

## Errors

### 'default' configuration not found

This is mostly probable because we are 'including' another project and gradle cannot find its build.gradle

## Old

buildscript for spring and maven mirror

	buildscript {
		repositories {
			maven {
				url "http://maven.aliyun.com/nexus/content/repositories/central/" 
			}
		}
	    dependencies {
	        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.6.RELEASE")
	    }
	}
