# gradle

Install:
> Download gradle-${version}-bin.zip, uncompress, add bin folder to path.

Configuration:

_Adding this config in `~/.gradle/init.gradle` file will make some tasks more conveniente, and faster access to repositories from China._

```groovy
allprojects {

	// task example
    task repositories {
		group 'help'
		description 'List the configured repositories and their values.' 

        doLast {
            println repositories.collect { "${it.name} = ${it.url}"  }
        }
    }
    
	task initSimple(type: InitBuild) {
		group 'build setup'
		description 'Initializes a gradle project without the wrapper.'
	}

    repositories {

		// create a repository for local publishing
		ivy {
			name "areponame"
			url "file:///path/to/the/local/repo"
		}

		// replace standard repositories with aliyun mirrors for fast China access
        all { ArtifactRepository repo ->
        
            if ( (repo.name == "BintrayJCenter") && (repo.url.toString() == "https://jcenter.bintray.com/") ) {
                repo.url = "http://maven.aliyun.com/nexus/content/repositories/jcenter/"
                println "Using Aliyun jcenter mirror: ${repo.url}"
            }
            if ( (repo.name == "MavenRepo") && ((repo.url.toString() == "https://repo1.maven.org/maven2/") || (repo.url.toString() == "https://repo.maven.apache.org/maven2/")) ) {
                repo.url = "http://maven.aliyun.com/nexus/content/repositories/central/"
                println "Using Aliyun maven mirror: ${repo.url}"
            }
        }
    }  
}
```

Basic commands:

```sh
# create project
gradle init --type <project type>

# create project without wrapper, using previosly defined 'initSimple' task
gradle initSimple --type <project type>

# initialize gradle wrapper on current project
gradle wrapper

# run multiple tasks
gradle task1 task2 task3
gradle buildFrontend war bootRepackage deploy

# run without daemon
gradle --no-daemon <task>

# stop daemon
gradle --stop

# check running daemons status
gradle --status

# run continuous, will rerun task when input changes
gradle --continuous <task>
gradle -t <task>



```

Project types:

* basic
* groovy-application
* groovy-library
* java-application
* java-library
* pom
* scala-library


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

## template idea

consider using Copy task for custom templates

```groovy
copy {
   into 'build/webroot'
   exclude '**/.svn/**'
   from('src/main/webapp') {
      include '**/*.jsp'
      filter(ReplaceTokens, tokens:[copyright:'2009', version:'2.3.1'])
   }
   from('src/main/js') {
      include '**/*.js'
   }
}
```

maybe use ~/.gradle/templates as base for downloaded
templates, could use zip and/or full folders (including git).

