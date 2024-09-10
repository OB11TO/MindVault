---
modified: 2024-09-10T18:10:32+03:00
---
## Blocking IO vs New (non-blocking) IO

![[images/Untitled 12.png|Untitled 12.png]]

### Input-Output

Пакет `java.io` обеспечивает ввод и вывод через потоки (stream) данных.

![[images/Untitled 1 4.png|Untitled 1 4.png]]

Он реализует две группы абстрактных классов:

- `InputStream` и `OutputStream` - потоки побайтового ввода-вывода;

![[images/Untitled 2 3.png|Untitled 2 3.png]]

- `Reader` и `Writer` - потоки посимвольного ввода-вывода.

![[images/Untitled 3 3.png|Untitled 3 3.png]]

  

![[images/Untitled 4 3.png|Untitled 4 3.png]]

### New Input-Output

`Java NIO` - альтернативная библиотека для работы с вводом-выводом.  
Основные принципы NIO:  
1. Не блокирует операции ввода-вывода: thread получает те данные из буфера, что в нем есть и переходит к исполнению дальнейших инструкций. Таким образом, например, thread может одновременно получать данные из нескольких каналов (а не по очереди).  
2. Буфер-ориентированность: работа с данными идет не побайтово/посимвольно, а с буфером, который является контейнером для массива примитивного типа данных.  

![[images/Untitled 5 3.png|Untitled 5 3.png]]

`Java NIO` базируется на следующих компонентах:

- Абстрактный класс `Buffer` имеет наследников для работы с разными типами данных, например, `ByteBuffer`. В нем содержатся данные, которые можно будет обработать, используя каналы.

![[images/Untitled 6 2.png|Untitled 6 2.png]]

- Интерфейс `Channel` - новый тип абстракции, похожий на Stream. В отличие от потока канал является двусторонним, и может одновременно считывать и записывать данные в буфер. Также поддерживает режим блокировки

![[images/Untitled 7 2.png|Untitled 7 2.png]]

- Класс `Selector` - объект, отслеживающий события в нескольких каналах. При неблокирующих IO-операциях, селектор используется для выбора каналов, готовых к вводу/выводу.

![[images/Untitled 8 2.png|Untitled 8 2.png]]

### Отличия IO от NIO

