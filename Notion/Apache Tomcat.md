---
modified: 2024-09-02T13:14:51+03:00
---
### Общая информация

[https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi)

==По свой сути это обычное== ==Java приложение,== ==которое может обрабатывать== ==HTTP== ==запросы.==

![[images/Untitled 145.png|Untitled 145.png]]

==Структура Apache TomCat==

- ==Сервлет== - extendts `HttpServlet`.
- ==Сервлеты== это мапа с ключом по url (путь)
- При первом вызове ==сервлета==, ==сначала нужно загрузить== его в я `jvm,` ==потом создается объект и вызывается метод== `init()` (ВСЁ ЭТО ПРОИСХОДИТ 1 РАЗ)

![[images/Untitled 1 7.png|Untitled 1 7.png]]

- `/bin` директория, где ==собран сам Apache Tomcat с различными скрипта для запуск==а, рестарта и т.д.
- `/conf` - где собраны ==конфигурационные файлы==
- `/lib` где собраны `jar` ==файлы==, необходимые для Apache Tomcat (сторонние зависимости)
- `/logs` - по умолчанию пустая, туда записываются ==логи==
- `/webapps` - там, где будут лежать ==приложения пользователя==  
      
      
    ==В каждом веб приложении есть папка== `WEB-INF`, которая по своей сути является `private` ==директорией==, там можно ==хранить== `html` страницы, ==к которым невозможно получить доступ.==

Папка `**WEB-INF**` обычно располагается внутри директории веб-приложения, которую вы разрабатываете. Если вы используете структуру стандартного веб-приложения Java EE, то папка `**WEB-INF**` должна находиться внутри каталога `**src/main/webapp**` вашего проекта. Вот пример структуры проекта Gradle:

```CSS

your-project/
├── src/
│   └── main/
│       └── webapp/
│           ├── WEB-INF/
│           │   ├── classes/
│           │   ├── lib/
│           │   └── web.xml
│           ├── index.jsp
│           └── other-web-resources...
├── build.gradle
└── other-files...

```

В этой структуре `**WEB-INF**` содержит следующие поддиректории:

- `**classes/**`: Здесь хранятся скомпилированные классы Java вашего приложения.
- `**lib/**`: Этот каталог содержит JAR-файлы библиотек и зависимостей вашего приложения.
- `**web.xml**`: Этот файл представляет собой дескриптор развертывания (deployment descriptor) вашего веб-приложения, где вы можете настроить параметры сервлетов, фильтров, слушателей событий и другие конфигурационные параметры.

После сборки проекта Gradle и создания war-файла, структура `**WEB-INF**` останется внутри war-архива. Когда вы развернете этот war-файл в Tomcat (скопируете его в каталог `**webapps**`), Tomcat автоматически распакует архив и разместит `**WEB-INF**` внутри директории развернутого веб-приложения.

![[images/Untitled 2 6.png|Untitled 2 6.png]]

  

### Установка ApacheTomCat:

1. Перейти по ссылке `[https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi)` , скачать архив, разархивировать.
2. Перейти в /bin выполнить следующий код:

```Bash
./startup.sh 
```

Если установка прошла успешно, увидим:

```Bash
Using CATALINA_BASE:   /home/vadim/ApacheTomcat10
Using CATALINA_HOME:   /home/vadim/ApacheTomcat10
Using CATALINA_TMPDIR: /home/vadim/ApacheTomcat10/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /home/vadim/ApacheTomcat10/bin/bootstrap.jar:/home/vadim/ApacheTomcat10/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```

