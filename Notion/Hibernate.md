---
modified: 2024-09-02T14:53:01+03:00
---
### Общая информация

`Hibernate` — это ==Библиотека== для языка Java, который предоставляет инструменты и функциональность для работы с реляционными базами данных. Он решает ряд проблем, связанных с взаимодействием приложения с базой данных. Вот некоторые из основных проблем, которые решает Hibernate:

1. `Постоянность данных:` Hibernate ==позволяет сохранять, извлекать и изменять данные в базе данных с помощью объектно-ориентированного подхода.== Он обеспечивает прозрачную синхронизацию данных между объектами Java и таблицами в базе данных, обеспечивая постоянность данных.
2. `Управление соединениями с базой данных:` Hibernate ==предоставляет механизмы управления соединениями с базой данных, скрывая сложности взаимодействия с JDBC (Java Database Connectivity). Он автоматически устанавливает и закрывает соединения, обрабатывает транзакции и управляет пулом соединений==.
3. `Сопоставление объектно-реляционной модели:` Hibernate ==позволяет разработчикам работать с объектами Java, а не с языком SQL, при взаимодействии с базой данных. Он предоставляет мощный механизм сопоставления объектно-реляционной модели (ORM), который позволяет преобразовывать данные из таблиц базы данных в объекты Java и обратно.==
4. Работа с запросами и языком `HQL`: Hibernate ==предлагает свой собственный язык запросов, называемый HQL== (Hibernate Query Language). HQL подобен SQL, но работает с объектами Java вместо таблиц базы данных. Он предоставляет мощные возможности для формирования запросов и извлечения данных из базы данных.
5. `Кэширование данных:` Hibernate ==предоставляет механизмы кэширования данных==, что позволяет улучшить производительность приложения. Он ==поддерживает кэширование на уровне объектов, запросов и вторичного кэша базы данных.==
6. `Управление транзакциями:` Hibernate обеспечивает управление транзакциями базы данных, что позволяет разработчикам безопасно выполнять операции в рамках транзакций. Он автоматически обрабатывает фиксацию, откат и изоляцию транзакций.
7. `Поддержка различных СУБД:` Hibernate ==поддерживает различные системы управления базами данных (СУБД)==, такие как MySQL, Oracle, PostgreSQL и др. 

### Внедрение

Для использования Hibernate необходимо подключить зависимость

Для Gradle

```Java
dependencies {
    // https://mvnrepository.com/artifact/org.hibernate/hibernate-core
    implementation 'org.hibernate:hibernate-core:6.2.5.Final'
```

После этого необходимо добавить конфигурационный файл `hibernate.cfg.xml` либо вручную в ресурсах, либо автоматически при добавлении модуля Hibernate через Project Structure→Modules→ **+** → Hibernate.

После этого в этом файле прописываются properties:

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="connection.url">jdbc:postgresql://localhost:5433/postgres</property>
        <property name="connection.driver_class">org.postgresql.Driver</property>
        <property name="connection.username">postgres</property>
        <property name="connection.password">pass</property>
 // указать тот диалект с какой СУБД работаем
        <property name="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</property>
				<property name="hibernate.show_sql">false</property> //вывод SQL запросов в консоль
		    <property name="hibernate.hbm2ddl">validate</property>
        <!-- DB schema will be updated if needed -->
        <!-- <property name="hibernate.hbm2ddl.auto">update</property> -->
    </session-factory>
