### Rules
```
tasks.addRule('Check correctness of running tests'){ String taskName ->
    gradle.taskGraph.whenReady{
        Map<String, String> args = gradle.startParameter.systemPropertiesArgs
        gradle.taskGraph.allTasks.each { Task task ->
            if (task.name.contains('integTest') && !args.containsKey('profile')) {
                throw new org.gradle.api.tasks.StopExecutionException("Profile was not specified to run tests (-Dprofile=ci).")
            }
        }
    }
}
```
Если мы запустим командой `gradle build`, будет ошибка о том, что мы не добавили обязательный параметр:
```
* What went wrong:
Profile was not specified to run tests (-Dprofile=ci).
```
Если же запустить командой `gradle build -Dprofile=ci`, то всё заработает.

