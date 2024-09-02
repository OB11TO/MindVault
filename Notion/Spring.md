---
modified: 2024-09-02T15:33:46+03:00
---
[https://www.notion.so/ob11to/Hibernate-bb45802417ac413e95455d116aba4672](https://www.notion.so/Hibernate-bb45802417ac413e95455d116aba4672?pvs=21)

## Tom: Хранилище разобрать позже

### REST

REST - это патттерн проектирования web приложений. Описывает, как по протоколу HTTP взаимодействовать с сервером для чтения, добавления, изменения, удаления данных. Описывает, какие URL, HTTP методы использовать.

CRUD (Create, Read, Update, Delete) и REST (Representational State Transfer) - это два разных концепта, но они могут быть взаимосвязаны в контексте разработки веб-приложений и API. Вот как они связаны:

1. **CRUD операции в RESTful API:**
    - В RESTful API, CRUD операции соответствуют стандартным HTTP методам:
        - `Create` соответствует HTTP методу `POST`. Он используется для создания новых ресурсов на сервере.
        - `Read` соответствует HTTP методу `GET`. Он используется для получения информации о ресурсах.
        - `Update` соответствует HTTP методу `PUT` или `PATCH`. Они используются для обновления существующих ресурсов. `PUT` полностью заменяет ресурс, в то время как `PATCH` обновляет только указанные атрибуты ресурса.
        - `Delete` соответствует HTTP методу `DELETE`. Он используется для удаления ресурсов.
2. **RESTful ресурсы:**
    - В RESTful API, каждый ресурс (например, объект или сущность) имеет уникальный URI, который является адресом для выполнения CRUD операций. Например:
        - Создание нового ресурса: `POST /api/users`
        - Получение информации о ресурсе: `GET /api/users/{id}`
        - Обновление ресурса: `PUT /api/users/{id}` или `PATCH /api/users/{id}`
        - Удаление ресурса: `DELETE /api/users/{id}`
3. **Операции и ресурсы вместе:**
    - CRUD операции и RESTful ресурсы работают вместе, чтобы обеспечить доступ к данным и их управление через HTTP. Вы проектируете API так, чтобы URI и методы соответствовали операциям CRUD для ваших ресурсов.
4. **Принцип REST:**
    - REST также определяет ограничения и принципы для проектирования API, такие как использование HTTP методов, Stateless (отсутствие состояния), ограниченный интерфейс и другие. Эти принципы помогают создавать простые, гибкие и масштабируемые веб-сервисы.

Итак, связь между CRUD и REST заключается в том, что CRUD операции могут быть реализованы в RESTful API, используя стандартные HTTP методы и URI для управления ресурсами. REST предоставляет архитектурные принципы и ограничения, которые облегчают создание эффективных и удобочитаемых API для доступа и управления данными.

Принципы REST (Representational State Transfer) представляют собой набор ограничений и принципов, которые определяют архитектурный стиль для проектирования веб-сервисов и веб-приложений. Эти принципы были впервые представлены в докторской диссертации Роя Филдинга в 2000 году и стали широко применяться при разработке веб-систем. Вот более подробное описание некоторых основных принципов REST:

1. **Использование HTTP методов:** REST использует стандартные методы HTTP (GET, POST, PUT, DELETE и другие) для выполнения операций над ресурсами. Это означает, что каждая операция имеет семантику, связанную с соответствующим HTTP методом. Например, `GET` используется для получения данных, `POST` - для создания новых данных, `PUT` - для обновления данных, и `DELETE` - для удаления данных. Это делает API более интуитивным и понятным.
2. **Отсутствие состояния (Stateless):** Сервер не хранит состояние между запросами от клиента. Каждый запрос от клиента должен содержать всю необходимую информацию для выполнения запроса. Это облегчает масштабирование серверов и уменьшает сложность взаимодействия между клиентом и сервером.
3. **Ограниченный интерфейс (Uniform Interface):** RESTful API должен предоставлять унифицированный и ограниченный интерфейс, который облегчает взаимодействие клиента и сервера. Это означает, что интерфейс должен быть простым и предсказуемым, и клиенты должны иметь представление о том, как взаимодействовать с ресурсами на основе URI и методов HTTP.
4. **Ресурсы и представления:** Все данные в RESTful API представлены как ресурсы, и каждый ресурс имеет уникальный URI. Ресурсы могут иметь разные представления, такие как JSON, XML или HTML. Клиенты могут выбирать представление, которое им нужно, и манипулировать ресурсами через URI и методы HTTP.
5. **Слои (Layered System):** Сервисы могут быть реализованы с использованием слоев, где каждый слой выполняет определенные функции, такие как балансировка нагрузки, кэширование, безопасность и другие. Это повышает гибкость и масштабируемость системы.
6. **Кеширование (Caching):** Сервер может указать, что определенные ответы могут быть кэшированы на стороне клиента. Это может улучшить производительность и снизить нагрузку на сервер.
7. **Код по требованию (Code on Demand, не обязательно):** Этот аспект REST не является обязательным и используется нечасто. Он предоставляет возможность серверу отправлять клиенту исполняемый код, который клиент может выполнить, например, JavaScript. Это может быть полезно для создания более интерактивных веб-приложений.

Все эти принципы REST способствуют созданию простых, гибких и масштабируемых веб-сервисов и API, которые легко понимать и поддерживать. Они также способствуют повышению производительности и безопасности веб-приложений.

### Spring MVC

MVC паттерн проектирования приложений

- Model - логика работы с данными
- View логика представления
- Controller логика навигации, обработка запросов

![[images/Untitled 158.png|Untitled 158.png]]

Импорт `org.springframework.ui.Model` относится к Spring Framework и используется в контексте веб-приложений с целью передачи данных между контроллером и представлением. Этот класс представляет собой интерфейс, который позволяет контроллерам добавлять атрибуты, которые могут быть использованы в представлениях.

Вот для чего используется `Model`:

1. **Передача данных из контроллера в представление:** Контроллеры в Spring MVC могут использовать объект `Model`, чтобы добавлять атрибуты, которые будут доступны в представлении. Эти атрибуты могут быть данными, которые должны быть отображены в HTML-странице.
2. **Инкапсуляция данных:** `Model` представляет собой абстракцию для передачи данных. Вместо того чтобы передавать данные напрямую, контроллеры могут добавлять их как атрибуты `Model`, что делает код более структурированным и удобным для тестирования.

Пример использования `Model` в контроллере:

```Java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MyController {

    @GetMapping("/hello")
    public String sayHello(Model model) {
        // Добавляем атрибут "message" в объект Model
        model.addAttribute("message", "Привет, мир!");

        // Возвращаем имя представления, которое должно отобразить этот атрибут
        return "helloPage";
    }
}
```

Здесь `Model` используется для добавления атрибута "message", который будет доступен в представлении с именем "helloPage". Представление может использовать этот атрибут для отображения приветственного сообщения.

В итоге, `Model` в Spring MVC служит как посредник между контроллером и представлением, облегчая передачу данных из контроллера в представление для отображения на веб-странице.

### Обойти ограничения HTML без использования ThymeLeaf

Если вы хотите обойти ограничения HTML и отправлять `PATCH` запросы, используя `POST` запросы с полем `hidden`, вам придется реализовать это вручную. В Spring Framework для обработки таких запросов можно использовать аспекты, интерцепторы или фильтры. Вот общий подход к решению этой задачи:

1. **На стороне клиента (HTML):**
    
    В вашем HTML-коде создайте форму, которая будет отправлять `POST` запрос с полем `hidden`, содержащим информацию о том, что это `PATCH` запрос. Например:
    
    ```HTML
    <form action="/update" method="post">
        <input type="hidden" name="_method" value="PATCH">
        <input type="hidden" name="id" value="123">
        <!-- Другие поля формы -->
        <button type="submit">Отправить PATCH запрос</button>
    </form>
    ```
    
    Здесь `_method` - это скрытое поле, которое указывает метод HTTP, который вы хотите использовать (`PATCH` в данном случае).
    
2. **На стороне сервера (Spring):**

В Spring Framework вам нужно настроить сервер, чтобы он корректно интерпретировал `POST` запросы с `_method` в качестве `PATCH` запросов. Вам потребуется подключить `HiddenHttpMethodFilter`:

![[images/Untitled 1 19.png|Untitled 1 19.png]]

```Java
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class WebConfig {

    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter() {
        return new HiddenHttpMethodFilter();
    }
}
```

Этот фильтр позволит Spring интерпретировать `_method` в `POST` запросах как метод HTTP для обработки.

1. **Контроллер для обработки** `**PATCH**` **запросов:**
    
    В вашем контроллере Spring Framework вы можете обрабатывать `PATCH` запросы так же, как и любые другие HTTP методы. Например:
    
    ```Java
    import org.springframework.web.bind.annotation.*;
    
    @RestController
    @RequestMapping("/update")
    public class UpdateController {
    
        @PatchMapping("/{id}")
        public ResponseEntity<String> updateResource(@PathVariable Long id, @RequestBody UpdateRequest request) {
            // Обработка PATCH запроса
            // ...
            return ResponseEntity.ok("Resource updated");
        }
    }
    ```
    
    В этом примере `@PatchMapping` используется для обработки `PATCH` запросов, и данные передаются в теле запроса через `@RequestBody`.
    

Теперь вы можете отправлять `PATCH` запросы с помощью формы, в которой используется скрытое поле `_method`, и Spring будет обрабатывать их как `PATCH` запросы. Не забудьте настроить обработку ошибок и проверки безопасности, в зависимости от ваших требований.

### Аннотации @PathVariable и @RequestParam

Аннотации `@PathVariable` и `@RequestParam` в Spring Framework используются для извлечения параметров из URL и запроса соответственно, но они работают с разными типами параметров и местами, откуда они извлекаются. Вот основные различия между этими двумя аннотациями:

1. `@PathVariable`:
    - Извлекает параметры из пути URL (URI шаблона).
    - Параметры передаются в части URL после слэшей, например, `/users/{userId}`.
    - Чаще всего используется для извлечения параметров, которые встроены непосредственно в URL-путь.
    - Не требует явного указания имени параметра, так как имя параметра совпадает с именем переменной в аннотации.

Пример использования `@PathVariable`:

```Java
@GetMapping("/users/{userId}")
public ResponseEntity<User> getUserById(@PathVariable Long userId) {
    // Логика для получения пользователя по userId
}
```

1. `@RequestParam`:
    - Извлекает параметры из строки запроса (query string), которая идет после символа `?` в URL.
    - Параметры передаются в виде `ключ=значение` в URL, например, `/users?name=John&age=30`.
    - Чаще всего используется для извлечения параметров, которые передаются как запросы от клиента.
    - Требует явного указания имени параметра с помощью атрибута `value` или `name` в аннотации.

Пример использования `@RequestParam`:

```Java
@GetMapping("/users")
public ResponseEntity<List<User>> getUsersByFilter(
    @RequestParam("name") String name,
    @RequestParam("age") int age) {
    // Логика для получения пользователей по параметрам запроса
}
```

В зависимости от вашего конкретного использования, вы можете выбрать между `@PathVariable` и `@RequestParam` для извлечения параметров из запроса. Если параметры передаются как часть URL-пути, используйте `@PathVariable`, а если они передаются как параметры запроса, используйте `@RequestParam`.

  

## Tom: Дополнение

### Schedule Tasks

@Scheduled

Аннотация определяет, когда запускается конкретный метод.

Метод с аннотацией @Scheduled должен возвращать void и не должен принимать на вход параметры.

В качество аргументов используется:

- fixedRate - интервал между запусками метода
- fixedDelay - интервал после завершения предыдущего исполнения метода
- initialDelay - первое исполнение начнется после указанной задержки
- cron - позволяет установить расписание cron-выражением:

Например, этот метод будет запускаться в 10:15 каждый 15 день месяца

```Java
@Scheduled(cron = "0 15 10 15 * ?", zone = "Europe/Paris")
```

Параметры задержки можно подставлять из файла конфигурации:

```Java
@Scheduled(fixedDelayString = "${fixedDelay.in.milliseconds}")

@Scheduled(fixedRateString = "${fixedRate.in.milliseconds}")

@Scheduled(cron = "${cron.expression}")
```

Или прописывать в XML-config:

```Java
<!-- Configure the scheduler -->
<task:scheduler id="myScheduler" pool-size="10" />

<!-- Configure parameters -->
<task:scheduled-tasks scheduler="myScheduler">
    <task:scheduled ref="beanA" method="methodA" 
      fixed-delay="5000" initial-delay="1000" />
    <task:scheduled ref="beanB" method="methodB" 
      fixed-rate="5000" />
    <task:scheduled ref="beanC" method="methodC" 
      cron="*/5 * * * * MON-FRI" />
</task:scheduled-tasks>
```

@EnableScheduling - данную аннотацию необходимо добавить в класс Конфигурации.

В примере используется @SpringBootApplication, которая включает в себя @Configuration:

```Java
@SpringBootApplication
@EnableScheduling
public class SchedulingTasksApplication {

	public static void main(String[] args) {
		SpringApplication.run(SchedulingTasksApplication.class);
	}
}
```

Пример использования расписания:

```Java
@Component
public class ScheduledTasks {

	private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

	private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

	@Scheduled(fixedRate = 5000)
	public void reportCurrentTime() {
		log.info("The time is now {}", dateFormat.format(new Date()));
	}
}
```

### Task Scheduler

Класс `**TaskScheduler**` является интерфейсом в Spring Framework, который предоставляет возможности для планирования выполнения задач и управления временем в приложении.

1. Интерфейс `**TaskScheduler**` определяет несколько методов для планирования и запуска задач. Основные методы включают:
    - `**ScheduledFuture<?> schedule(Runnable task, Trigger trigger)**`: Планирует выполнение задачи типа `**Runnable**` с использованием объекта `**Trigger**`, который определяет условия выполнения задачи.
    - `**ScheduledFuture<?> schedule(Runnable task, Date startTime)**`: Планирует выполнение задачи типа `**Runnable**` в указанное время `**startTime**`.
    - `**ScheduledFuture<?> scheduleAtFixedRate(Runnable task, Date startTime, long period)**`: Планирует выполнение задачи типа `**Runnable**` с фиксированной периодичностью `**period**` (в миллисекундах), начиная с указанного времени `**startTime**`.
    - `**ScheduledFuture<?> scheduleWithFixedDelay(Runnable task, Date startTime, long delay)**`: Планирует выполнение задачи типа `**Runnable**` с фиксированной задержкой `**delay**` (в миллисекундах) между завершением предыдущего выполнения и началом следующего выполнения, начиная с указанного времени `**startTime**`.
2. `**TaskScheduler**` предоставляет возможность выполнения задач в фоновых потоках. Вы можете настроить пул потоков для асинхронного выполнения задач, чтобы не блокировать основной поток приложения.
3. Spring предоставляет несколько реализаций интерфейса `**TaskScheduler**`, таких как `**ThreadPoolTaskScheduler**`, `**ConcurrentTaskScheduler**`, `**ThreadPoolExecutor**`, и другие. `**ThreadPoolTaskScheduler**` является наиболее распространенной реализацией, которая использует пул потоков для выполнения задач.

Trigger

```Java
@Component
public class MyTrigger implements Trigger {

    @Override
    public Date nextExecutionTime(TriggerContext triggerContext) {
        Calendar nextExecutionTime = Calendar.getInstance();
        nextExecutionTime.setTime(new Date());
        nextExecutionTime.add(Calendar.MINUTE, 15); // Добавляем 15 минут к текущему времени
        return nextExecutionTime.getTime();
    }
}
```

1. `**Trigger**` является функциональным интерфейсом, определяющим единственный метод `**nextExecutionTime(TriggerContext triggerContext)**`. Этот метод возвращает следующий момент времени, когда должна быть выполнена задача, на основе контекста триггера.
2. Классы, реализующие интерфейс `**Trigger**`, предоставляют различные стратегии для определения следующего времени выполнения задачи. В Spring Framework включены следующие реализации:
    - `**CronTrigger**`: Позволяет задать время выполнения задачи в виде выражения cron.
    - `**PeriodicTrigger**`: Позволяет задать периодичность выполнения задачи в миллисекундах с опциональным смещением и начальной задержкой.
    - `**SimpleTrigger**`: Позволяет задать частоту выполнения задачи в миллисекундах с опциональной начальной задержкой.
3. Классы, реализующие `**Trigger**`, также могут использовать информацию из объекта `**TriggerContext**` для определения следующего времени выполнения задачи. `**TriggerContext**` предоставляет доступ к предыдущему времени выполнения задачи, предыдущему запланированному времени выполнения и времени запуска приложения.
    
    ### Использование TaskScheduler для планирования задачи
    
    ```Java
    @Component
    public class MyTaskScheduler {
    
        @Autowired
        private TaskScheduler taskScheduler;
    
        @Autowired
        private MyTaskExecutor myTaskExecutor;
    
        @Autowired
        private MyTrigger myTrigger;
    
        private ScheduledFuture<?> scheduledFuture;
    
        public void scheduleDelayedTaskWithParameter(String parameter) {
            Runnable task = () -> myTaskExecutor.delayedTaskWithParameter(parameter);
            scheduledFuture = taskScheduler.schedule(task, myTrigger);
        }
    }
    ```
    

### Email Sender

Настройка сервера по отправке писем через application.properties

```Java
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=<login user to smtp server>
spring.mail.password=<login password to smtp server>
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

В качестве пароля в Gmail необходимо использовать специальный 16-значный пароль для приложений: [https://support.google.com/accounts/answer/185833](https://support.google.com/accounts/answer/185833)

Необходимо подключить зависимость Spring starter mail

### Amazon SES

```Java
spring.mail.host=email-smtp.us-west-2.amazonaws.com
spring.mail.username=username
spring.mail.password=password
spring.mail.properties.mail.transport.protocol=smtp
spring.mail.properties.mail.smtp.port=25
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
```

Please be aware that Amazon requires us to verify our credentials before using them. Follow the [link](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses.html) to verify your username and password.

### Отправка писем

Простое письмо без каких-либо прикрепленных файлов или html-разметки

```Java
@Component
public class EmailServiceImpl implements EmailService {

    @Autowired
    private JavaMailSender emailSender;

    public void sendSimpleMessage(
      String to, String subject, String text) {
        ...
        SimpleMailMessage message = new SimpleMailMessage(); 
        message.setFrom("noreply@baeldung.com");
        message.setTo(to); 
        message.setSubject(subject); 
        message.setText(text);
        emailSender.send(message);
        ...
    }
}
```

Отправка писем с прикрепленными файлами

```Java
@Override
public void sendMessageWithAttachment(
  String to, String subject, String text, String pathToAttachment) {
    // ...
    
    MimeMessage message = emailSender.createMimeMessage();
     
    MimeMessageHelper helper = new MimeMessageHelper(message, true);
    
    helper.setFrom("noreply@baeldung.com");
    helper.setTo(to);
    helper.setSubject(subject);
    helper.setText(text);
        
    FileSystemResource file 
      = new FileSystemResource(new File(pathToAttachment));
    helper.addAttachment("Invoice", file);

    emailSender.send(message);
    // ...
}
```

Для включения html-разметки нужно добавить параметр true после текста сообщения:

```Java
helper.setText(text, true);
```

### Обработка ошибок

JavaMail предоставляет SendFailedException, которая по идее должна выбрасываться, если письмо не может быть отправлено, однако скорее всего это не сработает. Согласно RFC 821 SMTP-сервер должен возвращать ошибку 550, при попытке отправить письмо на некорректный адрес, но большинство серверов этого не делают. Вместо этого тот же Gmail отправляет письмо “delivery failed”. Соответственно для обработки ошибок можно попробовать мониторить ящик на предмет таких писем с помощью Scheduled Task.

### Telegrambot

### Создание бота

1. Добавить зависимость telegrambots (лучше использовать telegrambots-starter)
2. Создать класс для бота:

```Java
@Component
public class TelegramBot implements LongPollingBot {

    private final String botToken = "YOUR_BOT_TOKEN";
    private final String botUsername = "YOUR_BOT_USERNAME";

    @Override
    public void onUpdateReceived(Update update) {
        // Обработка полученного обновления (сообщения)
    }

    @Override
    public String getBotUsername() {
        return botUsername;
    }

    @Override
    public String getBotToken() {
        return botToken;
    }
    public void startBot() {
        TelegramBotsApi botsApi = new TelegramBotsApi();
        try {
            BotSession session = botsApi.registerBot(this);
        } catch (TelegramApiException e) {
            // Обработка ошибок регистрации бота
        }
    }
}
```

Настроить `**YOUR_BOT_TOKEN**` и `**YOUR_BOT_USERNAME**` соответствующими значениями для Telegram бота.

Для того, чтобы добавить их в конфигурационный файл приложения необходимо использовать аннотация @Value.

Для запуска Telegram бота нужно вызвать метод `**startBot**` в классе `**TelegramBot**`**:**

```Java
@SpringBootApplication
public class YourApplication {
    public static void main(String[] args) {
        SpringApplication.run(YourApplication.class, args);

        TelegramBot telegramBot = new TelegramBot();
        telegramBot.startBot();
    }
}
```

В данном примере бот запускается при старте приложения.

### Отправка сообщений

Необходимо добавить в класс, реализующий бота соответствующий метод

```Java
  public void sendMessage(String chatId, String text) {
        SendMessage message = new SendMessage();
        message.setChatId(chatId);
        message.setText(text);
        try {
            execute(message);
        } catch (TelegramApiException e) {
            // Обработка ошибок отправки сообщения
        }
    }
```

Далее на сервисном слое вызвать метод:

```Java
@Service
public class MyService {
    @Autowired
    private TelegramBot telegramBot;

    public void sendTelegramMessage(String chatId, String message) {
        telegramBot.sendMessage(chatId, message);
    }
}
```

## Tom: Основная информация

### IoC Container

![[images/Untitled 2 17.png|Untitled 2 17.png]]

==**IoC Container в Spring:**== В Spring Framework существует два основных типа IoC Container: `BeanFactory` и `ApplicationContext`. Эти контейнеры берут на себя управление объектами, называемыми Spring Beans, и предоставляют им различные службы, такие как создание, настройка, жизненный цикл и внедрение зависимостей.

1. `**Spring Beans**`**:** Spring Beans - это объекты, которые управляются IoC Container. Они описываются с помощью метаданных (как правило, XML или аннотаций) и затем создаются и настраиваются контейнером. Эти бины предоставляют различные службы и функциональность вашего приложения.
2. `**Конфигурация Spring Beans**`**:** Spring Beans могут быть настроены с использованием XML-конфигурации или аннотаций. XML-конфигурация предоставляет более детальное описание бинов и их зависимостей, в то время как аннотации делают конфигурацию более компактной и читаемой.
3. `**Внедрение зависимостей (Dependency Injection)**`**:** Одна из ключевых особенностей Spring - это внедрение зависимостей. IoC Container автоматически внедряет зависимости бинов, что делает управление зависимостями в приложении более гибким и удобным.
4. `**Жизненный цикл Spring Beans**`**:** Контейнер Spring управляет жизненным циклом бинов, включая создание, инициализацию, вызов методов и уничтожение. Вы можете определить методы для настройки и очистки бинов.
    
    ![[images/Untitled 3 16.png|Untitled 3 16.png]]
    

### BeanFactory

Основной интерфейс (IoC container) это BeanFactory, от которого уже наследуются различные Contexts.

```Java
public interface BeanFactory {}
```

![[images/Untitled 4 16.png|Untitled 4 16.png]]

Так как функционал BeanFactory ограничен лучше использовать какую либо имплентацию, некоторые будут рассмотрены ниже в подгруппах.

Например

```Java
var context = new ClassPathXmlApplicationContext("application.xml");
```

Также в качестве параметра можно передать другой контекст:

![[images/Untitled 5 16.png|Untitled 5 16.png]]

  

### Bean Lificycle

![[images/Untitled 6 15.png|Untitled 6 15.png]]

- Жизненный цикл бина состоит из получение метаданных (Bean Definition).
- В самом начале запускается `BeanFactoryPostProcessor`

### BeanFactoryPostProcessor

[https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html)

[https://javarush.com/quests/lectures/questspring.level01.lecture26](https://javarush.com/quests/lectures/questspring.level01.lecture26)

`BeanFactoryPostProcessor` - это интерфейс в Spring Framework, который позволяет воздействовать на контейнер бинов (BeanFactory) до того, как бины будут фактически созданы и настроены. Этот интерфейс предоставляет более ранний уровень вмешательства в процесс конфигурации контейнера Spring.

Главная задача `BeanFactoryPostProcessor` - это изменять метаданные бинов и конфигурацию контейнера до создания бинов и начала их жизненного цикла. Это дает возможность влиять на то, как контейнер будет создавать и настраивать бины до их фактической инициализации.

IoC-контейнер Spring позволяет `BeanFactoryPostProcessor` считывать конфигурационные метаданные и потенциально изменять их _до того, как_ контейнер создаст экземпляры каких-либо бинов, помимо экземпляров `BeanFactoryPostProcessor`.

Если вы хотите изменить фактические экземпляры бинов (то есть объекты, которые создаются на основе конфигурационных метаданных), то необходимо использовать `BeanPostProcessor`. Хотя технически возможно работать с экземплярами бинов внутри `BeanFactoryPostProcessor` (например, используя `BeanFactory.getBean()`), это приводит к преждевременному созданию экземпляра бина, что нарушает стандартный жизненный цикл контейнера. Это может вызвать негативные побочные эффекты, такие как обход пост-обработки бина. После этого осуществляется сортировка метаданных, так как некоторые бины создаются на основе других.

То есть указанный класс `PropertySourcesPlaceholderConfigurer` наследуется от PropertyResourceConfigurer, который в свою очередь как раз имплементирует BeanFactoryPostProcessor. И этот класс перед созданием бина подменит `${db.userName}` на нужное значение. То есть изменит матаданные (bean definition) перед неопрседственной инициализацией бина.

```Java
public abstract class PropertyResourceConfigurer extends PropertiesLoaderSupport
		implements BeanFactoryPostProcessor, PriorityOrdered {
```

```Java
<bean name="configurer"
 class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer"

<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool"
          init-method="init"
          destroy-method="destroyBean">
     <constructor-arg index="0" name="userName" value="${db.userName}"/>
</bean>
```

### Custom BeanFactoryPostProcessor

`BeanFactoryPostProcessor` - это механизм Spring, который позволяет вам вмешиваться в процесс конфигурации бинов до их фактического создания. При работе с несколькими кастомными `BeanFactoryPostProcessor` можно задавать приоритет и порядок выполнения с использованием интерфейсов `Ordered` и `PriorityOrdered`. Давайте разберемся, как это работает:

1. ==**Ordered**==:
    
    Интерфейс `Ordered` определяет один метод, `getOrder()`, который возвращает числовое значение, представляющее порядок выполнения. Бины, реализующие `Ordered`, будут выполняться в порядке от наименьшего к наибольшему значениям `getOrder()`. Меньшие значения означают более высокий приоритет.
    
    Пример:
    
    ```Java
    import org.springframework.core.Ordered;
    import org.springframework.stereotype.Component;
    
    @Component
    public class MyOrderedBeanFactoryPostProcessor implements BeanFactoryPostProcessor, Ordered {
    
        @Override
        public int getOrder() {
            return 1; // Первый по приоритету
        }
    
        @Override
        public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
            // Логика обработки
        }
    }
    ```
    
2. ==**PriorityOrdered**==:
    
    Интерфейс `PriorityOrdered` имеет аналогичное назначение, но бины, реализующие этот интерфейс, выполняются раньше бинов, реализующих только `Ordered`. Если есть несколько бинов, реализующих `PriorityOrdered`, они выполняются в порядке от наименьшего к наибольшему значениям `getOrder()`.
    
    Пример:
    
    ```Java
    import org.springframework.core.PriorityOrdered;
    import org.springframework.stereotype.Component;
    
    @Component
    public class MyPriorityOrderedBeanFactoryPostProcessor implements BeanFactoryPostProcessor, PriorityOrdered {
    
        @Override
        public int getOrder() {
            return -1; // Высший приоритет
        }
    
        @Override
        public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
            // Логика обработки
        }
    }
    ```
    

Теперь о порядке выполнения:

1. Бины, реализующие `PriorityOrdered`, выполняются в порядке от наименьшего к наибольшему значениям `getOrder()`.
2. Затем бины, реализующие только `Ordered`, выполняются в порядке от наименьшего к наибольшему значениям `getOrder()`.
3. Наконец, остальные бины `BeanFactoryPostProcessor`, которые не реализуют ни `PriorityOrdered`, ни `Ordered`, выполняются.

Это позволяет точно управлять порядком выполнения ваших кастомных `BeanFactoryPostProcessor` и гарантировать, что они выполняются в правильной последовательности в зависимости от ваших потребностей.

  

- После метаданные сортируются. Так как создание некоторых бинов зависит от других.
- После этого вызывается конструктор

```Java
public ConnectionPool(String userName, Integer poolSize, List<Object> args, Map<String, Object> properties) {
        this.userName = userName;
        this.poolSize = poolSize;
        this.args = args;
        this.properties = properties;
    }
