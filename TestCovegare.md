### Test Covegare
File gradle/coverage.gradle:
```
apply plugin: "jacoco"

jacoco {
    toolVersion = "0.7.9"
}

check.dependsOn jacocoTestReport

jacocoTestReport {
    dependsOn 'test'
    reports {
        xml.enabled true
        csv.enabled false
        html.enabled true
    }
}

```
Add in build.gradle:
```
apply from: file("${rootProject.projectDir}/gradle/coverage.gradle")
```
