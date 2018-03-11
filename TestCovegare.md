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
To run:
```
gradle check
```
Reports will be in build/reports folder.

### Integrations test coverage
For it gradle/coverage.gradle should looks like:
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

task jacocoIntegrationTestReport(type: JacocoReport) {
    dependsOn integTest
    sourceSets sourceSets.main
    executionData integTest

    reports {
        xml {
            enabled true
            destination "$buildDir/reports/jacoco/integTest/jacocoIntegTestReport.xml"
        }
        csv.enabled false
        html {
            destination "$buildDir/reports/jacoco/integTest/html"
        }
    }
}

check.dependsOn jacocoIntegrationTestReport
```
