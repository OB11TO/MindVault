---
title: Добавление стороннего repository в pom.xml
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:38
modified: 2024-09-16T17:39:07+03:00
questions: 
notes: 
links: 
---

### Добавление стороннего repository в pom.xml

Подключить к вашему проекту сторонний репозиторий, то это можно сделать так же просто, как и добавление зависимостей:

У ==каждого репозитория есть 3 вещи:== `**Key/ID, Имя**` `и` `**URL**`. Имя можно указать любое – оно для твоего удобства, ID тоже для ваших внутренних нужд, фактически тебе нужно указать только URL.

```XML
<repositories>
 
  <repository>
  	<id>public-javarush-repo</id>
  	<name>Public JavaRush Repository</name>
  	<url>http://maven.javarush.com</url>
  </repository>
 
  <repository>
  	<id>private-javarush-repo</id>
  	<name>Private JavaRush Repository</name>
  	<url>http://maven2.javarush.com</url>
  </repository>
 
</repositories>
```
