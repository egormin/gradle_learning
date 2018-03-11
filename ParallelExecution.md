### Parallel Execution
Например, мы имее 3 модуля:
```
include ':common',
        ':services',
        ':integration'
```
Комманда `gradle test` выполнит тесты последовательно для всех модулей. Чтобы выполнить их параллельно, можно выполнить:
```
gradle test --parallel
```
Or:
```
gradle test --parallel --max-workers=2
```
По умолчанию gradle сам подбирает оптимальное число.