- Поток I/O блокирует файл(и thread, который вызвал операцию чтения или записи) до конца работы с ним. Нельзя одновременно считывать данные из файла и записывать что-то в него, в то же время thread ждет конца операции, и не идет дальше;
- В пакете IO не реализована поддержка файловых систем (на Windows путь выглядит примерно так: "C:\Users\Vladimir\java\file.txt”, на UNIX-like так: “/users/Vladimir/java/file.txt”) и поддержка ссылок;
- В IO большое количество проверяемых исключений
- NIO может напрямую получать атрибуты файла, например, его размер;
- В NIO реализованы методы для управления файлами (например, копирования);
- NIO поддерживает асинхронный поток данных.

---

## Java.io

### OutputStream - байтовый вывод

Класс `OutputStream` является абстрактным классом-родителем для всех классов, которые поддерживают байтовый вывод.

Методы класса `OutputStream`, которые обязаны реализовывать все его наследники:

|Методы|Описание|
|---|---|
|void write(int b)|Записывает один байт (не int) в поток.|
|void write(byte[] buffer)|Записывает массив байт в поток|
|void write(byte[] buffer, int off, int len)|Записывает часть массива байт в поток, начиная с индекса off, длиной len|
|void flush()|Записывает в поток все данные, которые хранятся в буфере|
|void close()|Закрывает поток|

При создании объекта класса-наследника `OutputStream` обычно указывается целевой объект или целевой поток, в который будут записываться данные.

Пример использования класса `**FileOutputStream**`:

```Java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamExample {
    public static void main(String[] args) {
        try {
            // Создаем объект FileOutputStream для файла "output.txt"
            FileOutputStream fos = new FileOutputStream("output.txt");

            // Записываем байты в поток вывода
            fos.write(65); // Записываем ASCII-код символа 'A'
            fos.write(new byte[]{66, 67, 68}); // Записываем массив байтов 
//('B', 'C', 'D')

            // Закрываем поток вывода
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### Классы-наследникиFileOutputStream

1. **`BufferedOutputStream`** (Класс `**BufferedOutputStream**` расширяет `**FileOutputStream**` и предоставляет буферизацию при записи байтовых данных в файл. Он улучшает производительность путем минимизации фактических обращений к файловой системе.)
2. `**DataOutputStream**` (Класс `**DataOutputStream**` также расширяет `**FileOutputStream**` и позволяет записывать примитивные типы данных и строки в байтовый поток вывода. Он предоставляет методы, такие как `**writeInt()**`, `**writeDouble()**`, `**writeUTF()**`, которые позволяют записывать данные определенного типа.)
3. `**ObjectOutputStream**` (Класс `**ObjectOutputStream**` расширяет `**FileOutputStream**` и предоставляет возможность записи объектов Java в байтовый поток вывода. Он использует механизм сериализации Java для преобразования объектов в последовательность байтов, которые можно записать в файл.)
4. `**PrintStream**` (Класс `**PrintStream**` также расширяет `**FileOutputStream**` и предоставляет удобные методы для вывода форматированного текста в байтовый поток вывода. Он может быть использован, например, для записи на консоль или в файл текстовых сообщений и данных.)

- Примеры использования:
    
    1. Пример использования `**BufferedOutputStream**`:
    
    ```Java
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
    
    2. Пример использования `**DataOutputStream**`:
    
    ```Java
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
    
    3. Пример использования `**ObjectOutputStream**`:
    
    ```Java
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectOutputStream;
    import java.util.ArrayList;
    import java.util.List;
    
    public class ObjectOutputStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("objects.bin");
                ObjectOutputStream oos = new ObjectOutputStream(fos);
    
                // Создаем список объектов
                List<String> list = new ArrayList<>();
                list.add("apple");
                list.add("banana");
                list.add("orange");
    
                // Записываем список в поток вывода
                oos.writeObject(list);
    
                // Закрываем потоки
                oos.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
    4. Пример использования `**PrintStream**`:
    
    ```Java
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
    

### Другие классы, реализующие абстрактный класс OutputStream

1. `**ByteArrayOutputStream**` (Класс `**ByteArrayOutputStream**` представляет поток вывода в виде массива байтов. Он записывает данные во внутренний буфер массива и предоставляет методы для получения содержимого буфера.)
2. `**FilterOutputStream**` (Класс `**FilterOutputStream**` является базовым классом для всех фильтрующих потоков вывода. Он добавляет функциональность к существующему потоку вывода, например, возможность сжатия данных или добавления шифрования.)

- Пример фильтрации:
    
    ```Java
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
    

1. `**PipedOutputStream**` (Класс `**PipedOutputStream**` представляет поток вывода, который связан с соответствующим потоком ввода `**PipedInputStream**`. Он используется для обмена данными между различными потоками внутри одной JVM.)

### Writer - символьный вывод

Класс `**Writer**` в пакете `**java.io**` является абстрактным классом, предоставляющим общий функционал для записи символьных данных в поток. Он служит базовым классом для различных конкретных классов писателей, таких как `**FileWriter**` и `**BufferedWriter**`, и предоставляет методы для записи символов в поток.

Методы класса `Writer` (и всех его классов-наследников):

|   |   |
|---|---|
|**Методы**|**Описание**|
|void write(int b)|Записывает один символ (не `int`) в поток.|
|void write(char[] buffer)|Записывает массив символов в поток|
|void write(char[] buffer, int off, int len)|Записывает часть массива символов в поток|
|void write(String str)|Записывает строку в поток|
|void write(String str, int off, int len)|Записывает часть строки в поток|
|void flush()|Записывает в поток все данные, которые хранятся в буфере|
|void close()|Закрывает поток|

Пример использования класса `FileWriter`:

```Java
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

После записи мы сбрасываем буфер с помощью метода `**flush()**`, чтобы убедиться, что все символы записаны, и затем закрываем поток вывода с помощью метода `**close()**`.

- Классы, реализующие `Writer`
    1. `**BufferedWriter**` (Класс `**BufferedWriter**` обеспечивает буферизованную запись символов в поток вывода. Он улучшает производительность, минимизируя количество операций записи в фактический поток вывода.)
    2. `**OutputStreamWriter**` (Класс `**OutputStreamWriter**` преобразует символы в потоке вывода байтов. Он используется для записи символов в поток вывода, например, в файл или сетевой сокет). Класс `FileWriter` - наследник этого класса.
    3. `**PrintWriter**` (Класс `**PrintWriter**` предоставляет удобные методы для форматированной записи текста в поток вывода. Он также предоставляет возможность автоматического сброса буфера и обработки исключений.)
    4. `**StringWriter**` (Класс `**StringWriter**` представляет поток вывода, который записывает символы в строку. Он полезен, когда требуется записать символы в память в виде строки.)

### InputStream - байтовый ввод

Класс `**InputStream**` в пакете `**java.io**` является абстрактным базовым классом для всех классов, представляющих входной поток байтов. Он обеспечивает методы для чтения байтов из потока. Вот описание класса `**InputStream**` и примеры его использования:

`**InputStream**` представляет абстрактный входной поток, который определяет следующие методы:

|   |   |
|---|---|
|**Методы**|**Описание**|
|int read()|Читает следующий байт из потока и возвращает его значение в виде целого числа от 0 до 255. Если достигнут конец потока, возвращает значение -1.|
|int read(byte[] buffer)|• Читает байты из потока и сохраняет их в буфер. Возвращает количество фактически прочитанных байтов или -1, если достигнут конец потока.|
|int read(byte[] buffer, int offset, int lenght)|Читает до `**length**` байтов из потока и сохраняет их в указанном буфере, начиная с смещения `**offset**`. Возвращает количество фактически прочитанных байтов или -1, если достигнут конец потока.|
|byte[] readAllBytes()|Читает все байты из потока|
|long skip(long n)|Пропускает `n` байт в потоке (читает и выкидывает)|
|int available()|Проверяет, сколько байт еще осталось в потоке|
|void close()|Закрывает поток|

Пример использования `ByteArrayInputStream` для чтения байтов из массива:

```Java
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

Чтение байтов в буфер:

```Java
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.IOException;

public class InputStreamExample {
    public static void main(String[] args) {
        try {
            InputStream inputStream = new FileInputStream("file.txt");
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

### Классы, наследующие InputStream

1. `**FileInputStream**` (Класс `**FileInputStream**` представляет поток ввода для чтения данных из файла.)
2. `**ByteArrayInputStream**` (Класс `**ByteArrayInputStream**` представляет поток ввода для чтения данных из массива байтов.)
3. `**FilterInputStream**` (Класс `**FilterInputStream**` является абстрактным базовым классом для всех фильтрующих потоков ввода. Он добавляет дополнительную функциональность к существующему потоку ввода.)
4. `**ObjectInputStream**` (Класс `**ObjectInputStream**` представляет поток ввода, который может десериализовать объекты Java из последовательности байтов.)

- Пример `**ObjectInputStream**`
    
    ```Java
    import java.io.FileInputStream;
    import java.io.ObjectInputStream;
    import java.io.IOException;
    
    public class ObjectInputStreamExample {
        public static void main(String[] args) {
            try {
                FileInputStream fileInputStream = new FileInputStream("object.dat");
                ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
    
                // Чтение объекта из потока
                Object object = objectInputStream.readObject();
    
                // Преобразование объекта к нужному типу и обработка
                if (object instanceof MyClass) {
                    MyClass myObject = (MyClass) object;
                    // Обработка объекта
                    System.out.println(myObject.toString());
                }
    
                objectInputStream.close();
            } catch (IOException | ClassNotFoundException e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
    В этом случае нужно обрабатывать `**ClassNotFoundException**`
    

1. `**PipedInputStream**` (Класс `**PipedInputStream**` представляет поток ввода, который может быть связан с соответствующим `**PipedOutputStream**` для создания канала между потоками ввода и вывода.)

### Reader - символьный ввод

Класс `**Reader**` в пакете `**java.io**` является абстрактным базовым классом для всех классов, представляющих входной поток символов.

`**Reader**` представляет абстрактный входной поток символов, который определяет следующие методы:

|   |   |
|---|---|
|**Методы**|**Описание**|
|int read()|Читает один `char` из потока. Если достигнут конец потока, возвращает значение -1|
|int read(char[] buffer)|Читает массив `char`’ов из потока. Если достигнут конец потока, возвращает значение -1|
|int read(char[] buffer, int offset, int length)|Читает до `**length**` символов из потока и сохраняет их в указанном буфере, начиная с смещения `**offset**`. Возвращает количество фактически прочитанных символов или -1, если достигнут конец потока.|
|long skip(long n)|Пропускает `n` `char`’ов в потоке (читает и выбрасывает)|
|boolean ready()|Проверяет, что в потоке еще что-то осталось|
|void close()|Закрывает поток|

Вот несколько примеров использования `**Reader**`:

1. Чтение символов из файла:

```Java
import java.io.FileReader;
import java.io.Reader;
import java.io.IOException;

public class FileReaderExample {
    public static void main(String[] args) {
        try {
            Reader reader = new FileReader("file.txt");

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

2. Чтение символов из массива:

```Java
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

- Классы, наследующие `Reader`
    
    1. `**FileReader**` (Класс `**FileReader**` представляет символьный входной поток для чтения данных из файла.)
    2. `**CharArrayReader**` (Класс `**CharArrayReader**` представляет символьный входной поток для чтения данных из массива символов.)
    3. `**BufferedReader**` (Класс `**BufferedReader**` представляет символьный входной поток, который буферизирует чтение данных для повышения производительности.)
    
    - Пример использования `**BufferedReader**`:
        
        ```Java
        import java.io.BufferedReader;
        import java.io.FileReader;
        import java.io.Reader;
        import java.io.IOException;
        
        public class BufferedReaderExample {
            public static void main(String[] args) {
                try {
                    Reader fileReader = new FileReader("file.txt");
                    BufferedReader reader = new BufferedReader(fileReader);
        
                    String line;
                    while ((line = reader.readLine()) != null) {
                        // Обработка прочитанной строки
                        System.out.println(line);
                    }
        
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        ```
        
    
    1. `**InputStreamReader**` (Класс `**InputStreamReader**` преобразует байтовой поток в символьный поток путем чтения байтов и декодирования их в символы с использованием определенной кодировки.)
    
    - Пример использования класса `**InputStreamReader**` для чтения текстового файла с определенной кодировкой:
        
        ```Java
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
        
    
    1. `**StringReader**` (Класс `**StringReader**` представляет символьный входной поток для чтения данных из строки.)
    2. `**LineNumberReader**` (Класс `**LineNumberReader**` представляет символьный входной поток, который поддерживает счетчик строк.)
    3. `**FilterReader**` (Класс `**FilterReader**` является абстрактным базовым классом для всех фильтрующих символьных потоков ввода. Он добавляет дополнительную функциональность к существующему потоку чтения.)
    4. `**PushbackReader**` (Класс `**PushbackReader**` представляет символьный входной поток, который поддерживает возврат символов обратно в поток чтения.)
    5. `**PipedReader**` (Класс `**PipedReader**` представляет символьный входной поток, который может быть связан с соответствующим `**PipedWriter**` для создания канала между потоками ввода и вывода.)

### Interfaces

- `**Closeable**`: Интерфейс `**Closeable**` определяет метод `**close()**`, который должен быть реализован классами, чтобы освободить ресурсы, связанные с объектом.
- `**Flushable**`: Интерфейс `**Flushable**` определяет метод `**flush()**`, который должен быть реализован классами, чтобы сбросить буферизованные данные в выходной поток.
- `**Serializable**`: Интерфейс `**Serializable**` используется для указания, что класс может быть сериализован и десериализован.
- `**AutoCloseable**`: Интерфейс `**AutoCloseable**` расширяет `**Closeable**` и определяет метод `**close()**`, который может генерировать исключения.
- `**DataInput**`: Интерфейс `**DataInput**` определяет методы для чтения примитивных данных из входного потока.
- `**DataOutput**`: Интерфейс `**DataOutput**` определяет методы для записи примитивных данных в выходной поток.
- `**ObjectInput**`: Интерфейс `**ObjectInput**` предоставляет методы для чтения объектов из входного потока.
- `**ObjectOutput**`: Интерфейс `**ObjectOutput**` предоставляет методы для записи объектов в выходной поток.

### try-with-resources

Try-with-resources - это специальная конструкция в Java, которая позволяет автоматически закрывать ресурсы (например, файлы, потоки ввода-вывода и т.д.), после того как они были использованы.

```Java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    System.err.println("Ошибка чтения файла: " + e.getMessage());
}
```

Конструкция try-with-resources гарантирует, что файл будет закрыт после того, как мы закончим чтение данных. Для этого мы объявляем `BufferedReader` в скобках после ключевого слова try.

Таким образом, даже если произойдет исключение в блоке try, файл будет автоматически закрыт, и мы не будем его удерживать открытым.

Обратите внимание, что класс `BufferedReader` реализует интерфейс `AutoCloseable`, что позволяет автоматически закрывать ресурсы при использовании try-with-resources.

Такой подход упрощает работу с ресурсами и позволяет избежать утечки ресурсов, что может привести к проблемам с производительностью или даже к аварийному завершению программы.

### File

Класс `**File**` в пакете `**java.io**` представляет файл или директорию в файловой системе. Он используется для работы с файлами и папками, включая создание, удаление, перемещение и проверку свойств файлов.

==ЭТОТ КЛАСС СОВМЕСТИМ С NIO==

Некоторые из основных методов класса `**File**` включают:

- `**toPath()**`: Возвращает объект типа `**Path**`, представляющий путь к файлу или директории.
- `**exists()**`: Проверяет, существует ли файл или директория.
- `**isDirectory()**`: Проверяет, является ли объект `**File**` директорией.
- `**isFile()**`: Проверяет, является ли объект `**File**` обычным файлом.
- `**createNewFile()**`: Создает новый файл.
- `**mkdir()**`: Создает директорию.
- `**mkdirs()**`: Создает директорию, включая все промежуточные директории.
- `**delete()**`: Удаляет файл или директорию.
- `**renameTo(File dest)**`: Переименовывает файл или директорию на указанный путь.
- `**getParent()**`: Возвращает родительскую директорию файла или директории.
- `**lastModified()**`: Возвращает время последней модификации файла или директории.

Примеры использования класса `**File**`:

1. Создание нового файла и запись в него:

```Java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("path/to/file.txt");
        
        try {
            if (file.createNewFile()) {
                System.out.println("Файл создан: " + file.getAbsolutePath());
                
                // Запись в файл
                FileWriter writer = new FileWriter(file);
                writer.write("Привет, мир!");
                writer.close();
            } else {
                System.out.println("Файл уже существует.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

2. Проверка существования файла и удаление его, если существует:

```Java
import java.io.File;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("path/to/file.txt");
        
        if (file.exists()) {
            if (file.delete()) {
                System.out.println("Файл удален.");
            } else {
                System.out.println("Не удалось удалить файл.");
            }
        } else {
            System.out.println("Файл не существует.");
        }
    }
}
```

---

## Java.nio

### Buffer

Класс `**Buffer**` в пакете `**java.nio**` представляет буфер для чтения и записи данных. Буфер является контейнером для элементов определенного типа данных, таких как байты, символы и примитивные значения. Он предоставляет методы для удобного доступа и манипулирования данными.

Вот основные методы и свойства класса `**Buffer**`:

- `**capacity()**`: Возвращает емкость буфера, т.е. максимальное количество элементов, которое буфер может содержать.
- `**position()**`: Возвращает текущую позицию в буфере, т.е. индекс следующего элемента, который будет считываться или записываться.
- `**limit()**`: Возвращает предел буфера, т.е. индекс первого непрочитанного или незаписанного элемента.
- `**mark()**`: Устанавливает метку в текущую позицию.
- `**reset()**`: Восстанавливает позицию на ранее установленную метку.
- `**flip()**`: Меняет предел с текущей позиции на текущую позицию, а текущую позицию на 0. Это готовит буфер к чтению после записи.
- `**rewind()**`: Устанавливает текущую позицию в 0, сохраняя предел и емкость неизменными. Это готовит буфер к повторному чтению или записи.
- `**clear()**`: Очищает буфер путем установки текущей позиции в 0, предела в емкость и сброса метки. Это готовит буфер к записи после чтения.
- `**hasRemaining()**`: Проверяет, есть ли еще непрочитанные или незаписанные элементы в буфере.
- `**remaining()**`: Возвращает количество оставшихся элементов, которые можно прочитать или записать.

Пример использования класса `**Buffer**` для чтения и записи данных:

```Java
import java.nio.Buffer;
import java.nio.ByteBuffer;

public class BufferExample {
    public static void main(String[] args) {
        // Создание буфера
        ByteBuffer buffer = ByteBuffer.allocate(10);

        // Запись данных в буфер
        buffer.put((byte) 1);
        buffer.put((byte) 2);
        buffer.put((byte) 3);

        // Подготовка буфера для чтения
        buffer.flip();

        // Чтение данных из буфера
        while (buffer.hasRemaining()) {
            byte data = buffer.get();
            System.out.println(data);
        }

        // Очистка буфера
        buffer.clear();
    }
}
```

- Подклассы `Buffer` для разных типов данных:
    
    1. `**ByteBuffer**`: Подкласс `**ByteBuffer**` предназначен для работы с байтовыми данными. Он предоставляет методы для чтения и записи байтов в буфер.
    2. `**CharBuffer**`: Подкласс `**CharBuffer**` предназначен для работы с символьными данными. Он предоставляет методы для чтения и записи символов в буфер.
    3. `**ShortBuffer**`: Подкласс `**ShortBuffer**` предназначен для работы с короткими целыми числами (short). Он предоставляет методы для чтения и записи short-значений в буфер.
    4. `**IntBuffer**`: Подкласс `**IntBuffer**` предназначен для работы с целыми числами (int). Он предоставляет методы для чтения и записи int-значений в буфер.
    5. `**LongBuffer**`: Подкласс `**LongBuffer**` предназначен для работы с длинными целыми числами (long). Он предоставляет методы для чтения и записи long-значений в буфер.
    6. `**FloatBuffer**`: Подкласс `**FloatBuffer**` предназначен для работы с числами с плавающей запятой одинарной точности (float). Он предоставляет методы для чтения и записи float-значений в буфер.
    7. `**DoubleBuffer**`: Подкласс `**DoubleBuffer**` предназначен для работы с числами с плавающей запятой двойной точности (double). Он предоставляет методы для чтения и записи double-значений в буфер.
    
    Каждый из этих подклассов имеет свои уникальные методы и свойства, специфичные для работы с соответствующим типом данных. Все они общими являются методы и свойства, унаследованные от базового класса `**Buffer**`, такие как `**capacity()**`, `**position()**`, `**limit()**` и другие, которые позволяют управлять данными в буфере.
    

### Channel

Класс `**Channel**` в пакете `**java.nio.channels**` представляет канал, который обеспечивает двустороннюю связь между источником данных и приемником данных. Каналы используются для чтения и записи данных из и в буферы.

Ниже приведены основные методы и свойства класса `**Channel**`:

- `**isOpen()**`: Возвращает `**true**`, если канал открыт, и `**false**`, если закрыт.
- `**close()**`: Закрывает канал.
- `**read(ByteBuffer buffer)**`: Читает данные из канала в указанный буфер.
- `**write(ByteBuffer buffer)**`: Записывает данные из указанного буфера в канал.
- `**transferTo(long position, long count, WritableByteChannel target)**`: Передает данные из текущего канала в указанный канал, начиная с указанной позиции и с заданным количеством байтов.
- `**transferFrom(ReadableByteChannel src, long position, long count)**`: Передает данные из указанного канала в текущий канал, начиная с указанной позиции и с заданным количеством байтов.

Пример использования класса `FileChannel` для чтения и записи данных:

```Java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class ChannelExample {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");
        String data = "Hello, Channel!";

        // Запись данных в файл с использованием канала
        try (FileChannel channel = FileChannel.open(path, StandardOpenOption.WRITE, StandardOpenOption.CREATE)) {
            ByteBuffer buffer = ByteBuffer.allocate(data.length());
            buffer.put(data.getBytes());
            buffer.flip();
            channel.write(buffer);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Чтение данных из файла с использованием канала
        try (FileChannel channel = FileChannel.open(path, StandardOpenOption.READ)) {
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int bytesRead = channel.read(buffer);
            buffer.flip();
            byte[] readData = new byte[bytesRead];
            buffer.get(readData);
            System.out.println(new String(readData));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- Подклассы `Channel`
    
    1. `**FileChannel**`: Реализует канал для чтения и записи данных в файлы.
    
    - Подробнее о **`FileChannel`**
        
        Класс `**FileChannel**` в пакете `**java.nio.channels**` представляет канал для чтения и записи данных в файлы. Он предоставляет более низкоуровневый доступ к файловой системе и позволяет выполнять операции чтения и записи с использованием буферов.
        
        Вот некоторые важные аспекты и методы класса `**FileChannel**`:
        
        - `**read(ByteBuffer dst)**`: Читает данные из канала в указанный буфер `**ByteBuffer**`. Метод возвращает количество прочитанных байтов или `**1**`, если достигнут конец файла.
        - `open(Path path, OpenOption... options)`
        - `open(Path path, Set<? extends OpenOption> options, FileAttribute<?>... attrs)` : Открывает или создает файл, возвращая `FileChannel` для доступа к нему.
        - `**write(ByteBuffer src)**`: Записывает данные из указанного буфера `**ByteBuffer**` в канал. Метод возвращает количество записанных байтов.
        - `**position()**`: Возвращает текущую позицию в файле, связанном с каналом.
        - `**position(long newPosition)**`: Устанавливает новую позицию в файле, связанном с каналом.
        - `**size()**`: Возвращает текущий размер файла, связанного с каналом.
        - `**truncate(long size)**`: Усекает размер файла до указанного размера. Если файл был больше указанного размера, лишние байты будут отброшены.
        - `**force(boolean metaData)**`: Заставляет записать все данные из канала на диск. Если параметр `**metaData**` равен `**true**`, то также будут сохранены метаданные файла.
        - `**lock()**`: Захватывает эксклюзивную блокировку файла. Другие каналы не смогут получить доступ к файлу, пока блокировка не будет освобождена.
        - `**tryLock()**`: Пытается захватить эксклюзивную блокировку файла, если она доступна. Возвращает объект `**FileLock**`, представляющий блокировку, или `**null**`, если блокировка не удалось получить.
        - `**map(MapMode mode, long position, long size)**`: Создает отображение файла в памяти в виде `**MappedByteBuffer**`, позволяя прямой доступ к содержимому файла без явной операции чтения/записи.
        
        ---
        
    
    1. `**SocketChannel**`: Предоставляет канал для клиентского сокета TCP. Позволяет установить соединение с удаленным сервером и обмениваться данными.
    
    - Подробнее о **`SocketChannel`**
        
        Класс `**SocketChannel**` в пакете `**java.nio.channels**` представляет канал для клиентского сокета, который позволяет осуществлять операции ввода-вывода с использованием сокета TCP.
        
        Вот некоторые методы и функциональности класса `**SocketChannel**`:
        
        - `**open()**`: Создает новый `**SocketChannel**`. Метод `**open()**` является статическим и возвращает новый экземпляр `**SocketChannel**`.
        - `**isOpen()**`: Проверяет, открыт ли `**SocketChannel**`.
        - `**connect(SocketAddress remote)**`: Устанавливает соединение с указанным удаленным адресом.
        - `**finishConnect()**`: Завершает процесс соединения, начатый методом `**connect()**`.
        - `**isConnected()**`: Проверяет, установлено ли соединение.
        - `**configureBlocking(boolean block)**`: Устанавливает блокирующий или неблокирующий режим для `**SocketChannel**`.
        - `**register(Selector sel, int ops)**`: Регистрирует `**SocketChannel**` на заданном селекторе (`**Selector**`) для заданных операций ввода-вывода.
        - `**read(ByteBuffer dst)**`: Читает данные из `**SocketChannel**` в заданный буфер (`**ByteBuffer**`).
        - `**write(ByteBuffer src)**`: Записывает данные из заданного буфера (`**ByteBuffer**`) в `**SocketChannel**`.
        - `**socket()**`: Возвращает сокет, связанный с данным `**SocketChannel**`.
        - `**validOps()**`: Возвращает набор допустимых операций для данного `**SocketChannel**`.
        
        Пример, в котором мы создаем `**SocketChannel**`, устанавливаем неблокирующий режим и пытаемся установить соединение с удаленным сервером. Затем мы ожидаем завершения соединения с помощью метода `**finishConnect()**`. После этого мы можем выполнять операции чтения и записи данных с использованием `**SocketChannel**` и соответствующих буферов (`**ByteBuffer**`). После завершения работы с каналом мы его закрываем методом `**close()**`:
        
        ```Java
        try {
            // Создание SocketChannel
            SocketChannel channel = SocketChannel.open();
            channel.configureBlocking(false);
        
            // Установка соединения
            InetSocketAddress address = new InetSocketAddress("example.com", 8080);
            channel.connect(address);
        
            // Ожидание завершения соединения
            while (!channel.finishConnect()) {
                // Дополнительные действия при ожидании
            }
        
            // Чтение данных из канала
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int bytesRead = channel.read(buffer);
        
            // Обработка прочитанных данных
        
            // Запись данных в канал
            buffer.flip();
            int bytesWritten = channel.write(buffer);
        
            // Дополнительные действия с каналом
        
            // Закрытие канала
            channel.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        ```
        
        ---
        
    
    1. `**ServerSocketChannel**`: Предоставляет канал для серверного сокета TCP. Позволяет прослушивать входящие подключения от клиентов.
    
    - Подробнее о **`ServerSocketChannel`**
        
        Класс `**ServerSocketChannel**` в пакете `**java.nio.channels**` представляет канал для серверного сокета, который прослушивает входящие подключения от клиентов и создает новые `**SocketChannel**` для каждого установленного соединения.
        
        Вот некоторые методы и функциональности класса `**ServerSocketChannel**`:
        
        - `**open()**`: Создает новый `**ServerSocketChannel**`. Метод `**open()**` является статическим и возвращает новый экземпляр `**ServerSocketChannel**`.
        - `**isOpen()**`: Проверяет, открыт ли `**ServerSocketChannel**`.
        - `**bind(SocketAddress local)**`: Привязывает `**ServerSocketChannel**` к заданному локальному адресу и начинает прослушивать входящие подключения.
        - `**accept()**`: Принимает входящее подключение и возвращает новый `**SocketChannel**` для установленного соединения.
        - `**configureBlocking(boolean block)**`: Устанавливает блокирующий или неблокирующий режим для `**ServerSocketChannel**`.
        - `**register(Selector sel, int ops)**`: Регистрирует `**ServerSocketChannel**` на заданном селекторе (`**Selector**`) для заданных операций ввода-вывода.
        - `**socket()**`: Возвращает серверный сокет, связанный с данным `**ServerSocketChannel**`.
        - `**validOps()**`: Возвращает набор допустимых операций для данного `**ServerSocketChannel**`.
        
        Пример использования `**ServerSocketChannel**`:
        
        ```Java
        try {
            // Создание ServerSocketChannel
            ServerSocketChannel serverChannel = ServerSocketChannel.open();
            serverChannel.configureBlocking(false);
        
            // Привязка и прослушивание на локальном адресе
            InetSocketAddress address = new InetSocketAddress("localhost", 8080);
            serverChannel.bind(address);
        
            // Создание Selector
            Selector selector = Selector.open();
        
            // Регистрация ServerSocketChannel на Selector для прослушивания входящих подключений
            serverChannel.register(selector, SelectionKey.OP_ACCEPT);
        
            // Бесконечный цикл обработки событий
            while (true) {
                // Ожидание готовности каналов
                selector.select();
        
                // Получение выбранных ключей
                Set<SelectionKey> selectedKeys = selector.selectedKeys();
        
                // Обработка выбранных ключей
                for (SelectionKey key : selectedKeys) {
                    if (key.isAcceptable()) {
                        // Принятие входящего подключения
                        ServerSocketChannel server = (ServerSocketChannel) key.channel();
                        SocketChannel client = server.accept();
        
                        // Дополнительные действия с клиентским сокетом
        
                        // Регистрация клиентского сокета на Selector для чтения/записи
                        client.configureBlocking(false);
                        client.register(selector, SelectionKey.OP_READ | SelectionKey.OP_WRITE);
                    }
                    if (key.isReadable()) {
                        // Чтение данных с клиентского сокета
                        SocketChannel client = (SocketChannel) key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        int bytesRead = client.read(buffer);
        
                        // Обработка прочитанных данных
        
                        // Дополнительные действия с клиентским сокетом
                    }
                    if (key.isWritable()) {
                        // Запись данных в клиентский сокет
                        SocketChannel client = (SocketChannel) key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        buffer.put("Hello, client!".getBytes());
                        buffer.flip();
                        int bytesWritten = client.write(buffer);
        
                        // Дополнительные действия с клиентским сокетом
                    }
                }
        
                // Очистка выбранных ключей
                selectedKeys.clear();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        ```
        
        В данном примере мы создаем `**ServerSocketChannel**`, привязываем его к локальному адресу и регистрируем на `**Selector**` для прослушивания входящих подключений. Затем мы запускаем бесконечный цикл обработки событий, в котором ожидаем готовность каналов и обрабатываем выбранные ключи. При принятии нового подключения мы создаем новый `**SocketChannel**`, регистрируем его на `**Selector**` и выполняем дальнейшие операции чтения и записи с использованием `**SocketChannel**` и соответствующих буферов (`**ByteBuffer**`).
        
        ---
        
    
    1. `**DatagramChannel**`: Реализует канал для обмена датаграммами по протоколам UDP или SCTP.
    
    - Подробнее о `**DatagramChannel**`
        
        Класс `**DatagramChannel**` в пакете `**java.nio.channels**` представляет канал для обмена датаграммами через протокол UDP (User Datagram Protocol). Он обеспечивает возможность отправки и приема датаграмм между приложениями.
        
        Вот некоторые методы и функциональности класса `**DatagramChannel**`:
        
        - `**open()**`: Создает новый `**DatagramChannel**`. Метод `**open()**` является статическим и возвращает новый экземпляр `**DatagramChannel**`.
        - `**isOpen()**`: Проверяет, открыт ли `**DatagramChannel**`.
        - `**bind(SocketAddress local)**`: Привязывает `**DatagramChannel**` к заданному локальному адресу для прослушивания входящих датаграмм.
        - `**receive(ByteBuffer dst)**`: Принимает входящую датаграмму и сохраняет ее в заданный буфер (`**ByteBuffer**`).
        - `**send(ByteBuffer src, SocketAddress target)**`: Отправляет заданную датаграмму из буфера (`**ByteBuffer**`) по указанному адресу.
        - `**connect(SocketAddress remote)**`: Устанавливает соединение с удаленным адресом. После установки соединения можно использовать методы `**write()**` и `**read()**` вместо `**send()**` и `**receive()**`.
        - `**disconnect()**`: Разрывает текущее соединение.
        - `**configureBlocking(boolean block)**`: Устанавливает блокирующий или неблокирующий режим для `**DatagramChannel**`.
        - `**register(Selector sel, int ops)**`: Регистрирует `**DatagramChannel**` на заданном селекторе (`**Selector**`) для заданных операций ввода-вывода.
        - `**socket()**`: Возвращает сокет, связанный с данным `**DatagramChannel**`.
        - `**validOps()**`: Возвращает набор допустимых операций для данного `**DatagramChannel**`.
        
        Пример использования `**DatagramChannel**`:
        
        ```Java
        try {
            // Создание DatagramChannel
            DatagramChannel channel = DatagramChannel.open();
            channel.configureBlocking(false);
        
            // Привязка к локальному адресу
            InetSocketAddress address = new InetSocketAddress("localhost", 8080);
            channel.bind(address);
        
            // Создание буфера для чтения и записи данных
            ByteBuffer buffer = ByteBuffer.allocate(1024);
        
            // Отправка датаграммы
            String message = "Hello, server!";
            ByteBuffer sendBuffer = ByteBuffer.wrap(message.getBytes());
            SocketAddress target = new InetSocketAddress("localhost", 8080);
            channel.send(sendBuffer, target);
        
            // Прием датаграммы
            SocketAddress sender = channel.receive(buffer);
            buffer.flip();
            String receivedMessage = new String(buffer.array(), 0, buffer.limit());
        
            // Дополнительные действия с датаграммой
        
            // Закрытие канала
            channel.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        ```
        
        В данном примере мы создаем `**DatagramChannel**`, привязываем его к локальному адресу и отправляем датаграмму на указанный удаленный адрес. Затем мы ожидаем приема датаграммы и выполняем дополнительные действия с полученными данными. В конце мы закрываем канал с помощью метода `**close()**`.
        
        ---
        
    
    1. `**Pipe.SinkChannel**` и `**Pipe.SourceChannel**`: Реализуют каналы для чтения и записи данных между двумя потоками в рамках одного процесса.
    2. `**AsynchronousFileChannel**`: Предоставляет канал для асинхронного чтения и записи данных в файлы.
    
    - Подробнее о `**AsynchronousFileChannel**`
        
        Класс `**AsynchronousFileChannel**` в пакете `**java.nio.channels**` представляет асинхронный канал для работы с файлами. Он позволяет выполнять асинхронные операции ввода-вывода над файлами, такие как чтение и запись, без блокировки потока выполнения.
        
        Вот некоторые методы и функциональности класса `**AsynchronousFileChannel**`:
        
        - `**open(Path path, Set<OpenOption> options, ExecutorService executor, FileAttribute<?>... attrs)**`: Создает новый `**AsynchronousFileChannel**` для заданного пути к файлу с определенными опциями и атрибутами. Метод `**open()**` является статическим и возвращает новый экземпляр `**AsynchronousFileChannel**`.
        - `**isOpen()**`: Проверяет, открыт ли `**AsynchronousFileChannel**`.
        - `**read(ByteBuffer dst, long position)**`: Асинхронно читает данные из файла в заданный буфер (`**ByteBuffer**`) начиная с указанной позиции.
        - `**write(ByteBuffer src, long position)**`: Асинхронно записывает данные из заданного буфера (`**ByteBuffer**`) в файл начиная с указанной позиции.
        - `**close()**`: Закрывает `**AsynchronousFileChannel**`.
        - `**lock(long position, long size, boolean shared)**`: Асинхронно блокирует область файла, начиная с указанной позиции, с определенным размером и разделяемой или исключительной блокировкой.
        - `**tryLock(long position, long size, boolean shared)**`: Пытается асинхронно установить блокировку на область файла, начиная с указанной позиции, с определенным размером и разделяемой или исключительной блокировкой.
        
        Пример использования `**AsynchronousFileChannel**`:
        
        ```Java
        try {
            // Открытие AsynchronousFileChannel
            Path path = Paths.get("file.txt");
            AsynchronousFileChannel channel = AsynchronousFileChannel.open(path, StandardOpenOption.READ);
        
            // Создание буфера для чтения данных
            ByteBuffer buffer = ByteBuffer.allocate(1024);
        
            // Чтение данных из файла
            long position = 0; // Позиция, с которой начинается чтение
            Future<Integer> future = channel.read(buffer, position);
        
            // Дополнительные действия до завершения чтения
        
            // Ожидание завершения чтения
            int bytesRead = future.get();
        
            // Обработка прочитанных данных
        
            // Закрытие AsynchronousFileChannel
            channel.close();
        } catch (IOException | InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
        ```
        
        В данном примере мы открываем `**AsynchronousFileChannel**` для чтения файла и создаем буфер для чтения данных. Затем мы запускаем асинхронную операцию чтения данных из файла в буфер с использованием метода `**read()**`. Мы можем выполнять дополнительные действия до завершения чтения, а затем ожидать завершения операции чтения с помощью метода `**get()**` для объекта `**Future**`. После завершения чтения мы можем обработать прочитанные данные и закрыть `**AsynchronousFileChannel**` с помощью метода `**close()**`.
        

### FileLock

Класс `**FileLock**` в пакете `**java.nio.channels**` представляет блокировку файла, которая может быть захвачена `**FileChannel**` для предотвращения доступа других каналов к файлу. Блокировка файла может быть эксклюзивной (только один канал может захватить блокировку) или разделяемой (несколько каналов могут захватить блокировку одновременно).

Вот некоторые важные методы класса `**FileLock**`:

- `**channel()**`: Возвращает `**FileChannel**`, связанный с блокировкой файла.
- `**position()**`: Возвращает позицию начала блокировки в файле.
- `**size()**`: Возвращает размер блокировки файла.
- `**isShared()**`: Возвращает `**true**`, если блокировка разделяемая, и `**false**`, если блокировка эксклюзивная.
- `**isValid()**`: Возвращает `**true**`, если блокировка действительна, и `**false**`, если блокировка недействительна.
- `**release()**`: Освобождает блокировку файла. Это позволяет другим каналам получить доступ к файлу.

Класс `**FileLock**` позволяет управлять блокировками файла и проверять их состояние. Он используется вместе с `**FileChannel**` для реализации механизмов синхронизации при работе с файлами в многопоточных или многопроцессных средах. Например, блокировки файлов могут быть использованы для предотвращения одновременной записи в файл несколькими процессами или потоками.

Важно отметить, что блокировки файлов не гарантируют взаимное исключение между процессами или потоками. Они предоставляют только механизм предотвращения конфликтов доступа к файлу между блокирующими и неблокирующими операциями на `**FileChannel**`.

### Selector

Класс `**Selector**` в пакете `**java.nio.channels**` представляет механизм для мультиплексирования операций ввода-вывода на неблокирующих каналах. Он позволяет приложению эффективно обрабатывать несколько каналов ввода-вывода с использованием одного потока.

Основной концепцией `**Selector**` является регистрация каналов и выбор готовых каналов для выполнения операций ввода-вывода. Он определяет метод `**select()**`, который блокирует вызывающий поток до тех пор, пока один или более каналов не станут готовыми для операций ввода-вывода.

Вот некоторые методы и функциональности класса `**Selector**`:

- `**open()**`: Создает новый объект `**Selector**`. Метод `**open()**` является статическим и возвращает новый экземпляр `**Selector**`.
- `**isOpen()**`: Проверяет, открыт ли `**Selector**`.
- `**select()**`: Блокирует вызывающий поток и ожидает готовности одного или более каналов для операций ввода-вывода. Возвращает количество готовых каналов.
- `**select(long timeout)**`: Блокирует вызывающий поток и ожидает готовности каналов в течение указанного времени. Возвращает количество готовых каналов.
- `**selectNow()**`: Немедленно проверяет готовность каналов для операций ввода-вывода и возвращает количество готовых каналов. Этот метод не блокирует вызывающий поток.
- `**wakeup()**`: Разблокирует поток, который блокируется в методе `**select()**` или `**select(long timeout)**`.
- `**keys()**`: Возвращает набор ключей, зарегистрированных в `**Selector**`. Каждый ключ представляет зарегистрированный канал.
- `**selectedKeys()**`: Возвращает набор ключей, которые были выбраны в результате последнего вызова метода `**select()**`, `**select(long timeout)**` или `**selectNow()**`.
- `**close()**`: Закрывает `**Selector**` и освобождает все связанные ресурсы.

Пример использования `**Selector**`:

```Java
try {
    // Создание Selector
    Selector selector = Selector.open();

    // Регистрация канала на Selector
    SocketChannel channel = SocketChannel.open();
    channel.configureBlocking(false);
    channel.register(selector, SelectionKey.OP_READ);

    // Бесконечный цикл обработки готовых каналов
    while (true) {
        // Ожидание готовности каналов
        int readyChannels = selector.select();

        if (readyChannels == 0) {
            continue;
        }

        // Получение выбранных ключей
        Set<SelectionKey> selectedKeys = selector.selectedKeys();

        // Обработка выбранных ключей
        for (SelectionKey key : selectedKeys) {
            if (key.isReadable()) {
                // Чтение данных с канала
                SocketChannel socketChannel = (SocketChannel) key.channel();
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                int bytesRead = socketChannel.read(buffer);

                // Обработка прочитанных данных

                buffer.clear();
            }
        }

        // Очистка выбранных ключей
        selectedKeys.clear();
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

В данном примере мы создаем `**Selector**`, регистрируем неблокирующий `**SocketChannel**` на нем и затем в бесконечном цикле ожидаем готовности каналов для операций ввода-вывода. Когда канал становится готовым для чтения (`**isReadable()**`), мы считываем данные с канала и обрабатываем их.

### SelectionKey

Класс `**SelectionKey**` в пакете `**java.nio.channels**` представляет ключ, связанный с регистрацией канала на селекторе (`**Selector**`). Ключ предоставляет информацию о готовности канала для определенных операций ввода-вывода.

Типы SelectionKey:

- **SelectionKey.OP_CONNECT** — канал, который готов к подключению к серверу.
- **SelectionKey.OP_ACCEPT** — канал, который готов принимать входящие соединения.
- **SelectionKey.OP_READ** — канал, который готов к чтению данных.
- **SelectionKey.OP_WRITE** — канал, который готов к записи данных.

Методы класса `**SelectionKey**`:

- `**isValid()**`: Проверяет, является ли ключ действительным.
- `**channel()**`: Возвращает канал, связанный с данным ключом.
- `**selector()**`: Возвращает селектор, к которому данный ключ относится.
- `**interestOps()**`: Возвращает набор операций, для которых данный ключ зарегистрирован.
- `**interestOps(int ops)**`: Задает новый набор операций для данного ключа.
- `**readyOps()**`: Возвращает набор готовых операций для данного ключа.
- `**isReadable()**`: Проверяет, готов ли канал для операции чтения.
- `**isWritable()**`: Проверяет, готов ли канал для операции записи.
- `**isAcceptable()**`: Проверяет, готов ли канал для операции принятия соединения.
- `**isConnectable()**`: Проверяет, готов ли канал для операции установки соединения.
- `**attach(Object obj)**`: Присоединяет объект к данному ключу.
- `**attachment()**`: Возвращает объект, присоединенный к данному ключу.

Пример использования `**SelectionKey**`:

```Java
try {
    // Создание Selector
    Selector selector = Selector.open();

    // Регистрация канала на Selector
    SocketChannel channel = SocketChannel.open();
    channel.configureBlocking(false);
    SelectionKey key = channel.register(selector, SelectionKey.OP_READ);

    // Проверка готовности канала для операций
    if (key.isReadable()) {
        // Выполнение операции чтения
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        int bytesRead = channel.read(buffer);

        // Обработка прочитанных данных

        buffer.clear();
    }

    // Присоединение объекта к ключу
    String attachment = "Some data";
    key.attach(attachment);

    // Получение прикрепленного объекта
    String attachedData = (String) key.attachment();
} catch (IOException e) {
    e.printStackTrace();
}
```

### Files - работа с файлами и файловой системой

### Files

Класс `**Files**` в пакете `**java.nio.file**` предоставляет удобные методы для работы с файлами и директориями в файловой системе. Он предоставляет функциональность по созданию, удалению, перемещению, копированию файлов, а также другие операции с файловой системой.

Некоторые из основных методов класса `**Files**` включают:

- `**exists(Path path, LinkOption... options)**`: Проверяет, существует ли файл или директория по указанному пути.
- `**createFile(Path path, FileAttribute<?>... attrs)**`: Создает новый файл по указанному пути.
- `**createDirectory(Path dir, FileAttribute<?>... attrs)**`: Создает новую директорию по указанному пути.
- `**createDirectories(Path dir, FileAttribute<?>... attrs)**`: Создает директорию вместе с промежуточными директориями по указанному пути.
- `**delete(Path path)**`: Удаляет файл или директорию по указанному пути.
- `**move(Path source, Path target, CopyOption... options)**`: Перемещает файл или директорию из исходного пути в целевой путь.
- `**copy(Path source, Path target, CopyOption... options)**`: Копирует файл или директорию из исходного пути в целевой путь.
- `**readAllLines(Path path)**`: Читает все строки из файла и возвращает их в виде списка строк.
- `**write(Path path, byte[] bytes, OpenOption... options)**`: Записывает массив байтов в файл по указанному пути.
- `**readAttributes(Path path, Class<A> type, LinkOption... options)**`: Возвращает атрибуты файла или директории в виде объекта типа `**A**`.

- Больше методов тут:
    - `**deleteIfExists(Path path)**`
    - `**isDirectory(Path path, LinkOption... options)**`
    - `**isExecutable(Path path)**`
    - `**isHidden(Path path)**`
    - `**isReadable(Path path)**`
    - `**isRegularFile(Path path, LinkOption... options)**`
    - `**isSameFile(Path path, Path path2)**`
    - `**isSymbolicLink(Path path)**`
    - `**isWritable(Path path)**`
    - `**lines(Path path)**`
    - `**move(Path source, Path target, CopyOption... options)**`
    - `**newBufferedReader(Path path, Charset cs)**`
    - `**newBufferedWriter(Path path, Charset cs, OpenOption... options)**`
    - `**newByteChannel(Path path, Set<? extends OpenOption> options, FileAttribute<?>... attrs)**`
    - `**newDirectoryStream(Path dir)**`
    - `**readAllLines(Path path, Charset cs)**`
    - `**readAttributes(Path path, String attributes, LinkOption... options)**`
    - `**setAttribute(Path path, String attribute, Object value, LinkOption... options)**`
    - `**size(Path path)**`
    - `**walk(Path start, FileVisitOption... options)**`
    - `**write(Path path, Iterable<? extends CharSequence> lines, Charset cs, OpenOption... options)**`

Примеры использования класса `**Files**`:

1. Создание нового файла:

```Java
import java.nio.file.Files;
import java.nio.file.Path;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) {
        Path filePath = Path.of("path/to/file.txt");
        
        try {
            Files.createFile(filePath);
            System.out.println("Файл создан: " + filePath.toAbsolutePath());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

2. Копирование файла:

```Java
import java.nio.file.Files;
import java.nio.file.Path;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) {
        Path sourcePath = Path.of("path/to/source.txt");
        Path targetPath = Path.of("path/to/target.txt");
        
        try {
            Files.copy(sourcePath, targetPath);
            System.out.println("Файл скопирован.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

- Отличия `Files` от `File`
    
    Класс `**File**` и класс `**Files**` являются частями Java API для работы с файлами и директориями, однако они предоставляют различные наборы функциональности и появляются в разных версиях Java.
    
    Вот основные отличия между классом `**File**` и классом `**Files**`:
    
    1. API-интерфейс: Класс `**File**` был представлен в Java версии 1.0 и использует более старую модель файловой системы. Класс `**Files**` введен в Java версии 7 и является частью пакета `**java.nio.file**`, который предоставляет более современные и удобные методы для работы с файловой системой.
    2. Объекты представления: Класс `**File**` представляет одиночный файл или директорию в файловой системе и содержит методы для работы с этими объектами. Класс `**Files**` предоставляет статические методы для выполнения операций над файлами и директориями, но сам по себе не представляет отдельные файлы или директории.
    3. Функциональность: Класс `**File**` предоставляет базовые операции, такие как создание, удаление, перемещение и проверку существования файлов и директорий. Класс `**Files**` предоставляет более расширенные возможности, такие как копирование, чтение/запись файлов, работа с атрибутами файлов, создание символических ссылок и другие операции, которые удобнее выполнять с использованием методов класса `**Files**`.
    4. Обработка исключений: Класс `**File**` обрабатывает ошибки и исключения путем генерации исключений `**IOException**`. Класс `**Files**` также генерирует исключения `**IOException**`, но также может использовать другие исключения, такие как `**NoSuchFileException**` или `**FileAlreadyExistsException**`, чтобы указать конкретные сценарии ошибок.
    
    В целом, класс `**Files**` является более современным и удобным способом работы с файловой системой в Java, предоставляя более широкий набор функциональности и улучшенную обработку ошибок. Рекомендуется использовать класс `**Files**` для новых проектов и миграции существующего кода к нему, если это возможно.
    

### Path

Класс `**Path**` в пакете `**java.nio.file**` представляет путь к файлу или директории в файловой системе. Он предоставляет методы для работы с путями, обеспечивая удобный способ выполнения операций с файлами и директориями независимо от операционной системы.

Основные методы:

1. `**getFileName()**`: Возвращает имя файла или директории в виде объекта `**Path**`.
2. `**getParent()**`: Возвращает родительскую директорию в виде объекта `**Path**`.
3. `**getRoot()**`: Возвращает корневую директорию в виде объекта `**Path**`.
4. `**resolve(Path other)**`: Разрешает путь относительно другого пути.
5. `**resolve(String other)**`: Разрешает путь относительно строки.
6. `**relativize(Path other)**`: Вычисляет относительный путь между двумя путями.
7. `**normalize()**`: Нормализует путь, удаляя точки и двойные точки.
8. `**toAbsolutePath()**`: Преобразует путь в абсолютный путь.
9. `**startsWith(Path other)**`: Проверяет, начинается ли данный путь с указанного пути.
10. `**endsWith(Path other)**`: Проверяет, заканчивается ли данный путь указанным путем.
11. `**getName(int index)**`: Возвращает компонент пути по заданному индексу.
12. `**getNameCount()**`: Возвращает количество компонентов пути.
13. `**subpath(int beginIndex, int endIndex)**`: Возвращает подпуть между заданными индексами.

Вот некоторые особенности класса `**Path**`:

1. Создание объекта `**Path**`: Объекты `**Path**` могут быть созданы с использованием статического метода `**of()**` или с помощью методов класса `**Paths**`. Например:

```Java
Path path1 = Path.of("path/to/file.txt");
Path path2 = Paths.get("path", "to", "file.txt");
```

2. Работа с компонентами пути: Класс `**Path**` предоставляет методы для получения различных компонентов пути, таких как имя файла, родительская директория, а также методы для добавления, удаления и изменения компонентов пути. Например:

```Java
Path path = Path.of("path/to/file.txt");
System.out.println(path.getFileName()); // Выводит "file.txt"
System.out.println(path.getParent()); // Выводит "path/to"
Path newPath = path.resolveSibling("newfile.txt"); // Создает новый путь "path/to/newfile.txt"
```

3. Разрешение пути: Класс `**Path**` предоставляет методы для разрешения пути относительно другого пути, а также для нормализации пути. Например:

```Java
Path basePath = Path.of("path/to");
Path relativePath = Path.of("file.txt");
Path resolvedPath = basePath.resolve(relativePath); // Создает новый путь "path/to/file.txt"
Path normalizedPath = resolvedPath.normalize(); // Нормализует путь (удаляет ".." и "."), если они есть
```

4. Проверка существования и типа пути: Класс `**Path**` предоставляет методы для проверки существования пути, а также для определения его типа, такого как файл, директория или символическая ссылка. Например:

```Java
Path path = Path.of("path/to/file.txt");
System.out.println(Files.exists(path)); // Проверяет, существует ли файл или директория
System.out.println(Files.isDirectory(path)); // Проверяет, является ли путь директорией
System.out.println(Files.isRegularFile(path)); // Проверяет, является ли путь обычным файлом
System.out.println(Files.isSymbolicLink(path)); // Проверяет, является ли путь символической ссылкой
```

5. Итерация по компонентам пути: Класс `**Path**` предоставляет методы для итерации по компонентам пути, таким как корневой элемент, имена директорий и имя файла. Например:

```Java
Path path = Path.of("path/to/file.txt");
for (Path component : path) {
    System.out.println(component);
}
```

### Paths

Класс `**Paths**` в пакете `**java.nio.file**` предоставляет статические методы для работы с путями файлов и директорий. Он служит для создания объектов `**Path**`, представляющих пути в файловой системе.

Вот некоторые методы класса `**Paths**`:

1. `**get(String first, String... more)**`: Создает объект `**Path**` из компонентов пути. Принимает первый компонент пути и дополнительные компоненты пути в виде строки переменной длины. ==!СОЗДАЕТ СПЕЦИФИЧНЫЙ ДЛЯ ОС ПУТЬ!==
2. `**get(URI uri)**`: Создает объект `**Path**` из объекта `**URI**`, представляющего путь.
3. `**of(String path)**`: Создает объект `**Path**` из строки, представляющей путь.
4. `**of(URI uri)**`: Создает объект `**Path**` из объекта `**URI**`, представляющего путь.
5. `**getFileSystem()**`: Возвращает объект `**FileSystem**`, представляющий файловую систему по умолчанию.
6. `**newInputStream(Path path, OpenOption... options)**`: Создает поток `**InputStream**` для чтения данных из файла, указанного по пути.
7. `**newOutputStream(Path path, OpenOption... options)**`: Создает поток `**OutputStream**` для записи данных в файл, указанный по пути.
8. `**exists(Path path, LinkOption... options)**`: Проверяет, существует ли файл или директория, указанные по пути.
9. `**notExists(Path path, LinkOption... options)**`: Проверяет, не существует ли файл или директория, указанные по пути.
10. `**isSameFile(Path path1, Path path2)**`: Проверяет, являются ли два пути ссылками на один и тот же файл.

### FileSystem

Класс `**FileSystem**` в пакете `**java.nio.file**` представляет абстракцию файловой системы, предоставляя методы для доступа к различным аспектам файловой системы, таким как корневые директории, файловые провайдеры и другие функции.

Вот некоторые методы класса `**FileSystem**`:

1. `**getPath(String first, String... more)**`: Возвращает объект `**Path**` для заданного пути. Этот метод является альтернативой методу `**Paths.get()**` и позволяет создавать объекты `**Path**` специфичные для данной файловой системы.
2. `**getFileStores()**`: Возвращает итератор по доступным файловым хранилищам (файловым системам), предоставляемым данной файловой системой.
3. `**getRootDirectories()**`: Возвращает итератор по корневым директориям файловой системы.
4. `**supportedFileAttributeViews()**`: Возвращает набор поддерживаемых представлений атрибутов файлов данной файловой системы.
5. `**getPathMatcher(String syntaxAndPattern)**`: Возвращает объект `**PathMatcher**` для заданного синтаксиса и шаблона. Позволяет выполнять сопоставление путей файлов с заданными шаблонами.
6. `**getUserPrincipalLookupService()**`: Возвращает объект `**UserPrincipalLookupService**`, который предоставляет возможность получать информацию о пользователях и группах в файловой системе.
7. `**isOpen()**`: Проверяет, открыта ли файловая система.
8. `**isReadOnly()**`: Проверяет, доступна ли файловая система только для чтения.
9. `**close()**`: Закрывает файловую систему и освобождает все связанные ресурсы.

`**FileSystem**` - это абстрактный класс, и для работы с конкретной файловой системой можно использовать реализацию этого класса, такую как `**FileSystems.getDefault()**`, чтобы получить файловую систему по умолчанию.

  

### FileSystems

Класс `**FileSystems**` в пакете `**java.nio.file**` предоставляет статические методы для создания объектов `**FileSystem**` и получения доступа к файловым системам.

Методы:

1. `**getDefault()**`: Возвращает объект `**FileSystem**` для работы с файловой системой по умолчанию.
2. `**getFileSystem(URI uri)**`: Возвращает объект `**FileSystem**` для заданного URI. Этот метод позволяет получить доступ к файловой системе, представленной указанным URI.

```Java
URI uri = new URI("file:///path/to/file.txt");
FileSystem fileSystem = FileSystems.getFileSystem(uri);
```

1. `**newFileSystem(Path path, Map<String, ?> env)**`: Создает новый объект `**FileSystem**` для указанного пути. Дополнительные параметры окружения могут быть переданы через `**Map**`.
2. `**newFileSystem(Path path, ClassLoader loader)**`: Создает новый объект `**FileSystem**` для указанного пути с использованием указанного загрузчика классов.
3. `**newFileSystem(URI uri, Map<String, ?> env, ClassLoader loader)**`: Создает новый объект `**FileSystem**` для указанного URI с использованием указанного загрузчика классов и дополнительных параметров окружения.

### FileAttribute

Атрибуты файла представляют метаданные или свойства файла, такие как разрешения доступа, дата создания, размер и т. д. Они могут использоваться при создании или модификации файлов с помощью классов из пакета `**java.nio.file**`.

Вместо определенного интерфейса `**FileAttribute**` в Java API, вы можете определять свои собственные атрибуты файла, реализуя соответствующие интерфейсы и классы из пакета `**java.nio.file.attribute**`, такие как `**BasicFileAttributes**`, `**PosixFileAttributes**`, `**DosFileAttributes**` и т. д. Эти интерфейсы предоставляют методы для доступа к различным атрибутам файлов, в зависимости от поддержки конкретной файловой системы.

Пример создания и установки атрибутов файла может выглядеть следующим образом:

```Java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.attribute.FileAttribute;
import java.nio.file.attribute.PosixFilePermissions;

public class FileAttributesExample {
    public static void main(String[] args) throws IOException {
        // Создание нового файла с атрибутами
        Path filePath = Paths.get("path/to/file.txt");
        FileAttribute<?>[] attributes = { 
            PosixFilePermissions.asFileAttribute(PosixFilePermissions.fromString("rw-r--r--"))
        };
        Files.createFile(filePath, attributes);
        
        // Получение атрибутов существующего файла
        BasicFileAttributes fileAttributes = Files.readAttributes(filePath, BasicFileAttributes.class);
        System.out.println("Size: " + fileAttributes.size());
        System.out.println("Creation Time: " + fileAttributes.creationTime());
        System.out.println("Last Modified Time: " + fileAttributes.lastModifiedTime());
        System.out.println("Is Directory: " + fileAttributes.isDirectory());
        // и так далее...
    }
}
```

### OpenOption

Интерфейс представляет опции для открытия файлов и других операций ввода-вывода. Он определяет методы, которые используются для указания дополнительных параметров при открытии файла.

Реализации:

1. `**StandardOpenOption**`: Перечисление, предоставляющее стандартные опции открытия файлов. Некоторые из наиболее распространенных значения включают:
    - `**READ**`: Файл открывается для чтения.
    - `**WRITE**`: Файл открывается для записи.
    - `**APPEND**`: Запись в файл будет происходить в конец файла.
    - `**CREATE**`: Если файл не существует, он будет создан.
    - `**TRUNCATE_EXISTING**`: Существующий файл будет обрезан до нулевой длины при открытии.
2. `**StandardCopyOption**`: Перечисление, предоставляющее стандартные опции для копирования файлов. Некоторые из наиболее распространенных значения включают:
    - `**REPLACE_EXISTING**`: Если целевой файл уже существует, он будет заменен.
3. `**LinkOption**`: Перечисление, предоставляющее опции для работы с символическими ссылками. Некоторые из наиболее распространенных значений включают:
    - `**NOFOLLOW_LINKS**`: Символические ссылки не будут разыменованы.

Эти опции могут быть переданы в методы, такие как `**Files.newInputStream()**`, `**Files.newOutputStream()**`, `**Files.copy()**` и другие, для настройки поведения операций открытия и копирования файлов.

Пример:

```Java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

public class OpenOptionExample {
    public static void main(String[] args) {
        Path filePath = Path.of("path/to/file.txt");
        
        try {
            // Открытие файла для чтения и записи
            Files.newOutputStream(filePath, StandardOpenOption.READ, StandardOpenOption.WRITE);
            
            // Дополнительные операции...
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

  

---

![[images/Untitled 9 2.png|Untitled 9 2.png]]