---
title: Reader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 16:26
modified: 2024-09-10T17:36:38+03:00
questions: 
notes: 
links: 
---
## Краткое описание
Для работы с символами Java предоставляет несколько классов ввода и вывода:

1. **`Reader`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков ввода символов.
2. **`FileReader`** — <mark class="hltr-purple">чтение символов из файла</mark>.
3. **`BufferedReader`** — <mark class="hltr-purple">буферизированное</mark> чтение для повышения производительности.
4. **`CharArrayReader`** — <mark class="hltr-blue">чтение символов из массива символов</mark>.
5. **`StringReader`** — <mark class="hltr-blue">чтение символов из строки.</mark>
6. **`PushbackReader`** — позволяет в<mark class="hltr-blue">озвращать символы обратно в поток.</mark>
7. **`FilterReader`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`LineNumberReader`** — <mark class="hltr-purple">отслеживание номеров строк при чтении.</mark>

## Полное описание
#### Классы для работы с символами:

---

### 1. **`Reader`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков ввода, работающих с символами (16-битные Unicode символы).
- **Методы**:
    - `int read()`: Читает один символ данных, возвращает -1, если достигнут конец потока.
    - `int read(char[] cbuf)`: Читает несколько символов в массив.
    - `long skip(long n)`: Пропускает указанное количество символов.
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Является базовым классом для всех потоков ввода символов.

---

### 2. **`FileReader`**

- **Описание**: Используется для <mark class="hltr-purple">чтения символов из файлов.</mark>
- **Особенности**:
    - <mark class="hltr-yellow">Читает текстовые файлы как последовательность символов.</mark>
    - Подходит для работы<mark class="hltr-yellow"> с текстовыми данными</mark>, сохраненными в файлах.
- **Пример**:
    
```java
FileReader fr = new FileReader("file.txt");
int data = fr.read();
while (data != -1) {
    System.out.print((char) data);
    data = fr.read();
}
fr.close();

```


---

### 3. **`BufferedReader`**

- **Описание**: Добавляет <mark class="hltr-purple">буферизацию</mark> к потоку ввода символов для повышения производительности.
- **Особенности**:
    - Оборачивает другой поток ввода (например, `FileReader`), добавляя буфер для уменьшения количества операций ввода.
    - <mark class="hltr-yellow">Поддерживает чтение строк и предоставляет метод</mark> `readLine()` для удобного чтения строк.
- **Пример**:
    
```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
String line;
while ((line = br.readLine()) != null) {
    System.out.println(line);
}
br.close();

```

---

### 4. **`CharArrayReader`**

- **Описание**: Читает<mark class="hltr-purple"> символы из массива символов как из потока.</mark>
- **Особенности**:
    - Полезен, когда нужно обрабатывать данные, находящиеся в массиве символов.
- **Пример**:
    
```java
char[] data = "Hello".toCharArray();
CharArrayReader car = new CharArrayReader(data);
int charData;
while ((charData = car.read()) != -1) {
    System.out.print((char) charData);
}
car.close();

import java.io.CharArrayReader;
import java.io.Reader;
import java.io.IOException;

public class CharArrayReaderExample {
    public static void main(String[] args) {
        char[] chars = { 'H', 'e', 'l', 'l', 'o', '!', ' ', 'w', 'o', 'r', 'l', 'd' };
        Reader reader = new CharArrayReader(chars);

        try {
            int charValue;
            while ((charValue = reader.read()) != -1) {
                // Обработка прочитанного символа
                System.out.println((char) charValue);
            }

            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```


---

### 5. **`StringReader`**

- **Описание**: <mark class="hltr-purple">Читает символы из строки.</mark>
- **Особенности**:
    - Полезен, когда нужно обрабатывать строку как поток символов.
- **Пример**:
    
```java
StringReader sr = new StringReader("Hello, World!");
int charData;
while ((charData = sr.read()) != -1) {
    System.out.print((char) charData);
}
sr.close();

```

---

### 6. **`PushbackReader`**

- **Описание**: Позволяет <mark class="hltr-red">возвращать прочитанные символы обратно в поток для повторного чтения.</mark>
- **Особенности**:
    - Полезен для парсинга текстов, где может потребоваться возвращение символов обратно в поток.
- **Пример**:
    
```java
PushbackReader pr = new PushbackReader(new FileReader("file.txt"));
int charData = pr.read();
pr.unread(charData);  // Возвращаем символ обратно в поток
pr.close();

```


---

### 7. **`FilterReader`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к другим потокам ввода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Сам по себе редко используется, но служит основой для других классов-декораторов, таких как `BufferedReader`.
- **Пример**:
    - Классы, такие как `BufferedReader`, наследуются от `FilterReader`.

---

### 8. **`LineNumberReader`**

- **Описание**: <mark class="hltr-red">Расширяет функциональность</mark> `BufferedReader`, <mark class="hltr-yellow">добавляя возможность отслеживания номеров строк.</mark>
- **Особенности**:
    - Полезен <mark class="hltr-yellow">при чтении текстов, где нужно знать номер строки.</mark>
- **Пример**:
    
```java
LineNumberReader lnr = new LineNumberReader(new FileReader("file.txt"));
String line;
while ((line = lnr.readLine()) != null) {
    System.out.println("Line " + lnr.getLineNumber() + ": " + line);
}
lnr.close();

```


### 9.  `InputStreamReader` 
преобразует байтовой поток в символьный поток путем чтения байтов и декодирования их в символы с использованием определенной кодировки.)

Пример использования класса `**InputStreamReader**` для чтения текстового файла с определенной кодировкой:

```java
		import java.io.FileInputStream;
        import java.io.InputStreamReader;
        import java.io.BufferedReader;
        import java.io.Reader;
        import java.nio.charset.StandardCharsets;
        import java.io.IOException;
        
        public class InputStreamReaderExample {
            public static void main(String[] args) {
                try {
                    FileInputStream fileInputStream = new FileInputStream("file.txt");
                    Reader reader = new InputStreamReader(fileInputStream, StandardCharsets.UTF_8);
                    BufferedReader bufferedReader = new BufferedReader(reader);
        
                    String line;
                    while ((line = bufferedReader.readLine()) != null) {
                        // Обработка прочитанной строки
                        System.out.println(line);
                    }
        
                    bufferedReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
```
