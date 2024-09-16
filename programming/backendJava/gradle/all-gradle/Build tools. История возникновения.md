---
title: Build tools. История возникновения
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:28
modified: 2024-09-16T18:29:01+03:00
questions: 
notes: 
links: 
---

Build tools. История возникновения

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