```

- После вызываются сеттеры, которые могут менять данные уже в созданных бинах.

```Java
public class Person {
    private String name;
    private int age;

    // Сеттеры для полей name и age
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    // Другие методы класса...
}
```

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Определение бина для класса Person -->
    <bean id="personBean" class="com.example.Person">
        <!-- Установка значений свойств через сеттеры -->
        <property name="name" value="John Doe"/>
        <property name="age" value="30"/>
    </bean>

    <!-- Другие бины и конфигурации могут быть здесь -->

</beans>
```

- На этом этапе запускается метод ==BeanPostProcessor== `postProcessBeforeInitialization(Object bean, String beanName)`, который возвращает сам бин либо его модифицированное значение.

### BeanPostProcessor

[https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)

BeanPostProcessor в Spring является интерфейсом, который предоставляет два метода для манипуляции бинами до и после их инициализации. Вот эти два метода:

1. `postProcessBeforeInitialization(Object bean, String beanName)`: Этот метод вызывается перед инициализацией бина. Вы можете использовать его, чтобы выполнить какие-либо дополнительные настройки или манипуляции над бином до его фактической инициализации. Возвращаемое значение может быть либо исходным бином, либо модифицированным бином.

```Java
public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
    // Ваш код обработки перед инициализацией бина
    return bean; // Возвращает либо исходный, либо модифицированный бин
}
```

