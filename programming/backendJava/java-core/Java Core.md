---
title: Java Core
tags:
  - JavaSE
related_topics: 
created: 2024-09-02 18:05
modified: 2024-09-11T18:27:01+03:00
difficulty: 
questions: 
notes: 
links: 
---
# Java SE
##### [[Примитивные типы данных]]
##### [[Преобразования примитивов]]
##### OOP
[[Абстракция]]
[[Инкапсуляция]]
[[Модификаторы доступа в Java]]
[[Наследование]]
[[Полиморфизм]]
[[Перегрузка метода]]
[[Композиция и агрегация]]
##### Вложенные/Внутренние классы
[[Inner class]]
[[Nested class]]
[[Анонимные внутренние классы]]
##### [[Класс Object]]
##### [[Enum]]
##### [[Switch конструкции]]
##### Array
[[Класс Arrays]]
[[Одномерный массив]]
[[Двумерный массив]]
##### CharSequence
[[Описание интерфейса CharSequence]]
[[CharBuffer and Segment]]
[[String]]
[[Строковый литерал и конструктор в String]]
[[Дедупликация Собственный пул строк]]
[[Компактные строки Java 9]]
[[StringBuilder]]
[[StringBuffer]]
##### [[Exception]]
##### [[Generics]]
##### [[Math]]
##### [[Annotations]]
##### [[Сериализация и десериализация]]

##### [[Разница между Jackson and Serialaze]]
##### [[Reflection API]]
##### [[Логирование]]
##### [[Record Class]]
##### [[Sealed Class]]
##### [[Date and Time]]
##### [[Dmdev Date and Time]]
##### [[Размер Объектов Java]]

##### [[SOLID]]

##### [[Утилитный класс ]]

##### [[Изменяемый класс ]]

##### [[Неизменяемый класс ]]

# Collections 
#### Collection
[[Interface Iterable]]
[[Interface Collection]]
[[Interface Iterator]]
[[ListIterator extends Iterator]]
[[Interfaces Comparable and Comparator]]
[[Interface Spliterator]]
##### List
[[Interface List]]

[[Vector and Stack]]
[[ArrayList]]
[[LinkedList]]

##### Set
[[Interface Set]]
[[Interface NavigableSet]]
[[Interface SortedSet]]

[[CopyOnWriteArraySet]]
[[ConcurrentSkipListSet]]
[[HashSet]]
[[LinkedHashSet]]
[[EnumSet]]
[[TreeSet]]

##### Queue
[[Interface Queue]]

[[ArrayDeque]]
[[PriorityQueue]]
[[SynchronousQueue]]
[[PriorityBlockingQueue]]
[[LinkedTransferQueue]]
[[LinkedBlockingQueue]]
[[LinkedBlockingDeque]]
[[Разница между ConcurrentLinkedDeque]]
[[ArrayBlockingQueue]]
[[ConcurrentLinkedDeque]]

[[ArrayList vs ArrayDeque]]
#### Map
[[Interface Map]]

[[HashMap]]
[[LinkedHashMap]]
[[TreeMap]]
[[WeakHashMap]]
[[IdentityHashMap]]
[[EnumMap]]
[[ConcurrentSkipListMap]]
[[ConcurrentHashMap]]

[[Hashtable]]

#### Класс Collections
[[java.util.Collections]]

# Stream API

[[Aнонимные классы]]
[[Функциональные интерфейсы. Ссылки на методы]]
[[Лямбда функции]]

[[Функциональный интерфейс Predicate]]
[[Функциональный интерфейс Function]]
[[Функциональный интерфейс Comparable]]
[[Функциональный интерфейс Comparator]]
[[Функциональный интерфейс Consumer]]
[[Функциональный интерфейс UnaryOperator]]
[[Функциональный интерфейс BinaryOperator]]
[[Функциональный интерфейс Supplier]]

[[Optional]]
[[Stream API. Часть 1. Вступление]]
[[Stream API. Часть 2. Промежуточные методы для фильтрации данных]]
[[Stream API. Часть 3. Промежуточные методы для изменения потока]]
[[Stream API. Часть 4. Промежуточные методы для изменения порядка потока]]

[[Stream API. Часть 5. Терминальные методы генерирующие результат на основании данных потока]]
[[Stream API. Часть 6. Аккумулирующие терминальные методы]]
[[Stream API. Часть 7. Терминальные методы collect]]

[[Stream API. Часть 8. Класс Collectors]]
[[Stream API. Часть 9. Терминальные методы выполняющие действия с элементами потока]]
[[Stream API. Часть 10. Примитивные специализации Stream]]

[[Stream API. Часть 11. Параллельные потоки]]
[[Stream API. Часть 12. Реализация Spliterator]]
# IO/NIO
##### Java.io
[[Input-Output]]
###### InputStream
[[InputStream]]
[[FileInputStream]]
[[ByteArrayInputStream]]
[[BufferedInputStream]]
[[DataInputStream]]
[[FilterInputStream]]
[[ObjectInputStream]]
[[PipedInputStream]]
[[SequenceInputStream]]
[[PushbackInputStream]]
###### OutputStream
[[OutputStream]]
[[FileOutputStream]]
[[ByteArrayOutputStream]]
[[BufferedOutputStream]]
[[DataOutputStream]]
[[FilterOutputStream]]
[[ObjectOutputStream]]
[[PipedOutputStream]]
[[PrintStream]]
[[SequenceOutputStream]]
[[PushbackOutputStream]]
###### Reader
[[Reader]]
[[FileReader]]
[[BufferedReader]]
[[CharArrayReader]]
[[StringReader]]
[[PushbackReader]]
[[FilterReader]]
[[LineNumberReader]]
[[InputStreamReader]]
###### Writer
[[Writer]]
[[FileWriter]]
[[BufferedWriter]]
[[CharArrayWriter]]
[[StringWriter]]
[[PrintWriter]]
[[FilterWriter]]
[[PipedWriter]]
###### Ather
[[Как работает BufferedInputStream и BufferedOutputStream в Java]]
[[Когда использовать flush с потоками]]
[[Interfaces]]
[[try-with-resources]]
##### Java.nio
[[New Input-Output]]

[[Buffer]]

[[Channel]]
[[FileChannel]]
[[SocketChannel]]
[[ServerSocketChannel]]
[[DatagramChannel]]
[[Pipe.SinkChannel и Pipe.SourceChannel]]
[[AsynchronousFileChannel]]

[[FileLock]]
[[Selector]]
[[SelectionKey]]

[[Отличия IO от NIO]]
##### Files - работа с файлами и файловой системой
[[File]]
[[Files]]
[[Отличия Files от File]]

[[Path]]
[[Paths]]
[[FileSystem]]
[[FileSystems]]
[[FileAttribute]]
[[OpenOption]]
# Multithriding 
[[Multithriding]]
[[Квантования времени выполнения потоков]]
[[Создание потоков, интерфейс Runnable методы]]
[[class java.lang.Thread - документация]]
[[Состояние потока]]
[[Прерывания потока]]
[[Атомарность операции]]
[[synhronized]]
[[synhronized collection]]
[[wait, notify, notifyAll]]
[[Потоки-демоны]]
[[Обработка непроверяемых исключений]]
[[Фабрика потоков]]
[[Virtual Threads (JEP 444)]]
[[Data Race]]
[[Решение Data Race с использованием атомарных операций Atomic]]
[[volatile]]
[[DeadLock]]
[[Избегание DeadLocks]]
[[Class ReentrantLock implements Lock]]