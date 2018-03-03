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

