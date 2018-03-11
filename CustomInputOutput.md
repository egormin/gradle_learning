Create file gradle/releaseNotes.gradle

```
ext.destDir = new File(buildDir, 'releaseNotes')
ext.releaseNotesTemplate = file('releaseNotes.tmpl.txt')

tasks.create(name: 'copyTask', type: org.gradle.api.tasks.Copy) {
    from releaseNotesTemplate
    into destDir

    doFirst {
        if (!destDir.exists()) {
            destDir.mkdir()
        }
    }
    rename { String fileName ->
        fileName.replace('.tmpl', '')
    }
}

tasks.create('releaseNotes') {
    inputs.file copyTask
    outputs.dir destDir
}
```
Add string in build.gradle:
```
apply from: file("${rootProject.projectDir}/gradle/releaseNotes.gradle")
```
Start with command:
```
gradle releaseNotes
```

Первый раз таск будет выполнен, второй раз будет пропущен, т.к. уже выполнялся, т.е. incremental build.
