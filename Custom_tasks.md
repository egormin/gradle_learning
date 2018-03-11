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
#### Custom task with variable passed as cli parameter.
Удобно, например, для девелоперов использовать дефолтный параметр, а для разных энвайронментов передавать параметр при запуске.
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
Or runJar could be:
```
task runJar {
        type Exec
        dependsOn jar
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
Можно запустить и без таска Jar:
```
task run (type: JavaExec, dependsOn: classes) {
        main 'net.egor.gradleTutorial.Main'
        classpath sourceSets.main.runtimeClasspath
        args 'Hello World'
}
```
В этом случае код будет собран и запущен в этом таске
Output:
```
> Task :run
[Hello World]
[hELLO wORLD]
```

### Conditions in tasks:
```
task hello2 {
        onlyIf { true } //any condition
} << {
        println 'It is true'
}
```
Output:
```
> Task :hello2
It is true
```
However if:
```
task hello2 {
        onlyIf { false } //any condition
} << {
        println 'It is true'
}
```
Will be no output.

And if we write:
```
task hello2 {
        onlyIf { true } //any condition
} << {
        println 'It is true'
}

hello2.enabled = false
```
Will be no output.

### Task for writing to a file:
```
task writeGreeting << {
        file('greeting.txt').text = 'Hello, people\n'
}
```
Output:
```
-bash-4.2# cat greeting.txt
Hello, people
```
Write to file only if text doesn't match:
```
task writeGreeting {
        onlyIf { file('greeting.txt').text != 'Hello, people\n' }
        doLast { file('greeting.txt').text = 'Hello, people\n' }
}
```
Or:
```
task writeGreeting {
        outputs.file file('greeting.txt')
} << {
        file('greeting.txt').text = 'Hello, people\n'
}
```





