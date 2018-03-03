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

