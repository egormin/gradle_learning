Add file gradle/releaseNotes.gradle
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

    ext.changesFile = file('changes.txt')
    inputs.file changesFile

    ext.bugs = []
    ext.features = []
    changesFile.eachLine { String line ->
        String bugSymbol = '#bug:'
        String featureSymbol = '#feature:'
        if (line.contains(bugSymbol)) {
            bugs << line.replace(bugSymbol, '')
        } else if (line.contains(featureSymbol)) {
            features << line.replace(featureSymbol, '')
        }
    }

    filter(org.apache.tools.ant.filters.ReplaceTokens, tokens: [bugs: bugs.join("\n"), features: features.join("\n")])
}

tasks.create('releaseNotes') {
    inputs.file copyTask
    outputs.dir destDir
}
```
Output:
```
Here is a list of notes regarding the latest release:

Bugs:
 1 Somethings is broken
 2 Somethings else is broken

Features:
```
Темплейт выглядит таким образом:
```
-bash-4.2# cat releaseNotes.tmpl.txt
Here is a list of notes regarding the latest release:

Bugs:
@bugs@

Features:
@features@
```
