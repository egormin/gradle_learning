### Plugins
Может быть написан следующими способами:
- Build script. Будет видим в build файле.
- buildSrc/src/main/groovy. Будет виден всему проекту и его подпроектам.
- Standalone project. Может быть расшарен между проектами.

**Наша задача: вывести список плагинов, которые были применены к проекту**.

**Решение:**

Создаем файл `PluginsPrinterPlugin.groovy` в `buildSrc/src/main/groovy`:
```
import org.gradle.api.Plugin
import org.gradle.api.Project

public class PluginsPrinterPlugin implements Plugin<Project> {

    void apply(Project project) {
        project.task('printPlugins') << {
            println 'Current project has next list of plugins:'
            ext.plugins = project.plugins.collect { plugin ->
                plugin.class.simpleName
            }
            println plugins
        }
    }
}
```
В build.gradle добавим строки:
```
allprojects {
    apply plugin: PluginsPrinterPlugin
}
```
Для запуска используем команду:
```
gradle printPlugins
```

