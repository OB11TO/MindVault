### Аннотации Lombok

- `@Getter`: Генерирует геттеры для полей класса.

```Java
import lombok.Getter;

public class Auto {


    @Getter
    private Engine engine;
    private String model;
    @Getter
    private Integer price;

}
```

- `@Setter`: Генерирует сеттеры для полей класса.

```Java
import lombok.Getter;
import lombok.Setter;
@Getter
@Setter
public class Auto {
    
    private Engine engine;
    private String model;
    private Integer price;

}
```

- `@ToString`: Генерирует метод toString() для класса, включая все его поля.

```Java
import lombok.AllArgsConstructor;
import lombok.ToString;

@ToString
@AllArgsConstructor
public class Auto {

    private Engine engine;
    private String model;
    private Integer price;

}

Auto auto = new Auto(Engine.ELECTRIC,"Hyundai", 1000);

System.out.println(auto); //Auto(engine=ELECTRIC, model=Hyundai, price=1000)
```

- `@EqualsAndHashCode`: Генерирует методы equals() и hashCode() для класса на основе его полей.
- `@NoArgsConstructor`: Генерирует конструктор без аргументов.

```Java
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
public class Auto {

    private Engine engine;
    private String model;
    private Integer price;

}

Auto auto = new Auto(Engine.ELECTRIC,"Hyundai", 1000);
Auto autoWithoutArgs = new Auto();
```

- `@RequiredArgsConstructor`: Генерирует конструктор, принимающий все final и @NonNull поля класса.
- `@Data`: Комбинированная аннотация, эквивалентная использованию `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode` и `@RequiredArgsConstructor` вместе.

```Java
import lombok.AllArgsConstructor;
import lombok.Data;

@Data
public class Auto {

    private Engine engine;
    private String model;
    private Integer price;

}
```

- `@Builder`: Генерирует класс-строитель для создания экземпляров класса с помощью паттерна Builder.
    
    ```Java
    import lombok.Builder;
    
    @Builder
    public class Auto {
    
        public Engine engine;
        public String model;
        public Integer price;
    
    }
    
    psvm(){
    Auto auto = Auto.builder()
                    .model("Hyundai")
                    .engine(Engine.ELECTRIC)
                    .price(1000)
                    .build();
    }
    ```
    

Также можно навесить билдер не только над самим классом, но и над конструктором, в том числе, над конструктором наследника, чтобы иметь доступ к полям родителя через билдер

```Java
public class Programmer extends User{

    @Enumerated(value = EnumType.STRING)
    private Language language;
    
    @Builder
    public Programmer(Long id, Long credentials, Language language) {
        super(id, credentials);
        this.language = language;
    }
}
```

- `@Value`: Создает класс, иммутабельный (неизменяемый), сгенерированный Lombok.

```Java
public final class FlightDto {
    private final Integer ID;
    private final String DESCRIPTION;
}

@Value
public class FlightDto {
     Integer ID;
     String DESCRIPTION;
}
```

- `@Slf4j`: Генерирует логгер с использованием SLF4J.

```Java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class Logger {

    public void logging(){
        log.warn("Warning!");
    }
}
```

- `@Log`: Генерирует логгер с использованием java.util.logging.

```Java
import lombok.extern.java.Log;

@Log
public class Logger {

    public void logging(){
        log.warning("Warning!");
    }
}
```

- `@NonNull`: Применяется к параметру метода или полю класса для указания, что значение не может быть null.

```Java
@Value
public class FlightDto {
    @NonNull
    Integer ID;
    String DESCRIPTION;
}
```

- `@Cleanup`: Гарантирует автоматическое закрытие ресурса (например, поток или соединение с базой данных).