1. `postProcessAfterInitialization(Object bean, String beanName)`: Этот метод вызывается после инициализации бина. Вы можете использовать его для дополнительных манипуляций или проверок над бином после того, как он был инициализирован. Возвращаемое значение также может быть исходным или модифицированным бином.

```Java
public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    // Ваш код обработки после инициализации бина
    return bean; // Возвращает либо исходный, либо модифицированный бин
}
```

### Default BeanPostProcessor

Существует ряд дефолтных процессоров, которые инициализируются самим фреймворком.

![[images/Untitled 7 14.png|Untitled 7 14.png]]

### interface Aware

Существует интерфейс `Aware` в Spring Framework. Этот интерфейс является маркерным интерфейсом и предназначен для обозначения бинов, которые могут быть уведомлены контейнером Spring о конкретных аспектах приложения через callback-метод. Этот интерфейс не содержит методов, и его реализация служит как сигнал для контейнера Spring о способности бина обрабатывать определенные аспекты.

Интерфейс `Aware` используется в сочетании с другими интерфейсами, такими как `ApplicationContextAware`, `EnvironmentAware`, и другими, чтобы бины могли получить доступ к соответствующим ресурсам и аспектам приложения через callback-методы.

в Spring существуют несколько интерфейсов, имена которых оканчиваются на "Aware", и они служат для уведомления бинов о различных аспектах приложения. Эти интерфейсы обычно содержат один метод для установки соответствующего объекта.

Примеры таких интерфейсов в Spring:

1. `EnvironmentAware`: Этот интерфейс позволяет бинам получить доступ к настройкам среды выполнения (например, переменным окружения) в Spring-контексте. Его метод `setEnvironment` позволяет установить объект `Environment`.
2. `EmbeddedValueResolverAware`: Дает бинам доступ к разрешению встроенных значений в текстовых строках. Его метод `setEmbeddedValueResolver` позволяет установить объект `EmbeddedValueResolver`.
3. `ResourceLoaderAware`: Этот интерфейс предоставляет доступ к загрузчику ресурсов, который позволяет бинам получать доступ к ресурсам, хранящимся внутри JAR-файлов или на файловой системе. Его метод `setResourceLoader` позволяет установить объект `ResourceLoader`.
4. `ApplicationEventPublisherAware`: Позволяет бинам получить доступ к публикатору событий приложения, чтобы они могли публиковать события в контексте приложения. Его метод `setApplicationEventPublisher` позволяет установить объект `ApplicationEventPublisher`.
5. `MessageSourceAware`: Интерфейс, который предоставляет бинам доступ к источнику сообщений, который используется для локализации сообщений и текстовых строк. Его метод `setMessageSource` позволяет установить объект `MessageSource`.
6. `ApplicationContextAware`: Как упоминалось ранее, этот интерфейс предоставляет бинам доступ к контексту приложения (ApplicationContext). Его метод `setApplicationContext` позволяет установить объект `ApplicationContext`.
7. `ApplicationStartupAware`: Позволяет бинам получить доступ к объекту, представляющему информацию о запуске приложения. Его метод `setApplicationStartup` позволяет установить объект `ApplicationStartup`.

### ApplicationContextAwareProcessor implements BeanPostProcessor

Имплементирует ApplicationContextAware, а тот в свою очередь наследуется от Aware.

Класс `ApplicationContextAwareProcessor` из пакета `org.springframework.context.support` представляет собой реализацию интерфейса `BeanPostProcessor`, который предоставляет бинам доступ к ApplicationContext, Environment или StringValueResolver для ApplicationContext для бинов, которые реализуют интерфейсы EnvironmentAware, EmbeddedValueResolverAware, ResourceLoaderAware, ApplicationEventPublisherAware, MessageSourceAware и/или ApplicationContextAware.

Реализованные интерфейсы удовлетворяются в порядке, указанном выше. То есть, если бин реализует несколько из этих интерфейсов, то он будет удовлетворен в том порядке, в котором они перечислены.

Этот процессор BeanPostProcessor автоматически регистрируется в контексте приложения с его базовой фабрикой бинов. Приложения обычно не используют его напрямую, так как он автоматически применяется при инициализации бинов, которые реализуют соответствующие интерфейсы, для предоставления им необходимых ресурсов и контекста.

```Java
class ApplicationContextAwareProcessor implements BeanPostProcessor {
	private final ConfigurableApplicationContext applicationContext;
	private final StringValueResolver embeddedValueResolver;

	public ApplicationContextAwareProcessor(ConfigurableApplicationContext applicationContext) {
		this.applicationContext = applicationContext;
		this.embeddedValueResolver = new EmbeddedValueResolver(applicationContext.getBeanFactory());
	}

	@Override
	@Nullable
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if (!(bean instanceof EnvironmentAware || bean instanceof EmbeddedValueResolverAware ||
				bean instanceof ResourceLoaderAware || bean instanceof ApplicationEventPublisherAware ||
				bean instanceof MessageSourceAware || bean instanceof ApplicationContextAware ||
				bean instanceof ApplicationStartupAware)) {
			return bean;
		}

		invokeAwareInterfaces(bean);
		return bean;
	}

	private void invokeAwareInterfaces(Object bean) {
		if (bean instanceof Aware) {
			if (bean instanceof EnvironmentAware environmentAware) {
				environmentAware.setEnvironment(this.applicationContext.getEnvironment());
			}
			if (bean instanceof EmbeddedValueResolverAware embeddedValueResolverAware) {
				embeddedValueResolverAware.setEmbeddedValueResolver(this.embeddedValueResolver);
			}
			if (bean instanceof ResourceLoaderAware resourceLoaderAware) {
				resourceLoaderAware.setResourceLoader(this.applicationContext);
			}
			if (bean instanceof ApplicationEventPublisherAware applicationEventPublisherAware) {
				applicationEventPublisherAware.setApplicationEventPublisher(this.applicationContext);
			}
			if (bean instanceof MessageSourceAware messageSourceAware) {
				messageSourceAware.setMessageSource(this.applicationContext);
			}
			if (bean instanceof ApplicationStartupAware applicationStartupAware) {
				applicationStartupAware.setApplicationStartup(this.applicationContext.getApplicationStartup());
			}
			if (bean instanceof ApplicationContextAware applicationContextAware) {
				applicationContextAware.setApplicationContext(this.applicationContext);
			}
		}
	}

}
```

Пример использования:

Когда бины в контексте Spring создаются и инициализируются, `ApplicationContextAwareProcessor` сканирует их и проверяет, реализует ли бин НАПРИМЕР интерфейс `ApplicationContextAware`. Если да, то он вызывает метод `setApplicationContext()` этого бина и передает в качестве аргумента текущий контекст приложения (ApplicationContext).

Интерфейс `ApplicationContextAware` определяет следующий метод:

```Java
void setApplicationContext(ApplicationContext applicationContext) throws BeansException;

```

С помощью этого метода бины могут получить доступ к контексту приложения и использовать его для выполнения различных операций, таких как получение других бинов, работа с настройками приложения и многое другое.

Пример использования `ApplicationContextAware`:

```Java
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.beans.BeansException;

public class MyBean implements ApplicationContextAware {
    private ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    public void doSomethingWithApplicationContext() {
        // Теперь у нас есть доступ к контексту приложения и можем его использовать
        SomeOtherBean otherBean = applicationContext.getBean(SomeOtherBean.class);
        // Выполняем операции с otherBean или другими бинами из контекста
    }
}

```

`MyBean` в этом примере реализует интерфейс `ApplicationContextAware` и получает доступ к контексту приложения через метод `setApplicationContext`. После этого он может использовать контекст приложения для выполнения различных операций, связанных с управлением бинами и компонентами приложения.

Вы можете создать собственные реализации интерфейса `BeanPostProcessor`, чтобы добавить дополнительную логику обработки бинов в вашем приложении Spring. Это может быть полезно, например, для внедрения дополнительных поведений в бины до или после их инициализации.

### Custom BeanPostProcessor

Чтобы создать свой BeanPostProcessor необходимо имплементироваться от аналогичного интерфейса и реализовать два метода:

```Java
public interface BeanPostProcessor {
@Nullable
	default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}

@Nullable
	default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}
}
```

==ВАЖНО в PostProccess нельзя использовать видоизменять бин и превращать в его в Proxy объект, могут возникнуть проблемы с dependency injection.==

Пример есть две транзакции

```Java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface InjectBean {
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Transaction {
}
```

Транзакцией @InjectBean я помечаю поле userRepo, InjectBeanPostProcessor будет осуществлять DI и присваивать bean класса ConnectionPool в это поле.

Аннотация @Transaction помечает класс для TransactionBeanPostProcessor для открытия и закрытия сессии при вызове методов Repository.

```Java
@Transaction
public class UserRepository  implements CrudRepository<Long, User> {
    @InjectBean
    private ConnectionPool pool;
@Override
    public Optional<User> findById(Long id) {
        System.out.println("findById method");
        return Optional.of(new User(id));
    }
}
```

Этот код представляет собой реализацию `BeanPostProcessor` в Spring Framework и используется для автоматического внедрения (инжекции) зависимостей в бины на основе аннотации `@InjectBean`

```Java

@Component
public class InjectBeanPostProcessor implements BeanPostProcessor, ApplicationContextAware, Ordered {
    private ApplicationContext applicationContext;
/*
В этом методе выполняется сканирование полей (fields) объекта bean,
переданного в качестве аргумента. Для каждого поля, помеченного
аннотацией @InjectBean, выполняются следующие действия:
Из контекста Spring (applicationContext) извлекается бин (beanToInject), тип которого соответствует типу поля.
С помощью ReflectionUtils (часть Spring Framework) делается доступным
(make accessible) поле для модификации.
Значение поля bean заменяется на beanToInject, тем самым происходит
инжекция зависимости.
*/
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        Arrays.stream(bean.getClass().getDeclaredFields())
                .filter(field -> field.isAnnotationPresent(InjectBean.class))
                .forEach(field -> {
                    Object beanToInject = applicationContext.getBean(field.getType());
                    ReflectionUtils.makeAccessible(field);
                    ReflectionUtils.setField(field, bean, beanToInject);
                });

        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
    }
/*
Метод setApplicationContext принимает ApplicationContext,
 предоставляя доступ к контексту Spring.
 Этот метод вызывается при инициализации бина и устанавливает
 applicationContext для использования в методе
 postProcessBeforeInitialization.
*/
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    @Override
    public int getOrder() {
        return 0;
    }
```

  

Код ниже представляет собой реализацию `BeanPostProcessor` в Spring Framework и используется для управления транзакциями для бинов, помеченных аннотацией `@Transaction`.

```Java
@Component
public class TransactionBeanPostProcessor implements BeanPostProcessor, Ordered {
/*
В этом методе происходит сканирование бинов до их инициализации
 (BeforeInitialization) и проверяется, содержит ли класс бина аннотацию
 @Transaction. Если да, то информация о данном бине добавляется
 в transactionBeans - это Map, которая связывает имя бина с его классом.
*/
    private final Map<String, Class<?>> transactionBeans = new HashMap<>();
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        if(bean.getClass().isAnnotationPresent(Transaction.class)){
            transactionBeans.put(beanName, bean.getClass());
        }
        return bean;
    }
/*
Этот метод вызывается после инициализации бинов (AfterInitialization).
Внутри метода проверяется, есть ли информация о бине в transactionBeans.
Если бин был помечен аннотацией @Transaction, то создается динамический
прокси для этого бина с помощью Proxy.newProxyInstance.
Прокси в данном случае добавляет вокруг вызовов методов бина начало и
завершение транзакции, выводя "Open transaction" перед выполнением
метода и "Close transaction" после выполнения метода.
*/
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        Class<?> beanClass = transactionBeans.get(beanName);
        if (beanClass != null){
            return Proxy.newProxyInstance(beanClass.getClassLoader(),
                    beanClass.getInterfaces(),
                    (proxy, method, args) ->{
                        System.out.println("Open transaction");
                        try{
                            return method.invoke(bean, args);
                        } finally {
                            System.out.println("Close transaction");
                        }
                    });
        }
        return bean;
    }
/*
Этот метод из интерфейса Ordered указывает на приоритет данного
 BeanPostProcessor. Он будет выполняться после InjectBeanPostProcessor.
*/
    @Override
    public int getOrder() {
        return 1;
    }
}
```

  

- ==Следующий этап - Intitialization Callbacks.==

Дальше по порядку вызывается метод помеченный аннотацией `@PostConstruct`

```Java
@PostConstruct
    public void init() {
        // Метод, выполняемый после создания бина
        System.out.println("Bean инициализирован. Вызван метод init().");
    }
```

Дальше вызывается метод из интерфейса `public interface InitializingBean` (это не нужно использовать и ниже блок init-method, достаточно @PostConstruct)

```Java
@Override
    public void afterPropertiesSet() throws Exception {}
```

После вызывается метод, указанный в бине с помощью `init-method`

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool"
    init-method="init">
```

```Java
private void init() {}
```

- После всех этапов вызывается метод BeanPostProcessor `postProcessAfterInitialization(Object bean, String beanName)`. Где также возвращаемое значение также может быть исходным или модифицированным бином.

В итоге бин создан.

Важно этап жизненного цикла, связанный с уничтожением бина возможен только для scope = singleton, так как prototype не хранятся в scope. Для них этот этап пропускается

Для singleton сначала выполняется метод помеченный аннотацией `@PreDestroy`

```Java
@PreDestroy
    public void cleanUp() {
        // Метод, выполняемый перед разрушением бина
        System.out.println("Bean разрушается. Вызван метод cleanUp().");
        // Вы можете выполнить здесь необходимые операции по очистке ресурсов или закрытию соединений.
    }
```

После этого выполняется метод destroy() из интерфейса `public interface DisposableBean` (если имплементировать естесственно)

```Java
void destroy() throws Exception;
```

Уже после этого методы помеченные в xml конфигурации под атрибутом `destroy-method`.

```Java
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool"
    init-method="init"
    destroy-method="destroyBean">
```

```Java
private void destroyBean() {}
```

### Bean Scopes

![[images/Untitled 8 13.png|Untitled 8 13.png]]

Bean scopes в Spring определяют область видимости бинов, то есть, сколько разных экземпляров бина будет создано и управляемо контейнером для различных компонентов вашего приложения. В Spring Framework существуют следующие основные области видимости (bean scopes).

### interface ScopeMetadataResolver

Этот интерфейс является частью механизма управления бинами в Spring и позволяет определить, в какой области видимости должны создаваться и управляться экземпляры бинов.

```Java
@FunctionalInterface
public interface ScopeMetadataResolver {

