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

### Working with dependencies
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

