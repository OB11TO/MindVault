---
modified: 2024-09-02T13:53:23+03:00
---
[https://www.youtube.com/watch?v=kOi4HFW9uMM](https://www.youtube.com/watch?v=kOi4HFW9uMM)

### Быстрый гайд

![[images/Untitled 151.png|Untitled 151.png]]

- Вот как это выглядит `Plugin` - ==просто проект==, где `goal` это ==классы==
    
    ![[images/Untitled 1 12.png|Untitled 1 12.png]]
    
    1. `**Environment Variables (переменные среды)**``:` Это переменные, которые определяются в рамках операционной системы и доступны для всех процессов, работающих в этой среде. Они используются для настройки окружения исполнения программы. Это могут быть такие переменные, как `**PATH**` (определяющая пути к исполняемым файлам), `**HOME**` (определяющая домашний каталог пользователя), и многие другие. В Java коде вы можете получить доступ к значениям переменных среды с помощью метода `**System.getenv()**`.
        
        1. `**JVM Arguments (аргументы JVM)**`: Это параметры, которые передаются виртуальной машине Java (JVM) при ее запуске и используются для настройки ее поведения и параметров выполнения программы Java. Это могут быть такие аргументы, как `**Xmx**` (максимальный размер кучи), `**Xms**` (начальный размер кучи), `**D**` (определение системного свойства), и так далее. Аргументы JVM определяются при запуске Java-приложения через командную строку или в скрипте запуска.
        
        Различие между ними заключается в том, что переменные среды определяются на уровне операционной системы и доступны для всех процессов, в то время как аргументы JVM используются конкретно для настройки виртуальной машины Java и параметров выполнения Java-приложения.
        
    
      
    

### Общие сведения

![[images/Untitled 2 11.png|Untitled 2 11.png]]

![[images/Untitled 3 10.png|Untitled 3 10.png]]

  

  

==Maven предложил универсальный открытый стандарт на основе== ==XML====,== в котором с помощью различных тегов описывается, что это за проект, как его нужно собирать и какие у него зависимости.

==Структура проекта описывается в файле== `pom.xml,` который должен  
находиться в корневой папке проекта. Содержимое проектного файла имеет  
следующий вид:  

```XML
<project>
        <!—Описание текущего проекта -->
        <groupId>...</groupId>
        <artifactId>...</artifactId>
        <packaging>...</packaging>
        <version>...</version>
        <properties>
            <!-- Секция свойств -->
        </properties>
        <repositories>
            <!-- Секция репозиториев -->
        </repositories>
        <dependencies>
            <!-- Секция зависимостей -->
        </dependencies>
        <build>
            <!-- Секция сборки -->
        </build>
</project>
```

```XML

//Информация о версии стандарта maven-проекта
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

 //   Информация о самом проекте
 
   <groupId>example.com</groupId>
   <artifactId>example</artifactId>
   <version>1.0-SNAPSHOT</version>

// groupId – пакет, к которому принадлежит приложение, с добавлением имени домена;
// artifactId – уникальный строковый ключ (id проекта);
// version – версия проекта.

//  Информация об используемых библиотеках
   <dependencies>
       <dependency>
           <groupId>commons-io</groupId>
           <artifactId>commons-io</artifactId>
        <version>2.6</version>
        </dependency>
   </dependencies>

</project>
```

![[images/Untitled 4 10.png|Untitled 4 10.png]]

### Pom.xml наследуется от Super-Pom.

Получить ==результирующий== `pom.xml` можно ==вызвав== `mvn help: effective-pom` ==обязательно в той же директории==, где лежит `pom.xml проекта`.

```XML
<project>
  <modelVersion>4.0.0</modelVersion>

  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>http://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>http://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>

  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>

      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>

      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>

</project>
```

### Секция build в pom.xml

==Секция build не является обязательной – Maven умеет собирать проект и без нее.== ==Но если ты хочешь настроить сборку более-менее сложного проекта, то понимание как там все устроено тебе пригодится.==

Данная секция (ниже) содержит основную информацию по сборке: где расположены Java-файлы, файлы ресурсов, какие плагины используются, куда складывать собранный проект.

```XML
<build>
        <finalName>projectName</finalName>
        <sourceDirectory>${basedir}/src/java</sourceDirectory>
        <outputDirectory>${basedir}/targetDir</outputDirectory>
        <resources>
                <resource>
                <directory>${basedir}/src/java/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                </includes>
                </resource>
        </resources>
        <plugins>
                . . .
        </plugins>
    </build>
```

Основных тегов тут четыре:

- `<finalName>` ==задает имя результирующего файла сборки== (jar, war, ear..), который создается в фазе package. Если параметр не задан, то используется значение ==по умолчанию — artifactId-version.==
- `<sourceDirectory>` ==позволяет переопределить месторасположения файлов с исходным кодом==. По умолчанию файлы располагаются в директории ${basedir}/src/main/java, но можно указать и любое другое место.
- `<outputDirectory>` ==задает директорию, куда компилятор будет сохранять результаты компиляции== – `*.class файлы`. По умолчанию определено значение target/classes.
- `<resources>` ==вложенные в нее тэги== \<resource> ==определяют местоположение файлов ресурсов. Файлы ресурсов при сборке просто копируются в директорию outputDirectory.== Значение по умолчанию директории с ресурсами равно src/main/resources.

### Переменные properties в Maven

Часто встречающиеся параметры Maven позволяет вынести в переменные.  
Это очень полезно, когда нужно чтобы в разных частях pom-файла параметры  
совпадали. Например, в переменную можно вынести версию Java, версии  
библиотеки, пути к определенным ресурсам.  

Для этого есть специальная секция в `pom.xml – <properties>`, в ней и объявляются переменные. Общий вид переменной такой:

`<имя-переменной>значение</имя-переменной>`

Пример:

```XML
<properties>
    <junit.version>5.2</junit.version>
    <project.artifactId>new-app</project.artifactId>
    <maven.compiler.source>1.13</maven.compiler.source>
    <maven.compiler.target>1.15</maven.compiler.target>
</properties>
```

Для обращения к переменным используется другой синтаксис:

`${имя-переменной}`

Там, где написан такой код, Maven подставит значение переменной.

Пример:

```XML
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <finalName>${project.artifactId}</finalName>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>2.3.2</version>
        <configuration>
            <source>${maven.compiler.source}</source>
            <target>${maven.compiler.target}</target>
        </configuration>
    </plugin>
</build>
```

При описании проекта в pom-файле можно использовать предопределенные переменные. Их можно условно разделить на несколько групп:

- Встроенные свойства проекта;
- Свойства проекта;
- Настройки.

Встроенных свойств проекта всего два:

|   |   |
|---|---|
|Свойство|Описание|
|${basedir}|корневой каталог проекта, где располагается `pom.xml`|
|${version}|версия артефакта; можно использовать `${project.version}` или `${pom.version}`|

На свойства проекта можно ссылаться с помощью префиксов `«project»` или `«pom»`. Их у нас четыре:

|   |   |
|---|---|
|Свойство|Описание|
|${project.build.directory}|`«target»` директория проекта|
|${project.build.outputDirectory}|`«target»` директория компилятора. По умолчанию `«target/classes»`|
|${project.name}|наименование проекта|
|${project.version}|версия проекта|

Доступ к свойствам `settings.xml` можно получить с помощью префикса `settings`. Имена могут быть любыми – берутся из `settings.xml`. Пример:

```Plain
${settings.localRepository} задает путь к локальному репозиторию.
```

### Dependencies

![[images/Untitled 5 10.png|Untitled 5 10.png]]

Если хочешь ==добавить в свой Maven-проект какую-нибудь библиотеку==, тебе просто нужно добавить ее в pom-файл, в раздел dependencies.

```XML
<dependencies>
 
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
	<version>5.3.18</version> 
  </dependency>

  <dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.0.0.Final</version>
  </dependency>

</dependencies>
```

### Области видимости dependencies(scope)

==**compile**== : Зависимый эффективный диапазон ==по  
умолчанию  ==. Если эффективная область зависимости не указана явно при  
определении зависимости, по умолчанию принимается действующая область  
зависимости. Такая зависимость  
==допустима при компиляции, запуске и  тестировании.

==**provided**== : Он ==действителен во время компиляции и  
тестирования, но недействителен во время выполнения  
==. Например:  
servlet-api, когда проект запущен, контейнер уже предоставлен, поэтому  
Maven не нужно вводить его снова и снова.  

==**runtime**== : ==Действителен при запуске и тестировании,  
но недействителен при компиляции кода.  
==Например: реализация драйвера  
JDBC, компиляция кода проекта требует только интерфейса JDBC,  
предоставляемого JDK, а конкретный драйвер JDBC, реализующий  
вышеуказанный интерфейс, требуется только при тестировании или запуске  
проекта.  

==**test**== : ==Действительно только во время тестирования==, например: JUnit.

```XML
<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>3.2.1.RELEASE</version>
            <scope>test</scope>
        </dependency>
```

==**system**== : ==Он действителен во время компиляции и  
тестирования, но недействителен во время выполнения.  
==`Отличие от provided`  
заключается в том, что ==при использовании зависимостей на уровне системы  
путь к зависимому файлу должен быть явно указан через элемент  
  
`systemPath`. Поскольку такие зависимости не разрешаются через репозиторий  
Maven и часто привязаны к собственной системе, что может сделать сборку  
непереносимой, ее следует использовать с осторожностью. Элемент  
systemPath может ссылаться на переменные среды. Например:  

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

### Транзитивные зависимости

- Вот, что есть при добавлении транзитивных зависимостей,
- `sha1` - удаленный
- ==`pom`== - как раз таки для ==транзитивных зависимостей==

![[images/Untitled 6 9.png|Untitled 6 9.png]]

- ==Есть нюансы, что транзитивные зависимости не совпадают и начинаются несовместимости версий.==
- `Maven` ==опускает некоторые зависимости, если главная зависимость идет выше==. Поэтому будет плохо, если будет иначе. В `Gradle` ==всегда берется версия выше.==
- `Решить можно тэгом`

![[images/Untitled 7 9.png|Untitled 7 9.png]]

### Жизненный цикл Maven, плагины, Goals плагинов

![[images/Untitled 8 9.png|Untitled 8 9.png]]

### Жизненный цикл

Все команды maven делятся на три группы – lifecycles. Их называют **жизненными циклами**, так как они задают порядок фаз, которые выполняются во время сборки или определенного жизненного цикла, потому что не все действия Maven являются сборкой.

Всего жизненных циклов три `clean`, `default`, `site`

Формально, список фаз можно изменять, хотя необходимость в этом бывает редко.

  

Порядок фаз `**clean**` **(**используется для того, чтобы полностью очистить папку target):

1. `pre-clean`
2. `**clean**`
3. `post-clean`

По умолчанию если вызвать goal фазы default, то clean не вызывается, но можно настроить конфигурацию и добавить вызов _(Подробнее об этом в разделе Goals.)_ либо через terminal вызвать mvn clean install.

```XML
<plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.2.0</version>
        <executions>
          <execution>
            <goals>
              <goal>clean</goal>
            </goals>
            <phase>validate</phase>
          </execution>
        </executions>
      </plugin>
```

  

Порядок фаз **defaul**t:

1. `**validate**` Подтверждает, является ли проект корректным и вся ли необходимая информация доступа для завершения процесса сборки
2. `initialize` Инициализирует состояние сборки, например, различные настройки.
3. `generate-sources` Включает любой исходный код в фазу компиляции.
4. `process-sources` Обрабатывает исходный код (подготавливает). Например, фильтрует определённые значения.
5. `generate-resources` Генерирует ресурсы, которые должны быть включены в пакет.
6. `process-resources` Копирует и отправляет ресурсы в указанную директорию. Это фаза перед упаковкой.
7. `**compile**` Комплирует исходный код проекта.
8. `process-classes` Обработка файлов, полученных в результате компиляции. Например, оптимизация байт-кода Java классов.
9. `generate test-sources` Генерирует любые тестовые ресурсы, которые должны быть включены в фазу компиляции.
10. `process-test-sources` Обрабатывает исходный код тестов. Например, фильтрует значения.
11. `test-compile` Компилирует исходный код тестов в указанную директорию тестов.
12. `process-test-classes` Обрабатывает файлы, полученные в результате компиляции исходного кода тестов.
13. `**test**` Запускает тесты, используя приемлемый фреймворк юнит-тестирования (например, Junit).
14. `prepare-package` Выполняет все необходимые операции для подготовки пакет, непосредственно перед упаковкой.
15. `**package**` Преобразует скомпилированный код и пакет в дистрибутивный формат. Такие как JAR, WAR или EAR.
16. `pre-integration-test` Выполняет необходимые действия перед выполнением интеграционных тестов.
17. `integration-test` Обрабатывает и распаковывает пакет, если необходимо, в среду, где будут выполняться интеграционные тесты.
18. `post-integration-test` Выполняет действия, необходимые после выполнения интеграционных тестов. Например, освобождение ресурсов.
19. `**verify**` Выполняет любые проверки для подтверждения того, что пакет пригоден и отвечает критериям качества.
20. `**install**` Устанавливает пакет в локальный репозиторий, который может быть использован как зависимость в других локальных проектах.
21. `**deploy**` Копирует финальный пакет (архив) в удалённый репозиторий для, того, чтобы сделать его доступным другим разработчикам и проектам.

  

Порядок фаз `**site**` используется для создания докладов, документации, развёртывания и т.д.:

1. `pre-site`
2. `**site**`
3. `post-site`
4. `site-deploy`

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

### Плагины

![[images/Untitled 10 7.png|Untitled 10 7.png]]

`Зависимости (dependency)` — это те библиотеки, которые непосредственно используются в вашем проекте для компиляции кода или его тестирования.  
Плагины (plugin) же используются самим мавеном при сборке проекта или для каких-то других целей (например деплоймент)  

Так как плагины являются такими же артефактами, как и зависимости, то  
они описываются практически так же. Вместо раздела dependencies –  
plugins, вместо dependency – plugin, вместо repositories –  
pluginRepositories, repository – pluginRepository.  

Пример:

```XML
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>2.6</version>
    </plugin>
</plugins>
```

Разные плагины вызываются Maven'ом на разных стадиях жизненного цикла. Так проект, описывающей десктопное Java-приложение на swing, имеет стадии жизненного цикла отличные от тех, что характерны для разработки web-приложения (war).

Или, например, когда выполняется команда “mvn test”, инициируeтся целый набор шагов в жизненном цикле проекта: “process-resources”, “compile”, “process-classes”, “process-test-resources”, “test-compile”, “test”. Упоминания этих фаз вы можете видеть в выводимых Maven-ом сообщениях:

```Plain
[INFO] Scanning for projects...
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources)     @ javarush ---
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile)      @ javarush
[INFO] --- maven-resources-plugin:2.6:testResources         (default-testResources) @ javarush ---
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile)          @ javarush ---
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test)         @ javarush ---
[INFO] Surefire report directory:           t:\ projects\javarush\target\surefire-reports
```

### Плагины сборки

|   |   |   |
|---|---|---|
||Плагин|Описание|
|1|maven-compiler-plugin|Управляет Java-компиляцией|
|2|maven-resources-plugin|Управляет включением ресурсов в сборку|
|3|maven-source-plugin|Управляет включением исходного кода в сборку|
|4|maven-dependency-plugin|Управляет процессом копирования библиотек зависимостей|
|5|maven-jar-plugin|Плагин для создания итогового jar-файла|
|6|maven-war-plugin|Плагин для создания итогового war-файла|
|7|maven-surefire-plugin|Управляет запуском тестов|
|8|buildnumber-maven-plugin|Генерирует номер сборки|

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

### maven-resources-plugin

Если ты собираешь web-приложение, то у тебя будет просто куча  
различных ресурсов в нем. Это jar-библиотеки, jsp-сервлеты, файлы  
настроек. Ну и конечно же это куча статических файлов типа  
`html`, `css`, `js`, а также различных картинок.

По умолчанию Maven при сборке проекта просто скопирует все ваши файлы из папки `src/main/resources` в директорию target. Если же ты хочешь внести изменения в это поведение, то тебе поможет плагин `maven-resources-plugin`.

```XML
<plugin>
    <artifactId>maven-resources-plugin</artifactId>
    <version>2.6</version>
    <executions>
        <execution>
            <id>copy-resources</id>
            <phase>validate</phase>
            <goals>
                <goal>copy-resources</goal>
            </goals>
            <configuration>
                <outputDirectory>
                   ${basedir}/target/resources
                </outputDirectory>
                <resources>
                    <resource>  инструкции по копированию ресурса 1
															 </resource>
                    <resource>  инструкции по копированию ресурса 2
															 </resource>
                    <resource>  инструкции по копированию ресурса N 
																</resource>
                </resources>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**Фильтрация ресурсов**

Ресурсы плагина можно задавать не только в виде файлов, а сразу в  
виде директорий. Более того, к директории можно добавить маску, которая  
задает какие именно файлы из нее будет включены в данный ресурс.  

```XML

            <resource>
                <directory>src/main/resources/images</directory>
                <includes>
                     <include>**/*.png</include>
                </includes>
            </resource>
```

Если ты хочешь исключить какие-нибудь файлы, можешь воспользоваться тегом `exclude`

```XML
<resource>
    <directory>src/main/resources/images</directory>
    <includes>
        <include>**/*.png</include>
    </includes>
    <excludes>
         <exclude>old/*.png</exclude>
    </excludes>
</resource>
```

Плагин умеет заглядывать внутрь файлов (если они текстовые, конечно). И, например, добавить в файл `application.properties` нужную версию сборки. Чтобы плагин обрабатывал содержимое файла, нужно указать ему параметр `<filtering>true</filtering>`.

```XML
<resource>
    <directory>src/main/resources/properties</directory>
    <filtering>true</filtering>
    <includes>
        <include>**/*. properties </include>
    </includes>
</resource>
```

  

### maven-source-plugin

Еще один полезный плагин – `maven-source-plugin` позволяет включать в сборку исходный код ваших java-файлов.

Кроме web-приложений, с помощью Maven собирается  
очень большое количество библиотек. Очень много Java-проектов следуют  
концепции open-source и распространяются среди Java-сообщества со своими  
исходниками.  

В любом сложном проекте исходники могут храниться в нескольких местах.

Во-вторых, часто используется генерация исходников на основе xml-спецификаций, такие исходники тоже нужно включать в сборку.

Ну и в-третьих, ты можешь решить не включать какие-нибудь особо секретные файлы в вашу сборку.

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.2.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <phase>verify</phase>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### maven-dependency-plugin

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <outputDirectory>
            ${project.build.directory}/lib/
        </outputDirectory>
    </configuration>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals>
                <goal>copy-dependencies</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

В этом примере прописано дефолтное поведение плагина – копирование библиотек в директорию `${project.build.directory}/lib`.

В секции execution прописано, что **плагин будет вызван во время фазы сборки – package**, goal – copy-dependences.

В целом, у этого плагина довольно большой набор целей, вот самые популярные из них:

|   |   |   |
|---|---|---|
||||
|1|dependency:analyze|анализ зависимостей (используемые, неиспользуемые, указанные, неуказанные)|
|2|dependency:analyze-duplicate|определение дублирующихся зависимостей|
|3|dependency:resolve|разрешение (определение) всех зависимостей|
|4|dependency:resolve-plugin|разрешение (определение) всех плагинов|
|5|dependency:tree|вывод на экран дерева зависимостей|

Также в разделе configuration можно задать дополнительные параметры:

|   |   |   |
|---|---|---|
||||
|1|outputDirectory|Директория, в которую будут копироваться зависимости|
|2|overWriteReleases|Флаг необходимости перезаписывания зависимостей при создании релиза|
|3|overWriteSnapshots|Флаг необходимости перезаписывания неокончательных зависимостей, в которых присутствует SNAPSHOT|
|4|overWriteIfNewer|Флаг необходимости перезаписывания библиотек с наличием более новых версий|

Пример:

```XML

<configuration>
    <outputDirectory>
         ${project.build.directory}/lib/
    </outputDirectory>
    <overWriteReleases>false</overWriteReleases>
    <overWriteSnapshots>false</overWriteSnapshots>
    <overWriteIfNewer>true</overWriteIfNewer>
 </configuration>
```

  

### maven-jar-plugin

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>2.4</version>
    <configuration>
        <includes>
            <include>**/properties/*</include>
        </includes>
        <archive>
           <manifestFile>src/main/resources/META-INF/MANIFEST.MF</manifestFile>
        </archive>
    </configuration>
</plugin>
```

с его помощью можно указать, какие файлы попадут в библиотеку, а какие – нет. С помощью тегов `<include>` в секции `<includes>` можно задать список директорий, чей контент нужно добавить в библиотеку.

Во-вторых, каждая jar-библиотека должна иметь манифест (файл **MANIFEST.MF**).  
Плагин сам положит его в нужное место библиотеки, вам всего лишь нужно  
указать, по какому пути его взять. Для этого используется тег  
`<manifestFile>`.

И наконец, плагин может самостоятельно сгенерировать манифест. Для этого вместо тега `<manifestFile>` вам нужно добавить тег `<manifest>` и в нем указать данные для будущего манифеста. Пример:

```XML
<configuration>
    <archive>
        <manifest>
            <addClasspath>true</addClasspath>
            <classpathPrefix>lib/</classpathPrefix>
            <mainClass>ru.javarush.MainApplication</mainClass>
        </manifest>
    </archive>
</configuration>
```

Тег `<addClasspath>` определяет необходимость добавления в манифест `CLASSPATH`.

Тег `<classpathPrefix>` позволяет дописывать префикс (в примере lib) перед каждым ресурсом. Определение префикса в `<classpathPrefix>` позволяет размещать зависимости в отдельной папке. Можно размещать библиотеки внутри другой библиотеки.

И наконец, тег `<mainClass>` указывает на главный исполняемый класс. Java-машина может запустить программу, которая задана не только Java-классом, но и jar-файлом и именно для такого случая нужен главный стартовый класс.

### Плагин генерации номера сборки buildnumber-maven-plugin

Очень часто jar-библиотеки и war-файлы включают в себя информацию с  
названием проекта и его версией, а также версией сборки. Мало того, что  
это полезно для управления зависимостями, так еще и упрощает  
тестирование: понятно, в какой версии библиотеки ошибка исправлена, а в  
какой – добавлена.  

Чаще всего эту задачу решают так – создают специальный файл `application.properties`,  
который содержит всю нужную информацию и включают его в сборку. Так же  
можно настроить сценарий сборки так, чтобы данные из этого файла  
перекочевывали в  
`MANIFEST.MF`.

у Maven есть специальный плагин, который может генерировать такой  
application.properties файл. Для этого нужно создать такой файл и  
заполнить его специальными шаблонами данных. Пример:  

```Plain
# application.properties
app.name=${pom.name}
app.version=${pom.version}
app.build=${buildNumber}
```

Значения всех трех параметров будут подставляться на этапе сборки.

Параметры `pom.name` и `pom.version` будут браться прямо из `pom.xml`. А для генерации уникального номера сборки в Maven есть специальный плагин – `buildnumber-maven-plugin`.

```XML
<packaging>war</packaging>
<version>1.0</version>
<plugins>
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>buildnumber-maven-plugin</artifactId>
        <version>1.2</version>
        <executions>
            <execution>
                <phase>validate</phase>
                <goals>
                    <goal>create</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <revisionOnScmFailure>true</revisionOnScmFailure>
            <format>{0}-{1,date,yyyyMMdd}</format>
            <items>
                 <item>${project.version}</item>
                 <item>timestamp</item>
            </items>
        </configuration>
    </plugin>
</plugins>
```

В примере выше происходят три важные вещи. Во-первых, указан сам плагин для задания версии сборки. Во-вторых, указано, что он будет запускаться во время фазы validate (самая первая фаза) и генерировать номер сборки – `${buildNumber}`.

И в-третьих, указан формат этого номера сборки, который склеивается из нескольких частей. Это версия проекта `project.version` и текущее время заданное шаблоном. Формат шаблона задается Java-классом `MessageFormat`.

  

### maven-surefire-plugin

Описан в разделе “Тестирование в Maven”

### Тестирование в Maven

Еще один важный момент работы Maven – это фаза тестирования. Она выполнится, если ты запустишь фазу test, package, verify или любую другую, которая идет после них.

Общий шаблон имен тестов:

- */Test*.java
- */*Test.java
- */*TestCase.java

Результаты тестирования в виде отчетов в форматах .txt и .xml сохраняются в директории ${basedir}/target/surefire-reports.

### Настройка тестирования

Вариантов запуска тестов обычно бывает очень много, поэтому  
разработчики Maven сделали специальный плагин, в параметрах к которому  
можно задать всю детальную информацию по тестированию. Плагин называется  
Maven Surefire Plugin и доступен  
[по ссылке](https://maven.apache.org/surefire/maven-surefire-plugin/).

В примере мы указали плагину, что ему нужно запустить единственный тестовый класс – Sample.java.

```XML
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
    	<version>2.12.4</version>
    	<configuration>
        	<includes>
                <include>Sample.java</include>
        	</includes>
    	</configuration>
	</plugin>
</plugins>
```

### Как исключить определенные тесты

Чтобы запустить проект на тестирование, нужно выполнить команду  
mvn test. Но чаще возникает потребность исключить из тестирования  
некоторые тесты. Например, они могут быть поломаны, выполняться слишком  
долго, или по любым другим причинам.  

Во-первых, можно просто сказать Maven пропустить тесты при выполнении фазы сборки. Пример: `mvn clean package -Dmaven.test.skip=true`  
  

Во-вторых, в конфигурации плагина можно отключить выполнение тестов:

```XML

<configuration>
    <skipTests>true</skipTests>
</configuration>
```

Ну и в-третьих, тесты можно исключить с помощью тега \<exclude>. Пример:

```XML

<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
    	<version>2.12.4</version>
    	<configuration>
        	<excludes>
           	<exclude>**/TestFirst.java</exclude>
	           <exclude>**/TestSecond.java</exclude>
    	</excludes>
    	</configuration>
    </plugin>
</plugins>
```

### Запуск интеграционных тестов

Запуск интеграционных тестов осуществляется с помощью Maven Failsafe Plugin

[https://maven.apache.org/surefire/maven-failsafe-plugin/index.html](https://maven.apache.org/surefire/maven-failsafe-plugin/index.html)

|   |   |
|---|---|
|Goal|Description|
|[failsafe:help](https://maven.apache.org/surefire/maven-failsafe-plugin/help-mojo.html)|Display help information on maven-failsafe-plugin. Call `mvn failsafe:help -Ddetail=true -Dgoal=<goal-name>` to display parameter details.|
|[failsafe:integration-test](https://maven.apache.org/surefire/maven-failsafe-plugin/integration-test-mojo.html)|Run integration tests using Surefire.|
|[failsafe:verify](https://maven.apache.org/surefire/maven-failsafe-plugin/verify-mojo.html)|Verify integration tests ran using Surefire.|

Использовать goals необходимо в такой последовательности:

```XML
<project>
      [...]
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>3.1.0</version>
            <executions>
              <execution>
                <goals>
                  <goal>integration-test</goal>
                  <goal>verify</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
      [...]
    </project>
```

По дефолту плагин будет тестировать файлы со следующим паттерном

- `"**/IT*.java"` - includes all of its subdirectories and all Java filenames that start with "IT".
- `"**/*IT.java"` - includes all of its subdirectories and all Java filenames that end with "IT".
- `"**/*ITCase.java"` - includes all of its subdirectories and all Java filenames that end with "ITCase".

![[images/Untitled 11 7.png|Untitled 11 7.png]]

### Jacoco maven plugin

[https://www.eclemma.org/jacoco/trunk/doc/maven.html](https://www.eclemma.org/jacoco/trunk/doc/maven.html)

Плагин используется для создания отчета прохождения тестов.

```XML
<plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.6</version>
        <executions>
          <execution>
            <id>prepare-agent</id>
            <goals>
              <goal>prepare-agent</goal>
            </goals>
          </execution>
          <execution>
            <id>generate-report-jacoco</id>
            <goals>
              <goal>report</goal>
            </goals>
            <phase>prepare-package</phase>
          </execution>
        </executions>
      </plugin>
```

После выполения фазы pakage создает в папке target/jacoco.exec. После создания документации с помощью goal site можно посмотреть в папке jacoco страницу html

![[images/Untitled 12 7.png|Untitled 12 7.png]]

### Развертывание проекта

![[images/Untitled 13 7.png|Untitled 13 7.png]]

И еще одна очень интересная тема — автоматический deploy собранного пакета. Допустим, мы собрали собственную библиотеку с помощью Maven. Как нам автоматически выложить ее в локальный, корпоративный или центральный Maven-репозиторий?

Для этого у Maven есть специальный плагин maven-deploy-plugin.

![[images/Untitled 14 6.png|Untitled 14 6.png]]

```XML
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-deploy-plugin</artifactId>
    	<version>2.5</version>
    	<configuration>
          <file>${project.build.directory}\${project.artifactId}-src.zip</file>
          <url>${project.distributionManagement.repository.url}</url>
          <repositoryId>${project.distributionManagement.repository.id}</repositoryId>
          <groupId>${project.groupId}</groupId>
          <artifactId>${project.artifactId}</artifactId>
          <version>${project.version}</version>
      	  <packaging>zip</packaging>
          <pomFile>pom.xml</pomFile>
    	</configuration>
  	</plugin>
```

Тег `file` задает файл, который будет отправлен в репозиторий Maven как новая библиотека.

Тег `url` — путь к репозиторию Maven (локальный/корпоративный/…).

Тег `repositoryId` задает идентификатор репозитория, в который будет производиться депллой.

Теги `groupId, artifactId`, version задают стандартную идентификацию пакета в репозитории Maven. Именно по этим трем параметрам можно однозначно идентифицировать библиотеку.

Тег `packaging` используется для того, чтобы результат был отправлен одним zip-файлом. Если его не указать, то будет один jar-файл, даже если у вас было несколько jar-файлов.

Тег `pomFile` опциональный и позволяет отправить в репозиторий другой pom.xml, который не содержит скрытых или служебных данных.

### Cоздание нового модуля, наследование зависимостей

Клик по проекту, новый модуль Maven. Пример:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.example</groupId>
        <artifactId>LifeCyclesMaven</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>database</artifactId>

    <properties>
        <maven.compiler.source>19</maven.compiler.source>
        <maven.compiler.target>19</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

В pom родителя добавился следующий код:

```XML
<modules>
    <module>database</module>
  </modules>
```

При выполнении mvn install в проекте-родителе, автоматически сбилдится наш модуль

```XML
[INFO] --- maven-install-plugin:2.4:install (default-install) @ database ---
[INFO] Installing /home/vadim/IdeaProjects/LifeCyclesMaven/database/target/database-1.0-SNAPSHOT.jar to /home/vadim/.m2/repository/org/example/database/1.0-SNAPSHOT/database-1.0-SNAPSHOT.jar
[INFO] Installing /home/vadim/IdeaProjects/LifeCyclesMaven/database/pom.xml to /home/vadim/.m2/repository/org/example/database/1.0-SNAPSHOT/database-1.0-SNAPSHOT.pom
```

При этом можно наследовать зависимости, в пом родителя в раздел <\dependencyManagement> добавляем наши зависимости. В наследника добавляем завиимость без добавления версии. Пример:

```XML
<groupId>org.example</groupId>
  <artifactId>LifeCyclesMaven</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <name>LifeCyclesMaven</name>
  <url>http://maven.apache.org</url>
  <modules>
    <module>database</module>
  </modules>

<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>jakarta.activation</groupId>
        <artifactId>jakarta.activation-api</artifactId>
        <version>2.1.1</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

```XML
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.example</groupId>
        <artifactId>LifeCyclesMaven</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>database</artifactId>

<dependencies>
        <dependency>
            <groupId>jakarta.activation</groupId>
            <artifactId>jakarta.activation-api</artifactId>
        </dependency>
    </dependencies>
```

![[images/Untitled 15 6.png|Untitled 15 6.png]]

### Ещё пару нюансов

![[images/Untitled 16 5.png|Untitled 16 5.png]]

  

- Чтобы запускать jar нужен манифест в МЕТА ИНФ
- Чтобы попал манифест, то нужно на фазе pakage переопределить плагин jar

![[images/Untitled 17 5.png|Untitled 17 5.png]]

![[images/Untitled 18 4.png|Untitled 18 4.png]]

  

- Но будет проблема, так как нет зависимостей. поэтому их нужно тоже взять
- Способ 1) Отдельно собирать зависимости в lib и добавлять classpath
- Способ 2)

![[images/Untitled 19 4.png|Untitled 19 4.png]]