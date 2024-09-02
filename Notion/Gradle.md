---
modified: 2024-09-02T13:50:28+03:00
---
# Section 1: Build tools. История возникновения

![[images/Untitled 20 4.png|Untitled 20 4.png]]

![[images/Untitled 21 4.png|Untitled 21 4.png]]

![[images/Untitled 22 4.png|Untitled 22 4.png]]

##  Gradle Object Model

==Интерфейсы== ==Gradle Settings Project Task Action Script==

- Под капотом считывается build scripts и строится модель
- ==Settings -== ==для конфигурации== `==settings.==``gradle` и случит для ==конфигурации иерархии== ==Project== ==объектов.==
- ==Project -== служит для конфигурации `build.gradle`
- ==Task== для ==описания задачи== в файле.
- ==Action -== все ==таски разбиваются на части==.
- ==Script -== описывает все скрипты, которые выше. Создает их.

# Section 2: Gradle Lifecycle

![[images/Untitled 23 4.png|Untitled 23 4.png]]

## Gradle object

![[images/Untitled 24 4.png|Untitled 24 4.png]]

## Settings object

![[images/Untitled 25 4.png|Untitled 25 4.png]]

## Project object

![[images/Untitled 26 4.png|Untitled 26 4.png]]

## Task object

==**ИНИЦИАЛИЗАЦИЯ TASK**==

![[images/Untitled 27 4.png|Untitled 27 4.png]]

![[images/Untitled 28 4.png|Untitled 28 4.png]]

- ==Контейнер всех наших== `Task`
- `tasks` - ==обращение== к контейнеру

![[images/Untitled 29 4.png|Untitled 29 4.png]]

- ==Можно через== `Closure`

![[images/Untitled 30 4.png|Untitled 30 4.png]]

- ==Предпочтительный вариант== (==Лучше скобки и кавычки убрать==)

![[images/Untitled 31 4.png|Untitled 31 4.png]]

==АЛОГИЧНО==

![[images/Untitled 32 4.png|Untitled 32 4.png]]

==Можно ещё вот так==

![[images/Untitled 33 4.png|Untitled 33 4.png]]

  

## Action object

![[images/Untitled 34 4.png|Untitled 34 4.png]]

![[images/Untitled 35 4.png|Untitled 35 4.png]]

- Есть 2 способа добавить

![[images/Untitled 36 4.png|Untitled 36 4.png]]

# Section 3: Task graph

## Task graph

![[images/Untitled 21 4.png|Untitled 21 4.png]]

- ==Можно прописывать зависимости== `task` с ==помощью== `dependsOn(Objects…. paths)`

![[images/Untitled 37 4.png|Untitled 37 4.png]]

![[images/Untitled 38 4.png|Untitled 38 4.png]]

- Есть `finalizedBy` которая говорит, что ==сначала 1, потом 2==

![[images/Untitled 39 4.png|Untitled 39 4.png]]

- `mustRunAfter` - ==обязан== идти после второго
- `shouldRunAfter` - ==должен== идти после второго

![[images/Untitled 40 4.png|Untitled 40 4.png]]

  

![[images/Untitled 41 4.png|Untitled 41 4.png]]

![[images/Untitled 42 4.png|Untitled 42 4.png]]

# Section 4: Properties

==**Первый вариант**==

![[images/Untitled 43 4.png|Untitled 43 4.png]]

![[images/Untitled 44 4.png|Untitled 44 4.png]]

==**Второй вариант**== ==`**gradle.propertues**`==

- ==Там пишут не любые значения.==
- ==Грейдл проперти для сборки, jvm args, cashe==

  

# Section 5: Plugins

## Plugins

В `Gradle` плагины играют ключевую роль, ==расширяя функциональность сборки и автоматизации проекта.== Они предоставляют различные инструменты и возможности, чтобы упростить управление зависимостями, выполнение задач, конфигурацию проекта и многое другое. Например, ==плагины== ==могут добавлять новые задачи сборки, интегрировать с другими инструментами разработки, обрабатывать ресурсы, управлять версиями, выполнять тестирование и т. д.== Каждый плагин в `Gradle` предназначен для определенной задачи или интеграции, что делает его мощным инструментом для настройки и управления проектом.

- ==Можно добавить plugin== `from` из ==script==`.gradle`
- Можно ==создать кастомный== и ==указать либо название класса, либо путь== `plugin`

![[images/Untitled 45 4.png|Untitled 45 4.png]]

![[images/Untitled 46 4.png|Untitled 46 4.png]]

### **==Новый Plugin DSL==**

- Он содержит только индификаторы.

![[images/Untitled 47 4.png|Untitled 47 4.png]]

- На примере `plugin java`
- ==Приносит задачи== (голы из maven похожи)

==**Под капотом**==