```Java
import lombok.Cleanup;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileExample {
    public static void main(String[] args) {
        copyFile("input.txt", "output.txt");
    }

    public static void copyFile(String inputFilePath, String outputFilePath) {
        @Cleanup FileInputStream inputFile = null;
        @Cleanup FileOutputStream outputFile = null;

        try {
            inputFile = new FileInputStream(inputFilePath);
            outputFile = new FileOutputStream(outputFilePath);

            byte[] buffer = new byte[1024];
            int bytesRead;
            while ((bytesRead = inputFile.read(buffer)) != -1) {
                outputFile.write(buffer, 0, bytesRead);
            }

            System.out.println("File copied successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- `@Synchronized`: Генерирует синхронизированный блок кода для метода или блока.

```Java
import lombok.Synchronized;

public class Counter {
    private int count;

    public Counter() {
        count = 0;
    }

    @Synchronized
    public void increment() {
        count++;
}
```

- `@AllArgsConstructor`: Генерирует конструктор, принимающий все поля класса.

```Java
import lombok.AllArgsConstructor;

@AllArgsConstructor
public class Auto {

    public Engine engine;
    public String model;
    public Integer price;

}
```

- `@AllArgsConstructor(staticName = "of")`: Генерирует конструктор, принимающий все поля класса, а также статический фабричный метод `of()`, который используется для создания экземпляра класса.

```Java

import lombok.AllArgsConstructor;
@AllArgsConstructor(staticName = "of")
public class Person {

    Integer age;
    String name;
}

psvm(String[] args){
Person person = Person.of(1, "Vadim");
}
```

- `@FieldDefaults`: Настраивает уровень доступа и модификаторы для полей класса.

```Java
import lombok.AccessLevel;
import lombok.experimental.FieldDefaults;

@FieldDefaults(level = AccessLevel.PRIVATE, makeFinal = true)
public class Person {
    String name;
    int age;
}
```

- Аннотация `@SneakyThrows` в проекте Lombok позволяет преобразовывать проверяемые исключения в непроверяемые исключения (unchecked exceptions) без явного использования `try-catch` блоков. Обратите внимание, что использование `@SneakyThrows` упрощает код, но также делает его менее читаемым и может скрыть ошибки, поэтому используйте его с осторожностью и только тогда, когда вы уверены, что это безопасно.Вот пример, как использовать `@SneakyThrows`:

```Java
import lombok.SneakyThrows;

public class Example {
    @SneakyThrows
    public void doSomething() {
        // Ваш код, который может бросить проверяемое исключение
        throw new Exception("Пример исключения");
    }

    public static void main(String[] args) {
        Example example = new Example();
        example.doSomething(); // Здесь исключение преобразуется в непроверяемое исключение
    }
}
```

  

### lombok.config

![[images/Untitled 146.png|Untitled 146.png]]

Внутри этого конфига уже можно прописывать необходимую конфигурацию.

Чтобы посмотреть все доступные ключи, можно скачать jar и выполнить

```Bash
 java -jar lombok.jar config -g --verbose
```

Определенные ключи позволяют упростить наши классы

```Java
@Data
@Component
public class ConnectionPool {
    
    private final String userName;
    private final String password;
    public ConnectionPool(@Value(value = "${db.userName}")String userName,
                          @Value(value = "${db.pool.size}") String poolSize) {
        this.userName = userName;
        this.password = poolSize;
    }
}
```

Если прописать в config, которая позволяет автоматически инжектить значения в конструктор создаваемый с помощью Lombok

```Java
lombok.copyableannotations += org.springframework.beans.factory.annotation.Value
```

То класс будет выглядеть следюущим образом

```Java
@Data
@Component
@RequiredArgsConstructor
public class ConnectionPool {
    
    @Value(value = "${db.userName}")
    private final String userName;
    