</hibernate-configuration>
```

Создаем конфиг класс и вызываем метод `configure`:

```Java
public class HibernateRunner {
    public static void main(String[] args) {
        Configuration configuration = new Configuration();
        configuration.configure();

        @Cleanup
        SessionFactory sessionFactory = configuration.buildSessionFactory();
        @Cleanup
        Session session = sessionFactory.openSession();
```

### Второй способ это в resources создать [hibernate.properties](http://hibernate.properties) и прописать все настройки там.

![[images/Untitled 153.png|Untitled 153.png]]

==Осторожно с update, может обнулить данные==

Третий способ сделать через экземпляр класса `Propertes`

![[images/Untitled 1 14.png|Untitled 1 14.png]]

### Под капотом у Hibernate

```Java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
@Table(name ="users", schema = "public")
public class User {
    @Id
    private String userName;
    private String firstName;
    private String lastName;
    @Column(name = "birth_date")
    private LocalDate birthDate;
    private Integer age;
}

class HibernateRunnerTest {
    @Test
    void checkReflectionApi() throws SQLException, IllegalAccessException {
        User user = User.builder()
                .userName("ivan228@yandex.ru")
                .firstName("Ivan")
                .lastName("Kara4evskiy")
                .birthDate(LocalDate.of(1996, 4, 21))
                .age(27)
                .build();
        String sql = """
                INSERT
                INTO
                %s
                (%s)
                values
                (%s)
                """;
        String tableName = Optional.ofNullable(user.getClass().getAnnotation(Table.class))
                .map(tableAnnotation -> tableAnnotation.schema() + "." + tableAnnotation.name())
                .orElse(user.getClass().getName());

        Field[] declaredFields = user.getClass().getDeclaredFields();

        String columnNames = Arrays.stream(declaredFields)
                .map(field -> Optional.ofNullable(field.getAnnotation(Column.class))
                        .map(Column::name)
                        .orElse(field.getName()))
                .collect(Collectors.joining(", "));

        String columnValues = Arrays.stream(declaredFields)
                .map(field -> "?")
                .collect(Collectors.joining(", "));

        String formatted = sql.formatted(tableName, columnNames, columnValues);
        @Cleanup
        Connection connection = null;
        @Cleanup
        PreparedStatement statement = connection.prepareStatement(formatted);
        for (Field declaredField : declaredFields) {
            declaredField.setAccessible(true);

            statement.setObject(1, declaredField.get(user));
                    
        }
    }
}
```

### Entities, Enumerated, Embedded components

### Type Converts

![[images/Untitled 2 13.png|Untitled 2 13.png]]

![[images/Untitled 3 12.png|Untitled 3 12.png]]

![[images/Untitled 4 12.png|Untitled 4 12.png]]

- ==Можно подключать другие конверторы==

  

### Hibernate Enity

Чтобы класс был сущностью Hibernate есть несколько правил:

- ==Класс должен быть POJO==. (Все поля сущности должны быть private и должны быть сеттеры и геттеры ко всем полям)
- Иметь ==пустой конструктор==
- Содержать ==аннотации== `@Entity` и `@Id`
- Добавить либо в `hibernate.cfg.xml` mapping либо добавить программно аннотированный класс методом `addAnnotatedClass(User.class):
    
    ```Java
    <mapping class="ru.konovalov.entity.User"/>
    ```
    
    ```Java
    configuration.addAnnotatedClass(User.class);
    ```
    
- Также необходимо указать названия таблицы через аннотацию @Table, если название сущности и название таблицы отличается

```Java
package ru.konovalov.entity;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDate;
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
@Table(name ="users", schema = "public")
public class User {
    @Id
    private String userName;
		@Basic
    private String firstName;
		@Basic
    private String lastName;
    @Column(name = "birth_date")
    private LocalDate birthDate;
		@Basic
    private Integer age;
}
```

При несоответствии названия поля и аналогичной колонки в таблице необходимо либо использовать аннотацию `@Column` либо если поля в CamelCase, а названия колонок в базе данных через Under Score, то можно установить стратегию преобразования у конфигурации:

```Java
configuration.setPhysicalNamingStrategy(
new CamelCaseToUnderscoresNamingStrategy()
);
```

### Пример добавления сущности в базу данных:

```Java
public class HibernateRunner {
    public static void main(String[] args) {
        Configuration configuration = new Configuration().configure();
        configuration.addAnnotatedClass(User.class);
        configuration.setPhysicalNamingStrategy(new CamelCaseToUnderscoresNamingStrategy());
        
        @Cleanup
        SessionFactory sessionFactory = configuration.buildSessionFactory(builder.build());
        @Cleanup
        Session session = sessionFactory.openSession();
        session.beginTransaction(); //необходимо начать транзакцию и завершить либо откатить
        User user = User.builder()
                .userName("ivan228@yandex.ru")
                .firstName("Ivan")
                .lastName("Kara4evskiy")
                .birthDate(LocalDate.of(1996, 4, 21))
                .age(27)
                .build();
        session.persist(user);
        session.getTransaction().commit();
    }

}
```

  

![[images/Untitled 5 12.png|Untitled 5 12.png]]

### @Enumerated

Аннотация `@Enumerated` в Hibernate используется для указания способа хранения перечисляемого типа (enum) в базе данных. Она позволяет установить, как Hibernate будет преобразовывать значения перечисляемого типа в соответствующие значения в базе данных и наоборот.

Применение аннотации `@Enumerated` к полю или геттеру/сеттеру перечисляемого типа позволяет задать одно из трех значений перечисления `EnumType.STRING`, `EnumType.ORDINAL` или `EnumType.NUMERIC`.

1. `EnumType.STRING` (по умолчанию): Значения перечисления будут сохраняться в базе данных как строки, соответствующие именам элементов перечисления.
2. `EnumType.ORDINAL`: Значения перечисления будут сохраняться в базе данных как целочисленные значения, соответствующие порядковому номеру элемента перечисления (начиная с 0).
3. `EnumType.NUMERIC`: Значения перечисления будут сохраняться в базе данных как целочисленные значения, соответствующие значению, заданному в атрибуте `value` аннотации `@Enumerated`.

Примеры использования аннотации `@Enumerated`:

```Java
public enum Gender {
    MALE,
    FEMALE
}

@Entity
public class Person {
    @Enumerated(EnumType.STRING)
    private Gender gender;

    // Геттеры и сеттеры
}
```

В данном примере значение поля `gender` будет сохраняться в базе данных как строка, соответствующая имени элемента перечисления `Gender`.

```Java
public enum Role {
    ADMIN,
    USER
}

@Entity
public class User {
    @Enumerated(EnumType.ORDINAL)
    private Role role;

    // Геттеры и сеттеры
}
```

В данном примере значение поля `role` будет сохраняться в базе данных как целочисленное значение, соответствующее порядковому номеру элемента перечисления `Role`.

Использование аннотации `@Enumerated` позволяет более гибко управлять способом хранения перечисляемых типов в базе данных при использовании Hibernate.

### Embedded components

Аннотации `@Embedded` и `@Embeddable` позволяют создавать сложные составные объекты( композитные) (embedded - встраиваемый)

Аннотация `@Embeddable` используется для обозначения класса, который будет встроен в другой класс с помощью аннотации `@Embedded`. Это означает, что поля класса, помеченного аннотацией `@Embeddable`, будут сохранены в той же таблице, что и содержащий его класс.

==ВАЖНО: В Hibernate версии 5 embeddable классы должны имплентировать Serializable, в 6-ке ничего имплементировать не нужно.==

```Java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Embeddable
@Builder
public class PersonInfo {

    private String userName;
    private String firstName;
    private String lastName;
    private Birthday birthDay;
    private Deathday deathDay;
}
```

```Java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
@Table(name = "users", schema = "public")
public class User {

    @Id
    @GeneratedValue(GenerationType.IDENTITY)
    private Long id;

    @Embedded
    @AttributeOverride(name = "birthDay", column = @Column(name = "birth_date"))
    @AttributeOverride(name = "deathDay", column = @Column(name = "death_date"))
    private PersonInfo info;

    @Enumerated(EnumType.STRING)
    private Role role;
```

В примере выше также используется аннотация `@AttributeOverride` , которая ==позволяет переопределить колонку назначения поля встраиваемого компонента.==

```Java
 Configuration configuration = new Configuration().configure();
        configuration.addAttributeConverter(DeathdayConverter.class, true);
        configuration.addAttributeConverter(BirthdayConverter.class, true);
        configuration.addAnnotatedClass(User.class);
        configuration.setPhysicalNamingStrategy(new CamelCaseToUnderscoresNamingStrategy());

        try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession()) {
            session.beginTransaction();
            User user = User.builder()
                    .info(PersonInfo.builder()
                            .userName("Vredina")
                            .firstName("Dmitriy")
                            .lastName("Parfenov")
                            .birthDay(new Birthday(LocalDate.now()))
                            .deathDay(new Deathday(LocalDate.now(), LocalDate.now().plusDays(10)))
                            .build())
                    .role(Role.ADMIN)
                    .recipe(new Recipe("cancer", "lie in bed"))
                    .build();
            session.persist(user);
            session.getTransaction().commit();
```

```Plain
Hibernate: 
    insert 
    into
        public.users
        (birth_date,death_date,first_name,last_name,user_name,recipe,role,id) 
    values
        (?,?,?,?,?,?,?,?)
```

![[images/Untitled 6 11.png|Untitled 6 11.png]]

  

### Entity States in JPA and Hibernate

В ==JPA существуют 4 состояния объекта==: New (`Transient`), `Persistent` (Managed), `Detached` (Unmanaged) и `Removed` (удаленный).

![[images/Untitled 7 10.png|Untitled 7 10.png]]

### Transient

В данном состоянии находятся ==вновь созданные сущности, которые не связаны с PersistentContext== ( то есть не переведены в Persistent состояние).

### Persistent

Объект, связанный с `PersistentContext` ==конкретной сессии== Hibernate ( ==у каждой сессии свой PersistentContext==), находится в состоянии Persistent.

### PersistenceContext and First-Level-Cache

`First level cache в Hibernate` - ==это механизм кэширования, который используется на уровне сессии.== Он представляет собой контекст сессии, в котором Hibernate ==сохраняет объекты, полученные из базы данных, чтобы избежать частых обращений к базе данных.==

`First level cache` ==работает на уровне объектов.== Когда Hibernate получает объект из базы данных, он сохраняет его в first level cache. При последующих запросах к этому объекту Hibernate возвращает уже сохраненный объект из кэша, не выполняя запрос к базе данных. Это позволяет уменьшить количество обращений к базе данных и повысить производительность.

Метод `evict()` используется для удаления объекта из first level cache. Когда объект больше не нужен или его изменения не должны влиять на состояние базы данных, можно вызвать метод evict() для удаления объекта из кэша. После вызова этого метода Hibernate не будет отслеживать изменения этого объекта и не будет автоматически синхронизировать его состояние с базой данных.

Метод `flush()` используется для синхронизации состояния объектов first level cache с базой данных. Когда вызывается метод flush(), Hibernate проверяет все объекты в кэше и выполняет соответствующие операции с базой данных, чтобы обновить состояние объектов. Это может включать выполнение SQL-запросов для сохранения изменений, удаления объектов или обновления связей между объектами.

Обратите внимание, что Hibernate обычно управляет кэшем автоматически, и в большинстве случаев вам не нужно явно вызывать методы evict() и flush(). Однако, в некоторых ситуациях, когда требуется более тонкое управление кэшем или контроль над обновлением базы данных, эти методы могут быть полезны.

`PersistenceContext` ==в Hibernate представляет собой в общем случае==  
  
`HashMap<EntityKey, Object>.` (EntityKey = айдишник сущности + ==**EntityPersister**==)

![[images/Untitled 8 10.png|Untitled 8 10.png]]

`PersistenceContext` отслеживает состояние объектов, загруженных или измененных в рамках текущей сессии работы с Hibernate. Он является частью сессии и каждая сессия имеет свой собственный PersistenceContext.

![[images/Untitled 9 10.png|Untitled 9 10.png]]

PersistenceContext в Hibernate использует различные структуры данных для хранения и управления объектами. Основной структурой данных, используемой в PersistenceContext, является HashMap, где ключом является идентификатор объекта, а значением - сам объект.

Помимо HashMap, PersistenceContext также использует другие структуры данных, такие как HashSet и списки, для отслеживания изменений и управления жизненным циклом объектов.  

Любые изменения, внесенные в объекты в этом состоянии, автоматически применяются к базе данных без необходимости явного вызова методов `persist, merge, remove`

ВАЖНО: Начиная с версии 6 save(), update() and saveOrUpdate() - deprecated!

- `void persist(Object object)`; `void persist(String entityName, Object object)`

Оба метода `persist` ==выполняют операцию сохранения объекта в контексте JPA==. Объекты, сохраненные с использованием метода `persist`, становятся управляемыми (persistent) и ==будут автоматически синхронизироваться с базой данных при фиксации транзакции или при наступлении других событий синхронизации.==

```Java
Configuration configuration = new Configuration().configure();
    configuration.addAnnotatedClass(Test.class);
    configuration.addAttributeConverter(new BirthdayConverter(), true);
    try(SessionFactory sessionFactory = configuration.buildSessionFactory();
    Session session = sessionFactory.openSession()) {
      session.beginTransaction();
      Test test = Test.builder().text("Hello?").birthday(new Birthday(LocalDate.of(1991, 4, 15))).build();
     
	 session.persist(test); // переводит сущность в persisten state

      session.getTransaction().commit();
```

- `public <T> T find(Class<T> entityClass, Object primaryKey);`

Метод `find()` ==достался интерфейсу== ==_Session_== ==от стандарта JPA.== А как ты знаешь, этот стандарт описывает не просто сигнатуру методов, но и регламентирует поведение.

Этот метод ==работает точно так же, как и метод== `get()`. Если объект по переданному ключу не был найден, то метод просто вернет null.

```Java
User user = session.find(User.class, -2); //метод вернет null
```

- `void refresh(Object object)` ==используется для обновления состояния объекта из базы данных==. Когда вы вызываете `refresh()`  
    для объекта, JPA или  
    `==Hibernate==` ==выполняют запрос к базе данных, чтобы  
    получить последнюю актуальную версию данных для этого объекта. Затем  
    значения всех полей объекта обновляются значениями из базы данных,  
    перезаписывая любые изменения, которые могли быть сделаны, но еще не  
    синхронизированы с базой данных.==
    
    ```Java
    try (Session session = factory.openSession()) {
                    session.beginTransaction();
                    User user1 = session.get(User.class, 3L);
                    user1.setRole(Role.USER); // USER не станет! 
                    session.refresh(user1); // так как здесь снова получаем ADMIN
                    session.getTransaction().commit();
                }
    ```
    
- `void flush()` ==используется для синхронизации изменений объектов с базой данных==. При вызове `flush()`, JPA или Hibernate ==выполняют все ожидающие операции записи== (INSERT, UPDATE, DELETE) в базу данных, чтобы применить внесенные изменения. Все изменения, внесенные в контексте сохранения, будут фактически записаны в базу данных.  
      
    `flush()` ==не завершает текущую транзакцию и не закрывает контекст сохранения.== Он только форсирует выполнение операций записи в базу данных. Часто, `flush()` ==вызывается автоматически перед выполнением запросов или перед завершением транзакции.==  
    Также можно задать FlushMode у сессии (AUTO или COMMIT)  
    
    ```Java
    session.setFlushMode(FlushModeType.COMMIT);
    ```
    

  

- `<T> T merge(T object)`; `<T> T merge(String entityName, T object)`

Оба метода `merge` ==выполняют операцию обновления объекта в контексте JPA.== Объекты, обновленные с использованием метода `merge`, становятся управляемыми (persistent) и будут автоматически синхронизироваться с базой данных при фиксации транзакции или при наступлении других событий синхронизации.

Важно отметить, что метод `merge` может использоваться для работы с объектами, находящимися в различных состояниях, таких как новый (Transient), отсоединенный (Detached) или удаленный (Removed). Он позволяет слияние изменений из переданного объекта с объектом, находящимся в контексте сохранения, и обновление соответствующих данных в базе данных.

Метод merge() возвращает результат – обновленный объект. Этот объект имеет состояние Persist и присоединен к объекту session. Объект передаваемый в метод merge() при этом не меняется.

```Java
User user = new User();
user.setName("Колян");
session.save(user);

session.evict(user);     // отсоединяем объект от сессии
user.setName("Маша");

User user2 = (User) session.merge(user);
```

Может показаться, что между user и user2 нет разницы, но это не так. В метод merge() можно передать POJO объект, а в качестве результата метод может вернуть proxy (зависит от настроек Hibernate). Поэтому просто запомни, что метод merge() не меняет передаваемый объект.

Во-вторых, если объект передаваемый в merge() имеет статус Transient (и у него нет ID), то для него создастся отдельная строчка в базе данных. Другими словами будет выполнена команда persist().

В-третьих, если в метод merge() передать объект уже присоединенный к сессии (со статусом Persist), то ничего не произойдет – метод просто вернет этот же объект. Почему? А все потому, что при коммите транзакции данные и так запишутся в базу:

```Java
Configuration configuration = new Configuration().configure();
        configuration.addAttributeConverter(DeathdayConverter.class, true);
        configuration.addAttributeConverter(BirthdayConverter.class, true);
        configuration.addAnnotatedClass(User.class);
        configuration.setPhysicalNamingStrategy(new CamelCaseToUnderscoresNamingStrategy());
        User user = User.builder()
                .info(PersonInfo.builder()
                        .userName("Vredina")
                        .firstName("Dmitriy")
                        .lastName("Parfenov")
                        .birthDay(new Birthday(LocalDate.now()))
                        .deathDay(new Deathday(LocalDate.now(), LocalDate.now().plusDays(10)))
                        .build())
                .role(Role.ADMIN)
                .recipe(new Recipe("cancer", "lie in bed"))
                .build();
        try (SessionFactory factory = configuration.buildSessionFactory()) {
            try (Session session = factory.openSession()) {
                session.beginTransaction();
                session.persist(user); //передали сущность в Context
                session.flush(); // добавили в БД
                session.evict(user); // удалили из контекста
                session.getTransaction().commit();
            }
            try (Session session = factory.openSession()) {
                session.beginTransaction();
                user.setRole(Role.USER); // установили user новую роль
                User userPrepareToMerge = session.get(User.class, 1L); // добавили сущность в контекст
                user.setRecipe(userPrepareToMerge.getRecipe());
                session.getTransaction().commit();
            }
            try (Session session = factory.openSession()) {
                session.beginTransaction();
                User mergedUser = session.merge(user); // здесь мержим изменения сделанные в другом контексте
                session.getTransaction().commit();
            }
        }
    }
```

### Detached

Объект ==становится отсоединенным== (detached), когда текущий контекст сохранения (Persistence Context) закрывается. ==Любые изменения, внесенные в detached объекты, больше автоматически не применяются к базе данных==.

- `void evict(Object object)`
- или `void detach(Object object)`  
    Эти методы удаляют экземпляр из PersistentContext( Persistent→Detached). Изменения в экземпляре не будут синхронизироваться с базой данных.  
    

```Java
try (Session session = factory.openSession()) {
                session.beginTransaction();
                User userPrepareToMerge = session.get(User.class, 1L);
                session.evict(userPrepareToMerge);
                userPrepareToMerge.setRole(Role.ADMIN); // АДМИНОМ НЕ БУДЕТ, так как объекта уже нет в PersistentContext
                session.getTransaction().commit();
            }
```

### Removed

- `void remove(Object object)` используется для удаления объекта из базы данных. Вот пример использования метода `remove.` ==ВАЖНО==: объект должен находиться в состоянии Persistent

```Java
session.beginTransaction();
User preparedToRemove = session.get(User.class, 1L);
session.remove(preparedToRemove);
session.getTransaction().commit();
```

### Identifiers in Hibernate/JPA

`Идентификаторы` в Hibernate ==представляют собой первичный ключ сущности. Это подразумевает уникальность значений, позволяющую идентифицировать конкретную сущность, отсутствие null и невозможность их изменения.==

Hibernate предоставляет несколько различных способов определения идентификаторов.

### Простой идентификатор @Id

C помощью данной аннотации происходит маппинг идентификаторы в примтивные типы или типы-обертки

```Java
@Entity
public class Student {

    @Id
    private long studentId;
    
    // standard constructor, getters, setters
}
```

### Генерируемые идентификаторы

==Если мы хотим автоматически генерировать значение первичного ключа, мы можем добавить аннотацию== `@GeneratedValue.`

При этом могут использоваться четыре типа генерации: `AUTO, IDENTITY, SEQUENCE и TABLE` (через GenerationType)

Если мы не указываем значение явно, то ==по умолчанию используется== тип генерации `AUTO`.

### Auto generation

Если мы используем генерацию по умолчанию, то значение будет определено на основе типа атрибута первичного числа ( числовое значение или UUID).

Для числовых значений генерация осуществляется на основе генератора последовательности(sequence) или таблицы(table), а для UUID-значений будет использоваться UUIDGenerator.

```Java
@Entity
public class Student {

    @Id
    @GeneratedValue
    private long studentId;

    // ...
}
```

### Identity generation

Самый часто испоользуемый тип генерации, ==auto increment.==

```Java
@Entity
public class Student {

    @Id
    @GeneratedValue (strategy = GenerationType.IDENTITY)
    private long studentId;

    // ...
}
```

### Sequence generation

Общее ==выражение создания sequence:==

```SQL
CREATE [ { TEMPORARY | TEMP } | UNLOGGED ] SEQUENCE [ IF NOT EXISTS ] name
    [ AS data_type ]
    [ INCREMENT [ BY ] increment ]
    [ MINVALUE minvalue | NO MINVALUE ] [ MAXVALUE maxvalue | NO MAXVALUE ]
    [ START [ WITH ] start ] [ CACHE cache ] [ [ NO ] CYCLE ]
    [ OWNED BY { table_name.column_name | NONE } ]
```

Привязываем его к таблице (несколько способов):

- до создания таблицы:
    
    ```SQL
    CREATE SEQUENCE IF NOT EXISTS users_custom_id_seq
    INCREMENT 1;
    ```
    
    ```SQL
    //
    CREATE TABLE IF NOT EXISTS users
    (
    id int default nextval('users_custom_id_seq') primary key,
    user_name VARCHAR(128);
    ```
    
- после создания таблицы:
    
    ```SQL
    CREATE SEQUENCE IF NOT EXISTS users_custom_id_seq
        INCREMENT 1
        owned by users.id;
    ```
    

После этого уже указываем его с помощью GeneratedType.SEQUENCE:

```Java
@Id
    @GeneratedValue(generator = "seqGen"
            ,strategy = GenerationType.SEQUENCE)
    @SequenceGenerator(name ="seqGen",
            sequenceName = "users_custom_id_seq", allocationSize = 1)
    private Long id;
```

### Table generation

```Java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, 
      generator = "table-generator")
    @TableGenerator(name = "table-generator", 
      table = "dep_ids", 
      pkColumnName = "seq_id", 
      valueColumnName = "seq_value")
    private long depId;

    // ...
}
```

### Custom generator

Создать кастомный генератор можно следующим образом:

- Создать стратегию имплентировать IdentifierGenerator, переопределить методы.
    
    ```Java
    public class CustomGenerationStrategy implements IdentifierGenerator {}
    ```
    
- Создать фунцкиональный интерфейс

```Java
@IdGeneratorType(CustomGenerationStrategy.class)
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD,ElementType.METHOD})
public @interface CustomGenerator{}
```

- Аннотировать id созданной аннотацией:
    
    ```Java
    @Id
    @CustomGenerator
    @GeneratedValue
    private Long id;
    ```
    

### Как было раньше в Hibernate 5

```Java
@Entity
public class Product {

    @Id
    @GeneratedValue(generator = "prod-generator")
    @GenericGenerator(name = "prod-generator", 
      parameters = @Parameter(name = "prefix", value = "prod"), 
      strategy = "com.baeldung.hibernate.pojo.generator.MyGenerator")
    private String prodId;

    // ...
}
```

```Java
public class MyGenerator 
  implements IdentifierGenerator, Configurable {}
```

### UuidGenerator

```Java
@IdGeneratorType(org.hibernate.id.uuid.UuidGenerator.class)
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD,ElementType.METHOD})
public @interface UuidGenerator {...}
```

```Java
@Id
@UuidGenerator(style = UuidGenerator.Style.Time)
@GeneratedValue
private UUid;
```

### Composite Identifiers

Помимо простых идентификаторов Hibernate позволяет ==определять составные идентификаторы.==

Составной идентификатор представляется классом первичного ключа с одним или несколькими постоянными атрибутами.

Класс первичного ключа `должен удовлетворять нескольким условиям:`

- Он должен ==быть определен с помощью аннотаций== `@EmbbededId` или `@IdClass`
- Он должен ==быть публичны и иметь публичный конструктор без аргументов== (в 5 еще и сериализуемый)
- ==Реализовыввать== `equals() и hashcode()`

Атрибуты класса могут быть базовыми, составными или ManyToOne, при этом следует избегать коллекций и атрибутов OneToOne.

### @EmbeddedId

```Java
@Embeddable
public class OrderEntryPK {

    private long orderId;
    private long productId;

    // standard constructor, getters, setters
    // equals() and hashCode() 
}

@Entity
public class OrderEntry {

    @EmbeddedId
    private OrderEntryPK entryId;

    // ...
}
```

### @IdClass

Схожа с аннотацией @EmbeddedId, но с уточнением в сущности, которая использует данную аннотацию над каждым полем, необходимо проставить @Id.

```Java
@Entity
@IdClass(OrderEntryPK.class)
public class OrderEntry {
    @Id
    private long orderId;
    @Id
    private long productId;
    
    // ...
}
```

  

### Mapping

### Hibernate 6 JSON and XML Mapping

Вам нужно только аннотировать свое поле в сущности с помощью аннотации @JdbcTypeCode и установить тип SqlTypes.JSON. Hibernate затем обнаруживает наличие библиотеки JSON в вашем classpath и использует ее для сериализации и десериализации.

Также необходимо внедрить зависимость `jackson-databind:`  
  
[https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind)

```XML
<dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.15.2</version>
        </dependency>
```

Entity:

```Java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
@Table(name = "users", schema = "public")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @JdbcTypeCode(SqlTypes.JSON)
    private Recipe recipe;
}
```

```Java
public static void main(String[] args) {

        Configuration configuration = new Configuration().configure();
        configuration.addAnnotatedClass(User.class);

        try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession()) {
            session.beginTransaction();
            User user = User.builder()
                    .recipe(new Recipe("cancer", "lie in bed"))
    
                    .build();
            session.persist(user);
            session.getTransaction().commit();
        }
    }
```

![[images/Untitled 10 8.png|Untitled 10 8.png]]

Если вы не хотите использовать стандартное отображение Hibernate, вы все равно можете реализовать пользовательский UserType. Это может быть полезно, если вы ранее использовали UserType, и его отображение JSON не соответствует стандартному отображению Hibernate.

  

Нечто похожее Hibernate может проделать и с XML:

```Java
@JdbcTypeCode(SqlTypes.SQLXML)
private Map<String, String> properties;
```

  

### Хранение файлов и картинок в БД

Иногда в базу данных нужно сохранить бинарные объекты. Например,  
файлы. Если файл большой, то разумнее всего хранить его в отдельной  
папке на диске, а в базе данных хранить его пути. Пример:  

```Plain
c:\db-files\users\12355\avatar.jpg
```

И в базе храним просто относительный путь к файлу:

```Plain
\12355\avatar.jpg
```

В базе удобно хранить относительный путь, так как из него потом легко получить URL:

```Plain
https://storage.javarush.ru/users/12355/avatar.jpg
```

Мы просто приклеиваем относительный путь к имени сервера и готово.

  

- Храним картинки прямо в базе

Однако, если картинки маленькие, их можно хранить прямо в базе и  
отдавать клиенту как набор байт. Для таких случаем в SQL есть  
специальный тип данных  
**BLOB** – Binary Large Object. Вернее, их даже два:

- **CLOB** – Character Large Object,
- **BLOB** – Binary Large Object.

CLOB используется для хранения очень больших текстов. А BLOB для  
хранения бинарных данных, таких как небольшие картинки, видео и тому  
подобное.  

Пример:

```Java
@Entity
@Table(name="user")
public class User {

    @Id
    private String id;

	@Column(name = "name", columnDefinition="VARCHAR(128)")private String name;

	@Lob
@Column(name = "photo", columnDefinition="BLOB")private byte[] photo;

	// ...
}
```

Аннотация `@Lob` подсказывает Hibernate, что в поле хранится **Large Object**. А `columnDefinition="BLOB"` уже говорит о том, как это все сохранить в базе.

Давай напишем пример кода, который сохраняет в базу нового пользователя и его фото:

```Java
byte[] imageBuffer = Files.readAllBytes(Paths.get("C:/temp/avatar.png"))

User user = new User();
user.setId("1");
user.setName("Zapp");
user.setPhoto(imageBuffer);

session.persist(user);
```

### AttributeConverter

В Java Persistence API (JPA) AttributeConverter - это интерфейс, который позволяет преобразовывать типы данных при сохранении и извлечении их из базы данных. Он предоставляет возможность настройки пользовательских преобразований типов, которые не являются стандартными для JPA.

В приведенном ниже примере `BirthdayConverter` реализует интерфейс `AttributeConverter` для преобразования типа `Birthday` в `Date` при сохранении в базу данных и обратно при извлечении из базы данных.

```Java
public record Birthday(LocalDate birthDate) {
  public long getAge() {
    return ChronoUnit.YEARS.between(birthDate, LocalDate.now());
  }
}

@Converter
public class BirthdayConverter implements AttributeConverter<Birthday, Date> {

  @Override
  public Date convertToDatabaseColumn(Birthday attribute) {
    return Optional.ofNullable(attribute)
        .map(Birthday::birthDate)
        .map(Date::valueOf)
        .orElse(null);
  }

  @Override
  public Birthday convertToEntityAttribute(Date dbData) {
    return Optional.ofNullable(dbData)
        .map(Date::toLocalDate)
        .map(Birthday::new)
        .orElse(null);
  }
}

@Entity
public class User {
	@Convert(converter = BirthdayConverter.class)
  private Birthday birthday;
}
```

### @Convert и @Converter

- `@Convert` - это аннотация, которая применяется напрямую к  
    атрибуту (полю) и указывает JPA, какой конвертер должен использоваться  
    для преобразования значения этого атрибута при сохранении и извлечении  
    из базы данных.  
    
- `@Converter` требует указания только класса конвертера.

Чтобы над каждым полем не прописывать  
  
`==@Convert(==``converter = T.class``==)==` можно сообщить Hibernate, чтобы он делал это всегда самостоятельно, когда сталкивался с экземпляром класса `T`, требующего конвертации. Это можно сделать двумя способами:

- С помощью `@Converter(autoApply = true).` При использовании аннотации `@Converter` без указания значения `autoApply`, значение по умолчанию для `autoApply` будет `false`.

```Java

@Converter(autoApply=true)
public class BirthdayConverter implements AttributeConverter<Birthday, Date> {
...
}
```

- Программно через метод конфигурации  
      
    `public Configuration addAttributeConverter(AttributeConverter<?, ?> attributeConverter, boolean autoApply)`, в данном случае после аннотации ==@Converter== не нужно прописывать параметр.

```Java
Configuration configuration = new Configuration();
    configuration.configure();
    configuration.addAnnotatedClass(Test.class);
    configuration.addAttributeConverter(new BirthdayConverter(), true);
```

### Mapping Entity Assosiations

### FetchType

`FetchType` - это ==параметр== аннотации `@ManyToOne` (или других аннотаций, таких как `@OneToOne` или `@OneToMany`) в объектно-реляционном отображении (ORM). Он ==определяет стратегию извлечения связанной сущности из базы данных==.

```Java
public enum FetchType {

    /** Defines that data can be lazily fetched. */
    LAZY,

    /** Defines that data must be eagerly fetched. */
    EAGER
}
```

- Когда используется `FetchType.EAGER` (Eager по умолчанию используется), связанная сущность ==будет извлекаться из базы данных немедленно при извлечении основной сущности==. Это означает, что при загрузке объекта, содержащего аннотацию @ManyToOne с FetchType.EAGER, связанная сущность будет загружена автоматически, без необходимости явного обращения к ней.
    
    ```SQL
    @ManyToOne(fetch = FetchType.EAGER)
        @JoinColumn(name = "company_id")
        private Company company;
    ```
    
    ```Java
    session.beginTransaction();
                User user1 = session.get(User.class, 3L);
                Company company1 = user1.getCompany();
                System.out.println(company1.toString());
                session.getTransaction().commit();
    ```
    
    ```Java
    Hibernate: 
        select
            u1_0.id,
            c1_0.id,
            c1_0.name,
            u1_0.user_name,
    
        from
            public.users u1_0 
        left join
            company c1_0 
                on c1_0.id=u1_0.company_id 
        where
            u1_0.id=
    ```
    
- Когда используется `FetchType.LAZY`, ==связанная сущность будет извлекаться из базы данных только в том случае, если она действительно нужна, т.е. когда к ней происходит обращение в коде.== В противном случае, если связанная сущность не требуется, она не будет извлекаться, и это позволяет избежать лишних запросов к базе данных. Использование FetchType.LAZY полезно в случаях, когда связанные сущности обычно не нужны в большинстве операций или если их загрузка требует большого количества ресурсов. Это помогает оптимизировать производительность при работе с базой данных и уменьшить время загрузки объектов.
    
    ```Java
    @ManyToOne(fetch = FetchType.LAZY)
        @JoinColumn(name = "company_id")
        private Company company;
    ```
    

При выбранном fetch = FetchType.LAZY, при получении User, запрос к таблице к company не будет выполнен. Будет выполен не на `3`, а на `4` строке!.

```Java
1			 			session.beginTransaction();
2            User user1 = session.get(User.class, 3L);
3            Company company1 = user1.getCompany();
4            System.out.println(company1.toString());
5            session.getTransaction().commit();
```

```SQL
Hibernate: 
    select
        u1_0.id,
        u1_0.company_id,
        u1_0.user_name,

    from
        public.users u1_0 // нет JOIN!!!
    where
        u1_0.id=?
//Во время выполнения кода в 4 строке
Hibernate: 
    select
        c1_0.id,
        c1_0.name 
    from
        company c1_0 
    where
        c1_0.id=?
Company(id=1, name=Google)
```

  

### LazyInitializationException

`LazyInitializationException` возникает в контексте Hibernate, фреймворка для работы с базами данных в Java. ==Ошибка возникает, когда происходит попытка доступа к ассоциированным сущностями объектами, которые не были инициализированы, и== ==сессия Hibernate уже закрыта== ==или неактивна.==

Hibernate поддерживает ленивую инициализацию ассоциаций, что означает, что ассоциированные объекты не загружаются из базы данных автоматически при загрузке родительской сущности, чтобы избежать избыточных запросов к базе данных. Вместо этого, объекты ассоциаций загружаются только при доступе к ним впервые.

==В данном случае ошибки не возникнет== так как сессия еще не закрыта, и мы подтягиваем список User

```Java

@Entity
@NoArgsConstructor
@AllArgsConstructor
@Data
@ToString(exclude = "users")
@EqualsAndHashCode(exclude = "users")
@Builder
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    @Builder.Default
    @OneToMany(mappedBy = "company", fetch = FetchType.LAZY)
    private Set<User> users = new HashSet<>();

}

try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession()) {
            session.beginTransaction();
            Company company = session.get(Company.class, 1L);
            session.getTransaction().commit();
            company.getUsers().forEach(System.out::println);
        }
```

==Вот пример, демонстрирующий, как может возникнуть== `LazyInitializationException`. Сессия закрылась, а сущность company была лениво подтянута.

```Java



try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession()) {
            session.beginTransaction();

            Company company = session.get(Company.class, 1L);
            
            session.getTransaction().commit();

            //company.getUsers().forEach(System.out::println);
            
        }

   company.getUsers().forEach(System.out::println); 
```

В данном примере после завершения транзакции и закрытия сессии `em.close()`, мы пытаемся получить доступ к детям родительской сущности `parent.getChildren()`, но сессия уже не активна, и ассоциированные объекты `children` не могут быть лениво инициализированы, что приводит к возникновению `LazyInitializationException`.

==Чтобы избежать данной ошибки, вы можете:==

1. ==Использовать жадную инициализацию== (`FetchType.EAGER`) вместо ленивой инициализации для ассоциаций, если это безопасно с точки зрения производительности и не приводит к избыточным запросам к базе данных.
2. Убедиться, что вы ==обращаетесь к ассоциированным объектам до закрытия сессии или внутри активной транзакции.==
3. ==Переоткрыть сессию== (или воспользоваться `EntityGraph`), чтобы инициализировать ассоциации перед доступом к ним, если это необходимо.

Важно понимать, что ленивая инициализация обеспечивает оптимизацию производительности, поскольку загружает только те данные, которые действительно нужны, но требует аккуратного управления состоянием сессии и доступом к ассоциированным объектам.

### @ManyToOne

```Java
public @interface ManyToOne {
    Class targetEntity() default void.class;
    CascadeType[] cascade() default {};
    FetchType fetch() default FetchType.EAGER;
    boolean optional() default true;
}
```

`@ManyToOne` - это аннотация, используемая в объектно-реляционном отображении (ORM), ==чтобы указать связь "многие-к-одному" между двумя сущностями.==

Пример:

Создание таблиц:

```SQL
CREATE TABLE company (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(32)
);

CREATE TABLE IF NOT EXISTS users
(
id BIGSERIAL PRIMARY KEY,
user_name VARCHAR(128),
company_id INT REFERENCES company(id),
);
```

Аннотация

```Java

@Entity
@Table(name = "users", schema = "public")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
		
		private String userName;

		@ManyToOne
    @JoinColumn(name = "company_id")
    private Company company;
}

@Entity
// Table не обязательна, так как бд называлась company (также)
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;
}
```

При выполнении запроса пол получению Usera будет выполнен следующий sql запрос (Выполняется LEFT JOIN):

```SQL
select
        u1_0.id,
        c1_0.id,
        c1_0.name,
        u1_0.user_name,
    from
        public.users u1_0 
    left join
        company c1_0 
            on c1_0.id=u1_0.company_id 
    where
        u1_0.id=?
```

  

### CascadeType

Каскадная настройка `@ManyToOne( cascade = CascadeType.’SomeType’)` определяет, какие операции должны распространяться с родительской сущностью на связанные дочерние сущности при выполнении определенных операций с родительской сущностью.

ВАЖНО: Чтобы избежать циклического вызова метода toString() и получить ошибку StackOverFlow - необходимо прописывать сущности в аннотацию Lombok `@ToString(exclude = {"company , profile""})` чтобы исключить их.

```Java
public enum CascadeType { 

    /** Cascade all operations */
    ALL, 

    /** Cascade persist operation */
    PERSIST, 

    /** Cascade merge operation */
    MERGE, 

    /** Cascade remove operation */
    REMOVE,

    /** Cascade refresh operation */
    REFRESH,

    /**
     * Cascade detach operation
     *
     * @since 2.0
     * 
     */   
    DETACH
}
```

==ВАЖНО!== ==**Нужно быть осторожным с каскадом, так как можно удалить последовательно все данные из разных таблиц.**==

Допустим есть две сущности User и его поле

```Java
@ManyToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "company_id")
    private Company company;
```

И сущность company и ее поле:

```Java
@Builder.Default
    @OneToMany(mappedBy = "company",cascade = CascadeType.ALL)
    private Set<User> users = new HashSet<>();
```

Удалив user он каскадно удалит компанию, а компания каскадно удалит всех юзеров. В итоге бд обнулятся.

### @OneToMany

### boolean orphanRemoval

Это переменная есть в отношениях `@OneToMany` и `@OneToOne`, в `@ManyToOne` ее нет.

Не путать с `CascadeType.REMOVE`. ==Ремув удаляет все связанные сущности с родительской сущностью.==

`orphanRemoval` - атрибут, который контролирует удаление "заброшенных" (orphans) элементов коллекции при обновлении родительской сущности. "Заброшенные" элементы - ==это те, которые больше не ассоциируются с родительской сущностью, и их необходимо удалить из базы данных.==

Для лучшего понимания, предположим, у нас есть две сущности - `ParentEntity` и `ChildEntity`, связанные отношением "один-ко-многим" (`@OneToMany`). Каждая родительская сущность может иметь несколько дочерних сущностей.

```Java
@Entity
public class ParentEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<ChildEntity> children;

    // Геттеры и сеттеры
}

@Entity
public class ChildEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private ParentEntity parent;

    // Геттеры и сеттеры
}
```

Здесь важным атрибутом для нас является `orphanRemoval = true`. Когда мы устанавливаем этот атрибут в `true`, то при обновлении родительской сущности и удалении каких-либо дочерних сущностей из коллекции `children`, эти дочерние сущности будут также удалены из базы данных. Если же атрибут установлен в `false`, удаление дочерних сущностей не будет автоматически обрабатываться.

Пример использования:

```Java
// Создаем родительскую сущность
ParentEntity parent = new ParentEntity();

// Создаем и добавляем дочерние сущности
ChildEntity child1 = new ChildEntity();
child1.setParent(parent);

ChildEntity child2 = new ChildEntity();
child2.setParent(parent);

parent.getChildren().add(child1);
parent.getChildren().add(child2);

// Сохраняем родительскую сущность и все дочерние сущности
entityManager.persist(parent);
```

При обновлении родительской сущности, если мы решим удалить одну из дочерних сущностей, например, `parent.getChildren().remove(child1);`, атрибут `orphanRemoval = true` приведет к удалению сущности `child1` из базы данных. Если бы атрибут был установлен в `false`, сущность `child1` не была бы удалена при обновлении родительской сущности.

  

  

Один ко многим ( допустим одна компания, в которой работает несколько сотрудников)

Настроить данную связь можно несколькими способами:

- Первый способ через @JoinColumn
    
    ```Java
    		@OneToMany(fetch = FetchType.LAZY)
        @JoinColumn(name = "company_id")
        private List<User> users;
    ```
    
- Второй способ, если присутствует в дочернем классе @ManyToOne, через аргумент `mappedBy`
    
    ```Java
    	@Entity
    public class Company {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Integer id;
    
        private String name;
    		
        @OneToMany(mappedBy = "company")
        private List<User> users;
    }
    
    
    @Entity
    public class User {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @ManyToOne(fetch = FetchType.EAGER, optional = false)
    		@JoinColumn(name = "company_id")
        private Company company;
    ```
    
    ```Java
    Hibernate: 
        select
            c1_0.id,
            c1_0.name 
        from
            company c1_0 
        where
            c1_0.id=?
    Hibernate: 
        select
            u1_0.company_id,
            u1_0.id,
            u1_0.birth_date,
            u1_0.death_date,
            u1_0.first_name,
            u1_0.last_name,
            u1_0.user_name,
            u1_0.recipe,
            u1_0.role 
        from
            public.users u1_0 
        where
            u1_0.company_id=?
    Hibernate: 
        insert 
        into
            public.users
            (company_id,birth_date,death_date,first_name,last_name,user_name,recipe,role) 
        values
            (?,?,?,?,?,?,?,?)
    Hibernate: 
        update
            public.users 
        set
            company_id=? 
        where
            id=?
    ```
    
    ```Java
    select
            c1_0.id,
            c1_0.name 
        from
            company c1_0 
        where
            c1_0.id=?
    Hibernate: 
        select
            u1_0.company_id,
            u1_0.id,
            u1_0.birth_date,
            u1_0.death_date,
            u1_0.first_name,
            u1_0.last_name,
            u1_0.user_name,
            u1_0.recipe,
            u1_0.role 
        from
            public.users u1_0 
        where
            u1_0.company_id=?
    Hibernate: 
        update
            public.users 
        set
            company_id=? 
        where
            id=?
    ```
    

### @OneToOne

### Без синтетически созданного ключа с использованием @PrimaryKeyJoinColumn

В данном примере ссылкой в отношении @OneToOne между Profile и User является Primary key класса User

```SQL
CREATE TABLE profile
(
    user_id BIGINT  PRIMARY KEY REFERENCES users(id),
    street VARCHAR(128)
);
```

Класс `User`:

```Java
import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;

    @OneToOne(mappedBy ="user", cascade = CascadeType.ALL)
    private Profile profile;

    // Конструкторы, геттеры и сеттеры, другие методы
}
```

Класс `Profile`:

```Java
@Entity
@Table(name = "profiles")
public class Profile {
    @Id
    private Long id; // Обратите внимание, что это поле не генерируется автоматически

    private String fullName;
    private int age;

    @OneToOne
		@PrimaryKeyJoinColumn
    private User user;

		public void setUser(User user){
        user.setProfile(this);
        this.user = user;
        this.id = user.getId();
    }
    // Конструкторы, геттеры и сеттеры, другие методы
}
```

`@PrimaryKeyJoinColumn` указывает, что `id` из `User` будет использоваться в качестве внешнего ключа для связи с `Profile`.

Этот способ считается менее предпочтительным, так как лучше использовать `GeneratedType.IDENTITY`

### Предпочтительный способ с синтетически созданным ключом

ВАЖНО!: Лучше не использовать в @OneToOne biderectional связь, либо отказаться в profile от синтетически генерируемого ключа, иначе не получится создать Lazy подтягивание сущностей.

Обязательно необходимо указать NOT NULL и UNIQUE, иначе это будет `@ManyToOne`, а не `@OneToOne`.

Теперь мы можем до сохранения установить в User Profile. А он автоматически каскадом подтянется после сохранения User`a

```SQL
CREATE TABLE profile
(
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL UNIQUE REFERENCES users(id),
    street VARCHAR(128)
);
```

Класс `User`:

```Java
import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;

    @OneToOne(mappedBy ="user", cascade = CascadeType.ALL)
    private Profile profile;

    // Конструкторы, геттеры и сеттеры, другие методы
}
```

Класс `Profile`:

```Java
@Entity
@Table(name = "profiles")
public class Profile {
    @Id
		@GeneratedValue(strategy = GeneratedType.IDENTITY)
    private Long id; //  поле генерируется автоматически

