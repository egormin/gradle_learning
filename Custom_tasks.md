## Custom tasks
Простейший таск выглядит так:
```
task hello
```
Output (`gradle tasks --all`):
```
Other tasks
-----------
compileJava - Compiles main Java source.
compileTestJava - Compiles test Java source.
hello
processResources - Processes main resources.
processTestResources - Processes test resources.
```
Добавим таску группу и описание:
```
task hello(group: 'greeting', description: 'Greets you')
```
Output:
```
Greeting tasks
--------------
hello - Greets you
```
Можно написать и так, будет то же самое:
```
task hello {
        group 'greeting'
        description 'Greets you'
}
```

Должно быть имя и тело таски.
Тело состоит из экшенов. Экшены можно задавать через **doLast** или **doFist**. DoLast - таск выполнится последним из всех тасков. doFirst- первым

```
task printParam {
        doLast { println 'Hello World!' }
}
```
To run this task: `gradle printParam`.

Output:
```
> Task :printParam
Hello World!
```
Tasks queues:
```
task printParam {
        doLast { println 'First' }
        doLast { println 'Second' }
        doFirst { println 'Third' }
}
```
Output:
```
> Task :printParam
Third
First
Second
```
Short notation of task with doLast:
```
task helloShort << { println 'short notation' }
```
Or:
```
task printParam {
        doLast { println 'First' }
        doLast { println 'Second' }
        doFirst { println 'Third' }
} << {
        println 'something'
}
```
Output:
```
> Task :printParam
Third
First
Second
something
```
Variables in custom tasks:
```
task varsTesting {
        ext.myVar = 'Hey, how\'s going'        // configuration phase
        doLast { println "Greeting: $myVar" }  // execution phase
}
```
Output:
```
> Task :varsTesting
Greeting: Hey, how's going
```
Custom task with variable passed as cli parameter
```
task printParam_cli {
        doLast { println givenParameter }
}
```
To run this task: `gradle printParam_cli -PgivenParameter='Hi!'`.

Output:
```
> Task :printParam_cli
Hi!
```
We can define this param in gradle.properties:
```
givenParameter=WeHaveDefault
```
And if we perform command `gradle printParam_cli` without any parameters, would be given param from properties file.

Output:
```
> Task :printParam_cli
WeHaveDefault
```

In case we configure task this way:
```
task printParam_cli {
        println givenParameter
}
```
This task will always perform, For example, `gradle clean`:
Output (In configuration phase, not execution phase):
```
> Configure project :
Without doLast

```

### To run java from gradle:
```
jar {
        from configurations.compile.collect { zipTree it }
        manifest.attributes 'Main-Class': 'net.egor.gradleTutorial.Main'
}

task runJar(type: Exec, dependsOn: jar) {
        executable 'java'
        args '-jar', "$jar.archivePath", 'Hello World'
}
```
Здесь `$jar` берется из таска `jar`, `archivePath` это свойство таска jar. Строка выглядит как `java -jar <path> Hello World`
Output:
```
Starting process 'command 'java''. Working directory: /GITs/Gradle_tutorial Command: java -jar /GITs/Gradle_tutorial/build/libs/Gradle_tutorial.jar Hello World
Successfully started process 'command 'java''
[Hello World]
[hELLO wORLD]
```