	ScopeMetadata resolveScopeMetadata(BeanDefinition definition);
}
```

Три класса, которые имплементируют этот интерфейс

- `AnnotationScopeMetadataResolver` Этот класс используется для разрешения области видимости бинов на основе аннотаций, примененных к классам бинов. Как уже упоминалось ранее, он анализирует аннотации, такие как `@Scope`, которые могут быть применены к бинам, и определяет область видимости бина на основе этих аннотаций. Это позволяет гибко настраивать область видимости для каждого бина в вашем приложении, используя аннотации.
- `Jsr330ScopeMetadataResolver` Этот класс используется для поддержки стандарта JSR-330 (Java Specification Request 330), который определяет стандартные аннотации для инъекции зависимостей и управления областью видимости.

![[images/Untitled 9 13.png|Untitled 9 13.png]]

- `CustomScopeResolver` Этот класс представляет собой пользовательскую имплементацию интерфейса `ScopeMetadataResolver`. Вы можете создать собственную реализацию этого класса, чтобы определить собственную стратегию разрешения области видимости бинов, не ограничиваясь стандартными аннотациями или стандартами.

  

  

  

  

  

  

  

  

==Основные scopes:==

1. `**Singleton(по умолчанию)**`:
    - **Один экземпляр**: Для бинов с областью видимости Singleton контейнер создает и управляет только одним единственным экземпляром бина в рамках всего контейнера Spring. Этот экземпляр используется для всех запросов на этот бин в приложении.
    - **Общий доступ**: Все компоненты, которые запрашивают бин с областью видимости Singleton, получают ссылку на один и тот же объект. Изменения, внесенные в одном месте, отражаются на этом объекте во всех остальных местах.
    - **Эффективность**: Singleton может быть более эффективным с точки зрения использования памяти, так как создается только один экземпляр.
2. `**Prototype**``:`
    - **Новый экземпляр при каждом запросе**: Для бинов с областью видимости Prototype контейнер создает новый экземпляр бина каждый раз, когда он запрашивается. То есть, каждый компонент, который запрашивает такой бин, получает свой собственный экземпляр.
    - **Изоляция**: Каждый экземпляр бина с областью видимости Prototype изолирован от других экземпляров. Изменения, внесенные в одном экземпляре, не влияют на другие.
    - **Потребление больше памяти**: Из-за создания новых экземпляров при каждом запросе бины с областью видимости Prototype могут потреблять больше памяти и ресурсов.
        
        ```XML
        <bean id="pool" name="connPool"
        class="com.konovalov.database.pool.ConnectionPool"
        scope="prototype"/>
        ```
        
        На картинке видно, что экземпляры класса ConnectionPool разные
        
        ![[images/Untitled 10 11.png|Untitled 10 11.png]]
        
        Для установки scope с помощью аннотации существует аннотация `@Scope`
        
        ```Java
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;
        
        @Component
        @Scope("prototype")
        public class MyPrototypeBean {
            // Код вашего бина
        }
        ```
        
3. **Request**:
    - Этот бин будет существовать в рамках одного HTTP-запроса (для веб-приложений). Каждый новый HTTP-запрос создает новый экземпляр бина с этой областью видимости.
4. **Session**:
    - Этот бин будет существовать в рамках одной HTTP-сессии (для веб-приложений). Каждый пользователь, выполняющий вход, будет иметь свой собственный экземпляр бина.
5. **Global Session**:
    - Этот бин существует в рамках одной глобальной HTTP-сессии, что обычно используется в портлетах (portlet) веб-приложений.
6. **Application**:
    - Этот бин будет существовать в рамках всего жизненного цикла приложения. В этой области видимости бин будет единственным на уровне всего приложения.
7. **WebSocket**:
    - Эта область видимости поддерживается для бинов, созданных в WebSocket-сеансах (для приложений, использующих WebSocket).

  

### DI(Dependency Injection)

### Терминология

![[images/Untitled 11 11.png|Untitled 11 11.png]]

```Java

//Пример внедрения зависимости

public class UserRepository {
    // Ваша реализация UserRepository
}

public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

public class Main {
    public static void main(String[] args) {
        // Создаем объект UserRepository
        UserRepository userRepository = new UserRepository();

        // Внедряем userRepository в UserService
        UserService userService = new UserService(userRepository);
    }
}
```

Когда мы говорим об Inversion Control Spring мы подразумеваем `Dependency Injection`

Dependency Injection (DI), или внедрение зависимости - это паттерн проектирования в объектно-ориентированном программировании, который представляет собой технику, используемую для доставки (внедрения) зависимостей в объекты или компоненты. Этот паттерн помогает управлять зависимостями между классами и снижает связанность (coupling) в приложении.

То есть в Spring по своей сути это одна из реализаций IoC, посредством которой созданием объекта и компоновой его зависимостей занимается этот фреймворк.

Пример IoC

```Java
public class Container {
    public <T> T get(Class<T> clazz){
        return null; //возвращение класса
    }
}

public static void main(String[] args) {

        var container = new Container();
        // передаем создание UserService классу Container
        var userService = container.get(UserService.class); //IoC
    }
}
```

Dependency Injection фреймворк определяет и внедряет зависимости:

- параметры конструктора
- параметры статического метода инициализации (фабричный метод)
- свойства объекта (set methods)

### Metadata Configuratuion(Bean Definitions)

### Bean Definition Readers

BeanDefinitionReaders - это часть внутреннего механизма Spring  
Framework, которая отвечает за чтение и регистрацию определений бинов  
(Bean Definitions). Они являются ключевой частью процесса создания бинов  
и конфигурирования Spring контейнера. BeanDefinitionReaders работают с  
разными источниками метаданных, такими как XML-файлы, аннотации и  
Java-конфигурации, и преобразуют эту метаданные в объекты  
`BeanDefinition`, которые затем используются для создания и настройки бинов.

Виды конфигурации и соответствующие readers в картинках

![[images/Untitled 12 11.png|Untitled 12 11.png]]

![[images/Untitled 13 11.png|Untitled 13 11.png]]

- **XmlBeanDefinitionReader**: Этот BeanDefinitionReader  
    читает метаданные бинов из XML-файлов конфигурации Spring. Он ищет и  
    парсит специальные элементы XML, такие как  
    `<bean>`, `<component-scan>`, `<import>`, и другие, чтобы создать BeanDefinition
    
    ```XML
    <bean id="myBean" class="com.example.MyBean" />
    ```
    
- `GroovyBeanDefinitionReader` для языка Groovy. Логично.
- `ClassPathBeanDefinitionScanner`: Этот инструмент предназначен для сканирования classpath и нахождения классов, которые следует зарегистрировать в качестве бинов в Spring контейнере. Он работает с классами, которые могут быть помечены аннотациями компонентов, такими как `@Component`, `@Service`, `@Repository`, а также классами, которые соответствуют заданным фильтрам. Это позволяет автоматически обнаруживать и регистрировать бины на основе классов в classpath.
- `AnnotationConfigBeanDefinitionReader` аналогичен `ClassPathBeanDefinitionScanner`, только он содержит перегруженные методы register(), которые позволяют вручную создавать beans.
- `ConfigurationClassBeanDefinitionReader` - это часть внутреннего механизма Spring Framework, отвечающая за чтение и регистрацию бинов (Bean Definitions), определенных в классах конфигурации (`@Configuration`). Этот класс используется внутри Spring для обработки конфигурации, определенной в аннотированных классах конфигурации и Java-конфигурации, и регистрирует бины в контейнере Spring. Он сканирует указанные классы, помеченные аннотацией `@Configuration`, для поиска бинов, определенных с помощью аннотации `@Bean`. Такие классы конфигурации содержат методы, которые создают и настраивают бины.
    
    ```Java
    @Configuration
    public class AppConfig {
    
        @Bean
        public MyBean myBean() {
            return new MyBean();
        }
    }
    ```
    

### XML Configuration

### application.xml

Для конфигурации через xml файл необходимо создать xml в resources

New File→ Xml Configuration → Spring Config

![[images/Untitled 14 9.png|Untitled 14 9.png]]

```Java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

Для добавления Bean прописываем в файл конфигурации

```Java
<bean class="com.konovalov.service.UserService"/>
```

Bean будет лежать в контексте, вызов осуществляется через Reflection API

```Java
var context = new ClassPathXmlApplicationContext("application.xml");
        System.out.println(context.getBean(UserService.class));

//com.konovalov.service.UserService@11c20519
```

  

IoC Container представляет собой Map<Sring, Object>, где String - идентификатор, а Object сам Bean, а изначальный идентификатор (ключ) - это относительный путь класса#номер

```Java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 <!-- два тех же класса-->
 <bean class="com.konovalov.service.UserService"/>
 <bean class="com.konovalov.service.UserService"/>

</beans>
```

Соответственно добавив два бина в конфигурацию → map.size() = 2

![[images/Untitled 15 9.png|Untitled 15 9.png]]

Так как map содержит два бина класа UserService при попытке получения бина через класс `context.getBean(UserService.class);` возникнет ошибка:

```Java
Exception in thread "main"
org.springframework.beans.factory.NoUniqueBeanDefinitionException:
 No qualifying bean of type 'com.konovalov.service.UserService'
 available: expected single matching bean but found 2
```

Так как непонятно какой конкретно Bean класса UserService необходимо вытащить из map, их там два в примере.

Решение присвоить бинам id:

```Java
<bean id="service1" class="com.konovalov.service.UserService"/>
<bean id="service2" class="com.konovalov.service.UserService"/> <!-- два тех же класса-->
```

Чтобы не получить Object, вторым параметром передается класс

```Java
UserService userService = context.getBean("service1",UserService.class);
```

Также можно присваивать alias бинам и искать их по этим именнам, причем один бин может содержать несколько имен

```Java
<bean id="service1" name="usrServ, mainServ" class="com.konovalov.service.UserService"/>
<bean id="service2" name="secondServ" class="com.konovalov.service.UserService"/> <!-- два тех же класса-->
```

```Java
context.getBean("mainServ",UserService.class);
```

### Constructor Injection

Для создания конфигурации бина в Spring XML-конфигурации, учитывая, что ваш класс `ConnectionPool` имеет конструктор с аргументами, вы можете воспользоваться следующим синтаксисом:

```Java
import lombok.Data;

import java.util.List;
import java.util.Map;
@Data
public class ConnectionPool {
    private final String userName;
    private final Integer poolSize;
    private final List<Object> args;
    private final Map<String, Object> properties;

    public ConnectionPool(String userName, Integer poolSize, List<Object> args, Map<String, Object> properties) {
        this.userName = userName;
        this.poolSize = poolSize;
        this.args = args;
        this.properties = properties;
    }
}
```

- Первый вариант конфигурации, когда нет конкретных ссылок на конкретные параметры конструктора( айди, названия полей). Тогда необходимо соблюдать порядок параметров

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
        <constructor-arg  value="postgres"/>
        <constructor-arg  value="10"/>
        <constructor-arg>
            <list>
                <value>--arg1=value1</value>
                <value>--arg2=value2</value>
            </list>
        </constructor-arg>
        <constructor-arg>
            <map>
                <entry key="pUrl" value="postgresURL"/>
                <entry key="sUrl" value="someURL"/>
            </map>
        </constructor-arg>
    </bean>
```

- Второй вариант. Можно не соблюдать порядок, но необходимо прописать нумерацию параметров через `index`

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
        <constructor-arg index="0" value="postgres"/>
        <constructor-arg index="1" value="10"/>
        <constructor-arg index="2">
            <list>
                <value>--arg1=value1</value>
                <value>--arg2=value2</value>
            </list>
        </constructor-arg>
        <constructor-arg index="3">
            <map>
                <entry key="pUrl" value="postgresURL"/>
                <entry key="sUrl" value="someURL"/>
            </map>
        </constructor-arg>
    </bean>
```

- Третий варинт через названия полей

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
        <constructor-arg name="userName" value="postgres"/>
        <constructor-arg name="poolSize" value="10"/>
        <constructor-arg name="args">
            <list>
                <value>--arg1=value1</value>
                <value>--arg2=value2</value>
            </list>
        </constructor-arg>
        <constructor-arg name="properties">
            <map>
                <entry key="pUrl" value="postgresURL"/>
                <entry key="sUrl" value="someURL"/>
            </map>
        </constructor-arg>
    </bean>
```

Также дополнительно через `type` указать тип параметра, особенно это важно, когда у класса есть несколько конструкторов и необходимо указать тип параметров.

```XML
<bean name="example" class="java.lang.String">
        <constructor-arg type="java.lang.StringBuffer" value="lineFromBuffer"/>
    </bean>
```

Также внутри map можно хранить ссылки на другие бины через `value-ref`

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
<constructor-arg index="3">
            <map>
                <entry key="service" value-ref="usrServ"/>
            </map>
        </constructor-arg>
</bean>

<bean id="service1" name="usrServ, mainServ" class="com.konovalov.service.UserService"/>
```

В list это просто `ref`

```XML
<list>
      <ref bean="usrServ"/>
</list>
```

Если необходимо создать бин класса, который принимает в качестве параметра в конструкторе другой класс ( необходимо передать бин), то делается это через `constructor-arg`

```Java
public class UserRepository {

    private final ConnectionPool pool;

    public UserRepository(ConnectionPool pool) {
        this.pool = pool;
    }
}
```

```Java
<bean id="userRepo" class="com.konovalov.repository.UserRepository">
        <constructor-arg ref="pool"/>
</bean>

<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
```

### Factory Method Injection

Осуществить DI можно с помощью статичного метода, используя `factory-method,` в котором необходимо указать название метода.

```Java
<bean id="userRepo" class="UserRepository" factory-method="of">
        <constructor-arg ref="pool"/>
</bean>
```

Метод либо создается вручную static

```Java
public class UserRepository {

    private final ConnectionPool pool;

    private UserRepository(ConnectionPool pool) {
        this.pool = pool;
    }

    public static UserRepository of(ConnectionPool pool){
        return new UserRepository(pool);
    }
}
```

Можно также Lombok использовать

```Java
@AllArgsConstructor(staticName = "of")
public class UserRepository {

    private final ConnectionPool pool;

}
```

  

  

  

### Property Injection

Также DI можно осуществлять через сеттеры, но есть минус, поле не может быть final, то есть объект не будет immutable

```Java
public class ConnectionPool {
    private final String userName;
    private final Integer poolSize;
    private final List<Object> args;

    public void setProperties(Map<String, Object> properties) {
        this.properties = properties;
    }

    private Map<String, Object> properties;
```

Делается это через `property`

```Java
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool">
        <constructor-arg index="0" value="postgres"/>
        <constructor-arg index="1" value="10"/>
        <constructor-arg index="2">
            <list>
                <value>--arg1=value1</value>
                <value>--arg2=value2</value>
                <ref bean="usrServ"/>
            </list>
        </constructor-arg>
            <constructor-arg index="3">
                <null/> // в конструктор изначально передаем null
            </constructor-arg>
        <property name="properties">
            <map>
                <entry key="pUrl" value="postgresURL"/>
                <entry key="sUrl" value="someURL"/>
                <entry key="service" value-ref="usrServ"/>
            </map>
        </property>
    </bean>
```

### Injection from Property Files

В Spring Framework реализована возможность внедрения значений из файлов свойств (property files) с помощью аннотаций или XML-конфигурации. Это позволяет легко настраивать бины и компоненты вашего приложения, используя значения, которые хранятся во внешних файлах свойств, что делает конфигурирование более гибким и удобным. Вот как это можно сделать c помощью xml конфигурации.. Для этого вы можете включить элемент `<context:property-placeholder>` в ваш файл конфигурации.

Пример XML-конфигурации:

application.properties

