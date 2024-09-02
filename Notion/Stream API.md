---
modified: 2024-08-31T14:01:45+03:00
---
# ==Анонимные классы==

==В Java анонимные классы имеют доступ только к== ==`final`== ==или== ==`effectively final`== ==переменным из внешнего контекста.==

![[images/Untitled 5.png|Untitled 5.png]]

==Анонимный класс== — это полноценный внутренний класс. ==(Реализация от класса или от интерфейса)== Поэтому у него ==есть доступ к переменным внешнего класса, в том числе к статическим и приватным:==

```Java
public class Main {

   private static int currentErrorsCount = 23;

   public static void main(String[] args) {

       MonitoringSystem errorModule = new MonitoringSystem() {

           @Override
           public void startMonitoring() {
               System.out.println("Мониторинг отслеживания ошибок стартовал!");
           }

           public int getCurrentErrorsCount() {

               return currentErrorsCount;
           }
       };
   }
}
```

Есть у них кое-что ==общее и с локальными классами==: ==они видны только внутри того метода, в котором определены.== В примере выше, любые попытки обратиться к объекту

```Plain
errorModule
```

за пределами метода

```Java
main()
```

будут неудачными.  
  
И еще одно  
==важное ограничение==, которое досталось анонимным классам от их «предков» — внутренних классов:

==**анонимный класс не может содержать статические переменные и методы**==

  

  

# ==Lambda==

![[images/Untitled 1 2.png|Untitled 1 2.png]]

![[images/Untitled 2 2.png|Untitled 2 2.png]]

- `this` - это ==ссылка на объект внешнего класса== (то есть, ==где объявлен лямбда, тот класс и будет==)
- ==Потому что в байт-коде== ==лямбда== ==представлена== ==методом====.==

![[images/Untitled 3 2.png|Untitled 3 2.png]]

  

# ==Функциональный интерфейс==

![[images/Untitled 4 2.png|Untitled 4 2.png]]

![[images/Untitled 5 2.png|Untitled 5 2.png]]

- `Неявно подставляется конкретный тип в первый параметр`

![[Untitled 6.png]]

- ==Не статический метод любого класса==

![[Untitled 7.png]]

![[Untitled 8.png]]

![[Untitled 9.png]]

![[Untitled 10.png]]

  

  

  

  

  

# ==Stream API==