Можно перейти по [http://localhost:8080/](http://localhost:8080/) чтобы увидеть запущенный сервер.

![[images/Untitled 3 6.png|Untitled 3 6.png]]

  

  

### Конфигурация

Чтобы добавить пользователя с правами админа и иметь возможность перейти в [http://localhost:8080/manager/html](http://localhost:8080/manager/html) :

Небходимо перейти в /conf

```Bash
cd /conf
nano tomcat-users.xml
```

Добавить тег <user…:

```XML
//
<user username="admin" password="admin" roles="manager-gui"/>
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>
-->
```

Основной файл с настройками: server.xml.

### Server.xml

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- Note:  A "Server" is not itself a "Container", so you may not
     define subcomponents such as "Valves" at this level.
     Documentation at /docs/config/server.html
 -->
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
  <!-- APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <!-- A "Service" is a collection of one or more "Connectors" that share
       a single "Container" Note:  A "Service" is not itself a "Container",
       so you may not define subcomponents such as "Valves" at this level.
       Documentation at /docs/config/service.html
   -->
  <Service name="Catalina">

    <!--The connectors can use a shared executor, you can define one or more named thread pools-->
    <!--
    <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
        maxThreads="150" minSpareThreads="4"/>
    -->


    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         HTTP Connector: /docs/config/http.html
         AJP  Connector: /docs/config/ajp.html
         Define a non-SSL/TLS HTTP/1.1 Connector on port 8080
    -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               />
    -->
    <!-- Define an SSL/TLS HTTP/1.1 Connector on port 8443 with HTTP/2
         This connector uses the NIO implementation. The default
         SSLImplementation will depend on the presence of the APR/native
         library and the useOpenSSL attribute of the AprLifecycleListener.
         Either JSSE or OpenSSL style configuration may be used regardless of
         the SSLImplementation selected. JSSE style configuration is used below.
    -->
    <!--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true"
               maxParameterCount="1000"
               >
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/localhost-rsa.jks"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
    -->

    <!-- Define an AJP 1.3 Connector on port 8009 -->
    <!--
    <Connector protocol="AJP/1.3"
               address="::1"
               port="8009"
               redirectPort="8443"
               maxParameterCount="1000"
               />
    -->

    <!-- An Engine represents the entry point (within Catalina) that processes
         every request.  The Engine implementation for Tomcat stand alone
         analyzes the HTTP headers included with the request, and passes them
         on to the appropriate Host (virtual host).
         Documentation at /docs/config/engine.html -->

    <!-- You should set jvmRoute to support load-balancing via AJP ie :
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="jvm1">
    -->
    <Engine name="Catalina" defaultHost="localhost">

      <!--For clustering, please take a look at documentation at:
          /docs/cluster-howto.html  (simple how to)
          /docs/config/cluster.html (reference documentation) -->
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->

      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <!-- This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm.  -->
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
    </Engine>
  </Service>
</Server>
```

### Равернуть приложение на Tomcat

1. Создать новые проект тип JakartaEE, указать путь к директории Tomcat, если версия Tomcat 10+, Java должна быть 8+.

![[images/Untitled 4 6.png|Untitled 4 6.png]]

  

1. Создать из существующего war архив.
    
    - Files→Project Structure→Artifacts→New module→Web( он будет exploded, разархивированный)
    - После этого Files→Project Structure→Artifacts→New module → Web application Archive → используя из exploded web архива, созданного выше.
    
    ![[images/Untitled 5 6.png|Untitled 5 6.png]]
    
    - После этого переместить архив в папку tomcat/webapps
    - Перезапустить tomcat, он автоматически разахивирует war архив
    
    ![[images/Untitled 6 5.png|Untitled 6 5.png]]
    
      
    
    1. через IDEA
    
    - Edit Configurations→ TomCat
    - После этого необходимо настроить и добавить артефакт( наш разархивированный war архив).
    - ВАЖНО⚠️ если tomcat уже запущен на порту 8080, необходимо его либо отключчить, чтобы запустить также на 8080 либо поменять порт. У меня 8081.
    
    ![[images/Untitled 7 5.png|Untitled 7 5.png]]
    
      
    

### mvc

![[images/Untitled 8 5.png|Untitled 8 5.png]]

![[images/Untitled 9 5.png|Untitled 9 5.png]]