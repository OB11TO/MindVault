---
modified: 2024-09-02T13:10:56+03:00
---
Курс - [https://www.asozykin.ru/courses/networks_online](https://www.asozykin.ru/courses/networks_online)

Конспект - [https://zinvapel.github.io/it/network/2017/11/13/sozykin/](https://zinvapel.github.io/it/network/2017/11/13/sozykin/)

![[images/Untitled 17.png|Untitled 17.png]]

### Модель OSI (Теория)

Open System Interconnection model - эталонная модель взаимодействия открытых систем, которая лежит в основе всех стандартов компьютерных сетей.

![[images/Untitled 1 6.png|Untitled 1 6.png]]

### Уровни

![[images/Untitled 2 5.png|Untitled 2 5.png]]

### Прикладной уровень

Обеспечивает связь между приложениями

Единственный уровень, который напрямую взаимодействует с данными от пользователей

Протоколы прикладного уровня `HTTP, FTP, SMTP, SSH` и другие

![[images/Untitled 3 5.png|Untitled 3 5.png]]

### Уровень представления

Presentation layer - основная задача этого уровня заключается в преобразовании передаваемх данных во взаимно согласованные форматы, шифровании, компрессии данных и обратные этим процессам, дешифрование, декомпрессия и тд.

Протоколы - TSL, SSL

![[images/Untitled 4 5.png|Untitled 4 5.png]]

### Сеансовый уровень

организует сеансы связи между устройствами, позволяя взаимодействовать длительное время

Другими словам, ответственный за открытие и закрытие соединений ( сессия - это и есть время между открытием и закрытием соединения между устройствами)

На практике протоколы сенасового уровня не выделяются, их работу выполняют протоколы верхних уровней

![[images/Untitled 5 5.png|Untitled 5 5.png]]

### Транспортный уровень

Разбивает поток данных на сегменты для передачи на сетевой уровень( и наоборот, склеивает пакеты из сетевого уровня в сегменты), добавляя свой заголовок к каждому сегменту (порты приложений), а также выполняет процедуры для обеспечения необходимого уровня надежности передачи информации.

![[images/Untitled 6 4.png|Untitled 6 4.png]]

### Основные протоколы TCP, UDP

Порт в контексте TCP (Transmission Control Protocol) и UDP (User Datagram Protocol) является числовым идентификатором, который используется для определения конечной точки коммуникации в сетевом приложении. Каждое сетевое приложение на устройстве, подключенном к сети, может использовать порт для обмена данными с другими устройствами.

![[images/Untitled 7 4.png|Untitled 7 4.png]]

_Время между открытием и закрытием соединения - сессия._

  

TCP и UDP — это два наиболее распространенных протокола транспортного уровня, которые обеспечивают доставку данных в компьютерных сетях. Они используют порты для обеспечения правильной доставки данных на конкретное приложение или службу.

В TCP/IP каждый порт представлен 16-битным числом (от 0 до 65535). Порты до 1024 называются известными портами (well-known ports) и зарезервированы для широко известных служб, таких как веб-серверы (порт 80 для HTTP, порт 443 для HTTPS), почтовые серверы (порт 25 для SMTP, порт 110 для POP3) и другие.

Порты от 1024 до 49151 называются зарегистрированными портами (registered ports) и могут быть использованы различными приложениями и службами, не являющимися широкоизвестными.

Порты от 49152 до 65535 называются динамическими или частными портами (dynamic/private ports). Они используются операционной системой для временных соединений и не связаны с конкретными службами.

Протокол TCP обеспечивает надежную, установленную и ориентированную на соединение передачу данных. Он использует порты для установления соединения между двумя конечными точками (IP-адресами) и обмена данными в рамках этого соединения.

UDP является протоколом без установления соединения и не гарантирует доставку данных. Он также использует порты для идентификации приложений, но каждый пакет данных отправляется независимо и может быть доставлен в любом порядке или потерян без уведомления.

Примеры использования UDP:

Там где файлы могут не доставиться( скайп звонок, ютуб).

Появляется возможность запускать на одной машине несколько приложений с доступом в сеть по шаблону IP: Port : `127.0.0.8080`

### Сетевой уровень

Главной задачей является разбиение сегментов с предыдущего уровня на пакеты данных ( и наооборот, пакеты собирать в сегменты для отправки на следующий уровень), а также доставка этих пакетов данных между разными сетями.  
Основной протокол сетевого уровня -  
`IP (Internet Protocol)`

![[images/Untitled 8 4.png|Untitled 8 4.png]]

На данной картинке показано, что сетевой уровень принимает из транспортного уровня (TL) и добавляет к метаинформации свой заголовок, предварительно разбивая сегмент на пакеты.

Для решения этой задачи вводится своя система адресации ( сетевой адрес IP4 или IPv6) и реализуется механизм маршрутизации ( определение пути передачи данных между сетями. Маршрутизатор == роутер.

Ip адреса должны быть униакальны в пределах всей сети Internet.

  

### IP aдрес (v4)

32 битное число, которое принято записывать 4 октетами, каждый октет разделен точкой и занимает ровно 8 бит и может принимать значения от 0 до 255. `192.168.0.1` , `172.217.16.4`

![[images/Untitled 9 4.png|Untitled 9 4.png]]

![[images/Untitled 10 3.png|Untitled 10 3.png]]

![[images/Untitled 11 3.png|Untitled 11 3.png]]

![[images/Untitled 12 3.png|Untitled 12 3.png]]

![[images/Untitled 13 3.png|Untitled 13 3.png]]

![[images/Untitled 14 2.png|Untitled 14 2.png]]

### IP aдрес (v6)

это 128-битное число, которое принято записывать в виде 8 четырезначных шестнадцатеричных чисел, разделенных двоеточием `2001:0db8:0000:0000:0000:ff00:0042:7879` , `2001:0db8`**`::`**`ff00:0042:7879`, нули могут опускаться двумя двоеточиями.

  

Наряду с public IP адресами существуют private IP адреса, которые можно самостоятельно назначать в локальных сетях, в таком случае доступ в глобальную сеть Internet осуществляется на основе NAT (Network Address Translation) или Proxy.

Диапазоны private IP адресов:

10.0.0.0 - 10.255.255.255

172.16.0.0 - 172.31.255.255

192.168.0.0 - 192.168.255.255

  

### Сетевая маска

Ip адреса недостаточно для определния сети, в которой находится компьюер.

Для определения адреса сети используется дополнительное 32-битное число - сетевая маска. Помогает определять в какой подсети находится наше устройство.

![[images/Untitled 15 2.png|Untitled 15 2.png]]

![[images/Untitled 16 2.png|Untitled 16 2.png]]

При отправке данных получателю 1 от отправителя не увидим трафика, так как адрес сети( первые три октета) совпадает, значит они находятся в одной подсети. А получатель 2 находится уже в другой сети( 3 октет не совпадает), придется задействовать маршрутизатор, чтобы перенаправить наши IP пакеты в другую сеть.

![[images/Untitled 17 2.png|Untitled 17 2.png]]

### Канальный уровень

Data link layer разбиваются пакеты с предыдущего уровня на кадры (frame) данных ( и наооборот, кадры преобразуются в пакеты) и производится доставка кадров в пределах одной сети, осуществляется проверка доступности среды передачи и контроль ошибок передачи.  
Основные протоколы транспортного уровня  
`Ethernet, PPP(Point to Point Protocol), PPpoE(разновидность Point to Point Protocol)`

![[Untitled 18.png]]

![[Untitled 19.png]]

![[Untitled 20.png]]

![[Untitled 21.png]]

### Физический уровень

Реализован аппаратно и определяет методы передачи битов данных по физическим каналам.

Описываются такие характеристики как вид среды передачи(кабель или радиоэфир), топологии сети и другие

![[Untitled 22.png]]

### DNS

Доменное имя представляет из себя понятный и хорошо запоминающийся человеку текст, например google.com

С каждым доменным именем связывается один или несколько IP адресов, и в свою очередь, с каждым IP адресом может быть связано одно или несколько доменных имен.

Cуществуют два способа реализации системы доменных имен:

1. был основан на использовании файла hosts, который представляет из себя обычный текстовый файл, где хранятся пары соответствий IP адресов и доменных имен.
    
    Windows - `C:\windows\system32\drivers\etc\hosts`
    
    Unix - `/etc/hosts`
    
2. Второй способ реализации доменных имен основан на использовании службы DNS  
    DNS (Domain Name System) - это распределенная система, в которой информация о доменах хранится на большом количестве связанных между собой DNS-серверов.  
    

Каждый DNS-сервер хранит информацию о доменах только своей зоны

Но т.к. все DNS серверы связаны между собой, то можно получить необходимую информацию( т.е. IP адрес по доменному имени) вне зависимости от того, к какому серверу вы обратились.

DNS предполагает, что все компьютеры в сети разделяются на логические группы - домены.

При этом доменные имена образуют иерархическую структуру, т.к одни домены могут являться частью других

В этой связи выделяют домены первого уровня, второго и т.д.

![[Untitled 23.png]]

Переход с домена на домен осуществляется справа-налево

![[Untitled 24.png]]

Обычно DNS используется для преобразования доменных имен в IP адреса - это называется прямое преобразование.

Обратное преобразование - по известному IP адресу получить доменное имя.

Для обратного преобразования в DNS используются обратные зоны, которые создаются и настраиваются независимо от прямых.

Созданием и поддержанием имен в доменах верхнего уровня занимаются спцеиальные компании - регистраторы доменных имен.

Сами домены верхнего уровня создаются и поддерживаются на уровне корневого домена. Этим занимается международная организация.

### HTTP

### Общая информация

HyperText Transfer Protocol - протокол передачи произвольных данных прикладного уровня, который изначально был предназначен для передачи только HTML (HYPER TEXT MARKUP LANGUAGE) документов. Сейчас отношение к гипертексту не имеет.

Протокол соответствует клиент-серверной архитектуре, где взаимодействие и клиента и веб-сервера осуществляется по стандартной схеме `request-response`

![[Untitled 25.png]]

Каждое HTTP-сообщение, независимо от того, следует оно от клиенту к серверу (request) или от сервера к клиенту (response), состоит трех основных частей:

1. Стартовая строка - определяет тип сообщения, в которой указываются следующие данные(general):
    
    1. `Request`:
    
    - метод (GET, POST, PUT. DELETE. etc) - название операции, которая должна быть выполнена HTTP запросом (request)
    - адрес (URL) - Unfiormj Resourse Locator состоит из 4 основных частей :
        - протокол
        - доменное имя или IP адрес (с портом)
        - адрес ресурса (по указанному доменному имени или IP адресу)
        - список параметров
        - ==`https://`====`www.examle.com:443`====`/users/1`====`?word=java&sourseid=chrome`==
    - версия протокола (HHTP”/1.1, HTTP/2)
    
    b. `Response`:
    
    - версия протокола (HTTP/1.1, HTTP/2)
    - код состояния  
        Код состояни (status code) - цифровой код ответа сервера, состоящий ищ трехзначного числа, первая цифра которого означает класс состояния.  
        
        ![[Untitled 26.png]]
        
        - 1xx - информация о состоянии процесса передачи  
            101 Continue  
            101 Switching protocols  
            
        - 2xx - информация об успешном запросе и его обработке  
            200 OK (get запрос)  
            201 Created (Created)  
            204 No content( delete запрос)  
            
        - 3xx - информация о том, что необходимо выполнить запрос по другому адресу, указанному в заголовке location(header)  
            301 Moved permanentl  
            302 Moved temporarily  
            
        - 4xx - информация об ошибках со стороны клиента  
            401 Unathorized  
            403 Forbidden  
            404 Not found  
            
        - 5xx информация об ошибках на стороне сервера  
            500 Internal Server Error  
            503 Service Unavailable  
            504 Gateway Timeout  
              
            
    - текстовое пояснение
    
    ![[Untitled 27.png]]
    
2. Заголовок(header)  
    Характеризует тело сообщения body и параметры его передачи в ваиде “header_name: header_value”  
    Другими словами, является метаинформацией HTTP сообщения (названия заголовков не чувствительны к регистру)  
    

![[Untitled 28.png]]

1. Тело сообщения (body) - это непосредственно пересылаемые данные HTTP сообщением. Отделяется от заголовка пустой строкой. Сами данные могут быть любые: HTML страницы, JSON, XML, видео, картинки, файлы

![[Untitled 29.png]]

### HTTP/2

Главные отличия от HTTP/1.1 :

- ==Бинарный вместо текстового==  
      
    ==Бинарные сообщения быстрее разбираются автоматически, представляяя из себя== ==фреймы и потоки.== ==Но в отличие от тектовых, не понятны для чтения человеком.==  
    Переходом на бинарный формат, HTTP/2 пытается решить проблему выросшей задержки (latency)  
    
    ![[Untitled 30.png]]
    

==  
Теперь все  
====HTTP== ==сообщения делятся на фреймы== ==(HEADERS, DATA, RST_STREAM, PUSH_PROMISE, PRIORITY,etc)==

![[Untitled 31.png]]

==Коллекция таких фреймов== - ==двунаправленные поток== (`Stream`). Следовательно, ==каждый фрейм содержит идентификатор== ==(id)== ==потока  
Каждый  
====клиентский== ==запрос использует== ==нечетные== ==id====, а ответ от== ==сервера== ==-== ==четные====.==

![[Untitled 32.png]]

- ==Мультиплексирование== (==несколько асинхронных запросов через одно TCP- соединение== )  
    Благодаря  
    ==бинарному протоколу и представлению данных в виде фреймов и потоков, клиент и сервер могут обмениваться сообщениями асинхронно, используя лишь одно TCP соединение.==  
    Это также  
    ==решило проблему блокировки очереди запросов, когда медленный или большой запрос блокировал все последующие.==
- ==Server Push== (несколько ответов на один запрос)
    
    Сервер, зная, что клиент собирается запросить определенный ресурс,может отправить его сам, не дожидаясь запроса  
    Для этого сервер отправляет специальный фрейм PUSH_PROMISE с таким же id, что и запрос клиента  
    
    ![[Untitled 33.png]]
    
    ![[Untitled 34.png]]
    
- ==Сжатие заголовков методом HPACK==  
    Формат сжатия HTTP/2 заголовков HPACK состоит из трез основныз частей:  
    - Статическая таблица - общая для всех TCP соединений и содержит 61 часто используемые заголовки, которые можно найти в документации протокола
    - Динамическая таблица - создается для каждого TCP соединения и содержит используемые заголовки во время обмена сообщениями (ограниченного размера)
        
        ![[Untitled 35.png]]
        
        ![[Untitled 36.png]]
        
    - Сжатие заголовков алгоритмом Хаффмана
- ==Приоритизация запросов==  
    Клиент может назначить приоритет потоку (stream), добавив соответствующую информацию во фрейме HEADERS (число от 1 до 256), либо обновить уже созданный поток с помощью фрейма PRIORITY  
    Также каждому потоку может быть дана явная зависимость от другого потока, что вместе с приоритетами (пункт 1) представляет собой “дерево приоритетов”  
    
    ![[Untitled 37.png]]
    
- ==Безопасность==  
    Большинство клиентов (браузеров) поддерживают HTTP/2 только если он используется поверх протокола TLS ( т.е. должен использоваться протокол HTTPS)  
    В своб очередь, спецификация не требует данного ограничения  
    Сокращение числа подключений ввиду перечисленныз преимуществ HTTP/2 приводит к сокращению затратных “рукопожатий TLS” (TLS handshake)  
    

### Mime types

![[Untitled 38.png]]

Content type определяет каким типом данных является body. accept означает какой тип данных ожидает получить клиент.

  

MIME тип (MIME - Multipurpose Internet Mail Extensions) - это стандарт, используемый для идентификации типов данных в Интернете. Он предназначен для указания характеристик содержимого определенного файла или потока данных.

MIME типы обычно состоят из двух частей, разделенных косой чертой ("/") `type/subtype`. Первая часть определяет основную категорию типа данных, а вторая часть указывает на конкретный подтип или формат данных.

Например, "text/plain" - это MIME тип для обычного текстового файла, а "image/jpeg" - для изображений в формате JPEG. Другие примеры MIME типов включают "application/pdf" для документов в формате PDF, "audio/mp3" для аудиофайлов в формате MP3 и "video/mp4" для видеофайлов в формате MP4.

Также могут быть переданы параметры `type/subtype;parameter=value`

Подробнее [https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

### Форматы данных

### XML

XML:

- XML (Расширяемый язык разметки) - это язык разметки, который используется для хранения и передачи структурированных данных.
- XML-документ должен начинаться с объявления версии и кодировки, которые указываются в заголовке:

```XML
<?xml version="1.0" encoding="UTF-8"?>
```

- Данные в XML представляются в виде элементов, заключенных в открывающие и закрывающие теги:

```XML
<элемент>данные</элемент>
```

- Элементы могут содержать атрибуты, которые указываются внутри открывающего тега:

```XML
<элемент атрибут="значение">данные</элемент>
```

- Вложенные элементы должны быть правильно вложены друг в друга:

```XML
<родительский>
  <дочерний>данные</дочерний>
</родительский>
```

- Специальные символы, такие как `<`, `>`, `&`, должны быть заменены соответствующими символами-сущностями:

```XML
<элемент>значение &gt; 5</элемент>
```

![[Untitled 39.png]]

Тег может либо содержать текстовый узел, либо другие теги, либо быть пустым в примере - <body/>

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<request id ="25">
    <start-row>
        <url>www.google.com</url>
    </start-row>
    <headers>
        <header>
            <name>content-type</name>
            <value>text/html</value>
        </header>
        <header>
            <name>accept</name>
            <value>application/json</value>
        </header>
    </headers>
    <body type = "text.plain"/> --пустое поле
</request>
```

### XML Parser

Необходимо добавить зависимости:

```XML
<dependencies>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
        <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-xml</artifactId>
            <version>2.15.2</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга:

```Java
import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.annotation.JsonPropertyOrder;
import lombok.Data;
import lombok.Getter;


@Data
@JsonInclude(JsonInclude.Include.NON_DEFAULT)
@JsonPropertyOrder({"author", "title", "pages"})
public class Book {

    private String title;
    private String author;
    private int pages;

    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }

    public Book() {
    }

    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", pages=" + pages +
                '}';
    }
}
```

Парсер:

```Java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.dataformat.xml.XmlMapper;

public class XMLParser {
    public static void parseXML(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new XmlMapper();
        mapper.enable(SerializationFeature.INDENT_OUTPUT);
        String xmlBook = mapper.writeValueAsString(book);
        System.out.println(xmlBook);
    }

    public static void deparseXML(String xmlString) throws JsonProcessingException {
        ObjectMapper mapper = new XmlMapper();
        Book book = mapper.readValue(xmlString, Book.class);
        System.out.println(book);
    }
}
```

Реализация парсинга:

```Java
import com.fasterxml.jackson.core.JsonProcessingException;


public class Runner {

    public static void main(String[] args) throws JsonProcessingException {

        String xmlString = """
                 <Book>
                  <title>Обитаемый остров</title>
                  <author>Стругацкий А., Стругацкий Б.</author>
                  <pages>413</pages>
                </Book>""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {
            XMLParser.parseXML(book);
            XMLParser.deparseXML(xmlString);
        }
        catch (JsonProcessingException e){
            System.out.println("Ошибка в методах обработки данных");
        }
    }
}
```

  

### HTML

Список тегов [https://html5book.ru/html-tags/](https://html5book.ru/html-tags/)

- HTML (HyperText Markup Language) - это язык разметки, используемый для создания веб-страниц.
- HTML-документ должен начинаться с объявления типа документа:

```HTML
<!DOCTYPE html>
```

- Элементы HTML описываются с помощью тегов:

```HTML
<тег>содержимое</тег>
```

- HTML-элементы могут иметь атрибуты, которые указываются внутри открывающего тега:

```HTML
<тег атрибут="значение">содержимое</тег>
```

- Вложенные элементы должны быть правильно вложены друг в друга:

```HTML
<div>
  <p>Абзац</p>
</div>
```

- Специальные символы, такие как `<`, `>`, `&`, должны быть заменены соответствующими символами-сущностями:

```HTML
<p>5 &gt; 3</p>
```

Это основные правила для написания данных в форматах XML, JSON, YAML и HTML. Надеюсь, это поможет вам в работе с этими форматами!

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="path/to/script.js"></script>
    <link rel="stylesheet" href="path/to/simple.css">
</head>
<body>
<div>

</div>
<h1>
    Hello world!
</h1>
<div>
    <form>
        <label>
            Username:
            <input type="text" method="post">
        </label>
        <label>
            Password:
            <input type="password" name="username">
        </label>
        <button type="submit">SEND</button>
    </form>
</div>
<span></span>

</body>
</html>
```

### JSON

JSON:

- JSON (JavaScript Object Notation) - это формат обмена данными, основанный на синтаксисе JavaScript.
- JSON представляет данные в виде пар "ключ-значение":

```JSON
{
  "ключ": "значение",
  "ключ2": "значение2"
}
```

- Ключи и значения должны быть заключены в двойные кавычки.
- Различные пары "ключ-значение" разделяются запятой.
- Значения могут быть строками, числами, логическими значениями, массивами, объектами или специальными значениями `null`.
- JSON не поддерживает комментарии.

```JSON
{
"id": 25,
"startRow": {
	"method": "GET",
	"url": "www.google.com",
	"version": 1.1
	},
"headers": [
		{
			"name": "accept",
			"value" : "text/html"
		},
		{
			"name": "accept",
			"value" : "text/json"
		}
	],
"body": {
	"type": "text/plain"
	}
}
```

### JSON Parser

Необходимо добавить зависимости:

```XML
<dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга

```Java
import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.annotation.JsonPropertyOrder;
import lombok.Data;
import lombok.Getter;


@Data
@JsonInclude(JsonInclude.Include.NON_DEFAULT)
@JsonPropertyOrder({"author", "title", "pages"})
public class Book {

    private String title;
    private String author;
    private int pages;

    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }

    public Book() {
    }

    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", pages=" + pages +
                '}';
    }
}
```

aaa

```Java
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

public class JSONParser {
    public static void parseJson(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper()
                .setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .enable(SerializationFeature.INDENT_OUTPUT);
        String jsonBook =  mapper.writeValueAsString(book);
        System.out.println(jsonBook);
    }

    public static void deparseJson(String jsonString) throws JsonProcessingException {
        Book book1 = new ObjectMapper()
                .setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
                .readValue(jsonString, Book.class);
        System.out.println(book1);
    }
}
```

Реализация парсинга

```Java
package ru.konovalov.url;

import com.fasterxml.jackson.core.JsonProcessingException;

public class Runner {
    public static void main(String[] args) {
        String jsonString = """
                {
                 	"title" : "Обитаемый остров",
                 	"author" : "Стругацкий А., Стругацкий Б.",
                 	"pages" : 413,
                 	"unknown property" : 42
                }""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {

						JsonParser.parseJson(book);
            JsonParser.deparseJson(jsonString);

        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }

    }
}
```

### Yaml

- YAML (Yet Another Markup Language) - это формат сериализации данных, который обычно используется для конфигурационных файлов.
- YAML представляет данные в виде иерархической структуры с помощью отступов:

```YAML
ключ: значение
другой_ключ:
  - элемент1
  - элемент2
```

- Значения могут быть строками, числами, логическими значениями, массивами, объектами или специальными значениями `null`.
- Списки элементов указываются с помощью дефиса и отступов.

### Yaml Parser

Зависимости

```XML
<dependencies>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-yaml -->
        <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-yaml</artifactId>
            <version>2.15.2</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга

```Java
package ru.konovalov.url;

public class Book {
    private String title;
    private String author;
    private int pages;

    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }

    public Book() {
    }

    @Override
    public String toString() {
        return "Book{" +
               "title='" + title + '\'' +
               ", author='" + author + '\'' +
               ", pages=" + pages +
               '}';
    }
}
```

Парсер

```Java
package  ru.konovalov.url;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.dataformat.yaml.YAMLMapper;

public class YamlParser {
    public static void parseYAML(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new YAMLMapper();
        mapper.enable(SerializationFeature.INDENT_OUTPUT);
        String yamlBook = mapper.writeValueAsString(book);
        System.out.println(yamlBook);
    }
    public static void deparseYAML(String yamlString) throws JsonProcessingException {
        ObjectMapper mapper = new YAMLMapper();
        Book book = mapper.readValue(yamlString, Book.class);
        System.out.println(book);
    }
}
```

Использование парсера

```Java
package ru.konovalov.url;

import com.fasterxml.jackson.core.JsonProcessingException;

public class Runner {
    public static void main(String[] args) {
        String yamlString =  """
           ---
           title: "Обитаемый остров"
           author: "Стругацкий А., Стругацкий Б."
           pages: 413""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {

            YamlParser.parseYAML(book);
            YamlParser.deparseYAML(yamlString);

        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }

    }
}
```

### Примеры создания серверов с помощью java.net

![[Untitled 40.png]]

### Class java.net.Socket implements java.io.Closeable

Так как Socket имплементирует Cloaseable, необходимо закрывать либо оборачивать в try-with-resources

  

  

### TCP&UDP Networking

**TCP:**

- java.net.Socket
- java.net.ServerSocket
- java.nio.SocketChannel
- java.nio.ServerSocketChannel

**UDP:**

- java.net.DatagramSocket
- java.nio.DatagramChannel

### Отправка и получение запроса на собственный сервер c помощью Socket и ServerSocket (TCP)

Сначала необходимо запустить ServerSocket, потом Socket, чтобы установить соединение по порту. Иначе выбросится исключение, так как это TCP протокол:

```Java
Exception in thread "main" java.net.ConnectException: В соединении отказано
```

```Java
package ru.konovalov;

import java.io.DataOutputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class SocketRunner {
    public static void main(String[] args) throws IOException {
        var inetAddress = InetAddress.getByName("localhost");
        try (Socket socket = new Socket(inetAddress, 7777);
             var outputStream = new DataOutputStream(socket.getOutputStream());
             var inputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            while (scanner.hasNextLine()){
                String request = scanner.nextLine();
                outputStream.writeUTF(request);
                System.out.println("Response from server: " + inputStream.readUTF());
            }

        }
    }
}
```

![[Untitled 41.png]]

```Java
package ru.konovalov;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class SocketServerRunner {
    public static void main(String[] args) throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(7777);
             Socket socket = serverSocket.accept();
             DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());
             DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            var request = dataInputStream.readUTF();
            while (!"stop".equals(request)) {
                System.out.println("Client request: " + request);
                var response = scanner.nextLine();
                dataOutputStream.writeUTF(response);
                request = dataInputStream.readUTF();
            }
        }
    }
}
```

![[Untitled 42.png]]

![[Untitled 43.png]]

Если в консоль Socket написать `"stop",` то получим исключение, так как у нас TCP соединение и мы не получим response от ServerSocket, а должны.

```Java
Exception in thread "main" java.io.EOFException
at java.base/java.io.DataInputStream.readUnsignedShort(DataInputStream.java:346)
at java.base/java.io.DataInputStream.readUTF(DataInputStream.java:595)
at java.base/java.io.DataInputStream.readUTF(DataInputStream.java:570)
at ru.konovalov.SocketRunner.main(SocketRunner.java:20)
```

Поэтому необходимо обработать случай закрытия соединения:

```Java
package ru.konovalov;

import java.io.DataOutputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class SocketRunner {
    public static void main(String[] args) throws IOException {
        var inetAddress = InetAddress.getByName("localhost");
        try (Socket socket = new Socket(inetAddress, 7777);
             var outputStream = new DataOutputStream(socket.getOutputStream());
             var inputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            do {

                String request = scanner.nextLine();
                outputStream.writeUTF(request);
                String repsonse = inputStream.readUTF();
                System.out.println("Response from server: " + repsonse);
                if (repsonse.equals("Close connection.")) {
                    break;
                }
            } while (scanner.hasNextLine());

        }
    }
}
```

```Java
package ru.konovalov;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class SocketServerRunner {
    public static void main(String[] args) throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(7777);
             Socket socket = serverSocket.accept();
             DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());
             DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            var request = dataInputStream.readUTF();

                do {
                    System.out.println("Client request: " + request);
                    var response = scanner.nextLine();
                    dataOutputStream.writeUTF(response);
                    request = dataInputStream.readUTF();
                } while (!"stop".equals(request));
                    var response = "Close connection.";
                    dataOutputStream.writeUTF(response);
        }
    }
}
```

### Отправка и получение запроса используя DatagramSocket (UDP)

При отправке данных по протоколу UDP, необходимо регламентировать размер принимающего баффера, иначе есть возможность потерять часть отправленных данных.

```Java
package ru.konovalov;

import java.io.IOException;
import java.net.*;

public class DatagramRunner {
    public static void main(String[] args) throws IOException {
        InetAddress inetAddress = InetAddress.getByName("localhost");
        try(DatagramSocket socket  = new DatagramSocket()){
            var bytes = "Hello from UDP client".getBytes();
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length, inetAddress, 7777);
            socket.send(packet);
        }
    }
}
```

```Java
package ru.konovalov;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class DatagramServerRunner {
    public static void main(String[] args) throws IOException {
        try(DatagramSocket server = new DatagramSocket(7777)){
            byte[] buffer = new byte[512];
            DatagramPacket datagramPacket = new DatagramPacket(buffer, buffer.length);
            server.receive(datagramPacket);
            String data = new String(buffer);
            System.out.println(data);
        }
    }
}
```

### Пример работы с HTTP протоколом через класс URL

Удобно работать только с GET запросами. C POST запросами во первых приходится разрешить URLconnection.setDoOutput(true), так как изначально он выставлен в false. Потом переводить данные в байты и работать через OutPutStream

```Java
package ru.konovalov.url;

import java.io.IOException;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

public class UrlExample {
    public static void main(String[] args) throws IOException {
        URL url = new URL("file:/home/vadim/IdeaProjects/http-servlets-starter/src/main/java/ru/konovalov/DatagramRunner.java");
        URLConnection urlConnection = url.openConnection();
        System.out.println(new String(urlConnection.getInputStream().readAllBytes())); // вывод полной информации о нашем файле


         */
    }

    private static void checkGoogle() throws IOException {
        URL url = new URL("https://www.google.com");
        URLConnection connection = url.openConnection();
//ниже выведем html страницу по адресу  "https://www.google.com"( get запрос) 
				System.out.println(connection.getInputStream.readAllBytes());
//ниже представлен постзапрос
        connection.setDoOutput(true);
        try (OutputStream outputStream = connection.getOutputStream()) {
            outputStream.write(new byte[512]);
        }
    }
}
```

### HTTPClientExample

Появился с версии Java11 и выступает в роли клиента к серверу. Стал предпочтительный для использования по сравнению с URL. Имеет более удобный API и расширенный функционал.

Можно либо создать дефолтный:

```Java
HttpClient.newHttpClient();
```

Либо создать через паттерн builder настривая различные параметры.

![[Untitled 44.png]]

### Пример GET и POST запроса с помощью HTTPClient

- Помимо метода send() в данном классе имеется sendAsync() возвращающий CompletableFuture<HttpResponse\<T>> позволяющий не останавливать выполнение потока из-за ожидания response.

```Java
package ru.konovalov.url;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.file.Path;

public class HTTPClientExample {
    public static void main(String[] args) throws IOException, InterruptedException {
        HttpClient client = HttpClient.newBuilder()
                .version(HttpClient.Version.HTTP_1_1)  // по умолчанию версия 2
                .build();
        //GET запрос
        HttpRequest request = HttpRequest.newBuilder(URI.create("https://www.google.com"))
                .GET()
                .build();
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
        System.out.println(response.headers()); // вернет html страницу

				//POST запрос
        HttpRequest request2 = HttpRequest.newBuilder(URI.create("https://www.google.com"))
                .POST(HttpRequest.BodyPublishers.ofFile(Path.of("path", "to", "file")))
                .build();
    }
}
```

  

### Создание собственного Servlet


- Создание папки lib в корне проекта → Add as Library и перенести в эту директорую jar файл (либо зависимость в pom jakarta.servlet-api)

```Bash
cp servlet-api.jar /home/vadim/IdeaProjects/http-servlets-starter/lib
```

- Создать свой класс extends HtttpServlet из servlet-api.jar

```Java
package servlet;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet("/first")
public class FirstServlet extends HttpServlet {

    @Override
    public void init(ServletConfig config) throws ServletException {

        super.init(config);
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.service(req, resp);
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

Хорошим тоном является переопределение не метода service, а конкретных методов:

![[Untitled 45.png]]

```Java
package servlet;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/first")
public class FirstServlet extends HttpServlet {

    @Override
    public void init(ServletConfig config) throws ServletException {

        super.init(config);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      // позволяет вывести в консоль с какого устройства был выполнен запрос
			System.out.println(req.getHeader("user-agent"));
			
			resp.setContentType("text/html");

//инициализирует нужную кодировку
			resp.setCharacterEncoding(StandardCharsets.UTF_8.name());

//либо так resp.setContentType("text/html" ; "charset-UTF-8");

//добавляет кастомный header
		resp.setHeader("token", "12345");


        try (PrintWriter writer = resp.getWriter()) {
            writer.write("<h1>Hello from first Servlet</h1>");
        }
        super.doGet(req, resp);
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

После запуска томкэта на порту 8081 http://localhost:8081/first

![[Untitled 46.png]]

![[Untitled 47.png]]

![[Untitled 48.png]]

  

Еще один пример. Реализация get метода со скачиванием текстового файла, который содержит информацию о переданном json файле.

```Java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

@WebServlet("/download")
public class DownloadServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setHeader("Content-Disposition", "attachment; filename=\"filename.txt\"");
        resp.setContentType("application/json");
        resp.setCharacterEncoding(StandardCharsets.UTF_8.name());
        try(var printWriter = resp.getOutputStream();
        var stream = DownloadServlet.class.getClassLoader().getResourceAsStream("first.json"))
        {
            printWriter.write(stream.readAllBytes());
        }
    }


}
```

localhost:8081/download

![[Untitled 49.png]]

  

### Два взаимосвязанных сервлета через параметр

```Java
package servlet;

import dto.FlightDto;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import service.FlightService;


import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.util.List;

@WebServlet("/flights")
public class FlightServlet extends HttpServlet {

    private final FlightService flightService = FlightService.getInstance();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding(StandardCharsets.UTF_8.name());
        try(var writer = resp.getWriter()){
            writer.write("<h1>Список перелетов:</h1>");
            writer.write("<ul>");
            List<FlightDto> flightsDto = flightService.findAll();
            flightsDto.forEach(it-> writer.write(
                    """
                            <li>
                            <a href ="/tickets?flightId =%d">%s</a>
                            </li>
                            """.formatted(it.getID(),it.getDescription())
            ));

            writer.write("</ul>");
        }
    }
}
```

```Java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

@WebServlet("tickets")
public class TicketServlet extends HttpServlet {
    private final TicketService ticketService = TicketService.getInstance();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        var flightId = Long.valueOf(req.getParameter("flight_id"));
        resp.setContentType("text/html");
        resp.setCharacterEncoding(StandardCharsets.UTF_8.name());
        try(var writer = resp.getWriter()){
            writer.write("<h1> Билеты:</h1>");
            writer.write("<ul>");
            ticketService.findAllByFlightId(flightId).forEach(ticketDTO -> writer.write(
                    """
                            <li>
                            %s
                            </li>
                            """.formatted(ticketDTO.getTicketNumbers())
            ));
            writer.write("</ul>");
        }
    }
}
```

### Сookies

![[Untitled 50.png]]

```Java
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Arrays;
import java.util.concurrent.atomic.AtomicInteger;

public class CookieServlet extends HttpServlet {
    private static final String UNIQUE_ID = "user_id";
    private final AtomicInteger counter = new AtomicInteger();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        var cookies = req.getCookies();
        if(cookies == null  || Arrays.stream(cookies)
                .filter(cookie -> UNIQUE_ID.equals(cookie.getName()))
                .findFirst()
                .isEmpty()){
            var cookie = new Cookie(UNIQUE_ID, "1");
            cookie.setPath("/cookies");
            cookie.setMaxAge(15*60);
            resp.addCookie(cookie);
            counter.incrementAndGet();
        }
        resp.setContentType("text/html");
        try(var writer = resp.getWriter()){
            writer.write(counter.get());
        }
    }
}
```

![[Untitled 51.png]]

### Session от Tomcat

==Сессия с помощью куки определяет одна и та же эта сессия или нет.==

- Это всё `происходит в недрах Каталины.`
- Есть класс `Request` из ==Catalina==, который обертка над классом `Request` из ==Cayota==
- Достаем из ==Contexta== сессию, ==так как в томкате могут крутиться несколько приложений и у каждого будет свой контекст.==
- Из ==Manager== сессий, ==получаем сессию== по `jsessionId`
- Иначе ==создать== ==session==, со всеми данными и кладем в map с новый генерированным `jsessionId`

![[Untitled 52.png]]

![[Untitled 53.png]]

### Attributes

`Session` в ==основном используется, чтобы ещё хранить там какие-либо значания и ассоциировать с множестом== `request`

Если сессия будет впервые создана, то user будет равен null. Если сессия будет создана, но удалить cookie, связанный с сессией (JSESSIONID), то мы не сможем связать старую сессию, и будет создана новая, а соответственно и заново user.

```Java
import dto.UserDto;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

@WebServlet("/sessions")
public class SessionServlet extends HttpServlet {

    private static final String USER = "user";

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        UserDto user = (UserDto) session.getAttribute(USER);
        if (user == null) {
            user = UserDto.builder()
                    .id(1L)
                    .mail("konvvadim@gmail.com")
                    .build();
            session.setAttribute(USER, user);
        }
    }
}
```

`Контекст` - ==это глобальная переменная для всех сессий и запросов.== ==методы== ==getAttributes()== ==можно вызвать на каждой цепочке схемы ниже, задать также.==

- В ==ServletContext== - будут доступны ==глобально для всего приложения==
- ==HttpSession== - Положить пользователя, чтобы ==следующие запросы ассоциировать с этим пользователем.==
- ==HttpServletRequest== (живет один запрос) - Нужен для ==jsp==

![[Untitled 54.png]]

### Перенаправление запросов

![[Untitled 55.png]]

==**ВАЖНО**==: Все что записано после метода forward обработано не будет,в отличие от include(), так как запрос полностью передается другому сервлету.

Есть хитрая вещь, по сути она не решает проблему с форвардом, но если не закрывать поток, то и пока мы в Диспатчере не доделаем ничего, то клиент будет ждать

```Java
@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("sessions").forward(req,resp);

        resp.getWriter().write("<h1>IT WILL !!!NOT!!! BE WRITTEN</h1>");
    }
}
```

![[Untitled 56.png]]

```Java
@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("sessions").include(req,resp);
        
        resp.getWriter().write("<h1>IT WILL BE WRITTEN</h1>");
    }
```

- `Header`, которые мы установили в `FightServlet` не будут работать в `DispatcherServlet`. потому что `DS` главный здесь, он отвечает за это.

  

![[Untitled 57.png]]

![[Untitled 58.png]]

![[Untitled 59.png]]

```Java
@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.sendRedirect("/sessions");
    }
}
```

- Перенаправление через RequestDispatcher

```Java

@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       RequestDispatcher requestDispatcher = req.getRequestDispatcher("/flights");
       req.setAttribute("1", "234");
       requestDispatcher.forward(req,resp);
    }
}
```

Можно обойти создание переменной и перенаправить запрос сразу, если нет необходимости добавлять внутреннюю логику:

```Java
@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       req.getRequestDispatcher("/flights").forward(req,resp);
    }
```

- Также можно получить глобальную переменную контекст и у нее получить диспатчер:

```Java
@WebServlet("/dispatcher")
public class ServletDispatcher extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        RequestDispatcher requestDispatcher = getServletContext().getRequestDispatcher("/sessions");
        requestDispatcher.forward(req,resp);
    }
}

@WebServlet("/sessions")
public class SessionServlet extends HttpServlet {
...
}
```

![[Untitled 60.png]]

### JSP and JSTL

Шаблонный движок (Template Engine) - это программное средство, которое позволяет создавать динамические веб-страницы, генерируя HTML-код на основе шаблонов и данных. Он используется для разделения логики и представления веб-приложений, что упрощает разработку и поддержку.

### Жизненный цикл

JSP(Java Server Pages) - это технология Java, которая позволяет разработчикам создавать динамические веб-страницы, содержащие HTML, XML и другие типы документов, в которые встраивается Java код. JSP является одним из способов создания динамических веб-страниц в Java-приложениях.

Cтраница при первом обращении к ней превращается в java класс (Jasper самостоятельно его создает) , который наследутеся от HttpJspBase → после этого класс превращается в Servlet и дальше происходит стандартный жизненный цикл Servlet.

![[Untitled 61.png]]

![[Untitled 62.png]]

### Директивы

- page - нужна для конфигурации jsp страницы или еще include класса (для скриплетов!! (НЕ РЕКОМЕНДУЕТСЯ))

```HTML
<%@page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
```

- taglib - используется для указания других библиотек, которые мо хотим использовать в jsp странице
    
    ```HTML
    <% taglib prefix="c" uri "http://company.com" %>
    ```
    
- include - для включения в нашу страницу содержимое другой jsp или html страницы
    
    ```HTML
    <% include file="index.html" %>
    ```
    

### Expression language

Специальный язык , позволяющий максимально просто вставлять в страницу JSP результаты выражений.

Общий вид;

```Java
${выражение}
```

### Объекты в EL

![[Untitled 63.png]]

  

### Операторы EL

![[Untitled 64.png]]

### Пример EL в JSP

```Java

@WebServlet("/content")
public class ContentServlet extends HttpServlet {
    private static final ArrayList<Object> flights = new ArrayList<>(10){{
        add(new Object());
        add(new Object());
    }};
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("flights", flights);

        req.getRequestDispatcher("/WEB-INF/jsp/content.jsp")
                .forward(req,resp);
    }
}
```

content.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%@ include file="header.jsp" %>
<div>
    <span> Content. Русский </span>
    <p>Size: ${requestScope.flights.size()}</p>
    <p>id: ${requestScope.flights[1]}</p>
    <p>JSESSION ID: ${cookie["JSESSIONID"].value}, unique identifier</p>
    <p>Header: ${header["Cookie"]}</p>
    <p>Param test: ${param.test}</p>
</div>
<%@ include file="footer.jsp" %>
</body>
</html>
```

![[Untitled 65.png]]

  

### JSTL

[http://bsac.by/projects/eemc/java/theory/files/L18.pdf](http://bsac.by/projects/eemc/java/theory/files/L18.pdf) PDF теория по всему JSTL

### Добавление в проект

[https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api](https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api)

[https://mvnrepository.com/artifact/org.glassfish.web/jakarta.servlet.jsp.jstl](https://mvnrepository.com/artifact/org.glassfish.web/jakarta.servlet.jsp.jstl)

![[Untitled 66.png]]

  

### Условные выражения, циклы

Общий вид `if` выражения:

```XML
<c:if test="${условие}">
Выводимый результат
</c:if?>
```

Пример `if` выражения::

```XML
<c:if test="${requestScope.user.role eq 'ADMIN'}>
<button type="submit"> DELETE </button>
</c:if>
```

Пример `if else` выражения:

```XML
<c:choose>

<c:when test="{requestScope.user.role eq 'ADMIN'}">
<span>Hey admin! </span>
</c:when>

<c:when test="{requestScope.user.role eq 'USER'}">
<span> Hello user, ${requestScope.user.name}!</span>
</c:when>

<c:otherwise>
<span> Hello anonymous!</span>
</c:otherwise>

</c:choose>
```

Пример цикла

```XML
<C:forEach var="ticket" items="${requestScope.tickets}">
<p>${ticket.seatNo}</p>
</c:forEach>

<C:forEach var="index" begin="0" end="10">
<p>${requestScope.tickets[index].seatNo}</p>  // Во коллекция должна поддерживать получение элемента по индексу, с Set такой способ не сработает
</c:forEach>
```

  

### Функции JSTL

Теги из этой библиотеки выполняют роль, подобную смыслу библиотеки `fmt`, только не для чисел и дат, а для строк. Теги библиотеки используют пре-

фикс `fn` и во многом копируют известные методы класса `String` и не составляют особой сложности в применении.

Подключение библиотеки осуществляется директивой `taglib`:

```XML
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

Список тегов:

- `${fn:length(аргумент)}` — подсчитывает число элементов в коллекции или длину строки;
- `${fn:toUpperCase(String str)}` и `${fn:toLowerCase(String str)}` — изменяет регистр строки;
- `${fn:substring(String str, int from, int to)}` — извлекает подстроку;
- `${fn:substringAfter(String str, String after)}` и `${fn:substringBefore(String str, String before)}` — извлекает подстроку до или после указанной во втором аргументе;
- `${fn:trim(String str)}` — обрезает все пробелы по краям строки;
- `${fn:replace(String str, String str1, String str2)}` — заменяет все вхождения строки str1 на строку str2;
- `${fn:split(String str, String delim)}` — разбивает строку на подстроки, используя delim, как разделитель;
- `${fn:join(массив, String delim)}` — соединяет массив в строку, вставляя между элементами подстроку delim;
- `${fn:escapeXml(String xmlString)}` — сохраняет в обрабатывемой строке теги;
- `${fn:indexOf(String str, String searchString)}` — возвращает индекс первого вхождения строки searchString;
- `<c:if test="${fn:startsWith(String str, String part)}">` — возвращает истину, если строка начинается с подстроки part;
- `<c:if test="${fn:endsWith(String str, String part)}">` — возвращает истину, если строка заканчивается на подстроку part;
- `<c:if test="${fn:contains(String name, String searchString)}">` — возвращает истину, если строка содержит подстроку searchString;
- `<c:if test="${fn:containsIgnoreCase(String name, String searchString))}">` — возвращает истину, если строка содержит подстроку searchString, без учета регистра

```Java
@WebServlet("/content")
public class ContentServlet extends HttpServlet {
    private static final FlightService flightService = FlightService.getInstance();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("flights", flightService.findAll());

        req.getRequestDispatcher("/WEB-INF/jsp/flights.jsp")
                .forward(req,resp);


    }
}
```

flights.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" pageEncoding="UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<html>
<head>
    <title>Title</title>
</head>
	<body>
		<h1> Список перелетов: </h1>
		<ul>
	    <c:forEach var="flight" items="${requestScope.flights}">
        <li>
						<a href="${pageContext.request.contextPath}/tickets?flightId=${flight.id}">${flight.description}</a>
				</li>
	    </c:forEach>
		</ul>
	</body>
</html>
```

==ВАЖНО!:== ==pageContext.request.contextPath== добавляет путь к application context, если мы поменяли это в конфигурациях tomcat==.==

В итоге будет ==htttp://localhost:8080==/==ourApp==/==tickets====?flightId=3==

![[Untitled 67.png]]

![[Untitled 68.png]]

### Интернационализация в JSTL

Для работы с этим механизмом необходимо подключить к jsp странице библиотеку `fmt`:

```XML
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

### Отображение ошибки в виде jsp страницы

error,jsp:

```Java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    Error! Ooops
</body>
</html>
```

web.xml:

```Java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <error-page>
        <exception-type>java.lang.Throwable</exception-type>
        <location>/WEB-INF/jsp/error.jsp</location>
    </error-page>
</web-app>
```

  

### Servlet Filters

Servlet Filters являются мощным механизмом веб-программирования в Java, предназначенным для обработки и модификации запросов и ответов веб-приложения до и после их достижения сервлетов или JSP-страниц. Они выполняют промежуточную обработку HTTP-запросов и ответов, что позволяет вам применять общие функции или преобразования для нескольких сервлетов или JSP-страниц.

![[Untitled 69.png]]

```Java
@WebFilter("/*")
public class CharsetFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding(StandardCharsets.UTF_8.name());
        response.setCharacterEncoding(StandardCharsets.UTF_8.name());
        chain.doFilter(request,response);
    }
}
```

### Ограничение применения фильтра с помощью servletNames

Также можно завязать фильтр на конкретный список названий сервлетов:

```Java
@WebServlet(value = "/register", name = "RegistrationServlet")
public class RegistrationServlet extends HttpServlet {}
```

```Java
@WebFilter(servletNames = {
        "RegistrationServlet",
        "ContentServlet"
})
public class CharsetFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding(StandardCharsets.UTF_8.name());
        response.setCharacterEncoding(StandardCharsets.UTF_8.name());
        chain.doFilter(request,response);
    }
}
```

### dispatcherTypes

Также можно задавать параметры через @WebInitParam (аналог метода init(FilterConfig config) и через dispatcherTypes указать в каком случае срабатывает фильтр( по умолчанию DispatcherType.REQUEST)

```Java
@WebFilter(value = "/*", servletNames = {
        "RegistrationServlet",
        "ContentServlet"
},
initParams = {
        @WebInitParam(name="param1", value = "param1Value"),
        @WebInitParam(name="param2", value =  "param2Value")
},
        dispatcherTypes = DispatcherType.ERROR
)
```

В Jakarta Servlet Specification, `dispatcherTypes` является одним из параметров фильтра, который определяет, в каких ситуациях фильтр должен быть применен к запросу. Значение `dispatcherTypes` может быть одним или комбинацией следующих констант:

1. `REQUEST` (запрос) - фильтр будет применен к запросам, поступающим непосредственно от клиента.
2. `FORWARD` (переадресация) - фильтр будет применен к запросам, перенаправленным на другой ресурс внутри сервера.
3. `INCLUDE` (включение) - фильтр будет применен к запросам, включающим другой ресурс внутри сервера.
4. `ERROR` (ошибка) - фильтр будет применен к запросам, обрабатываемым обработчиком ошибок.
5. `ASYNC` (асинхронный) - фильтр будет применен к запросам, обработка которых осуществляется асинхронно.

Эти значения могут быть комбинированы, указывая несколько констант через запятую. Например, `REQUEST, FORWARD` означает, что фильтр будет применен к запросам непосредственно от клиента и к переадресованным запросам.

Данные типы диспетчеризации позволяют вам контролировать, как фильтр обрабатывает различные типы запросов в сервлетном контейнере Jakarta. Вы можете использовать их для выполнения различных действий, таких как проверка и модификация запросов, аутентификация и авторизация, установка кодировки и других манипуляций с запросами и ответами.

  

  

### Перенаправление при невыполнении условий фильтрации

```Java
@WebServlet("/admin")
public class UnsafeFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        var user =  (UserDto) ((HttpServletRequest) request).getSession().getAttribute("user");
        if(user == null){
            chain.doFilter(request,response);
        }
        else{
            ((HttpServletResponse) response).sendRedirect("/registration");
        }
    }
}
```

### Цепочки фильтров

Для добавления нескольких фильтров в цепочку вы можете использовать различные методы, включая указание порядка регистрации в файле  
развёртывания (  
`web.xml`) или с помощью аннотаций `@WebFilter`

- Регистрация фильтров в файле развёртывания (web.xml):  
    В файле web.xml вы можете определить порядок регистрации фильтров, указывая элемент \<filter-mapping> для каждого фильтра. Ниже приведен пример web.xml , который демонстрирует регистрацию нескольких фильтров и их порядок выполнения:  
    
    ```Java
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
        <filter>
            <filter-name>CharsetFilter</filter-name>
            <filter-class>filter.CharsetFilter</filter-class>
        </filter>
        <filter>
            <filter-name>LoginFilter</filter-name>
            <filter-class>filter.LoginFIlter</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>CharsetFilter</filter-name>
            <url-pattern>/login</url-pattern>
        </filter-mapping>
        <filter-mapping>
            <filter-name>LoginFilter</filter-name>
            <url-pattern>/login</url-pattern>
        </filter-mapping>
        <servlet>
            <servlet-name>LoginServlet</servlet-name>
            <servlet-class>util.LoginServlet</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>LoginServlet</servlet-name>
            <url-pattern>/login</url-pattern>
        </servlet-mapping>
    </web-app>
    ```
    
    ==ВАЖНО:== Если добавлять URL с помощью маппинга в web.xml не нужно его добавлять его в аргумент аннотации, иначе возникнет ошибка  
    (когда я продублировал @WebServlet(”/login”) получил это  
    ==Caused by: java.lang.IllegalArgumentException: The servlets named [LoginServlet] and [util.LoginServlet] are both mapped to the url-pattern [/login] which is not permitted==
    
    ```Java
    
    @WebFilter
    public class CharsetFilter implements Filter {
        @Override
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
            System.out.println("Cработал CharSetFilter");
            request.setCharacterEncoding(StandardCharsets.UTF_8.name());
            response.setCharacterEncoding(StandardCharsets.UTF_8.name());
            chain.doFilter(request, response);
        }
    }
    
    @WebServlet
    public class LoginServlet extends HttpServlet {
    
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("Cработал сервлет");
        }
    }
    ```
    

### Аутентификация, logout, авторизация

### аутентификация

![[Untitled 70.png]]

login.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Login</title>
</head>
<body>
<form: action="${pageContext.request.contextPath}/login" method="post">
    <label for="email">Email:
    <input type="text" name="email" id="email" required>
</label><br>
    <label for="password">Password:
        <input type="password" name="password" id="password" required>
    </label><br>
     
    <button href="${pageContext.request.contextPath}/registration">
        <button type="button">Register</button>
    </a>
        <c:if test="${param.error != null}">
        <div style="color: red">
            <span> Error </span>
        </c:if>
</form:>
</body>
</html>
```

```Java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {

    private final UserService service = UserService.getInstance();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("login")
                .forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        service.login(req.getParameter("email"), req.getPart("password"))
                .ifPresentOrElse(
                        user-> onLoginSuccess(user,req,resp),
                        ()-> onLoginFail(req,resp)
                );
    }
    @SneakyThrows
    private void onLoginFail(HttpServletRequest req, HttpServletResponse resp) {
        resp.sendRedirect("/login?error&email=" + req.getParameter("email"));
    }

    @SneakyThrows
    private void onLoginSuccess(UserDto user, HttpServletRequest req, HttpServletResponse resp){
        req.getSession().setAttribute("user", user);
        resp.sendRedirect("/flights");
    }
}
```

Дальше необходимо реализовать метод public Optional\<UserDto> login(String email, Part password).

![[Untitled 71.png]]

```Java
package mapper;

import dto.UserDto;
import entity.User;
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

@Mapper
public interface UserMapper {

    UserMapper INSTANCE = Mappers.getMapper(UserMapper.class);

    @Mapping(target = "id", source = "user.id")
    @Mapping(target = "name", source = "user.name")
    @Mapping(target = "email", source = "user.email")
    UserDto mapToDto(User user);

    @Mapping(target = "id", ignore = true)
    @Mapping(target = "name", source = "userDto.name")
    @Mapping(target = "email", source = "userDto.email")
    User mapToEntity(UserDto userDto);
}
```

### logout

Метод invalidate() делает сессию невалидной, удаляет из ассоциативного массива, который хранится на сервере

```Java
@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().invalidate();
        resp.sendRedirect("/login");
    }
}
```

Чтобы доступ к кнопке был в любом месте приложения, необходимо добавить ее в header.jsp, и добавлять этот header,jsp с помощью <%@ include file =”header.jsp”%> в другие jsp страницы.

```Java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<div>
    <c:if test="${not empty sessionScope.user}">
        <form action="${pageContext.request.contextPath}/logout" method="post">
            <button type ="submit">Logout </button>
        </form>
    </c:if>
</div>
```

### Авторизация

```Java
@WebFilter("/*")
public class AuthorizationFilter implements Filter {
    private static final Set<String> PUBLIC_PATH = Set.of("/login", "/registration");
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        String requestURI = ((HttpServletRequest) request).getRequestURI();
        if(isPublicPath(requestURI) || isUserLoggedIn(request)){
            chain.doFilter(request,response);
        } else{
            String prevPage = ((HttpServletRequest) request).getHeader("referer");
            ((HttpServletResponse) response).sendRedirect(prevPage!= null ? prevPage : "/login");
        }
        
    }

    private boolean isUserLoggedIn(ServletRequest request) {
        Object user = ((HttpServletRequest) request).getSession().getAttribute("user");
        UserDto userDto = (UserDto) user;
        return userDto != null;
//      return userDto != null && userDto.getRole() == Role.USER;
    }

    private boolean isPublicPath(String requestURI) {
        return PUBLIC_PATH.stream().anyMatch(requestURI::startsWith);
    }
}
```

![[Untitled 72.png]]

![[Untitled 73.png]]

![[Untitled 74.png]]

  

  

# Общие сведения о компьютерных сетях

## Лекция "Классификация сетей" (презентация).

![[Untitled 75.png]]

![[Untitled 76.png]]

![[Untitled 77.png]]

![[Untitled 78.png]]

![[Untitled 79.png]]

![[Untitled 80.png]]

## Лекция "Модель OSI"

![[Untitled 81.png]]

  

![[Untitled 82.png]]

![[Untitled 83.png]]

![[Untitled 84.png]]

![[Untitled 85.png]]

![[Untitled 86.png]]

![[Untitled 87.png]]

![[Untitled 88.png]]

- На практике сеансовый уровень не используется

![[Untitled 89.png]]

![[Untitled 90.png]]

![[Untitled 91.png]]

![[Untitled 92.png]]

  

## Лекция "Модель и стек протоколов TCP/IP"

![[Untitled 93.png]]

![[Untitled 94.png]]

![[Untitled 95.png]]

![[Untitled 96.png]]

  

  

### **Классический Ethernet (Shared Ethernet):**

- **Характеристики**: В классическом Ethernet используется среда передачи, общая для всех узлов сети. Это означает, что все устройства в сети могут слышать все передаваемые кадры.
- **Коллизии**: Поскольку все устройства используют общую среду передачи, могут возникать коллизии (столкновения) данных, когда несколько устройств пытаются передать данные одновременно.
- **Метод доступа**: В классическом Ethernet используется метод доступа CSMA/CD (Carrier Sense Multiple Access with Collision Detection), который позволяет устройствам контролировать доступ к среде передачи и обнаруживать коллизии.

### **Коммутированный Ethernet (Switched Ethernet):**

- **Характеристики**: В коммутированном Ethernet используются коммутаторы (switches), которые обеспечивают индивидуальное соединение между каждой парой устройств в сети. Каждое устройство имеет свое собственное соединение с коммутатором.
- **Отсутствие коллизий**: Поскольку каждое устройство имеет свое собственное соединение с коммутатором, коллизии данных практически исключены.
- **Метод доступа**: Коммутаторы используют коммутацию (switching), что означает, что они могут перенаправлять кадры только на те порты, на которые адресованы.

### **Основное отличие:**

Основное отличие между коммутированным и классическим Ethernet заключается в том, что в коммутированном Ethernet используется коммутатор для обеспечения индивидуального соединения между устройствами, в то время как в классическом Ethernet используется общая среда передачи, что может привести к коллизиям данных. Коммутированный Ethernet обеспечивает более высокую производительность и надежность сети, поскольку исключает коллизии и обеспечивает более эффективное использование пропускной способности.

# Модель OSI - именно про модель и общую концепцию всего

- `Прикладной уровень` Получает информацию, как есть в данных, которые ввел пользователь.

![[Untitled 97.png]]

- Заключается в том, чтобы данные `превратить в массив байт` (==Обязательно==)
- ==Шифрование/компрессия== (бысрее передавать по проводам) (необязательно)

![[Untitled 98.png]]

- `Session` - время открытие и закрытия соединение, когда общаются. (==Просто открывается соединение== и отправляются данные на уровень ниже)

![[Untitled 99.png]]

- ==В сети могут происходить сбои, пакеты могут быть битые и транспортный уровень занимается этим.==

![[Untitled 100.png]]

![[Untitled 101.png]]

- Разбиение сегмента на пакеты данных
- Доставка между разными сетями

![[Untitled 102.png]]

![[Untitled 103.png]]

![[Untitled 104.png]]

![[Untitled 105.png]]

![[Untitled 106.png]]

![[Untitled 107.png]]

![[Untitled 108.png]]

  

- `Канальный уровень` - проверяем целостность кадров.

![[Untitled 109.png]]

![[Untitled 110.png]]

![[Untitled 111.png]]

  

![[Untitled 112.png]]

  

# Сетевая маска

![[Untitled 113.png]]

![[Untitled 114.png]]

[localhost](http://localhost)

![[Untitled 115.png]]

![[Untitled 116.png]]

- ==Маска используется только для отправителя== (Потому что нужно определить подсеть, чтобы понять адрес сети)
- ==И когда у получается совпадает адрес сети, тогда маршрутизатор не отправит в другую сеть.==

![[Untitled 117.png]]

![[Untitled 118.png]]

![[Untitled 119.png]]

- `traceroate`

  

# DNS

![[Untitled 120.png]]

![[Untitled 121.png]]

![[Untitled 122.png]]

![[Untitled 123.png]]

![[Untitled 124.png]]

![[Untitled 125.png]]

- ==Происходит цепочка вызовов нужных доменов по уровням.==
- После ==получения доменого имени и IP адреса происходит уже запрос по IP==
- ==Кешируются== такие запросы локально и в днс серере (сразу вернут адрес ip)

==**Как это выглядит**==

![[Untitled 126.png]]

==Сначала в приоритете проверяется локальный файл== ==`hosts`====, если там не находится информация, которая нужна,то запрос идет по рутовоу доменному имени, получает== ==`ip`== ==следующей зоны и тд.==

  

# TCP / UDP

![[Untitled 127.png]]

- `TCP` и `UDP` ==могут быть одновременно на одном порту, так как компьютер обрабатывает их по разному==

# HTTP

![[Untitled 128.png]]

![[Untitled 129.png]]

![[Untitled 130.png]]

![[Untitled 131.png]]

![[Untitled 132.png]]

![[Untitled 133.png]]

![[Untitled 134.png]]

![[Untitled 135.png]]

![[Untitled 136.png]]

![[Untitled 137.png]]

Да, ==**можно получать данные через POST и отправлять данные через GET,**== но это не рекомендуется в соответствии с общепринятыми стандартами и семантикой HTTP. Передача конфиденциальных данных через `URL` запроса `GET` ==является небезопасной практикой, так как данные могут быть видны в адресной строке браузера или сохранены в истории браузера==. Вместо этого рекомендуется использовать методы соответственно их предназначению: `GET` для получения данных, `POST` для отправки данных.

Строго говоря, согласно спецификации протокола HTTP, ==метод GET не предполагает наличие тела сообщения==. GET-запросы используются исключительно для запроса ресурсов и передачи данных через URL строки запроса (query string). Таким образом, технически GET-запрос не имеет тела сообщения.

==Тем не менее, некоторые клиенты и серверы могут позволять включать тело сообщения в GET-запросы.== ==Однако это считается нарушением стандартов и может привести к непредсказуемому поведению==. Например, некоторые серверы могут проигнорировать тело сообщения в GET-запросе, а другие могут вернуть ошибку.

Поэтому, хотя некоторые реализации могут позволять вставлять тело сообщения в GET-запросы, это не является хорошей практикой и может привести к несовместимости и непредсказуемому поведению вашего приложения. Вместо этого, для передачи данных между клиентом и сервером через HTTP следует использовать методы, предназначенные для этой цели, такие как POST или PUT.

![[Untitled 138.png]]

![[Untitled 139.png]]

# MIME type

![[Untitled 140.png]]

![[Untitled 141.png]]

  

# Минусы Rest

![[Untitled 142.png]]

![[Untitled 143.png]]

![[Untitled 144.png]]