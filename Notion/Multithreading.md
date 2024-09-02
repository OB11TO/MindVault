## Основы

### Квантования времени выполнения потоков

- Процессор перемещается между потоками очень быстро. Из за этого складывается впечатление, что потоки обрабатываются одновременно.
- НО если у нас несколько ядер, то на каждое ядро будет выполняется отдельный поток. А остальные будут переключаться

![[images/Untitled 13.png|Untitled 13.png]]

![[images/Untitled 1 5.png|Untitled 1 5.png]]

![[images/Untitled 2 4.png|Untitled 2 4.png]]

![[images/Untitled 3 4.png|Untitled 3 4.png]]

### Создание потоков, class Thread, методы

- Использовать функциональный интерфейс Runnable

```Java
class Printer implements Runnable
{
@Override
public void run(){
}
System.out.println("Печатаю...");
}

public static void main(String[] args) {
Thread newThread = new Thread(new Printer());
newThread.start();
}
```

При использовании данного интерфейса можно создавать несколько потоков на основе одного объекта, имплементирующего Runnable.

```Java
public static void main(String[] args)
{
Printer printer = new Printer();

Thread thread1 = new Thread(printer);
thread1.start();

Thread thread2 = new Thread(printer);
thread2.start();

Thread thread3 = new Thread(printer);
thread3.start();
}
```