    private String fullName;
    private int age;

    @OneToOne
		@JoinColumn(name = "user_id")
    private User user;

		public void setUser(User user){
        user.setProfile(this);
        this.user = user;
    }
    // Конструкторы, геттеры и сеттеры, другие методы
}
```

### @ManyToMany

==Важно: При данном отношении необходимо создавать соответствие на уровне Java модели, не только на уровне БД аннотациями. То есть создать сеттер как в примере. Также можно сделать аналогичный сеттер в классе Chat, который добавляет экземпляр User.==

```SQL
CREATE TABLE IF NOT EXISTS users
(
id BIGSERIAL primary key,
user_name VARCHAR(128),
first_name VARCHAR(128),
last_name VARCHAR(128),
birth_date DATE,
death_date DATE,
company_id INT REFERENCES company(id),
role VARCHAR(32),
recipe JSONB
);

CREATE TABLE chat
(
    id BIGSERIAL PRIMARY KEY ,
    name VARCHAR(65) NOT NULL  UNIQUE
);

CREATE TABLE  users_chat
(
    user_id BIGINT REFERENCES users(id),
    chat_id BIGINT REFERENCES chat(id),
    PRIMARY KEY (user_id,chat_id)
)
```

Также необходимо быть осторожным с каскадным удалением и исключать в ToString сущности, чтобы не было StackOverFlow.

```SQL
@Entity
@Table(name = "users", schema = "public")
public class User {
		@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

@Builder.Default
    @ManyToMany
    @JoinTable(name = "users_chat",
                joinColumns = @JoinColumn(name = "user_id"),
                inverseJoinColumns = @JoinColumn(name ="chat_id"))
    private Set<Chat> chats = new HashSet<>();

    public void addChat(Chat chat){
        chats.add(chat);
        chat.getUsers().add(this);
    }
}
```

```Java
@Entity
@Table(name = "chat")
@EqualsAndHashCode(of = "name")
@ToString(exclude = "users")
public class Chat {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Basic
    @Column(nullable = false, unique = true)
    private String name;

    @Builder.Default
    @ManyToMany(mappedBy = "chats")
    private Set<User> users = new HashSet<>();
}
```

Аннотация @JoinTable - это одна из аннотаций, используемых в Hibernate, чтобы настроить отношения между сущностями базы данных. В данном примере аннотация применяется к полю `chats` класса, чтобы установить связь "многие ко многим" между сущностями `User` и `Chat`.

1. `@JoinTable`: Эта аннотация указывает Hibernate, что должна быть создана промежуточная таблица (таблица-соединение) для установления связи между сущностями `User` и `Chat`. Промежуточная таблица используется для отображения отношения "многие ко многим" между этими двумя сущностями.
2. `name`: Это атрибут аннотации `@JoinTable`, который определяет имя промежуточной таблицы, которая будет создана в базе данных. В данном примере промежуточная таблица будет называться "users_chat".
3. `joinColumns`: Этот атрибут определяет столбец, который будет использоваться для связывания сущности `User` с промежуточной таблицей. В данном случае мы используем столбец "user_id", который будет хранить идентификаторы пользователей.
4. `inverseJoinColumns`: Этот атрибут определяет столбец, который будет использоваться для связывания сущности `Chat` с промежуточной таблицей. В данном случае мы используем столбец "chat_id", который будет хранить идентификаторы чатов.

Таким образом, данная аннотация определяет связь "многие ко многим" между сущностями `User` и `Chat`, где каждый пользователь может принадлежать к нескольким чатам, и каждый чат может включать несколько пользователей. Промежуточная таблица "users_chat" будет отображать это отношение, содержа соответствующие идентификаторы пользователей и чатов для установления связей между ними.

### @ElementCollection

`@ElementCollection` - это аннотация, которая используется в Hibernate (и JPA - Java Persistence API) для отображения коллекций простых значений (не сущностей) в базе данных. Эта аннотация позволяет сохранять элементы коллекции в отдельной таблице, которая связана с основной сущностью, вместо того чтобы создавать дополнительные сущности для каждого элемента коллекции.

```Java
package org.example.entity;

import jakarta.persistence.Embeddable;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor(staticName = "of")
@Embeddable
public class LocaleInfo {
    private String lang;
    private String description;
}

@Entity
@NoArgsConstructor
@AllArgsConstructor
@Data
@ToString(exclude = "users")
@EqualsAndHashCode(exclude = "users")
@Builder
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    @Builder.Default
    @ElementCollection
    @CollectionTable(name = "company_locale",joinColumns = @JoinColumn(name = "company_id"))
    @AttributeOverride(name = "lang", column = @Column(name="language"))
    private List<LocaleInfo> locales = new ArrayList<>();
```

Основное применение `@ElementCollection` состоит в том, чтобы сохранять и управлять коллекциями значений, такими как строки, числа, даты и другие примитивные типы данных, а также их классы-обертки, в базе данных. Например, список телефонных номеров, адресов или электронных адресов пользователя можно хранить в отдельной таблице, связанной с сущностью пользователя.

Преимущества использования `@ElementCollection`:

1. **Упрощение структуры данных**: Если коллекция состоит только из простых значений и не имеет своих собственных атрибутов или отношений с другими сущностями, использование `@ElementCollection` позволяет избежать создания дополнительных сущностей и упрощает структуру данных.
2. **Управление зависимостью**: Когда элементы коллекции тесно связаны с основной сущностью и их существование зависит от сущности, использование `@ElementCollection` обеспечивает управление зависимостью между элементами коллекции и основной сущностью. Это означает, что элементы коллекции будут удалены, когда удаляется основная сущность.
3. **Улучшенная производительность**: В некоторых случаях, когда у вас есть большие коллекции простых значений, хранение их в отдельной таблице может улучшить производительность запросов и уменьшить размер основной таблицы сущности.

Однако стоит помнить, что использование `@ElementCollection` имеет свои ограничения. Например, вы не можете выполнять сложные запросы напрямую к элементам коллекции через JPQL или Criteria API. Если ваша коллекция имеет свои собственные атрибуты или отношения, то может быть лучше использовать отдельную сущность для этой коллекции. Как всегда, выбор между `@ElementCollection` и созданием отдельной сущности зависит от требований вашего приложения и структуры данных.

### Collections ordering

Для сортировки коллекций можно использовать аннотацию @OrderBy. Данная аннотация присутсвует как и в Hibernate, так и в JPA.

- `org.hibernate.annotations.OrderBy()` предпочтитетельно использовать sql. В данном случае сортируется по соответствующим колонкам.
    
    ```Java
    @Builder.Default
        @OneToMany(mappedBy = "user")
        @org.hibernate.annotations.OrderBy(clause = "user_name DESC, last_name ASC")
        private Set<UserChat> userChats = new HashSet<>();
    ```
    
- `jakarta.persistence.OrderBy()` используется HQL, то есть не колонки БД как в предыдущем примере, а поля сущности
    
    ```Java
    @Entity
    public class Customer {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String name;
    
        @OneToMany(mappedBy = "customer")
        @OrderBy("orderDate DESC") // Указываем порядок сортировки по полю orderDate в обратном порядке
        private List<Order> orders;
    
        // Геттеры, сеттеры и другие поля класса
    }
    ```
    
    1. В примере выше мы применили аннотацию `@OrderBy("orderDate DESC")`, чтобы указать сортировку элементов в поле `orders`. Здесь `orderDate` — это поле в классе `Order`, которое мы хотим использовать для сортировки, а `DESC` означает сортировку по убыванию (от самой поздней даты к более ранним).
    2. Убедитесь, что в классе `Order` у вас есть соответствующее поле `orderDate`, и этот класс также помечен как сущность (Entity) с аннотацией `@Entity`.
    3. После сохранения данных в базу данных, при получении объектов класса `Customer` из базы данных, их коллекция `orders` будет автоматически отсортирована по указанному полю и порядку.
    
    Важно отметить, что `@OrderBy` работает только с коллекциями, хранящимися в виде списка (`List`) или множества (`Set`) в сущности. Если вы хотите осуществить сложную или динамическую сортировку, вам, возможно, потребуется использовать другие способы, такие как использование `@OrderBy` вместе с `@OrderColumn` или программное сортирование после получения данных из базы данных.
    
      
    
- `@OrderColumn` - еще одна аннотация, которая позволяет отсортировать по колонке в БД. Можно использовать только с List. Нечасто используется, так как в качестве аргумента надо передать колонку с Integer. А также, если отсуствуют значения в порядке возрастания будут вставлены null.
    
    ```Java
    1 2 3 null 4 5 etc
    ```
    

### Cортировка множеств

Если есть необходимость при сохранении сущности и дальнейшем ее хранении в отсортированном множестве можно использовать TreeSet (не забывать имплементировать Comparable у класса сортируемых сущностей)

```Java
@Entity
@Table(name = "users", schema = "public")
public class User {
@Builder.Default
    @OneToMany(mappedBy = "user")
    @OrderBy(value = "user.id")
    @OrderColumn(name = "id")
    private Set<UserChat> userChats = new TreeSet<>();
}
}
```

ВАЖНО: Это работает только при хранении в отсортированном порядке, если необходимо также получать множество в отсортированном порядке - необходимо добавить аннотацию @SortNatural илииспользовать SortedSet ( в Hiber эта реализация PersistentSortedSet) дополнительно указав `@SortNatural` или `@SortComparator`.

```Java
    @OneToMany(mappedBy = "user")
    @OrderBy(value = "user.id")
    @OrderColumn(name = "id")
		@SortNatural
    private SortedSet<UserChat> userChats = new TreeSet<>();
```

### Сортировка Map

Так как в Map ключ это уникальное значение необходимо также указать UNIQUE поле в качестве ключа, а также указать с помощью аннотации @MapKey, что является в качестве ключа в Map.

```Java
@OneToMany(mappedBy = "company", cascade = CascadeType.ALL)
@MapKey(name = "userName")
private Map<String, User> users = new HashMap<>();
```

@SortNatural используется не только со множествами, но и с Map. В примере ниже при вызове getAll данные будут сортироваться.

```Java
@OneToMany(mappedBy = "company", cascade = CascadeType.ALL)
@MapKey(name = "userName")
@SortNatural
private Map<String, User> users = new HashMap<>();
```

Если необходимо хранить в БД отсортированные данные HashMap меняем на TreeMap

### @MapKeyColumn

`@MapKeyColumn` — это аннотация из Java Persistence API (JPA), которая используется для маппинга отображения (`Map`) сущностей в базу данных. Она применяется к полю или свойству типа `Map`, чтобы определить столбец, который будет хранить ключи отображения в таблице базы данных.

Когда вы храните `Map` в базе данных, каждая запись в таблице будет представлять пару "ключ-значение". Аннотация `@MapKeyColumn` позволяет указать имя столбца, который будет хранить ключи отображения в этой таблице.

Вот пример использования `@MapKeyColumn`:

Предположим, у нас есть две сущности: `Department` и `Employee`, и между ними существует отношение один-ко-многим (one-to-many). Каждый отдел имеет множество сотрудников, и мы хотим хранить информацию о сотрудниках в виде отображения, используя их идентификаторы в качестве ключей.

1. Определим сущность `Department`, содержащую отображение сотрудников:

```Java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "department")
    @MapKeyColumn(name = "employee_id") // Указываем имя столбца для хранения ключей отображения
    private Map<Long, Employee> employees;

    // Геттеры, сеттеры и другие поля класса
}
```

1. Теперь определим сущность `Employee`:

```Java
@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // Геттеры, сеттеры и другие поля класса
}
```

1. В примере выше мы определили поле `employees` в сущности `Department` с аннотацией `@MapKeyColumn(name = "employee_id")`. Это означает, что в базе данных будет создан столбец с именем `employee_id`, в котором будут храниться идентификаторы сотрудников (ключи отображения).

Тепер, когда вы сохраняете объекты `Department` и связанные с ними `Employee`, Hibernate будет автоматически сохранять их в таблицы базы данных, используя отображение. Ключи отображения будут храниться в столбце `employee_id`.

В результате таблица для `Department` может выглядеть примерно так:

|   |   |
|---|---|
|id|name|
|1|HR|
|2|Marketing|

А таблица для `Employee` может выглядеть так:

|   |   |   |
|---|---|---|
|id|name|department_id|
|1|John Doe|1|
|2|Jane Smith|2|

Здесь `department_id` — это внешний ключ, связывающий таблицы `Department` и `Employee`. В то же время, ключи отображения для каждого отдела хранятся в столбце `employee_id` для каждой записи `Department`.

### Testing with Docker TestContainer

Для использования необходимо внедрить зависимость, в примере указана для postgresql

[https://mvnrepository.com/artifact/org.testcontainers](https://mvnrepository.com/artifact/org.testcontainers)

```XML
<!-- https://mvnrepository.com/artifact/org.testcontainers/postgresql -->
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <version>1.18.3</version>
    <scope>test</scope>
