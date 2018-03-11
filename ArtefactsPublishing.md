### Artefacts Publishing
Create gradle/publishing.gradle
```
apply plugin: 'maven-publish'

publishing {
    repositories {
        maven {
            url "http://artifactory.vagrantshare.com/artifactory/libs-release-local"
            credentials {
                username 'admin'
                password 'password'
            }
        }
    }

    publications {
        mavenJava(MavenPublication) {
            groupId "name.webdizz.${rootProject.name}"
            version = uploadVersion
            from components.java
        }
    }
}
```
Add in build.gradle string:
```
apply from: file("${rootProject.projectDir}/gradle/publishing.gradle")
```