- Унаследоваться от Thread
    
    ### class java.lang.Thread - документация
    
    - `static int activeCount()` Возвращает оценку количества активных потоков платформы в текущей группе потоков текущего потока и его подгруппах.
    
    ```Java
    int activeThreads = Thread.activeCount();
    System.out.println("Количество активных потоков: " + activeThreads);
    ```
    
    - `static Thread currentThread()` Возвращает объект Thread для текущего потока.
    
    ```Java
    Thread current = Thread.currentThread();
    System.out.println("Текущий поток: " + current.getName());
    ```
    
    - `static void dumpStack()` Выводит трассировку стека текущего потока в стандартный поток ошибок.
    
    ```Java
    System.out.println("Дамп стека текущего потока:");
    Thread.dumpStack();
    ```
    
    - `static int enumerate(Thread[] tarray)` Копирует в указанный массив все активные потоки платформы в текущей группе потоков текущего потока и его подгруппах.
    
    ```Java
    Thread[] threads = new Thread[Thread.activeCount()];
    Thread.enumerate(threads);
    
    System.out.println("Список активных потоков:");
    for (Thread thread : threads) {
        System.out.println(thread.getName());
    }
    ```
    
    - `static Map<Thread, StackTraceElement[]> getAllStackTraces()` Возвращает карту трассировок стека для всех активных потоков платформы.
    
    ```Java
    Map<Thread, StackTraceElement[]> threadStackTraces = Thread.getAllStackTraces();
    
    for (Map.Entry<Thread, StackTraceElement[]> entry : threadStackTraces.entrySet()) {
        Thread thread = entry.getKey();
        StackTraceElement[] stackTrace = entry.getValue();
        System.out.println("Поток: " + thread.getName());
        for (StackTraceElement element : stackTrace) {
            System.out.println("  " + element.toString());
        }
    }
    ```
    
    - `ClassLoader getContextClassLoader()` Возвращает контекстный ClassLoader для этого потока.
    
    ```Java
    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
    System.out.println("Контекстный ClassLoader: " + classLoader);
    ```
    
    - `static Thread.UncaughtExceptionHandler getDefaultUncaughtExceptionHandler()` Возвращает обработчик по умолчанию, вызываемый при внезапном завершении потока из-за необработанного исключения.
    
    ```Java
    Thread.UncaughtExceptionHandler defaultHandler = Thread.getDefaultUncaughtExceptionHandler();
    System.out.println("Обработчик по умолчанию: " + defaultHandler);
    ```
    
    - `final String getName()` Возвращает имя этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Имя потока: " + Thread.currentThread().getName());
    });
    myThread.start();
    ```
    
    - `final int getPriority()` Возвращает приоритет этого потока.  
        ВАЖНО! Никто не гарантирует, что поток с более высоким приоритетом запустится первее.  
        
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Приоритет потока: " + Thread.currentThread().getPriority());
    });
    myThread.setPriority(Thread.MAX_PRIORITY); // Установка максимального приоритета
    myThread.start();
    ```
    
    - `StackTraceElement[] getStackTrace()` Возвращает массив элементов трассировки стека, представляющий дамп стека этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        StackTraceElement[] stackTrace = Thread.currentThread().getStackTrace();
        for (StackTraceElement element : stackTrace) {
            System.out.println(element.toString());
        }
    });
    myThread.start();
    ```
    
    - `Thread.State getState()` Возвращает состояние этого потока.
    
    У класса `Thread` в Java есть несколько возможных состояний (states), которые отражают его текущее состояние во время выполнения. Эти состояния определены в перечислении `Thread.State`. Вот список возможных состояний:
    
    1. `NEW`: Поток был создан, но еще не запущен.
    2. `RUNNABLE`: Поток выполняется и готов к выполнению на процессоре.
    3. `BLOCKED`: Поток ожидает блокировку для входа в критическую секцию, которая уже захвачена другим потоком.
    4. `WAITING`: Поток ожидает, пока другой поток выполнит определенное действие. Например, он может ожидать вызова метода `wait()`.
    5. `TIMED_WAITING`: Поток находится в ожидании с тайм-аутом. Это состояние возникает, когда поток ждет в течение определенного времени, после чего он автоматически продолжит выполнение. Например, при вызове метода `sleep()`.
    6. `TERMINATED`: Поток завершил выполнение и больше не активен.
    
    Эти состояния позволяют отслеживать и управлять выполнением потоков в многопоточных приложениях.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.State state = Thread.currentThread().getState();
        System.out.println("Состояние потока: " + state);
    });
    myThread.start();
    ```
    
    - `final ThreadGroup getThreadGroup()` Возвращает группу потоков этого потока или null, если поток завершился.
    
    ```Java
    Thread myThread = new Thread(() -> {
        ThreadGroup group = Thread.currentThread().getThreadGroup();
        System.out.println("Группа потоков: " + group.getName());
    });
    myThread.start();
    ```
    
    - `Thread.UncaughtExceptionHandler getUncaughtExceptionHandler()` Возвращает обработчик, вызываемый при внезапном завершении этого потока из-за необработанного исключения.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.UncaughtExceptionHandler handler = Thread.currentThread().getUncaughtExceptionHandler();
        System.out.println("Обработчик необработанных исключений: " + handler);
    });
    myThread.start();
    ```
    
    - `static boolean holdsLock(Object obj)` Возвращает true только в том случае, если текущий поток удерживает монитор объекта obj.
    - `void interrupt()` Прерывает этот поток.
    
    ```Java
    Thread myThread = new Thread(() -> {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    
    myThread.start();
    
    // Перед тем как завершиться, прерываем поток
    myThread.interrupt();
    ```
    
    - `static boolean interrupted()` Проверяет, был ли текущий поток прерван.
    
    ```Java
    Thread.currentThread().interrupt(); // Устанавливаем флаг прерывания для текущего потока
    
    if (Thread.interrupted()) {
        System.out.println("Поток был прерван.");
    } else {
        System.out.println("Поток не был прерван.");
    }
    ```
    
    - `final boolean isAlive()` Проверяет, жив ли этот поток.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
    });
    
    myThread.start();
    
    if (myThread.isAlive()) {
        System.out.println("Поток живой.");
    } else {
        System.out.println("Поток неактивен.");
    }
    ```
    
    - `final boolean isDaemon()` Проверяет, является ли этот поток демоническим.
    
    ```Java
    Thread daemonThread = new Thread(() -> {
        System.out.println("Демонический поток выполняется...");
    });
    daemonThread.setDaemon(true); // Установка потока как демонического
    daemonThread.start();
    if (daemonThread.isDaemon()) {
        System.out.println("Поток является демоническим.");
    } else {
        System.out.println("Поток не является демоническим.");
    }
    ```
    
    - `boolean isInterrupted()` Проверяет, был ли этот поток прерван.
    
    ```Java
    Thread myThread = new Thread(() -> {
        while (!Thread.currentThread().isInterrupted()) {
            System.out.println("Поток выполняется...");
        }
    });
    myThread.start();
    // Перед тем как завершиться, прерываем поток
    myThread.interrupt();
    ```
    
    - `final boolean isVirtual()` Возвращает true, если этот поток является виртуальным потоком.
    - `final void join()` Ожидает завершения этого потока.
    
    ```Java
    Thread thread1 = new Thread(() -> {
        System.out.println("Поток 1 выполняется...");
    });
    Thread thread2 = new Thread(() -> {
        System.out.println("Поток 2 выполняется...");
    });
    thread1.start();
    thread2.start();
    try {
        thread1.join();
        thread2.join();
    } catch (InterruptedException e) {
        System.out.println("Поток был прерван!");
    }
    System.out.println("Основной поток завершил выполнение.");
    ```
    
    - `final void join(long millis)` Ожидает не более millis миллисекунд завершения этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        myThread.join(3000); // Ожидаем не более 3 секунд
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `final void join(long millis, int nanos)` Ожидает не более millis миллисекунд плюс nanos наносекунд завершения этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        myThread.join(3, 500000); // Ожидаем не более 3 секунд и 500 миллисекунд
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `final boolean join(Duration duration)` Ожидает завершения этого потока в течение указанной продолжительности.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        Duration duration = Duration.ofSeconds(3); // Ожидаем не более 3 секунд
        boolean joined = myThread.join(duration);
        if (joined) {
            System.out.println("Поток успешно завершил выполнение.");
        } else {
            System.out.println("Ожидание завершения потока превысило указанный интервал.");
        }
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `static Thread.Builder.OfPlatform ofPlatform()` Возвращает билдер для создания платформенного потока или фабрики потоков, создающей платформенные потоки.
    - `static Thread.Builder.OfVirtual ofVirtual()` Возвращает билдер для создания виртуального потока или фабрики потоков, создающей виртуальные потоки.
    - `static void onSpinWait()` Указывает, что вызывающий поток временно не может продвигаться, пока не произойдут одно или несколько действий со стороны других действий.
    
    ```Java
    System.out.println("Начало выполнения программы");
    while (true) {
        // Имитация ожидания
        Thread.onSpinWait();
        System.out.println("Выполнение работы...");
    }
    ```
    
    - `void run()` Этот метод выполняется потоком при его запуске.
    - `void setContextClassLoader(ClassLoader cl)` Устанавливает контекстный ClassLoader для этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        ClassLoader contextClassLoader = Thread.currentThread().getContextClassLoader();
        System.out.println("Контекстный ClassLoader: " + contextClassLoader);
    });
    myThread.start();
    ```
    
    - `final void setDaemon(boolean on)` Помечает этот поток как демонический или недемонический.
    
    ```Java
    Thread daemonThread = new Thread(() -> {
        System.out.println("Демонический поток выполняется...");
    });
    daemonThread.setDaemon(true); // Установка потока как демонического
    daemonThread.start();
    ```
    
    - `static void setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler ueh)` Устанавливает обработчик по умолчанию, вызываемый при внезапном завершении потока из-за необработанного исключения, если для этого потока не был определен другой обработчик.
    
    ```Java
    Thread.setDefaultUncaughtExceptionHandler((thread, throwable) -> {
        System.out.println("Поток " + thread.getName() + " завершился с исключением: " + throwable.getMessage());
    });
    ```
    
    - `final void setName(String name)` Изменяет имя этого потока на заданное имя.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток с именем: " + Thread.currentThread().getName());
    });
    myThread.setName("Мой Поток"); // Установка имени потока
    myThread.start();
    ```
    
    - `final void setPriority(int newPriority)` Изменяет приоритет этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Приоритет потока: " + Thread.currentThread().getPriority());
    });
    myThread.setPriority(Thread.MAX_PRIORITY); // Установка максимального приоритета
    myThread.start();
    ```
    
    - `void setUncaughtExceptionHandler(Thread.UncaughtExceptionHandler ueh)` Устанавливает обработчик, вызываемый при внезапном завершении этого потока из-за необработанного исключения.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.currentThread().setUncaughtExceptionHandler((thread, throwable) -> {
            System.out.println("Поток " + thread.getName() + " завершился с исключением: " + throwable.getMessage());
        });
        // Генерируем исключение
        throw new RuntimeException("Исключение в потоке");
    });
    myThread.start();
    ```
    
    - `static void sleep(long millis)` Заставляет текущий выполняющийся поток спать (временно прекратить выполнение) на указанное количество миллисекунд, с учетом точности системных таймеров и планировщиков.
        
        Более того, в методе **sleep** есть автоматическая проверка переменной **isInterrupted**. Если нить вызовет метод **sleep**, то этот метод сначала проверит, а не установлена ли для текущей (вызвавшей его нити) переменная **isInterrupted** в true. И если установлена, то метод не будет спать, а выкинет исключение **InterruptedException**.
        
    
    ```Java
    public class SleepExample {
        public static void main(String[] args) {
            System.out.println("Начало выполнения программы");
    
            try {
                // Приостанавливаем выполнение текущего потока на 3 секунды
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                // Обработка исключения, если поток был прерван
                e.printStackTrace();
            }
    
            System.out.println("Завершение программы");
        }
    }
    ```
    
    - `static void sleep(long millis, int nanos)` Заставляет текущий выполняющийся поток спать на указанное количество миллисекунд и наносекунд, с учетом точности системных таймеров и планировщиков.
        
        ```Java
        System.out.println("Начало выполнения программы");
        
        try {
            // Приостанавливаем выполнение текущего потока на 2 секунды и 500 миллисекунд
            Thread.sleep(2, 500000);
        } catch (InterruptedException e) {
            // Обработка исключения, если поток был прерван
            e.printStackTrace();
        }
        
        System.out.println("Завершение программы");
        ```
        
    - `static void sleep(Duration duration)` Заставляет текущий выполняющийся поток спать на указанную продолжительность, с учетом точности системных таймеров и планировщиков.
        
        ```Java
        import java.time.Duration;
        
        public class SleepExample {
            public static void main(String[] args) {
                System.out.println("Начало выполнения программы");
        
                try {
                    Duration duration = Duration.ofSeconds(3); // Создаем объект Duration на 3 секунды
                    Thread.sleep(duration); // Засыпаем на указанное количество времени
                } catch (InterruptedException e) {
                    // Обработка исключения, если поток был прерван
                    e.printStackTrace();
                }
        
                System.out.println("Завершение программы");
            }
        }
        ```
        
    - `void start()` Планирует начало выполнения этого потока.
    - `static Thread startVirtualThread(Runnable task)` Создает виртуальный поток для выполнения задачи и планирует его выполнение.
    - `final long threadId()` Возвращает идентификатор этого потока.
    
    ```Java
    long threadId = Thread.currentThread().getId();
    System.out.println("Идентификатор текущего потока: " + threadId);
    ```
    
    - `String toString()` Возвращает строковое представление этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Строковое представление потока: " + Thread.currentThread());
    });
    myThread.start();
    ```
    
    - `static void yield()`Подсказка планировщику, что текущий поток готов уступить использование процессора.
    
    ```Java
    Thread thread1 = new Thread(() -> {
        for (int i = 0; i < 5; i++) {
            System.out.println("Поток 1 выполняется...");
            Thread.yield(); // Уступаем процессорное время
        }
    });
    
    Thread thread2 = new Thread(() -> {
        for (int i = 0; i < 5; i++) {
            System.out.println("Поток 2 выполняется...");
            Thread.yield(); // Уступаем процессорное время
        }
    });
    
    thread1.start();
    thread2.start();
    ```
    

  

```Java
class Printer extends Thread
{
@Override
public void run(){
}
System.out.println("Печатаю...");
}