</dependency>
```

Также необходимо настроить hibernate.cfg.xml

```XML
<session-factory>
    <property name="connection.driver_class">org.postgresql.Driver</property>
    <property name="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</property>
    <property name="show_sql">false</property>
    <property name="format_sql">true</property>
    <property name="hibernate.id.new_generator_mappings">true</property>
    <property name="hibernate.show_sql">true</property>
    <property name="hibernate.hbm2ddl">validate</property>
    <!-- DB schema will be updated if needed -->
    <!-- <property name="hibernate.hbm2ddl.auto">update</property> -->
  </session-factory>
```

После этого создается утилитный класс, в котором инициализируется соотвествующий контейнер.

```Java
import org.testcontainers.containers.PostgreSQLContainer;

@UtilityClass
public class HibernateTestUtil {


    private static final PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15.3");

    static{
        postgres.start();
        postgres.close();
    }
    public static SessionFactory buildSessionFactory(){
        Configuration configuration = HibernateUtil.buildConfiguration();
        configuration.setProperty("hibernate.connection.url", postgres.getJdbcUrl());
        configuration.setProperty("hibernate.connection.username", postgres.getUsername());
        configuration.setProperty("hibernate.connection.password", postgres.getPassword());
        configuration.configure();

        return configuration.buildSessionFactory();
    }
}
```

Открытие сессии в тестах происходит через этот класс

```Java
@Test
    void checkDocker(){
        try(SessionFactory sessionFactory = HibernateTestUtil.buildSessionFactory();
        var session = sessionFactory.openSession()){
            session.beginTransaction();

            var com = Company.builder().name("Google").build();

            session.getTransaction().commit();
        }
    }
```

ВАЖНО: docker не должен запускаться через sudo. иначе не будет доступа для создания контейнера. Чтобы включить возможность запуска docker без sudo прописать команды ниже и перезагрузить компьютер

```Bash
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo service docker restart
```

### Mapping Inheritance

### @MappedSuperClass

Помечая класс как `@MappedSuperclass`, мы сообщаем JPA, что  
этот класс не должен быть сущностью (то есть не должен быть сохранен в  
базе данных напрямую), но его поля могут быть унаследованы другими  
сущностями. Это позволяет создавать общую базовую структуру для всех  
сущностей, которые будут иметь одинаковые поля  

```Java
import java.io.Serializable;
@MappedSuperclass
public abstract class BaseEntity<T extends Serializable> {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private T id;
}
```

```Java
@Entity
@Table(name = "users", schema = "public")
public class User  extends BaseEntity<Integer>{
...
}
```

Этот способ удобен, когда у сущностей наверняка есть автогенерируемые синтетические ключи, но бывает легаси код, где этих id нет. В таких случаях можно сделать BaseEntity не абстрактным классом, а интерфейсом. Этот вариант более гибкий.

```Java
import java.io.Serializable;

public interface BaseEntity<T extends Serializable> {

    void setId(T id);
    
    T getId();
}
```

Однако, если есть поля, которые необходимо все таки вынести в абстракцию можно сделать следующим образом:

```Java
@MappedSuperClass
public abstract class AuditableEntity<T extends Serializable> implements BaseEntity<T> {
    private Instant createdAt;
    
    private String createdBy;
    
}
```

И когда нужно наследоваться от AuditableEntity, а когда имплементировать BaseEnity и не быть привязанным к GeneratetionType.IDENTITY.

### @Inheritance(strategy = InheritanceType….)

![[images/Untitled 11 8.png|Untitled 11 8.png]]

- `InheritanceType.``_SINGLE_TABLE_`

В этой стратегии все подклассы и суперкласс отображаются в одну таблицу, которая содержит все атрибуты всех классов. В таблице используется дополнительное поле, которое указывается в родительской сущности через аннотацию `@DiscriminatorColumn(name = "type")`, в наследнике же прописывается `@DiscriminatorValue(value = "customView"),`которое указывает, что будет показываться в колонке type.

Преимущества стратегии "Single Table":

- Простота и эффективность запросов. Все данные хранятся в одной таблице, что обеспечивает быстрый доступ к данным без необходимости соединения таблиц.
- Отсутствие дублирования данных. Так как все атрибуты хранятся в одной таблице, нет необходимости дублировать общие данные между таблицами.

Недостатки стратегии "Single Table":

- При наличии множества подклассов может привести к таблице с большим числом NULL значений, если у различных подклассов разное количество атрибутов.
- При изменении структуры иерархии классов может потребоваться изменение схемы базы данных, что может быть проблематично в продакшн-среде.
    
    ```Java
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    @Entity
    @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    @DiscriminatorColumn(name = "type")
    @Table(name = "users")
    public abstract class User {
        
        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE)
        private Long id;
        private Long credentials;
    }
    
    @Entity
    @DiscriminatorValue(value = "programmer")
    public class Programmer extends User{
    
        @Enumerated(value = EnumType.STRING)
        private Language language;
    
        @Builder
        public Programmer(Long id, Long credentials, Language language) {
            super(id, credentials);
            this.language = language;
        }
    }
    
    @Entity
    @DiscriminatorValue(value = "manager")
    public class Manager extends User{
    
        private String projectName;
    
        @Builder
        public Manager(Long id, Long credentials, String projectName) {
            super(id, credentials);
            this.projectName = projectName;
        }
    }
    ```
    

![[images/Untitled 12 8.png|Untitled 12 8.png]]

- `InheritanceType.``_JOINED_` _join осуществляется через аннотацию_ `_@PrimaryKeyJoinColumn(name = "id")_`

В этой стратегии каждый подкласс имеет свою собственную таблицу, а также существует таблица для суперкласса, которая содержит общие атрибуты для всех подклассов. Это позволяет избежать дублирования данных, поскольку общие атрибуты хранятся только в одной таблице, и каждая из подтаблиц содержит только специфичные для своего класса атрибуты.

Преимущества стратегии "Joined":

- Четкое разделение данных между таблицами подклассов, что может уменьшить количество NULL значений и дублирования данных.
- Поддерживает легкое добавление новых подклассов без изменения схемы базы данных, поскольку для каждого нового подкласса будет создана отдельная таблица.

Недостатки стратегии "Joined":

- Более сложные запросы, требующие объединения таблиц при необходимости получения данных из разных подклассов.
- Несколько таблиц в базе данных могут привести к небольшому снижению производительности по сравнению с другими стратегиями.

```Java
@Entity
@Table(name = "users")
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class User {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Long id;
    private Long credentials;
}

@Data
@NoArgsConstructor
@AllArgsConstructor
@Entity
@PrimaryKeyJoinColumn(name = "id")
public class Programmer extends User{

    @Enumerated(value = EnumType.STRING)
    private Language language;

    @Builder
    public Programmer(Long id, Long credentials, Language language) {
        super(id, credentials);
        this.language = language;
    }

    @Override
    public String toString() {
        return "Programmer{" + "id=" + super.getId() + "credentials=" + super.getCredentials()+
               "language=" + language +
               '}';
    }
}
```

- `InheritanceType.``_TABLE_PER_CLASS_`

В этой стратегии каждый подкласс отображается в отдельной таблице, которая содержит все атрибуты этого класса и его суперклассов. Это означает, что каждый класс имеет свою собственную таблицу, и таблица подкласса содержит все атрибуты как из собственного класса, так и из его суперклассов. При этом, для суперкласса также будет существовать своя таблица, но она будет пустой, так как не содержит непосредственно объектов этого суперкласса.

Преимущества стратегии "Table Per Class":

- Четкое разделение данных между таблицами подклассов, что может уменьшить количество NULL значений и дублирования данных.
- Позволяет добавлять и изменять атрибуты подклассов без необходимости изменения схемы базы данных для суперкласса и других подклассов.

Недостатки стратегии "Table Per Class":

- Более сложные запросы, требующие объединения таблиц при необходимости получения данных из разных подклассов.
- При большом числе подклассов может привести к созданию множества таблиц и увеличению размера базы данных.

```Java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class User {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Long id;
    private Long credentials;
}

@Entity
@Table(name = "programmers")
public class Programmer extends User{

    @Enumerated(value = EnumType.STRING)
    private Language language;

    @Builder
    public Programmer(Long id, Long credentials, Language language) {
        super(id, credentials);
        this.language = language;
    }
}

@Entity
@Table(name = "managers")
public class Manager extends User{

    private String projectName;

    @Builder
    public Manager(Long id, Long credentials, String projectName) {
        super(id, credentials);
        this.projectName = projectName;
    }
}
```

### Языки запросов (HQL, Criteria API)

### HQL

[https://docs.jboss.org/hibernate/core/3.3/reference/en/html/queryhql.html](https://docs.jboss.org/hibernate/core/3.3/reference/en/html/queryhql.html)

Ключевые отличия HQL от SQL:

1. Использование имени класса вместо имени таблицы
2. Использование имени поля класса вместо имени колонки таблицы
3. Необязательное использование SELECT

Чтобы вернуть всех юзеров достаточно вызвать `from User`

  

```Java

@Entity
@Table(name="user_data")
class User {
   @Id
   @GeneratedValue
   public Integer id;

   @Column(name="user_name")
   public String name;

   @Column(name="user_level")
   public Integer level;

   @Column(name="user_created")
   public Date created;
}
 
from User                   	        select * from user_data
from User where id=3 	                select * from user_data where id=3
from User where level in (10,20,30) 	select * from user_data where user_level IN (10, 20, 30)
from User order by created asc 	      select * from user_data order by user_created asc
from User where name like 'тест' 	    select * from user_data where user_name like 'тест'
```

### Criteria API

`(Criteria API)` ==является частью фреймворка== Hibernate и предоставляет программистам ==возможность создавать запросы к базе данных с помощью типобезопасного и объектно-ориентированного подхода.==

Criteria API позволяет строить запросы к базе данных, используя объекты и методы, вместо написания явных строковых запросов на языке HQL или SQL. Он предоставляет мощные и гибкие возможности для формирования запросов с использованием цепочек вызовов методов, что облегчает создание динамических и сложных запросов.

Преимущества Criteria API включают:

1. Типобезопасность: Criteria API использует типы данных и проверку на этапе компиляции, что позволяет избежать ошибок, связанных с опечатками или неправильными именами полей или свойств.
2. Объектно-ориентированный подход: Запросы строятся с использованием объектов, что делает код более понятным и поддерживаемым. Каждый элемент запроса представлен соответствующим объектом, таким как `CriteriaQuery`, `Root`, `Predicate`, `Order`, и т.д.
3. Динамическое формирование запросов: Criteria API позволяет добавлять и комбинировать условия фильтрации, сортировку и ограничение результатов запроса на основе условий, которые могут меняться во время выполнения программы.
4. Улучшенная поддержка IDE: Использование Criteria API облегчает разработку в среде разработки (IDE) благодаря автодополнению, подсказкам и статическому анализу кода.

Для удобного использования лучше внедрить зависимость

[https://mvnrepository.com/artifact/org.hibernate/hibernate-jpamodelgen/6.2.6.Final](https://mvnrepository.com/artifact/org.hibernate/hibernate-jpamodelgen/6.2.6.Final)

```XML
<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-jpamodelgen -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-jpamodelgen</artifactId>
    <version>6.2.6.Final</version>
    <type>pom</type>
    <scope>provided</scope>
</dependency>
```

После внедрения необходимо gradle→build

```Java
/**
     * Возвращает всех сотрудников
     */
    public List<User> findAll(Session session) {
        CriteriaBuilder builder = session.getCriteriaBuilder();
        CriteriaQuery<User> criteriaQuery = builder.createQuery(User.class);
        Root<User> user = criteriaQuery.from(User.class);
        criteriaQuery.select(user);

        return session.createQuery(criteriaQuery).list();
    }

    /**
     * Возвращает всех сотрудников с указанным именем
     */
    public List<User> findAllByFirstName(Session session, String firstName) {
        CriteriaBuilder builder = session.getCriteriaBuilder();
        CriteriaQuery<User> criteriaQuery = builder.createQuery(User.class);
        Root<User> user = criteriaQuery.from(User.class);
        criteriaQuery.select(user).where(
                builder.equal(user.get("person alInfo").get("firstName"), firstName));

        return session.createQuery(criteriaQuery).list();
    }
```

### N+1 problem

`Проблема N+1 (N+1 Problem)` - это ситуация, ==возникающая при выполнении запросов к базе данных в контексте ORM== (Object-Relational Mapping), ==когда при загрузке коллекции объектов также выполняется дополнительный запрос для загрузки связанных данных для каждого объекта в коллекции==. Таким образом, для загрузки N объектов может потребоваться выполнить N+1 запросов к базе данных, что может значительно снизить производительность и привести к избыточным запросам.

`Пример проблемы N+1:`  
Предположим, у нас есть сущности Company и User, связанные отношением один-ко-многим.  

```Java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "company_id")
    private Company company;
}
@Entity
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    @OneToMany(mappedBy = "company", fetch = FetchType.EAGER)
    private Set<User> users = new HashSet<>();

}
```

==Если мы попытаемся загрузить список всех компаний и для каждой компании получить список юзеров, работающих в компании.== `То возникнет проблема N+1.` Сначала будет выполнен запрос для получения списка компаний, а потом N запросов (сколько компаний) на получение списка юзеров:

```Java
Query<Company> selectCFromCompanyC = session.createQuery("select c from Company c", Company.class);
List<Company> resultList = selectCFromCompanyC.getResultList();
            for (Company company : resultList) {
                Set<User> users = company.getUsers();
                System.out.println(users);
            }
```

В моем примере было 4 компании, соотвественно было выполнено 5 запросов:\

```Java
Hibernate: 
    select
        c1_0.id,
        c1_0.name 
    from
        company c1_0
Hibernate: 
    select
        u1_0.company_id,
        u1_0.id
    from
        public.users u1_0 
    where
        u1_0.company_id=?
Hibernate: 
    select
        u1_0.company_id,
        u1_0.id
    from
        public.users u1_0 
    where
        u1_0.company_id=?
Hibernate: 
    select
        u1_0.company_id,
        u1_0.id
    from
        public.users u1_0 
    where
        u1_0.company_id=?
Hibernate: 
    select
        u1_0.company_id,
        u1_0.id
    from
        public.users u1_0 
    where
        u1_0.company_id=?
```

p.s. можно подумать, что переключение с EAGER на LAZY решит проблему, но нет, при обращении к полю users класса Company все равно hiber будет выполнять запросы к БД в том же количестве.

### Решения проблемы N+1

### Batch Loading(пакетная загрузка) @BatchSize

При пакетной загрузке данные загружаются пакетами, снижая количество выполняемых запросов. Это частично решает проблему доп запросов, ==так как вместо== `N+1` ==запросов будет== `N/BatchSize.`

Для ее реализации необходимо добавить аннотацию @BatchSize(int = …) над полем, которое осуществляет связь @OneToMany

```Java
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    @Builder.Default
    @BatchSize(size = 4)
    @OneToMany(mappedBy = "company", fetch = FetchType.EAGER)
    private Set<User> users = new HashSet<>();

}
```

При выполнении данного запроса после получения списка компаний будет вызвано столько запросов к БД users сколько компаний делить на BatschSize.

```Java
Query<Company> selectCFromCompanyC = session.createQuery("select c from Company c", Company.class);
            List<Company> resultList = selectCFromCompanyC.getResultList();
            for (Company company : resultList) {
                Set<User> users = company.getUsers();
                System.out.println(users);
            }
```

В моем случае 4 компании, size =4, соответственно запрос к users будет один

```Java
Hibernate: 
    select
        c1_0.id,
        c1_0.name 
    from
        company c1_0
Hibernate: 
    select
        u1_0.company_id,
        u1_0.id,
        u1_0.birth_date,
        u1_0.death_date,
        u1_0.first_name,
        u1_0.last_name,
        u1_0.user_name,
        u1_0.recipe,
        u1_0.role 
    from
        public.users u1_0 
    where
        u1_0.company_id = any (?)
```

### Fetch JOIN и @Fetch(FetchMode…)

- FETCH JOIN - это конструкция, используемая в языке запросов (JPQL или HQL) в Hibernate для явного объединения связанных данных при выполнении запроса к базе данных. Это позволяет загрузить связанные данные одновременно с основными объектами, избегая проблемы N+1 (когда для каждого основного объекта выполняется дополнительный запрос для загрузки связанных данных).
    
    ==ВАЖНО==: в полях, которые связывают сущности должны быть FetchType.LAZY ( как в User так и Company)
    
    ==ВАЖНО: limit и offset не работают с fetch join==
    
    ```Java
    String jpql = "SELECT u FROM User u JOIN FETCH u.company";
    
    // Выполняем запрос и получаем список пользователей с информацией о компаниях
    List<User> usersWithCompanies = session.createQuery(jpql, User.class).getResultList();
    
    // Проходимся по списку пользователей с информацией о компаниях
    for (User usersWithCompany : usersWithCompanies) {
      System.out.println(usersWithCompany.toString());
    }
    ```
    
    ```Java
    Hibernate: 
        select
            u1_0.id,
            c1_0.id,
            c1_0.name, // так как используется fetch эти данные есть
    в результирующем наборе
            u1_0.birth_date,
            u1_0.death_date,
            u1_0.first_name,
            u1_0.last_name,
            u1_0.user_name,
            u1_0.recipe,
            u1_0.role 
        from
            public.users u1_0 
        join
            company c1_0 
                on c1_0.id=u1_0.company_id
    ```
    
- @Fetch(FetchMode. SELECT OR JOIN OR SUBSELECT)
    
    1. **FetchMode.SELECT**: Этот режим означает, что при доступе к связанной сущности Hibernate будет использовать отдельные запросы SELECT для загрузки данных. Этот режим является стандартным для многих ассоциаций и коллекций и может использоваться в случае, когда ленивая загрузка не является критичной, и вы хотите избежать загрузки лишних данных в память.
        
        При получении связанных сущностей будут выполняться отдельные запросы.
        
        ```Java
        Hibernate: 
            select
                u1_0.id,
                u1_0.company_id,
                u1_0.birth_date,
                u1_0.death_date,
                u1_0.first_name,
                u1_0.last_name,
                u1_0.user_name,
                u1_0.recipe,
                u1_0.role 
            from
                public.users u1_0 
            where
                u1_0.id=?
        Hibernate: 
            select
                c1_0.id,
                c1_0.name 
            from
                company c1_0 
            where
                c1_0.id=?
        ```
        
    2. **FetchMode.JOIN**: Этот режим означает, что Hibernate будет использовать JOIN для объединения данных связанных сущностей в один запрос. Используется, когда вы хотите выполнить один запрос для загрузки всех связанных данных, чтобы избежать проблемы N+1. Однако, этот режим не подходит для ленивой загрузки.
        
        Не решает проблему N+1, если пытаешься получить списком все сущности связанные с другой. Но можно вместо двух запросов при получении одной сущности, которая связана с другой(поле получаемой сущности - это другая сущность), выполнить один запрос через LEFT JOIN.
        
        То есть данная аннотация работает, если мы получаем сущность по id. Пример:
        
        ```Java
        package org.example.entity;
        
        import jakarta.persistence.*;
        import lombok.AllArgsConstructor;
        import lombok.Builder;
        import lombok.Data;
        import lombok.NoArgsConstructor;
        import org.hibernate.annotations.Fetch;
        import org.hibernate.annotations.FetchMode;
        import org.hibernate.annotations.JdbcTypeCode;
        import org.hibernate.type.SqlTypes;
        
        import java.util.Objects;
        
        @Data
        @NoArgsConstructor
        @AllArgsConstructor
        @Builder
        @Entity
        @Table(name = "users", schema = "public")
        public class User {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
        
            @ManyToOne(fetch = FetchType.LAZY)
            @JoinColumn(name = "company_id")
            @Fetch(FetchMode.JOIN)
            private Company company;
        
            @JdbcTypeCode(SqlTypes.JSON)
            private Recipe recipe;
        }
        ```
        
        ```Java
        User user1 = session.get(User.class, 1L);
        ```
        
        ```Java
        Hibernate: 
            select
                u1_0.id,
                c1_0.id,
            from
                public.users u1_0 
            left join
                company c1_0 
                    on c1_0.id=u1_0.company_id 
            where
                u1_0.id=?
        ```
        
    3. **FetchMode.SUBSELECT**: Этот режим также использует SELECT, но с подзапросом (subselect) для загрузки связанных данных.  
        Может выбросить AnnotationException, если пометить аннотацией не коллекцию. То есть этой аннотацией можно помечать @OneToMany, но не @ManyToOne  
        
        ```Java
        @ManyToOne(fetch = FetchType.LAZY)
            @JoinColumn(name = "company_id")
            @Fetch(FetchMode.SUBSELECT)
            private Company company;
        
        
        Exception in thread "main" org.hibernate.AnnotationException: Association 'company' is annotated '@Fetch(SUBSELECT)' but is not many-valued
        ```
        
        Пример использования
        
        ```Java
        @Entity
        public class Company {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Integer id;
        
            private String name;
        
            @OneToMany(mappedBy = "company",fetch = FetchType.LAZY)
            @Fetch(FetchMode.SUBSELECT)
            private Set<User> users = new HashSet<>();
        }
        ```
        
        ```Java
        Query<Company> selectCFromCompanyC = session.createQuery("select c from Company c", Company.class);
        List<Company> resultList = selectCFromCompanyC.getResultList();
        for (Company company : resultList) {
                        Set<User> users = company.getUsers();
                        System.out.println(users);
        }
        ```
        
        ```Java
        Hibernate: 
            select
                c1_0.id,
                c1_0.name 
            from
                company c1_0
        Hibernate: 
            select
        				u1_0.id,
                u1_0.company_id
            from
                public.users u1_0 
            where
                u1_0.company_id in(select
                    c1_0.id 
                from
                    company c1_0)
        ```
        
    
      
    

### @FetchProfile

Аннотация `@FetchProfile` в Hibernate позволяет определить  
пользовательские профили извлечения (fetch profiles), которые позволяют  
настроить, какие атрибуты и связанные объекты должны быть предварительно  
загружены из базы данных при выполнении запросов. Это позволяет  
оптимизировать производительность запросов и избежать проблемы N+1.  

  

Аннотация @FetchProfile Repeatable поэтому можно создавать несколько profiles Сама аннотация содержит 2 поля - можно задать название и fetchOverrides - которому можно присвоить массив из @FetchProfile.FetchOverride.

В который передается класс, на поле в сущности, которое ассоциируется и режим( доступен только FetchMode.JOIN)  
Если использовать другой режим выбросится исключение  

```Java
Exception in thread "main" org.hibernate.MappingException: Only FetchMode.JOIN is currently supported
```

Пример:

```Java
@Entity
@FetchProfile(name = "customFetchProfile", fetchOverrides = {@FetchProfile.FetchOverride(
        entity = User.class, association = "company", mode = FetchMode.JOIN)})
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "company_id")
    private Company company;
}
@Entity
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    @OneToMany(mappedBy = "company", fetch = FetchType.EAGER)
    private Set<User> users = new HashSet<>();

}
```

Перед выполнением кода необходимо разрешить использование fetchProfile:

```Java
try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession()) {
            session.beginTransaction();
            session.enableFetchProfile("customFetchProfile");
Query<User> selectUFromUserU = session.createQuery("select u from User u", User.class);
            List<User> resultList = selectUFromUserU.getResultList();

            for (User user1 : resultList) {
                System.out.println(user1.toString());
            }
}
```

```Java
Hibernate: 
    select
        u1_0.id,
        c1_0.id,
        c1_0.name,
    from
        public.users u1_0 
    left join
        company c1_0 
            on c1_0.id=u1_0.company_id

