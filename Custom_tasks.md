## Custom tasks
Должно быть имя и тело таски.
Тело состоит из экшенов. Экшены можно задавать через **doLast** или **doFast**. DoLast - таск выполнится последним из всех тасков. doFirst- первым

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
