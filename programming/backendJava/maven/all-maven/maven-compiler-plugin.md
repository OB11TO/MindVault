---
title: maven-compiler-plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:41
modified: 2024-09-16T17:42:10+03:00
questions: 
notes: 
links: 
---

### maven-compiler-plugin

Самый популярный плагин, позволяющий управлять версией компилятора и  
используемый практически во всех проектах, – это компилятор  
`maven-compiler-plugin`.

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.2</version>
    <configuration>
        <source>1.11</source>
        <target>1.13</target>
        <encoding>UTF-8</encoding>
    </configuration>
</plugin>
```

Параметр `source` позволяет нам задать версию Java для наших исходников. Параметр `target`  
– версию Java-машины, под которую нужно скомпилировать классы. Если  
версия кода или Java-машины не задана, то по умолчанию используется  
параметр 1.3  

Наконец параметр `encoding` позволяет указать кодировку Java-файлов. Мы указали `UTF-8`. Сейчас практически все исходники хранятся в кодировке `UTF-8`. Но если этот параметр не указан, то выберется текущая кодировка операционной системы. Для Windows – это кодировка `Windows-1251`.

Плагин `maven-compiler-plugin` имеет три цели (goals):

- `compiler:compile` – компиляция исходников, по умолчанию связана с фазой compile
- `compiler:testCompile` – компиляция тестов, по умолчанию связана с фазой test-compile.
- `compiler:help` - вывод информации о целях

Также можно указать список аргументов, которые будут переданы javac-компилятору в командной строке:

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.2</version>
    <configuration>
        <compilerArgs>
            <arg>-verbose</arg>
            <arg>-Xlint:all,-options,-path<arg>
        </compilerArgs>
    </configuration>
</plugin>
```
