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
Output:
```
> Configure project :
Without doLast

```