    @Value(value = "${db.pool.size}")
    private final String password;
}
```

### Config keys

- **Key : checkerframework**
    - **Type**: checkerframework-version
    - **Description**: Если установлено с версией [checkerframework.org](http://checkerframework.org/) (в формате major.minor) или 'true' для последней поддерживаемой версии, создайте соответствующие аннотации [checkerframework.org](http://checkerframework.org/) для кода, который генерируется Lombok.
    - **Примеры**:
        - `clear checkerframework`
        - `checkerframework = major.minor (пример: 3.2 - и не выше 4.0) или true или false`
- **Key : config.stopBubbling**
    - **Type**: boolean
    - **Description**: Указывает системе конфигурации прекратить поиск других файлов конфигурации (по умолчанию: false).
    - **Примеры**:
        - `clear config.stopBubbling`
        - `config.stopBubbling = [false | true]`
- **Key : lombok.accessors.capitalization**
    - **Type**: enum (lombok.core.configuration.CapitalizationStrategy)
    - **Description**: Какую стратегию использовать при преобразовании имен полей в имена методов доступа и наоборот (по умолчанию: basic).
    - **Примеры**:
        - `clear lombok.accessors.capitalization`
        - `lombok.accessors.capitalization = [BASIC | BEANSPEC]`
- **Key : lombok.accessors.chain**
    - **Type**: boolean
    - **Description**: Генерировать методы-сеттеры, которые возвращают 'this' вместо 'void' (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.accessors.chain`
        - `lombok.accessors.chain = [false | true]`
- **Key : lombok.accessors.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Accessors.
    - **Примеры**:
        - `clear lombok.accessors.flagUsage`
        - `lombok.accessors.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.accessors.fluent**
    - **Type**: boolean
    - **Description**: Генерировать геттеры и сеттеры, используя только имя поля, без префикса get/set (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.accessors.fluent`
        - `lombok.accessors.fluent = [false | true]`
- **Key : lombok.accessors.makeFinal**
    - **Type**: boolean
    - **Description**: Генерировать геттеры, сеттеры и методы с префиксом 'with' с модификатором 'final' (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.accessors.makeFinal`
        - `lombok.accessors.makeFinal = [false | true]`
- **Key : lombok.accessors.prefix**
    - **Type**: list of string
    - **Description**: Удалять указанный префикс из имен, генерируемых геттеров, сеттеров и методов 'with'.
    - **Примеры**:
        - `clear lombok.accessors.prefix`
        - `lombok.accessors.prefix += <text>`
        - `lombok.accessors.prefix -= <text>`
- **Key : lombok.addGeneratedAnnotation**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @javax.annotation.Generated для всего сгенерированного кода (по умолчанию: false). Устарело, используйте 'lombok.addJavaxGeneratedAnnotation' вместо этого.
    - **Примеры**:
        - `clear lombok.addGeneratedAnnotation`
        - `lombok.addGeneratedAnnotation = [false | true]`
- **Key : lombok.addJakartaGeneratedAnnotation**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @jakarta.annotation.Generated для всего сгенерированного кода (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.addJakartaGeneratedAnnotation`
        - `lombok.addJakartaGeneratedAnnotation = [false | true]`
- **Key : lombok.addJavaxGeneratedAnnotation**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @javax.annotation.Generated для всего сгенерированного кода (по умолчанию: следовать значению 'lombok.addGeneratedAnnotation').
    - **Примеры**:
        - `clear lombok.addJavaxGeneratedAnnotation`
        - `lombok.addJavaxGeneratedAnnotation = [false | true]`
- **Key : lombok.addLombokGeneratedAnnotation**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @lombok.Generated для всего сгенерированного кода (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.addLombokGeneratedAnnotation`
        - `lombok.addLombokGeneratedAnnotation = [false | true]`
- **Key : lombok.addNullAnnotations**
    - **Type**: nullity-annotation-library
    - **Description**: Генерировать некоторый стиль аннотации null для сгенерированного кода, где это применимо (по умолчанию: none).
    - **Примеры**:
        - `clear lombok.addNullAnnotations`
        - `lombok.addNullAnnotations = none | javax | jakarta | eclipse | jetbrains | netbeans | androidx | android.support | checkerframework | findbugs | spring | jml | CUSTOM:com.foo.my.nonnull.annotation:com.foo.my.nullable.annotation`
- **Key : lombok.addSuppressWarnings**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @java.lang.SuppressWarnings("all") для всего сгенерированного кода (по умолчанию: true).
    - **Примеры**:
        - `clear lombok.addSuppressWarnings`
        - `lombok.addSuppressWarnings = [false | true]`
- **Key : lombok.allArgsConstructor.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @AllArgsConstructor.
    - **Примеры**:
        - `clear lombok.allArgsConstructor.flagUsage`
        - `lombok.allArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.anyConstructor.addConstructorProperties**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @ConstructorProperties для сгенерированных конструкторов (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.anyConstructor.addConstructorProperties`
        - `lombok.anyConstructor.addConstructorProperties = [false | true]`
- **Key : lombok.anyConstructor.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется одна из аннотаций XxxArgsConstructor.
    - **Примеры**:
        - `clear lombok.anyConstructor.flagUsage`
        - `lombok.anyConstructor.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.anyConstructor.suppressConstructorProperties**
    - **Type**: boolean
    - **Description**: Подавлять генерацию аннотации @ConstructorProperties для сгенерированных конструкторов (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.anyConstructor.suppressConstructorProperties`
        - `lombok.anyConstructor.suppressConstructorProperties = [false | true]`
- **Key : lombok.builder.className**
    - **Type**: string
    - **Description**: Имя по умолчанию для сгенерированного класса-билдера. Знак * заменяется на имя соответствующего типа (по умолчанию = *Builder).
    - **Примеры**:
        - `clear lombok.builder.className`
        - `lombok.builder.className = <text>`
- **Key : lombok.builder.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Builder.
    - **Примеры**:
        - `clear lombok.builder.flagUsage`
        - `lombok.builder.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.cleanup.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Cleanup.
    - **Примеры**:
        - `clear lombok.cleanup.flagUsage`
        - `lombok.cleanup.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.copyableAnnotations**
    - **Type**: list of type-name
    - **Description**: Копировать эти аннотации для геттеров, сеттеров, методов with, сеттеров билдера и т. д.
    - **Примеры**:
        - `clear lombok.copyableAnnotations`
        - `lombok.copyableAnnotations += <fully.qualified.Type>`
        - `lombok.copyableAnnotations -= <fully.qualified.Type>`
- **Key : lombok.data.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Data.
    - **Примеры**:
        - `clear lombok.data.flagUsage`
        - `lombok.data.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.delegate.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Delegate.
    - **Примеры**:
        - `clear lombok.delegate.flagUsage`
        - `lombok.delegate.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.equalsAndHashCode.callSuper**
    - **Type**: enum (lombok.core.configuration.CallSuperType)
    - **Description**: При генерации методов equals и hashCode для классов, расширяющих что-то (кроме Object), либо автоматически учитывать реализацию суперкласса (call), либо нет (skip), либо выводить предупреждение и не учитывать (warn) (по умолчанию = warn).
    - **Примеры**:
        - `clear lombok.equalsAndHashCode.callSuper`
        - `lombok.equalsAndHashCode.callSuper = [CALL | SKIP | WARN]`
- **Key : lombok.equalsAndHashCode.doNotUseGetters**
    - **Type**: boolean
    - **Description**: Не вызывать геттеры, а использовать поля напрямую в сгенерированных методах equals и hashCode (по умолчанию = false).
    - **Примеры**:
        - `clear lombok.equalsAndHashCode.doNotUseGetters`
        - `lombok.equalsAndHashCode.doNotUseGetters = [false | true]`
- **Key : lombok.equalsAndHashCode.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @EqualsAndHashCode.
    - **Примеры**:
        - `clear lombok.equalsAndHashCode.flagUsage`
        - `lombok.equalsAndHashCode.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.experimental.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется экспериментальная функция.
    - **Примеры**:
        - `clear lombok.experimental.flagUsage`
        - `lombok.experimental.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.extensionMethod.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @ExtensionMethod.
    - **Примеры**:
        - `clear lombok.extensionMethod.flagUsage`
        - `lombok.extensionMethod.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.extern.findbugs.addSuppressFBWarnings**
    - **Type**: boolean
    - **Description**: Генерировать аннотацию @edu.umd.cs.findbugs.annotations.SuppressFBWarnings для всего сгенерированного кода (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.extern.findbugs.addSuppressFBWarnings`
        - `lombok.extern.findbugs.addSuppressFBWarnings = [false | true]`
- **Key : lombok.fieldDefaults.defaultFinal**
    - **Type**: boolean
    - **Description**: Если true, поля, в любом файле (с аннотацией Lombok или без), помечаются как final. Используйте @NonFinal, чтобы переопределить это.
    - **Примеры**:
        - `clear lombok.fieldDefaults.defaultFinal`
        - `lombok.fieldDefaults.defaultFinal = [false | true]`
- **Key : lombok.fieldDefaults.defaultPrivate**
    - **Type**: boolean
    - **Description**: Если true, поля без модификатора доступа, в любом файле (с аннотацией Lombok или без), помечаются как private. Используйте @PackagePrivate или явно задайте модификатор, чтобы переопределить это.
    - **Примеры**:
        - `clear lombok.fieldDefaults.defaultPrivate`
        - `lombok.fieldDefaults.defaultPrivate = [false | true]`
- **Key : lombok.fieldDefaults.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @FieldDefaults.
    - **Примеры**:
        - `clear lombok.fieldDefaults.flagUsage`
        - `lombok.fieldDefaults.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.fieldNameConstants.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @FieldNameConstants.
    - **Примеры**:
        - `clear lombok.fieldNameConstants.flagUsage`
        - `lombok.fieldNameConstants.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.fieldNameConstants.innerTypeName**
    - **Type**: identifier-name
    - **Description**: Имя по умолчанию для внутреннего типа, сгенерированного аннотацией @FieldNameConstants (по умолчанию: 'Fields').
    - **Примеры**:
        - `clear lombok.fieldNameConstants.innerTypeName`
        - `lombok.fieldNameConstants.innerTypeName = <javaIdentifier>`
- **Key : lombok.fieldNameConstants.uppercase**
    - **Type**: boolean
    - **Description**: Имя по умолчанию для констант во внутреннем типе, сгенерированном аннотацией @FieldNameConstants, будет точно соответствовать имени переменной. Если эта настройка установлена в true, Lombok будет преобразовывать их в верхний регистр, насколько это возможно (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.fieldNameConstants.uppercase`
        - `lombok.fieldNameConstants.uppercase = [false | true]`
- **Key : lombok.getter.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Getter.
    - **Примеры**:
        - `clear lombok.getter.flagUsage`
        - `lombok.getter.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.getter.lazy.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Getter(lazy=true).
    - **Примеры**:
        - `clear lombok.getter.lazy.flagUsage`
        - `lombok.getter.lazy.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.getter.noIsPrefix**
    - **Type**: boolean
    - **Description**: Если true, генерировать и использовать методы getFieldName() для boolean-геттеров вместо isFieldName().
    - **Примеры**:
        - `clear lombok.getter.noIsPrefix`
        - `lombok.getter.noIsPrefix = [false | true]`
- **Key : lombok.helper.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Helper.
    - **Примеры**:
        - `clear lombok.helper.flagUsage`
        - `lombok.helper.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.jacksonized.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Jacksonized.
    - **Примеры**:
        - `clear lombok.jacksonized.flagUsage`
        - `lombok.jacksonized.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.apacheCommons.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @CommonsLog.
    - **Примеры**:
        - `clear lombok.log.apacheCommons.flagUsage`
        - `lombok.log.apacheCommons.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.custom.declaration**
    - **Type**: custom-log-declaration
    - **Description**: Задать сгенерированное пользовательское поле логгера.
    - **Примеры**:
        - `clear lombok.log.custom.declaration`
        - `lombok.log.custom.declaration = my.cool.Logger my.cool.LoggerFactory.createLogger()(TOPIC,TYPE)`
- **Key : lombok.log.custom.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @CustomLog.
    - **Примеры**:
        - `clear lombok.log.custom.flagUsage`
        - `lombok.log.custom.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.fieldIsStatic**
    - **Type**: boolean
    - **Description**: Создавать сгенерированные поля логгера как статические (по умолчанию: true).
    - **Примеры**:
        - `clear lombok.log.fieldIsStatic`
        - `lombok.log.fieldIsStatic = [false | true]`
- **Key : lombok.log.fieldName**
    - **Type**: identifier-name
    - **Description**: Использовать это имя для сгенерированных полей логгера (по умолчанию: 'log').
    - **Примеры**:
        - `clear lombok.log.fieldName`
        - `lombok.log.fieldName = <javaIdentifier>`
- **Key : lombok.log.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется хотя бы одна из аннотаций логгера.
    - **Примеры**:
        - `clear lombok.log.flagUsage`
        - `lombok.log.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.flogger.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Flogger.
    - **Примеры**:
        - `clear lombok.log.flogger.flagUsage`
        - `lombok.log.flogger.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.javaUtilLogging.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Log.
    - **Примеры**:
        - `clear lombok.log.javaUtilLogging.flagUsage`
        - `lombok.log.javaUtilLogging.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.jbosslog.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @JBossLog.
    - **Примеры**:
        - `clear lombok.log.jbosslog.flagUsage`
        - `lombok.log.jbosslog.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.log4j.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Log4j.
    - **Примеры**:
        - `clear lombok.log.log4j.flagUsage`
        - `lombok.log.log4j.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.log4j2.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Log4j2.
    - **Примеры**:
        - `clear lombok.log.log4j2.flagUsage`
        - `lombok.log.log4j2.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.slf4j.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Slf4j.
    - **Примеры**:
        - `clear lombok.log.slf4j.flagUsage`
        - `lombok.log.slf4j.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.log.xslf4j.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @XSlf4j.
    - **Примеры**:
        - `clear lombok.log.xslf4j.flagUsage`
        - `lombok.log.xslf4j.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.noArgsConstructor.extraPrivate**
    - **Type**: boolean
    - **Description**: Генерировать приватный конструктор без аргументов для аннотаций @Data и @Value (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.noArgsConstructor.extraPrivate`
        - `lombok.noArgsConstructor.extraPrivate = [false | true]`
- **Key : lombok.noArgsConstructor.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @NoArgsConstructor.
    - **Примеры**:
        - `clear lombok.noArgsConstructor.flagUsage`
        - `lombok.noArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.nonNull.exceptionType**
    - **Type**: enum (lombok.core.configuration.NullCheckExceptionType)
    - **Description**: Тип исключения, которое будет сгенерировано, если переданный аргумент равен null (по умолчанию: NullPointerException).
    - **Примеры**:
        - `clear lombok.nonNull.exceptionType`
        - `lombok.nonNull.exceptionType = [NullPointerException | IllegalArgumentException | Assertion | JDK | Guava]`
- **Key : lombok.nonNull.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @NonNull.
    - **Примеры**:
        - `clear lombok.nonNull.flagUsage`
        - `lombok.nonNull.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.onX.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация onX.
    - **Примеры**:
        - `clear lombok.onX.flagUsage`
        - `lombok.onX.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.requiredArgsConstructor.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @RequiredArgsConstructor.
    - **Примеры**:
        - `clear lombok.requiredArgsConstructor.flagUsage`
        - `lombok.requiredArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.setter.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Setter.
    - **Примеры**:
        - `clear lombok.setter.flagUsage`
        - `lombok.setter.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.singular.auto**
    - **Type**: boolean
    - **Description**: Если true (по умолчанию): автоматически сделать имена переменных/параметров в единственном числе, если используется аннотация @Singular.
    - **Примеры**:
        - `clear lombok.singular.auto`
        - `lombok.singular.auto = [false | true]`
- **Key : lombok.singular.useGuava**
    - **Type**: boolean
    - **Description**: Генерировать поддержку неизменяемых структур данных для аннотации @Singular для типов java.util.* с использованием Guava (по умолчанию: false). Обычно используются изменяемые типы java.util, которые затем оборачиваются для создания неизменяемых структур данных.
    - **Примеры**:
        - `clear lombok.singular.useGuava`
        - `lombok.singular.useGuava = [false | true]`
- **Key : lombok.sneakyThrows.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @SneakyThrows.
    - **Примеры**:
        - `clear lombok.sneakyThrows.flagUsage`
        - `lombok.sneakyThrows.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.standardException.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @StandardException.
    - **Примеры**:
        - `clear lombok.standardException.flagUsage`
        - `lombok.standardException.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.superBuilder.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @SuperBuilder.
    - **Примеры**:
        - `clear lombok.superBuilder.flagUsage`
        - `lombok.superBuilder.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.synchronized.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Synchronized.
    - **Примеры**:
        - `clear lombok.synchronized.flagUsage`
        - `lombok.synchronized.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.toString.callSuper**
    - **Type**: enum (lombok.core.configuration.CallSuperType)
    - **Description**: При генерации метода toString для классов, которые расширяют что-то (кроме Object), автоматически учитывать реализацию суперкласса (call), не учитывать (skip) или выводить предупреждение и не учитывать (warn) (по умолчанию: skip).
    - **Примеры**:
        - `clear lombok.toString.callSuper`
        - `lombok.toString.callSuper = [CALL | SKIP | WARN]`
- **Key : lombok.toString.doNotUseGetters**
    - **Type**: boolean
    - **Description**: Не вызывать геттеры, а использовать поля напрямую при генерации метода toString (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.toString.doNotUseGetters`
        - `lombok.toString.doNotUseGetters = [false | true]`
- **Key : lombok.toString.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @ToString.
    - **Примеры**:
        - `clear lombok.toString.flagUsage`
        - `lombok.toString.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.toString.includeFieldNames**
    - **Type**: boolean
    - **Description**: Включать имена полей в сгенерированный метод toString (по умолчанию: true).
    - **Примеры**:
        - `clear lombok.toString.includeFieldNames`
        - `lombok.toString.includeFieldNames = [false | true]`
- **Key : lombok.toString.onlyExplicitlyIncluded**
    - **Type**: boolean
    - **Description**: Включать только поля/методы, явно отмеченные аннотацией @ToString.Include. В противном случае включаются все нестатические поля с именами, не начинающимися с доллара (по умолчанию: false).
    - **Примеры**:
        - `clear lombok.toString.onlyExplicitlyIncluded`
        - `lombok.toString.onlyExplicitlyIncluded = [false | true]`
- **Key : lombok.utilityClass.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @UtilityClass.
    - **Примеры**:
        - `clear lombok.utilityClass.flagUsage`
        - `lombok.utilityClass.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.val.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется ключевое слово 'val'.
    - **Примеры**:
        - `clear lombok.val.flagUsage`
        - `lombok.val.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.value.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @Value.
    - **Примеры**:
        - `clear lombok.value.flagUsage`
        - `lombok.value.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.var.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется ключевое слово 'var'.
    - **Примеры**:
        - `clear lombok.var.flagUsage`
        - `lombok.var.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.with.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @With.
    - **Примеры**:
        - `clear lombok.with.flagUsage`
        - `lombok.with.flagUsage = [WARNING | ERROR | ALLOW]`
- **Key : lombok.withBy.flagUsage**
    - **Type**: enum (lombok.core.configuration.FlagUsageType)
    - **Description**: Выводить предупреждение или ошибку, если используется аннотация @WithBy.
    - **Примеры**:
        - `clear lombok.withBy.flagUsage`
        - `lombok.withBy.flagUsage = [WARNING | ERROR | ALLOW]`