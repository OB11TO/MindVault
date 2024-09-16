---
title: Goals
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:40
modified: 2024-09-16T17:40:29+03:00
questions: 
notes: 
links: 
---
### Goals

В Maven есть еще такое понятие как цель (goal). goal – это как бы  
цель запуска Maven.  

На деле плагин это Java-проект, который содержит классы (Goals), которые наследуются от AbstractMojo, который в свою очередь имплементирует интерфейс Mojo и переопределяет метод execute.

```Java
@Mojo( name = "clean", threadSafe = true )
public class CleanMojo extends AbstractMojo
{
@Override
public void execute() throws MojoExecutionException{}
```

![[images/Untitled 9 9.png|Untitled 9 9.png]]

- `==Можем вызвать ещё раз, помимо дефолтного.==` ==!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==

Если тебе нужно выполнить какие-нибудь нестандартные действия в определенной фазе, то всего лишь нужно добавить соответствующий плагин в pom.xml. В поле ==<execution> </execution>== выполнить goal и указать в какой фазе его выполнить, некоторые goals не привязаны к жизненному циклу, их нужно прописывать вручную. Чтобы узнать какие goals есть у фазы, необходимо вызвать `mvn фаза:help`

```Plain
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>имя-плагина</artifactId>
  <executions>
    <execution>
      <id>customTask</id>
      <phase>generate-sources</phase>
      <goals>
        <goal>pluginGoal</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```