```XML
db.userName=postgres
```

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="classpath:application.properties"/>

    <!-- Определение компонента -->
    <bean id="pool" class="com.example.ConnectionPool">
        <property name="userName" value="${db.userName}"/>
    </bean>
</beans>
```

### SpEL(Spring Experssion Language)

**ССЫЛКА НА ДОКУМЕНТАЦИЮ:**

**[https://docs.spring.io/spring-framework/docs/3.0.x/reference/expressions.html](https://docs.spring.io/spring-framework/docs/3.0.x/reference/expressions.html)**

Spring Expression Language (SpEL) предоставляет мощные средства для обработки и вычисления значений в Spring-контексте. Вы можете использовать SpEL в Spring-конфигурации, включая внедрение значений из файлов свойств (property files). Вот как это можно сделать:

1. **Внедрение значений с использованием SpEL в XML-конфигурации**:
    
    Для использования SpEL в XML-конфигурации, вы можете внедрить выражение SpEL как значение свойства. Вот пример:
    
    ```XML
    <beans xmlns="<http://www.springframework.org/schema/beans>"
           xmlns:context="<http://www.springframework.org/schema/context>"
           xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
           xsi:schemaLocation="<http://www.springframework.org/schema/beans>
           <http://www.springframework.org/schema/beans/spring-beans.xsd>
           <http://www.springframework.org/schema/context>
           <http://www.springframework.org/schema/context/spring-context.xsd>">
    
        <context:property-placeholder location="classpath:application.properties"/>
    
        <bean id="myComponent" class="com.example.MyComponent">
            <property name="myProperty" value="#{systemProperties['user.home']}"/>
        </bean>
    </beans>
    ```
    
    В этом примере значение `myProperty` берется из системных свойств (`systemProperties`) с использованием SpEL.
    
2. **Внедрение значений с использованием SpEL с аннотацией** `**@Value**`:
    
    Вы также можете использовать SpEL с аннотацией `@Value` в Java-коде. Вот пример:
    
    ```Java
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.stereotype.Component;
    
    @Component
    public class MyComponent {
        @Value("#{systemProperties['user.home']}")
        private String myProperty;
    
        // ...
    }
    ```
    
    Здесь значение `myProperty` внедряется с использованием SpEL для получения значения системного свойства `user.home`.
    

SpEL предоставляет множество возможностей, включая доступ к свойствам объектов, операции, условные выражения и многое другое. Вы можете создавать сложные выражения, чтобы вычислять значения на основе различных факторов в вашем приложении.

Обратите внимание, что для использования SpEL в XML-конфигурации, вы должны убедиться, что в вашем проекте подключены соответствующие схемы (`xmlns:context="<http://www.springframework.org/schema/context>"`) и пространства имен.

### Annotation-based Configuration

### Внедрение зависимости, основные аннотации и процессоры, которые их обрабатывают

В первую очередь, чтобы иметь возможность конфигурации Spring с помощью аннотаций необходимо внедрить зависимость Jakarta Annotations API.

[https://mvnrepository.com/artifact/jakarta.annotation/jakarta.annotation-api/2.1.1](https://mvnrepository.com/artifact/jakarta.annotation/jakarta.annotation-api/2.1.1)

После внедрения зависимости становится доступна библиотека с различным аннотациями.

Допустим из жизненного цикла бина `@PostConstruct` и `@PreDestroy`:

```Java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import org.springframework.stereotype.Component;

@Component
public class MyBean {

    @PostConstruct
    public void init() {
        System.out.println("Бин инициализирован. Вызван метод init().");
        // Ваши настройки и логика инициализации
    }

    // Другие методы и поля бина

    @PreDestroy
    public void cleanUp() {
        System.out.println("Бин разрушается. Вызван метод cleanUp().");
        // Ваша логика разрушения и очистки ресурсов
    }
}
```

Необходимо в `application.xml` добавить бин, который занимается обработкой аннотаций spring

```XML
<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor"/>
```

Но более удобный способ - это убрать этот бин и добавить

```XML
 <context:annotation-config/>
