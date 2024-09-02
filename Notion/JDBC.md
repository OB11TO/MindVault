## Общие сведения

![[images/Untitled 152.png|Untitled 152.png]]

JDBC (Java Database Connectivity) - это Java API (Application Programming Interface), который предоставляет возможность взаимодействия с базами данных с использованием языка программирования Java.

Для использования JDBC необходимо подключить соответствующий драйвер базы данных к Java-приложению. Драйверы JDBC обеспечивают специфическую реализацию JDBC API для конкретной СУБД.

![[images/Untitled 1 13.png|Untitled 1 13.png]]

В JDBC есть 3 основных интерфейса:

- **Connection** – отвечает за соединение с базой данных
- **Statement** – отвечает за запрос к базе данных
- **ResultSet** – отвечает за результат запроса к базе данных

## Connection, Statement, ResultSet, ResultSetMetaData, MetaData

### Connection

[https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html)

Connection это интерфейс extends Wrapper, AutoCloseable (необходимо закрывать либо оборачивать в try with resources)

Подключиться к БД можно следующим образом:

```Java
String url = "jdbc:postgresql://localhost:5432/postgres";
String userName = "vadimko99";
String password = "knopka9955";
try{
Connection connection = DriverManager.getConnection(url, userName, password);
}
catch(Exception e){}
finally{
connection.close();
}
```

Но по-хорошему стоит создать отдельный утилитарный класс следующим образом:

```Java
package org.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public final class ConnectionManager {
    private static final String URL = "jdbc:postgresql://localhost:5432/postgres";
    private static final String USER_NAME = "vadimko99";
    private static final String PASSWORD = "knopka9955";

    static {
        loadDriver();
    }

    private static void loadDriver() {
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    private ConnectionManager(){
    }

    public static Connection open() throws RuntimeException {
        try{
            return DriverManager.getConnection(URL, USER_NAME, PASSWORD);
        } catch (SQLException e){
            throw new RuntimeException(e);
        }

    }
}
```

```Java
package org.example;

import java.sql.Connection;

public class JDBCRunner {
    public static void main(String[] args) {
		try(Connection connection = ConnectionManager.open()){
}

    }
}
```

Еще лучше вынести все в resources/application.properties и вытаскивать значения оттуда с помощью PropertiesUtil

```Java
// application.properties

db.url=jdbc:postgresql://localhost:5432/postgres
db.username=vadimko99
db.password=knopka9955
```

```Java
import java.sql.Connection;

public class JDBCRunner {
    public static void main(String[] args) {
try(Connection connection = ConnectionManager.open()){
       }
    }
}
```

```Java
import example.util.PropertiesUtil;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public final class ConnectionManager {
    private static final String URL_KEY = "db.url";
    private static final String USER_NAME_KEY = "db.username";
    private static final String PASSWORD_KEY ="db.password";

    static {
        loadDriver();
    }

    private static void loadDriver() {
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    private ConnectionManager(){
    }

    public static Connection open() throws RuntimeException {
        try{
            return DriverManager.getConnection(
                    PropertiesUtil.get(URL_KEY),
                    PropertiesUtil.get(USER_NAME_KEY),
                    PropertiesUtil.get(PASSWORD_KEY));
        } catch (SQLException e){
            throw new RuntimeException(e);
        }

    }
}
```

```Java
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class PropertiesUtil {
    private static final Properties PROPERTIES = new Properties();

    static {
        loadProperties();
    }
    public static String get(String key){
        return PROPERTIES.getProperty(key);
    }

    private static void loadProperties() {
        try (InputStream stream = PropertiesUtil.class.getClassLoader().getResourceAsStream("application.properties")) {
            PROPERTIES.load(stream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private PropertiesUtil(){}

}
```

  

### Statement

[https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html](https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html)

Интерфейс `Statement` из пакета `java.sql` представляет собой механизм для отправки SQL-запросов к базе данных и получения результатов. Этот интерфейс является частью Java Database Connectivity (JDBC) API, который обеспечивает взаимодействие с различными реляционными базами данных.

Вот некоторые основные методы и их описание, доступные в интерфейсе `Statement`:

- `void execute(String sql)`: Выполняет указанный SQL-запрос, который может быть любым допустимым SQL-выражением (например, SELECT, INSERT, UPDATE). Возвращает `true`, если результат запроса является `ResultSet`, и `false` в противном случае.
- `ResultSet executeQuery(String sql)`: Выполняет указанный SQL-запрос, который должен быть запросом на выборку (например, SELECT). Возвращает объект `ResultSet`, содержащий результаты запроса.
- `int executeUpdate(String sql)`: Выполняет указанный SQL-запрос, который обновляет базу данных (например, INSERT, UPDATE, DELETE). Возвращает количество измененных строк.
- `boolean isClosed()`: Возвращает `true`, если `Statement` закрыт, и `false` в противном случае.
- `void close()`: Закрывает `Statement` и освобождает все связанные ресурсы.
- `void addBatch(String sql)`: Добавляет SQL-запрос в пакет для пакетной обработки.
- `void clearBatch()`: Очищает все ранее добавленные запросы из пакета.
- `int[] executeBatch()`: Выполняет все запросы в пакете и возвращает массив целых чисел, содержащий результаты для каждого запроса.
- `ResultSet getResultSet()`: Возвращает текущий `ResultSet`, если последний выполненный запрос был запросом на выборку.
- `int getUpdateCount()`: Возвращает количество измененных строк в последнем выполненном запросе.

### PreparedStatement extends Statement

[https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html)

1. Предварительная компиляция: PreparedStatement компилирует SQL-запрос на стороне базы данных при его создании. При выполнении запроса база данных может повторно использовать скомпилированный план выполнения, что приводит к более эффективной обработке запросов. ==В случае обычного Statement каждый sql запрос компилируется== непосредственно перед выполнением, что может вызывать накладные расходы на компиляцию для каждого запроса.
2. Параметризованные запросы: PreparedStatement позволяет использовать параметры в SQL-запросах. Параметры представляются в виде плейсхолдеров (например, "?") и могут быть заполнены значениями во время выполнения. Это позволяет безопасно передавать пользовательский ввод в запросы, предотвращая атаки типа ==**SQL-инъекции.**==
3. ==Улучшенная производительность:== При использовании PreparedStatement база данных ==может кэшировать скомпилированные планы выполнения запросов==, что приводит к более быстрой обработке последующих запросов. Это особенно полезно, если вы выполняете один и тот же запрос с различными значениями параметров.

```Java
// Переменные для примера
String firstName = "Harry";
String lastName = "Potter";
String phone = "+12871112233";
String email = "harry@example.com";

// Запрос с указанием мест для параметров в виде знака "?"
String sql = "INSERT INTO JC_CONTACT
(FIRST_NAME, LAST_NAME, PHONE, EMAIL)
VALUES (?, ?, ?, ?)";

// Создание запроса. Переменная con — это объект типа Connection
PreparedStatement stmt = con.prepareStatement(sql);

// Установка параметров
stmt.setString(1, firstName);
stmt.setString(2, lastName);
stmt.setString(3, phone);
stmt.setString(4, email);

// Выполнение запроса
stmt.executeUpdate();
```

