---
title: InputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 16:05
modified: 2024-09-10T17:32:31+03:00
questions: 
notes: 
links: 
---
Каждый из этих классов предоставляет уникальные возможности для работы с байтовыми потоками:

1. **`InputStream`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков ввода, работающих с байтами.
2. **`FileInputStream`** — для <mark class="hltr-blue">чтения данных из файлов.</mark>
3. **`ByteArrayInputStream`** — для <mark class="hltr-blue">чтения данных из массива байтов</mark>.
4. **`BufferedInputStream`** — <mark class="hltr-blue">добавляет буферизацию</mark> для повышения производительности.
5. **`DataInputStream`** — позволяет<mark class="hltr-blue"> читать примитивные типы данных</mark>.
6. **`FilterInputStream`** — базовый <mark class="hltr-purple">класс для потоков-декораторов</mark>.
7. **`ObjectInputStream`** — для <mark class="hltr-blue">десериализации объектов</mark>.
8. **`PipedInputStream`** — для <mark class="hltr-blue">межпотоковой</mark> передачи данных.
9. **`SequenceInputStream`** — для п<mark class="hltr-purple">оследовательного чтения из нескольких потоков</mark>.
10. **`PushbackInputStream`** — позволяет <mark class="hltr-blue">возвращать байты в поток для повторного чтения</mark>.

Эти классы покрывают широкий спектр задач по работе с байтовыми потоками в Java.

### Примеры:
### 1. **`InputStream`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый <mark class="hltr-blue">класс для всех потоков ввода, которые работают с байтами.</mark> Все <mark class="hltr-yellow">остальные классы потоков ввода наследуются от этого класса.</mark>
- **Методы**:
    - `int read()`: Читает один байт данных, возвращает -1, если достигнут конец потока.
    - `int read(byte[] b)`: Читает несколько байт в массив.
    - `int available()`: Возвращает количество доступных для чтения байт.
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Является базой для специализированных классов потоков.

### 2. **`FileInputStream`**

- **Описание**: Используется<mark class="hltr-purple"> для чтения байтов из файлов</mark>.
- **Особенности**:
    - <mark class="hltr-yellow">Может читать файл как последовательность байтов.</mark>
    - Поддерживает чтение из существующих файлов на файловой системе.
- **Пример**:
```java
FileInputStream fis = new FileInputStream("file.txt");
int data = fis.read();
while (data != -1) {
    System.out.print((char) data);
    data = fis.read();
}
fis.close();

    public static void main(String[] args) {
        try {
            InputStream inputStream = new FileInputStream("file.txt");
            // читаем данные в буффер
            byte[] buffer = new byte[1024];
            int bytesRead;

            while ((bytesRead = inputStream.read(buffer)) != -1) {
                // Обработка прочитанных байтов
                for (int i = 0; i < bytesRead; i++) {
                    System.out.println(buffer[i]);
                }
            }

            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```


### 3. **`ByteArrayInputStream`**

- **Описание**: <mark class="hltr-purple">Читает байты из массива байтов</mark> как из потока.
- **Особенности**:
    - Поток <mark class="hltr-yellow">работает с массивом байтов, который содержится в памяти.</mark>
    - <mark class="hltr-yellow">Полезен</mark>, когда нужно о<mark class="hltr-yellow">брабатывать данные, находящиеся в памяти, как поток</mark>.
- **Пример**:

```java
byte[] data = "Hello".getBytes();
ByteArrayInputStream bais = new ByteArrayInputStream(data);
int byteData;
while ((byteData = bais.read()) != -1) {
    System.out.print((char) byteData);
}
bais.close();

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.IOException;

public class InputStreamExample {
    public static void main(String[] args) {
        byte[] bytes = { 65, 66, 67, 68, 69 };
        InputStream inputStream = new ByteArrayInputStream(bytes);

        try {
            int byteValue;
            while ((byteValue = inputStream.read()) != -1) {
                // Обработка прочитанного байта
                System.out.println(byteValue);
            }

            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```



### 4. **`BufferedInputStream`**

- **Описание**: Предоставляет <mark class="hltr-purple">возможность буферизованного ввода для повышения производительности</mark> при чтении байтов из потока.
- **Особенности**:
    - <mark class="hltr-yellow">Оборачивает другой поток ввода</mark> (например, `FileInputStream`), добавляя буфер для повышения эффективности.
    - <mark class="hltr-yellow">Уменьшает количество операций чтения с диска или сети</mark>.
- **Пример**:
    
```java
FileInputStream fis = new FileInputStream("file.txt");
BufferedInputStream bis = new BufferedInputStream(fis);
int data = bis.read();
while (data != -1) {
    System.out.print((char) data);
    data = bis.read();
}
bis.close();

```

    

### 5. **`DataInputStream`**

- **Описание**: Используется <mark class="hltr-purple">для чтения примитивных типов данных</mark> (int, long, float и т.д.) из байтового потока.
- **Особенности**:
    - Читает <mark class="hltr-yellow">данные в формате, подходящем для использования примитивных типов </mark>(например, `int`, `double`).
    - <mark class="hltr-green2">Полезен для работы с потоками, которые содержат бинарные данные.</mark>
- **Пример**:
    
```java
FileInputStream fis = new FileInputStream("data.bin");
DataInputStream dis = new DataInputStream(fis);
int number = dis.readInt();
System.out.println(number);
dis.close();

```

    