```

`<context:annotation-config/>` добавляет не только этот бин автоматически, но и некоторые другие бины из этой картинки( кроме **PersistenceAnnotationBeanPostProcessor** - это добавится только после добавления зависимости Spring Data JPA)

Cписок классов, которые добавляются в IoC Container отвечающих за обработку аннотаций в Spring:

![[images/Untitled 16 8.png|Untitled 16 8.png]]

1. **CommonAnnotationBeanPostProcessor**: Обрабатывает аннотации `@Autowired`, `@Resource`, `@PostConstruct`, `@PreDestroy` и другие стандартные аннотации JSR-250 и Common Annotations for the Java Platform.
2. **AutowiredAnnotationBeanPostProcessor**: Специализируется на обработке аннотации `@Autowired` и связанных аннотаций, таких как `@Qualifier`. Обеспечивает внедрение зависимостей.
3. **ConfigurationClassPostProcessor**: Этот класс обрабатывает аннотации, связанные с конфигурацией, такие как `@Configuration`, `@Bean`, `@ComponentScan` и другие. Он играет важную роль в поддержке JavaConfig (Java-based configuration) в Spring.
4. **PersistenceAnnotationBeanPostProcessor**: Используется для обработки аннотаций из пакета `javax.persistence`, таких как `@PersistenceContext` и `@PersistenceUnit`. Обеспечивает внедрение EntityManager и EntityManagerFactory.
5. **EventListenerMethodProcessor**: Обрабатывает аннотации, связанные с обработкой событий в Spring, такие как `@EventListener`. Позволяет бинам реагировать на события, опубликованные через Spring Application Event.

  

### @Autowired, @Value, @Resource

Как было рассмотрено в жизненном цикле ранее такие аннотации обрабатываются BeanPostProcessors.

(По хорошему не принято их использовать над полями и сеттерами, а принято надо кнонструкторами. Но чтобы использовать надо конструкторами необходимо не использовать xml конфигурацию)

- `**@Autowired**` является аннотацией Spring и используется для внедрения зависимостей между компонентами (бинами) Spring. Он работает на основе типа, и Spring пытается найти бин, тип которого совпадает с типом поля, помеченного `@Autowired`.`@Autowired` не требует явного указания имени бина или имени свойства.

```Java
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD,
ElementType.PARAMETER, ElementType.FIELD,
ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Autowired {

	/**
Этот параметр указывает, является ли внедрение зависимости обязательным.
Если required = true (по умолчанию), то Spring выбросит исключение NoSuchBeanDefinitionException, если не удастся найти подходящий бин для внедрения. 
Если required = false, то внедрение будет выполнено, если бин не найден, и поле останется null.
	 */
	boolean required() default true;
```

У `@Autowired` нет возможности передать через name название бина, который необходимо внедрить. (Если несколько бинов одного типа и спринг не поймет, какой внедрить).

Но есть дополнительная аннотация `@Qualifier`

```XML
<bean id="pool" name="connPool" class="com.konovalov.database.pool.ConnectionPool"/>
```

```Java
public class UserRepository  implements CrudRepository<Long, User> {

    @Autowired
    @Qualifier(value = "connPool")
    private ConnectionPool pool;
}
```

Либо можно назвать переменную в соответствии с названием бина, Spring сам найдет.

```Java
public class UserRepository  implements CrudRepository<Long, User> {

    @Autowired
    private ConnectionPool connPool;
}
```

- `**@Resource**` не является аннотацией Spring, это из jakarta, но она поддерживается Spring и по сути выполняет тот же функционал, Только над конструктором ее не поставишь. Требует явного указания имени бина или имени свойства в аннотации.

```Java
@Target({TYPE, FIELD, METHOD})
@Retention(RUNTIME)
@Repeatable(Resources.class)
public @interface Resource {

    String name() default "";

... там много всего
}
```

Пример использования `@Resource`:

```Java
public class UserRepository  implements CrudRepository<Long, User> {

    @Resource(name="connPoll")
    private ConnectionPool connPool;
}
```

- `@Value` используется для внедрения значений примитивных типов (например, строки, числа) из внешних конфигурационных файлов или свойств. Он позволяет внедрять значения из файла свойств, системных свойств, переменных окружения и других источников. Чаще всего используется для настройки свойств бинов. `@Value` работает на уровне поля, метода сеттера или параметра конструктора и требует указания значения, которое будет внедрено. Можно использовать EL или SpEL
    
    Пример использования `@Value`:
    
    ```Java
    @Value("${app.someProperty}")
    private String someProperty;
    ```
    

### ClassPath Scaning

Так как не принято использовать аннотацию `@Autowired` надо полями и сеттерами, принято использовать над конструкторами. Чтобы использовать только конструкторы небходимо отойти от создания бинов черех xml конфигурацию.

Для этого необходимо в xml конфиг добавить сканнер аннотации `@Component` (специальный BeanPostProcessor)

### @Component, @Repository, @Controller, @Service

`@Component`- эта аннотация означает, что Spring будет создавать экземпляры этого класса (бин) и управлять их жизненным циклом.

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {

	String value() default "";

}
```

В Spring есть несколько аннотаций, которые также являются компонентами и наследуют функциональность от базовой аннотации `@Component`. Вот некоторые из них:

1. **@Service**: Эта аннотация используется для пометки классов, которые представляют сервисы в приложении. Сервисы обычно содержат бизнес-логику и предоставляют ее другим частям приложения.
2. **@Repository**: Классы, отмеченные этой аннотацией, обычно используются для взаимодействия с базой данных. Они представляют слой доступа к данным (Data Access Layer).
3. **@Controller**: Эта аннотация применяется к классам, которые являются контроллерами в архитектуре MVC (Model-View-Controller). Контроллеры обрабатывают HTTP-запросы и возвращают соответствующие HTTP-ответы.
4. **@Configuration**: Классы, отмеченные этой аннотацией, используются для определения настроек и бинов (бин - это объект, управляемый Spring) в виде Java-кода. Это позволяет создавать конфигурационные классы вместо XML-конфигурации.
5. **@ControllerAdvice**: Эта аннотация используется для создания глобальных обработчиков исключений для контроллеров. Она позволяет централизованно обрабатывать исключения в приложении.
6. **@RestController**: Подобно `@Controller`, но добавляет функциональность RESTful-контроллеру, позволяя автоматически сериализовать и десериализовать данные в формате JSON.

  

```XML
<context:component-scan
            base-package="com.konovalov"
            annotation-config="true"
            name-generator="com.konovalov.generator.CustomNameGenerator"
            resource-pattern="**/*.class"
            scoped-proxy="targetClass"
            scope-resolver="com.konovalov.resolvers.CustomScopeResolver"
						use-default-filters="false"> // по умолчанию true
			        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
				       <context:exclude-filter type="regex" expression="com\..+Repository"/>
</context:component-scan>
```

1. `base-package`: Этот параметр определяет базовый пакет, в котором Spring будет сканировать компоненты. В данном случае, он установлен на `"com.konovalov"`, что означает, что Spring будет искать аннотированные компоненты (классы с аннотациями, такими как `@Component`, `@Service`, `@Controller`, и т. д.) внутри этого пакета и его подпакетов.
2. `annotation-config`: Этот параметр установлен в `true`. Он указывает, что Spring должен включить обработку аннотаций, что позволяет вам использовать аннотации для настройки бинов и компонентов.
    
    Также можно удалить `<context:annotation-config/>`, так как component-scan внутри себя уже содержит этот config и добавляет все стандартные BeanPostProcessors.
    
3. `name-generator`: Параметр, который может определять специфический генератор имен для создаваемых компонентов. В данном случае, он пустой, что означает использование имени компонента по умолчанию.
    
    ```Java
    @Component
    public class CustomNameGenerator implements BeanNameGenerator {
        @Override
        public String generateBeanName(BeanDefinition definition, BeanDefinitionRegistry registry) {
            // Здесь вы можете определить собственную логику генерации имени бина
            return "customBeanName"; // Здесь установите ваше собственное имя бина
        }
    }
    ```
    
4. `resource-pattern`: Здесь установлен шаблон для поиска ресурсов (классов) внутри указанного пакета и его подпакетов. `*/*.class` означает, что Spring будет сканировать все `.class` файлы внутри пакетов и подпакетов `com.konovalov`.
5. `scoped-proxy`: Этот параметр указывает, что для создаваемых компонентов должен быть создан прокси с указанным скоупом. В данном случае, установлено значение `"targetClass"`, что означает создание прокси с таким же скоупом, как у целевого бина.
6. `scope-resolver`: Этот параметр позволяет указать специфический резольвер скоупа, который определит, какой скоуп должен быть у компонентов. В данном случае, он также пустой, что означает использование скоупа по умолчанию.
    
    ```Java
    @Component
    public class CustomScopeResolver implements ScopeMetadataResolver {
        @Override
        public ScopeMetadata resolveScopeMetadata(BeanDefinition beanDefinition) {
            // Здесь вы можете определить логику для выбора скоупа для компонентов
            ScopeMetadata scopeMetadata = new ScopeMetadata();
            scopeMetadata.setScopeName("customScope"); // Здесь установите имя вашего собственного скоупа
            return scopeMetadata;
        }
    }
    ```
    
7. `use-default-filters`: Установлен в `true`, что указывает Spring использовать стандартные фильтры для определения компонентов. Стандартные фильтры включают в себя аннотации `@Component`, `@Repository`, `@Service` и `@Controller`.
    
    ![[images/Untitled 17 8.png|Untitled 17 8.png]]
    
    - `annotation` Этот тип фильтра позволяет включать классы на основе аннотаций, которыми они помечены.
        
        ```Java
        ...
        use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
        <--допустим это тоже самое если написать true-->
        ...
        ```
        
    - `assignable` Этот тип фильтра позволяет включать классы, которые являются подклассами или реализуют указанный класс или интерфейс.
    - `aspectj` Этот тип фильтра позволяет использовать выражения AspectJ для выбора классов на основе их структуры и поведения.
    - `regex` Этот тип фильтра позволяет использовать регулярные выражения для выбора классов на основе их имени.
        
        ```Java
        <context:include-filter type="regex" expression="com\..+Repository"/>
        Это будет создавать бины, даже если над ними не стоит аннотация @Component,
        ъно название класса заканчивается на Repository
        ```
        
    - `custom` Этот тип фильтра позволяет определить собственный класс фильтра, который реализует интерфейс `org.springframework.core.type.filter.TypeFilter` и определяет, какие классы должны быть включены.

  

Теперь вместо того, чтобы прописывать в xml конфигурации bean достаточно навесить над классом аннотацию

```XML
<bean id="userRepo" class="com.konovalov.repository.UserRepository"/>
```

```Java
@Component(value = "pool1")
public class ConnectionPool {
    private final String userName;
    private final Integer poolSize;
    public ConnectionPool(@Value(value = "${db.userName}")String userName,
                          @Value(value = "${db.pool.size}") Integer poolSize) {
        this.userName = userName;
        this.poolSize = poolSize;
    }
}
```

ВАЖНО: эти аннотации создают бины в единственном экземпляре, то есть если нужно несколько бинов одного и того же типа, чтобы отойти от xml конфигурации, необходимо создавать Java конфигурацию, более подробно в следующей теме.

А также @Component нельзя навесить над базовыми Java классами, например String. Это решить можно также через Java конфигурацию, что также будет позже рассмотрено:

```Java
<bean class="java.lang.String">
    <constructor-arg value="${db.userName}"/>
</bean>
```

  

### Java-based Configuration

### Переход от xml, @Configuration, @ComponentScan, @PropertySource

Для перехода к Java-based конфигурации с помощью аннотаций от следующего XML необходимо выполнить следующие шаги:

- `<context:annotation-config/>` заменяется на `@Configuration`

```Java
@Configuration
public class ApplicationConfiguration {
}
```

- Импорт настроек из properties файла `<context:property-placeholder location="classpath:application.properties" />` заменяется на `@PropertySource`

```Java
@Configuration
@PropertySource("classpath:application.properties")
public class ApplicationConfiguration {
}
```

- Следующий тег, связанный со сканирование компонентов на наличие аннотаций и т.д., который включает стандартны постпроцессоры `annotation-config="true"`, (мы уже заменили это с помощью `@Configuration`)

```Java
<context:component-scan
            base-package="com.konovalov"
            annotation-config="true"
            name-generator="com.konovalov.generator.CustomNameGenerator"
            resource-pattern="**/*.class"
            scoped-proxy="targetClass"
            scope-resolver="com.konovalov.resolvers.CustomScopeResolver"
						use-default-filters="false"> // по умолчанию true
			        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
				       <context:exclude-filter type="regex" expression="com\..+Repository"/>
</context:component-scan>
```

Меняется на `@ComponentScan`

```Java
@ComponentScan(
        basePackages = "com.konovalov",
        nameGenerator = com.konovalov.generator.CustomNameGenerator.class,
        scopedProxy = ScopedProxyMode.INTERFACES,
        scopeResolver = com.konovalov.resolvers.CustomScopeResolver.class,
        useDefaultFilters = false,
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ANNOTATION, value = org.springframework.stereotype.Component.class),
                @ComponentScan.Filter(type = FilterType.REGEX, pattern = "com\\..+Repository"),
                @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = CrudRepository.class)
        }
```

- Итоговый вид конфигурационного файла и получение контекста:

```Java
@Configuration
@PropertySource("classpath:application.properties")
@ComponentScan(
        basePackages = "com.konovalov",
        nameGenerator = com.konovalov.generator.CustomNameGenerator.class,
        scopedProxy = ScopedProxyMode.INTERFACES,
        scopeResolver = com.konovalov.resolvers.CustomScopeResolver.class,
        useDefaultFilters = false,
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ANNOTATION, value = org.springframework.stereotype.Component.class),
                @ComponentScan.Filter(type = FilterType.REGEX, pattern = "com\\..+Repository"),
                @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = CrudRepository.class)
        }
)
public class ApplicationConfiguration {
}
```

```Java
public class Main {
    public static void main(String[] args) {

        @Cleanup var context = new AnnotationConfigApplicationContext(ApplicationConfiguration.class);
    }
}
```

  

  

### @Import, @ImportResource

Можно комбинировать Java конфигурацию и xml конфигурацию с помощью аннотации `@ImportResource`

```Java
@ImportResource(locations = "classpath:ss.xml")
@Configuration
public class ApplicationConfiguration {
}
```

Аннотация `@Import` позволяет подгружать вложенно другую конфигурацию (помеченную `@Configuration`) даже в другом пакете или модуле через import

```Java
package com.konovalov.config;

import com.andreev.web.config.WebConfiguration;

@Configuration
@Import(WebConfiguration.class)
public class ApplicationConfiguration {
}
```

### @Bean

`@Bean` - это аннотация в Spring Framework, которая используется для определения методов, возвращающих бины (компоненты) в контейнере Spring.

Эту аннотацию можно ставить в любых классах, но принято только в классах, которые помечены `@Configuration`

```Java
@Configuration
public class AppConfig {

    @Bean
    public ConnectionPool connPool() {
        return new ConnectionPool("userName", "password", 10);
    }
}
```

Чтобы Spring автоматически внедрил зависимость этого бина достаточно указать также название

```Java
@Repository
public class UserRepository  implements CrudRepository<Long, User> {
    private final ConnectionPool pool;

    public UserRepository(ConnectionPool connPool) {
        this.pool = connPool;
    }
```

Либо используем аннотацию `@Qualifier`

```Java
@Repository
public class UserRepository  implements CrudRepository<Long, User> {
    private final ConnectionPool pool;

    public UserRepository(@Qualifier("connPool") ConnectionPool pool) {
        this.pool = pool;
    }
```

Можно (один из способов DI) вынести создание этого бина UserRepository в конфигурационный файл и также пометить его @Bean

```Java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {

    @Bean
		@Scope(BeanDefinition.SCOPE_SINGLETON)
    public ConnectionPool connPool(@Value("${db.username}" String userName,@Value("${db.password}" String password) {
        return new ConnectionPool(userName, password);
    }

		@Bean
		public UserRepository userRepository( ConnectionPool connPool) {
        return new UserRepository(connPool);
    }
}
```

Еще один способ, который нечасто используется на практике, но имеет место на существование - это вызов метода. Такой способ работает только при default proxyBeanMethods = true. Так как return new UserRepository(==connPoolProxy()==); в этой строчке он вернет Proxy объект. Если выставить false, такой способ будет недоступен. Idea подчеркнет ошибку.

```Java
@Configuration(proxyBeanMethods = true)
public class ApplicationConfiguration {
@Bean
    @Scope(BeanDefinition.SCOPE_SINGLETON)
    public ConnectionPool connPoolProxy() {
        return new ConnectionPool("username", "password");
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepository(connPoolProxy());
    }
}
```

### Profiles

Профили (Profiles) в Spring позволяют вам определять и управлять различными наборами определений бинов и настроек для разных сред выполнения или сценариев. Это особенно полезно, когда у вас есть разные конфигурации для разработки, тестирования, продакшена и так далее. Чтобы использовать и включить профили в Spring есть два основных варианта.

Cама аннотация выглядит следующим образом

```Java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ProfileCondition.class)
public @interface Profile {

	/**
	 * The set of profiles for which the annotated component should be registered.
	 */
	String[] value();

}
```

- Первый способ включения profiles через properties файл

```Java
spring.profiles.active = web,prod,test
```

Так как аннотацию можно ставить над методом, классом или самой конфигурацией выглядит это так:

```Java
@Profile("test")
@Configuration
public class TestApplicationConfiguration {
}

@Configuration
public class ApplicationConfiguration {

		@Bean
    @Scope(BeanDefinition.SCOPE_SINGLETON)
    public ConnectionPool connPoolProxy() {
        return new ConnectionPool("username", "password");
    }
		@Bean
    @Profile("!test & prod")                     // можно использовать ! & и |
    public UserRepository userRepository() {
        return new UserRepository(connPoolProxy());
    }

		@Bean
    @Scope(BeanDefinition.SCOPE_SINGLETON)
		@Profile("web | prod")
    public ConnectionPool connPool(@Value("${db.username}") String userName, @Value("${db.password}") String password) {
        return new ConnectionPool(userName, password);
    }
}
```

- Второй вариант через код

```Java
@Cleanup var context = new AnnotationConfigApplicationContext();
        context.register(ApplicationConfiguration.class);
        context.getEnvironment().setActiveProfiles("web","prod");
				context.refresh();
```

ВАЖНО не передавать new AnnotationConfigApplicationContext() в качестве параметра наш config класс, так как конструктор для него вызывает метод refresh(), который запускает жизенный цикл бинов.

```Java
public AnnotationConfigApplicationContext(Class<?>... componentClasses) {
		this();
		register(componentClasses);
		refresh();
	}
```

А нам необходимо запустить этот жизненный цикл только после того, как мы укажем с какими профайлами это делать( потом вручную вызывать `refresh()`)

  

### Event Listeners

![[images/Untitled 18 7.png|Untitled 18 7.png]]

Для создания слушателя (listener) в Spring вы можете использовать аннотацию `@EventListener`, чтобы определить метод, который будет вызываться при возникновении  
определенного события в вашем приложении.  

- Вначале создайте пользовательский класс события, который расширяет `ApplicationEvent` или другой подходящий базовый класс события. Этот класс представляет собой событие, которое вы хотите слушать.

```Java
public class EntityEvent extends ApplicationEvent {

    private final AccesType accessType;

    public EntityEvent(Object event, AccesType accessType) {
        super(event);
        this.accessType = accessType;
    }
}

public enum AccesType {
    CREATE, READ, UPDATE, DELETE
}
```

После этого создаем listener и помечаем методы аннотацией `@EventListener`

```Java
@Component
public class EntityListener {

    @EventListener
    @Order(10) // для упорядочивания работы listeners
    public void acceptEntity(EntityEvent event){
        System.out.println("Entity: " + event);
    }
}
```

### @EventListener

```Java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Reflective
public @interface EventListener {

@AliasFor("classes")
	Class<?>[] value() default {};

@AliasFor("value")
	Class<?>[] classes() default {};

String condition() default "";

String id() default "";
```

`String condition() default "";` в аннотации `@EventListener` служит для задания условия, при котором метод, помеченный данной аннотацией, будет вызываться или игнорироваться в зависимости от результата вычисления этого условия. Он предоставляет возможность задать условие в виде строки с использованием Spring Expression Language (SpEL).

- Событие будет обработано, если выражение SpEL вычисляется в значение типа `boolean` равное `true` или одну из следующих строк: "true", "on", "yes" или "1".
- По умолчанию выражение SpEL равно "", что означает, что событие всегда будет обрабатываться.
- Выражение SpEL будет оцениваться в контексте, который предоставляет следующие метаданные:
    - `\#root.event` или `event` для ссылок на объект `ApplicationEvent`.
    - `\#root.args` или `args` для ссылок на массив аргументов метода.
    - Аргументы метода могут быть доступны по индексу. Например, первый аргумент можно получить через `\#root.args[0]`, `args[0]`, `\#a0` или `\#p0`.
    - Аргументы метода могут быть доступны по имени (с предшествующим символом решетки), если имена параметров доступны в скомпилированном байт-коде.

```Java
@Component
public class EntityListener {

    @EventListener(condition = "\#root.args[0].accessType.name() == 'READ'")  // или "\#p0.accessType.name() == 'READ'"
    @Order(10) // для упорядочивания работы listeners
    public void acceptEntity(EntityEvent event){
        System.out.println("Entity: " + event);
    }
}
```

  

  

![[images/Untitled 19 7.png|Untitled 19 7.png]]

После этого можно в нашем коде использовать метод `publishEvent(ApplicationEvent event)` интерфейса `ApplicationEventPublisher,` который регистрирует ивент, то есть дает указание нашему Listener выполнить метод, в котором в качестве аргумента передан наш EntityEvent и метод помечент `@EventListener`.Cам этот интерфейс наследуется от Aware, а соответственно `ApplicationContextAwareProcessor` заинжектит нам publischer.

![[images/Untitled 20 7.png|Untitled 20 7.png]]

```Java
@Service
public class UserService {

    private final CrudRepository<Integer, User> userCrudRepository;
    private final ApplicationEventPublisher eventPublisher;


    public UserService(CrudRepository<Integer, User> userCrudRepository,
                       ApplicationEventPublisher eventPublisher) {
        this.userCrudRepository = userCrudRepository;
        this.eventPublisher = eventPublisher;
    }

    public Optional<UserDto> findById(Integer id){
        return userCrudRepository.findById(id)
                .map(user ->{
                    eventPublisher.publishEvent(new EntityEvent(user, AccesType.READ));
                return new UserDto(user.id());
                });
    }
}
```

## Tom: Spring Boot

Как сбилдить Spring boot приложение:

1. Установите версию Spring Boot.
2. Добавьте/создайте стартер Spring Boot.
3. Добавьте настройки (свойства).
4. Добавьте/замените бины Spring Boot Starter.

### @Conditional

Spring Boot позволяет добавлять необходимые модули, чтобы отключать и включать их динамические необходимо использовать `@Conditional`

```Java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Conditional {

	/**
	 * All {@link Condition} classes that must {@linkplain Condition\#matches match}
	 * in order for the component to be registered.
	 */
	Class<? extends Condition>[] value();

}

@FunctionalInterface
public interface Condition {

	/**
	 * Determine if the condition matches.
	 * @param context the condition context
	 * @param metadata the metadata of the {@link org.springframework.core.type.AnnotationMetadata class}
	 * or {@link org.springframework.core.type.MethodMetadata method} being checked
	 * @return {@code true} if the condition matches and the component can be registered,
	 * or {@code false} to veto the annotated component's registration
	 */
	boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);

}
```

Conditions:

- ClassPath
- Properties
- Bean
- SpEl

```Java

@Conditional(JPACondition.class)
@Configuration
public class JPAConfiguration {
}


public class JPACondition implements Condition {

    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        try {
            context.getClassLoader().loadClass("org.postgresql.Driver");
            return true;
        } catch (ClassNotFoundException e) {
            return false;
        }
    }
}
```

Код связан с использованием аннотации `@Conditional` в Spring Framework.

1. Вы определяете класс `JPAConfiguration`, который представляет собой конфигурацию для работы с Java Persistence API (JPA). Этот класс помечен аннотацией `@Configuration`, что указывает Spring, что он содержит настройки для контейнера приложения.
2. `@Conditional(JPACondition.class)` над классом `JPAConfiguration` сообщает Spring, что данный будет активироваться только в том случае, если условие, определенное в классе `JPACondition`, вернет `true`. В данном случае, - это собственная реализация интерфейса `Condition`, и метод `matches` этого интерфейса определяет условие активации конфигурации.
3. В методе `matches` класса `JPACondition` проверяется наличие класса `org.postgresql.Driver` в класспасе (Classpath). Если этот класс найден (то есть, если есть доступ к PostgreSQL JDBC драйверу), то условие считается истинным (`true`), и конфигурация `JPAConfiguration` будет активирована. Если класс не найден, то условие считается ложным (`false`), и конфигурация не будет активирована.

Это полезный механизм в Spring, который позволяет активировать или деактивировать компоненты конфигурации в зависимости от наличия определенных условий.

### @SpringBootApplication

Вы предоставили код аннотации `@SpringBootApplication`, которая является ключевой аннотацией в Spring Boot приложениях. Эта аннотация объединяет несколько других аннотаций и позволяет определить настройки и конфигурации для вашего Spring Boot приложения. Давайте рассмотрим назначение и значимость некоторых из её элементов:

1. `@SpringBootConfiguration`:
    - Эта аннотация является синонимом `@Configuration` и сообщает Spring о том, что класс `@SpringBootApplication` является конфигурационным классом для вашего приложения.
2. `@EnableAutoConfiguration`:
    - Эта аннотация активирует автоматическую настройку в Spring Boot. Она позволяет приложению автоматически сканировать и настраивать бины (компоненты) на основе зависимостей, находящихся в classpath. Spring Boot автоматически настраивает множество стандартных бинов, чтобы облегчить разработку. позволяет запустить процесс org.springframwork.boot:spring-boot-autoconfigure
3. `@ComponentScan`:
    - Эта аннотация указывает Spring на необходимость сканировать и находить компоненты (бины) в указанных пакетах. Она также может определять фильтры для исключения определенных классов из сканирования.
4. `exclude` и `excludeName`:
    - Эти элементы позволяют вам исключать определенные классы из автоматической настройки. Вы можете указать классы, которые не должны быть настроены автоматически при помощи аннотации `@EnableAutoConfiguration`.
5. `scanBasePackages` и `scanBasePackageClasses`:
    - Эти элементы определяют базовые пакеты для сканирования компонентов. Вы можете указать, в каких пакетах Spring должен искать компоненты. По умолчанию, Spring Boot будет сканировать пакет, где находится класс с аннотацией `@SpringBootApplication`.
6. `nameGenerator`:
    - Этот элемент позволяет указать класс, который будет использоваться для генерации имен бинов в контейнере Spring. По умолчанию используется `AnnotationBeanNameGenerator`.
7. `proxyBeanMethods`:
    - Этот элемент позволяет настраивать, должны ли методы, помеченные как `@Bean`, быть проксированными Spring, чтобы обеспечить правильное управление жизненным циклом бинов. По умолчанию установлено значение `true`, что означает, что методы `@Bean` будут проксированы.

Эта аннотация позволяет объединить конфигурации, автоматическую настройку и сканирование компонентов в одной аннотации, что делает код более чистым и удобным для чтения.

Важно, чтобы класс, помеченный этой аннотацией лежал выше в иерархии над всеми классами, так как эта аннотация помечена @ComponentScan , которая сканирует классы на содержание аннотация springa.

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication
```

Также бойлерплейтным кодом становится аннотация @PropertySource. так как Spring boot приложение автоматически сканирует application.properties

```Java
@Configuration
@PropertySource("classpath:application.properties")
public class ApplicationConfiguration {
```

### Настройка проекта

[https://plugins.gradle.org/plugin/io.spring.dependency-management](https://plugins.gradle.org/plugin/io.spring.dependency-management)

Внедряем плагины spring boot, а также менеджер зависимостей

```Java
plugins {
    id 'java'
    id "io.freefair.lombok" version "8.3"
    id 'org.springframework.boot' version '3.1.4'
    id 'io.spring.dependency-management' version "1.1.3"
}
```

Добавляем starter, ссылка на все стартеры [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters)

Spring Boot starters - это обычные зависимости (jars), которые добавляют нужный функционал в проект и следуют определенному шаблону именования: spring-boot-starter-*.

Добавляем в зависимости

```Java
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
}
```

Версию указывать не нужно, она будет транзитивно подтянулта из плагина. Также можно убрать много лишнего

```Java
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
    implementation group: 'jakarta.annotation', name: 'jakarta.annotation-api', version: '2.1.1'
    testImplementation platform('org.junit:junit-bom:5.9.1')
    testImplementation 'org.junit.jupiter:junit-jupiter'     // есть spring starter test
    implementation 'org.springframework:spring-core:6.0.11'  //подтянетсЯ из плагинв
    implementation 'org.springframework:spring-context:6.0.11'
    implementation 'org.postgresql:postgresql:42.6.0' // версия не нужна подтянется транзитивно
}
```

Помечаем основной класс аннотацией `@SpringBootApplication` и можно запускать через статичный метод `run()` класса SpringApplication, который также возвращает context

```Java
@SpringBootApplication
public class ApplicationRunner {
    public static void main(String[] args) {

		ConfigurableApplicationContext context = SpringApplication.run(ApplicationRunner.class, args);
    }
}
```

Проще создавать проекты двумя способами

![[images/Untitled 21 7.png|Untitled 21 7.png]]

Дальше вручную можно выбирать зависимости

![[images/Untitled 22 7.png|Untitled 22 7.png]]

Либо можно нажать и настроить на сайте

![[images/Untitled 23 7.png|Untitled 23 7.png]]

![[images/Untitled 24 6.png|Untitled 24 6.png]]

### Properties

В spring есть два зарезервированных properties файла:

- application.properties
- spring.properties

Можно заменить [application.properties](http://application.properties) на application.yml:

```Java
db.username=${username.value:postgres}
db.pool.size = 10
db.password = password
db.driver = PostgresDriver
db.url = postgres:5432
db.hosts = localhost:127.0.0.1
spring.profiles.active= true
```

```YAML
db:
  username: ${username.value:postgres}
  pool:
    size: 10
  password: password
  driver: PostgresDriver
  url: postgres:5432
  hosts: localhost:127.0.0.1
