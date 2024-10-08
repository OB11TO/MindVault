---
title: Action object
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:31
modified: 2024-09-18T15:23:14+03:00
questions: 
notes: 
links: 
---

## Action object

![[images/Untitled 34 4.png|Untitled 34 4.png]]

![[images/Untitled 35 4.png|Untitled 35 4.png]]

- Есть 2 способа добавить
![[images/Untitled 36 4.png|Untitled 36 4.png]]



В Gradle **`Action`** <mark class="hltr-red">в контексте задач (Task)</mark> представляет собой некоторое <mark class="hltr-yellow">действие, которое выполняется, когда задача запускается</mark>. Это механизм добавления пользовательского поведения или выполнения определённых шагов в задаче. В простых терминах, `Action` — это фрагмент кода, который будет выполняться, когда задача будет вызвана.

### Зачем нужен `Action` в Task?

Задачи в Gradle могут выполнять разнообразные действия: компиляцию кода, копирование файлов, упаковку артефактов и многое другое. Чтобы настроить задачу и добавить к ней логику, используется `Action`. Он позволяет разработчику указать, какие конкретно шаги должны быть выполнены в рамках задачи. `Action` можно рассматривать как тело задачи.

### Как написать задачу в Gradle?

Задачи в Gradle можно создавать несколькими способами:

1. **Простой способ создания задачи** — это использование метода `task`, за которым следует определение `Action` через замыкание (closure) или лямбда-функцию.

Пример:

```groovy
task hello {
    doLast {
        println 'Hello, Gradle!'
    }
}

```

#### Объяснение:

- **`task hello`** — определение новой задачи с именем `hello`.
- **`doLast { ... }`** — добавление действия, которое выполнится, когда задача будет вызвана. В данном случае, будет просто выведена строка "Hello, Gradle!".
- **`doFirst { ... }`** — если нужно, чтобы действие выполнялось перед другими, можно использовать `doFirst`.

### Что такое `doFirst` и `doLast`?

Gradle поддерживает возможность добавлять несколько `Action` к одной задаче. Есть два метода для добавления действий:

- **`doFirst`** — выполняется перед всеми остальными действиями задачи.
- **`doLast`** — выполняется после всех других действий задачи.

#### Пример использования:

```groovy
task buildApp {
    doFirst {
        println 'Starting build...'
    }
    
    doLast {
        println 'Build finished!'
    }
}

```

В этом примере:

- Сначала будет выведено сообщение **"Starting build..."**.
- Затем выполнится основное действие задачи (если оно есть).
- И в конце будет выведено сообщение **"Build finished!"**.

### Пример задачи с логикой:

Рассмотрим пример задачи, которая выполняет простое копирование файлов:

```groovy
task copyFiles(type: Copy) {
    from 'src/resources'
    into 'build/resources'
    doLast {
        println 'Files copied successfully!'
    }
}

```

#### Объяснение:

- **`task copyFiles(type: Copy)`** — определяет задачу типа `Copy`, которая встроена в Gradle для копирования файлов.
- **`from 'src/resources'`** — указывает исходный каталог для копирования файлов.
- **`into 'build/resources'`** — указывает каталог назначения, куда нужно копировать файлы.
- **`doLast { ... }`** — действие, которое выполняется в конце задачи, выводя сообщение о завершении.

### Важные моменты про `Action` и задачи:

1. **Кратность действий**: К одной задаче можно добавить несколько `Action`. Они будут выполняться в порядке, в котором были добавлены.
2. **Динамическое поведение**: Используя условные операторы или циклы внутри `Action`, можно динамически управлять логикой выполнения задач.
3. **Типы задач**: Gradle имеет множество встроенных типов задач (например, `Copy`, `Exec`, `JavaCompile`), которые можно расширять с помощью `Action`.

### Пример более сложной задачи:

```groovy
task processFiles {
    doFirst {
        println 'Checking files before processing...'
    }

    doLast {
        file('output.txt').withWriter { writer ->
            writer.write('Processed data')
        }
        println 'Files processed and output.txt created!'
    }
}

```


Здесь задача **`processFiles`**:

1. Сначала выводит сообщение перед выполнением.
2. Создаёт файл `output.txt` и записывает в него строку "Processed data".
3. Выводит сообщение об успешной обработке файлов.

### Как писать задачу:

1. **Определите имя задачи** с помощью ключевого слова `task`.
2. **Добавьте `Action`** через `doFirst` и/или `doLast` для определения порядка выполнения действий.
3. **Используйте методы** для доступа к файлам, каталогам, параметрам и логике.
4. **При необходимости используйте встроенные типы задач**, такие как `Copy`, для расширения функциональности.

Пример простой пользовательской задачи:

```groovy
task customTask {
    doLast {
        def currentDate = new Date()
        println "Custom task executed on: $currentDate"
    }
}

```


В итоге, `Action` позволяет гибко управлять поведением задач, добавляя в них логику, которая будет выполняться в нужный момент.