### 6. **`FilterInputStream`**

- **Описание**: Базовый класс для потоков, которые <mark class="hltr-red">декорируют другие потоки для добавления дополнительных функциональностей</mark> (например, буферизация, сжатие).
- **Особенности**:
    - <mark class="hltr-yellow">Все классы, которые оборачивают другие потоки</mark> (например, `BufferedInputStream`, `DataInputStream`), наследуются от `FilterInputStream`.
    - Этот класс сам по себе редко используется напрямую, но предоставляет основу для потоков-декораторов. [[Декоратор]]
    - <mark class="hltr-yellow">Цель</mark> этого класса — <mark class="hltr-yellow">расширить возможности стандартных потоков ввода</mark> (например, добавить буферизацию, обработку данных и т.п.) без изменения их внутренней логики.

##### Пример использования `FilterInputStream`

Допустим, мы хотим прочитать файл и при этом добавить буферизацию. В этом случае мы можем обернуть `FileInputStream` в `BufferedInputStream`, что улучшит производительность при чтении больших файлов.

##### Пример без декоратора:
```java
import java.io.FileInputStream;
import java.io.IOException;

public class SimpleReadExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("file.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
Этот код читает файл байт за байтом, но каждый вызов метода `read()` обращается к диску, что делает его неэффективным при работе с большими объемами данных.

###### Пример с использованием декоратора (`BufferedInputStream`):

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class BufferedReadExample {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.txt"))) {
            int data;
            while ((data = bis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

В этом примере:

- `BufferedInputStream` оборачивает `FileInputStream`.
- Теперь данные читаются буферами, что уменьшает количество обращений к диску, повышая производительность.

##### Как работает `FilterInputStream`?

`FilterInputStream` сам по себе не изменяет поведение потоков ввода. Он делегирует вызовы методов, таких как `read()`, внутреннему потоку (`in`), который был передан в его конструктор.

##### Пример создания собственного декоратора:

Допустим, мы хотим создать поток, который подсчитывает количество прочитанных байтов.
```java
import java.io.FilterInputStream;
import java.io.IOException;
import java.io.InputStream;

class CountingInputStream extends FilterInputStream {
    private int bytesRead = 0;

    protected CountingInputStream(InputStream in) {
        super(in);
    }

    @Override
    public int read() throws IOException {
        int data = super.read();
        if (data != -1) {
            bytesRead++;
        }
        return data;
    }

    @Override
    public int read(byte[] b, int off, int len) throws IOException {
        int result = super.read(b, off, len);
        if (result != -1) {
            bytesRead += result;
        }
        return result;
    }

    public int getBytesRead() {
        return bytesRead;
    }
}

public class Main {
    public static void main(String[] args) {
        try (CountingInputStream cis = new CountingInputStream(new FileInputStream("file.txt"))) {
            while (cis.read() != -1) {
                // Читаем данные
            }
            System.out.println("Прочитано байтов: " + cis.getBytesRead());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

### 7. **`ObjectInputStream`**

- **Описание**: Используется <mark class="hltr-purple">для десериализации объектов из байтового потока, то есть для преобразования байтов обратно в объекты Java.</mark>
- **Особенности**:
    -<mark class="hltr-yellow"> Позволяет читать объекты, которые были сериализованы через</mark> `ObjectOutputStream`.
    - Полезен для работы с сериализацией объектов.
- **Пример**:
```java
FileInputStream fis = new FileInputStream("objectData.ser");
ObjectInputStream ois = new ObjectInputStream(fis);
MyObject obj = (MyObject) ois.readObject();
ois.close();

```

    

### 8. **`PipedInputStream`**

- **Описание**: Используется<mark class="hltr-red"> для связи между потоками. Читает данные из канала (pipe), которые были записаны в другой поток с помощью</mark> `PipedOutputStream`.
- **Особенности**:
    - Позволяет<mark class="hltr-purple"> организовать межпотоковое взаимодействие через каналы.</mark>
    - Используется в многопоточных приложениях для передачи данных между потоками.
- **Пример**:
    
```java
PipedInputStream pis = new PipedInputStream();
PipedOutputStream pos = new PipedOutputStream(pis);

```

    

### 9. **`SequenceInputStream`**

- **Описание**: <mark class="hltr-yellow">Объединяет несколько потоков ввода в один</mark>. <mark class="hltr-green2">Читает данные из одного потока за другим последовательно.</mark>
- **Особенности**:
    - Полезен для последовательного <mark class="hltr-purple">чтения данных из нескольких потоков</mark> (например, из нескольких файлов).
- **Пример**:
    
```java
FileInputStream fis1 = new FileInputStream("file1.txt");
FileInputStream fis2 = new FileInputStream("file2.txt");
SequenceInputStream sis = new SequenceInputStream(fis1, fis2);

```

    

### 10. **`PushbackInputStream`**

- **Описание**: Предоставляет <mark class="hltr-yellow">возможность</mark> "<mark class="hltr-red">откатывания</mark>" пр<mark class="hltr-yellow">очитанных байтов обратно в поток, чтобы они могли быть прочитаны снова.</mark>
- **Особенности**:
    - Полезен в случаях, когда нужно «вернуть» байты обратно в поток для повторного чтения.
- **Пример**:
    
```java
PushbackInputStream pis = new PushbackInputStream(new FileInputStream("file.txt"));
int data = pis.read();
pis.unread(data);  // Возвращаем байт обратно в поток

```

