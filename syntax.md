#### Write text
```
println 'Hello, World!'
```

#### Apply plugin
```
apply plugin: 'java'
```

#### Change wrapper version in build.gradle
```
task wrapper(type: Wrapper) {
	gradleVersion = '3.4.1'
}
```

#### For multimodule projects settings.gradle syntax
```
include ':one',':two'
```


## Working with dependencies
**Транзитивная звисимость** - это зависимость зависимости.

To define repositories where gradle will get libs.
```
allprojects { currproject ->
        repositories {
                mavenLocal()
                mavenCentral()
                jcenter()
                maven {url 'http://artifactory.mycompany.com/artifactory/maven'}
        }
}
```
`allprojects` - это значит для всех проектов, в т.ч. и вложенных.

`mavenLocal` - это локальная дирректория m2.

`mavenLocal` - mavenCentral репозиторий.

`mavenLocal` - jcenter репозиторий.

`maven {}`   - artifactory или nexus репозиторий.

#### Dependencies defining for all subprojects
```
subprojects {
        apply plugin: 'java'
        dependencies {
                compile 'org.slf4j:slf4j-api:1.7.25'
                testCompile 'org.mockito:mockito-core:2.7.18',
                        'junit:junit:4.12'
        }
}
```
#### To define dependencies for subproject we should in build.gradle of those project add
```
dependencies {
	compile 'org.projectlombok:lombok:1.16.14'
}
```
Or:
```
dependencies {
	compile group:'org.projectlombok', name: 'lombok', version:: '1.16.14'
}
```
#### Show all dependencies
```
gradle dependencies
```
#### To exclude some dependency 
На уровне модуля (т.е. исключим по artifact id).
```
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3
```

```
dependencies {
                compile 'org.slf4j:slf4j-api:1.7.25'
                testCompile 'org.mockito:mockito-core:2.7.18',
                'junit:junit:4.12'
                testCompile ('junit:junit:4.12') {
                      exclude module: 'hamcrest-core'
                }
        }
```
After:
```
\--- junit:junit:4.12
```
#### To exclude group of dependencies
Before:
```
+--- org.mockito:mockito-core:2.7.18
|    +--- net.bytebuddy:byte-buddy:1.6.11
|    +--- net.bytebuddy:byte-buddy-agent:1.6.11
|    \--- org.objenesis:objenesis:2.5
```
```
 dependencies {
                compile 'org.slf4j:slf4j-api:1.7.25'
                testCompile ('org.mockito:mockito-core:2.7.18'){
                        exclude group: 'net.bytebuddy'
                }
         }

```
After:
```
+--- org.mockito:mockito-core:2.7.18
|    \--- org.objenesis:objenesis:2.5
```

## gradle.properties using
We can define different variables in this file.

**gradle.properties:**
```
mockitoVersion=2.7.10
```
To use it in **build.gradle:**
```
dependencies {
	testCompile "org.mockito:mockito-core:$mockitoVersion"
}
```
## Parameters defining
Define path were keep src files for code and for tests:
```
sourceSets {
        main.java.srcDirs = ['scr/main']
	test.java.srcDirs = ['scr/test']
}
```
or so:
```
sourceSets.main.java.srcDirs = ['source']
sourceSets.test.java.srcDirs = ['test']
```
If we run our jar file with command: 
```
java -jar build/libs/Gradle_tutorial.jar "Hello Gradle" "Hello Egor"
```
We get an error about manifest. To avoid this, we need use construction in build.gradle:
```
jar {
        manifest.attributes "Main-Class": "net.egor.gradleTutorial.Main"
}
```
Now if we run the command `java -jar ...` wi'll get:
```
[Hello Gradle, Hello Egor]
```
If we added some dependencies and built jar file it will not work because of we have no our dependencies in jar archive.
To add them:
```
jar {
        from configurations.compile.collect { zipTree it }
        manifest.attributes "Main-Class": "net.egor.gradleTutorial.Main"
}
```
It will be added as tree in jar file.

To archive any folder:
```
jar {
        from configurations.compile.collect { zipTree it }
        from configurations.compile.collect { fileTree("src") }
        manifest.attributes "Main-Class": "net.egor.gradleTutorial.Main"
}
```
Will bi archived src as well. Path is from build.gradle path.

Now output will be:
```
[Hello Gradle, Hello Egor]
[hELLO gRADLE, hELLO eGOR]
```