public static void main(String[] args) {
Printer printer = new Printer();
printer.start();
}
```

У такого решения есть минусы :

- Вам может понадобиться запустить несколько нитей на основе одного единственного объекта, как это сделано в примере с 3 потоками выше.
- Если вы унаследовались от класса Thread, вы не можете добавить еще один класс-родитель к своему классу.

  

### Состояние потока

- Получить состояние из `State` - Enum

![[images/Untitled 4 4.png|Untitled 4 4.png]]

![[images/Untitled 5 4.png|Untitled 5 4.png]]

- `**ЕСЛИ В ОДНОМ ПОТОКЕ ПРОИСХОДИТ ИСКЛЮЧЕНИЕ, ТО ОСТАЛЬНЫЕ ПОТОКИ ПРОДОЛЖАЮТ ВЫПОЛНЯТСЯ**`

### Прерывания потока

- Метод interapted можно вызвать для потока.
- И метод sleep тоже проверяет состояние interapted
- Нужно продумывать логику для работы с этим. и пробрасывать исключ

### Атомарность операции

  

### synhronized

![[images/Untitled 6 3.png|Untitled 6 3.png]]

![[images/Untitled 7 3.png|Untitled 7 3.png]]

![[images/Untitled 8 3.png|Untitled 8 3.png]]

![[images/Untitled 9 3.png|Untitled 9 3.png]]

![[images/Untitled 10 2.png|Untitled 10 2.png]]

### synhronized collection

- ==Захват монитора является трудоемкой операцией.== Занимает процессорное время и влияет на перфоманс приложение.
- ==Такое использовать только в крайней случае==. Когда уже есть список.
- `НУЖНО ИСПОЛЬЗОВАТЬ Cuncurrent`

![[images/Untitled 11 2.png|Untitled 11 2.png]]

### Потоки-демоны

- ==Если в программе остались только потоки демоны, то программа завершается.==
- Если ==мы становили поток, как демона, то все наследующиеся или созданные от него потоки будут тоже== ==`демоны`====.==
- Если ==запустить поток и затем сделать его== ==`демоном`====, то будет исключение.==
- Нужно быть аккуратным с потоками-демонами и ==не создавать их для операции чтения файла, работы с бд.==
- Если ==все потоки завершились и программа завершится, то поток демон не выполнит блок кода finaly==

![[images/Untitled 12 2.png|Untitled 12 2.png]]

![[images/Untitled 13 2.png|Untitled 13 2.png]]

![[Untitled 14.png]]

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Date;

public class SessionCleanupDaemon {

    private static final String DB_URL = "jdbc:mysql://localhost:3306/yourdatabase";
    private static final String USER = "yourusername";
    private static final String PASS = "yourpassword";

    public static void main(String[] args) {
        Thread cleanupThread = new Thread(new Runnable() {
            @Override
            public void run() {
                try (Connec
                tion conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
                    while (true) {
                        cleanupOldSessions(conn);
                        Thread.sleep(60000); // Очистка выполняется каждую минуту
                    }
                } catch (InterruptedException | SQLException e) {
                    e.printStackTrace();
                }
            }

            private void cleanupOldSessions(Connection conn) {
                String sql = "DELETE FROM sessions WHERE last_access_time < ?";
                try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
                    // Удаляем сессии, неактивные более 30 минут
                    pstmt.setTimestamp(1, new java.sql.Timestamp(System.currentTimeMillis() - 30 * 60 * 1000));
                    int deletedRows = pstmt.executeUpdate();
                    System.out.println("Удалено устаревших сессий: " + deletedRows + " в " + new Date());
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        });

        // Устанавливаем поток как демон
        cleanupThread.setDaemon(true);
        cleanupThread.start();

        // Основной поток сервера, эмулирующий работу сервера
        try {
            while (true) {
                System.out.println("Сервер работает...");
                Thread.sleep(10000); // Эмуляция работы сервера
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### Обработка непроверяемых исключений

- ==Интерфейс Runnable может вывести исключения==, но как их отлавливать, `перед тем, как поток умер`. Очень просто ==есть функ. интерфейс, который может это делать.==
- ==Также для всех потоков можно сделать его глобальным.==

![[Untitled 15.png]]

### Фабрика потоков

![[Untitled 16.png]]

  

### Virtual Threads (JEP 444)

Виртуальные потоки, которые много лет разрабатывались в рамках проекта [Loom](https://openjdk.org/projects/loom/) и появились в [Java 19](https://openjdk.org/jeps/425) в режиме preview, теперь наконец-то стали стабильными.

Виртуальные потоки, в отличие от потоков операционной системы,  
являются легковесными и могут создаваться в огромном количестве  
(миллионы экземпляров). Это свойство должно значительно облегчить  
написание конкурентных программ, поскольку позволит применять простой  
подход «один запрос – один поток» (или «одна задача – один поток») и не  
прибегать к более сложным асинхронному или реактивному программированию.  
При этом миграция на виртуальные потоки уже существующего кода должна  
быть максимально простой, потому что виртуальные потоки являются  
экземплярами существующего класса  
[`java.lang.Thread`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html) и практически полностью совместимы с классическими потоками: поддерживают стек-трейсы, [`interrupt()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html#interrupt()), [`ThreadLocal`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ThreadLocal.html) и т.д.