User(id=1, info=PersonInfo(firstName=Vadim), company=Company(id=1, name=GOOGLE))
User(id=2, info=PersonInfo(firstName=Rika), company=Company(id=1, name=GOOGLE))
```

### Entity Graph API

[https://www.baeldung.com/jpa-entity-graph](https://www.baeldung.com/jpa-entity-graph) Статья

Два варианта использование аннотации или написать вручную.

- с помощью аннотации @NamedEntityGraph  
    Аннотация @NamedEntityGraph позволяет указать атрибуты, которые следует включить, когда мы хотим загрузить сущность и связанные ассоциации.  
    

```Java
@Entity
@Table(name = "users", schema = "public")
@NamedEntityGraph(name = "UserWithCompany",
attributeNodes = {
        @NamedAttributeNode(value = "company")
})
public class User {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@ManyToOne(fetch = FetchType.EAGER)
@JoinColumn(name = "company_id")
private Company company;}

// ниже код

Map<String, Object> graph = Map.of(
        GraphSemantic.LOAD.getJakartaHintName(), session.getEntityGraph("UserWithCompany")
);
var user = session.find(User.class, 1L, graph); // по id
List<User> selectUFromUserU = session.createQuery("SELECT u FROM User u", User.class)
        .setHint(GraphSemantic.LOAD.getJakartaHintName(), session.getEntityGraph("UserWithCompany"))
        .getResultList(); //список всех users
```

Также можно загружать связанные подсущности с помощью subgraphs

```Java
@Entity
@NamedEntityGraph(name = "UserWithCompanyAndAddress",
        attributeNodes = {
								@NamedAttributeNode(value = "company")
                @NamedAttributeNode(value = "company", subgraph = "companySubgraph")
        },
        subgraphs = {
                @NamedSubgraph(name = "companySubgraph",
                        attributeNodes = {
                                @NamedAttributeNode("address")
                        })
        })
public class User {
   @ManyToOne
    private Company company;
}

@Entity
public class Company {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    @OneToOne
    private Address address;
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String street;
    private String city;
    private String postalCode;
}
```

При получении сущности или списка сущностей необходимо явно указывать graph:

```Java
session.beginTransaction();
Map<String, Object> graph = Map.of(
	GraphSemantic.LOAD.getJakartaHintName(),
  session.getEntityGraph("UserWithCompany"));

var user = session.find(User.class, 1L, graph);
List<User> selectUFromUserU = session.createQuery("SELECT u FROM User u", User.class)
      .setHint(GraphSemantic.LOAD.getJakartaHintName(),
							 session.getEntityGraph("UserWithCompany"))
      .getResultList();
```

**_GraphSemantic.LOAD и GraphSemantic.FETCH отличается тем, что LOAD подгружает указанный FetchType, то есть если над полем сущности был явно указан EAGER будет EAGER. если LAZY, то LAZY. В случае с FETCH всегда будет LAZY._**

  

- Программный способ создать entityGraph:

```Java
RootGraph<User> entityGraph = session.createEntityGraph(User.class);
            entityGraph.addAttributeNodes("company");
SubGraph<Address> company = entityGraph.addSubgraph("company");
company.addAttributeNodes("address");
List<User> selectUFromUserU = session.createQuery("SELECT u FROM User u", User.class)
  .setHint(GraphSemantic.LOAD.getJakartaHintName(), entityGraph)
  .getResultList();
```

### Transaction and Locks

### JPA Transactions

Чтобы настроить уровень изоляции транзакции в Hibernate можно прописать в `hibernate.cfg.xml,` где уровни изоляции согласно стандарту JDBC 1 2 4 8.

```Java
<property name="connection.isolation">4</property>
```

Чтобы посмотреть уровень изоляции существует метод session.doWork(Work work)

```Java
Work work = new Work() {
                @Override
                public void execute(Connection connection) throws SQLException {
                    int transactionIsolation = connection.getTransactionIsolation();
                    System.out.println(transactionIsolation);
                }
            };
            session.doWork(work); //4
        }
```

Программно осуществлять коммиты и ролбэки можно следующим образом (нежелательный)

```Java
try{
                    session.beginTransaction();
                    var user = session.find(User.class,1L);
                    var user2 = session.find(User.class,2L);
                    session.getTransaction().commit();
                }
                catch (Exception e){
                    session.getTransaction().rollback();
                    throw e;
                }
```

  

### Locks

Блокировки в транзакционной среде используются для управления доступом к данным в многопользовательской среде. Они гарантируют, что две или более транзакции не будут одновременно изменять одни и те же данные. Hibernate предоставляет механизмы оптимистических и пессимистических блокировок для этой цели.

В качестве параметра метода find из JPA можно передать LockModeType и необходимый режим.

Оптимистические блокировки на уровне приложения, пессимистические на уровне БД.

Передать соответствующий режим блокировки можно следующим образом:

```Java
LockOptions lockOptions = new LockOptions();
lockOptions.setLockMode(LockMode.PESSIMISTIC_WRITE); // Устанавливаем желаемый режим блокировки

Map<String, Object> properties = new HashMap<>();
properties.put("javax.persistence.lock.timeout", 5000); // Пример установки таймаута блокировки в миллисекундах
properties.put("org.hibernate.lockOptions", lockOptions); // Передаем LockOptions в properties
session.find(User.class, 1L, LockModeType.NONE, Map<String, Object> properties);
```

Либо через LockOptions и метод get

```Java
LockOptions options = new LockOptions(LockMode.PESSIMISTIC_READ, 5000, PessimisticLockScope.EXTENDED)
var user3 = session.get(User.class, 1, options);
```

Также можно задать программно для всей выборки возвращаемых результатов из БД

```Java
session.createQuery("select u from User u")
                    .setLockMode(LockModeType.PESSIMISTIC_READ)
                    .setTimeout(5000).list();

session.createQuery("select u from User u")
                    .setLockMode(LockModeType.PESSIMISTIC_READ)
                    .setHibernateLockMode(LockMode.PESSIMISTIC_READ)
                    .setHint("javax.persistence.lock.timeout", 5000).list();

LockOptions options = new LockOptions(LockMode.PESSIMISTIC_READ, 5000, PessimisticLockScope.EXTENDED)
session.createQuery("select u from User u")
                    .setLockOptions(options).list();
```

### enum LockModeType

[https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/lockmodetype](https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/lockmodetype)

```Java
package jakarta.persistence;


public enum LockModeType
{
    /**
     *  Synonymous with <code>OPTIMISTIC</code>.
     *  <code>OPTIMISTIC</code> is to be preferred for new
     *  applications.
     *
     */
    READ,

    /**
     *  Synonymous with <code>OPTIMISTIC_FORCE_INCREMENT</code>.
     *  <code>OPTIMISTIC_FORCE_IMCREMENT</code> is to be preferred for new
     *  applications.
     *
     */
    WRITE,

    /**
     * Optimistic lock.
     *
     * @since 2.0
     */
    OPTIMISTIC,

    /**
     * Optimistic lock, with version update.
     *
     * @since 2.0
     */
    OPTIMISTIC_FORCE_INCREMENT,

    /**
     *
     * Pessimistic read lock.
     *
     * @since 2.0
     */
    PESSIMISTIC_READ,

    /**
     * Pessimistic write lock.
     *
     * @since 2.0
     */
    PESSIMISTIC_WRITE,

    /**
     * Pessimistic write lock, with version update.
     *
     * @since 2.0
     */
    PESSIMISTIC_FORCE_INCREMENT,

    /**
     * No lock.
     *
     * @since 2.0
     */
    NONE
}
```

### enum PessimisticLockScope

[https://docs.oracle.com/javaee/6/api/javax/persistence/PessimisticLockScope.html](https://docs.oracle.com/javaee/6/api/javax/persistence/PessimisticLockScope.html)

- это перечисление (enum) в Hibernate, которое предоставляет различные уровни пессимистической блокировки при работе с базой данных. Оно позволяет управлять тем, какие ресурсы и данные будут заблокированы во время выполнения определенных операций.

1. **NORMAL**: Это наиболее общий уровень блокировки. Он используется для блокирования данных, чтобы предотвратить одновременное изменение данных разными пользователями. В этом режиме блокируются строки данных, и другие пользователи будут ждать, пока блокировка не будет снята.
2. **EXTENDED**: Этот уровень блокировки обычно более долговременный. Он может использоваться, например, для блокирования диапазона данных, что может быть полезно в некоторых сценариях, например, при обработке больших объемов данных.

### Optimistic Locks

Оптимистическая блокировка - это подход, при котором система позволяет нескольким транзакциям работать с данными параллельно, допуская возможность конфликтов при сохранении изменений. Однако перед сохранением изменений каждая транзакция проверяет, не были ли данные изменены другой транзакцией за время её выполнения.

Hibernate предоставляет механизм оптимистических блокировок через использование версионирования. Это означает, что каждая сущность имеет атрибут, который инкрементируется при каждом изменении. При попытке сохранения сущности Hibernate проверяет, соответствует ли текущее значение атрибута значению, которое было установлено в момент загрузки сущности. Если значения не совпадают, Hibernate понимает, что сущность была изменена другой транзакцией, и генерирует исключение, позволяя вам обработать эту ситуацию.

Для ,блокировок в Hibernate используется аннотация `@OptimisticLocking`, куда уже передается в качестве параметра `OptimisticLockType` и режим.

### OptimisticLockType.VERSION

Для оптимистической блокировки в Hibernate используется аннотация `@OptimisticLocking` с указанием типа блокировки `OptimisticLockType.VERSION`. Кроме того, к сущности следует добавить атрибут с аннотацией `@Version`,  
который будет служить версионным атрибутом. Этот атрибут будет  
автоматически инкрементироваться Hibernate при каждом изменении  
сущности, позволяя обнаруживать конфликты при параллельных изменениях  
данных.  

```Java
@Entity
@OptimisticLocking(type = OptimisticLockType.VERSION)
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @Version
    private Long version; // Версионный атрибут
}
```

Также вместо целочисленного значения можно использовать даты, но не желательно, так как время может быть расинхронизировано на разных машинах.

Пример кода, где будет выброшены исключения `OptimisticLockException` caused by `LockAcquisitionException`

```Java
try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession();
             Session session2 = factory.openSession()) {
            session.beginTransaction();
            session2.beginTransaction();

            var user = session.find(User.class, 1, LockModeType.OPTIMISTIC);
            user.setBalance(user.getBalance()+50);
            var user2 = session2.find(User.class,1, LockModeType.OPTIMISTIC);
            user2.setBalance(user.getBalance()+100);

            session.getTransaction().commit();
            session2.getTransaction().commit();
        }
```

Исключение `OptimisticLockException` указывает на конфликт оптимистической блокировки между двумя транзакциями. Ошибка происходит, потому что обе транзакции пытаются изменить одну и ту же сущность `User` параллельно, и Hibernate обнаруживает, что версионный атрибут (в данном случае `version`) изменился в промежутке между чтением и записью.

ВАЖНО: version перед началом чтения, должен быть проинициилизирован значеним, обычно 1. Если будет null, то выбросится NullPointerException.

### OptimisticLockType.ALL

`OptimisticLockType.ALL` - это один из возможных значений для параметра `type` аннотации `@OptimisticLocking` в Hibernate. Это значение представляет собой комбинацию оптимистических блокировок по всем полям сущности, что означает, что при обновлении сущности будут учитываться все поля для обнаружения конфликтов ( каждое поле считается версионируемым, и если хотя бы одно поле было изменено с момента загрузки сущности, то будет сгенерирована ошибка оптимистической блокировки при попытке сохранения).

ВАЖНО: при использовании ALL или DIRTY необходимо добавлять аннотацию `@DynamicUpdate` иначе выбросится исключение:

```Java
Exception in thread "main" org.hibernate.MappingException: optimistic-lock=all|dirty requires dynamic-update="true": org.example.entity.User
```

```Java
@OptimisticLocking(type = OptimisticLockType.ALL)
@DynamicUpdate
public class User {
@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
...
}
```

В случае с ALL в условие where будут переданы все поля класса USER

```Java
var user = session.find(User.class, 1);
            user.setBalance(user.getBalance()+50);

Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
        u1_0.company_id,
        u1_0.birth_date,
        u1_0.death_date,
        u1_0.first_name,
        u1_0.last_name,
        u1_0.user_name,
        u1_0.recipe,
        u1_0.role 
    from
        public.users u1_0 
    where
        u1_0.id=?
Hibernate: 
    update
        public.users 
    set
        balance=? 
    where
        id=? 
        and balance=? 
        and company_id=? 
        and birth_date is null 
        and death_date is null 
        and first_name=? 
        and last_name is null 
        and user_name is null 
        and recipe is null 
        and role=?
```

Аналогично с предыдущим примером при изменении поля сущности в разных сессиях выбросится OptimisticLockException

```Java
try (SessionFactory factory = configuration.buildSessionFactory();
             Session session = factory.openSession();
             Session session2 = factory.openSession()) {
            session.beginTransaction();
            session2.beginTransaction();

            var user = session.find(User.class, 1); //LockModeType не нужен
            user.setBalance(user.getBalance()+50);
            var user2 = session2.find(User.class,1);
            user2.setBalance(user.getBalance()+100);

            session.getTransaction().commit();
            session2.getTransaction().commit(); //Exception in thread "main" jakarta.persistence.OptimisticLockException
        }
```

  

  

  

### OptimisticLockType.DIRTY

`@OptimisticLocking(type = OptimisticLockType.DIRTY)` указывает Hibernate использовать оптимистическую блокировку по "грязным" полям, то есть тем, которые были изменены в рамках текущей транзакции. Это позволяет уменьшить объем блокировки, так как только измененные поля будут участвовать в конфликтах.

Комбинирование `@OptimisticLocking(type = OptimisticLockType.DIRTY)` с `@DynamicUpdate` позволяет улучшить производительность и снизить нагрузку на базу данных, сохраняя при этом механизм оптимистической блокировки.

```Java
@Entity
@OptimisticLocking(type = OptimisticLockType.DIRTY)
@DynamicUpdate
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;

    // Геттеры и сеттеры
}
```

При изменении balance в условие where будет передан только balance в отличие от предыдущего примера с ALL, где были переданы все поля

```Java
Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
        u1_0.company_id,
        u1_0.birth_date,
        u1_0.death_date,
        u1_0.first_name,
        u1_0.last_name,
        u1_0.user_name,
        u1_0.recipe,
        u1_0.role 
    from
        public.users u1_0 
    where
        u1_0.id=?
Hibernate: 
    update
        public.users 
    set
        balance=? 
    where
        id=? 
        and balance=?
```

### Pessimistic Locks

Локи на уровне БД в разных СУБД выглядят по-разному.  
Для PostgreSQL  
[https://www.postgresql.org/docs/current/explicit-locking.html](https://www.postgresql.org/docs/current/explicit-locking.html)

FOR UPDATE

- FOR UPDATE приводит к тому, что строки, выбранные оператором SELECT, блокируются, как если бы они были заблокированы для обновления. Это предотвращает их блокировку, изменение или удаление другими транзакциями до завершения текущей транзакции. Другие транзакции, пытающиеся выполнить UPDATE, DELETE, SELECT FOR UPDATE, SELECT FOR NO KEY UPDATE, SELECT FOR SHARE или SELECT FOR KEY SHARE для этих строк, будут заблокированы до завершения текущей транзакции. Соответственно, SELECT FOR UPDATE будет ждать конкурентную транзакцию, выполнившую любую из этих команд для той же строки, а затем заблокирует и вернет обновленную строку (или не вернет строку, если строка была удалена). Однако в рамках транзакции REPEATABLE READ или SERIALIZABLE будет вызвана ошибка, если строка, которую нужно заблокировать, изменилась с момента начала транзакции. Дополнительное обсуждение см. в разделе 13.4.

FOR NO KEY UPDATE

- Ведет себя аналогично FOR UPDATE, за исключением того, что получаемая блокировка слабее: эта блокировка не блокирует команды SELECT FOR KEY SHARE, которые пытаются получить блокировку для тех же строк. Этот режим блокировки также приобретается при любом UPDATE, который не приобретает блокировку FOR UPDATE.

FOR SHARE

- Ведет себя аналогично FOR NO KEY UPDATE, за исключением того, что он приобретает общую блокировку, а не эксклюзивную, для каждой выбранной строки. Общая блокировка блокирует другие транзакции от выполнения операций UPDATE, DELETE, SELECT FOR UPDATE или SELECT FOR NO KEY UPDATE для этих строк, но не мешает им выполнять SELECT FOR SHARE или SELECT FOR KEY SHARE.

FOR KEY SHARE

- Ведет себя аналогично FOR SHARE, за исключением того, что блокировка слабее: SELECT FOR UPDATE блокируется, но не SELECT FOR NO KEY UPDATE. Блокировка ключа-доступа блокирует другие транзакции от выполнения DELETE или любого UPDATE, изменяющего значения ключа, но не блокирует другие операции UPDATE, и также не мешает выполнению SELECT FOR NO KEY UPDATE, SELECT FOR SHARE или SELECT FOR KEY SHARE.

### OptimisticLockType.PESSIMISTIC_READ

`PESSIMISTIC_READ`: при этом режиме блокировки записи блокируются с начала транзакции, только для чтения. В основном это означает "Я не хочу, чтобы кто-либо обновлял эту запись, пока я читаю ее, но мне не противно, если другие также будут читать". Это означает, что другие, также пытающиеся выполнить `PESSIMISTIC_READ`, будут успешны, но те, кто пытается выполнить `PESSIMISTIC_WRITE`, не смогут.

```Java
session.beginTransaction();
            session2.beginTransaction();

            var user = session.find(User.class, 1, LockModeType.PESSIMISTIC_READ);
            user.setBalance(user.getBalance() + 50);
            var user2 = session2.find(User.class, 1);
            user2.setBalance(user.getBalance() + 100);

            session2.getTransaction().commit();
            session.getTransaction().commit();