```Java
public class JDBCRunner {
    public static void main(String[] args) throws SQLException {
        LocalDateTime start  = LocalDateTime.of(2023, Month.AUGUST, 15, 12, 0);
        LocalDateTime end  = LocalDateTime.of(2023, Month.SEPTEMBER, 1, 0, 0);
        List<Long> listOfFlights = getAllFlights(start,end);
    }

    private static List<Long> getAllFlights(LocalDateTime start, LocalDateTime end) throws SQLException {
        String sql = "SELECT * FROM tickets WHERE departure_date BETWEEN ? AND ?";

        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(sql)) {
            statement.setTimestamp(1, Timestamp.valueOf(start));
            statement.setTimestamp(1, Timestamp.valueOf(start));
            ResultSet resultSet = statement.executeQuery();
            List<Long> list = new ArrayList<>();
            while (resultSet.next()){
                list.add(resultSet.getLong("id"));
            }
            return list;
        }
    }
}
```

### CallableStatement extends PreparedStatement

[https://docs.oracle.com/javase/8/docs/api/java/sql/CallableStatement.html](https://docs.oracle.com/javase/8/docs/api/java/sql/CallableStatement.html)

JDBC есть еще один интерфейс для еще более сложных сценариев. Он унаследован от _PreparedStatement_ и называется _CallableStatement_.

Он используется для вызова (Call) хранимых процедур в базе данных. Особенность такого вызова в том, что кроме результата ResultSet такой хранимой процедуре можно еще и передать параметры.

Для создания объекта _CallableStatement_ предназначен метод Connection.prepareCall().

Вот представь, что у тебя есть хранимая процедура ADD, которая  
принимает параметры a, b и c. Эта процедура складывает a и b и помещает  
результат сложения в переменную с.  

```Java
// Подключение к серверу
Connection connection = DriverManager.getConnection("jdbc:as400://mySystem");

// Создание объекта CallableStatement. Он выполняет предварительную обработку
// вызова хранимой процедуры. Знаки вопроса
// указывают, где должны быть подставлены входные параметры, а где выходные
// Первые два параметра являются входными,
// а третий — выходным.
CallableStatement statement = connection.prepareCall("CALL MYLIBRARY.ADD (?, ?, ?)");

// Настройка входных параметров. Передаем в процедуру 123 и 234
statement.setInt (1, 123);
statement.setInt (2, 234);

// Регистрация типа выходного параметра
statement.registerOutParameter (3, Types.INTEGER);

// Запуск хранимой процедуры
statement.execute();

// Получение значения выходного параметра
int sum = statement.getInt(3);

// Закрытие CallableStatement и Connection
statement.close();
connection.close();
```

Работа почти как с _PreparedStatement_, только есть нюанс. Наша функция ADD возвращает в третьем параметре результат сложения. Только вот объект _CallableStatement_ об этом ничего не знает. Поэтому мы говорим ему об это явно, вызвав метод registerOutParameter():

`registerOutParameter(номерПараметра, типПараметра)`

После этого можно вызывать процедуру через метод execute() и затем читать данные из третьего параметра с помощью методa getInt().

### ResultSet

All Superinterfaces:[AutoCloseable](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html), [Wrapper](https://docs.oracle.com/javase/8/docs/api/java/sql/Wrapper.html)

[https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html](https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html)

ResultSet – это не множество, он просто так называется. В нем хранится результат выполнения запроса. Этот объект чем-то похож на итератор: он позволяет устанавливать/менять текущую строку результата, а затем из этой текущей строки можно получить данные.

ResultSet имеет у себя внутри указатель на текущую строку. И последовательно переключает строки для их чтения с помощью метода next(). Он возвращает true, если такая строка есть, и false, если строки закончились. Результаты как бы обрамлены пустыми строками с обоих сторон. Поэтому изначально текущая строка находится **перед первой** строкой результата. И **чтобы получить первую строку, нужно хотя бы раз вызвать метод next().**

Затем из текущей строки объекта ResultSet можно получить данные из его колонок:

- `getRow()` – возвращает номер текущей строки в объекте ResultSet
- `getInt(N)` – вернет данные N-й колонки текущей строки как тип int
- `getString(N)` – вернет данные N-й колонки текущей строки как тип String

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");
while (results.next()) {
        	Integer id = results.getInt(1);
        	String name = results.getString(2);
        	System.out.println(results.getRow() + ". " + id + "\t"+ name);
}
```

Если ты **на последней** стоке вызвал метод next(), то ты перешел на строку **после последней**. Данных из нее прочитать ты не можешь, но никакой ошибки не произойдет. Тут метод isAfterLast() будет возвещать true в качестве результата.

|   |   |   |
|---|---|---|
||Метод|Описание|
|1|`next()`|Переключиться на следующую строку|
|2|`previous()`|Переключиться на предыдущую строку|
|3|`isFirst()`|Текущая строка первая?|
|4|`isBeforeFirst()`|Мы перед первой строкой?|
|5|`isLast()`|Текущая строка последняя?|
|6|`isAfterLast()`|Мы после последней сроки?|
|7|`absolute(int n)`|Делает N-ю строку текущей|
|8|`relative(int n)`|Двигает текущую строку на N позиций вперед. N может быть <0|

**Получение данных из текущей строки**

Для этого у объекта ResultSet есть специальные методы, которые все можно описать одним шаблоном:

|   |   |
|---|---|
|SQL Datatype|getXXX() Methods|
|CHAR|`getString()`|
|VARCHAR|`getString()`|
|INT|`getInt()`|
|FLOAT|`getDouble()`|
|CLOB|`getClob()`|
|BLOB|`getBlob()`|
|DATE|`getDate()`|
|TIME|`getTime()`|
|TIMESTAMP|`getTimestamp()`|

- `getType(номерКолонки)`

Впрочем, если у колонки есть имя, то можно получать и по имени колонки:

- `getType(имяКолонки)`

```Java
while (results.next()) {
        	Integer id = results.getInt(“id”);
        	String name = results.getString(“name”);
        	System.out.println(results.getRow() + ". " + id + "\t"+ name);
}
```

### Типы ResultSet

ResultSet может быть определенного типа. Тип определяет некоторые характеристики и возможности ResultSet.

Не все типы поддерживаются всеми базами данных и драйверами JDBC.  
Тебе придется проверить свою базу данных и драйвер JDBC, чтобы увидеть,  
поддерживает ли он тип, который ты хочешь использовать. Метод DatabaseMetaData.supportsResultSetType(int type) возвращает  
_true_ или _false_ в зависимости от того, поддерживается данный тип или нет.

На момент написания статьи существует три типа ResultSet:

- **ResultSet.TYPE_FORWARD_ONLY**
- **ResultSet.TYPE_SCROLL_INSENSITIVE**
- **ResultSet.TYPE_SCROLL_SENSITIVE**

Тип по умолчанию — TYPE_FORWARD_ONLY.

**TYPE_FORWARD_ONLY** означает, что ResultSet можно  
перемещать только вперед. То есть ты можешь перемещаться только из  
строки 1, строки 2, строки 3 и т. д. В ResultSet ты не можешь двигаться  
назад: нельзя считать данные из 9-й строки после чтения десятой.  

**TYPE_SCROLL_INSENSITIVE** означает, что ResultSet  
можно перемещать (прокручивать) как вперед, так и назад. Ты также можешь  
перейти к позиции относительно текущей позиции или перейти к абсолютной  
позиции.  

ResultSet этого типа нечувствителен к  
изменениям в базовом источнике данных, пока ResultSet открыт. То есть  
если запись в ResultSet изменяется в базе данных другим потоком или  
процессом, она не будет отражена в уже открытых ResultSet этого типа.  

**TYPE_SCROLL_SENSITIVE** означает, что ResultSet можно  
перемещать (прокручивать) как вперед, так и назад. Ты также можешь  
перейти к позиции относительно текущей позиции или перейти к абсолютной  
позиции.  

ResultSet этого типа чувствителен к  
изменениям в базовом источнике данных, пока ResultSet открыт. То есть  
если запись в ResultSet изменяется в базе данных другим потоком или  
процессом, она будет отражена в уже открытых ResultSet этого типа.  

  

Параллельность ResultSet определяет, может ли ResultSet обновляться, или только считываться.

Некоторые базы данных и драйверы JDBC поддерживают обновление ResultSet, но не все. Метод DatabaseMetaData.supportsResultSetConcurrency(int concurrency) возвращает значение _true_ или _false_ в зависимости от того, поддерживается данный режим параллелизма или нет.

ResultSet может иметь один из двух уровней параллелизма:

- **ResultSet.CONCUR_READ_ONLY**
- **ResultSet.CONCUR_UPDATABLE**

**CONCUR_READ_ONLY** означает, что ResultSet может быть только прочитан.

**CONCUR_UPDATABLE** означает, что ResultSet может быть прочитан и изменен.

  

С помощью этих параметров ты можешь управлять создаваемым Statement и его ResultSet.

Например, можно создать обновляемый ResultSet и с его помощью менять  
базу данных. При создании Statement важно соблюсти следующие условия:  

- указывается только одна таблица
- не содержит предложений join или group by
- столбцы запроса должны содержать первичный ключ

При выполнении вышеуказанных условий обновляемый ResultSet может быть  
использован для модификации таблицы в базе данных. При создании объекта  
Statement нужно указать такие параметры:  

```Java
Statement st = createStatement(Result.TYPE_SCROLL_INSENSITIVE, Result.CONCUR_UPDATABLE)
```

Результатом выполнения такого оператора является обновляемый набор  
результатов. Метод обновления заключается в перемещении курсора  
ResultSet в строку, которую ты хочешь обновить, а затем в вызове метода updateXXX().  

Метод updateXXX работает аналогично методу getXXX(). Метод updateXXX()  
имеет два параметра. Первый — это номер обновляемого столбца, который  
может быть именем столбца или серийным номером. Второй — это данные,  
которые необходимо обновить, и этот тип данных должен быть тот же, что и  
XXX.  

Чтобы строка реально обновилась в базе, нужно вызвать метод updateRow() до того, как курсор ResultSet покинет измененную строку, в противном случае изменения так и не попадут в базу.

Также можно добавлять новые строки в таблицу:

Сначала нужно переместить курсор на пустую строку. Для этого нужно вызвать метод moveToInsertRow().

Затем нужно заполнить эту строку данными с помощью метода updateXXX().

Затем нужно вызвать метод inserRow(), чтобы строка добавилась в базу.

Ну и наконец нужно вернуть курсор обратно, вызвав метод moveToCurrentRow().

### RowSet extends ResultSet

[https://docs.oracle.com/javase/8/docs/api/javax/sql/RowSet.html](https://docs.oracle.com/javase/8/docs/api/javax/sql/RowSet.html)

Это интерфейс, расширяющий ResultSet и являющийся новым более эффективным для использования.

Основные преимущества:

- RowSet расширяет интерфейс ResultSet, поэтому его функции более мощные, чем у ResultSet.
- RowSet более гибко перемещается по данным таблицы и может прокручивать назад и вперед.
- RowSet поддерживает кэшированные данные, которые можно использовать даже после закрытия соединения.
- RowSet поддерживает новый метод подключения, ты можешь подключиться к базе данных без подключения. Также он поддерживает чтение источника  
    данных XML.  
    
- RowSet поддерживает фильтр данных.
- RowSet также поддерживает операции соединения таблиц.

Типы RowSet:

- **CachedRowSet**
- **FilteredRowSet**
- **JdbcRowSet**
- **JoinRowSet**
- **WebRowSet**

### Создание объекта RowSet

Есть три разных способа получить рабочий объект .

Во-первых, его можно заполнить данными из **ResultSet**, полученного классическим способом.

Например, мы можем закешировать данные **ResultSet** с помощью **CachedRowSet**:

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");

RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();
crs.populate(results);		// Используем ResultSet для заполнения
```

Во-вторых, можно создать полностью автономный объект **RowSet**, создав для него свое собственное подключение к базе данных:

```Java
JdbcRowSet rowSet = RowSetProvider.newFactory().createJdbcRowSet();
rowSet.setUrl("jdbc:mysql://localhost:3306/test");
rowSet.setUsername("root");
rowSet.setPassword("secret");

rowSet.setCommand("SELECT * FROM user");
rowSet.execute();
```

И в-третьих, можно подключить RowSet к уже существующему соединению:

```Java
Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "root");JdbcRowSet rowSet = new JdbcRowSetImpl(con);rowSet.setCommand("SELECT * FROM user");rowSet.execute();
```

### Примеры работы с RowSet

Пример первый: **кэширование**.

Давай напишем код, где с помощью **CachedRowSet** закешируем все данные и будем читать их из уже закрытого соединения:

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");

RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();
crs.populate(results);	// Используем ResultSet для заполнения

connection.close();		// Закрываем соединение// Данные из кэша все еще доступныwhile(crs.next()) {
  	System.out.println(crs.getString(1)+"\t"+ crs.getString(2)+"\t"+ crs.getString(3));
}
```

Пример второй: **изменение строк через RowSet**:

```Java
// Подключаемся к базе данных
 CachedRowSet crs = rsf.createCachedRowSet();
 crs.setUrl("jdbc:mysql://localhost/test");
 crs.setUsername("root");
 crs.setPassword("root");
 crs.setCommand("SELECT * FROM user");
 crs.execute();

// Этот тип операции может изменить только автономный RowSet
// Сначала перемещаем указатель на пустую (новую) строку, текущая позиция запоминается
 crs.moveToInsertRow();
 crs.updateString(1, Random.nextInt());
 crs.updateString(2, "Клон" + System.currentTimeMillis());
 crs.updateString(3, "Женский");
 crs.insertRow();  // Добавляем текущую (новую) строку к остальные строкам
 crs.moveToCurrentRow(); // Возвращаем указатель на ту строку, где он был до вставки

 crs.beforeFirst();
 while(crs.next()) {
 	System.out.println(crs.getString(1) + "," + crs.getString(2) + "," + crs.getString(3));
}

// А теперь мы можем все наши изменения залить в базу
Connection con = DriverManager.getConnection("jdbc:mysql://localhost/dbtest", "root", "root");
con.setAutoCommit(false); // Нужно для синхронизации
crs.acceptChanges(con);// Синхронизировать данные в базу данных
```

### ResultSetMetaData

[https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSetMetaData.html](https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSetMetaData.html)

ResultSetMetaData: Интерфейс ResultSetMetaData используется для получения информации о метаданных конкретного результирующего набора (ResultSet) после выполнения запроса к базе данных. Он предоставляет информацию о столбцах в результирующем наборе, такую как их имена, типы данных, размеры и т.д. С помощью ResultSetMetaData можно получить информацию о структуре данных, содержащихся в результирующем наборе, и использовать эту информацию для динамической обработки результатов запроса.

- `getCatalogName(int column):` Возвращает имя каталога таблицы, к которой относится указанный столбец.
- `getColumnClassName(int column)`: Возвращает полное квалифицированное имя класса Java, объекты которого создаются при вызове метода ResultSet.getObject для извлечения значения из столбца.
- `getColumnCount()`: Возвращает количество столбцов в этом объекте ResultSet.
- `getColumnDisplaySize(int column)`: Возвращает нормальную максимальную ширину указанного столбца в символах.
- `getColumnLabel(int column)`: Получает предлагаемый заголовок для указанного столбца, который может использоваться при выводе на печать или отображении.
- `getColumnName(int column)`: Возвращает имя указанного столбца.
- `getColumnType(int column)`: Возвращает SQL-тип указанного столбца.
- `getColumnTypeName(int column)`: Возвращает имя типа указанного столбца в базе данных.
- `getPrecision(int column)`: Возвращает указанный размер столбца.
- `getScale(int column)`: Получает количество десятичных знаков справа от десятичной точки в указанном столбце.
- `getSchemaName(int column)`: Возвращает схему таблицы, к которой относится указанный столбец.
- `getTableName(int column)`: Возвращает имя таблицы указанного столбца.
- `isAutoIncrement(int column)`: Указывает, является ли указанный столбец автоматически нумеруемым.
- `isCaseSensitive(int column)`: Указывает, имеет ли регистр значения столбца значение.
- `isCurrency(int column)`: Указывает, является ли указанный столбец денежным значением.
- `isDefinitelyWritable(int column)`: Указывает, будет ли успешно выполнена запись в указанный столбец.
- `isNullable(int column)`: Указывает возможность присутствия значений null в указанном столбце.
- `isReadOnly(int column)`: Указывает, является ли указанный столбец определенно нередактируемым.
- `isSearchable(int column)`: Указывает, может ли указанный столбец быть использован в предложении WHERE.
- `isSigned(int column)`: Указывает, являются ли значения в указанном столбце знаковыми числами.
- `isWritable(int column)`: Указывает, может ли произойти успешная запись в указанный столбец.

Пример получения метаданных:

```Java
ResultSetMetaData metaData = results.getMetaData();
int columnCount = metaData.getColumnCount();
for (int column = 1; column <= columnCount; column++)
{
        	String name = metaData.getColumnName(column);
        	String className = metaData.getColumnClassName(column);
        	String typeName = metaData.getColumnTypeName(column);
        	int type = metaData.getColumnType(column);

        	System.out.println(name + "\t" + className + "\t" + typeName + "\t" + type);


//ВЫВОД:
id	java.lang.Integer	INT	4
name	java.lang.String	VARCHAR	12
level	java.lang.Integer	INT	4
created_date	java.sql.Date	DATE	91
```

  

### MetaData

[https://docs.oracle.com/javase/8/docs/api/java/sql/DatabaseMetaData.html](https://docs.oracle.com/javase/8/docs/api/java/sql/DatabaseMetaData.html)

MetaData: Интерфейс MetaData (иногда называемый DatabaseMetaData) предоставляет информацию о базе данных в целом, такую как название базы данных, версия СУБД, доступные таблицы и столбцы, поддерживаемые типы данных и т.д. Он позволяет получить общую информацию о базе данных и ее структуре, но не об отдельных результатов запросов.

Некоторые из методов:

1. `**getSchemas()**`: Возвращает результаты запроса о доступных схемах в базе данных.
2. `**getCatalogs()**`: Возвращает результаты запроса о доступных каталогах в базе данных.
3. `**getProcedureColumns(String catalog, String schemaPattern, String procedureNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о столбцах, связанных с хранимыми процедурами в базе данных.
4. `**getProcedures(String catalog, String schemaPattern, String procedureNamePattern)**`: Возвращает результаты запроса о доступных хранимых процедурах в базе данных.
5. `**getFunctionColumns(String catalog, String schemaPattern, String functionNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о столбцах, связанных с функциями в базе данных.
6. `**getFunctions(String catalog, String schemaPattern, String functionNamePattern)**`: Возвращает результаты запроса о доступных функциях в базе данных.
7. `**getResultSetHoldability()**`: Возвращает уровень сохраняемости результирующего набора (ResultSet) в базе данных.
8. `**getConnection()**`: Возвращает объект Connection, представляющий соединение с базой данных.
9. `**getDatabaseProductName()**`: Возвращает название продукта базы данных.
10. `**getDatabaseProductVersion()**`: Возвращает версию продукта базы данных.
11. `**getDriverName()**`: Возвращает название драйвера базы данных.
12. `**getDriverVersion()**`: Возвращает версию драйвера базы данных.
13. `**getURL()**`: Возвращает URL-адрес базы данных.
14. `**getUserName()**`: Возвращает имя пользователя базы данных.
15. `**supportsResultSetType(int type)**`: Проверяет, поддерживает ли база данных указанный тип результирующего набора.
16. `**supportsTransactionIsolationLevel(int level)**`: Проверяет, поддерживает ли база данных указанный уровень изоляции транзакций.
17. `**getTables(String catalog, String schemaPattern, String tableNamePattern, String[] types)**`: Возвращает результаты запроса о доступных таблицах в базе данных.
18. `**getColumns(String catalog, String schemaPattern, String tableNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о доступных столбцах в таблице.
19. `**getTypeInfo()**`: Возвращает информацию о доступных типах данных в базе данных.
20. `**getPrimaryKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса о первичных ключах для указанной таблицы.
21. `**getImportedKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса об импортированных ключах для указанной таблицы.
22. `**getExportedKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса об экспортированных ключах из указанной таблицы.

## Типы данных, приведение типов, работа со временем

Создатели JDBC начали с того, что зафиксировали список SQL-типов. Есть специальный класс Types с константами: [https://docs.oracle.com/javase/8/docs/api/java/sql/Types.html](https://docs.oracle.com/javase/8/docs/api/java/sql/Types.html)

Число — это не порядковый номер в классе, а ID-тип в списке SQL-типов в спецификации SQL.

```Java
class java.sql.Types {
   public static final int CHAR         =   1;
   public static final int NUMERIC    	=   2;
   public static final int DECIMAL     	=   3;
   public static final int INTEGER      =   4;
   public static final int FLOAT        =   6;
   public static final int REAL         =   7;
  …
}
```

Также в классе ResultSet есть методы, которые умеют преобразовывать  
одни типы данных в другие. Не все типы можно преобразовать друг в друга,  
но логика достаточно понятна. Вот тебе хорошая таблица на первое время:  

|   |   |
|---|---|
|Метод|Тип данных SQL|
|int getInt()|NUMERIC, INTEGER, DECIMAL|
|float getFloat()|NUMERIC, INTEGER, DECIMAL, FLOAT, REAL|
|double getDoublel()|NUMERIC, INTEGER, DECIMAL, FLOAT, REAL|
|Date getDate()|DATE, TIME, TIMESTAMP|
|Time getTime()|DATE, TIME, TIMESTAMP|
|Timestamp getTimestamp()|DATE, TIME, TIMESTAMP|
|String getString()|CHAR, VARCHAR|

### NULL

Есть таблица в базе денных, которая имеет колонку с каким либо типом, но также может быть и NULL.  
Как получить значение null из этой колонки?  

**Решение первое**. Если SQL-тип в Java представлен ссылочным типом, таким как Date или String, то проблемы нет. Переменные этого типа могут принимать значения null.

**Решение второе**. Примитивные типы не могут принимать значения null, поэтому методы типа getInt() будут просто возвращать значение по умолчанию. Для int это 0, для float = 0.0f, для double = 0.0d и тому подобное.

**Решение третье**. У класса ResultSet есть специальный метод wasNull(), который возвращает true, если только что вместо NULL метод вернул другое значение.

```Java
Connection connection = ConnectionManager.open();

        Statement statement = connection.createStatement();
        ResultSet results = statement.executeQuery("SELECT * FROM buyers");
        results.next();
        String name = results.getString("name");

        if (results.wasNull()) {
            System.out.println("Name is null");
        } else {
            System.out.println("Name is " + name);
```

### Работа со временем

### Blob

Если ты хочешь сохранить какой-то объект в базу данных, то самый  
простой способ сделать это — воспользовавшись SQL-типом BLOB. У JDBC  
есть его аналог, который называется Blob.  

BLOB расшифровывается как **Binary Large Object**.  
Он используется для хранения массива байт. Тип Blob в JDBC является  
интерфейсом и в него можно класть (и получать) данные двумя способами:  

- С помощью InputStream
- С помощью массива байт

Пример: колонка номер 3 содержит тип BLOB:

```Java
Statement statement = connection.createStatement();
    ResultSet results = statement.executeQuery("SELECT * FROM user");
    results.first();

    Blob blob = results.getBlob(3);
    InputStream is = blob.getBinaryStream();
```

Чобы создать свой объект Blob, нужно воспользоваться функцией createBlob(). Пример:

```Java
String insertQuery = “INSERT INTO images(name, image) VALUES (?, ?)”;
PreparedStatement statement = connection.prepareStatement(insertQuery);

// Создаем объект Blob и получаем у него OtputStream для записи в него данных
Blob blob = connection.createBlob();

// Заполняем Blob данными  …
OutputStream os = blob.setBinaryStream(1);

// Передаем Вlob как параметр запроса
statement.setBlob(2, blob);
statement.execute();
```

Заполнить Blob данными можно двумя способами. Первый — через OutputSteam:

```Java
Path avatar = Paths.get("E:\\images\\cat.jpg");
OutputStream os = blob.setBinaryStream(1);
Files.copy(avatar, os);
```

И второй — через заполнение байтами:

```Java
Path avatar = Paths.get("E:\\images\\cat.jpg");
byte[] content = Files.readAllBytes(avatar);
blob.setBytes(1, content);
```

### bytea

Цель следующая:

1. Превратить экземлпяр класса Person в набор байтов, записать в таблицу байты
2. Считать байты, десериализовать в Person

```Java
import java.io.Serializable;

public class Person implements Serializable {
    private String name;
    private String lastName;
    private int salary;
    private int department_id;

    public Person(String name, String lastName, int salary, int department_id) {
        this.name = name;
        this.lastName = lastName;
        this.salary = salary;
        this.department_id = department_id;
    }

    @Override
    public String toString() {
        return "Person{" +
               "name='" + name + '\'' +
               ", lastName='" + lastName + '\'' +
               ", salary=" + salary +
               ", department_id=" + department_id +
               '}';
    }
}
```

```Java
Person person = new Person("Vadim", "Konovalov", 1000, 1);
String insert = "INSERT INTO blob_employees (object) VALUES(?)";
insert(person, insert);
```

```Java
private static void insert(Person person, String insert) throws SQLException, IOException {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(insert);
             ByteArrayOutputStream bos = new ByteArrayOutputStream();
             ObjectOutputStream oos = new ObjectOutputStream(bos)) {
            oos.writeObject(person);
            oos.flush();
            statement.setBytes(1, bos.toByteArray());
            statement.execute();
        }
    }
```

Результат выполнения данного кода

![[images/Untitled 2 12.png|Untitled 2 12.png]]

Десериализация

```Java
String select = "SELECT * FROM blob_employees WHERE id = 1";
Person gettedPerson = getPerson(select);
System.out.println(gettedPerson);
```

```Java
private static Person getPerson(String select) throws SQLException, IOException, ClassNotFoundException {
        try (var connection = ConnectionManager.open();
             var statement = connection.createStatement()){
            ResultSet resultSet = statement.executeQuery(select);
            resultSet.next();
            byte[] bytes = resultSet.getBytes("object");
            ByteArrayInputStream bis = new ByteArrayInputStream(bytes);
            ObjectInputStream ois = new ObjectInputStream(bis);
            return (Person) ois.readObject();
        }
    }
```

```Java
ВЫВОД:Person{name='Vadim', lastName='Konovalov', salary=1000, department_id=1}
```

  

  

  

## Запросы

### DDL

- в большинстве случаев execute() будет оптимальным вариантом

```Java
public static void main( String[] args ) throws SQLException {
        String sql = "CREATE TABLE IF NOT EXISTS employees (id SERIAL PRIMARY KEY, name VARCHAR(32))";

            try(var connection = ConnectionManager.open();
                var statement = connection.createStatement()){
                    var executeResult = statement.execute(sql);
                    System.out.println(executeResult);
 //false, так как это не SELECT, никаких данных не возвращается
                }
            }
```

### DML

Все DML SQL-запросы можно условно разделить на две группы:

- **Получение данных** — к ним относится оператор SELECT.
- **Изменение данных** — к ним относятся операторы INSERT, UPDATE и DELETE.

Для первой группы используется уже знакомый нам метод интерфейса _Statement_ — `executeQuery()`. Он покрывает очень большой процент запросов.

```Java
String line = "SELECT * from employees WHERE name = 'Vadim';";
try(var connection: Connection = connection.Manager.open();
		 var statement: Statement = connection.createStatement()){
ResultSet set = statement.executeQuery(line);
}
```

Для второй группы запросов нужно использовать другой метод интерфейса Statement — `executeUpdate()`. В отличии от метода `executeQuery()`, который возвращает ResultSet, этот метод возвращает целое число, которое говорит сколько строк в таблице было изменено при исполнении вашего запроса.

```Java
String line = "UPDATE  employee SET salary = salary+1000";
try(var connection: Connection = connection.Manager.open();
		 var statement: Statement = connection.createStatement()){
int updatedCount = statement.executeUpdate(line);
}
```

Могут возникнуть ситуации, когда неизвестно, какой запрос тебе приходится исполнять — выборка или изменение данных. На этот случай создатели JDBC добавили в него еще один универсальный метод — `execute()`.

Метод `execute()` возвращает `boolean`. Если это значение равно _true_, то значит выполнялся запрос на получение данных, и тебе нужно вызвать метод `getResultSet()`.

```Java
boolean hasResults = statement.execute("SELECT Count(*) FROM user"); //true
    if ( hasResults ) {
        	ResultSet results =  statement.getResultSet();
        	results.next();
        	int count = results.getInt(1);
}
```

Если это значение равно `_false_`, то значит выполнялся запрос на изменение данных, и тебе нужно вызвать метод `getUpdateCount()`, чтобы получить количество измененных строк. Пример:

```Java
boolean hasResults = statement //false
.execute("UPDATE  employee SET salary = salary+1000");
if ( !hasResults ) {int count = statement.getUpdateCount();}
```

### Generated Keys

Позволяет после запрос получить id вставляемого объекта без повторного запроса на поиск.

```Java
String sql = "INSERT INTO employees (name) VALUES ('Vadim')";

        try (var connection = ConnectionManager.open();
             var statement = connection.createStatement()) {

            var pos = statement.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);
            var generatedKeys = statement.getGeneratedKeys();
            if(generatedKeys.next()){
                var generatedId = generatedKeys.getInt("id");
                System.out.println(generatedId);
                // после первого запуска 1
                // после второго 2
                // и тд
```

### Ключевое слово RETURNING

Можно превратить команду UPDATE или INSERT в команду SELECT, чтобы после изменения данных, эти данные возвращались в качестве ResultSet:

![[images/Untitled 3 11.png|Untitled 3 11.png]]

```Java
String sql = "UPDATE buyers SET money = 1000 WHERE name = 'Vadim' or name = 'Dima' RETURNING *;";

        try (var connection = ConnectionManager.open();
             var statement = connection.createStatement()) {
            ResultSet resultSet = statement.executeQuery(sql);
            while (resultSet.next()) {
                System.out.println(resultSet.getString(1) + " " + resultSet.getString(2));
            }
        }

//ВЫВОД
Dima 1000
Dima 1000
Vadim 1000
Vadim 1000
```

![[images/Untitled 4 11.png|Untitled 4 11.png]]

### FetchSize

Метод `setFetchSize()` используется для установки размера выборки (fetch size) при извлечении данных из базы данных с использованием ResultSet. Размер выборки определяет количество строк, которые будут извлечены за одну операцию из базы данных и переданы клиентскому приложению.

Метод `setQueryTimeout(int seconds)` используется для установки таймаута выполнения SQL - запроса в секундах. Если операция выполнения запроса занимает больше времени, чем указанное значение таймаута, то будет сгенерировано исключение `SQLException`.

Метод `setMaxRows(int max)` используется для установки  
максимального числа строк, которые будут возвращены в результирующем  
наборе при выполнении SQL - запроса. Если запрос возвращает больше строк,  
чем указанное значение  
`max`, то оставшиеся строки будут игнорироваться.

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class FetchSizeExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "password";

        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement()) {

	          // Установка размера выборки
	         int fetchSize = 100; // Устанавливаем размер выборки равным 100
            statement.setFetchSize(fetchSize);

            // Установка таймаута запроса в 30 секунд
            int queryTimeout = 30;
            statement.setQueryTimeout(queryTimeout);

						// Установка максимального числа строк равным 10
            int maxRows = 10;
            statement.setMaxRows(maxRows);

            String sql = "SELECT * FROM users";
            ResultSet resultSet = statement.executeQuery(sql);

            // Обработка результата
            while (resultSet.next()) {
                // Чтение данных из результирующего набора
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                // Дополнительная обработка данных
                System.out.println("User ID: " + id + ", Name: " + name);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

  

### Batching запросов

В реальных проектах часто возникает ситуация, когда необходимо сделать очень много однотипных запросов (наиболее часто в этом случае  
встречается  
_PreparedStatement_), например, надо вставить несколько десятков или сотен записей.

Если выполнять каждый запрос отдельно, то это займет кучу времени и снизит производительность приложения. Чтобы не допустить этого, можно  
использовать batch-режим вставки. Он заключается в том, что ты накапливаешь некоторый буфер своими запросами, а потом выполняешь их сразу.  

  

```Java
PreparedStatement stmt = con.prepareStatement(
        	"INSERT INTO jc_contact (first_name, last_name, phone, email) VALUES (?, ?, ?, ?)");

for (int i = 0; i < 10; i++) {
	// Заполняем параметры запроса
	stmt.setString(1, "FirstName_" + i);
    stmt.setString(2, "LastNAme_" + i);
    stmt.setString(3, "phone_" + i);
    stmt.setString(4, "email_" + i);
	// Запрос не выполняется, а укладывается в буфер,
	//  который потом выполняется сразу для всех команд
	stmt.addBatch();
}
// Выполняем все запросы разом
int[] results = stmt.executeBatch();
```

==**ВАЖНО**==: в данном примере выполняется однотипный запрос через PreparedStatement. Когда несколько разных PreparedStatement с разными запросами использовать batching не получится, придется использовать обычный Statement, в который можно сразу будет передавать строку и добавлять statement в пул.

==ВАЖНО==: важен порядок добавления в батч запросов, которые связаны внешники ссылками.

```Java
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

public class TransactionRunner {
    public static void main(String[] args) {
        int id = 10;
        try {
            deleteEmployee(id);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void deleteEmployee(int id) throws SQLException {
        var deleteEmployeeSql = "DELETE FROM employees WHERE salary = " +id;
        var deletePassport = "DELETE FROM passports WHERE salary = "+id;
        Connection connection = null;
        Statement statement = null;
        try{
            connection = ConnectionManager.open();
            connection.setAutoCommit(false);
            statement = connection.createStatement();

            statement.addBatch(deletePassport);
            statement.addBatch(deleteEmployeeSql);
            int[] ints = statement.executeBatch();
        }
        catch (Exception e){
            if(connection!= null){
                connection.rollback();
            }
            throw e;
        }
        finally {
            if(connection != null){
                connection.close();
            }
            if(statement!= null){
                statement.close();
            }
        }
    }
}
```

Вместо того, чтобы выполнять запрос методом execute(), мы складываем его в пакет с помощью метода addBatch().

А затем, когда набралось несколько сотен запросов, можно их разом отправить на сервер, вызвав команду executeBatch().

**Полезно.** Метод executeBatch() возвращает массив целых чисел — int[]. Каждая ячейка этого массива содержит число, которое означает количество строк, измененных соответствующим запросом. Если запрос номер 3 в batch’е изменил 5 строк, то 3-я ячейка массива будет содержать число 5.

`**fetchSize**` и `**batchSize**` - это параметры, используемые при работе с базами данных для оптимизации производительности при извлечении данных. Вот их простое объяснение и различие:

1. **Fetch Size (Размер извлечения)**:
    - **Определение**: Определяет количество записей, которые база данных извлекает за один раз.
    - **Пример**: Представьте, что у вас есть база данных с 1000 записями. Если вы устанавливаете `**fetchSize**` в 100, то при запросе данных будет извлечено только первые 100 записей. Если вам нужно больше данных, то будет сделан дополнительный запрос к базе данных.
2. **Batch Size (Размер пакета)**:
    - **Определение**: Определяет количество записей, которые приложение будет обрабатывать за один раз, прежде чем отправить запрос на дополнительные данные.
    - **Пример**: Допустим, вы загружаете данные из базы данных в приложение для обработки. Если `**batchSize**` установлен в 50, ваше приложение будет обрабатывать эти 50 записей, а затем отправит запрос на базу данных для получения следующих 50 записей.

**Различие**:

- `**fetchSize**` контролирует, сколько записей извлекается из базы данных за один раз.
- `**batchSize**` определяет, сколько записей ваше приложение будет обрабатывать одновременно перед тем, как запросить дополнительные данные из базы.

**Пример**:  
Представьте, что вы читаете данные из большой таблицы с 1000 записями с использованием  
`**fetchSize**` и `**batchSize**`. Если установлены значения `**fetchSize=100**` и `**batchSize=50**`, то ваше приложение извлечет первые 100 записей (из-за `**fetchSize**`), а затем обработает их по 50 записей за раз (из-за `**batchSize**`). После обработки первых 50 записей, ваше приложение запросит дополнительные 50 записей (из-за `**batchSize**`) и так далее, пока все записи не будут обработаны.

## Транзакции в JDBC

int _TRANSACTION_NONE_ = 0;

int _TRANSACTION_READ_UNCOMMITTED_ = 1;

int _TRANSACTION_READ_COMMITTED_ = 2;

int _TRANSACTION_REPEATABLE_READ_ = 4;

int _TRANSACTION_SERIALIZABLE_ = 8;

  

В Postgres Pro вы можете запросить любой из четырёх уровней изоляции транзакций, однако внутри реализованы только три различных уровня, то есть режим Read Uncommitted в Postgres Pro действует как Read Committed. Причина этого в том, что только так можно сопоставить стандартные уровни изоляции с реализованной в Postgres Pro архитектурой многоверсионного управления конкурентным доступом.

В этой таблице также показано, что реализация Repeatable Read в Postgres  
Pro не допускает фантомное чтение. Стандарт SQL допускает возможность  
более строгого поведения: четыре уровня изоляции определяют только,  
какие особые условия не должны наблюдаться, но не какие  
_обязательно должны_.

|   |   |   |   |   |
|---|---|---|---|---|
|Уровень изоляции|«Грязное» чтение|Неповторяемое чтение|Фантомное чтение|Аномалия сериализации|
|Read uncommited (Чтение незафиксированных данных)|Допускается, но не в PG|Возможно|Возможно|Возможно|
|Read committed (Чтение зафиксированных данных)|Невозможно|Возможно|Возможно|Возможно|
|Repeatable read (Повторяемое чтение)|Невозможно|Невозможно|Допускается, но не в PG|Возможно|
|Serializable (Сериализуемость)|Невозможно|Невозможно|Невозможно|Невозможно|

Установить или получить уровень изоляции транзакзциями можно следующим образом:

```Java
Connection connection = DriverManager.getConnection(url, userName, password);
connection.setTransactionIsolation(4);
int level = connection.getTransactionIsolation();
```

### Транзакции в jdbc

Каждый вызов метода execute() объекта Statement выполняется в отдельной транзакции. Для этого у Connection есть параметр AutoCommit. Если он выставлен в _true_, то commit() будет вызываться после каждого вызова метода execute().

Если ты хочешь выполнить несколько команд в одной транзакции, то сделать это можно так:

- отключаем AutoCommit
- вызываем наши команды
- вызываем метод commit() явно

```Java
connection.setAutoCommit(false);

Statement statement = connection.createStatement();
int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount3 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");

connection.commit();
```

Если во время работы метод commit() на сервере произойдет ошибка, то SQL-сервер отменит все три действия.

Но бывают ситуации, когда ошибка возникает еще на стороне клиента, и мы так и не дошли до вызова метода commit():

```Java
connection.setAutoCommit(false);

Statement statement = connection.createStatement();
int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount3 = statement.executeUpdate("UPDATE  несколько опечаток приведут к исключению");

connection.commit();
```

Если во время работы одного executeUpdate() произойдет ошибка, то метод commit() вызван так и не будет. Чтобы откатить все сделанные действия, нужно вызвать метод rollback().

Есть две таблицы employees и passports @One to many. Удалить запись из первой без удаления соответствующей записи из второй нельзя. Решение через транзакции:

```Java
package ru.konovalov;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TransactionRunner {
    public static void main(String[] args) {
        int id = 10;
        try {
            deleteEmployee(id);
        } catch (SQLException e) {
            System.out.println(e.printStackTrace(););
        }
    }
    
    public static void deleteEmployee(int id) throws SQLException {
        var deleteEmployeeSql = "DELETE FROM employees WHERE salary = ?";
        var deletePassport = "DELETE FROM passports WHERE salary = ?";
        Connection connection = null;
        PreparedStatement deletePassStat = null;
        PreparedStatement deleteEmployeeStat = null;
        try{
            connection = ConnectionManager.open();
            connection.setAutoCommit(false);
            deleteEmployeeStat = connection.prepareStatement(deleteEmployeeSql);
            deletePassStat = connection.prepareStatement(deletePassport);
            deleteEmployeeStat.setLong(1,id);
            deletePassStat.setLong(1,id);
            
            deleteEmployeeStat.executeUpdate();
            deletePassStat.executeUpdate();
            
        }
        catch (Exception e){
            if(connection!= null){
                connection.rollback();
            }
            throw e;
        }
        finally {
            if(connection != null){
                connection.close();
            }
            if(deleteEmployeeStat!= null){
                deleteEmployeeStat.close();
            }
            if(deletePassStat!= null){
                deletePassStat.close();
            }
        }
    }
}
```

### Точки сохранения

Для того, чтобы сохраниться, нужно создать точку сохранения, делается это командой:

`Savepoint save = connection.setSavepoint();`

Возврат к точке сохранения делается командой:

`connection.rollback(save);`

```Java
try{
  	connection.setAutoCommit(false);

  	Statement statement = connection.createStatement();
  	int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
  	int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");

  	Savepoint save = connection.setSavepoint();
 	 try{
      	int rowsCount3 = statement.executeUpdate("UPDATE  несколько опечаток приведут к исключению");
 	 }
 	 catch (Exception e) {
    	   connection.rollback(save);
 	 }

	  connection.commit();
 }
 catch (Exception e) {
   connection.rollback();
}
```

## Работа с пулом потоков

[https://javarush.com/quests/lectures/questhibernate.level08.lecture06](https://javarush.com/quests/lectures/questhibernate.level08.lecture06)

В реальных приложениях изначально создается пул соединений, можно использовать для этого очередь, во время последующей работы приложения новые соединения не создаются( так как это дорогостоящая операция), а берутся из пула.

## DAO

Пример использования в проекте: [https://github.com/Onemyname/ConsulApp/tree/master/src/main/java/ru/konovalov](https://github.com/Onemyname/ConsulApp/tree/master/src/main/java/ru/konovalov)

В контексте JDBC (Java Database Connectivity), DAO означает Data Access Object (Объект доступа к данным). DAO является шаблоном проектирования, используемым для организации доступа к данным в приложениях Java.

DAO позволяет отделить бизнес-логику приложения от механизмов доступа к данным, что упрощает разработку и поддержку кода. Он обеспечивает абстракцию уровня доступа к данным, что позволяет изменять и заменять источник данных без влияния на остальные части приложения.

В JDBC DAO позволяет выполнять операции с базой данных, используя JDBC API. Он может содержать методы для создания соединения с базой данных, выполнения SQL-запросов, извлечения и обработки результатов запросов и управления транзакциями.

### Пример

### Таблица consumers

![[images/Untitled 5 11.png|Untitled 5 11.png]]

### DAORunner

```Java
package ru.konovalov;

import java.util.List;
import java.util.Optional;

public class DaoRunner {
    private static final  ConsumerDAO CONSUMER_DAO = ConsumerDAO.getInstance();

    public static void main(String[] args) {

        saveConsumer("Dmitriy","Parfenov",2, 1500);

        deleteConsumer(3);

        List<Consumer> list = getAllConsumers();

        var maybeConsumer = getConsumerById(4);

        maybeConsumer.ifPresent(consumer -> {
            consumer.setSalary(3000);
            consumer.setLastName("Parfen4ik");
            CONSUMER_DAO.update(consumer);
        });

    }


    public static String saveConsumer(String name, String lastName, int passport, int salary){
        Consumer consumer = new Consumer();
        consumer.setFirstName(name);
        consumer.setLastName(lastName);
        consumer.setPassport(passport);
        consumer.setSalary(salary);

        return CONSUMER_DAO.save(consumer);

    }
    public static boolean deleteConsumer(int id){

        return CONSUMER_DAO.delete(id);
    }

    public static boolean updateConsumer(Consumer consumer){

        return CONSUMER_DAO.update(consumer);
    }

    public static List<Consumer> getAllConsumers(){

        return CONSUMER_DAO.getAllConsumers();
    }
    public static Optional<Consumer> getConsumerById(int id){

        return CONSUMER_DAO.findConsumerById(id);
    }
}
```

### Consumer

```Java
package ru.konovalov;

import lombok.Data;

@Data
public class Consumer {
    private int id;
    private String firstName;
    private String lastName;
    private int passport;
    private int salary;

    public Consumer(int id, String firstName, String lastName, int passport, int salary) {
        this.id = id;
        this.firstName = firstName;
        this.lastName = lastName;
        this.passport = passport;
        this.salary = salary;
    }

    public Consumer() {
    }
}
```

### ConsumerDAO

```Java
package ru.konovalov;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class ConsumerDAO {
    private static final ConsumerDAO INSTANCE = new ConsumerDAO();
    private static final String INSERT_SQL = "INSERT INTO consumers (first_name, last_name, passport, salary)" +
                                             " VALUES(?,?,?,?)";
    private static final String DELETE_SQL = "DELETE FROM consumers WHERE id = ?";
    private static final String UPDATE_SQL = "UPDATE consumers" +
                                             " SET first_name =?, last_name = ?, passport = ?, salary = ?" +
                                             " WHERE id = ?";

    private static final String FIND_ALL_SQL = "SELECT id, first_name, last_name, passport, salary FROM consumers";

    private static final String FIND_BY_ID = FIND_ALL_SQL + " WHERE id = ?";

    private ConsumerDAO() {
    }

    public static ConsumerDAO getInstance() {
        return INSTANCE;
    }

    public String save(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(INSERT_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport());
            statement.setInt(4, consumer.getSalary());
            int executed = statement.executeUpdate();
            var generatedKeys = statement.getGeneratedKeys();
            generatedKeys.next();
            consumer.setId(generatedKeys.getInt("id"));
            return "Consumer is saved! id = " + consumer.getId();
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean delete(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(DELETE_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setInt(1, id);
            int executed = statement.executeUpdate();
            return executed > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean update(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(UPDATE_SQL)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport());
            statement.setInt(4, consumer.getSalary());
            statement.setInt(5, consumer.getId());

            return statement.executeUpdate() > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public List<Consumer> getAllConsumers() {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_ALL_SQL)) {
            ResultSet resultSet = statement.executeQuery();
            List<Consumer> consumers = new ArrayList<>(10);
            while (resultSet.next()) {
                consumers.add(build(resultSet));
            }
            return consumers;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public Optional<Consumer> findConsumerById(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_BY_ID)) {
            statement.setInt(1, id);
            ResultSet resultSet = statement.executeQuery();
            Consumer consumer = null;
            if (resultSet.next()) {
                consumer = build(resultSet);
            }
            return Optional.ofNullable(consumer);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    private Consumer build(ResultSet resultSet) throws SQLException {
        return new Consumer(
                resultSet.getInt("id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
                resultSet.getInt("passport"),
                resultSet.getInt("salary"));
    }

}
```

### Сложный EntryMapping

В предыдущеим примере Consumer содержал поле int passport, но в моих таблицах это FOREIGN KEY на `таблицу passports @One to One. Ниже приведено как реализовать, чтобы при выполнении был не int, а отдельная сущность Passport.

### 1 пример с JOIN

![[images/Untitled 5 11.png|Untitled 5 11.png]]

![[images/Untitled 6 10.png|Untitled 6 10.png]]

```Java
package ru.konovalov;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class ConsumerDAO {
    private static final ConsumerDAO INSTANCE = new ConsumerDAO();
    private static final String INSERT_SQL = "INSERT INTO consumers (first_name, last_name, passport, salary)" +
                                             " VALUES(?,?,?,?)";
    private static final String DELETE_SQL = "DELETE FROM consumers WHERE id = ?";
    private static final String UPDATE_SQL = "UPDATE consumers" +
                                             " SET first_name =?, last_name = ?, passport = ?, salary = ?" +
                                             " WHERE id = ?";

    private static final String FIND_ALL_SQL = "SELECT c.id, first_name, last_name, passport, salary, p.number " +
                                               "FROM consumers c " +
                                               "JOIN passports p on p.id = c.passport";

    private static final String FIND_BY_ID = FIND_ALL_SQL + " WHERE c.id = ?";

    private ConsumerDAO() {
    }

    public static ConsumerDAO getInstance() {
        return INSTANCE;
    }

    public String save(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(INSERT_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport().id());
            statement.setInt(4, consumer.getSalary());
            int executed = statement.executeUpdate();
            var generatedKeys = statement.getGeneratedKeys();
            generatedKeys.next();
            consumer.setId(generatedKeys.getInt("id"));
            return "Consumer is saved! id = " + consumer.getId();
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean delete(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(DELETE_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setInt(1, id);
            int executed = statement.executeUpdate();
            return executed > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean update(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(UPDATE_SQL)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport().id());
            statement.setInt(4, consumer.getSalary());
            statement.setInt(5, consumer.getId());

            return statement.executeUpdate() > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public List<Consumer> getAllConsumers() {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_ALL_SQL)) {
            ResultSet resultSet = statement.executeQuery();
            List<Consumer> consumers = new ArrayList<>(10);
            while (resultSet.next()) {
                consumers.add(build(resultSet));
            }
            return consumers;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public Optional<Consumer> findConsumerById(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_BY_ID)) {
            statement.setInt(1, id);
            ResultSet resultSet = statement.executeQuery();
            Consumer consumer = null;
            if (resultSet.next()) {
                consumer = build(resultSet);
            }
            return Optional.ofNullable(consumer);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    private Consumer build(ResultSet resultSet) throws SQLException {
        var passport = new Passport(resultSet.getInt("id"),
                resultSet.getString("number"));
        return new Consumer(
                resultSet.getInt("id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
                passport,
                resultSet.getInt("salary"));
    }

}
```

```Java
List<Consumer> list = getAllConsumers();
list.stream().forEach(System.out::println);
Consumer(id=2, firstName=Vadim, lastName=Konovalski, passport=Passport[id=2, number=7584848], salary=1000)
Consumer(id=4, firstName=Dmitriy, lastName=Parfen4ik, passport=Passport[id=4, number=753848], salary=3000)
```

Минусы такого подохода:

- ConsumerDao внутри метода build() должен знать как билдить объект типа Passport.
- В моем случае есть дополнительная сущность Passport, которая содержит только number, а если она будет содержать внутри себя еще сущности,а они будут содержать другие сущности? По-хорошему придется делать большой JOIN и билдить большое количество объектов в одном методе. Понимание кода усложнится.

  

### 2 пример с DAO

Интерфейс, который будут реализовать все DAO:

```Java
package ru.konovalov;

import java.util.List;
import java.util.Optional;

public interface DAO<T,E> {
    String save(E clazz);
    boolean delete(T id);
    boolean update(E clazz);
    List<E> findAll();
    Optional<E> findById(T id);
}
```

Добавляем доп сущности:

```Java
package ru.konovalov;

public record Passport(int id, String number) {
}
```

```Java
package ru.konovalov;

import java.sql.SQLException;
import java.util.List;
import java.util.Optional;

public class PassportDAO implements DAO<Integer,Passport> {
    private static final PassportDAO INSTANCE = new PassportDAO();
    private static final String FIND_BY_ID_SQL = """
            SELECT id, number FROM passports
            WHERE id=?
            """;

    private PassportDAO(){}
    public static PassportDAO getInstance(){
        return INSTANCE;
    }

@Override
    public Optional<Passport> findById(Integer id) {
        try (var conection = ConnectionManager.open()){
            return findById(id,conection);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public Optional<Passport> findById(Integer id, Connection connection) {
        try (var statement = connection.prepareStatement(FIND_BY_ID_SQL)) {
            statement.setInt(1,id);
            var resultSet = statement.executeQuery();
            Passport passport = null;
            if(resultSet.next()){
                passport = new Passport(
                        resultSet.getInt("id"),
                        resultSet.getString("number")
                );
            }
            return Optional.ofNullable(passport);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }
}
```

Меняем build в классе consumerDAO:

```Java
private Consumer build(ResultSet resultSet) throws SQLException {
        return new Consumer(
                resultSet.getInt("id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
PASSPORT_DAO.findById(resultSet.getInt("passport"), resultSet.getStatement().getConnection()).orElse(null),
                resultSet.getInt("salary"));
    }
```

```Java
System.out.println(getConsumerById(4));
Optional[Consumer(id=4, firstName=Dmitriy, lastName=Parfen4ik, passport=Passport[id=2, number=753848], salary=3000)]
```

ВАЖНО: в данном примере мы не открываем новое соединение для поиска Passport, а переиспользуем существующее, используя перегрузку.