spring:
  profiles:
    active: true
```

Также можно смапить properties на отдельный класс с помощью `@ConfigurationProperties(prefix = "db")`

```Java

@ConfigurationProperties(prefix = "db")
public record DatabaseProperties(String username,
                                 PoolProperties pool,
                                 String password,
                                 String driver,
                                 String url,
                                 String hosts) {

    public record PoolProperties(Integer size) {
    }
}
```

![[images/Untitled 25 6.png|Untitled 25 6.png]]

## Tom: Spring Boot Starter Logger

![[images/Untitled 26 6.png|Untitled 26 6.png]]

![[images/Untitled 27 6.png|Untitled 27 6.png]]

Можно в properties прописывать настройки

```Java
logging.file.name= springLogs.log
logging.file.path= /logging.level.root = INFO
```

А также в случае необходимости переопределить дефолтное поведение можно создать logback-spring.xml, который является зарезервированным и переписать в соответсвии с документацией

[https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/howto-logging.html](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/howto-logging.html)

  

## Tom: Spring Boot Starter Test

Подгружаем зависимость

```Groovy
dependencies {
testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
    useJUnitPlatform()  // JUnit5
}
```

Основные пакеты в зависимостях

```Groovy
org.springframework.boot:spring-boot-test-autoconfigure:3.1.4
org.springframework.boot:spring-boot-test:3.1.4
org.springframework:spring-test:6.0.12  // здесь содержатся аннотации
```

Также в test- resources добавляем properties и меняем названием. application.properties

```Java
application-test.properties
```

Помечаем тесты аннотацией ``@ActiveProfiles("test")``

```Java
@ActiveProfiles("test")
@SpringBootTest
public class UserServiceIT {
```

Можно создать свою аннотацию и вынести туда все

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@ActiveProfiles("test")
@SpringBootTest
public @interface IT {
}

@IT
public class UserServiceIT {
```

### Обычный тест Service с помощью Mockito

Чтобы протестировать Service

```Groovy
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public Optional<UserDto> findById(Long id){
        return userRepository.findById(id)
                .map(x-> new UserDto(x.id()));
    }
}
```

```Groovy
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    private static final Long USER_ID = 1L;
    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void findById() {
        Mockito.doReturn(Optional.of(new User(USER_ID)))
                .when(userRepository).findById(USER_ID);

        Optional<UserDto> actualResult = userService.findById(USER_ID);

        Assertions.assertTrue(actualResult.isPresent());

        UserDto expectedResult = new UserDto(USER_ID);

        actualResult.ifPresent(actual -> assertEquals(expectedResult, actual));

        verify(userRepository, times(1)).findById(USER_ID);
    }

}
```

### Integration tests

### Теория

![[images/Untitled 28 6.png|Untitled 28 6.png]]

В spring реализован свой framework чтобы писать интеграционные тесты.

Мы помечая наши тестовые классы`@ExtendWith(SpringExtension.class)` позволяем спрингу под капотом создать для каждого класса создать свой объект TestContextManager. Этот Manager ответственен за интеграционные тесты.

![[images/Untitled 29 6.png|Untitled 29 6.png]]

В свою очередь SpringExtension class имплементирует следующие функциональные интерфейсы из junit.

```Java
public class SpringExtension implements BeforeAllCallback, AfterAllCallback, TestInstancePostProcessor,
		BeforeEachCallback, AfterEachCallback, BeforeTestExecutionCallback, AfterTestExecutionCallback,
		ParameterResolver
```

И переопределяет методы этих интерфейсов, где уже непосредственно получает TestContenxtManager на основе ExtensionContext из junit

```Java
@Override
	public void beforeAll(ExtensionContext context) throws Exception {
		getTestContextManager(context).beforeTestClass();
	}
```

### Интеграционный тест Servicer

1. `@ExtendWith(SpringExtension.class)` - Эта аннотация указывает JUnit 5 использовать `SpringExtension` для выполнения тестов. `SpringExtension` интегрирует Spring Framework с JUnit 5 и обеспечивает настройку контекста Spring перед выполнением тестов и очистку его после выполнения тестов. Это позволяет вам использовать возможности Spring, такие как инъекция зависимостей, ваши сервисы и репозитории в интеграционных тестах.
2. `@ContextConfiguration(classes = ApplicationRunner.class)` - Эта аннотация указывает на конфигурационный класс (или классы), которые будут использоваться для настройки контекста Spring во время выполнения тестов. В вашем случае, класс `ApplicationRunner` предоставляет конфигурацию для контекста Spring. В этом контексте будут созданы бины, определенные в классе `ApplicationRunner`, а также любые другие бины, необходимые для выполнения интеграционных тестов.

```Java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = ApplicationRunner.class)
public class UserServiceIT {

    @Test
    void findById(){

    }
}
```

Если необходимо считывать application.yml то нужно добавить `I ConfigDataApplicationContextInitializer`

```Java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = ApplicationRunner.class, initializers = {
        ConfigDataApplicationContextInitializer.class
})
public class UserServiceIT {

    @Test
    void findById(){

    }
}
```

Обе эти аннотации можно заменить на `@SpringBootTest.` Она уже содержит `@ExtendWith(SpringExtension.class)` и сама автомачески сканит класс, который помечент `@SpringBootApplication`

```Java
@SpringBootTest
public class UserServiceIT {
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@BootstrapWith(SpringBootTestContextBootstrapper.class)
@ExtendWith(SpringExtension.class)
public @interface SpringBootTest {
```

Тест выглядит таким образом, здесь так как это интеграционный необходимо внедрять реальные объекты через `@Autowired`

```Java
@IT
@RequiredArgsConstructor
public class UserServiceIT {
    private static final Long USER_ID = 1L;
    @Autowired
    private UserService userService;

    @Autowired
    private DatabaseProperties databaseProperties;
    @Test
    void findById(){

        Optional<UserDto> actualResult = userService.findById(USER_ID);

        Assertions.assertTrue(actualResult.isPresent());

        UserDto expectedResult = new UserDto(USER_ID);

        actualResult.ifPresent(actual -> assertEquals(expectedResult, actual));

    }
}
```

Можно избавиться от `@Autowired` с помощью `@RequiredAgrsConstructor` и `@TestConstructor(autowireMode = TestConstructor.AutowireMode.ALL)`

```Java
@IT
@RequiredArgsConstructor
@TestConstructor(autowireMode = TestConstructor.AutowireMode.ALL)
public class UserServiceIT {
    private static final Long USER_ID = 1L;
    private final UserService userService;
    private final DatabaseProperties databaseProperties;
    @Test
    void findById(){

        Optional<UserDto> actualResult = userService.findById(USER_ID);

        Assertions.assertTrue(actualResult.isPresent());

        UserDto expectedResult = new UserDto(USER_ID);

        actualResult.ifPresent(actual -> assertEquals(expectedResult, actual));

    }
}
```

Можно убрать @TestConstructor(autowireMode = TestConstructor.AutowireMode.ALL), если в тестовых ресурсах добавить [spring.properties](http://spring.properties) и прописать туда

```Java
spring.test.constructor.autowire.mode = all
```

  

## Tom: Spring Data JPA Starter

### Преимущества

![[images/Untitled 30 6.png|Untitled 30 6.png]]

Минусы Hibernate:

1. Необходимо создавать DAO слой для доступа к БД.
2. Hibernate не предоставляет какого-то Transaction manager, его необходимо создавать самому, чтобы открывать и закрывать транзакцию
3. Настройка сложная, xml, необходимо создавать Session Factory

![[images/Untitled 31 6.png|Untitled 31 6.png]]

1. Spring Data JPA решает все эти проблемы, Repository делает автоматически DAO для всех сущностей.
2. Manager уже есть встроенный для работы с транзакциями.
3. Автоконфигурация, настраиваемая через properties или yaml файл без xml конфигурации.

  

  

  

### Внедрение

```Java
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

Эта имплементация транзитивно подтягивает spring-boot, spring boot starter jdbc, а также по умолчанию использует HikariCP (Connection pool)

  

### Configuration

Основной класс отвечающий за автоматическую настройку Hibernate - это `HibernateJPACAutoOnfiguration`

```Java
@AutoConfiguration(
		after = { DataSourceAutoConfiguration.class, TransactionManagerCustomizationAutoConfiguration.class },
		before = TransactionAutoConfiguration.class)
@ConditionalOnClass({ LocalContainerEntityManagerFactoryBean.class, EntityManager.class, SessionImplementor.class })
@EnableConfigurationProperties(JpaProperties.class)
@Import(HibernateJpaConfiguration.class)
public class HibernateJpaAutoConfiguration {

}
```

1. `@AutoConfiguration`: Эта аннотация указывает, что класс `HibernateJpaAutoConfiguration` является частью автоконфигурации Spring Boot. Автоконфигурация - это механизм Spring Boot, который автоматически настраивает приложение на основе его зависимостей и настроек.
2. `after` и `before`: Эти атрибуты аннотации `@AutoConfiguration` указывают, что данная конфигурация должна быть выполнена после `DataSourceAutoConfiguration` и `TransactionManagerCustomizationAutoConfiguration`, но перед `TransactionAutoConfiguration`. Это важно для правильного порядка настройки бинов в приложении.

  

```Java
@AutoConfiguration(before = SqlInitializationAutoConfiguration.class)
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })
@ConditionalOnMissingBean(type = "io.r2dbc.spi.ConnectionFactory")
@EnableConfigurationProperties(DataSourceProperties.class)
@Import(DataSourcePoolMetadataProvidersConfiguration.class)
public class DataSourceAutoConfiguration {
```

```Java
@ConfigurationProperties(prefix = "spring.datasource")
public class DataSourceProperties implements BeanClassLoaderAware, InitializingBean {
```

  

  

1. `@ConditionalOnClass`: Эта аннотация указывает, что данная конфигурация будет активирована только в том случае, если определенные классы, перечисленные внутри `@ConditionalOnClass`, присутствуют в класспасе приложения. В данном случае, это классы `LocalContainerEntityManagerFactoryBean`, `EntityManager` и `SessionImplementor`, которые связаны с JPA и Hibernate.
    
    Аннотация включает в себя `LocalContainerEntityManagerFactoryBean.class`, и `EntityManager.class` по сути обертка над Hibernate и его классов для создания Connections.
    
2. `@EnableConfigurationProperties(JpaProperties.class)`: Эта аннотация позволяет использовать настройки JPA, определенные в классе `JpaProperties`, внутри данной конфигурации. Это позволяет настраивать поведение JPA и Hibernate через параметры приложения.
    
    ```Java
    @ConfigurationProperties(prefix = "spring.jpa")
    public class JpaProperties {
    ```
    
    Который можно также создать вручную
    
3. `@Import(HibernateJpaConfiguration.class)`: Эта аннотация импортирует дополнительную конфигурацию `HibernateJpaConfiguration`, которая содержит дополнительные настройки для Hibernate и JPA.
    
    `HibernateJpaConfiguration.class` (здесь все связано с Hibernate) наследуется от абстрактного класса `JpaBaseConfiguration`, здесь происходит основная настройка EntityManagerFactory, TransactionManager и тд.
    
    ```Java
    @Configuration(proxyBeanMethods = false)
    @EnableConfigurationProperties(JpaProperties.class)
    public abstract class JpaBaseConfiguration {
    