Виртуальные потоки реализованы поверх обычных потоков и существуют  
только для JVM, но не для операционной системы (отсюда и название  
«виртуальные»). Поток, на котором в данный момент выполняется  
виртуальный поток, называется потоком-носителем. Если потоки платформы  
полагаются на планировщик операционной системы, то планировщиком для  
виртуальных потоков является  
[`ForkJoinPool`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ForkJoinPool.html).  
Когда виртуальный поток блокируется на некоторой блокирующей операции,  
то он размонтируется от своего потока-носителя, что позволяет  
потоку-носителю примонтировать другой виртуальный поток и продолжить  
работу. Такой режим работы и дешевизна виртуальных потоков позволяет им  
очень хорошо масштабироваться. Однако на данный момент есть два  
исключения:  
`synchronized` блоки и JNI. При их выполнении  
виртуальный поток не может быть размонтирован, поскольку он привязан к  
своему потоку-носителю. Такое ограничение может препятствовать  
масштабированию. Поэтому при желании максимально использовать потенциал  
виртуальных потоков рекомендуется избегать  
`synchronized` блоков и операции JNI, которые выполняются часто или занимают длительное время.

Несмотря на привлекательность виртуальных потоков, вовсе  
необязательно предпочитать только их и всегда избегать классических  
потоков. Например, для задач, интенсивно и долго использующих CPU, лучше  
подойдут обычные потоки. Или если нужен поток, не являющийся  
[демоном](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html#setDaemon(boolean)), то также придётся использовать обычный поток, потому что виртуальный поток всегда является демоном.

Для создания виртуальных потоков и работы с ними появилось следующее API:

- [`Thread.Builder`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.Builder.html) – билдер потоков. Например, виртуальный поток можно создать путём вызова `Thread.ofVirtual().name("name").unstarted(runnable)`.
- [`Thread.startVirtualThread(Runnable)`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html#startVirtualThread(java.lang.Runnable)) – создаёт и сразу же запускает виртуальный поток.
- [`Thread.isVirtual()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html#isVirtual()) – проверяет, является ли поток виртуальным.
- [`Executors.newVirtualThreadPerTaskExecutor()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/Executors.html#newVirtualThreadPerTaskExecutor()) – возвращает исполнитель, который создаёт новый виртуальный поток на каждую задачу.

Для виртуальных потоков также добавилась поддержка в инструментарии JDK (дебаггер, JVM TI, Java Flight Recorder).

### Data Race, synchronized, volatile, Atomics из java.util.concurrent.atomic

Проблема: Гонка за данными (Data Race)

```Java
public class Counter {
    private int count = 0;

    public void increment() {
        count++; // Неатомарная операция инкремента
    }

    public int getCount() {
        return count;
    }
}
```

В этом примере `Counter` представляет собой простой счетчик, который не использует атомарные операции для инкремента. Проблема здесь заключается в том, что если несколько потоков вызывают `increment()` одновременно, то может произойти гонка за данными, и значение `count` может быть неверным.

### Synchronized

`Synchronized` (синхронизированный) - это ключевое слово в Java, которое используется для обеспечения потокобезопасности и синхронизации доступа к общим ресурсам (переменным, методам, объектам) из нескольких потоков. Оно гарантирует, что только один поток может выполнять код, помеченный как синхронизированный, в любой момент времени.

Основные концепции и способы использования `synchronized`:

1. **Синхронизация методов:** Вы можете сделать целый метод синхронизированным, добавив ключевое слово `synchronized` к его объявлению. Это означает, что только один поток может одновременно выполнять этот метод на данном объекте.
    
    ```Java
    public synchronized void synchronizedMethod() {
        // Код, требующий синхронизации
    }
    ```
    
2. **Синхронизация блоков кода:** Вы также можете синхронизировать конкретные блоки кода, используя ключевое слово `synchronized` и объект блокировки. Это более гибкий способ, который позволяет управлять областью синхронизации.
    
    ```Java
    synchronized (lockObject) {
        // Код, требующий синхронизации
    }
    ```
    
3. **Статическая синхронизация:** В случае, когда нужно синхронизировать статические методы или обращаться к статическим полям класса, вы можете использовать ключевое слово `synchronized` с `static`. Это синхронизирует доступ ко всему классу, а не к конкретному экземпляру.
    
    ```Java
    public static synchronized void synchronizedStaticMethod() {
        // Код, требующий синхронизации для статического метода
    }
    ```
    
    ```Java
    class MyClass
    {
    private static String name1 = "Оля";
    private static String name2 = "Лена";
    
    public synchronized void swap()
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    
    public static synchronized void swap2()
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    
    
    //Что происходит на самом деле
    class MyClass
    {
    private static String name1 = "Оля";
    private static String name2 = "Лена";
    
    public void swap()
    {
    synchronized (this)
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    
    public static void swap2()
    {
    synchronized (MyClass.class)
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    }
    ```
    

Использование `synchronized` позволяет избегать состояний гонки, когда несколько потоков пытаются изменить общие данные одновременно, что может привести к непредсказуемым и нежелательным результатам. Однако следует помнить, что неправильное использование синхронизации может привести к проблемам, таким как блокировки и замедление производительности. Поэтому важно заботиться о правильном проектировании и использовании синхронизации в многопоточных приложениях.

### volatile

Ключевое слово `volatile` в Java используется для обеспечения видимости изменений переменной между потоками и предотвращения оптимизаций, которые могли бы нарушить эту видимость. То есть простыми словами, каждый поток имеет свой собственный кэш и volatile запрещает его использовать для потоков. Данные всегда будут браться из основной памяти и записываться в основную память.

```Java
public class SynchronizedVolatileCounter {
    private volatile int count = 0;

    public synchronized void increment() {
        count++; // Атомарная операция благодаря synchronized
    }

    public int getCount() {
        return count;
    }
}
```

Основные цели использования `volatile` включают в себя:

1. **Обеспечение видимости изменений:** Когда переменная объявлена как `volatile`, изменения, внесенные одним потоком в эту переменную, будут немедленно видны всем другим потокам. Это гарантирует, что обновленное значение переменной не будет кэшироваться локально в потоках и будет доступно всем.
2. **Предотвращение переупорядочивания инструкций:** Компилятор и/или аппаратное обеспечение могут переупорядочивать инструкции в коде для оптимизации производительности. `volatile` предотвращает переупорядочивание инструкций вокруг операций с `volatile` переменными, что может быть критически важно в многопоточных сценариях.
3. **Синхронизация потоков:** Хотя `volatile` не предоставляет полной синхронизации, он обеспечивает атомарность операций чтения и записи для примитивных типов данных (таких как `int`, `boolean`, `long`, `float`, `double`) и может использоваться для создания простых синхронизационных механизмов.

### Решение Data Race с использованием атомарных операций (Atomic):

```Java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet(); // Атомарная операция инкремента
    }

    public int getCount() {
        return count.get();
    }
}
```

Здесь `AtomicCounter` использует `AtomicInteger`, который предоставляет атомарные операции для инкремента и получения значения. Это обеспечивает безопасность при работе с счетчиком в многопоточной среде и предотвращает гонку за данными.

- `AtomicBoolean`: Булевое значение, которое может быть атомарно обновлено.
- `AtomicInteger`: Целочисленное значение, которое может быть атомарно обновлено.
- `AtomicIntegerArray`: Массив целых чисел, в котором элементы могут быть атомарно обновлены.
- `AtomicIntegerFieldUpdater<T>`: Утилита, использующая рефлексию и позволяющая атомарные обновления для полей с типом `volatile int` в указанных классах.
- `AtomicLong`: Длинное целочисленное значение, которое может быть атомарно обновлено.
- `AtomicLongArray`: Массив длинных целых чисел, в котором элементы могут быть атомарно обновлены.
- `AtomicLongFieldUpdater<T>`: Утилита, использующая рефлексию и позволяющая атомарные обновления для полей с типом `volatile long` в указанных классах.
- `AtomicMarkableReference<V>`: Атомарный объект, который содержит ссылку на объект и бит метки, которые могут быть атомарно обновлены.
- `AtomicReference<V>`: Ссылка на объект, которая может быть атомарно обновлена.
- `AtomicReferenceArray<E>`: Массив ссылок на объекты, в котором элементы могут быть атомарно обновлены.
- `AtomicReferenceFieldUpdater<T,V>`: Утилита, использующая рефлексию и позволяющая атомарные обновления для полей с типом `volatile reference` в указанных классах.
- `AtomicStampedReference<V>`: Атомарный объект, который содержит ссылку на объект и целое число "метку", которые могут быть атомарно обновлены.
- `DoubleAccumulator`: Один или несколько переменных, которые вместе поддерживают текущее значение типа `double`, обновляемое с использованием предоставленной функции.
- `DoubleAdder`: Один или несколько переменных, которые вместе поддерживают сумму типа `double`, начальное значение которой равно нулю.
- `LongAccumulator`: Один или несколько переменных, которые вместе поддерживают текущее значение типа `long`, обновляемое с использованием предоставленной функции.
- `LongAdder`: Один или несколько переменных, которые вместе поддерживают сумму типа `long`, начальное значение которой равно нулю.

### DeadLock

### Что такое

Deadlock (в переводе с английского — "тупик") — это ситуация, когда два или более потока взаимодействуют в таком образе, что каждый из них ожидает, что другой поток освободит ресурс, который он сам нуждается для продолжения выполнения, и, следовательно, оба потока остаются заблокированными вечно, без возможности завершения своей работы. Это приводит к замороженному состоянию программы и блокировке выполнения потоков.

Пример Deadlock на Java:

```Java
public class DeadlockExample {
    private static Object lock1 = new Object();
    private static Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Поток 1: Удерживает lock1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 1: Ожидает lock2...");
                synchronized (lock2) {
                    System.out.println("Поток 1: Захватил lock2.");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Поток 2: Удерживает lock2...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 2: Ожидает lock1...");
                synchronized (lock1) {
                    System.out.println("Поток 2: Захватил lock1.");
                }
            }
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

В этом примере есть два потока, каждый из которых пытается получить доступ к двум ресурсам (lock1 и lock2), которые другие потоки могут также попытаться захватить. При выполнении программы поток 1 захватывает lock1 и ожидает lock2, в то время как поток 2 захватывает lock2 и ожидает lock1. Оба потока ожидают друг друга, что приводит к Deadlock, и выполнение программы зависает.

### Избегание DeadLocks

Избегание Deadlock в Java можно достичь, используя правильную стратегию управления блокировками и избегая циклических зависимостей между ресурсами. Давайте рассмотрим два примера:

- **Использование Упорядочивания Ресурсов**: Упорядочивание ресурсов позволяет всем потокам получать доступ к ресурсам в одном и том же порядке, что и другие потоки, тем самым избегая циклических зависимостей.

```Java
public class DeadlockAvoidanceExample {
    private static Object resource1 = new Object();
    private static Object resource2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (resource1) {
                System.out.println("Поток 1: Удерживает resource1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 1: Ожидает resource2...");
                synchronized (resource2) {
                    System.out.println("Поток 1: Захватил resource2.");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (resource1) {
                System.out.println("Поток 2: Удерживает resource1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 2: Попытка получения resource2...");
                synchronized (resource2) {
                    System.out.println("Поток 2: Захватил resource2.");
                }
            }
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

В этом примере оба потока получают доступ к ресурсам (resource1 и resource2) в одном и том же порядке, что предотвращает возможность Deadlock.

- **Использование java.util.concurrent.locks.Lock**: Java предоставляет более гибкие средства управления блокировками с использованием класса `ReentrantLock` из пакета `java.util.concurrent.locks`. Этот класс позволяет легко управлять блокировками и предоставляет методы для избежания Deadlock, такие как `tryLock()` и `lockInterruptibly()`.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class DeadlockAvoidanceWithLockExample {
    private static Lock lock1 = new ReentrantLock();
    private static Lock lock2 = new ReentrantLock();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            lock1.lock();
            System.out.println("Поток 1: Удерживает lock1...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Поток 1: Ожидает lock2...");
            lock2.lock();
            System.out.println("Поток 1: Захватил lock2.");
            lock1.unlock();
            lock2.unlock();
        });

        Thread thread2 = new Thread(() -> {
            lock1.lock();
            System.out.println("Поток 2: Удерживает lock1...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Поток 2: Ожидает lock2...");
            lock2.lock();
            System.out.println("Поток 2: Захватил lock2.");
            lock1.unlock();
            lock2.unlock();
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

В этом примере мы используем `ReentrantLock` для управления блокировками. Этот класс позволяет легко управлять блокировками и предотвращать Deadlock с помощью методов блокировки и разблокировки.

### Class ReentrantLock implements Lock

- `int getHoldCount()`: Возвращает количество удержаний блокировки текущим потоком.

```Java
Lock lock = new ReentrantLock();
lock.lock();
int holdCount = lock.getHoldCount();
System.out.println("Количество удержаний: " + holdCount); // Выведет "Количество удержаний: 1"
lock.unlock();
```

- `protected Thread getOwner()`: Возвращает поток, владеющий блокировкой в данный момент, или null, если блокировка не занята.

```Java
Lock lock = new ReentrantLock();
lock.lock();
Thread owner = lock.getOwner();
System.out.println("Владелец блокировки: " + owner); // Выведет информацию о текущем потоке
lock.unlock();
```

- `protected Collection<Thread> getQueuedThreads()`: Возвращает коллекцию потоков, которые могут ожидать блокировку.

```Java
Lock lock = new ReentrantLock();
lock.lock();
Collection<Thread> queuedThreads = lock.getQueuedThreads();
System.out.println("Ожидающие потоки: " + queuedThreads); // Выведет информацию о потоках в ожидании
lock.unlock();
```

- `int getQueueLength()`: Возвращает оценку количества потоков, ожидающих получения блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
int queueLength = lock.getQueueLength();
System.out.println("Длина очереди ожидания: " + queueLength); // Выведет "Длина очереди ожидания: 0"
lock.unlock();
```

- `protected Collection<Thread> getWaitingThreads(Condition condition)`: Возвращает коллекцию потоков, ожидающих указанное условие.

```Java
Lock lock = new ReentrantLock();
Condition condition = lock.newCondition();
lock.lock();
Collection<Thread> waitingThreads = lock.getWaitingThreads(condition);
System.out.println("Ожидающие потоки для условия: " + waitingThreads); // Выведет информацию о потоках в ожидании
lock.unlock();
```

- `int getWaitQueueLength(Condition condition)`: Возвращает оценку количества потоков, ожидающих указанное условие.

```Java
Lock lock = new ReentrantLock();
Condition condition = lock.newCondition();
lock.lock();
int waitQueueLength = lock.getWaitQueueLength(condition);
System.out.println("Длина очереди ожидания для условия: " + waitQueueLength); // Выведет "Длина очереди ожидания для условия: 0"
lock.unlock();
```

- `boolean hasQueuedThread(Thread thread)`: Проверяет, ожидает ли указанный поток получение блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
boolean hasQueued = lock.hasQueuedThread(Thread.currentThread());
System.out.println("Текущий поток ожидает блокировку: " + hasQueued); // Выведет "Текущий поток ожидает блокировку: true"
lock.unlock();
```

- `boolean hasQueuedThreads()`: Проверяет, ожидают ли какие-либо потоки получение блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
boolean hasQueuedThreads = lock.hasQueuedThreads();
System.out.println("Ожидают ли потоки блокировку: " + hasQueuedThreads); // Выведет "Ожидают ли пот
```

- `boolean hasWaiters(Condition condition)`: Проверяет, есть ли потоки, ожидающие указанное условие, связанное с блокировкой.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

public class HasWaitersExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        Condition condition = lock.newCondition();
        
        // Проверяем, есть ли ожидающие потоки по условию
        if (lock.hasWaiters(condition)) {
            // Есть ожидающие потоки
        } else {
            // Нет ожидающих потоков
        }
    }
}
```

- `boolean isFair()`: Возвращает true, если блокировка установлена как справедливая.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsFairExample {
    public static void main(String[] args) {
        Lock fairLock = new ReentrantLock(true); // Создаем справедливую блокировку
        Lock nonFairLock = new ReentrantLock(); // Создаем несправедливую блокировку
        
        boolean isFair1 = fairLock.isFair();
        boolean isFair2 = nonFairLock.isFair();
        
        System.out.println("Справедливая блокировка: " + isFair1);
        System.out.println("Несправедливая блокировка: " + isFair2);
    }
}
```

- `boolean isHeldByCurrentThread()`: Проверяет, удерживается ли блокировка текущим потоком.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsHeldByCurrentThreadExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        // Проверяем, удерживается ли блокировка текущим потоком
        if (lock.isHeldByCurrentThread()) {
            // Блокировка удерживается текущим потоком
        } else {
            // Блокировка не удерживается текущим потоком
        }
    }
}
```

- `boolean isLocked()`: Проверяет, удерживается ли блокировка каким-либо потоком.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsLockedExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        // Проверяем, удерживается ли блокировка
        if (lock.isLocked()) {
            // Блокировка удерживается каким-либо потоком
        } else {
            // Блокировка не удерживается ни одним потоком
        }
    }
}
```

- `void lock()`: Захватывает блокировку.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        lock.lock(); // Захватываем блокировку
        
        try {
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
```

- `void lockInterruptibly()`: Захватывает блокировку, если текущий поток не был прерван.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockInterruptiblyExample {
    public static void main(String[] args) throws InterruptedException {
        Lock lock = new ReentrantLock();

        try {
            lock.lockInterruptibly(); // Попытка захвата блокировки, может быть прервана
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
```

- `Condition newCondition()`: Возвращает экземпляр Condition для использования с этой блокировкой.

```Java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class NewConditionExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        Condition condition = lock.newCondition(); // Создание экземпляра Condition

        // Теперь можно использовать condition для ожидания и сигнализации
    }
}
```

- `String toString()`: Возвращает строку, идентифицирующую эту блокировку и ее состояние.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ToStringExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();

        String lockInfo = lock.toString();
        System.out.println(lockInfo);
    }
}
```

- `boolean tryLock()`: Захватывает блокировку, если она не удерживается другим потоком в момент вызова.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class TryLockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        if (lock.tryLock()) {
            try {
                // Критическая секция, блокировка захвачена
            } finally {
                lock.unlock(); // Освобождение блокировки
            }
        } else {
            // Блокировка уже удерживается другим потоком
        }
    }
}
```

- `boolean tryLock(long timeout, TimeUnit unit)`: Захватывает блокировку, если она не удерживается другим потоком в течение указанного времени ожидания и текущий поток не был прерван.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.TimeUnit;

public class TryLockWithTimeoutExample {
    public static void main(String[] args) throws InterruptedException {
        Lock lock = new ReentrantLock();
        
        if (lock.tryLock(2, TimeUnit.SECONDS)) {
            try {
                // Критическая секция, блокировка захвачена
            } finally {
                lock.unlock(); // Освобождение блокировки
            }
        } else {
            // Блокировка уже удерживается другим потоком или ожидание истекло
        }
    }
}
```

- `void unlock()`: Освобождает блокировку.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class UnlockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();

        try {
            lock.lock(); // Захват блокировки
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
```

  

### wait, notify, notifyAll

### Синхронизированные коллекции

В классе Collections существуют методы, которые возвращают синхронизированную обертку над коллекцией:

- `static <T> Collection<T> synchronizedCollection(Collection<T> c)` - возвращает синхронизированную (потокобезопасную) коллекцию, поддерживаемую указанной коллекцией.
- `static <T> List<T> synchronizedList(List<T> list)` - возвращает синхронизированный (потокобезопасный) список, поддерживаемый указанным списком.
- `static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)` - возвращает синхронизированную (thread-safe) map, которая поддерживается указанной map.
- `static <K,V> NavigableMap<K,V> synchronizedNavigableMap(NavigableMap<K,V> m)` - возвращает синхронизированную (thread-safe) NavigableMap, которая поддерживается указанной NavigableMap.
- `static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)` - возвращает синхронизированную (thread-safe) NavigableSet, которая поддерживается указанным NavigableSet.
- `static <T> Set<T> synchronizedSet(Set<T> s)` - возвращает синхронизированный (thread-safe) set, который поддерживается указанным set.
- `static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)` - возвращает синхронизированный (thread-safe) sorted map, который поддерживается указанным sorted map.
- `static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)` - возвращает синхронизированный (thread-safe) sorted set, который поддерживается указанным sorted set.