- ==Наследуется==
- Есть метод `apply` + ==делегирует== ==другие== ==вызовы==

![[images/Untitled 48 4.png|Untitled 48 4.png]]

![[images/Untitled 49 4.png|Untitled 49 4.png]]

  

- Есть корневой лайфсайкл, который и приносит задачи

![[images/Untitled 50 4.png|Untitled 50 4.png]]

- ==Берем== `контейнер задач` и ==делаем== `registr()`
- `registr()` отличается от `create()` в том второй сразу создает и добавляет, а первый делает ==lazy==.

![[images/Untitled 51 4.png|Untitled 51 4.png]]

![[images/Untitled 52 4.png|Untitled 52 4.png]]

  

- Graph

![[images/Untitled 53 4.png|Untitled 53 4.png]]

## SourseSet

- Есть конфигурация с нашими исходниками, где они лежат

![[images/Untitled 54 4.png|Untitled 54 4.png]]

1. `**sourceSets**`: Этот блок позволяет вам настроить различные наборы источников для вашего проекта. Обычно он используется для определения структуры каталогов, содержащих исходные файлы вашего проекта.
2. `**main**` и `**test**`: Это имена источников. `**main**` обычно содержит основной код вашего приложения, а `**test**` содержит код юнит-тестов.
3. `**java**`: Этот блок используется для настройки исходных файлов Java в вашем проекте.
4. `**srcDirs**` и `**srcDir**`: Это свойства, указывающие на каталоги с исходными файлами. В `**srcDirs**` указываются несколько каталогов, а в `**srcDir**` - только один каталог.
5. `**generatedSourcesDir**`: Предполагается, что это переменная, содержащая путь к каталогу сгенерированных исходных файлов.

![[images/Untitled 55 4.png|Untitled 55 4.png]]

![[images/Untitled 56 4.png|Untitled 56 4.png]]

`ВОПРОС!11111111111`

  

# Section 6: Dependency Management

## Repositories

==Репозитории== представляют собой ==источники==, ==из которых== `Gradle` ==загружает библиотеки и плагины, необходимые для проекта.==

- `mavenCentral()`
- `flatDir` - локально на компе где-то

![[images/Untitled 57 4.png|Untitled 57 4.png]]

![[images/Untitled 58 4.png|Untitled 58 4.png]]

  

##  Dependency management

- Вызов метода add()

![[images/Untitled 59 3.png|Untitled 59 3.png]]

![[images/Untitled 60 3.png|Untitled 60 3.png]]

- `Конфигураця` в виде json
- `Исходный код`
- `pom` для подтягивания транзитивных зависимостей
- `jar`

![[images/Untitled 61 2.png|Untitled 61 2.png]]

## Dependency Configuration

![[images/Untitled 62 2.png|Untitled 62 2.png]]

![[images/Untitled 63 2.png|Untitled 63 2.png]]

Этот блок кода в файле `**build.gradle**` определяет настройки конфигураций зависимостей. В частности, в данном случае используется блок `**resolutionStrategy**`, который предоставляет стратегии разрешения конфликтов между зависимостями.

В вашем примере:

```Groovy
configurations {
    all {
        resolutionStrategy {
            cacheChangingModulesFor(0, 'seconds')
        }
    }
}
```

Вы устанавливаете стратегию кеширования для всех конфигураций (`**all**`).

- `**cacheChangingModulesFor(0, 'seconds')**`: Этот метод устанавливает время кеширования для изменяющихся модулей (changing modules) в 0 секунд, что означает, что Gradle не будет кешировать такие модули вообще.

Что такое "изменяющиеся модули" (changing modules)? Это зависимости, которые могут изменяться во время разработки, например, если вы работаете с локальным модулем исходного кода и он может измениться в любой момент.

Установка времени кеширования в 0 секунд гарантирует, что Gradle всегда будет проверять изменяющиеся модули на наличие обновлений при каждой сборке, что может быть полезно для предотвращения ошибок, связанных с использованием устаревших зависимостей. Однако это может сказаться на времени сборки проекта, поскольку Gradle будет постоянно проверять обновления зависимостей.

## Транзитивные зависимости

![[images/Untitled 64 2.png|Untitled 64 2.png]]

В `Gradle` есть дефолтная стратегия, которая берет версию выше всегда

![[images/Untitled 65 2.png|Untitled 65 2.png]]

## PlatformSupport

![[images/Untitled 66 2.png|Untitled 66 2.png]]

![[images/Untitled 67 2.png|Untitled 67 2.png]]

# Section 8: Multiproject builds

## Multiproject builds

![[images/Untitled 68 2.png|Untitled 68 2.png]]

![[images/Untitled 69 2.png|Untitled 69 2.png]]

- в root делать так плохо

![[images/Untitled 70 2.png|Untitled 70 2.png]]