### To run integrational tests we create file:
`gradle/integTest.gradle` and modify file `build.gradle`

gradle/integTest.gradle:
```
sourceSets {
    integTest {
        compileClasspath += main.output + test.output
        runtimeClasspath += main.output + test.output
    }
}

configurations {
    integTestCompile.extendsFrom testCompile
    integTestRuntime.extendsFrom testRuntime
}

task integTest(type: Test){
    testClassesDir = sourceSets.integTest.output.classesDir
    classpath = sourceSets.integTest.runtimeClasspath
    shouldRunAfter 'test'
}
```
build.gradle:
```
subprojects { project ->
    apply plugin: 'java'
    apply from: file("${rootProject.projectDir}/gradle/integTest.gradle")  // It was added
    
    dependencies {
        compile "org.slf4j:slf4j-api:$slf4jVersion"

        testCompile 'org.mockito:mockito-core:2.7.18',
                'junit:junit:4.12'
    }
}
```
