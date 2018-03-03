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
                maven {url 'http://artifactory.eu.compumark.com/artifactory/maven'}
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

## Custom tasks
Должно быть имя и тело таски.
Тело состоит из экшенов. Экшены можно задавать через **doLast** или **doFast**. DoLast - таск выполнится последним из всех тасков. doFirst- первым

```
task printParam {
        doLast { println 'Hello World!' }
}
```
To run this task: `gradle printParam`.

Output:
```
> Task :printParam
Hello World!
```
Custom task with variable passed as cli parameter
```
task printParam_cli {
        doLast { println givenParameter }
}
```
To run this task: `gradle printParam_cli -PgivenParameter='Hi!'`.

Output:
```
> Task :printParam_cli
Hi!
```
We can define this param in gradle.properties:
```
givenParameter=WeHaveDefault
```
And if we perform command `gradle printParam_cli` without any parameters, would be given param from properties file.

Output:
```
> Task :printParam_cli
WeHaveDefault
```

In case we configure task this way:
```
task printParam_cli {
        println givenParameter
}
```
This task will always perform, For example, `gradle clean`:
Output:
```
> Configure project :
Without doLast

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





