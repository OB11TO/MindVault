---
title: OutputStream
tags:
  - IO-NIO
related_topics:
  - "[[Когда использовать flush с потоками]]"
created: 2024-09-10 16:06
modified: 2024-09-10T17:28:32+03:00
questions: 
notes: 
links: 
---
Каждый из этих классов предоставляет различные способы работы с байтовыми потоками в Java:

1. **`OutputStream`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков вывода байтов.
2. **`FileOutputStream`** —<mark class="hltr-purple"> запись байтов в файл.</mark>
3. **`ByteArrayOutputStream`** — запись <mark class="hltr-blue">байтов в массив байтов в памяти</mark>.
4. **`BufferedOutputStream`** — <mark class="hltr-purple">буферизированная запись</mark> для повышения производительности.
5. **`DataOutputStream`** — запись <mark class="hltr-blue">примитивных типов данных в байтовый поток</mark>.
6. **`FilterOutputStream`** — базовый класс для <mark class="hltr-purple">потоков-декораторов.</mark>
7. **`ObjectOutputStream`** — <mark class="hltr-purple">сериализация</mark> объектов.
8. **`PipedOutputStream`** — <mark class="hltr-blue">межпотоковая</mark> запись данных через каналы.
9. **`PrintStream`** — <mark class="hltr-red">удобная запись строк и данных с методами</mark> `print()` и `println()`.
10. **`SequenceOutputStream`** — <mark class="hltr-purple">объединение нескольких потоков вывода в один.</mark>


Для работы с потоками вывода, которые обрабатывают данные в виде **байтов**, Java предоставляет несколько базовых классов, находящихся в пакете `java.io`. Эти классы используются для записи данных в различные источники: файлы, массивы байтов, сетевые соединения и т.д.

Вот краткое описание каждого базового класса потоков вывода:

---

### 1. **`OutputStream`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков вывода, которые работают с байтами.
- **Методы**:
    - `void write(int b)`: Записывает один байт данных.
    - `void write(byte[] b)`: Записывает массив байтов.
    - `void flush()`: Принудительно записывает данные из буфера (если используется буферизация).
    - `void close()`: Закрывает поток и освобождает все ресурсы.
- **Использование**: Основной класс, от которого наследуются все остальные классы вывода.

---

### 2. **`FileOutputStream`**

- **Описание**: Поток <mark class="hltr-purple">вывода для записи байтов в файл.</mark>
- **Особенности**:
    - Используется <mark class="hltr-yellow">для записи данных в файл на файловой системе.</mark>
    - Можно указать, дописывать ли данные в файл или перезаписывать его (через соответствующий конструктор).
- **Пример**:
    
```java
FileOutputStream fos = new FileOutputStream("output.txt");
fos.write("Hello, World!".getBytes());
fos.close();

```


---

### 3. **`ByteArrayOutputStream`**

- **Описание**: Поток в<mark class="hltr-purple">ывода для записи байтов в массив байтов в памяти</mark>.
- **Особенности**:
    - Данные <mark class="hltr-yellow">записываются в массив байтов, который находится в оперативной памяти, а не на диске.</mark>
    - Полезен для работы с временными данными.
- **Методы**:
    - `toByteArray()`: Возвращает все записанные байты в виде массива байтов.
- **Пример**:

```java
ByteArrayOutputStream baos = new ByteArrayOutputStream();
baos.write("Hello".getBytes());
byte[] data = baos.toByteArray();
baos.close();

```

---

### 4. **`BufferedOutputStream`**

[[Как работает BufferedInputStream и BufferedOutputStream в Java]]

- **Описание**: Поток вывода с <mark class="hltr-red">буферизацией</mark> для повышения производительности.
- **Особенности**:
    - <mark class="hltr-green2">Оборачивает другой поток вывода</mark> (например, `FileOutputStream`) и использует буфер для временного хранения данных перед записью на диск.
    - Уменьшает количество операций ввода-вывода, повышая эффективность.
    - Он <mark class="hltr-yellow">улучшает производительность путем минимизации фактических обращений к файловой системе.</mark>
- **Пример**:
    
```java
 FileOutputStream fos = new FileOutputStream("output.txt");
BufferedOutputStream bos = new BufferedOutputStream(fos);
bos.write("Buffered data".getBytes());
bos.flush();  // Принудительная запись данных из буфера
bos.close();

```

```java
  import java.io.BufferedOutputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    
    public class BufferedOutputStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("output.txt");
                BufferedOutputStream bos = new BufferedOutputStream(fos);
    
                // Записываем байты в буферизованный поток вывода
                bos.write(65); // Записываем ASCII-код символа 'A'
                bos.write(new byte[]{66, 67, 68}); // Записываем массив байтов
    
                // Сбрасываем буфер и записываем оставшиеся символы в файл
                bos.flush();
    
                // Закрываем потоки
                bos.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

```


---

### 5. **`DataOutputStream`**

- **Описание**: <mark class="hltr-purple">Поток вывода для записи примитивных типов данных</mark> (int, float, long и т.д.).
- **Особенности**:
    - Позволяет<mark class="hltr-yellow"> записывать примитивные типы данных</mark> (например, числа и строки) в <mark class="hltr-yellow">байтовый поток в формате, подходящем для последующего чтения с использованием</mark> `DataInputStream`.
    - Полезен при работе с <mark class="hltr-red">бинарными форматами</mark> данных.
    - Он предоставляет методы, такие как `**writeInt()**`, `**writeDouble()**`, `**writeUTF()**`, которые позволяют записывать данные определенного типа