==Хорошая статья для повторения:== ==[https://annimon.com/article/2778](https://annimon.com/article/2778)==

`<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)`>`

Методы делятся на:

- ==промежуточные== (”intermediate” , еще называют “lazy”) ==- обрабатывают поступающием элементы и возвращают стрим. Промежуточных в цепочке== ==может быть много.==
- ==терминальные== (”terminal”, еще называют “eager”) - ==обрабатывают элементы и заавершают работу стрима, так что терминальный метод в цепочке== ==только один.== ==Обработка не начнется пока не будет вызван терминальный метод!!!==

  

`Экземпляр стрима нельзя использовать более одного раза.`

Потоки не хранят элементы.

Операции с потоками не изменяют источника данных.

В основу использования стримов лежат функциональные интерфейсы

`Функциональный интерфейс` - это интерфейс, который имеет только один абстрактный метод. Он может содержать любое количество методов по умолчанию и статических методов, но только один метод без реализации. Функциональные интерфейсы часто используются для передачи лямбда-выражений или методов как параметров методов.

  

```Java
@FunctionalInterface
interface MyFunction {
    int apply(int x, int y); // функциональный метод

default void writeToCOnsole(T t){
System.out.println("Текущий объект" + " " + t.toString()); //дефолтный
}

static boolean isNotNull(T t){
return t != null;  // статический
}
```

## В основе Stream API лежит интерфейс BaseStream, AutoCloseable

```Java
interface BaseStream<T, S extends BaseStream<T,S>>
параметр T означает тип данных в потоке, а S - тип потока
```

От BaseStream наследуются ряд интерфейсов, Stream\<T>, IntStream, DoubleStream, LongStream

  

Методы BaseStream [`close`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#close--)`,` [`isParallel`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#isParallel--)`,` [`iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#iterator--)`,` [`onClose`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#onClose-java.lang.Runnable-)`,` [`parallel`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#parallel--)`,` [`sequential`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#sequential--)`,` [`spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#spliterator--)`,` [`unordered`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#unordered--)

  

```Java
isParallel(): возвращает true, если поток является параллельным
S parallel(): возвращает паралельный поток // parallelStream()

BaseStream baseStream = list.stream().parallel();

Iterator<T> iterator(): возвращает ссылку на итератор потока
Iterator baseStream = list.stream().iterator();

Spliterator<T> spliterator(): возвращает ссылку на сплитератор потока

S sequential(): возвращает последовательный поток
S unordered(): возвращает неупорядоченный поток
close(): закрывает поток
```

## Функциональные интерфейсы

Stream API в Java основан на использовании функциональных интерфейсов, которые определены в пакете `java.util.function`.

Ниже приведены некоторые из наиболее распространенных функциональных интерфейсов, используемых в Stream API:

- `Predicate<T>` - принимает один аргумент типа `T` и возвращает `boolean`. Используется для проверки условия на элементах в потоке.

```Java
@FunctionInterface
public interface Predicate<T>{
	boolean test(T t);
}

Predicate<Integer> isEvenNumber = x-> x % 2 ==0;
System.out.println(isEvenNumber.test(4));
```

- `Function<T, R>` - принимает один аргумент типа `T` и возвращает результат типа `R`. Используется для преобразования элементов в потоке из одного типа в другой.

```Java
@FunctionalInterface
public interface Function<T,R>{
R apply(T t);
}

Function<String, Integer> value = x -> Integer.valueOf(x);
System.out.println(value.apply("678");
```

- `Consumer<T>` - принимает один аргумент типа `T` и не возвращает результат. Используется для выполнения действий над элементами в потоке.

```Java
@FunctionalInterface
public interface Comsumer<T> {
void accept(T t);
}

Consumer<String> greetings = x-> sout("Hello" + x + "!");
greetings.accept("Elena");
```

- `Supplier<T>` - не принимает аргументов и возвращает результат типа `T`. Используется для создания элементов, которые будут переданы в поток.

```Java
@FinctionalInterface Supplier<T>{
T get();
}

Supplier<String> randomName = () -> {
int value = (int) (Math.random() * nameList.size());
return nameList.get(value);
};
```

- `Comparator<T>` - принимает два аргумента типа `T` и возвращает целочисленное значение. Используется для сравнения элементов в потоке.

```Java
@FunctionalInterface
public interface Comparator<T> {
int compare(T o1, T o2);}

Comparator<Integer> doubleComparator = (s1,s2)->  Double.compare(s1,s2);
  doubleComparator.compare(2,3);
```

- Конструкция `interface UnaryOperator<T> extends Function<T, T>` означает, что интерфейс `UnaryOperator` является наследником интерфейса `Function` с одним типом `T` и реализует метод `apply()`, принимающий объект типа `T` и возвращающий объект того же типа `T`. Таким образом, `UnaryOperator` представляет функциональный интерфейс, принимающий один аргумент и возвращающий результат того же типа.

```Java
public interface UnaryOperator<T> extends Function<T, T> {

    static <T> UnaryOperator<T> identity() {
        return t -> t;
    }
}

UnaryOperator<Integer> square = x -> x * x;
System.out.println(square.apply(5)); // output: 25
```

## Optional\<T>

Optional\<T> - это контейнер, который может содержать один объект определенного типа T или же не содержать ничего (null). Это позволяет избежать NullPointerException в случаях, когда метод может вернуть null. Optional\<T> был введен в Java 8 для облегчения работы с null значениями и написания более безопасного и читаемого кода.

### Методы

- `static <T>` [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<T>` [`empty`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#empty--)`()` возвращает пустой Optional

```Java
Optional<String> optional = Optional.empty();
```

- `boolean` [`equals`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#equals-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `obj)` сравнивает Optional

```Java
Optional<String> optional = Optional.of("Hello");
        Optional<String> optional2 = Optional.of("Hello");
        System.out.println(optional2.equals(optional));  //true
```

- [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<`[`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`>` [`filter`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#filter-java.util.function.Predicate-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`> predicate)` возвращает Optional, содержащийся в Optional, если он соответствует заданному условию, или пустой Optional, если он не соответствует.

```Java
Optional<String> optionalString = Optional.of("Hello");
Optional<String> filteredOptional = optionalString.filter(s -> s.length() > 5);

	sout(filteredOptional.get());  //java.util.NoSuchElementException:
																 //No value present
```

- [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) [`get`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#get--)`()` возвращает значения, содержащееся в Optional, бросает NoSuchElementException если нет внутри экземпляра

```Java
Optional<String> optional = Optional.of("Hello");
        System.out.println(optional.get()); // Hello

        Optional<String> optional2 = Optional.empty();
        System.out.println(optional2.get()); //NoSuchElementException: No value present
```

- `int` [`hashCode`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#hashCode--)`()` возвращает hashCode

```Java
Optional<String> optional = Optional.of("Hello");
        System.out.println(optional.hashCode());
```

- `void` [`ifPresent`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#ifPresent-java.util.function.Consumer-)`(`[`Consumer`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`> consumer)` вызывает переданный в метод Consumer, если значение присутствует;

```Java
Optional<String> optional = Optional.of("value");
optional.ifPresent(System.out::println); // выведет "value"
Optional<String> emptyOptional = Optional.empty();
emptyOptional.ifPresent(System.out::println); // ничего не выведет
```

- `boolean` [`isPresent`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#isPresent--)`()` возвращает булево значение пустой Optional или нет

```Java
Optional<String> optional = Optional.of("Hello");
        Optional<String> optionalEmpty = Optional.empty();
        System.out.println(optional.isPresent()); //true
        System.out.println(optionalEmpty.isPresent()); //false
```

- `<U>` [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<U>` [`map`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#map-java.util.function.Function-)`(`[`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`,? extends U> mapper)` преобразует в другой тип Optional, не изменяет старый

```Java
Optional<String> optional = Optional.of("123");
        Optional<Integer> optional2 = optional.map(Integer::valueOf);
        System.out.println(optional.get()+1); //"1231"
        System.out.println(optional2.get()+1); //124
```

- `<U>` [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<U>` [`flatMap`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#flatMap-java.util.function.Function-)`(`[`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`,`[`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<U>> mapper)`

Как и метод `map`, метод `flatMap` принимает функциональный интерфейс, который преобразует значение в объекте `Optional`. Однако, в отличие от метода `map`, функциональный интерфейс, переданный в `flatMap`, должен сам возвращать объект `Optional`.

Таким образом, если в результате выполнения функционального интерфейса возвращается пустой объект `Optional`, то метод `flatMap` также возвращает пустой объект `Optional`. Если же функциональный интерфейс возвращает непустой объект `Optional`, то метод `flatMap` возвращает этот объект, содержащий преобразованное значение.

```Java
Optional<String> optionalStr = Optional.of("hello");
Optional<String> result = optionalStr.flatMap(str -> Optional.of(str.toUpperCase()));
System.out.println(result); // Optional[HELLO]
```

- `static <T>` [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<T>` [`of`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#of-T-)`(T value)` возвращает Optional содержащий T

```Java
Optional<String> optional = Optional.of("Hello!");
```

- `static <T>` [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<T>` [`ofNullable`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#ofNullable-T-)`(T value)` возвращает объект `Optional` с заданным значением, если значение не является `null`, или возвращает пустой объект `Optional`, если значение `null`.

```Java
Optional<Student> optionalStudent = Optional
.ofNullable(findStudentById(studentId));

if (optionalStudent.isEmpty()) {
    // студент не найден
} else {
    Student student = optionalStudent.get();
    // делаем что-то с найденным студентом
}
```

- [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) [`orElse`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#orElse-T-)`(`[`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) `other)` возвращает значение, если оно присутствует, иначе возвращает значение по умолчанию, переданное в метод;

```Java
Optional<String> optional = Optional.of("value");
String result = optional.orElse("default");
System.out.println(result); // выведет "value"
Optional<String> emptyOptional = Optional.empty();
String emptyResult = emptyOptional.orElse("default");
System.out.println(emptyResult); // выведет "default"
```

- [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) [`orElseGet`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#orElseGet-java.util.function.Supplier-)`(`[`Supplier`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)`<? extends` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`> other)` возвращает значение, если оно присутствует, иначе вызывает переданный в метод Supplier, чтобы получить значение по умолчанию;

```Java
Optional<String> optional = Optional.empty();
String result = optional.orElseGet(() -> "default");
System.out.println(result); // выведет "default"
```

- `<X extends` [`Throwable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html)`>` [`orElseThrow`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#orElseThrow-java.util.function.Supplier-)`(`[`Supplier`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)`<? extends X> exceptionSupplier)` возвращает значение, если оно присутствует, иначе выбрасывает исключение, полученное из Supplier;

```Java
Optional<String> optional = Optional.empty();
try {
    String result = optional.orElseThrow(() -> new Exception("Value is not present"));
} catch (Exception e) {
    System.out.println(e.getMessage()); // выведет "Value is not present"
}
```

## Список основных методов

### lazy methods

Lazy methods (ленивое вычисление):

- `distinct()`: возвращает стрим, содержащий только уникальные элементы.

```Java
int [] array = {1,1,2,2,3,3,4,4,5,5,5,5};
        Arrays.stream(array).distinct().forEach(System.out::print);
// вывод 12345
```

- `filter(Predicate<T> predicate)`: возвращает стрим, содержащий только элементы, удовлетворяющие условию `predicate`.

```Java
employees.stream().filter(e->e.getSalary>50000).forEach();
```

- `flatMap(Function<T,Stream<R>> mapper)`: преобразует каждый элемент стрима в стрим и возвращает конкатенацию всех полученных стримов.

```Java
List<String> list1 = Arrays.asList("java", "python", "ruby");
        List<String> list2 = Arrays.asList("c++", "c#", "go");
        List<String> list3 = Arrays.asList("scala", "kotlin", "swift");

        List<String> combinedList = Arrays.asList(list1, list2, list3)
                .stream()
                .flatMap(List::stream)
                .collect(Collectors.toList());
```

- `mapMulti(BiConsumer<T, Consumer<R>> mapper)`

Появился в Java 16. Этот оператор похож на flatMap, но использует императивный  
подход при работе. Теперь вместе с элементом стрима приходит ещё и Consumer, в который можно передать одно или несколько значений, либо не передавать вовсе.  

У mapMulti есть несколько преимуществ перед flatMap. Во-первых, если приходится пропускать значения (как в примере выше, где пропускались нечётные элементы), то не будет затрат на создание пустого стрима. Во-вторых, consumer легко передать в другой метод, в котором можно проводить преобразования, включая рекурсивные.

```Java
void processSerializable(Serializable ser, Consumer<String> consumer) {
        if (ser instanceof String str) {
            consumer.accept(str);
        } else if (ser instanceof List) {
            for (Serializable s : (List<Serializable>) ser) {
                processSerializable(s, consumer);
            }
        }
    }
     
    Serializable arr(Serializable... elements) {
        return Arrays.asList(elements);
    }
     
    Stream.of(arr("A", "B"), "C", "D", arr(arr("E"), "F"), "G")
        .mapMulti(this::processSerializable)
        .forEach(System.out::println);
    // A, B, C, D, E, F, G
```

- `limit(long maxSize)`: возвращает стрим, содержащий не более `maxSize` элементов.

```Java
employees.stream().limit(2).forEach(...)
```

- `map(Function<T,R> mapper)`: применяет функцию `mapper` ко всем элементам стрима и возвращает стрим, содержащий результаты применения функции.

```Java
List<String> list = new ArrayList<>();
        list.add("privet");
        list.add("hello");

        List<Integer> newListLength = list.stream().map(String::length)
                .collect(Collectors.toList());

// вместо слов, теперь содержится длина
```

- `peek(Consumer<T> action)`: выполняет заданное действие `action` для каждого элемента стрима и возвращает тот же стрим. (для просмотра промежуточного результата)

```Java
int [] array = {1,2,3,4,5,6,7,8,9,10};
        Arrays.stream(array)
.skip(5)
.peek(x->System.out.println("Number that is not skipped" +x))
.filter(x->x % 2 ==0)
.forEach(System.out::println);


//Вывод
Number that is not skipped6
6
Number that is not skipped7
Number that is not skipped8
8
Number that is not skipped9
Number that is not skipped10
10
```

- `skip(long n)`: возвращает стрим, пропускающий первые `n` элементов.

```Java
int [] array = {1,2,3,4,5,6,7,8,9,10};
        Arrays.stream(array).skip(5).forEach(System.out::print); //678910
```

### eager methods

Eager methods (немедленно вычисляются):

- `allMatch(Predicate<T> predicate)`: возвращает `true`, если все элементы стрима удовлетворяют условию `predicate`, иначе `false`.

```Java
Boolean isAllSalaryGreaterThan5000= employees.stream()
.allMatch(e->e.getSalary()>15000); // true
```

- `anyMatch(Predicate<T> predicate)`: возвращает `true`, если хотя бы один элемент стрима удовлетворяет условию `predicate`, иначе `false`.

```Java
Boolean isSalaryGreaterThan5000= employees.stream()
.anyMatch(e->e.getSalary()>150000);  //false
```

- [`noneMatch`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#noneMatch-java.util.function.Predicate-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)`> predicate)`

```Java
Boolean allNamesAreNotOleg= employees.stream()
.noneMatch(e->e.getFirstName().equals("Олег")); //false, Олег есть
```

- `collect(Collector<T,A,R> collector)`: применяет коллектор для преобразования элементов стрима в один объект типа `R`.

```Java
employees.stream.collect(Collectors.toList();
```

- `count()`: возвращает количество элементов в стриме.

```Java
Long count= employees.stream().count();
```

- `findFirst()`: возвращает первый элемент стрима или `null`, если стрим пуст.

```Java
String nameWithLowSalary= employees
.stream()
.sorted(Comparator.comparing(Employee::getSalary))
.map(Employee::getFirstName)
.findFirst()
.get();
        System.out.println(nameWithLowSalary); // Иван
```

- [`findAny`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#findAny--)`()` возвращает Optional

```Java
Employee employee = employees.stream().findAny().get();
```

- `forEach(Consumer<T> action)`: выполняет заданное действие `action` для каждого элемента стрима.

```Java
employees.stream()
.map(Employee::getFirstName)
.forEach(System.out::println);
```

- `forEachOrdered(Consumer<T> action)`: выполняет заданное действие `action` для каждого элемента стрима в порядке их следования.

```Java
employees.parallelStream()
.map(Employee::getFirstName)
.forEachOrdered(System.out::println);
```

- `max(Comparator<T> comparator)`: возвращает максимальный элемент в стриме в соответствии с заданным компаратором `comparator`.

```Java
Optional<Employee> value = employees
.stream()
.max(Comparator.comparing(Employee::getSalary));
```

- `min(Comparator<T> comparator)`: возвращает минимальный элемент в стриме в соответствии с заданным компаратором `comparator`.

```Java

List<Integer> numbers = Arrays.asList(4, 2, 6, 1, 8, 3);
Optional<Integer> min = numbers.stream()
.min(Integer::compareTo);

//salary
Optional<Employee> value = employees.stream()
.min(Comparator.comparing(Employee::getSalary));
```

- `reduce(T identity, BinaryOperator<T> accumulator)`: выполняет бинарную операцию `accumulator` для каждого элемента стрима, начиная с `identity`.

```Java
double totalSalary = employees.stream()
    .mapToDouble(Employee::getSalary)
    .reduce(0.0, Double::sum);
```

- `toArray()`: возвращает массив элементов стрима.]

```Java
Employee[] array = employees
.stream()
.sorted((o1, o2) -> (int) (o1.getSalary()-o2.getSalary()))
.toArray(Employee[]::new);
```

- `List<T> toList()`Наконец-то добавлен в Java 16. Возвращает список, подобно collect(Collectors.toList()). Отличие в том, что теперь возвращаемый список гарантированно нельзя будет модифицировать. Любое добавление или удаление элементов в полученный список будет сопровождаться исключением UnsupportedOperationException.

### default static methods

- [`generate`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#generate-java.util.function.Supplier-)`(`[`Supplier`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)`<T> s)` Returns an infinite sequential unordered stream where each element is generated by the provided `Supplier`.

```Java
Stream.generate(Math::random).limit(3).forEach();
```

- static[`concat`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#concat-java.util.stream.Stream-java.util.stream.Stream-)`(`[`Stream`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)`<? extends T> a,`[`Stream`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)`<? extends T> b)` конкатенирует потоки

```Java
Stream<Integer> stream1 = Stream.of(1, 2, 3);
Stream<Integer> stream2 = Stream.of(4, 5, 6);
Stream<Integer> concatenatedStream = Stream.concat(stream1, stream2);
concatenatedStream.forEach(System.out::println); // выводит 1, 2, 3, 4, 5, 6
```

- static[`of`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#of-T...-)`(T... values)` or static [`of`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#of-T-)`(T t)` Returns a sequential ordered stream whose elements are the specified values. or Returns a sequential `Stream` containing a single element.

```Java
Stream.of(1,2,3,4,5);
```

- static\<T> Stream\<T> ofNullable(T t) возвращает пустой стрим, если в качестве аргумента передан null, в противном случае, возвращает стрим из одного элемента

```Java
Stream.ofNullable(str).forEach();
```

  

```Java

list.stream() стрим из List
map.entrySet().stream() стрим из Map
Arrays.stream(array) стрим из массива
Stream.of("1","2","3") стрим из указанных элементов
```

- `sorted()`: возвращает стрим, содержащий элементы, отсортированные по умолчанию.

```Java
Employee[] array = employees
.stream()
.sorted((o1, o2) -> (int) (o1.getSalary()-o2.getSalary()))
.toArray(Employee[]::new);
```

- [`empty`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#empty--)`()`Returns an empty sequential `Stream`.

```Java
Stream.empty() пустой стрим
```

- [`builder`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#builder--)`()` Returns a builder for a `Stream`.

```Java
Stream.builder()...
```

## Collectors

Все методы данного класса static.

Используемый класс для разбора методов:

```Java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Employee {
    private String firstName;
    private String lastName;
    private String department;
    private double salary;
}

List<Employee> employees = new ArrayList<>();

new Employee("Иван", "Иванов", "SL", 50000);
new Employee("Петр", "Петров", "IT", 80000);
new Employee("Сергей", "Сергеев", "PR", 60000);
new Employee("Андрей", "Андреев", "SL", 55000);
new Employee("Олег", "Олегов", "PR", 75000);
new Employee("Дмитрий", "Дмитриев", "IT", 65000);
```

- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)`>` [`averagingDouble`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#averagingDouble-java.util.function.ToDoubleFunction-)`(`[`ToDoubleFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToDoubleFunction.html)`<? super T> mapper)`,
- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)`>` [`averagingInt`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#averagingInt-java.util.function.ToIntFunction-)`(`[`ToIntFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToIntFunction.html)`<? super T> mapper)`
- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)`>` [`averagingLong`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#averagingLong-java.util.function.ToLongFunction-)`(`[`ToLongFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToLongFunction.html)`<? super T> mapper)`

```Java
Double value = employees.stream()
                .collect(Collectors.averagingDouble(Employee::getSalary));
        System.out.println(value); //64166.666666666664
```

- `<T,A,R,RR>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,A,RR>` [`collectingAndThen`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#collectingAndThen-java.util.stream.Collector-java.util.function.Function-)`(`[`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,A,R> downstream,` [`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<R,RR> finisher)` возвращает коллектор, который сначала применяет указанный коллектор для сбора элементов, а затем применяет функцию к результату.

```Java
Employee[] array = employees.stream()
                .collect(Collectors.collectingAndThen(
                        Collectors.toCollection(HashSet::new),
                        set -> set.toArray(new Employee[0])));
```

- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Long`](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html)`>` [`counting`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#counting--)`()` возвращает коллектор, который подсчитывает количество элементов в стриме и возвращает результат в виде `Long`.

```Java
Long countEmployees = employees.stream().collect(Collectors.counting());
        System.out.println(countEmployees);  //6
```

- `<T,K>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,`[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T>>>` [`groupingBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingBy-java.util.function.Function-)`(`[`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<? super T,? extends K> classifier)` возвращает коллектор, который группирует элементы стрима по заданному классификатору и сохраняет результат в виде `Map<K,List<T>>`, где `K` - тип ключа, а `T` - тип элементов стрима.

```Java
Map<String, List<Employee>> map = employees
                .stream()
                .collect(Collectors.groupingBy(Employee::getDepartment));
        map.entrySet().forEach(System.out::println);

PR=[Employee(firstName=Сергей, lastName=Сергеев, department=PR, salary=60000.0), Employee(firstName=Олег, lastName=Олегов, department=PR, salary=75000.0)]
SL=[Employee(firstName=Иван, lastName=Иванов, department=SL, salary=50000.0), Employee(firstName=Андрей, lastName=Андреев, department=SL, salary=55000.0)]
IT=[Employee(firstName=Петр, lastName=Петров, department=IT, salary=80000.0), Employee(firstName=Дмитрий, lastName=Дмитриев, department=IT, salary=65000.0)]
```

- `<T,K,A,D>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,D>>` [`groupingBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingBy-java.util.function.Function-java.util.stream.Collector-)`(`[`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<? super T,? extends K> classifier,`[`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<? super T,A,D> downstream)`

Метод `groupingBy` с двумя аргументами также группирует элементы стрима в Map, но в качестве значения каждой группы использует результат применения коллектора `downstream` к элементам данной группы.

Значения Map в данном случае имеют тип D, который является результатом работы коллектора `downstream`. Это позволяет применять несколько коллекторов к элементам группировки.

Например, если мы хотим группировать список работников по департаментам и вычислять среднюю зарплату каждого департамента, мы можем использовать `groupingBy` для группировки по департаментам, а затем применить коллектор `averagingDouble` для вычисления средней зарплаты:

```Java
Map<String, Double> avgSalaryByDept = employees.stream()
        .collect(Collectors
.groupingBy(Employee::getDepartment,
 Collectors.averagingDouble(Employee::getSalary)));
```

Аналогичные методы с [`groupingByConcurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingByConcurrent-java.util.function.Function-)() возвращающие concurrent коллекции

- [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<`[`CharSequence`](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html)`,?,`[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)`>` [`joining`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining--)`(),` [`joining`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining-java.lang.CharSequence-)`(`[`CharSequence`](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html) `delimiter),` [`joining`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining-java.lang.CharSequence-java.lang.CharSequence-java.lang.CharSequence-)`(`[`CharSequence`](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html) `delimiter,` [`CharSequence`](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html) `prefix,` [`CharSequence`](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html) `suffix)`

```Java
String names = employees.stream()
                        .map(Employee::getName)
                        .map(String::toUpperCase)
                        .collect(Collectors.joining(", "));

String result = employees.stream()
    .map(e -> String.format("%s %s", e.getFirstName(), e.getLastName()))
    .collect(Collectors.joining(", "));
```

- `<T,U,A,R>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,R>` [`mapping`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#mapping-java.util.function.Function-java.util.stream.Collector-)`(`[`Function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)`<? super T,? extends U> mapper,` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<? super U,A,R> downstream)` Метод `mapping()` из класса `Collectors` позволяет собирать элементы потока, преобразовывая каждый элемент в новый тип данных, а затем собирая результаты в коллекцию.

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Map<Integer, List<String>> map = numbers.stream()
    .collect(Collectors.groupingBy(
        n -> n % 2,
        Collectors.mapping(
            n -> n.toString(),
            Collectors.toList()
        )
    ));
System.out.println(map);
// Вывод: {0=[2, 4], 1=[1, 3, 5]}
```

- возвращающие `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<T>>` методы [`maxBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#maxBy-java.util.Comparator-)`(`[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)`<? super T> comparator)` и [`minBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#minBy-java.util.Comparator-)`(`[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)`<? super T> comparator)`

```Java
Optional<Employee> maxSalaryEmployee = employees.stream()
    .collect(Collectors.maxBy(Comparator.comparing(Employee::getSalary)));

Employee(firstName=Петр, lastName=Петров, department=IT, salary=80000.0)

Optional<Employee> maxSalaryEmployee = employees.stream()
    .collect(Collectors.minBy(Comparator.comparing(Employee::getSalary)));

Employee(firstName=Иван, lastName=Иванов, department=SL, salary=50000.0)
```

  

- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<`[`Boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html)`,`[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T>>>` [`partitioningBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#partitioningBy-java.util.function.Predicate-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super T> predicate)` Метод `partitioningBy` в классе `Collectors`  
    возвращает коллектор, который разделяет элементы стрима на две группы в  
    соответствии с заданным условием (предикатом) и помещает их в мапу типа  
    `Map<Boolean, List<T>>`, где ключ `true` содержит элементы, для которых предикат вернул `true`, а ключ `false` содержит элементы, для которых предикат вернул `false`. Например, мы можем разделить список чисел на четные и нечетные числа:

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

Map<Boolean, List<Integer>> isEven = numbers.stream()
        .collect(Collectors.partitioningBy(n -> n % 2 == 0));

// вывод {false=[1, 3, 5, 7, 9], true=[2, 4, 6, 8, 10]}
```

- `<T,D,A>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<`[`Boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html)`,D>>` [`partitioningBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#partitioningBy-java.util.function.Predicate-java.util.stream.Collector-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super T> predicate,` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<? super T,A,D> downstream)` Существует также перегруженный метод `partitioningBy`, который принимает дополнительный аргумент `downstream`, являющийся `Collector'ом`  
    для элементов, которые попали в каждую из двух групп. Таким образом, мы  
    можем сначала выполнить какое-то преобразование для каждого элемента, а  
    затем разделить их на две группы на основе результата этого  
    преобразования.  
    

В данном примере каждый элемент списка умножается на 2, а затем  
разделяется на две группы на основе того, является ли он четным или  
нечетным. Полученный результат - это  
`Map<Boolean, List<Integer>>`, где каждому ключу соответствует список элементов, удовлетворяющих заданному условию:

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
Map<Boolean, List<Integer>> partitionedMap = numbers.stream()
        .collect(Collectors
.partitioningBy(n -> n % 2 == 0,
 Collectors.mapping(n -> n * 2, Collectors.toList())));

System.out.println(partitionedMap); // Output: {false=[2, 6, 10, 14, 18], true=[4, 8, 12, 16, 20]}
```

- `<T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)`<T>>` [`reducing`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#reducing-java.util.function.BinaryOperator-)`(`[`BinaryOperator`](https://docs.oracle.com/javase/8/docs/api/java/util/function/BinaryOperator.html)`<T> op)` возвращает `Collector`, который сводит все элементы потока к единственному значению, используя заданную функцию сведения.

```Java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
Optional<Integer> result = stream.collect(Collectors
					.reducing(BinaryOperator.maxBy(Integer::compare)));


//Второй вариант используется, когда нужно сведение
// всех элементов потока к единственному значению
// с помощью заданной функции сведения. Например, 
//чтобы найти сумму всех элементов потока из Integer:
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
Integer sum = stream.collect(Collectors.reducing(0, (a, b) -> a + b));
```

  

|   |   |
|---|---|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`DoubleSummaryStatistics`](https://docs.oracle.com/javase/8/docs/api/java/util/DoubleSummaryStatistics.html)`>`|[`summarizingDouble`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summarizingDouble-java.util.function.ToDoubleFunction-)`(`[`ToDoubleFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToDoubleFunction.html)`<? super T> mapper)`Returns a `Collector` which applies an `double`-producing  <br>mapping function to each input element, and returns summary statistics  <br>for the resulting values.|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`IntSummaryStatistics`](https://docs.oracle.com/javase/8/docs/api/java/util/IntSummaryStatistics.html)`>`|[`summarizingInt`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summarizingInt-java.util.function.ToIntFunction-)`(`[`ToIntFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToIntFunction.html)`<? super T> mapper)`Returns a `Collector` which applies an `int`-producing  <br>mapping function to each input element, and returns summary statistics  <br>for the resulting values.|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`LongSummaryStatistics`](https://docs.oracle.com/javase/8/docs/api/java/util/LongSummaryStatistics.html)`>`|[`summarizingLong`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summarizingLong-java.util.function.ToLongFunction-)`(`[`ToLongFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToLongFunction.html)`<? super T> mapper)`Returns a `Collector` which applies an `long`-producing  <br>mapping function to each input element, and returns summary statistics  <br>for the resulting values.|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)`>`|[`summingDouble`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summingDouble-java.util.function.ToDoubleFunction-)`(`[`ToDoubleFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToDoubleFunction.html)`<? super T> mapper)`Returns a `Collector` that produces the sum of a double-valued  <br>function applied to the input elements.|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)`>`|[`summingInt`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summingInt-java.util.function.ToIntFunction-)`(`[`ToIntFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToIntFunction.html)`<? super T> mapper)`Returns a `Collector` that produces the sum of a integer-valued  <br>function applied to the input elements.|
|`static <T>` [`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)`<T,?,`[`Long`](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html)`>`|[`summingLong`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summingLong-java.util.function.ToLongFunction-)`(`[`ToLongFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/ToLongFunction.html)`<? super T> mapper)`Returns a `Collector` that produces the sum of a long-valued  <br>function applied to the input elements.|

```Java
double totalSalary = employees.stream()
    .collect(Collectors.summingDouble(Employee::getSalary));


DoubleSummaryStatistics salaryStats = employees.stream()
    .collect(Collectors.summarizingDouble(Employee::getSalary));
double maxSalary = salaryStats.getMax();
double minSalary = salaryStats.getMin();
double averageSalary = salaryStats.getAverage();
long count = salaryStats.getCount();
double totalSalary = salaryStats.getSum();
```

Метод `summarizing` создает коллектор, который вычисляет  
некоторую статистику (минимальное, максимальное, среднее значения, сумму  
и количество элементов) для числовых значений элементов стрима.  

- методы [`toList`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toList--)`()`, [`toMap`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toMap-java.util.function.Function-java.util.function.Function-)`(),` [`toSet`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toSet--)`()`, [`toConcurrentMa`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toConcurrentMap-java.util.function.Function-java.util.function.Function-)`p(),` [`toCollection`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toCollection-java.util.function.Supplier-)`(`[`Supplier`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)`<C> collectionFactory)`

```Java
//toSet
Set<Employee> set = employees
                .stream()
                .filter(e-> e.getDepartment().equals("IT"))
                .collect(Collectors.toSet());
        set.forEach(System.out::println); 

//toList

List<Employee> list = employees
                .stream()
                .filter(e-> e.getDepartment().equals("IT"))
                .collect(Collectors.toList());
        set.forEach(System.out::println);

//toMap

Map<String, Double> map = employees
                .stream()
                .filter(e-> e.getDepartment().equals("IT"))
                .collect(Collectors.toMap(Employee::getFirstName,Employee::getSalary));
        map.entrySet().forEach(System.out::println);

Петр=80000.0
Дмитрий=65000.0


//toCollection
HashSet<Employee> set = employees.stream()
               .filter(e->e.getSalary()>70000).
               collect(Collectors.toCollection(HashSet::new));
```

# ParallelStream

## ForkJoinPool

  

Количество потоков в общем пуле (количество ядер процессора - 1)

Разделяет исходные данные между рабочими потоками и обрабатывает обратный вызов при завершении задачи.

Можно запустить параллельный поток в кастомном пуле потоков

```Java
List<Integer> listOfNumbers = List.of(1, 2, 3, 4, 5);
ForkJoinPool customPool = ForkJoinPool.commonPool();

int sum = customPool.submit(() -> listOfNumbers
                        .parallelStream()
                        .reduce(0, Integer::sum))
				                .get();
customPool.shutdown();
System.out.println(sum); //15
```

  

## Модель NQ

N- количество элементов исходных данных

Q- представляет объем вычислений для каждого элемента

Чем больше произведение NQ, тем больше вероятность того, что мы получим прирост производительности от использования параллельных потоков.

  

Минусы:

Затраты на поиск, разделение и слияние

Чем проще структура данных, с которой работают потоки, тем быстрее будут происходить операции.

Например использовать ArrayList легко, так как структура данной коллекции предполагает последовательность несвязанных между собой данных.

Обратная ситуация с LinkedList, так как данные связаны нодами.

Чем больше указателей у нас есть в нашей структуре, тем большую нагрузку мы оказываем на память.

## Упорядоченность в параллельных потоках

```Java
List<Integer> listOfNumbers = List.of(1, 2, 3, 4, 5);
        listOfNumbers
.stream()
.parallel()
.forEach(System.out::print); //34152
        listOfNumbers
.stream()
.parallel()
.forEachOrdered(System.out::print); //12345
```