```

```Java
Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
    from
        public.users u1_0 
    where
        u1_0.id=? for share
Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
    from
        public.users u1_0 
    where
        u1_0.id=?
Hibernate: 
    update
        public.users 
    set
        balance=?,
    where
        id=?
```

### OptimisticLockType.PESSIMISTIC_WRITE

При этом режиме блокировки записи блокируются с начала транзакции, только для записи. Вы говорите: "Я собираюсь обновить эту запись, поэтому никто не может читать или писать в нее, пока я не закончу". Это означает, что и те, кто пытаются выполнить `PESSIMISTIC_READ`, и те, кто пытаются выполнить `PESSIMISTIC_WRITE`, не смогут.

```Java
Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
    from
        public.users u1_0 
    where
        u1_0.id=? for no key update
Hibernate: 
    select
        u1_0.id,
        u1_0.balance,
    from
        public.users u1_0 
    where
        u1_0.id=?
Hibernate: 
    update
        public.users 
    set
        balance=?,
    where
        id=?
```

### OptimisticLockType.PESSIMISTIC_FORCE_INCREMENT

`PESSIMISTIC_FORCE_INCREMENT` работает как `PESSIMISTIC_WRITE` и дополнительно инкрементирует атрибут версии сущности, имеющей версионирование.Объект будет обновлен и версия будет увеличена, даже если атрибуты объекта не были изменены.

```Java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Version
    private Long version;

    private Long balance;
}
```

```Java
session.beginTransaction();
            session2.beginTransaction();

            var user = session.find(User.class, 1, LockModeType.PESSIMISTIC_FORCE_INCREMENT);
            user.setBalance(user.getBalance() + 50);
            var user2 = session2.find(User.class, 1);
            user2.setBalance(user.getBalance() + 100);

            session2.getTransaction().commit();
            session.getTransaction().commit();
```

### Read Only Mode

Режим только для чтения (Read-Only Mode) - это режим работы, при котором доступна только операция чтения данных, а любые попытки модификации данных, такие как обновление или удаление, запрещены. Этот режим полезен, когда ты хочешь обеспечить безопасность данных и предотвратить случайные или нежелательные изменения данных.

Чтобы задать этот режим для всей сессии:

```Java
session.setDefaultReadOnly(true);
session.beginTransaction();
```

Либо для конкретной сущности или сущностей:

```Java
var user = session.find(User.class, 1, LockModeType.PESSIMISTIC_WRITE );
session.setReadOnly(user, true)


session.createQuery("select u from User u")
                    .setReadOnly(true);
```

Либо можно на уровне БД

```Java
session.createNativeQuery("SET TRANSACTION READ ONLY ").executeUpdate();
```

  

### Listeners, Interceptors, Envers

Хорошая обзорная статья  
  
[https://www.baeldung.com/database-auditing-jpa](https://www.baeldung.com/database-auditing-jpa)

### Аннотация @PrePersist etc

|   |   |
|---|---|
|**@PrePersist**|Вызывается перед сохранением объекта в базу. (SQL INSERT)|
|**@PostPersist**|Вызывается сразу после сохранения объекта в базу. (SQL INSERT)|
|**@PreRemove**|Вызывается перед удалением объекта в базе.|
|**@PostRemove**|Вызывается после удаления объекта в базе.|
|**@PreUpdate**|Вызывается перед обновлением (SQL UPDATE) объекта в базе.|
|**@PostUpdate**|Вызывается после обновления (SQL UPDATE) объекта в базе.|
|**@PostLoad**|Вызывается после того, как объект был загружен из базы.|

Пример, где мы прописываем классу правильное время создания и обновления его объектов:

```Java
@Entity
@Table(name = "entities")
public class Entity {
  ...

  @Column(name="created_time")
private Date created;

  @Column(name="updated_time")
private Date updated;

  @PrePersist
protected void onCreate() {
    created = new Date();
  }

  @PreUpdate
protected void onUpdate() {
  updated = new Date();
  }
}
```

Если Hibernate сохраняет объект в первый раз, то он вызовет метод, помеченный аннотацией `@PrePersist`. Если обновляет в базе существующий объект, то вызовет метод, помеченный аннотацией `@PreUpdate`.

### Создание Listener`a

Спецификация JPA позволяет тебе объявить listener-классы, которые будут вызываться в определенные моменты обработки Entity-объектов.

Если у тебя много похожих Entity-объектов, то ты можешь вынести их часть в базовый класс и добавить Listener, который бы управлял их поведением. Пример:

  

```Java
@MappedSuperclass
public abstract class BaseEntity {

    private Timestamp createdOn;

    private Timestamp updatedOn;

}


@Entity
public class User extends BaseEntity {

     @Id
     private Long id;

     private String name;
}
```

Тогда для класса BaseEntity можно создать специальный listener-класс:

```Java
public class TimeEntityListener {

    public void onPersist(Object entity) {
    	if (entity instanceof BaseEntity) {
        	BaseEntity baseEntity = (BaseEntity) entity;
        	baseEntity.createdOn = now();
    	}
    }

    public void onUpdate(Object entity) {
    	if (entity instanceof BaseEntity) {
        	BaseEntity baseEntity = (BaseEntity) entity;
        	baseEntity.updatedOn = now();
    	}
    }

    private Timestamp now() {
    	return Timestamp.from(LocalDateTime.now().toInstant(ZoneOffset.UTC)   );
    }
}
```

И связать класс User и его listener можно с помощью парочки аннотаций:

```Java
@Entity
@EntityListeners(class= TimeEntityListener.class)
public class User extends BaseEntity {

     @Id
     private Long id;

     private String name;
}
```

### Event Listeners

![[images/Untitled 13 8.png|Untitled 13 8.png]]

Список всех Listeners можно посмотреть в пакете `**org.hibernate.event.spi**`

[https://docs.jboss.org/hibernate/orm/5.6/javadocs/org/hibernate/event/spi/package-summary.html](https://docs.jboss.org/hibernate/orm/5.6/javadocs/org/hibernate/event/spi/package-summary.html)

Чтобы создать свой собственный слушатель необходимо имплементировать интерфейс определенного Event Listenera и реализовать его методы.

```Java
public class AuditTableListener implements PreDeleteEventListener, PreInsertEventListener {
    @Override
    public boolean onPreDelete(PreDeleteEvent event) {
       auditEntity(event, Audit.Operation.DELETE);

        return false;
    }

    @Override
    public boolean onPreInsert(PreInsertEvent event) {
        auditEntity(event, Audit.Operation.INSERT);

        return false;
    }

    private void auditEntity(AbstractPreDatabaseOperationEvent event, Audit.Operation operation){
        if(!(event.getEntity() instanceof Audit)){ //важно, чтобы не создавался аудит на аудит
            Audit audit = Audit.builder()
                    .entityID((Serializable) event.getId())
                    .entityContent(event.getEntity().toString())
                    .entityName(event.getPersister().getEntityName())
                    .operation(operation)
                    .build();
            event.getSession().persist(audit);
        }
    }
}
```

В данном примере слушатель наблюдает за сохранением и удалением сущностей, и до выполнения этих действий заносит в отдельную таблицу сводную информацию о том, что было сохранено.

ВАЖНО: не забыть добавить сущность в конфигурацию

```Java
 configuration.addAnnotatedClass(Audit.class);
```

А также до открытия сессии необходимо преобразовать `SessionFactory` в `SessionFactoryImpl` у этого экземпляра получить `EventListenerRegistry` и добавить в него наш Listener с указанием конкретной стадии жизненного цикла из enum `EventType`

```Java
@UtilityClass
public class HibernateUtil {

    public static SessionFactory buildSessionFactory(){
        Configuration configuration = buildConfiguration();
        configuration.configure();

        var sessionFactory = configuration.buildSessionFactory();
        var sessionFactoryImpl = sessionFactory.unwrap(SessionFactoryImpl.class);
        EventListenerRegistry service = sessionFactoryImpl.getServiceRegistry().getService(EventListenerRegistry.class);
        service.appendListeners(EventType.PRE_INSERT, AuditTableListener.class);
        service.appendListeners(EventType.PRE_DELETE, AuditTableListener.class);
        return sessionFactory;
    }

    public static  Configuration buildConfiguration(){
        Configuration configuration = new Configuration();
        configuration.addAttributeConverter(DeathdayConverter.class, true);
        configuration.addAttributeConverter(BirthdayConverter.class, true);
        configuration.addAnnotatedClass(User.class);
        configuration.addAnnotatedClass(Company.class);
        configuration.addAnnotatedClass(Address.class);
        configuration.addAnnotatedClass(Audit.class);
        configuration.setPhysicalNamingStrategy(new CamelCaseToUnderscoresNamingStrategy());
        return configuration;
    }
}
```

После сохранения сущности User в таблицу AUDIT будет сохранена информация о сущности USER, операции enum Operation ( в классе Audit, в моем случае INSERT - это 3)

```SQL
Hibernate: 
    create table audit (
        operation smallint check (operation between 0 and 3),
        id bigserial not null,
        entity_content varchar(255),
        entity_name varchar(255),
        entityid bytea,
        primary key (id)
    )
```

![[images/Untitled 14 7.png|Untitled 14 7.png]]

### Interceptors

Интерсепторы, как подсказывает название, предоставляют обратные вызовы для определенных событий, происходящих внутри Hibernate. Это помогает реализовать пересекающиеся концерны в стиле AOP и расширять функциональность Hibernate.

1. Создание интерсептора

Для создания нового интерсептора в Hibernate необходимо реализовать интерфейс org.hibernate.Interceptor. Этот интерфейс предоставляет методы для проверки и/или манипуляции свойствами постоянного объекта перед сохранением, обновлением, удалением или загрузкой.

До Hibernate 6.0 предпочтительным способом было наследоваться от `EmptyInterceptor`, чтобы переопределить только необходимые методы, потому что для реализации интерсептора нам нужно было реализовать все 14 методов в интерфейсе. Очевидно, что это не всегда было удобным.

С Hibernate 6.0 `EmptyInterceptor` устарел. Методы внутри интерфейса Interceptor стали методами по умолчанию, поэтому теперь нам нужно переопределять только необходимые методы.

```Java
public class AuditInterceptor implements Interceptor {
}

```

1. Переопределение методов интерсептора

Интерфейс интерсептора предоставляет следующие важные методы для перехвата конкретных событий:

- afterTransactionBegin(): Вызывается при начале транзакции Hibernate.
- afterTransactionCompletion(): Вызывается после фиксации или отката транзакции.
- beforeTransactionCompletion(): Вызывается перед фиксацией транзакции (но не перед откатом).
- onCollectionRecreate(): Вызывается перед (повторным) созданием коллекции.
- onCollectionRemove(): Вызывается перед удалением коллекции.
- onCollectionUpdate(): Вызывается перед обновлением коллекции.
- onDelete(): Вызывается перед удалением объекта.
- onFlushDirty(): Вызывается, когда объект обнаруживается как измененный во время флаша.
- onLoad(): Вызывается непосредственно перед инициализацией объекта.
- onSave(): Вызывается перед сохранением объекта.
- postFlush(): Вызывается после флаша.
- preFlush(): Вызывается перед флашем.

```Java
package org.example.interceptors;

import lombok.extern.slf4j.Slf4j;
import org.hibernate.CallbackException;
import org.hibernate.Interceptor;
import org.hibernate.type.Type;

import java.util.Arrays;
@Slf4j
public class GlobalInterceptor implements Interceptor {

    @Override
    public boolean onFlushDirty(Object entity, Object id, Object[] currentState, Object[] previousState, String[] propertyNames, Type[] types) throws CallbackException {

        if (log.isInfoEnabled()) {
            log.info("********************AUDIT INFO START*******************");
            log.info("Entity Name    :: " + entity.getClass());
            log.info("Current  state :: " + Arrays.deepToString(currentState));
            log.info("propertyNames  :: " + Arrays.deepToString(propertyNames));
            log.info("********************AUDIT INFO END*******************");
        }

        return Interceptor.super.onFlushDirty(entity, id, currentState, previousState, propertyNames, types);
    }
}
```

1. Регистрация интерсептора

Мы можем зарегистрировать интерсептор в контексте постоянства двумя способами.

- С сессией

Если нам нужно использовать интерсептор только в нескольких местах в приложении, мы можем зарегистрировать его с экземплярами Session в этих местах.

```Java
try (Session session = sessionFactory.withOptions()
        .interceptor(new GlobalInterceptor()).openSession()) {
      session.getTransaction().begin();

      //...
}

```

- С SessionFactory

Чтобы включить интерсептор во всех созданных сессиях в приложении, мы можем добавить интерсептор в саму SessionFactory.

```Java
try {
  StandardServiceRegistry standardRegistry
      = new StandardServiceRegistryBuilder()
      .configure("hibernate-test.cfg.xml")
      .build();

  Metadata metadata = new MetadataSources(standardRegistry)
      .addAnnotatedClass(EmployeeEntity.class)
      .getMetadataBuilder()
      .build();

  sessionFactory = metadata
      .getSessionFactoryBuilder()
      .applyInterceptor(new AuditInterceptor())
      .build();

} catch (Throwable ex) {
  throw new ExceptionInInitializerError(ex);
}

```

Убедитесь, что мы не храним какую-либо информацию о состоянии в интерсепторе, потому что он будет использоваться несколькими потоками. Для увеличения безопасности от случайного использования мы также можем сделать контекст сессии потоково локальным.

```XML
hibernate.current_session_context_class=org.hibernate.context.internal.ThreadLocalSessionContext
```

Для демонстрации давай создадим сущность и обновим её информацию.

```Java
User user = User.builder().role(Role.ADMIN).balance(100L).build();
            session.persist(user);
            user.setBalance(200L);


2023-08-17 21:18:33.866 [main] INFO  org.example.interceptors.GlobalInterceptor - ********************AUDIT INFO START*******************
2023-08-17 21:18:33.866 [main] INFO  org.example.interceptors.GlobalInterceptor - Entity Name    :: class org.example.entity.User
2023-08-17 21:18:33.867 [main] INFO  org.example.interceptors.GlobalInterceptor - Previous state :: [100, null, null, null, ADMIN]
2023-08-17 21:18:33.867 [main] INFO  org.example.interceptors.GlobalInterceptor - Current  state :: [200, null, null, null, ADMIN]
2023-08-17 21:18:33.867 [main] INFO  org.example.interceptors.GlobalInterceptor - propertyNames  :: [balance, company, info, recipe, role]
2023-08-17 21:18:33.867 [main] INFO  org.example.interceptors.GlobalInterceptor - ********************AUDIT INFO END*******************
```

### Hibernate Envers

### Внедрение

Для использования необходимо внедрить зависимость

[https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-envers/6.3.0.CR1](https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-envers/6.3.0.CR1)

```XML
<!-- https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-envers -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-envers</artifactId>
    <version>6.3.0.CR1</version>
</dependency>
```

Чтобы таблица аудировалась над сущность необходимо поставить над сущностью аннотацию @Audited

```Java
@Entity
@Audited
@Table(name = "users", schema = "public")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
...
}
```

==**ВАЖНО :**== **Если существует mapping в сущности необходимо также аннотировать @Audited либо @NotAudited, если аудирование не нужно.**

```Java
@Entity
@Audited
@Table(name = "users", schema = "public")
@AuditTable(value = "users_audit", schema = "public")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

@ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "company_id")
    @NotAudited
    private Company company;
}
```

По умолчанию создадутся таблицы сущность + _aud: “users_aud”, можно переопределить название таблицы с помощью_ `_@AuditTeable(value =””, schema = “”)_`

![[images/Untitled 15 7.png|Untitled 15 7.png]]

В каждой аудит таблице есть rev, который ссылается на общую таблицу rev, в refinfo создается запись о ревизии, и к ней прикрепляются все сущности, которые были созданы или изменены в рамках этой ревизии. reststmp это TimeStamp, то есть хранится информация о дате выполнения транзакции.

![[images/Untitled 16 6.png|Untitled 16 6.png]]

В аудит таблице хранится информации об id сущности ее полях, а также `revtype`, который предоставляет информацию о том, какое конкретное действие было выполнено над сущностью в определенной ревизии аудита.

- `0`: Добавление новой сущности.
- `1`: Обновление существующей сущности.
- `2`: Удаление сущности.

  

==**ВАЖНО:**==

Аннотация `@Audited` из Hibernate Envers используется для включения аудита для сущности. Однако, иногда возникают ситуации, когда вы хотите включить аудит для сущности, но не включать аудит для связанных сущностей. Для этого используется параметр `target` аннотации `@Audited`.

Аннотация `@Audited(target = RelationTargetAuditMode.NOT_AUDITED)` применяется к свойствам сущности, которые являются ссылками на другие сущности. Она говорит Hibernate Envers, что хотя данное свойство может ссылаться на другие сущности, аудит для этих связанных сущностей не должен включаться.

Потому что если просто поставить @NotAudited то поле не будет включено в таблицу аудирования.

```Java
@Entity
@Audited
public class Order {
    @Id
    private Long id;

    private String customerName;

    @ManyToOne
    @Audited(target = RelationTargetAuditMode.NOT_AUDITED)
    private Product product;

    // Другие поля и методы сущности
}
```

В данном примере аудит будет включен для сущности `Order`, но свойство `product`, являющееся ссылкой на сущность `Product`, не будет аудитироваться. Это может быть полезно, если вы хотите отслеживать изменения в заказах, но не хотите включать в аудит изменения в продуктах, на которые ссылаются эти заказы.

Таким образом, аннотация `@Audited(target = RelationTargetAuditMode.NOT_AUDITED)` позволяет тонко настроить аудит для связанных сущностей в Hibernate Envers.

ПРИМЕР:

```Java
@Entity
@Audited(targetAuditMode = RelationTargetAuditMode.NOT_AUDITED)
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Table(name = "users")
@AuditTable(value = "users_audit", schema = "public")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "company_id")
    private Company company;
}

// НЕТ @Audited
@Entity
@NoArgsConstructor
@AllArgsConstructor
@Builder
@AuditTable(value = "company_audit", schema = "public")
public class Company {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

```Java
Company company = Company.builder().name("Google").build();
            User user = User.builder().name("Vadim").company(company).build();
            session.beginTransaction();
            session.persist(company);
            session.flush();
            session.persist(user);
```

То есть засчет этого мода удалось добиться включения информации о поле company, но в тоже время не была создана аудирующая таблица для Company, только users_audit

![[images/Untitled 17 6.png|Untitled 17 6.png]]

  

  

### Revision

Revisions - это мощный инструмент Hibernate Envers, который позволяет вам выполнять запросы к историческим данным, а также получать информацию о ревизиях и изменениях в данных. Давайте рассмотрим некоторые основные аспекты и примеры запросов к историческим данным с использованием Revisions:

1. **Получение ревизий**:
    
    Вы можете получить список всех ревизий для определенной сущности. Это можно сделать с помощью `AuditReader.getRevisions(Class<?> entityClass, Object id)`.
    
    Пример:
    
    ```Java
    AuditReader auditReader = AuditReaderFactory.get(entityManager);
    List<Number> revisions = auditReader.getRevisions(User.class, 2L); //2L - userId
    for (Number revision : revisions) {
        System.out.println("Ревизия: " + revision);
    }
    ```
    
    Этот код вернет список номеров ревизий для сущности `User` с указанным `userId`.
    
2. **Получение данных на определенную ревизию**:
    
    Вы можете получить данные сущности на конкретной ревизии с помощью `AuditReader.find(Class<?> entityClass, Object id, Number revision)`.
    
    Пример:
    
    ```Java
    AuditReader auditReader = AuditReaderFactory.get(entityManager);
    User user = auditReader.find(User.class, userId, revisionNumber);
    ```
    
    Этот код вернет состояние сущности `User` на указанной ревизии.
    
    ВАЖНО: условие проверки ревизии ≤ NumberRevision, соответственно, если есть три ревизии, но запрашиваем 4, вернется максимальное значение, то есть 3 ревизию.
    
3. **Получение изменений между ревизиями**:
    
    Вы можете получить изменения в сущности между двумя ревизиями с помощью `AuditReader.createQuery().forRevisionsOfEntity(Class<?> entityClass, true, true).add(AuditEntity.id().eq(id)).add(AuditEntity.revisionNumber().between(revision1, revision2)).getResultList()`.
    
    Пример:
    
    ```Java
    AuditReader auditReader = AuditReaderFactory.get(entityManager);
    List<Object[]> revisions = auditReader.createQuery()
        .forRevisionsOfEntity(User.class, true, true)
        .add(AuditEntity.id().eq(userId))
        .add(AuditEntity.revisionNumber().between(revision1, revision2))
        .getResultList();
    
    for (Object[] revision : revisions) {
        User user = (User) revision[0];
        RevisionType revisionType = (RevisionType) revision[1];
        // Обработка изменений
    }
    ```
    
    Этот код вернет список изменений, произошедших в сущности `User` между указанными ревизиями.
    
4. **Получение информации о типе изменения**:
    
    Вы можете получить информацию о типе изменения (добавление, обновление или удаление) с помощью `RevisionType`, который возвращается при запросе изменений между ревизиями.
    
    Пример:
    
    ```Java
    if (revisionType == RevisionType.ADD) {
        System.out.println("Добавление сущности");
    } else if (revisionType == RevisionType.MOD) {
        System.out.println("Обновление сущности");
    } else if (revisionType == RevisionType.DEL) {
        System.out.println("Удаление сущности");
    }
    ```
    
    Этот код позволяет определить тип изменения для каждой ревизии.
    

Revisions в Hibernate Envers обеспечивают богатые возможности для анализа исторических данных в вашей базе данных. Вы можете выполнять разнообразные запросы и анализировать изменения в данных с использованием API Hibernate Envers, что делает его мощным инструментом для отслеживания и аудита данных.

### Сustom revision table

Чтобы создать кастомную таблицу для хранения аудиторских данных через revision entity в Hibernate Envers осуществляется с помощью аннотаций : `@RevisionEntity.` Данная сущность должна обязательно содержать `@RevisionNumber` и `@RevisionTimeStamp`

```Java
@Data
@AllArgsConstructor
@NoArgsConstructor
@RevisionEntity(value = MyRevisionListener.class) //может существовать только одна в проекте

public class MyRevision {

    @RevisionNumber
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @RevisionTimestamp
    private Long timestamp;

    private String user;
}
```

Можно отнаследоваться от DefaultRevisionEntity, данный класс уже содержит данные поля.

```Java
import org.hibernate.envers.DefaultRevisionEntity;
import org.hibernate.envers.RevisionEntity;

import javax.persistence.Entity;

@Entity
@RevisionEntity(CustomRevisionListener.class) // Опционально, если вы хотите использовать кастомный слушатель
public class MyRevision extends DefaultRevisionEntity {
   
 private String username; // Ваше кастомное поле

}
```

Важно не забыть добавить сущность в конфигурацию гибера.

```Java
configuration.addAnnotatedClass(MyRevision.class);
```

Создать необходимую таблицу

```SQL
CREATE TABLE custom_revision_entity (
    id BIGINT NOT NULL,
    revision_date TIMESTAMP,
    username VARCHAR(255),
    -- Другие поля, определенные в CustomRevisionEntity
    PRIMARY KEY (id)
);
```

Теперь у вас должна быть настроена кастомная таблица для хранения аудиторских данных через revision entity. Она будет содержать поля, определенные в вашей кастомной сущности, и будет использоваться Hibernate Envers для отслеживания изменений в данных.

### Second Level Cache

Second Level Cache - это механизм кэширования данных, который находится на уровне всего приложения (или sessionFactory).

В моем коде я использовал EhCache v3  
  
[https://www.ehcache.org/documentation/3.0/](https://www.ehcache.org/documentation/3.0/)

First Level Cache включен всегда, в отличие от второго уровня, его необходимо дополнительно включать.

![[images/Untitled 18 5.png|Untitled 18 5.png]]

- **Cache Hit (Попадание в кэш)**: Событие, когда данные успешно найдены в кэше.
- **Cache Miss (Промах кэша)**: Событие, когда данные не найдены в кэше и приложение обращается к источнику данных.
- **Cache Put (Добавление в кэш)**: Событие, при котором данные добавляются в кэш для будущего использования.

Как включить:

Данный пример с использованием `[**Hibernate ORM Hibernate JCache**](https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-jcache)``**.**` **Эта зависимость предоставляет API для работы с кэшем.**

```XML
<!-- https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-jcache -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-jcache</artifactId>
    <version>6.3.0.CR1</version>
</dependency>
```

Также необходимо в конфигурации прописать настройки для второго уровня кэширования и какую фабрику использовать.

```XML
<property name="hibernate.cache.use_second_level_cache">true</property>
<property name="hibernate.cache.region.factory_class">org.hibernate.cache.jcache.internal.JCacheRegionFactory</property>
```

Существует две аннотации `@Cacheable` из JPA и `@org.hibernate.annotations.Cache`

Разница между ними заключается в том, что @Cacheable не принимает в себя какие либо параметры, он просто указывает, что сущность может быть закэширована на втором уровне.

Аннотация `@org.hibernate.annotations.Cache` является аннотацией Hibernate и используется для настройки кэширования объектов. Вот подробное описание параметров этой аннотации:

1. `usage()`: Параметр `usage` определяет стратегию использования кэша. Это обязательный параметр и должен быть указан. В Java это поле типа `CacheConcurrencyStrategy`. Hibernate предоставляет различные стратегии кэширования, такие как `READ_ONLY`, `NONSTRICT_READ_WRITE`, `READ_WRITE`, и другие. Каждая стратегия определяет, как будет использоваться кэш (например, только для чтения, с поддержкой записи и т. д.). Выбор стратегии зависит от требований к твоему приложению.
2. `region()`: Параметр `region` позволяет указать имя региона кэша, в котором будут храниться объекты. Это необязательный параметр. Если он не указан, Hibernate будет использовать имя класса для региона по умолчанию. Регион - это логическое имя для группировки объектов в кэше.
3. `includeLazy()`: Параметр `includeLazy` определяет, должны ли лениво загруженные (lazy-loaded) свойства объектов быть кэшированными. Это булево значение (true или false). Если установлено в `true` (значение по умолчанию), то лениво загруженные свойства также будут кэшироваться. Если установлено в `false`, лениво загруженные свойства не будут включены в кэш.
4. `include()`: **[УСТАРЕВШИЙ]** Этот параметр был помечен как устаревший (deprecated) и больше не рекомендуется использовать. Он позволял указать, какие атрибуты объекта должны быть включены в кэш. Вместо этого, рекомендуется использовать более гибкие стратегии кэширования, предоставляемые параметром `usage()`.

### CacheConcurrencyStrategy

`CacheConcurrencyStrategy` - это перечисление (enum) в Hibernate, которое определяет стратегии кэширования для сущностей (Entity) и коллекций. Эти стратегии определяют, как Hibernate должен управлять кэшированием объектов и какие операции с объектами будут поддерживаться в кэше. Вот основные стратегии кэширования, доступные в `CacheConcurrencyStrategy`:

1. `READ_ONLY`: Эта стратегия подходит для объектов, которые никогда не изменяются после того, как они были загружены из базы данных. Все операции на чтение (SELECT) могут быть выполнены из кэша, но изменения не поддерживаются. Это подходит для данных, которые редко меняются и должны быть быстро доступными для чтения.
2. `NONSTRICT_READ_WRITE`: Эта стратегия поддерживает чтение и запись данных в кэше, но с более осторожным подходом к синхронизации. Данные могут быть читаемыми из кэша без блокировки записи, что делает эту стратегию более подходящей для данных, которые часто читаются, но редко изменяются. Она может позволять небольшие задержки в обновлении кэша.
3. `READ_WRITE`: Эта стратегия поддерживает как чтение, так и запись данных в кэше и обеспечивает более строгую синхронизацию записи в кэш. Это подходит для данных, которые часто читаются и изменяются, и обеспечивает более надежное управление кэшированием.
4. `TRANSACTIONAL`: Эта стратегия предоставляет транзакционное управление кэшем. Данные в кэше будут обновляться в рамках транзакций. Это подходит для сценариев, где требуется строгая синхронизация между кэшем и базой данных.

Выбор стратегии кэширования зависит от требований к приложению и характера данных. Например, если у тебя есть данные, которые редко меняются, и ты хочешь максимизировать производительность чтения, то `READ_ONLY` может быть подходящей стратегией. С другой стороны, если данные часто изменяются и важна согласованность между кэшем и базой данных, то `READ_WRITE` или `TRANSACTIONAL` могут быть более подходящими.

Пример использования `CacheConcurrencyStrategy` в аннотации `@org.hibernate.annotations.Cache`:

```Java
@Entity
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Product {
    // Сущность Product
}
```

### Пример работы кэша второго уровня

Есть сущности User и Company.

```Java
@Entity
@Audited(targetAuditMode = RelationTargetAuditMode.NOT_AUDITED)
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Table(name = "users")
@AuditTable(value = "users_audit", schema = "public")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "company_id")
    private Company company;
}