    	private final DataSource dataSource;
    
    	private final JpaProperties properties;
    
    	private final JtaTransactionManager jtaTransactionManager;
    
    	...
    	}
    ```
    
    Многие методы помечены аннотацией `@ConditionalOnMissingBean(TransactionManager.class)`, это означает что мы можем переопределить создание ENtityManager, и эти методы не будут вызванаю
    
      
    
    В properties можно настраивать spring.jpa и spring.datasource
    
    ```Java
    spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
    spring.datasource.username=postgres
    spring.datasource.password=postgres
    spring.jpa.show-sql=true
    spring.jpa.generate-ddl=true
    
    spring.datasource.driver-class-name=org.postgresql.Driver
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
    ```
    

  

`**==DataSourceProperties.class,==**` **==который можно настраивать с помощью==** `**==spring.datasource==**`

`DataSource.class` из пакета `package javax.sql,` который отвечает за получение connection к БД. на уровне СУБД. (HikariDataSource наследуется от DataSource)

### Transactions

в Spring Data у нас есть TransactionManager, который конфигурируется в JPABaseConfiguration

![[images/Untitled 32 6.png|Untitled 32 6.png]]

Этот TransactionManager можно использовать двумя способами декларативным через аннотации, а также вручную через TransactionTemplate

![[images/Untitled 33 6.png|Untitled 33 6.png]]

### Декларативный метод @Transactional

### TestContext

В первую очередь manager - это proxy, по сути обертка над нашим соединением

```Java
manager = {$Proxy123@12436} "Shared EntityManager proxy for target factory [org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean@4a29a1e6]"
h = {SharedEntityManagerCreator$SharedEntityManagerInvocationHandler@12441}
```

Если вызвать код в таком виде, то будет LazyInitializationException, так как мы должны сохранять сессию и транзакцию во время получения нашего юзера.

```Java
@SpringBootTest
public class BookRepositoryTest {

    private final EntityManager manager;

    @Autowired
    public BookRepositoryTest(EntityManager manager) {
        this.manager = manager;
    }
    @Test
    void findById(){
        Book book = manager.find(Book.class, 1);
        assertThat(book).isNotNull();
        assertThat(book.getUser().getName()).isEqualTo("Иван");
    }
}
```

Решение - это аннотация из spring @Transactional

```Java
@SpringBootTest
public class BookRepositoryTest {

    private final EntityManager manager;

    @Autowired
    public BookRepositoryTest(EntityManager manager) {
        this.manager = manager;
    }
    @Test
		@Transactional
    void findById(){
        Book book = manager.find(Book.class, 1);
        assertThat(book).isNotNull();
        assertThat(book.getUser().getName()).isEqualTo("Иван");
    }
}
```

  

### TransactionTestExecutionListener, @Rollback, @Commit

`TransactionTestExecutionListener` - это слушатель тестовых выполнений, предоставляемый Spring Framework. Он предназначен для управления транзакциями в тестах. Этот слушатель позволяет начинать и завершать транзакции перед и после выполнения тестовых методов.

По умолчанию TransactionManager, который предоставляет этот слушатель, реализован по логике rollback, наши тесты не вносят в бд изменения. `@Rollback(value = true)` по дефолту стоит true

```Java
@SpringBootTest
public class BookRepositoryTest {

    @Test
		@Transactional
		@Rollback(value = true)  
    void findById(){
        Book book = manager.find(Book.class, 1);
        assertThat(book).isNotNull();
        assertThat(book.getUser().getName()).isEqualTo("Иван");
    }
}
```

Для того, чтобы изменить логику на коммит, можно поставить false, либо заменить на аннотацию @Commit

```Java
@SpringBootTest
public class BookRepositoryTest {

    @Test
		@Transactional
		@Commit  
    void findById(){
        Book book = manager.find(Book.class, 1);
        assertThat(book).isNotNull();
        assertThat(book.getUser().getName()).isEqualTo("Иван");
    }
}
```

### AOP

### TransactionAutoConfiguration

### TransactionTemplate

  

## Tom: Spring Security Starter

### Section: Введение

![[images/Untitled 34 6.png|Untitled 34 6.png]]

- Как было в `Servlet`

![[images/Untitled 35 6.png|Untitled 35 6.png]]

- Как стало в ==Spring==

![[images/Untitled 36 6.png|Untitled 36 6.png]]

  

- Когда ==подключили== `S S Starter`, то у ==нас уже есть настроенная конфигурация==

![[images/Untitled 37 6.png|Untitled 37 6.png]]

- Задача настраивать `SecurityFilerChain`

![[images/Untitled 38 6.png|Untitled 38 6.png]]

  

### Section:  Authentication Architecture

![[images/Untitled 39 6.png|Untitled 39 6.png]]

![[images/Untitled 40 6.png|Untitled 40 6.png]]

]

### Section: DaoAuthenticationProvider

- _Самый первый и распространенный_ `_Authentication Provider_`_, который используется практически во всех приложениях - это_ `_DaoAuthenticationProvider_`_, т.е._ ==_вход в приложения по логину и паролю. Для его реализации нам понадобится добавить password колонку в таблицу users, подправить сущности и реализовать_== `_UserDetailsService_`_, который и_ ==_будет делать запрос в базу данных и искать нашего пользователя в ней._==

![[images/Untitled 41 6.png|Untitled 41 6.png]]

![[images/Untitled 42 6.png|Untitled 42 6.png]]

  

## Tom: Spring AOP

![[images/Untitled 43 6.png|Untitled 43 6.png]]

### Терминология

[https://javarush.com/quests/lectures/questspring.level01.lecture60](https://javarush.com/quests/lectures/questspring.level01.lecture60)

- ==Аспект (aspect)==: Модульно организованная функциональность  
    (concern), которая охватывает несколько  
    ==классов==. Управление транзакциями является хорошим примером сквозной функциональности в корпоративных  
    Java-приложения  
    ==х. В Spring AOP аспекты реализуются с помощью обычных  
    классов (подход на основе схем  
    ==) или обычных классов, аннотированных  
    аннотацией  
    `@Aspect` (стиль @AspectJ).
- ==Точка соединения (joint point):== ==Точка во время выполнения  
    программы, например, выполнение метода или обработка исключения.  
    ====В  
    Spring AOP точка соединения всегда представляет собой выполнение метода.  
    ==
- ==Advice==: ==Действие, предпринимаемое аспектом в определенной точке  
    соединения.  
    ==Различные виды Advice включают Advice "вместо (`around`)",  
    "перед (  
    `before`)" и "после (`after`)". (Типы Advice будут рассмотрены  
    дальше). Многие АОП-фреймворки, включая Spring, моделируют Advice как  
    перехватчик и поддерживают цепочку перехватчиков вместо точки  
    соединения.  
    
- ==Срез (pointcut):== ==Предикат, который соответствует точкам  
    соединения  
    ==. `Advice` ==связан с выражением среза и выполняется в любой точке соединения, совпадающей со срезом== (например, выполнение метода с  
    определенным именем). Концепция точек соединения, сопоставленных с  
    выражениями среза, является центральной в АОП, а Spring по умолчанию  
    использует язык выражений срезов AspectJ.  
    
- ==Введение (introduction):== Объявление дополнительных методов или  
    полей от имени типа. Spring AOP позволяет вам внедрять новые интерфейсы  
    (и соответствующую реализацию) в любой снабженный Advice-ом объект.  
    Например, можно использовать введение, чтобы в принудительном порядке  
    бин реализовывал интерфейс  
    `IsModified` для упрощения кэширования. (В сообществе AspectJ введение известно как межтиповое объявление).
- ==Целевой объект (Target object):== Объект, который снабжается  
    Advice-ом по одному или нескольким аспектам. Также называется  
    "снабжаемый Advice-ом объект". Поскольку Spring AOP реализован с  
    использованием динамических прокси, этот объект всегда является  
    проксированным объектом.  
    
- ==Прокси АОП==: Объект, создаваемый АОП-фреймворком для реализации  
    аспектных контрактов (снабжает Advice-ом выполнение методов и так  
    далее). В Spring Framework прокси АОП – это динамический прокси JDK или  
    прокси CGLIB.  
    
- ==Связывание (weaving):== связывание аспектов с другими типами  
    приложений или объектами для создания снабженного советом объекта. Оно  
    может быть произведено во время компиляции (например, с помощью  
    компилятора AspectJ), во время загрузки или во время выполнения  
    программы. Spring AOP, как и другие чистые АОП-фреймворки на Java,  
    осуществляет связывание во время выполнения программы.  
    

### PointCut (срезы для внедрения в них сквозной логики)

Срезы ищут соответсвующие joinpoints через регулярные выражения

```JavaScript
package ru.konovalov.qsmsa.aop;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class FirstAspect {
    /*
    @within - проверка аннотации на уровне класса
     */

    @Pointcut("@within(org.springframework.stereotype.Controller)")
    public void isControllerLayer() {
    }
    
     /*
    within - проверка класса по имени
     */

    @Pointcut("within(ru.konovalov.qsmsa..*Service)")
    public void isServiceLayer() {
    }
    
     /*
    within здесь не подходит, так как может вернуться прокси с другим именем, а аннотацию можно не ставить
    this  - проверяет текущий аоп прокси класс
    target - проверяет класс вокруг которого оборачивали текущий объект
   
     */

    @Pointcut("this(org.springframework.data.repository.Repository)")
    //@Pointcut("this(org.springframework.data.repository.Repository)")
    public void isRepositoryLayer() {
    }

    /*
    @annotation - проверяет аннотацию на уровне метода
    чтобы ограничивать поиск по бинам, можно комбинировать условия
     */
    @Pointcut("isControllerLayer() && @annotation(ru.konovalov.qsmsa.aop.LogExecutionTime)")
    public void hasGetMapping() {
    }

    /*
    args проверяет параметры метода
    * - любой тип параметра
    .. - 0+ любой тип параметра, в примере ниже нас не интересует сколько параметров будет после Model
     */
    @Pointcut("args(org.springframework.ui.Model,..)")
    public void hasModelParam() {
    }

    /*
    @args - проверяет аннотацию над типом параметра
     */
    @Pointcut("@args(ru.konovalov.spring.validation.UserInfo)")
    public void hasUserInfoParamAnnotation() {
    }

    /*
    bean - создает срез по названию бина
     */
    @Pointcut("bean(*Service)")
    public void isBeanName() {
    }
    
    

    @Pointcut("execution(public *ru.konovalov.qsmsa.*Controller.*findById(*))")
    public void anyFindByIdServiceMethod() {
    }

}
```

### Сквозная функциональность (Advices)

### @Before

  

### @After

  

### @Around

  

### JoinPoint. Params

Давайте создадим список методов класса `JoinPoint` и предоставим краткое описание каждого метода, а также приведем примеры на Java:

1. `getArgs()`
    
    - Возвращаемый тип: `java.lang.Object[]`
    - Описание: Возвращает аргументы данной точки среза.
    - Пример:
    
    ```Java
    Object[] args = joinPoint.getArgs();
    ```
    
2. `getKind()`
    
    - Возвращаемый тип: `java.lang.String`
    - Описание: Возвращает строку, представляющую тип точки среза.
    - Пример:
    
    ```Java
    String kind = joinPoint.getKind();
    ```
    
3. `getSignature()`
    
    - Возвращаемый тип: `Signature`
    - Описание: Возвращает сигнатуру (Signature) данной точки среза.
    - Пример:
    
    ```Java
    Signature signature = joinPoint.getSignature();
    ```
    
4. `getSourceLocation()`
    
    - Возвращаемый тип: `SourceLocation`
    - Описание: Возвращает исходное местоположение (SourceLocation), соответствующее данной точке среза.
    - Пример:
    
    ```Java
    SourceLocation sourceLocation = joinPoint.getSourceLocation();
    ```
    
5. `getStaticPart()`
    
    - Возвращаемый тип: `JoinPoint.StaticPart`
    - Описание: Возвращает объект, инкапсулирующий статические части данной точки среза.
    - Пример:
    
    ```Java
    JoinPoint.StaticPart staticPart = joinPoint.getStaticPart();
    ```
    
6. `getTarget()`
    
    - Возвращаемый тип: `java.lang.Object`
    - Описание: Возвращает целевой объект (target object).
    - Пример:
    
    ```Java
    Object target = joinPoint.getTarget();
    ```
    
7. `getThis()`
    
    - Возвращаемый тип: `java.lang.Object`
    - Описание: Возвращает объект, который в данный момент выполняет метод.
    - Пример:
    
    ```Java
    Object thisObject = joinPoint.getThis();
    ```
    
8. `toLongString()`
    
    - Возвращаемый тип: `java.lang.String`
    - Описание: Возвращает расширенное строковое представление данной точки среза.
    - Пример:
    
    ```Java
    String longString = joinPoint.toLongString();
    ```
    
9. `toShortString()`
    
    - Возвращаемый тип: `java.lang.String`
    - Описание: Возвращает сокращенное строковое представление данной точки среза.
    - Пример:
    
    ```Java
    String shortString = joinPoint.toShortString();
    ```
    
10. `toString()`
    
    - Возвращаемый тип: `java.lang.String`
    - Описание: Возвращает строковое представление данной точки среза.
    - Пример:
    
    ```Java
    String pointString = joinPoint.toString();
    ```
    
    Понял вас. Давайте рассмотрим, как методы класса `JoinPoint` могут использоваться вместе с REST-контроллером. Для этого предоставлю примеры, как эти методы могут быть применены в аспекте, связанном с REST-контроллером.
    
    Предположим, у вас есть простой REST-контроллер:
    
    ```Java
    @RestController
    public class MyController {
    
        @GetMapping("/example")
        public String getExample(@RequestParam("param1") String param1, @RequestParam("param2") int param2) {
            return "Received parameters: param1=" + param1 + ", param2=" + param2;
        }
    }
    ```
    
    И аспект, который применяется к этому контроллеру с использованием Spring AOP:
    
    ```Java
    @Aspect
    public class RestControllerLoggingAspect {
    
        @Before("@annotation(org.springframework.web.bind.annotation.GetMapping)")
        public void logRestRequest(JoinPoint joinPoint) {
            // Методы класса JoinPoint используются для логирования информации о запросе.
    
            // Получение аргументов запроса.
            Object[] args = joinPoint.getArgs();
            System.out.println("Received arguments: " + Arrays.toString(args));
    
            // Получение сигнатуры метода.
            Signature signature = joinPoint.getSignature();
            System.out.println("Method Signature: " + signature.toLongString());
    
            // Получение целевого объекта (контроллера).
            Object target = joinPoint.getTarget();
            System.out.println("Target Object: " + target);
    
            // Получение текущего объекта.
            Object thisObject = joinPoint.getThis();
            System.out.println("Current Object: " + thisObject);
        }
    }
    ```
    
    В этом примере аспект `RestControllerLoggingAspect` использует методы `JoinPoint` для логирования информации о запросах к REST-контроллеру. Когда GET-запрос выполняется к `/example`, аспект срабатывает перед выполнением метода `getExample`.
    
    Таким образом, методы `JoinPoint` используются для извлечения информации о запросе, сигнатуре метода, целевом и текущем объектах, и эту информацию можно легко логировать или выполнять другие действия на уровне аспекта.
    

### Best Practices