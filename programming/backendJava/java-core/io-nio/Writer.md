---
title: Writer
tags:
  - IO-NIO
related_topics:
  - "[[Когда использовать flush с потоками]]"
created: 2024-09-10 16:28
modified: 2024-09-10T17:28:24+03:00
questions: 
notes: 
links: 
---
## Краткое описание
Для записи символов в Java:

1. **`Writer`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков вывода символов.
2. **`FileWriter`** —<mark class="hltr-purple"> запись символов в файл.</mark>
3. **`BufferedWriter`** — <mark class="hltr-purple">буферизированная</mark> запись для повышения производительности.
4. **`CharArrayWriter`** — <mark class="hltr-blue">запись символов в массив символов в памяти</mark>.
5. **`StringWriter`** — <mark class="hltr-blue">запись символов в строку.</mark>
6. **`PrintWriter`** — <mark class="hltr-yellow">удобное написание строк и других данных.</mark>
7. **`FilterWriter`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`PipedWriter`** — <mark class="hltr-orange">межпотоковая</mark> запись данных через каналы.


## Полное описание
#### Классы для работы с символами:

---

### 1. **`Writer`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков вывода, работающих с символами.
- **Методы**:
    - `void write(int c)`: Записывает один символ.
    - `void write(char[] cbuf)`: Записывает массив символов.
    - `void write(String str)`: Записывает строку.
    - `void flush()`: Принудительно записывает данные из буфера (если используется буферизация).
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Основной класс, от которого наследуются все классы вывода символов.

---

### 2. **`FileWriter`**

- **Описание**: Поток вывода<mark class="hltr-purple"> для записи символов в файл.</mark>
- **Особенности**:
    - Записывает текстовые данные в файл на файловой системе.
    - Можно указать, дописывать ли данные в файл или перезаписывать его.
- **Пример**:
    
```java
FileWriter fw = new FileWriter("output.txt");
fw.write("Hello, World!");
fw.close();

import java.io.FileWriter;
import java.io.IOException;
import java.io.Writer;

public class WriterExample {
    public static void main(String[] args) {
        try {
            // Создаем объект FileWriter для файла "output.txt"
            Writer writer = new FileWriter("output.txt");

            // Записываем символы в поток вывода
            writer.write('H');
            writer.write("ello, ");
            writer.write("World!");

            // Сбрасываем буфер и записываем оставшиеся символы
            writer.flush();

            // Закрываем поток вывода
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```


---

### 3. **`BufferedWriter`**

- **Описание**: Поток вывода с <mark class="hltr-purple">буферизацией</mark> для повышения производительности.
- **Особенности**:
    - Оборачивает другой поток вывода (например, `FileWriter`), добавляя буфер для уменьшения количества операций записи.
    - Поддерживает запись строк и предоставляет метод `newLine()` для вставки символов новой строки.
- **Пример**:
    
```java
BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"));
bw.write("Buffered data");
bw.newLine();  // Добавляет новую строку
bw.close();

```


---

### 4. **`CharArrayWriter`**

- **Описание**: <mark class="hltr-purple">Записывает символы в массив символов, который находится в памяти.</mark>
- **Особенности**:
    - Полезен для работы с временными данными в памяти.
    - Позволяет получить массив символов через метод `toCharArray()`.
- **Пример**:
    
```java
CharArrayWriter caw = new CharArrayWriter();
caw.write("Hello".toCharArray());
char[] data = caw.toCharArray();
caw.close();

```


---

### 5. **`StringWriter`**

- **Описание**: <mark class="hltr-yellow">Записывает символы в строку.</mark>
- **Особенности**:
    - Полезен, когда нужно накопить текст в строку для последующей обработки.
    - Позволяет получить строку через метод `toString()`.
- **Пример**:
    
```java
StringWriter sw = new StringWriter();
sw.write("Hello, World!");
String result = sw.toString();
sw.close();

```

---

### 6. **`PrintWriter`**

- **Описание**: Поток вывода, <mark class="hltr-red">который добавляет методы для удобной записи строк и других данных </mark>(например, `print()`, `println()`).
- **Особенности**:
    - Обеспечивает удобный синтаксис для записи данных <mark class="hltr-pink">и автоматическое управление ошибками </mark>(подавляет `IOException`).
- **Пример**:
    
```java
PrintWriter pw = new PrintWriter(new FileWriter("output.txt"));
pw.println("Hello, World!");
pw.close();

```


---

### 7. **`FilterWriter`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к другим потокам вывода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Не используется напрямую, но служит основой для других классов-декораторов, таких как `BufferedWriter`.
- **Пример**:
    - Классы, такие как `BufferedWriter`, наследуются от `FilterWriter`.

---

### 8. **`PipedWriter`**

- **Описание**: Поток вывода для <mark class="hltr-yellow">межпотоковой</mark> передачи данных, записывающий данные, которые могут быть прочитаны через `PipedReader`.
- **Особенности**:
    - Используется для организации связи между потоками через каналы (pipe).
- **Пример**:
    
```java
PipedWriter pw = new PipedWriter();
PipedReader pr = new PipedReader(pw);

```