@Entity
@NoArgsConstructor
@AllArgsConstructor
@Builder
@AuditTable(value = "company_audit", schema = "public")
public class Company {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

Если выполнить следующий код в рамках двух сессий. То Hibernate выполнит два select на получение user под id 1L. В первой сессии запрос на получение юзера будет единожды, так как тот же экземпляр будет храниться на в PersistentContext сессии ( первый уровень кэша). И второй select будет в рамках второй сессии. Так как у каждой сессии свой PersistentContext.

```Java
try (SessionFactory factory = HibUtil.buildSessionFactory()) {
            User user = null;
            try (var session = factory.openSession()) {
                session.setCacheMode(CacheMode.NORMAL);
                session.beginTransaction();
                user = session.find(User.class, 1L);
                var user2 = session.find(User.class, 1L);
                session.getTransaction().commit();
            }
            try (var session = factory.openSession()) {
                session.setCacheMode(CacheMode.NORMAL);
                session.beginTransaction();
                var user3 = session.find(User.class, 1L);
                session.getTransaction().commit();
            }
        }
```

Добавление к сущности `@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.``_READ_WRITE_``)`

```Java
@Entity
@Audited(targetAuditMode = RelationTargetAuditMode.NOT_AUDITED)
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Table(name = "users")
@AuditTable(value = "users_audit", schema = "public")
@Data
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "company_id")
    private Company company;
}
```

Теперь экземпляр сущности User c id 1L будет помещаться сначала в кэш второго уровня, а уже после этого добавляться в кэш первого уровня сессий. То есть мы сократим количество запросов с 2 до 1.

Аналогично, если запросить в отдельных сессиях одну и ту же компанию, то Hibernate сделает отдельные запросы к БД.

Но можно также проставить аннотацию @Cache над сущностью Company, чтобы он также сохранялась в кэш второго уровня.

```Java
@Entity
@NoArgsConstructor
@AllArgsConstructor
@Builder
@AuditTable(value = "company_audit", schema = "public")
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Company {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

ВАЖНО: Хоть данные хранятся в кэше, создаются новые объекты, доставаясь из кэша второго уровня.( Десериализуются)

![[images/Untitled 19 5.png|Untitled 19 5.png]]

Если необходимо кэшировать коллекцию, то нужно проставлять `@Cache` не только над связываемой сущностью, но и над полем, которое представляет собой коллекцию, иначе будут в кэше храниться частичные данные и все равно придется выпонлять доп запросы к БД.

```Java
@Entity
@Cacheable
public class Order {
    @Id
    private Long id;
    
    // Кэширование коллекции заказанных продуктов
    @OneToMany(mappedBy = "order")
    @Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
    private List<OrderItem> orderItems = new ArrayList<>();
    
    // Другие поля и методы
}

@Entity
@Cacheable
public class OrderItem {
    @Id
    private Long id;
    
    // Ссылка на продукт
    @ManyToOne
    @Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
    private Product product;
    
    // Другие поля и методы
}
```

### Regions

Regions - это по своей сути области памяти в кэше второго уровня. По умолчанию, если мы не задаем свои области. Название Region формируется из пути к сущности или полю, над которыми поставлена аннотация @Cache.

Выполнив код в дебаге, видим Region для User сущности по умолчанию и кастомный companyCache для сущности Company

```Java

factory.getCache();// Это SessionFactory
```

![[images/Untitled 20 5.png|Untitled 20 5.png]]

```Java
@Entity
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE, region = "companyCache")
public class Company {
...
}
```

Чтобы настроить пул кэша необходимо следовать данной инструкции ( если используете ehcache v3) :

[https://www.ehcache.org/documentation/3.0/xml.html](https://www.ehcache.org/documentation/3.0/xml.html)

Пример для сущностей User и Company

```XML
<ehcache:config xmlns:ehcache="http://www.ehcache.org/v3"
                xmlns:jcache="http://www.ehcache.org/v3/jsr107">

    <!-- Определение региона для сущности User -->
    <ehcache:cache alias="userCache">
        <!-- Указываем тип ключа -->
        <ehcache:key-type>java.lang.Long</ehcache:key-type>

        <!-- Указываем тип значения -->
        <ehcache:value-type>com.example.User</ehcache:value-type>

        <!-- Время жизни элементов в секундах -->
        <ehcache:expiry>
            <ehcache:ttl unit="seconds">10</ehcache:ttl>
        </ehcache:expiry>

        <!-- Максимальное количество элементов в кэше -->
        <ehcache:heap unit="entries">1000</ehcache:heap>
    </ehcache:cache>

    <!-- Определение региона для сущности Company -->
    <ehcache:cache alias="companyCache">
        <!-- Указываем тип ключа -->
        <ehcache:key-type>java.lang.Long</ehcache:key-type>

        <!-- Указываем тип значения -->
        <ehcache:value-type>com.example.Company</ehcache:value-type>

        <!-- Время жизни элементов в секундах -->
        <ehcache:expiry>
            <ehcache:ttl unit="seconds">10</ehcache:ttl>
        </ehcache:expiry>

        <!-- Максимальное количество элементов в кэше -->
        <ehcache:heap>1000</ehcache:heap>
    </ehcache:cache>

</ehcache:config>
```

  

  

  

### Query Cache (кэш запросов SQL)

Чтобы включить кэш запросов необходимо указать в конфигурации следующую property:

```XML
<property name="hibernate.cache.use_query_cache">true</property>
```

Сохранение в кэш query осуществляется через setCacheable(), также можно задать region

```Java
String s = "select u from User u where u.id = :userId";
                Query<User> query = session.createQuery(s, User.class)
                        .setParameter("userId", 1).
                        setCacheable(true)
                        .setCacheRegion("cacheForusers");
```

  

  

### DAO and Repository

![[images/Untitled 21 5.png|Untitled 21 5.png]]

```Java
package com.konovalov.dao;

import com.konovalov.entities.BaseEntity;

import java.io.Serializable;
import java.util.List;
import java.util.Optional;

public interface Repository<K extends Serializable, E extends BaseEntity<K>> {
// K - ключ, E -сущность наследуемая от BaseEntity
    E save(E entity);

    void delete(K id);

    void update(E entity);

    Optional<E> findById(K id);

    List<E> findAll();
}
```

BaseEntity:

```Java
package com.konovalov.entities;

import java.io.Serializable;

public interface BaseEntity<T extends Serializable> {
    void setId(T id);

    T getId();
}
```

Чтобы не имплементировать в каждую DAO-сущность базовый интерфейс Repository и реализовать одни и те же по своей сути методы, можно вынести обобщенную реализацию этих методов в абстрактный класс RepositoryBase

```Java
package com.konovalov.dao;

import com.konovalov.entities.BaseEntity;
import jakarta.persistence.criteria.CriteriaQuery;
import lombok.RequiredArgsConstructor;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.query.criteria.HibernateCriteriaBuilder;

import java.io.Serializable;
import java.util.List;
import java.util.Optional;

@RequiredArgsConstructor // нужен для создания конструктора, включающий в себя приватные поля
public abstract class RepositoryBase<K extends Serializable, E extends BaseEntity<K>> implements Repository<K, E> {
    private final SessionFactory sessionFactory; // вскоре отойдем в примерах от пула, до конкретной сессии
    private final Class<E> clazz; // нужен для обобщенной реализации метода, где необходимо передавать сущность в createQuery

    @Override
    public E save(E entity) {
        Session session = sessionFactory.getCurrentSession();
        session.persist(entity);
        return entity;
    }

    @Override
    public void delete(K id) {
        Session session = sessionFactory.getCurrentSession();
        session.remove(id);
        session.flush();

    }

    @Override
    public void update(E entity) {
        Session session = sessionFactory.getCurrentSession();
        session.merge(entity);
    }

    @Override
    public Optional<E> findById(K id) {
        Session session = sessionFactory.getCurrentSession();
        var maybeUser = session.find(clazz, id);
        return Optional.ofNullable(maybeUser);
    }

    @Override
    public List<E> findAll() {
        Session session = sessionFactory.getCurrentSession();
        HibernateCriteriaBuilder builder = session.getCriteriaBuilder();
        CriteriaQuery<E> criteriaQuery = builder.createQuery(clazz);
        criteriaQuery.from(clazz);

        return session.createQuery(criteriaQuery)
                .getResultList();
    }
}
```

В данных методах не стоит открывать новые сессии через openSession, так как быстро израсходуется пул доступных соединений.

Также не стоит использовать @Cleanup в сочетании с openSession() так как возникнет LazyInitializationException

```Java
    @Override
    public Optional<E> findById(K id) {
        @Cleanup Session session = sessionFactory.openSession(); //так нельзя!
        var maybeUser = session.find(clazz, id);
        return Optional.ofNullable(maybeUser);
    }
```

Но встает проблема, как получить доступ к конкретной сесиии, для этого существует специальный интерфейс CurrentSessionContext, который предоставляет Hibernate.

![[images/Untitled 22 5.png|Untitled 22 5.png]]

### CurrentSessionContext

`CurrentSessionContext` - это интерфейс в Hibernate, который определяет контракт для управления сессиями в контексте текущей исполнительной среды. Этот интерфейс предоставляет методы для получения текущей сессии в зависимости от сценария исполнения.

Например, для реализации `CurrentSessionContext`, Hibernate предоставляет следующие классы:

1. `ThreadLocalSessionContext`: Этот класс реализует `CurrentSessionContext` и использует `ThreadLocal` для хранения текущей сессии в пределах одного потока. Это позволяет каждому потоку иметь свою собственную сессию. Пример использования:
    
    ```Java
    Session session = sessionFactory.getCurrentSession();
    ```
    
2. `JTASessionContext`: Этот класс используется в среде, интегрированной с Java Transaction API (JTA). Он позволяет управлять сессиями в контексте JTA транзакций.
3. `ManagedSessionContext`: Этот класс предназначен для использования в управляемых средах, таких как Java EE контейнеры. Он обеспечивает управление жизненным циклом сессии в контейнерной среде.
4. Вы также можете создать собственные реализации c помощью `AbstractCurrentSessionContext` если ваши требования отличаются от стандартных сценариев использования.

Чтобы указать Hibernate, какой контекст использовать необходимо в конфигурации hibernate.cfg.xml указать следующую property

```Java
<property name="current_session_context_class">thread</property>
```

- `thread` для ThreadLocalSessionContext
- `jta` для JTASessionContext
- `managed` для ManagedSessionContext

В моем примере используется ThreadLocalSessionContext согласно документации:

impl actually generate a session upon first request and then clean it up after the Transaction associated with that session is committed/rolled-back. In order for ensuring that happens, the sessions generated here are unusable until after Session.beginTransaction() has been called. If close() is called on a session managed by this class, it will be automatically unbound.

Этот контекст управляет получением или открытием текущей сессии и автоматическим ее закрытием после завершения транзакции.

```Java
try (SessionFactory factory = HibUtil.buildSessionFactory()) {
            var session = factory.getCurrentSession(); // не нужно оборачивать в try-with-resources или прописывать @Cleanup
            session.beginTransaction();
            var userRep = new UserRepository(factory);
            userRep.findById(1L).ifPresent(System.out::println);
            session.getTransaction().commit();
        }