- **Пример**:
    
```java
  FileOutputStream fos = new FileOutputStream("data.bin");
DataOutputStream dos = new DataOutputStream(fos);
dos.writeInt(123);
dos.writeDouble(10.5);
dos.close();

```

```java
    import java.io.DataOutputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    
    public class DataOutputStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("data.bin");
                DataOutputStream dos = new DataOutputStream(fos);
    
                // Записываем данные в поток вывода
                dos.writeInt(42);
                dos.writeDouble(3.14);
                dos.writeUTF("Hello");
    
                // Закрываем потоки
                dos.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```


---

### 6. **`FilterOutputStream`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к потокам вывода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Не используется напрямую, но является основой для классов-декораторов, таких как `BufferedOutputStream` и `DataOutputStream`.
- **Пример**:
    - `BufferedOutputStream` и `DataOutputStream` являются примерами классов, которые наследуются от `FilterOutputStream`.
- Можно создать СВОЮ Реализацию 

```java
import java.io.FileOutputStream;
    import java.io.FilterOutputStream;
    import java.io.IOException;
    import java.io.OutputStream;
    
    public class FilterOutputStreamExample {
        public static void main(String[] args) {
            try {
                // Создаем файловый поток вывода
                FileOutputStream fos = new FileOutputStream("output.txt");
    
                // Создаем фильтрующий поток вывода с FilterOutputStream
                CustomFilterOutputStream filter = new CustomFilterOutputStream(fos);
    
                // Записываем данные в фильтрующий поток вывода
                filter.write("Hello, world!".getBytes());
    
                // Закрываем потоки
                filter.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    
    // Пример пользовательского фильтра, расширяющего FilterOutputStream
    class CustomFilterOutputStream extends FilterOutputStream {
        public CustomFilterOutputStream(OutputStream out) {
            super(out);
        }
    
        @Override
        public void write(byte[] b) throws IOException {
            // Добавляем дополнительную функциональность перед записью данных
            System.out.println("Before writing data");
    
            // Вызываем метод write() суперкласса для записи данных в поток вывода
            super.write(b);
    
            // Добавляем дополнительную функциональность после записи данных
            System.out.println("After writing data");
        }
    }
```


---

### 7. **`ObjectOutputStream`**

- **Описание**: Поток вывода <mark class="hltr-red">для сериализации объектов.</mark>
- **Особенности**:
    - Позволяет записывать объекты в поток для их последующего восстановления через `ObjectInputStream` (процесс называется сериализация).
    - Полезен для сохранения состояния объектов в файлы или передачи объектов через сеть.
- **Пример**:
    
```java
   FileOutputStream fos = new FileOutputStream("objectData.ser");
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(new MyObject());
oos.close();

```

---

### 8. **`PipedOutputStream`**

- **Описание**: Поток вывода для <mark class="hltr-purple">межпотоковой передачи данных</mark>. Записывает данные, которые могут быть прочитаны через `PipedInputStream`.
- **Особенности**:
    - Используется для организации связи между потоками <mark class="hltr-yellow">через каналы (pipe).</mark>
    - Полезен в многопоточных приложениях, где один поток записывает данные, а другой их читает.
- **Пример**:
    
```java
 PipedOutputStream pos = new PipedOutputStream();
PipedInputStream pis = new PipedInputStream(pos);

```


---

### 9. **`PrintStream`**

- **Описание**: <mark class="hltr-red">Поток вывода, который добавляет методы для удобной записи строк, а также автоматически обрабатывает ошибки</mark> (может подавлять `IOException`).
- **Особенности**:
    - Обладает методами `print()` и `println()`, что делает его <mark class="hltr-yellow">удобным для записи текстовых данных.</mark>
    - Может использоваться <mark class="hltr-green2">для работы с консолью, файлами или любыми другими потоками вывода.</mark>
- **Пример**:
    
```java

PrintStream ps = new PrintStream(new FileOutputStream("output.txt"));
ps.println("Hello, World!");
ps.close();

import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.PrintStream;
    
    public class PrintStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("output.txt");
                PrintStream ps = new PrintStream(fos);
    
                // Выводим текст в поток вывода
                ps.println("Hello, world!");
                ps.printf("The answer is %d", 42);
    
                // Закрываем потоки
                ps.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```


---

### 10. **`SequenceOutputStream`**

- **Описание**: <mark class="hltr-yellow">Объединяет несколько потоков вывода в один</mark>. Записывает данные последовательно в несколько потоков.
- **Особенности**:
    - Полезен, если нужно объединить <mark class="hltr-green2">несколько источников данных для записи в один поток вывода.</mark>
- **Пример**:
    
```java
FileOutputStream fos1 = new FileOutputStream("file1.txt");
FileOutputStream fos2 = new FileOutputStream("file2.txt");
SequenceOutputStream sos = new SequenceOutputStream(fos1, fos2);

```


---

### 11. **`PushbackOutputStream`**

- **Описание**: Этот класс не существует в стандартной библиотеке Java. Для ввода существует `PushbackInputStream`, но для вывода такого класса нет, так как логика «возврата байтов» применима только к вводу данных.