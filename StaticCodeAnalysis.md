### Static Code Analysis
Gradle поддерживает следующие анализаторы кода:
- PMD
- FindBugs
- CheckStyle
- CodeNarc
- JDepend

Это ad-hoc инструменты. Они делают одноразовые срезы качества кода. Не видно динамики. 

Это решает:
- SonarQube

#### To use it:
Create filr gradle/codeQuality.gradle:
```
subprojects {
    apply plugin: 'findbugs'
    findbugs {
        ignoreFailures = true    // билд не валится при найденных ошибках
        toolVersion = '3.0.1'
    }

    apply plugin: 'pmd'
    pmd {
        toolVersion = '5.5.4'
    }
}
```
Add in build.gradle string:
```
apply from: file("${rootProject.projectDir}/gradle/codeQuality.gradle")
```