```

То есть обязательно начинать транзакцию и закрывать ее, так как текущая сессия помещается в переменную выбранного контекста. Если закомментить старт транзакции и ее закрытие выскочит следующая ошибка

```Java
Exception in thread "main" org.hibernate.HibernateException:
Calling method 'find' is not valid without an active transaction
(Current status: NOT_ACTIVE)
```

Чтобы отойти от внедрения зависимости sessionFactory в Repository используем entityManager

```Java
package com.konovalov.dao;

import com.konovalov.entities.BaseEntity;
import jakarta.persistence.EntityManager;
import jakarta.persistence.criteria.CriteriaQuery;
import lombok.RequiredArgsConstructor;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.query.criteria.HibernateCriteriaBuilder;

import java.io.Serializable;
import java.util.List;
import java.util.Optional;

@RequiredArgsConstructor
public abstract class RepositoryBase<K extends Serializable, E extends BaseEntity<K>> implements Repository<K, E> {
    private final EntityManager entityManager;
    private final Class<E> clazz;

    @Override
    public E save(E entity) {
        entityManager.persist(entity);
        return entity;
    }

    @Override
    public void delete(K id) {
        entityManager.remove(id);
        entityManager.flush();

    }

    @Override
    public void update(E entity) {
        entityManager.merge(entity);
    }

    @Override
    public Optional<E> findById(K id) {
        var maybeUser = entityManager.find(clazz, id);
        return Optional.ofNullable(maybeUser);
    }

    @Override
    public List<E> findAll() {
        var builder = entityManager.getCriteriaBuilder();
        CriteriaQuery<E> criteriaQuery = builder.createQuery(clazz);
        criteriaQuery.from(clazz);

        return entityManager.createQuery(criteriaQuery)
                .getResultList();
    }
}
```

Теперь в EntityRepository необходимо в конструктор передавать EntityManager:

```Java
public class UserRepository extends RepositoryBase<Long, User> {

    public UserRepository(EntityManager entityManager) {
        super(entityManager, User.class);
    }
    
    //todo другой функционал
}
```

  

Нужно создать proxy, который самостоятельно будет обращаться к SessionFactory, получать текующую открытую сессию для нашего репозитория. Мы не можем использовать одну и ту же сессию, так как она открываается и закрывается, и если в рамках другого потока мы обратимся к сессии, она может быть уже закрыта и возникнет ошибка.

Так будет работать в многопоточности и достаточно создать один экземпляр UserRepository.

```Java
try (SessionFactory factory = HibUtil.buildSessionFactory()) {
            var session = (Session) Proxy.newProxyInstance(SessionFactory.class.getClassLoader(), new Class[]{Session.class}
                    , ((proxy, method, args1) -> method.invoke(factory.getCurrentSession(), args1)));
            session.beginTransaction();
            var userRep = new UserRepository(session);
            userRep.findById(1L).ifPresent(System.out::println);
            session.getTransaction().commit();
        }
```

### уровень Service

Уровень service не работает напрямую с БД. Поиск и удаление в данном примере осуществляется через userService→userRep.

```Java
package com.konovalov.service;

import com.konovalov.dao.UserRepository;
import com.konovalov.dto.UserReadDto;
import com.konovalov.entities.User;
import com.konovalov.mapper.UserReadMapper;
import lombok.RequiredArgsConstructor;
import org.hibernate.graph.GraphSemantic;

import java.util.Map;
import java.util.Optional;

@RequiredArgsConstructor

public class UserService {
    private final UserReadMapper mapper;
    private final UserRepository userRepository;

    public Optional<UserReadDto> findById(Long id) {
        Map<String, Object> properties = Map.of(GraphSemantic.LOAD.getJakartaHintName(), userRepository.getEntityManager().getEntityGraph("withCompany"));
        return userRepository.findById(id,properties)
                .map(mapper::mapFrom);
    }

    public boolean delete(Long id) {
        Optional<User> maybeUser = userRepository.findById(id);
        maybeUser.ifPresent(user -> userRepository.delete(user.getId()));
        return maybeUser.isPresent();
    }
}
```

А уже UserRepository наследуясь от RepositoryBase осуществляет доступ к БД через entityManager.

  

Обрати внимание, что в в методе findById в UserService происходит маппинг сущности User в UserReadDto, который содержит CompanyReadDto

```Java
package com.konovalov.dto;

public record UserReadDto(Long id, String name, CompanyReadDto company) {
}

package com.konovalov.dto;

public record CompanyReadDto(Long id, String name) {
}
```

Маппинг осуществляется с помощью UserReadMapper и CompanyReadMapper, которые имплеменитуют интерфейс Mapper. Написаны вручную, но можно использовать MapStruct

```Java
package com.konovalov.mapper;

public interface Mapper<F,T> {

    T mapFrom(F object);
}
```

```Java
package com.konovalov.mapper;

import com.konovalov.dto.UserReadDto;
import com.konovalov.entities.User;
import lombok.AllArgsConstructor;
import lombok.RequiredArgsConstructor;

import java.util.Optional;
@RequiredArgsConstructor
public class UserReadMapper implements Mapper<User, UserReadDto> {

    private final CompanyReadMapper companyReadMapper;

    @Override
    public UserReadDto mapFrom(User user) {
        return new UserReadDto(
                user.getId(),
                user.getName(),
                Optional.ofNullable(user.getCompany()).map(companyReadMapper::mapFrom)
                        .orElse(null)
                //поэтому необходимо делать Optional либо NotNull constraints
        );
    }
}
```

```Java
package com.konovalov.mapper;

import com.konovalov.dto.CompanyReadDto;
import com.konovalov.entities.Company;

public class CompanyReadMapper implements Mapper<Company, CompanyReadDto> {
    @Override
    public CompanyReadDto mapFrom(Company object) {
        return new CompanyReadDto(
                object.getId(),
                object.getName()
        );
    }
}
```

После этого инжектируем все зависимости и вызывая через сервис поиск сущности по айди- получаем userReadDto

```Java
try (SessionFactory factory = HibUtil.buildSessionFactory()) {
            var session = (Session) Proxy.newProxyInstance(SessionFactory.class.getClassLoader(), new Class[]{Session.class}
                    , ((proxy, method, args1) -> method.invoke(factory.getCurrentSession(), args1)));
            session.beginTransaction();

            CompanyReadMapper companyReadMapper = new CompanyReadMapper();
            UserReadMapper userReadMapper = new UserReadMapper(companyReadMapper);
            var userRep = new UserRepository(session);
            UserService userService = new UserService(userReadMapper, userRep);
            userService.findById(1L).ifPresent(System.out::println);
            session.getTransaction().commit();
        }
```

```SQL
Hibernate: 
    select
        u1_0.id,
        c1_0.id,
        c1_0.name,
        u1_0.name 
    from
        users u1_0 
    left join
        company c1_0 
            on c1_0.id=u1_0.company_id 
    where
        u1_0.id=?
UserReadDto[id=1, name=Vadim, company=CompanyReadDto[id=1, name=Google]]
```

Один запрос выполнился за счет применения графа в сервисе

```Java
@NamedEntityGraph(name = "withCompany",
        attributeNodes = {@NamedAttributeNode("company")
        })
public class User implements BaseEntity<Long> {




public Optional<UserReadDto> findById(Long id) {
        Map<String, Object> properties = Map.of(GraphSemantic.LOAD.getJakartaHintName(), userRepository.getEntityManager().getEntityGraph("withCompany"));
        return userRepository.findById(id,properties)
                .map(mapper::mapFrom);
    }
```

Чтобы использовать properties в RepositoryBase мы изменили аргументы метода findById

```Java
@Override
    public Optional<E> findById(K id, Map<String, Object> properties) {
        return Optional.ofNullable(entityManager.find(clazz, id,properties));
    }
```

А в интерфейсе определен default метод, который принимает один аргумент, но вызывает второй и передает туда в качестве второго аргумента пустую карту properties. Так как мы не всегда захотим передавать properties

```Java
default Optional<E> findById(K id) {
        return findById(id, emptyMap());
}

Optional<E> findById(K id, Map<String, Object> properties);
```

Аналогично можно создать сохранение сущности

```Java
public class UserService {
    private final UserRepository userRepository;
    private final UserCreateMapper userCreateMapper;
    public Long create(UserCreateDto userDto){
        //  здесь должна быть validation, но ее нет
       
        User user = userCreateMapper.mapFrom(userDto); //map

        return  userRepository.save(user).getId();
    }
}


@RequiredArgsConstructor
public class UserCreateMapper implements Mapper<UserCreateDto, User> {
    private final CompanyRepository companyRepository;
    @Override
    public User mapFrom(UserCreateDto object) {
        return User.builder().name(object.name())
                .company(companyRepository.findById(object.companyId()).orElseThrow(IllegalArgumentException::new))
                .build();
    }
}

@Builder
public record UserCreateDto(Long id, String name, Long companyId) {
}
```

  

Чтобы не управлять в ручную транзакциями

```Java
session.beginTransaction();
...
session.getTransaction().commit();
```

Можно создать Interceptor, который будет перехватывать методы Service, помеченные аннотацией @Transactional

```Java
public class UserService {
    private final UserReadMapper mapper;
    private final UserRepository userRepository;
    private final UserCreateMapper userCreateMapper;
@Transactional
    public Long create(UserCreateDto userDto){
        // validation
        //map
        User user = userCreateMapper.mapFrom(userDto);

        return  userRepository.save(user).getId();
    }
    @Transactional
    public Optional<UserReadDto> findById(Long id) {
        Map<String, Object> properties = Map.of(GraphSemantic.LOAD.getJakartaHintName(), userRepository.getEntityManager().getEntityGraph("withCompany"));
        return userRepository.findById(id,properties)
                .map(mapper::mapFrom);
    }
    @Transactional
    public boolean delete(Long id) {
        Optional<User> maybeUser = userRepository.findById(id);
        maybeUser.ifPresent(user -> userRepository.delete(user.getId()));
        return maybeUser.isPresent();
    }
}
```

Best Practice поставить над классом и передатьь readOnly = true

```Java
@Transactional(readOnly=true)
public class UserService {}
```

  

Intercetor:

```Java
package com.konovalov.interceptor;

import jakarta.transaction.Transactional;
import lombok.RequiredArgsConstructor;
import net.bytebuddy.implementation.bind.annotation.Origin;
import net.bytebuddy.implementation.bind.annotation.RuntimeType;
import net.bytebuddy.implementation.bind.annotation.SuperCall;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;

import java.lang.reflect.Method;
import java.util.concurrent.Callable;

@RequiredArgsConstructor
public class TransactionInterceptor {

    private final SessionFactory sessionFactory;

    @RuntimeType
    public Object intercept(@SuperCall Callable<Object> call, @Origin Method method) {
        Transaction transaction = null;
        boolean transactionStarted = false;
        if (method.isAnnotationPresent(Transactional.class)) {
            transaction = sessionFactory.getCurrentSession().getTransaction();
            if (!transaction.isActive()) {
                transaction.begin();
                transactionStarted = true;
            }
            Object result = null;
            try {
                result = call.call();
                if(transactionStarted){
                    transaction.commit();
                }
            } catch (Exception e) {
                if (transactionStarted) {
                    transaction.rollback();
                }
            }
            return result;
        }

        return null;
    }
}
```

`TransactionInterceptor` используется для перехвата методов с аннотацией `jakarta.transaction.Transactional` и управления Hibernate-транзакциями. Давай разберемся, что делает этот код и какие аннотации и методы используются.

1. `@RequiredArgsConstructor`: Эта аннотация добавляется с помощью проекта Lombok и генерирует конструктор, который принимает все обязательные параметры, такие как `SessionFactory`, и инициализирует их. Это упрощает создание экземпляра класса.
2. `@RuntimeType`: Эта аннотация из библиотеки Byte Buddy указывает, что метод `intercept` должен быть вызван во время выполнения (runtime).
3. `public Object intercept(@SuperCall Callable<Object> call, @Origin Method method)`: Этот метод является точкой входа для перехвата. Он принимает два параметра:
    - `call`: Это объект `Callable`, который представляет оригинальный вызываемый метод, который будет перехвачен.
    - `method`: Это объект `Method`, представляющий метод, который был аннотирован как `Transactional`.

Давай разберем, что происходит внутри метода `intercept`:

- Сначала создается переменная `transaction` типа `Transaction` и устанавливается в `null`. Это объект Hibernate, который представляет транзакцию базы данных.
- Создается булевая переменная `transactionStarted`, инициализированная значением `false`. Она будет использоваться для отслеживания, была ли начата транзакция.
- Затем выполняется проверка наличия аннотации `Transactional` на методе, переданном в параметре `method`. Если метод аннотирован этой аннотацией, то начинается управление транзакцией.
- Получается текущая сессия Hibernate с помощью `sessionFactory.getCurrentSession()`.
- Проверяется, активна ли уже транзакция с помощью `transaction.isActive()`. Если транзакция не активна, то она начинается с вызова `transaction.begin()`, и устанавливается флаг `transactionStarted` в `true`.
- Далее выполняется вызов оригинального метода, представленного объектом `call.call()`. Результат вызова сохраняется в переменной `result`.
- Если транзакция была начата (`transactionStarted` равно `true`), то она фиксируется с помощью `transaction.commit()`. Если произошла ошибка (исключение), то транзакция откатывается с помощью `transaction.rollback()`.
- В конце метод возвращает `result`.

Этот код позволяет автоматически управлять транзакциями для методов, аннотированных как `Transactional`. Если метод выполняется успешно, транзакция фиксируется, в противном случае она откатывается.

```Java
package com.konovalov;

import com.konovalov.dao.CompanyRepository;
import com.konovalov.dao.UserRepository;
import com.konovalov.dto.UserCreateDto;
import com.konovalov.interceptor.TransactionInterceptor;
import com.konovalov.mapper.CompanyReadMapper;
import com.konovalov.mapper.UserCreateMapper;
import com.konovalov.mapper.UserReadMapper;
import com.konovalov.service.UserService;
import com.konovalov.utils.HibUtil;
import jakarta.transaction.Transactional;
import net.bytebuddy.ByteBuddy;
import net.bytebuddy.implementation.MethodDelegation;
import net.bytebuddy.matcher.ElementMatchers;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Proxy;

public class Main {
    @Transactional
    public static void main(String[] args) {

        try (SessionFactory factory = HibUtil.buildSessionFactory()) {
            var session = (Session) Proxy.newProxyInstance(SessionFactory.class.getClassLoader(), new Class[]{Session.class}, ((proxy, method, args1) -> method.invoke(factory.getCurrentSession(), args1)));

            CompanyReadMapper companyReadMapper = new CompanyReadMapper();
            UserReadMapper userReadMapper = new UserReadMapper(companyReadMapper);
            CompanyRepository companyRepository = new CompanyRepository(session);
            UserCreateMapper userCreateMapper = new UserCreateMapper(companyRepository);
            var userRep = new UserRepository(session);
            
            TransactionInterceptor transactionInterceptor = new TransactionInterceptor(factory);
            UserService userService = new ByteBuddy().subclass(UserService.class)
                    .method(ElementMatchers.any())
                    .intercept(MethodDelegation.to(transactionInterceptor))
                    .make()
                    .load(UserService.class.getClassLoader())
                    .getLoaded().getDeclaredConstructor(UserReadMapper.class, UserRepository.class, UserCreateMapper.class)
                    .newInstance(userReadMapper, userRep, userCreateMapper);

            userService.findById(1L).ifPresent(System.out::println);
            UserCreateDto usCrDto = UserCreateDto.builder()
                    .name("Vadim4i4ek")
                    .companyId(1L)
                    .build();
            userService.create(usCrDto);
        } catch (InvocationTargetException | InstantiationException | IllegalAccessException | NoSuchMethodException e) {
            throw new RuntimeException(e);
        }
    }
}
```

Этот блок кода выполняет несколько действий:

1. `TransactionInterceptor transactionInterceptor = new TransactionInterceptor(factory);`: Создается экземпляр `TransactionInterceptor`, который используется для перехвата методов в сервисе `UserService`. Конструктор `TransactionInterceptor` принимает `SessionFactory` (`factory`), который будет использоваться для управления транзакциями.
2. `UserService userService = new ByteBuddy().subclass(UserService.class)`: Здесь создается динамический подкласс `UserService` с помощью библиотеки Byte Buddy. Это позволяет создать подкласс `UserService`, который будет иметь тот же интерфейс и методы, что и оригинальный `UserService`.
3. `.method(ElementMatchers.any())`: Указывается, что будут перехватываться все методы внутри `UserService`. Мы используем `ElementMatchers.any()`, чтобы указать, что перехватывать нужно все методы.
4. `.intercept(MethodDelegation.to(transactionInterceptor))`: Здесь указывается, что все перехваченные методы должны быть перенаправлены к объекту `transactionInterceptor`. Это означает, что каждый вызов метода `UserService` будет сначала перехвачен `TransactionInterceptor`, который будет управлять транзакциями, а затем вызывать оригинальный метод `UserService`.
5. `.make()`: Создается подкласс `UserService` с учетом всех настроек, указанных выше.
6. `.load(UserService.class.getClassLoader())`: Загружается класс `UserService` в текущий ClassLoader.
7. `.getLoaded().getDeclaredConstructor(UserReadMapper.class, UserRepository.class, UserCreateMapper.class)`: Получается конструктор созданного динамического подкласса `UserService`, который принимает параметры `UserReadMapper`, `UserRepository` и `UserCreateMapper`.
8. `.newInstance(userReadMapper, userRep, userCreateMapper)`: Создается экземпляр динамического подкласса `UserService`, используя полученный конструктор и передавая ему необходимые зависимости, такие как `userReadMapper`, `userRep` и `userCreateMapper`.

Таким образом, в результате выполнения этого блока кода мы получаем объект `userService`, который является экземпляром динамического подкласса `UserService`, с перехватом методов через `TransactionInterceptor`, что позволяет управлять транзакциями при вызове методов этого сервиса.

### Validation

Статья по основам [https://www.baeldung.com/java-validation](https://www.baeldung.com/java-validation)

Чтобы использовать hibernate валидацию необходимо внедрить зависимость

```Java
<!-- https://mvnrepository.com/artifact/org.hibernate.validator/hibernate-validator -->
    <dependency>
      <groupId>org.hibernate.validator</groupId>
      <artifactId>hibernate-validator</artifactId>
      <version>8.0.1.Final</version>
    </dependency>
```

После этого можно расставлять аннотации:

```Java
import jakarta.validation.constraints.NotNull;
import lombok.Builder;

@Builder
public record UserCreateDto(Long id, @NotNull String name, @NotNull Long companyId) {
}
```

Программно на уровне сервиса валидация происходит внутри метода

```Java
public class UserService {
    private final UserRepository userRepository;
    private final UserCreateMapper userCreateMapper;

    @Transactional
    public void create(UserCreateDto userDto) {
        @ Cleanup ValidatorFactory factory = Validation.byDefaultProvider()
                .configure()
                .messageInterpolator(new ParameterMessageInterpolator())
                .buildValidatorFactory();
        Validator validator = factory.getValidator();

        Set<ConstraintViolation<UserCreateDto>> violations = validator.validate(userDto);
        if (!violations.isEmpty()) {
            throw new ConstraintViolationException(violations);
        }
        User user = userCreateMapper.mapFrom(userDto);

        userRepository.save(user);
    }
```

В данном примере мы делаем name = null

```Java
UserCreateDto usCrDto = UserCreateDto.builder()
                    .name("Diman4ik")
                    .companyId(1L)
                    .build();
            userService.create(usCrDto);
```

В соответствии с аннотации из jakarta.validation @NotNull Interceptor перехватит ошибку валидации

```Java
package com.konovalov.interceptor;

import jakarta.transaction.Transactional;
import lombok.RequiredArgsConstructor;
import net.bytebuddy.implementation.bind.annotation.Origin;
import net.bytebuddy.implementation.bind.annotation.RuntimeType;
import net.bytebuddy.implementation.bind.annotation.SuperCall;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;

import java.lang.reflect.Method;
import java.util.concurrent.Callable;

@RequiredArgsConstructor
public class TransactionInterceptor {

    private final SessionFactory sessionFactory;

    @RuntimeType
    public Object intercept(@SuperCall Callable<Object> call, @Origin Method method) {
        Transaction transaction = null;
        Object result = null;
        boolean transactionStarted = false;
        if (method.isAnnotationPresent(Transactional.class)) {
            transaction = sessionFactory.getCurrentSession().getTransaction();
            if (!transaction.isActive()) {
                transaction.begin();
                transactionStarted = true;
            }

            try {
                result = call.call();
                if (transactionStarted) {
                    transaction.commit();
                }
            } catch (Exception e) {
                if (transactionStarted) {
                    transaction.rollback();
                }
            }
        }

        return result;
    }
}
```

![[images/Untitled 23 5.png|Untitled 23 5.png]]