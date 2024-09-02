---
modified: 2024-09-02T12:48:26+03:00
---
# Interface Collection

[https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)

![[images/Untitled 11.png|Untitled 11.png]]

Родитель интерфейс - Iterable\<E>

Не работают с примитивными типами, только с классами обертками.

Для правильного поиска объектов в коллекциях необходимо переопределять equals и hashcode.

Для правильной сортировки объектов необходимо, чтобы класс имплементировал Comparable, вызывая метод сортировки, можно передавать в аргументы Comparator

  

All Known Subinterfaces:
[BeanContext](https://docs.oracle.com/javase/8/docs/api/java/beans/beancontext/BeanContext.html)
[BeanContextServices](https://docs.oracle.com/javase/8/docs/api/java/beans/beancontext/BeanContextServices.html), 
[BlockingDeque](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html)\<E>, [BlockingQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)\<E>, [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html\)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)\<E>, [NavigableSet](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)\<E>, [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)\<E>, [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)\<E>, [SortedSet](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)\<E>, [TransferQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TransferQueue.html)\<E>

![[images/Untitled 1 3.png|Untitled 1 3.png]]

## Interface Iterable

[https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)

является корневым для всех коллекций в Java. Он объявляет один метод, который должны реализовывать все его подклассы.

Позволяет итерироваться по коллекции с помощью for-Each.

- [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`<`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`>` [`iterator`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#iterator--)`()` возвращает итератор, который можно использовать для последовательного перебора элементов коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String str = it.next();
    System.out.println(str);
}
```

- `default void` [`forEach`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#forEach-java.util.function.Consumer-)`(`[`Consumer`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`> action)` метод выполняет указанное действие для каждого элемента коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

list.forEach(str -> System.out.println(str));
```

- `default` [`Spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)`<`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`>` [`spliterator`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#spliterator--)`()` метод возвращает `Spliterator`, который можно использовать для параллельного перебора элементов коллекции. `Spliterator` является абстракцией для параллельного перебора элементов коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

Spliterator<String> spliterator = list.spliterator();
spliterator.trySplit().forEachRemaining(str -> System.out.println(str));
```

## Interface Iterator

**Преимущества**.

- Вы можете использовать эти итераторы для любого класса Collection.
- Поддерживает операции чтения и удаления.
- Если вы используете цикл for, вы не можете обновить (добавить / удалить) коллекцию, тогда как с помощью итератора вы можете легко обновить коллекцию.
- Универсальный для API коллекции.
- Удаление с помощью итератора в LinkedList происходит за const

**Недостатки:**

- Вы можете выполнять только итерацию в прямом направлении, то есть однонаправленный итератор.
- Замена и добавление нового элемента не поддерживается.
- ListIterator – самый мощный, но он применим только для классов, реализованных в List.
- Когда вы используете операции CRUD, он не поддерживает операции создания и обновления.
- Когда вы сравниваете его со Spliterator, он не позволяет выполнять итерацию элементов параллельно. Это означает, что он поддерживает только последовательную итерацию.
- Он не поддерживает лучшую производительность для итерации большого объема данных.

### Методы

- `default void` [`forEachRemaining`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#forEachRemaining-java.util.function.Consumer-)`(`[`Consumer`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`> action)` позволяет применить заданный метод к каждому элементу коллекции, оставшемуся после текущей позиции итератора.

```Java
List<String> list = new ArrayList<>();
list.add("one");
list.add("two");
list.add("three");

Iterator<String> iterator = list.iterator();
iterator.next(); // пропускаем первый элемент

iterator.forEachRemaining(System.out::println); // выводим оставшиеся элементы
```

- `boolean` [`hasNext`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#hasNext--)`()` возвращает `true`, если в коллекции есть следующий элемент, и `false`, если элементов больше нет.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html) [`next`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#next--)`()`возвращает следующий элемент коллекции. Если в коллекции больше нет элементов, будет выброшено исключение `NoSuchElementException`.
- `default void` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#remove--)`()` удаляет текущий элемент из коллекции. Если метод `next()` не был вызван после последнего вызова `remove()`, будет выброшено исключение `IllegalStateException`.

```Java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);

// получаем итератор для списка
Iterator<Integer> iterator = list.iterator();

// проверяем наличие следующего элемента и выводим его на экран
while (iterator.hasNext()) {
    Integer element = iterator.next();
    System.out.println(element);
    
    // удаляем текущий элемент из списка
    iterator.remove();
}
```

### ListIterator extends Iterator

[https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html)

Интерфейс, расширяющий интерфейс `Iterator`, предназначенный для перебора элементов списков. `ListIterator` обладает дополнительными методами, позволяющими осуществлять двунаправленный проход по списку и изменять его элементы.

### Методы

Наследуемые от Iterator:

- `boolean` [`hasNext`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#hasNext--)`()` возвращает `true`, если в коллекции есть следующий элемент, и `false`, если элементов больше нет.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html) [`next`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#next--)`()`возвращает следующий элемент коллекции. Если в коллекции больше нет элементов, будет выброшено исключение `NoSuchElementException`.
- `default void` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#remove--)`()` удаляет текущий элемент из коллекции. Если метод `next()` не был вызван после последнего вызова `remove()`, будет выброшено исключение `IllegalStateException`.

  

Собственные методы:

`void` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#add-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) `e)` добавляет указанный элемент в список между элементами, который будет возвращен следующим вызовом `next()` или `previous()`.

`boolean` [`hasPrevious`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#hasPrevious--)`()` возвращает `true`, если список имеет предыдущий элемент, и `false`, если элементов больше нет.

`int` [`nextIndex`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#nextIndex--)`()` возвращает индекс следующего элемента.

[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) [`previous`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#previous--)`()` возвращает предыдущий элемент списка. Если в списке больше нет элементов, будет выброшено исключение `NoSuchElementException`.

`int` [`previousIndex`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#previousIndex--)`()` возвращает индекс предыдущего элемента.

`void` [`set`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#set-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) `e)` аменяет последний элемент, который был возвращен методом `next()` или `previous()`, на указанный элемент.

```Java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;
import java.util.NoSuchElementException;

public class ListIteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple");
        list.add("banana");
        list.add("cherry");

        ListIterator<String> iterator = list.listIterator();

        // перебор списка вперед и вывод на экран
        System.out.println("Перебор списка вперед:");
        while (iterator.hasNext()) {
            int nextIndex = iterator.nextIndex();
            String nextElement = iterator.next();
            int previousIndex = iterator.previousIndex();
            System.out.println("  nextIndex=" + nextIndex + ", previousIndex=" + previousIndex + ", nextElement=" + nextElement);
        }

        // перебор списка назад и вывод на экран
        System.out.println("Перебор списка назад:");
        while (iterator.hasPrevious()) {
            int nextIndex = iterator.nextIndex();
            int previousIndex = iterator.previousIndex();
            String previousElement = iterator.previous();
            System.out.println("  nextIndex=" + nextIndex + ", previousIndex=" + previousIndex + ", previousElement=" + previousElement);
        }

        // замена второго элемента на "orange"
        iterator = list.listIterator();
        while (iterator.hasNext()) {
            String nextElement = iterator.next();
            if (nextElement.equals("banana")) {
                int previousIndex = iterator.previousIndex();
                System.out.println("Замена элемента на позиции " + previousIndex + " на \"orange\"");
                iterator.set("orange");
                break;
            }
        }

        // добавление элемента "mango" после "orange"
        iterator.add("mango");
        int previousIndex = iterator.previousIndex();
        System.out.println("Добавление элемента \"mango\" на позицию " + previousIndex);

        // вывод на экран измененного списка
        System.out.println("Измененный список:");
        for (String element : list) {
            System.out.println("  " + element);
        }
    }
}
```

## Interfaces Comparable and Comparator

### Comparator\<T>

предназначен для сравнения двух объектов типа T. Он определяет метод compare(T o1, T o2), который сравнивает два объекта и возвращает целое число, указывающее, какой из них должен быть отсортирован первым.

Метод compare должен возвращать отрицательное число, если o1 должен быть отсортирован перед o2, положительное число, если o1 должен быть отсортирован после o2, и ноль, если они равны.

Интерфейс Comparator\<T> используется вместе с классами, которые не реализуют интерфейс Comparable\<T>, чтобы определить порядок сортировки элементов. Классы, которые реализуют интерфейс Comparator\<T>, могут использоваться для сортировки массивов, списков и других коллекций.

### Методы

### Пример создания кастомного компаратора

```Java
package org.example;

public class Person {
    private String name;
    private double salary;

    public Person(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

@Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", salary=" + salary +
                '}';
    }
}
```

```Java
package org.example;

import java.util.Comparator;

public class PersonComparator implements Comparator<Person> {
    @Override
    public int compare(Person o1, Person o2) {
        if(o1.getSalary() > o2.getSalary()){
            return 1;
        }
        else if(o1.getSalary()< o2.getSalary()){
            return -1;
        }
        else return 0;
    }
    }
}
```

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(new PersonComparator());
        persons.forEach(System.out::println);

ВЫВОД:
Person{name='Kostya', salary=300.0}
Person{name='Vova', salary=500.0}
Person{name='Vadim', salary=1000.0}
```

### Анонимный компаратор, использование лямбда выражения

Анонимный класс:

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                if(o1.getSalary() > o2.getSalary()){
                    return 1;
                }
                else if(o1.getSalary()< o2.getSalary()){
                    return -1;
                }
                else return 0;
            }
        });
        }
    }
```

### Использование static метода класса Comparator

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(Comparator.comparingDouble(Person::getSalary));
```

### Comparable\<T>

определяет единственный метод `compareTo`, который позволяет  
упорядочить объекты данного класса. Этот интерфейс позволяет сравнивать  
объекты одного класса между собой, используя определенный порядок.  

- `int` [`compareTo`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-)`(`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) `o)` метод принимает на вход объект типа `T` и возвращает целое число, которое указывает на отношение порядка между текущим объектом и переданным в метод объектом. Возвращаемое значение может быть отрицательным, нулевым или положительным:
- отрицательное число указывает на то, что текущий объект меньше переданного в метод;
- ноль указывает на то, что текущий объект равен переданному в метод;
- положительное число указывает на то, что текущий объект больше переданного в метод.

```Java
public class Person implements Comparable<Person> {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Person other) {
        return name.compareTo(other.getName());
    }
}
```

```Java
List<Person> persons = new ArrayList<>();
persons.add(new Person("John"));
persons.add(new Person("Alice"));
persons.add(new Person("Bob"));
Collections.sort(persons);
System.out.println(persons); // [Alice, Bob, John]
```

## Interface Spliterator\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)

Spliterator (split-iterator) - это интерфейс, который был добавлен в Java 8 в пакет java.util. Он представляет собой новый тип итератора, который может рекурсивно разбивать коллекцию на несколько фрагментов для обработки в параллельном режиме.

Основное отличие между Spliterator и обычным итератором заключается в возможности разделить коллекцию на несколько частей для параллельной обработки, что может значительно ускорить процесс выполнения операций.

Сам Spliterator имеет несколько методов, которые нужны для выполнения задачи разбиения коллекции на части. Один из таких методов - это trySplit (), который разделяет итерируемый объект на две части. Результатом вызова этого метода является новый Spliterator, который может обрабатывать одну из двух частей, в то время как первый Spliterator обрабатывает другую часть.

Другие методы Spliterator включают методы для проверки оставшихся элементов в итерируемой коллекции, для выполнения операции для каждого элемента, для оценки количества элементов и т.д.

Вот небольшой пример использования Spliterator на Java:

```Java
List<Integer> myList = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
        Spliterator<Integer> mySplitIterator = myList.spliterator();
        mySplitIterator.trySplit().forEachRemaining(System.out::print); //вывод:12345
```

Кроме того, Spliterator используется во многих других классах Java Collections API, таких как HashSet, TreeSet, ArrayList и т.д. В некоторых случаях, когда требуется многопоточная обработка данных, использование Spliterator может быть значительно быстрее, чем использование обычного итератора.

### Методы

- `tryAdvance(Consumer<? super T> action)`: Если остался ещё элемент, выполняет заданное действие на нём и возвращает `true`; иначе возвращает `false`.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
while (spliterator.tryAdvance(System.out::println)) {
    // Выводит в консоль элементы: "a", "b", "c"
}
```

- `trySplit()`: Если этот сплитератор может быть разделён на части, возвращает новый сплитератор, который охватывает элементы, которые не будут охвачены этим сплитератором после вызова этого метода.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
Spliterator<String> newSpliterator = spliterator.trySplit();
if (newSpliterator != null) {
    newSpliterator.forEachRemaining(System.out::println); // Выводит в консоль элементы: "b", "c"
}
```

- `forEachRemaining(Consumer<? super T> action)`: Выполняет заданное действие для каждого оставшегося элемента, последовательно в текущем потоке, пока все элементы не будут обработаны или действие не вызовет исключение.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
spliterator.forEachRemaining(System.out::println); // Выводит в консоль элементы: "a", "b", "c"
```

- `estimateSize()`: Возвращает оценку количества элементов, которые будут найдены при обходе `forEachRemaining(Consumer<? super T> action)`, или возвращает `Long.MAX_VALUE`, если количество элементов бесконечно, неизвестно или вычисляется слишком долго.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
System.out.println(spliterator.estimateSize()); // Выводит в консоль "3"
```

- `getExactSizeIfKnown()`: Удобный метод, который возвращает `estimateSize()`, если этот сплитератор имеет размер, иначе возвращает `1`.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
System.out.println(spliterator.getExactSizeIfKnown()); // Выводит в консоль "3"
```

- `getComparator()`: Если источник этого сплитератора отсортирован с помощью компаратора, возвращает этот компаратор; иначе возвращает `null`.

```Java
Spliterator<String> spliterator = Arrays.asList("c", "a", "b").spliterator();
Comparator<String> comparator = spliterator.getComparator(); // Возвращает null
```

- `hasCharacteristics(int characteristics)`: возвращает `true`, если у данного сплитератора все характеристики, указанные в параметре `characteristics`.  
    Характеристики описывают особенности сплитератора и его элементов,  
    такие как имеющиеся у них свойства или ограничения. Данный метод может  
    использоваться, например, для проверки того, что сплитератор является  
    упорядоченным (  
    `ORDERED`), неизменяемым (`IMMUTABLE`) или сортированным (`SORTED`).

```Java
ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
Spliterator<Integer> spliterator = list.spliterator();

// Проверяем, содержит ли Spliterator характеристики ORDERED и SIZED
if (spliterator.hasCharacteristics(Spliterator.ORDERED | Spliterator.SIZED)) {
    System.out.println("Spliterator содержит характеристики ORDERED и SIZED");
} else {
    System.out.println("Spliterator не содержит характеристики ORDERED и SIZED");
}
```

## Interface List\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/List.html](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)

All Known Implementing Classes:

[AbstractList](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractList.html), [AbstractSequentialList](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSequentialList.html), [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), [AttributeList](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeList.html), [CopyOnWriteArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html), [LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html), [RoleList](https://docs.oracle.com/javase/8/docs/api/javax/management/relation/RoleList.html), [RoleUnresolvedList](https://docs.oracle.com/javase/8/docs/api/javax/management/relation/RoleUnresolvedList.html), [Stack](https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html), [Vector](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)

  

default methods

`void` [`replaceAll`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#replaceAll-java.util.function.UnaryOperator-)`(`[`UnaryOperator`](https://docs.oracle.com/javase/8/docs/api/java/util/function/UnaryOperator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`> operator)` заменяет каждый элемент списка на результат применения оператора `UnaryOperator` к этому элементу.

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.replaceAll(n -> n + 1);
```

  

`void` [`sort`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#sort-java.util.Comparator-)`(`[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`> c)` сортирует с помощью переданного компаратора

```Java
employees.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                if(o1.getSalary() > o2.getSalary()){
                    return  1;
                }
                else if(o1.getSalary() < o2.getSalary()){
                    return -1;
                }
                return 0;
            }
        });
```

[`Spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`>` [`spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#spliterator--)`()` возвращает объект `Spliterator`, который позволяет эффективно разбить коллекцию на части, что может быть полезно при параллельной обработке.

`Spliterator` похож на `Iterator`, но добавляет поддержку разбиения на части. Метод `tryAdvance` позволяет обработать каждый элемент последовательно, а метод `trySplit` возвращает новый объект `Spliterator`, который содержит часть элементов, на которых можно параллельно выполнить операции.

Использование `Spliterator` позволяет эффективно распараллелить операции над большими коллекциями данных.

  

### ArrayList

Наследуется от AbstractList

All Implemented Interfaces:

[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)\<E>, [RandomAccess](https://docs.oracle.com/javase/8/docs/api/java/util/RandomAccess.html)

Основные особенности ArrayList:

- ArrayList основан на массивах, но предоставляет динамическую размерность, что позволяет добавлять и удалять элементы в массиве в любое время.
- ArrayList может содержать объекты любого типа данных, включая объекты пользовательского класса.
- ArrayList автоматически увеличивает свой размер при добавлении новых элементов и уменьшает его при удалении элементов. Это обеспечивает гибкость при работе с массивом.
- ArrayList не является потокобезопасным, поэтому если необходимо использовать ArrayList в многопоточной среде, необходимо использовать синхронизацию.

### Конструкторы

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList--)`()` создает ArrayList с Capacity равным 10

При добавлении элемента в ArrayList, в котором не хватает места под капотом создается новый массив с большей размерностью, это занимает время, поэтмоу лучше использовать конструктов с размерностью, если известно, сколько элементов будет храниться в списке.

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` конструктор, который создает новый экземпляр списка `ArrayList`, содержащий элементы из заданной коллекции.

```Java
List<String> list1 = new ArrayList<>(Arrays.asList("apple",
 "banana", "orange"));
List<String> list2 = new ArrayList<>(list1);
System.out.println(list2); // вывод: [apple, banana, orange]
```

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList-int-)`(int initialCapacity)`

```Java
ArrayList<DataType> list = new ArrayList<>(30);
```

### Методы

- `boolean` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#add-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `e)` добавляет объект в лист

```Java
employees.add(new Employee("Vadim", "Konovalov"));
```

- `void` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#add-int-E-)`(int index,`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `element)` добавляет на определенную позицию объект, смещая остальные дальше

```Java
employees.add(new Employee(1, "Vadim", "Konovalov"));
```

- `boolean` [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#addAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` добавляет в лист значения из другой коллекции

```Java
List<Employee> list = new ArrayList<>(10);
        list.addAll(employees);

List<String> myList = new ArrayList<>();
myList.addAll(Arrays.asList("one", "two", "three"));
```

- `boolean` [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#addAll-int-java.util.Collection-)`(int index,` [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` добавляет коллекцию с определенной позиции, смещая объекты дальше

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
list.addAll(2,list);//  [1, 2, 1, 2, 3, 4, 3, 4]
```

- `void` [`clear`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#clear--)`()` очищает лист

```Java
list.clear();
```

- `boolean` [`contains`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#contains-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` проверяет содержится ли значение в листе

```Java
List<Integer> list = new ArrayList<Integer>();
        list.add(1);
        list.add(2);
	      boolean isContains = list.contains(2); // true
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`get`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#get-int-)`(int index)` возвращает объект по индексу

```Java
List<Integer> list = new ArrayList<Integer>();
        list.add(1);
        list.add(2);
        Integer value = list.get(1);  //2
```

- `int` [`indexOf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#indexOf-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` возвращает позицию объекта

```Java
List<Integer> list = new ArrayList<>();
list.add(5);
list.add(10);
sout(list.indexOf(10));  //return 1
```

- `boolean` [`isEmpty`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#isEmpty--)`()` проверяет пустой список или нет

```Java
List<Integer> list = new ArrayList<Integer>();
        System.out.println(list.isEmpty()); // true
```

- `int` [`lastIndexOf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#lastIndexOf-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` возвращает индекс последнего вхождения указанного объекта в список, или -1, если список не содержит указанного объекта.

```Java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 2, 5, 6, 2));
int index = list.lastIndexOf(2);
System.out.println(index); // Output: 7
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#remove-int-)`(int index)` удаляет из списка объект по индексу

```Java
employees.remove(1);
```

- `boolean` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#remove-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет объект из списка

```Java
Student student = new Student("Vadim");
employees.remove(student);
```

- `boolean` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#removeAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<?> c)` удаляет все элементы содержащиеся в прееданной коллекции

```Java
List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(i);
        }

List<Integer> list2 = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            list2.add(i);
        }
list.removeAll(list2);
System.out.println(list); [5, 6, 7, 8, 9]//
```

- `boolean` [`removeIf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#removeIf-java.util.function.Predicate-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> filter)` удаляет элементы в соответствии с условием

```Java
List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(i);
        }
        list.removeIf(x-> x % 2!=0);
        System.out.println(list); // [0, 2, 4, 6, 8]
```

- `void` [`replaceAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#replaceAll-java.util.function.UnaryOperator-)`(`[`UnaryOperator`](https://docs.oracle.com/javase/8/docs/api/java/util/function/UnaryOperator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> operator)` применяет функцию с замены, используя функции переданную в качестве аргумента

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.replaceAll(x->x*10);
        System.out.println(list); [10,20]
```

- `boolean` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#retainAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<?> c)` оставляет в списке, только те элементы, которые содержатся в коллекци, переданную в качестве аргумента

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
List<Integer> list2 = new ArrayList<>();
        list2.add(1);
        list2.add(3);
list.retainAll(list2);
System.out.println(list);  //1
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`set`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#set-int-E-)`(int index,` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `element)` устанавливает на позицию новый элемент, удаляя предыдущий

```Java
employees.set(1, new Employee("Vadim","Konovalov");
```

- `int` [`size`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#size--)`()` возвращает размер списка

```Java
for(int i = 0; i< employees.size(); i++{
doSomething();
}
```

- [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`>` [`subList`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#subList-int-int-)`(int fromIndex, int toIndex)` возвращает представление (view) списка, которое содержит элементы в диапазоне от индекса `fromIndex` (включительно) до индекса `toIndex` (исключительно).

```Java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<Integer> sublist = list.subList(1, 4); // sublist содержит элементы [2, 3, 4]
```

То есть, вызывая `list.subList(fromIndex, toIndex)`, вы  
получаете новый список, который является представлением части исходного  
списка. Изменения в этом списке будут отражаться на исходном списке и  
наоборот.  

```Java
sublist.set(1, 10); // sublist теперь содержит элементы [2, 10, 4]
System.out.println(list); // [1, 2, 10, 4, 5]
```

- [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)`[]` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#toArray--)`()`

```Java
Employee [] array = (Employee[]) employees.toArray();
```

### Vector and Stack

[https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)

Наследуется от AbstractList.

Это устаревший synchronized класс в Java, который представляет собой динамический массив, очень похожий на ArrayList. Главное отличие заключается в том, что Vector является потокобезопасным, в то время как ArrayList - нет. Это означает, что несколько потоков могут одновременно обращаться к методам Vector без каких-либо проблем синхронизации.

Не рекомендован для использования.

Некоторые из методов, которые принадлежат только классу Vector в Java:

- `addElement(E obj)`: добавляет объект в конец вектора.
- `insertElementAt(E obj, int index)`: вставляет объект в заданную позицию вектора.
- `removeElement(Object obj)`: удаляет первое вхождение указанного объекта из вектора.
- `removeElementAt(int index)`: удаляет элемент в указанной позиции вектора.
- `removeAllElements()`: удаляет все элементы из вектора.
- `capacity()`: возвращает текущую емкость вектора.
- `trimToSize()`: уменьшает емкость вектора до его текущего размера.
- `setSize(int newSize)`: устанавливает новый размер вектора. Если новый размер больше текущего, то добавляются пустые элементы, если меньше - удаляются элементы с конца вектора.

  

### Stack наследник Vector

[https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html](https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html)

Устаревший synchronized класс. Использует принцип LIFO. Last In First Oust.

Пример игра в дурака, с колоды берем последнюю положенную карту.

Не рекомендован для использования.

Некоторые методы Stack:

1. `push(E item)` - добавляет элемент на вершину стека.
2. `pop()` - удаляет и возвращает элемент на вершине стека. Может бросить EmptyStackException
3. `peek()` - возвращает элемент на вершине стека без его удаления. Может бросить EmptyStackException
4. `empty()` - проверяет, пуст ли стек.
5. `search(Object o)` - возвращает 1-based индекс нахождения объекта в стеке. Если объект не найден, возвращается -1.

  

### class LinkedList\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)<\E>, [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)\<E>

  

LinkedList - это реализация связанного списка в Java, где каждый узел списка хранит ссылку на следующий элемент в списке. Каждый узел также содержит данные, которые могут быть любого типа.

Преимущества LinkedList:

- Быстрое добавление и удаление элементов в середине списка. Это происходит за время O(1), так как не требуется перемещение других элементов, как при работе с массивами.
- Список может быть изменен в размере во время выполнения программы.
- Быстрое добавление и удаление элементов в начале списка. Это происходит за время O(1), так как нет необходимости сдвигать другие элементы, как при работе с массивами.

Недостатки LinkedList:

- Доступ к элементам осуществляется за время O(n), где n - это индекс элемента, так как для получения доступа к элементу нужно пройти по всему списку.
- Использование памяти выше, чем при использовании массивов, так как каждый узел списка должен хранить ссылки на следующий элемент в списке.
- Индексация элементов неэффективна, так как для получения элемента по индексу нужно пройти по всему списку.

  

Наследуемые методы:

LinkedList не наследует методов от ArrayList напрямую, так как они реализуют разные интерфейсы - LinkedList реализует интерфейс List, Queue и Deque, а ArrayList - только List и RandomAccess. Однако, обе коллекции реализуют базовый интерфейс Collection, поэтому многие методы у них совпадают.

  

Отличающиеся методы.

- `void` [`addFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#addFirst-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка

```Java
LinkedList<String> list = new LinkedList<>();
list.add("Alice");
list.addFirst("Bob");
System.out.println(list); // [Bob, Alice]
```

- `void` [`addLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#addLast-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в конец списка

```Java
LinkedList<String> list = new LinkedList<>();
list.add("Alice");
list.addLast("Charlie");
System.out.println(list); // [Alice, Charlie]
```

- [`descendingIterator`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#descendingIterator--)`()` определен в интерфейсе `Deque` и возвращает итератор, проходящий по элементам списка в обратном порядке (с конца списка к началу). В классе `LinkedList` данный метод реализуется следующим образом

```Java
public Iterator<E> descendingIterator() {
    return new DescendingIterator();
}

private class DescendingIterator implements Iterator<E> {
    private final ListIterator<E> itr = new ListItr(size());

    public boolean hasNext() {
        return itr.hasPrevious();
    }

    public E next() {
        return itr.previous();
    }

    public void remove() {
        itr.remove();
    }
}
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`element`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#element--)`()` возвращает первый элемент в списке, метод queue
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`getFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#getFirst--)`()` возвращает первый элемент в списке.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`getLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#getLast--)`()` возвращает последний элементв списке
- `boolean` [`offer`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offer-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)`и `boolean` [`offerLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offerLast-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в конец списка.

```Java
LinkedList<String> list = new LinkedList<>();
list.offerLast("Alice");
list.offerLast("Bob");
System.out.println(list); // [Alice, Bob]
```

- `boolean` [`offerFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offerFirst-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка

```Java
LinkedList<String> list = new LinkedList<>();
list.offerFirst("Alice");
list.offerFirst("Bob");
System.out.println(list); // [Bob, Alice]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peek`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peek--)`()` возвращает первый эелемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peekFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peekFirst--)`()` возвращает первый элемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peekLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peekLast--)`()` возвращает последний элемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`poll`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#poll--)`()` и [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pollFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pollFirst--)`()` удаляют и возвращают первый элемент в списке.

```Java
LinkedList<String> list = new LinkedList<>();
list.offer("Alice");
list.offer("Bob");
String first = list.pollFirst();
System.out.println(first); // Alice
System.out.println(list); // [Bob]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pollLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pollLast--)`()` удаляет и возвращает последний элемент в списке.

```Java
LinkedList<String> list = new LinkedList<>();
list.offer("Alice");
list.offer("Bob");
String last = list.pollLast();
System.out.println(last); // Bob
System.out.println(list); // [Alice]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pop`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pop--)`()` удаляет первый элемент
- `void` [`push`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#push-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`removeFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeFirst--)`()` удаляет первый элемент
- `boolean` [`removeFirstOccurrence`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeFirstOccurrence-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет первое вхождение

```Java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1,2,3,4,5,6,7,8,9,10,1));
        list.removeFirstOccurrence(1);
        System.out.println(list);

[2, 3, 4, 5, 6, 7, 8, 9, 10, 1]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`removeLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeLast--)`()` удаляет последний элемент
- `boolean` [`removeLastOccurrence`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeLastOccurrence-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет первое вхождение, начиная с конца

```Java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1,2,3,4,5,6,7,8,9,10,1));
        list.removeLastOccurrence(1);
        System.out.println(list);
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

  

## Interface Set\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/Set.html](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)

это интерфейс в языке программирования Java, который расширяет интерфейс `Collection` и представляет собой набор уникальных элементов, не допускающих повторений.

Интерфейс `SortedSet<E>` расширяет интерфейс `Set<E>` и добавляет возможность итерирования элементов в отсортированном порядке. Кроме того, он предоставляет методы для получения первого и последнего элементов множества, а также подмножеств между двумя элементами.

Интерфейс `NavigableSet<E>` также расширяет интерфейс `SortedSet<E>` и добавляет дополнительные методы для навигации в множестве. Например, он позволяет получать элементы, находящиеся ближайшими к определенному значению, и выполнять различные операции с элементами, основанные на их порядке.

### Методы interface Set\<E>

- boolean add(E e) - добавляет указанный элемент в этот набор, если его там еще нет (необязательная операция).
- boolean addAll(Collection\<? extends E> c) - добавляет все элементы из указанной коллекции в этот набор, если они еще не присутствуют (необязательная операция).
- void clear() - удаляет все элементы из этого набора (необязательная операция).
- boolean contains(Object o) - возвращает true, если этот набор содержит указанный элемент.
- boolean containsAll(Collection\<?> c) - возвращает true, если этот набор содержит все элементы указанной коллекции.
- boolean equals(Object o) - сравнивает указанный объект с этим набором на равенство.
- int hashCode() - возвращает хэш-код для этого набора.
- boolean isEmpty() - возвращает true, если этот набор не содержит элементов.
- Iterator\<E> iterator() - возвращает итератор по элементам в этом наборе.
- boolean remove(Object o) - удаляет указанный элемент из этого набора, если он присутствует (необязательная операция).
- boolean removeAll(Collection\<?> c) - удаляет из этого набора все его элементы, которые содержатся в указанной коллекции (необязательная операция).
- boolean retainAll(Collection\<?> c) - оставляет в этом наборе только элементы, которые содержатся в указанной коллекции (необязательная операция).
- int size() - возвращает количество элементов в этом наборе (его кардинальность).
- default Spliterator\<E> spliterator() - создает Spliterator по элементам в этом наборе.
- Object[] toArray() - возвращает массив, содержащий все элементы этого набора.
- \<T> T[] toArray(T[] a) - возвращает массив, содержащий все элементы этого набора; тип массива времени выполнения возвращаемого массива является указанным массивом.

### Методы interface NavigableSet\<E>

- `ceiling(E e)`: возвращает наименьший элемент в этом наборе, который больше или равен заданному элементу `e`, или `null`, если такого элемента нет.
- `descendingIterator()`: возвращает итератор по элементам в этом наборе в обратном порядке.
- `descendingSet()`: возвращает вид на элементы, содержащиеся в этом наборе в обратном порядке.
- `floor(E e)`: возвращает наибольший элемент в этом наборе, который меньше или равен заданному элементу `e`, или `null`, если такого элемента нет.
- `headSet(E toElement)`: возвращает вид на часть этого набора, элементы которой строго меньше `toElement`.
- `headSet(E toElement, boolean inclusive)`: возвращает вид на часть этого набора, элементы которой меньше (или равны, если `inclusive = true`) `toElement`.
- `higher(E e)`: возвращает наименьший элемент в этом наборе, который строго больше заданного элемента `e`, или `null`, если такого элемента нет.
- `iterator()`: возвращает итератор по элементам в этом наборе в возрастающем порядке.
- `lower(E e)`: возвращает наибольший элемент в этом наборе, который строго меньше заданного элемента `e`, или `null`, если такого элемента нет.
- `pollFirst()`: извлекает и удаляет первый (наименьший) элемент, или возвращает `null`, если этот набор пуст.
- `pollLast()`: извлекает и удаляет последний (наибольший) элемент, или возвращает `null`, если этот набор пуст.
- `subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)`: возвращает вид на часть этого набора, элементы которой находятся в диапазоне от `fromElement` до `toElement`.
- `subSet(E fromElement, E toElement)`: возвращает вид на часть этого набора, элементы которой находятся в диапазоне от `fromElement` (включительно) до `toElement` (исключительно).
- `tailSet(E fromElement)`: возвращает вид на часть этого набора, элементы которой больше или равны `fromElement`.
- `tailSet(E fromElement, boolean inclusive)`: возвращает вид на часть этого набора, элементы которой больше (или равны, если `inclusive = true`) `fromElement`.
- `comparator()`: возвращает компаратор, используемый для упорядочивания элементов в этом наборе, или `null`, если набор использует естественное упорядочение своих элементов.

Classes: [AbstractSet](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html), [ConcurrentHashMap.KeySetView](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.KeySetView.html), [ConcurrentSkipListSet](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListSet.html), [CopyOnWriteArraySet](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArraySet.html), [EnumSet](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html), [HashSet](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html), [JobStateReasons](https://docs.oracle.com/javase/8/docs/api/javax/print/attribute/standard/JobStateReasons.html), [LinkedHashSet](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html), [TreeSet](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)

### Методы interface SortedSet\<E>

- `comparator()`: возвращает компаратор, используемый для упорядочивания элементов в этом наборе, или `null`, если набор использует естественное упорядочение своих элементов.
- `first()`: возвращает первый (наименьший) элемент в этом наборе.
- `headSet(E toElement)`: возвращает представление части этого набора, элементы которой строго меньше `toElement`.
- `last()`: возвращает последний (наибольший) элемент в этом наборе.
- `spliterator()`: создает Spliterator для элементов в этом отсортированном наборе.
- `subSet(E fromElement, E toElement)`: возвращает представление части этого набора, элементы которой находятся в диапазоне от `fromElement`, включительно, до `toElement`, исключительно.
- `tailSet(E fromElement)`: возвращает представление части этого набора, элементы которой больше или равны `fromElement`.

## Классы имплементирующие эти интерфейсы

### class AbstractSet\<E> extends java.util.AbstractCollection\<E>

All Implemented Interfaces:[Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)<\E>

### class CopyOnWriteArraySet<\E> (java.util.concurrent)

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)<E

является потокобезопасной реализацией интерфейса `Set<E>` в Java, которая обеспечивает консистентность данных при одновременном доступе из нескольких потоков.

Плюсы:

1. Потокобезопасность: `CopyOnWriteArraySet<E>` обеспечивает потокобезопасность без необходимости использования дополнительных механизмов синхронизации, таких как явная синхронизация или блокировки.
2. Итератор безопасен: Итераторы, возвращаемые `CopyOnWriteArraySet<E>`, не выбрасывают `ConcurrentModificationException`, так как они работают на неизменной копии исходных данных.
3. Высокая производительность для чтения: При чтении данных нет блокировок и синхронизации, что может привести к улучшенной производительности в случае, когда чтение является основной операцией.

Минусы:

1. Потребление памяти: Каждое изменение `CopyOnWriteArraySet<E>` приводит к созданию полной копии внутреннего массива, что может привести к значительному потреблению памяти при большом объеме данных или частых изменениях.
2. Задержка обновлений: Изменения в `CopyOnWriteArraySet<E>` не отражаются немедленно, а создаются копия данных. Поэтому, если другой поток выполняет чтение в то время, когда изменения еще не применены, он может получить устаревшие данные.
3. Не подходит для частых изменений: `CopyOnWriteArraySet<E>` предназначен в основном для ситуаций, когда изменения происходят редко по сравнению с операциями чтения. Если изменения происходят часто, то `CopyOnWriteArraySet<E>` может оказаться неэффективным в плане производительности.

### class ConcurrentSkipListSet\<E>

Плюсы:

1. Потокобезопасность: `ConcurrentSkipListSet<E>` обеспечивает потокобезопасность при одновременных операциях чтения и записи. Различные потоки могут безопасно обращаться к набору одновременно без необходимости использования явной синхронизации.
2. Упорядоченность элементов: Элементы в `ConcurrentSkipListSet<E>` хранятся в отсортированном порядке, что обеспечивает возможность получения элементов в отсортированном виде. Это особенно полезно при работе с данными, требующими определенного порядка.
3. Высокая производительность: `ConcurrentSkipListSet<E>` обладает хорошей производительностью для операций чтения, вставки и удаления элементов, благодаря структуре данных Skip List. Это особенно заметно при одновременном доступе нескольких потоков к набору.
4. Масштабируемость: `ConcurrentSkipListSet<E>` хорошо масштабируется при увеличении количества потоков. Благодаря своей структуре данных, он может эффективно обрабатывать параллельные операции и распределять нагрузку между потоками.

Минусы:

1. Большее потребление памяти: `ConcurrentSkipListSet<E>` требует больше памяти по сравнению с другими реализациями `Set`, так как требуется хранить дополнительные данные для структуры данных Skip List. Если вам необходимо оптимизировать потребление памяти, другие реализации `Set` могут быть более подходящими.
2. Более сложная структура данных: Структура данных Skip List сложнее в реализации и понимании по сравнению с другими структурами данных, такими как обычные списки или деревья. Это может затруднить отладку и понимание работы `ConcurrentSkipListSet<E>`.
3. Ограниченные операции: `ConcurrentSkipListSet<E>` не предоставляет некоторые операции, доступные в других реализациях `Set`. Например, отсутствуют методы для получения элемента по индексу или операции массового обновления.

### class HashSet\<E>

HashSet не предоставляет прямого способа получить значение по индексу, поскольку значения в HashSet хранятся в неупорядоченном виде. Если вам требуется получить значение по определенному индексу, вам может потребоваться использовать другую реализацию, такую как ArrayList или LinkedList.

В HashSet значения хранятся в неупорядоченном виде и доступ к ним осуществляется с использованием метода `contains()` или `iterator()`.

Плюсы:

1. Быстрый доступ: Добавление, удаление и поиск элементов в HashSet выполняются очень быстро. Это происходит благодаря использованию хэш-таблицы, которая обеспечивает эффективный доступ к элементам по хэш-коду.
2. Уникальность элементов: HashSet гарантирует, что каждый элемент в наборе является уникальным. Если вы пытаетесь добавить дублирующийся элемент, он будет проигнорирован.
3. Нет порядка элементов: В HashSet элементы не упорядочены и могут располагаться внутри структуры данных в произвольном порядке. Если вам не требуется определенный порядок элементов, HashSet может быть хорошим выбором.
4. Высокая производительность: HashSet обладает хорошей производительностью при больших объемах данных. Операции добавления, удаления и поиска имеют почти постоянное время выполнения, что делает HashSet эффективным для работы с большими наборами данных.

Минусы:

1. Отсутствие гарантированного порядка: В HashSet нет гарантии относительного порядка элементов. Порядок может изменяться при изменении набора данных или перехэшировании хэш-таблицы. Если вам требуется сохранение порядка элементов, вам следует рассмотреть другие реализации Set, такие как LinkedHashSet.
2. Неупорядоченный итератор: Итератор HashSet не гарантирует определенный порядок обхода элементов. Порядок итерации могут влиять на внутренние механизмы хэш-таблицы, поэтому не стоит полагаться на порядок элементов при обходе набора.
3. Неэффективное использование памяти: HashSet может использовать больше памяти, чем другие реализации Set, особенно если в нем содержится большое количество элементов или элементы занимают много места в памяти. Это связано с необходимостью выделения памяти для хранения хэш-таблицы.
4. Затраты на хеширование: При использовании HashSet происходит вычисление хэш-кода для каждого добавленного элемента. В некоторых случаях это может быть затратной операцией.

### Методы

- `boolean add(E e)` Добавляет указанный элемент в множество, если его еще нет.
- `void clear()`Удаляет все элементы из множества.
- `Object clone()`Возвращает поверхностную копию данного экземпляра HashSet: элементы сами по себе не клонируются.
- `boolean contains(Object o)`Возвращает true, если множество содержит указанный элемент.
- `boolean isEmpty()`Возвращает true, если множество не содержит элементов.
- `Iterator<E> iterator()`Возвращает итератор по элементам множества.
- `boolean remove(Object o)`Удаляет указанный элемент из множества, если он присутствует.
- `int size()`Возвращает количество элементов в множестве (его мощность).
- `Spliterator<E> spliterator()`Создает late-binding и fail-fast Spliterator по элементам множества.

```Java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        // Создание экземпляра HashSet
        Set<String> set = new HashSet<>();

        // Добавление элементов в множество
        set.add("яблоко");
        set.add("банан");
        set.add("апельсин");

        // Вывод размера множества
        System.out.println("Размер множества: " + set.size());

        // Проверка наличия элемента в множестве
        boolean containsBanana = set.contains("банан");
        System.out.println("Множество содержит банан? " + containsBanana);

        // Итерация по элементам множества с использованием итератора
        Iterator<String> iterator = set.iterator();
        System.out.println("Элементы множества:");
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }

        // Удаление элемента из множества
        boolean removed = set.remove("яблоко");
        System.out.println("Элемент 'яблоко' удален? " + removed);

        // Очистка множества
        set.clear();
        System.out.println("Множество после очистки: " + set);
    }
}
```

### Methods inherited from class java.util.[AbstractSet](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html)

- [`equals`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#equals-java.lang.Object-)`,` [`hashCode`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#hashCode--)`,` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#removeAll-java.util.Collection-)

### Methods inherited from class java.util.[AbstractCollection](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html)

- [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#addAll-java.util.Collection-)`,` [`containsAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#containsAll-java.util.Collection-)`,` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#retainAll-java.util.Collection-)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toArray--)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toArray-T:A-)`,` [`toString`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toString--)

### **Methods inherited from class java.lang.**[**Object**](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)

- [`finalize`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#finalize--)`,` [`getClass`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)`,` [`notify`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--)`,` [`notifyAll`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait--)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait-long-)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait-long-int-)

### **Methods inherited from interface java.util.**[**Set**](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)

- [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#addAll-java.util.Collection-)`,` [`containsAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#containsAll-java.util.Collection-)`,` [`equals`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#equals-java.lang.Object-)`,` [`hashCode`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#hashCode--)`,` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#removeAll-java.util.Collection-)`,` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#retainAll-java.util.Collection-)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#toArray--)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#toArray-T:A-)

### Methods inherited from interface java.util.[Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)

- [`parallelStream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#parallelStream--)`,` [`removeIf`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#removeIf-java.util.function.Predicate-)`,` [`stream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--)

### **Methods inherited from interface java.lang.**[**Iterable**](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)

- [`forEach`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#forEach-java.util.function.Consumer-)

### class LinkedHashSet\<E> extends HashSet<\E>

Плюсы:

1. Порядок вставки элементов: LinkedHashSet сохраняет порядок вставки элементов. При итерации по множеству элементы будут возвращаться в том порядке, в котором они были добавлены. Это полезно, когда требуется сохранить порядок элементов и выполнить операции на основе порядка вставки.
2. Уникальность элементов: Как и другие реализации Set, LinkedHashSet гарантирует уникальность элементов. Если вы пытаетесь добавить элемент, который уже присутствует в множестве, операция добавления будет проигнорирована.
3. Быстрая операция поиска: LinkedHashSet использует хэш-таблицу для быстрой операции поиска элементов. Поиск элемента выполняется со сложностью O(1) в среднем случае.
4. Потокобезопасность: Если LinkedHashSet используется в многопоточной среде, можно использовать блокировку или другие механизмы синхронизации для обеспечения потокобезопасности при доступе к LinkedHashSet.

Минусы:

1. Относительно большое потребление памяти: LinkedHashSet требует дополнительной памяти для хранения связанного списка, который поддерживает порядок вставки элементов. Это может привести к большему потреблению памяти по сравнению с другими реализациями Set.
2. Медленные операции вставки и удаления: Вставка и удаление элементов в LinkedHashSet выполняются медленнее по сравнению с другими реализациями Set, такими как HashSet. Это связано с необходимостью поддержки порядка вставки элементов и обновления связанного списка.

### Методы

Наследует все методы от наследников

```Java
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class HashSetVsLinkedHashSetExample {
    public static void main(String[] args) {
        // Пример с HashSet
        Set<String> hashSet = new HashSet<>();

        hashSet.add("банан");
        hashSet.add("яблоко");
        hashSet.add("апельсин");
        hashSet.add("груша");
        hashSet.add("манго");

        System.out.println("HashSet:");
        for (String fruit : hashSet) {
            System.out.println(fruit);
        }

HashSet:
манго
банан
яблоко
груша
апельсин

        // Пример с LinkedHashSet
        Set<String> linkedHashSet = new LinkedHashSet<>();

        linkedHashSet.add("банан");
        linkedHashSet.add("яблоко");
        linkedHashSet.add("апельсин");
        linkedHashSet.add("груша");
        linkedHashSet.add("манго");

        System.out.println("\nLinkedHashSet:");
        for (String fruit : linkedHashSet) {
            System.out.println(fruit);
        }
    }
}

LinkedHashSet:
банан
яблоко
апельсин
груша
манго
```

### class EnumSet<E extends Enum\<E>>

это специализированная реализация интерфейса `Set`, предназначенная для работы с перечислениями (`enum`). Вот список плюсов и минусов использования `EnumSet`:

Плюсы:

1. Компактность: `EnumSet` использует внутреннее представление битовой маски, которая эффективно использует память для хранения элементов перечисления. Это делает `EnumSet` очень компактным и эффективным с точки зрения использования памяти.
2. Высокая производительность: Благодаря использованию битовой маски, операции добавления, удаления, проверки принадлежности и пересечения элементов выполняются очень быстро. `EnumSet` предлагает высокую производительность для операций над множествами элементов перечисления.
3. Проверка типов на этапе компиляции: `EnumSet` работает только с элементами перечисления, что обеспечивает проверку типов на этапе компиляции. Это гарантирует, что в `EnumSet` могут храниться только элементы определенного перечисления, что устраняет возможность ошибок типов во время выполнения.
4. Удобство использования: `EnumSet` предоставляет удобные методы для работы с множествами, такие как `add()`, `remove()`, `contains()`, `isEmpty()` и другие. Он также поддерживает итерацию и предоставляет методы для создания и комбинирования `EnumSet`.

Минусы:

1. Ограничение на использование перечислений: `EnumSet` может использоваться только с элементами перечисления. Это ограничение делает его не подходящим для работы с другими типами данных или коллекциями, не связанными с перечислениями.
2. Ограниченные возможности манипулирования множествами: `EnumSet` предоставляет базовый набор операций над множествами, таких как добавление, удаление и проверка принадлежности. Однако, он не поддерживает более сложные операции над множествами, такие как объединение, разность или симметрическая разность, которые доступны в более общих реализациях `Set`.
3. Неупорядоченность элементов: `EnumSet` не гарантирует сохранение порядка элементов в множестве. Если вам необходимо сохранить порядок элементов, вам может потребоваться использовать другую реализацию `Set`, такую как `LinkedHashSet`.

### class TreeSet<\E>

Это реализация интерфейса Set в Java, основанная на красно-черном дереве. Вот список плюсов и минусов использования TreeSet:

Плюсы:

1. Сортировка элементов: TreeSet автоматически сортирует элементы по их естественному порядку или заданному компаратору. Это позволяет получить элементы в упорядоченном виде при итерации или при выполнении операций поиска.
2. Поддержка навигации: TreeSet предоставляет методы для выполнения операций, связанных с навигацией по элементам, таких как получение наименьшего или наибольшего элемента, поиск элемента, нахождение ближайшего элемента и другие.
3. Использование пользовательского компаратора: TreeSet позволяет указать свой компаратор для сортировки элементов. Это позволяет гибко определить порядок сортировки, даже для типов данных, не реализующих интерфейс Comparable.
4. Потокобезопасность: Если TreeSet используется в многопоточной среде, можно использовать блокировку или другие механизмы синхронизации для обеспечения потокобезопасности при доступе к TreeSet.

Минусы:

1. Более медленная производительность: TreeSet обеспечивает относительно медленную производительность по сравнению с хэш-таблицами, используемыми в HashSet. Вставка, удаление и поиск элементов требуют времени O(log n), где n - количество элементов в TreeSet.
2. Ограничения на типы элементов: Для корректной работы TreeSet требуется, чтобы элементы были сравнимы. Это может быть проблемой для пользовательских типов данных, которые не реализуют интерфейс Comparable или не предоставляют компаратор.
3. Затраты на память: TreeSet требует дополнительной памяти для хранения структуры дерева. Каждый элемент требует дополнительных ссылок и узлов для организации дерева, что может привести к увеличению потребления памяти по сравнению с другими реализациями Set.

### Методы

- `boolean add(E e)` - Добавляет указанный элемент в это множество, если его еще нет.
- `boolean addAll(Collection<? extends E> c)` - Добавляет все элементы из указанной коллекции в это множество.
- `E ceiling(E e)` - Возвращает наименьший элемент в этом множестве, больший или равный указанному элементу, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(20);
        set.add(30);
        set.add(40);
        set.add(50);

        // Использование метода ceiling
        Integer result1 = set.ceiling(25);
        System.out.println("Ceiling of 25: " + result1);
 // Ожидаемый результат: 30

        Integer result2 = set.ceiling(35);
        System.out.println("Ceiling of 35: " + result2);
 // Ожидаемый результат: 40

        Integer result3 = set.ceiling(55);
        System.out.println("Ceiling of 55: " + result3);
 // Ожидаемый результат: null
    }
}
```

- `void clear()` - Удаляет все элементы из этого множества.
- `Object clone()` - Возвращает поверхностную копию этого экземпляра TreeSet: сами элементы не клонируются.
- `Comparator<? super E> comparator()` - Возвращает компаратор, используемый для упорядочения элементов в этом множестве, или null, если это множество использует естественный порядок своих элементов.
- `boolean contains(Object o)` - Возвращает true, если это множество содержит указанный элемент.
- `Iterator<E> descendingIterator()` - Возвращает итератор по элементам этого множества в обратном порядке.
- `NavigableSet<E> descendingSet()` - Возвращает представление элементов этого множества в обратном порядке.
- `E first()` - Возвращает первый (наименьший) элемент, находящийся в данный момент в этом множестве.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода first
        Integer firstElement = set.first();
        System.out.println("First element: " + firstElement);
 // Ожидаемый результат: 5
    }
}
```

- `E floor(E e)` - Возвращает наибольший элемент в этом множестве, меньший или равный указанному элементу, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода floor
        Integer floorElement = set.floor(12);
        System.out.println("Floor element: " + floorElement); // Ожидаемый результат: 10
    }
}
```

- `SortedSet<E> headSet(E toElement)` - Возвращает представление части этого множества, элементы которой строго меньше toElement.

```Java
import java.util.TreeSet;
import java.util.SortedSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода headSet
        SortedSet<Integer> headSet = set.headSet(15);
        System.out.println("Head set: " + headSet);
 // Ожидаемый результат: [5, 10]
    }
}
```

- `NavigableSet<E> headSet(E toElement, boolean inclusive)` - Возвращает представление части этого множества, элементы которой меньше (или равны, если inclusive равен true) toElement.

```Java
import java.util.TreeSet;
import java.util.NavigableSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода headSet
        NavigableSet<Integer> headSet = set.headSet(15, true);
        System.out.println("Head set: " + headSet);
 // Ожидаемый результат: [5, 10, 15]
    }
}
```

- `E higher(E e)` - Возвращает наименьший элемент в этом множестве, строго больший указанного элемента, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода higher
        Integer higherElement = set.higher(10);
        System.out.println("Higher element: " + higherElement);
 // Ожидаемый результат: 15
    }
}
```

- `boolean isEmpty()` - Возвращает true, если это множество не содержит элементов.
- `Iterator<E> iterator()` - Возвращает итератор по элементам этого множества в возрастающем порядке.
- `E last()` - Возвращает последний (наивысший) элемент, находящийся в данный момент в этом множестве.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода last
        Integer lastElement = set.last();
        System.out.println("Last element: " + lastElement);
 // Ожидаемый результат: 20
    }
}
```

- `E lower(E e)` - Возвращает наибольший элемент в этом множестве, строго меньший указанного элемента, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода lower
        Integer lowerElement = set.lower(15);
        System.out.println("Lower element: " + lowerElement); 
// Ожидаемый результат: 10
    }
}
```

- `E pollFirst()` - Извлекает и удаляет первый (наименьший) элемент, или возвращает null, если это множество пусто.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода pollFirst
        Integer firstElement = set.pollFirst();
        System.out.println("First element: " + firstElement); 
// Ожидаемый результат: 5
        System.out.println("Set after removing first element: " + set);
 // Ожидаемый результат: [10, 15, 20]
    }
}
```

- `E pollLast()` - Извлекает и удаляет последний (наивысший) элемент, или возвращает null, если это множество пусто.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода pollLast
        Integer lastElement = set.pollLast();
        System.out.println("Last element: " + lastElement); // Ожидаемый результат: 20
        System.out.println("Set after removing last element: " + set); // Ожидаемый результат: [5, 10, 15]
    }
}
```

- `boolean remove(Object o)` - Удаляет указанный элемент из этого множества, если он присутствует.
- `int size()` - Возвращает количество элементов в этом множестве.
- `Spliterator<E> spliterator()` - Создает Spliterator, который относится к элементам этого множества и обладает поздней привязкой и стратегией быстрой остановки.
- `NavigableSet<E> subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)` - Возвращает представление части этого множества, элементы которой находятся в диапазоне от fromElement до toElement.

```Java
import java.util.TreeSet;
import java.util.NavigableSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);
        set.add(25);
        set.add(30);

        // Использование метода subSet
        NavigableSet<Integer> subset = set.subSet(10, true, 25, false);
        System.out.println("Subset: " + subset);
 // Ожидаемый результат: [10, 15, 20]
    }
}
```

- `SortedSet<E> subSet(E fromElement, E toElement)` - Возвращает представление части этого множества, элементы которой находятся в диапазоне от fromElement (включительно) до toElement (исключительно).

```Java
SortedSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9));
SortedSet<Integer> subset = set.subSet(3, 8);
System.out.println(subset); // выведет [3, 4, 5, 6, 7]
```

- `SortedSet<E> tailSet(E fromElement)` - Возвращает представление части этого множества, элементы которой больше или равны fromElement.

```Java
SortedSet<String> set = new TreeSet<>(Arrays.asList("apple", "banana", "cherry", "date", "elderberry"));
SortedSet<String> tailSet = set.tailSet("cherry");
System.out.println(tailSet); // выведет [cherry, date, elderberry]
```

- `NavigableSet<E> tailSet(E fromElement, boolean inclusive)` - Возвращает представление части этого множества, элементы которой больше (или равны, если inclusive равен true) fromElement.

```Java
NavigableSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5));
NavigableSet<Integer> tailSet = set.tailSet(3, true);
System.out.println(tailSet); // выведет [3, 4, 5]
```

## Interface Queue\<E>

Представляет собой коллекцию элементов, в которой элементы добавляются в конец (tail) и удаляются из начала (head). Такая структура данных называется очередью (queue) или FIFO (first-in, first-out).

## Классы имплементирующие эти интерфейсы:

### class AbstractQueue\<E> extends java.util.AbstractCollection\<E>

### class SynchronousQueue\<E>

Класс `SynchronousQueue<E>` в Java представляет собой реализацию блокирующей очереди, в которой каждая операция добавления элемента ожидает соответствующую операцию удаления элемента, и наоборот.

```Java
SynchronousQueue<Integer> integers = new SynchronousQueue<>();
        integers.add(1);
        integers.add(2);
        System.out.println(integers);

Exception in thread "main"
java.lang.IllegalStateException: Queue full
```

Вот некоторые плюсы и минусы использования `SynchronousQueue<E>`:

Плюсы:

1. Простота синхронизации: `SynchronousQueue` предоставляет удобный механизм синхронизации между потоками, поскольку он автоматически блокирует операции добавления и удаления до тех пор, пока соответствующая операция не будет доступна.
2. Эффективность памяти: `SynchronousQueue` обычно использует меньше памяти, чем другие реализации очередей, так как не требует внутреннего буфера для хранения элементов.
3. Гарантия доставки: Когда один поток добавляет элемент в `SynchronousQueue`, операция добавления будет заблокирована до тех пор, пока другой поток не выполнит операцию удаления элемента. Это гарантирует, что элемент успешно доставлен и получен другим потоком.

Минусы:

1. Ограниченная вместимость: `SynchronousQueue` не имеет внутреннего буфера для хранения элементов, поэтому каждая операция добавления должна ждать соответствующую операцию удаления. Если отсутствует поток, который ожидает удаления элемента, операция добавления будет заблокирована.
2. Потенциальная блокировка: Использование `SynchronousQueue` может привести к блокировкам потоков, особенно если нет гарантии, что все операции добавления и удаления будут правильно сопоставлены и взаимосвязаны.
3. Сложность реализации: Использование `SynchronousQueue` требует более внимательного и аккуратного управления синхронизацией и управлением потоками, чтобы избежать потенциальных проблем с блокировками и неправильной синхронизацией.

### class PriorityQueue\<E>

Класс `PriorityQueue<E>` представляет собой реализацию очереди с приоритетом(определяется на основе их "естественного порядка" или с использованием компаратора) в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

```Java
public class Person {
    private String name;
    private double salary;}

PriorityQueue<Person> personPriorityQueue = new PriorityQueue<>();
personPriorityQueue.add(new Person("Vadim", 1000));

Exception in thread "main"
java.lang.ClassCastException:
class org.example.Person
 cannot be cast to class java.lang.Comparable
 (org.example.Person is in unnamed module of loader 'app';
 java.lang.Comparable is in module java.base of loader 'bootstrap')
```

```Java
public class Person implements Comparable<Person> {
    private String name;
    private double salary;
    
    // Конструкторы, геттеры и сеттеры
    
    @Override
    public int compareTo(Person otherPerson) {
        // Сравнение по полю salary
        if (this.salary < otherPerson.getSalary()) {
            return -1; // Текущий объект имеет меньшую заработную плату
        } else if (this.salary > otherPerson.getSalary()) {
            return 1; // Текущий объект имеет большую заработную плату
        } else {
            return 0; // Заработная плата равна
        }
    }
}
```

Плюсы:

1. Приоритетная очередь автоматически сортирует элементы на основе их приоритета. Это позволяет эффективно получать элементы с наивысшим приоритетом.
2. Класс `PriorityQueue<E>` обеспечивает быструю вставку и извлечение элементов. Вставка элемента в приоритетную очередь имеет сложность O(log n), а извлечение - O(1).
3. Можно использовать `PriorityQueue<E>` для реализации алгоритмов, таких как алгоритм Дейкстры или алгоритм А* (A-star), которым требуется обработка элементов в порядке их приоритета.
4. Класс `PriorityQueue<E>` предоставляет набор методов для работы с очередью, таких как добавление элемента (`add()`), удаление и возврат элемента с наивысшим приоритетом (`poll()`), проверка наличия элементов (`isEmpty()`), получение количества элементов (`size()`) и другие.

Минусы:

1. Приоритетная очередь не является потокобезопасной. Если необходимо использовать `PriorityQueue<E>` в многопоточной среде, требуется синхронизация доступа к нему.
2. Класс `PriorityQueue<E>` не допускает хранение дублирующихся элементов. Если необходимо хранить несколько элементов с одинаковым приоритетом, потребуется использовать дополнительные механизмы для разрешения коллизий.
3. Время доступа к произвольному элементу в приоритетной очереди не является эффективным. Если требуется частое обращение к элементам по их индексу, другие структуры данных, такие как список или массив, могут быть более подходящими.
4. При изменении приоритета элемента, содержащегося в `PriorityQueue<E>`, не происходит автоматическая пересортировка очереди. Это может потребовать дополнительных операций для поддержания корректного состояния приоритетной очереди.

### class PriorityBlockingQueue\<E>

Класс `PriorityBlockingQueue<E>` представляет собой реализацию блокирующей приоритетной очереди в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

Плюсы:

1. `PriorityBlockingQueue<E>` предоставляет механизм блокировки, который позволяет ожидать или блокировать потоки при попытке извлечения элементов из пустой очереди или вставки элементов в полную очередь. Это особенно полезно в многопоточных сценариях, когда необходима синхронизация доступа к приоритетной очереди.
2. Как и `PriorityQueue<E>`, `PriorityBlockingQueue<E>` автоматически сортирует элементы на основе их приоритета, что обеспечивает эффективное извлечение элементов с наивысшим приоритетом.
3. Класс `PriorityBlockingQueue<E>` реализует интерфейс `BlockingQueue<E>`, который предоставляет дополнительные методы для ожидания или блокировки потоков при попытке выполнить операции с очередью, такие как `take()` и `put()`. Это позволяет эффективно управлять параллельными задачами и обменом данными между потоками.
4. `PriorityBlockingQueue<E>` является потокобезопасной реализацией, что позволяет безопасно использовать ее в многопоточных средах без дополнительной синхронизации.

Минусы:

1. Приоритетная блокирующая очередь может иметь некоторые накладные расходы на управление блокировками и синхронизацией, особенно в ситуациях с высокой нагрузкой на многопоточную обработку. В некоторых случаях это может снизить производительность по сравнению с непотокобезопасными реализациями `PriorityQueue<E>`.
2. Использование блокировок в `PriorityBlockingQueue<E>` может потенциально привести к состоянию гонки (race condition) или блокировке потоков, если не управлять ими правильно. Необходимо аккуратно обрабатывать исключения и гарантировать корректное использование методов блокировки.

### class LinkedTransferQueue\<E>

Класс `LinkedTransferQueue<E>` представляет собой реализацию блокирующей очереди с возможностью передачи элементов между потоками в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

Плюсы:

1. `LinkedTransferQueue<E>` обеспечивает эффективную передачу элементов между потоками без блокировки при наличии готового получателя. Это особенно полезно в ситуациях, когда один поток ждет, чтобы передать элемент другому потоку, и передача должна произойти немедленно, если есть готовый получатель.
2. Класс `LinkedTransferQueue<E>` реализует интерфейс `TransferQueue<E>`, который предоставляет дополнительные методы, такие как `tryTransfer()`, `hasWaitingConsumer()`, `getWaitingConsumerCount()`, позволяющие более гибко управлять передачей элементов и контролировать состояние очереди.
3. `LinkedTransferQueue<E>` является потокобезопасной реализацией, что позволяет безопасно использовать ее в многопоточных средах без дополнительной синхронизации.
4. `LinkedTransferQueue<E>` обладает хорошей производительностью и масштабируемостью, особенно в сценариях с большим количеством потоков, благодаря своей внутренней структуре данных.

Минусы:

1. Использование `LinkedTransferQueue<E>` может потреблять больше памяти по сравнению с другими реализациями очередей, особенно при хранении большого количества элементов. Это следует учитывать при проектировании приложений с ограниченными ресурсами.
2. Передача элементов в `LinkedTransferQueue<E>` может потребовать согласованности между производителями и потребителями, чтобы избежать блокировки или ожидания. Некорректное использование методов передачи элементов может привести к заблокированным или ожидающим потокам.

### class LinkedBlockingQueue\<E>

Класс `LinkedBlockingQueue<E>` представляет собой реализацию блокирующей очереди в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

Плюсы:

1. `LinkedBlockingQueue<E>` обеспечивает потокобезопасное добавление и удаление элементов из очереди без необходимости явной синхронизации со стороны программиста. Это особенно полезно в многопоточных сценариях, где необходимо безопасно обмениваться данными между потоками.
2. Очередь `LinkedBlockingQueue<E>` имеет возможность ограничивать емкость, что позволяет контролировать использование ресурсов и предотвращать переполнение памяти.

```Java
// Создание очереди с максимальной емкостью 5
        LinkedBlockingQueue<Integer> queue = new LinkedBlockingQueue<>(5);

        // Добавление элементов в очередь
        for (int i = 1; i <= 7; i++) {
            try {
                queue.put(i); // Если очередь заполнена, поток будет заблокирован
                System.out.println("Элемент " + i + " добавлен в очередь.");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
Элемент 1 добавлен в очередь.
Элемент 2 добавлен в очередь.
Элемент 3 добавлен в очередь.
Элемент 4 добавлен в очередь.
Элемент 5 добавлен в очередь. и программа ждет, не завершается
```

1. Класс `LinkedBlockingQueue<E>` реализует интерфейс `BlockingQueue<E>`, который предоставляет дополнительные методы для ожидания или блокировки потоков при попытке выполнить операции с очередью, такие как `take()` и `put()`. Это позволяет эффективно управлять параллельными задачами и обменом данными между потоками.
2. `LinkedBlockingQueue<E>` обладает хорошей производительностью и масштабируемостью, особенно в сценариях с большим количеством потоков, благодаря своей внутренней структуре данных.

Минусы:

1. При добавлении элементов в полную очередь или извлечении элементов из пустой очереди, поток может быть заблокирован до тех пор, пока не будет освобождено место или появится новый элемент. В ситуациях с высокой нагрузкой на многопоточную обработку это может привести к простою или замедлению выполнения программы.
2. Использование блокировок в `LinkedBlockingQueue<E>` может потенциально привести к состоянию гонки (race condition) или блокировке потоков, если не управлять ими правильно. Необходимо аккуратно обрабатывать исключения и гарантировать корректное использование методов блокировки.

### class LinkedBlockingDeque\<E>

`LinkedBlockingDeque` - это реализация интерфейса `BlockingDeque` в Java, которая представляет собой двустороннюю блокирующую очередь (дек) на основе связанного списка. Вот некоторые плюсы и минусы `LinkedBlockingDeque`:

Плюсы:

- Потокобезопасность: `LinkedBlockingDeque` является потокобезопасной структурой данных, что позволяет ей быть безопасно использованной в многопоточных средах без необходимости в явной синхронизации.
- Блокировка: `LinkedBlockingDeque` предоставляет операции блокировки, такие как `take()` и `put()`, которые могут блокировать потоки до тех пор, пока не будет доступен элемент для чтения или место для записи. Это полезно в сценариях, где потоки должны ждать доступа к очереди.
- Ограничение на размер: `LinkedBlockingDeque` имеет ограниченную емкость, которая может быть определена при создании экземпляра `LinkedBlockingDeque`. Это позволяет контролировать использование ресурсов и предотвращать переполнение очереди.

Минусы:

- Ограничение на размер: В случае, когда `LinkedBlockingDeque` достигает своей максимальной емкости, добавление элементов может быть заблокировано до освобождения места в очереди. Это может привести к блокировке потоков и замедлению выполнения программы, если не предусмотрены соответствующие стратегии обработки переполнения.
- Дополнительные расходы памяти: `LinkedBlockingDeque` использует связанный список для хранения элементов, что может потреблять больше памяти по сравнению с некоторыми другими реализациями `Deque`.

### Разница между ConcurrentLinkedDeque

`ConcurrentLinkedDeque` и `LinkedBlockingDeque` - это две разные реализации интерфейса `Deque` в Java, которые предоставляют двустороннюю очередь (дек). Они отличаются следующим образом:

1. Потокобезопасность: `ConcurrentLinkedDeque` является потокобезопасной реализацией, что означает, что она может быть безопасно использована в многопоточной среде без необходимости в явной синхронизации. С другой стороны, `LinkedBlockingDeque` является реализацией `Deque`, предназначенной для использования в многопоточных средах, но с использованием блокировок для обеспечения синхронизации между потоками.
2. Ограничение на размер: `ConcurrentLinkedDeque` не имеет ограничений на размер, и его вместимость может динамически изменяться по мере добавления и удаления элементов. С другой стороны, `LinkedBlockingDeque` имеет ограниченную емкость, которая определяется при создании экземпляра `LinkedBlockingDeque`. Если очередь достигает своего предела, добавление элементов может быть заблокировано до освобождения места в очереди.
3. Операции блокировки: `LinkedBlockingDeque` предоставляет дополнительные операции блокировки, такие как `take()` и `put()`, которые могут блокировать потоки до тех пор, пока не будет доступен элемент для чтения или место для записи соответственно. Такие операции особенно полезны в сценариях, где потоки должны ждать доступа к очереди.

В зависимости от ваших потребностей, вы можете выбрать между `ConcurrentLinkedDeque` и `LinkedBlockingDeque`. Если вам требуется потокобезопасная реализация с динамическим изменением размера, `ConcurrentLinkedDeque` может быть предпочтительнее. Если же вам нужны операции блокировки и контроль над емкостью очереди, `LinkedBlockingDeque` может быть более подходящим вариантом.

### class DelayQueue\<E extends Delayed>

### class ConcurrentLinkedQueue<\E>

Класс `ConcurrentLinkedQueue<E>` представляет собой реализацию неблокирующей (lock-free) очереди в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

Плюсы:

1. `ConcurrentLinkedQueue<E>` обеспечивает высокую производительность в многопоточных средах, поскольку операции добавления и удаления элементов выполняются без блокировки потоков и с минимальной синхронизацией. Это позволяет эффективно обрабатывать большое количество потоков, работающих с очередью одновременно.
2. Очередь `ConcurrentLinkedQueue<E>` не имеет ограничения по емкости, что позволяет динамически увеличивать или уменьшать размер очереди по мере необходимости.
3. Класс `ConcurrentLinkedQueue<E>` реализует интерфейс `Queue<E>`, предоставляя стандартные методы для добавления, удаления и доступа к элементам очереди. Это делает его легко интегрируемым в существующий код, который работает с интерфейсом `Queue`.
4. `ConcurrentLinkedQueue<E>` является потокобезопасной реализацией, что позволяет безопасно использовать ее в многопоточных средах без дополнительной синхронизации.

Минусы:

1. В отличие от блокирующих очередей, `ConcurrentLinkedQueue<E>` не предоставляет механизм блокировки или ожидания при попытке извлечения элементов из пустой очереди или вставки элементов в полную очередь. Потоки, ожидающие элементы в пустой очереди, будут продолжать свое выполнение без блокировки или ожидания.
2. Поскольку `ConcurrentLinkedQueue<E>` не блокирует потоки при операциях вставки или удаления, нет гарантии наличия определенного элемента в очереди в любой момент времени. Это может быть нежелательным, если требуется строгая синхронизация или контроль над состоянием очереди.
3. `ConcurrentLinkedQueue<E>` не поддерживает операции, которые зависят от порядка элементов в очереди, такие как сортировка или обратное извлечение элементов. Если вам нужны такие операции, возможно, следует рассмотреть другие реализации очередей.

### class ArrayBlockingQueue\<E>

Класс `ArrayBlockingQueue<E>` представляет собой реализацию блокирующей очереди в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

Плюсы:

1. `ArrayBlockingQueue<E>` обеспечивает потокобезопасное добавление и удаление элементов из очереди без необходимости явной синхронизации со стороны программиста. Это особенно полезно в многопоточных сценариях, где необходимо безопасно обмениваться данными между потоками.
2. Очередь `ArrayBlockingQueue<E>` имеет ограниченную емкость, которая задается при создании экземпляра класса. Это позволяет контролировать использование ресурсов и предотвращать переполнение памяти.
3. `ArrayBlockingQueue<E>` реализует интерфейс `BlockingQueue<E>`, который предоставляет дополнительные методы для ожидания или блокировки потоков при попытке выполнить операции с очередью, такие как `take()` и `put()`. Это позволяет эффективно управлять параллельными задачами и обменом данными между потоками.
4. `ArrayBlockingQueue<E>` обеспечивает предсказуемый порядок элементов в очереди, поскольку они хранятся во внутреннем массиве. Это может быть полезно, если требуется соблюдение порядка элементов или доступ к элементам по индексу.

Минусы:

1. При добавлении элементов в полную очередь или извлечении элементов из пустой очереди, поток может быть заблокирован до тех пор, пока не будет освобождено место или появится новый элемент. В ситуациях с высокой нагрузкой на многопоточную обработку это может привести к простою или замедлению выполнения программы.
2. Использование блокировок в `ArrayBlockingQueue<E>` может потенциально привести к состоянию гонки (race condition) или блокировке потоков, если не управлять ими правильно. Необходимо аккуратно обрабатывать исключения и гарантировать корректное использование методов блокировки.
3. При удалении элементов из середины очереди `ArrayBlockingQueue<E>` может потребоваться перестановка оставшихся элементов, что может вызвать небольшую задержку при выполнении операции удаления.

`ArrayBlockingQueue<E>` и `LinkedBlockingQueue<E>` - это две разные реализации блокирующей очереди в языке программирования Java. Они отличаются друг от друга следующим образом:

1. Внутреннее представление данных:
    - `ArrayBlockingQueue<E>` использует фиксированный массив для хранения элементов очереди. Это означает, что емкость очереди задается при создании и не может быть изменена.
    - `LinkedBlockingQueue<E>` использует связанный список для хранения элементов очереди. Он не имеет ограничения на емкость и может динамически расти или уменьшаться по мере добавления или удаления элементов.
2. Производительность:
    - Вставка и удаление элементов в `ArrayBlockingQueue<E>` происходит с использованием блокировок на уровне массива, что может привести к блокировке или ожиданию потоков при попытке выполнить операции вставки или удаления.
    - В `LinkedBlockingQueue<E>` блокировки применяются на уровне узлов связанного списка, что позволяет другим потокам продолжать выполнение без блокировки или ожидания.
3. Использование памяти:
    - `ArrayBlockingQueue<E>` имеет фиксированную емкость и выделяет память под все элементы сразу при создании.
    - `LinkedBlockingQueue<E>` не имеет ограничений по емкости и выделяет память для каждого элемента при его добавлении в очередь.
4. Управление памятью:
    - `ArrayBlockingQueue<E>` может использовать немного больше памяти, так как выделяет фиксированный массив для хранения элементов, даже если не все ячейки заполнены.
    - `LinkedBlockingQueue<E>` использует память только для хранения реально добавленных элементов, что может быть более эффективным в случае очередей с небольшим количеством элементов.

Выбор между `ArrayBlockingQueue<E>` и `LinkedBlockingQueue<E>` зависит от конкретных требований вашего приложения. Если вам требуется фиксированная емкость и эффективное использование памяти, `ArrayBlockingQueue<E>` может быть предпочтительным выбором. Если вам нужна динамическая емкость и лучшая производительность в многопоточной среде, `LinkedBlockingQueue<E>` может быть более подходящим.

  

### class ConcurrentLinkedDeque\<E> extends java.util.AbstractCollection\<E>

Плюсы:

- Потокобезопасность: `ConcurrentLinkedDeque` является потокобезопасной структурой данных, что означает, что он безопасно может использоваться в многопоточной среде без необходимости во внешней синхронизации.
- Блокирующие и неблокирующие операции: `ConcurrentLinkedDeque` предоставляет как блокирующие, так и неблокирующие операции для добавления, удаления и получения элементов из очереди. Это позволяет выбирать наиболее подходящий подход в зависимости от конкретных требований.
- Эффективное добавление и удаление: добавление и удаление элементов из `ConcurrentLinkedDeque` выполняется эффективно, особенно в конце списка, что делает его хорошим выбором для задач, где требуется интенсивное добавление и удаление элементов.

Минусы:

- Отсутствие произвольного доступа: `ConcurrentLinkedDeque` не предоставляет операции для произвольного доступа к элементам по индексу, так как он представляет собой связанный список. Если требуется быстрый доступ к элементам по индексу, возможно, следует рассмотреть альтернативные реализации, такие как `ArrayList` или `LinkedList`.
- Большее потребление памяти: `ConcurrentLinkedDeque` потребляет больше памяти по сравнению с другими реализациями очередей, так как каждый элемент содержит ссылки на предыдущий и следующий элементы.

В целом, `ConcurrentLinkedDeque` является хорошим выбором для задач, где требуется многопоточное добавление и удаление элементов без блокировки всей структуры данных. Однако, если вам нужен произвольный доступ к элементам по индексу или вам требуется более компактная структура данных, возможно, стоит рассмотреть альтернативные реализации.

### class ArrayDeque<\E> extends java.util.AbstractCollection<\E>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)<\E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)<\E>, [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)<\E>, [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)<\E>

Класс `ArrayDeque<E>` представляет собой реализацию двусторонней очереди (дек) на основе массива в Java. Вот некоторые плюсы и минусы данного класса:

Плюсы:

- Эффективность вставки и удаления: `ArrayDeque` обеспечивает быструю вставку и удаление элементов как в начало, так и в конец очереди. Это делает его хорошим выбором для задач, где требуется интенсивное добавление и удаление элементов.
- Произвольный доступ: `ArrayDeque` предоставляет возможность произвольного доступа к элементам по индексу. Это позволяет эффективно получать элементы из середины очереди или выполнять операции, требующие произвольного доступа.
- Компактное использование памяти: по сравнению с другими реализациями деков, `ArrayDeque` обычно потребляет меньше памяти, так как использует внутренний массив для хранения элементов.

Минусы:

- Ограниченный размер: `ArrayDeque` имеет ограниченный размер, который определяется размером внутреннего массива. Если количество элементов в очереди достигает предела внутреннего массива, происходит его расширение, что может потребовать временных затрат.
- Не потокобезопасность: `ArrayDeque` не является потокобезопасной структурой данных. Если требуется использование `ArrayDeque` в многопоточной среде, необходимо обеспечить синхронизацию доступа к ней вручную.

В целом, `ArrayDeque` является хорошим выбором для многих задач, где требуется эффективное добавление, удаление и произвольный доступ к элементам. Однако, если вам требуется многопоточная безопасность или динамическое изменение размера без ограничений, возможно, стоит рассмотреть другие реализации, такие как `ConcurrentLinkedDeque`.

### Методы

- `boolean add(E e)`: Добавляет указанный элемент в конец дека.
- `void addFirst(E e)`: Вставляет указанный элемент в начало дека.
- `void addLast(E e)`: Вставляет указанный элемент в конец дека.
- `void clear()`: Удаляет все элементы из дека.
- `ArrayDeque<E> clone()`: Возвращает копию этого дека.
- `boolean contains(Object o)`: Возвращает true, если дек содержит указанный элемент.
- `Iterator<E> descendingIterator()`: Возвращает итератор по элементам дека в обратном порядке.
- `E element()`: Извлекает, но не удаляет, первый элемент очереди, представляемый деком.
- `E getFirst()`: Извлекает, но не удаляет, первый элемент дека. **ВАЖНО! Бросает NoSuchElementException!!!**
- `E getLast()`: Извлекает, но не удаляет, последний элемент дека.**ВАЖНО! Бросает NoSuchElementException!!!**
- `boolean isEmpty()`: Возвращает true, если дек не содержит элементов.
- `Iterator<E> iterator()`: Возвращает итератор по элементам дека.
- `boolean offer(E e)` - Вставляет указанный элемент в конец этой двусторонней очереди.
- `boolean offerFirst(E e)` - Вставляет указанный элемент в начало этой двусторонней очереди.
- `boolean offerLast(E e)` - Вставляет указанный элемент в конец этой двусторонней очереди.
- `E peek()` - Возвращает, но не удаляет, головной элемент очереди, представленной этой двусторонней очередью, или возвращает null, если эта очередь пуста.
- `E peekFirst()` - Возвращает, но не удаляет, первый элемент этой двусторонней очереди, или возвращает null, если эта очередь пуста.  
    ВАЖНО!  
    **Не бросает NoSuchElementException!!!**
- `E peekLast()` - Возвращает, но не удаляет, последний элемент этой двусторонней очереди, или возвращает null, если эта очередь пуста.
- ВАЖНО! **Не бросает NoSuchElementException!!!**
- `E poll()` - Извлекает и удаляет головной элемент очереди, представленной этой двусторонней очередью (то есть первый элемент этой очереди), или возвращает null, если эта очередь пуста.
- `E pollFirst()` - Извлекает и удаляет первый элемент этой двусторонней очереди, или возвращает null, если эта очередь пуста.
- `E pollLast()` - Извлекает и удаляет последний элемент этой двусторонней очереди, или возвращает null, если эта очередь пуста.
- `E pop()` - Извлекает элемент из стека, представленного этой двусторонней очередью. ВАЖНО! Б**росает NoSuchElementException!!!**
- `void push(E e)` - Помещает элемент в стек, представленный этой двусторонней очередью.
- `E remove()` - Извлекает и удаляет головной элемент очереди, представленной этой двусторонней очередью.
- `boolean remove(Object o)` - Удаляет один экземпляр указанного элемента из этой двусторонней очереди.
- `E removeFirst()` - Извлекает и удаляет первый элемент этой двусторонней очереди. ВАЖНО! Б**росает NoSuchElementException!!!**
- `boolean removeFirstOccurrence(Object o)` - Удаляет первое вхождение указанного элемента в этой двусторонней очереди (при обходе очереди от головы к хвосту).
- `E removeLast()` - Извлекает и удаляет последний элемент этой двусторонней очереди. **ВАЖНО! Бросает NoSuchElementException!!!**
- `boolean removeLastOccurrence(Object o)` - Удаляет последнее вхождение указанного элемента в этой двусторонней очереди (при обходе очереди от головы к хвосту).
- `int size()` - Возвращает количество элементов в этой двусторонней очереди.
- `Spliterator<E> spliterator()` - Создает "late-binding" (ленивую) и "fail-fast" (быстро обнаруживающую ошибки) Spliterator для элементов в этой двусторонней очереди.
- `Object[] toArray()` - Возвращает массив, содержащий все элементы этой двусторонней очереди в правильной последовательности (от первого до последнего элемента).
- `<T> T[] toArray(T[] a)` - Возвращает массив, содержащий все элементы этой двусторонней очереди в правильной последовательности (от первого до последнего элемента); тип возвращаемого массива определяется типом указанного массива.

# Interface Map<K,E>

Это интерфейс, который представляет отображение ключ-значение. Он определяет методы для взаимодействия с данными в карте.

[https://docs.oracle.com/javase/8/docs/api/java/util/Map.html](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)

### Классы имплементирующие эти интерфейсы

### class AbstractMap<K,V> implements Map<K,V>

### class HashMap<K,V>

В основе HashMap лежит массив(элементы массива часто называют бакетами, (то есть корзина). Элементами данного массива являются структуры LinkedList. Данные структуры LinkedList заполняются элементами, которые мы добавляем в HaashMap.


Плюсы:

- Быстрый доступ к элементам по ключу. `HashMap` использует хеш-таблицу, что позволяет выполнять операции `get` и `put` за время O(1) в большинстве случаев.
- Гибкость в использовании. `HashMap` позволяет хранить любые объекты в качестве ключей и значений, что делает его очень гибким при использовании.

Минусы:

- Отсутствие гарантии порядка элементов. `HashMap` не гарантирует порядок элементов в хранилище, что может быть проблемой, если порядок элементов имеет значение для приложения.
- Низкая производительность в случае коллизий. При возникновении коллизий `HashMap` необходимо выполнить дополнительную работу, чтобы разрешить коллизию, что может привести к снижению производительности.
- Использование большого объема памяти. `HashMap` использует хеш-таблицу, которая может занимать больше памяти, чем другие структуры данных. Кроме того, размер хеш-таблицы должен быть достаточно большим, чтобы уменьшить вероятность коллизий, что может привести к дополнительному использованию памяти.
- Не подходит для упорядоченных данных. `HashMap` не является подходящим выбором, если нужно хранить данные в упорядоченном виде, поскольку он не гарантирует порядок элементов.

### Методы

- `void clear()`Удаляет все отображения из данной карты.

```Java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        // Создаем экземпляр HashMap
        HashMap<String, Integer> hashMap = new HashMap<>();

        // Добавляем некоторые элементы в HashMap
        hashMap.put("A", 1);
        hashMap.put("B", 2);
        hashMap.put("C", 3);

        System.out.println("Исходная HashMap: " + hashMap);

        // Очищаем HashMap с помощью метода clear()
        hashMap.clear();

        System.out.println("После очистки HashMap: " + hashMap);
    }
}
Исходная HashMap: {A=1, B=2, C=3}
После очистки HashMap: {}
```

- `Object clone()`Возвращает поверхностную копию этого экземпляра HashMap: ключи и значения не клонируются.
- `V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`Пытается вычислить отображение для указанного ключа и его текущего сопоставленного значения (или null, если нет текущего отображения).

```Java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);

BiFunction<String, Integer, Integer> func = (key, value) -> value + 1;
map.compute("a", func); // обновит значение для ключа "a" на 2
map.compute("c", func); // не изменит отображение, так как ключа "c" нет в отображении
```

- `V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)`Если указанный ключ еще не связан со значением (или связан с null), пытается вычислить его значение с помощью данной функции отображения и вводит его в эту карту, если значение не null.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.computeIfPresent("one", (key, value) -> value + 1);
System.out.println(map); // Output: {one=2, two=2}

map.computeIfPresent("three", (key, value) -> value + 1);
System.out.println(map); // Output: {one=2, two=2}
```

- `V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`Если значение для указанного ключа присутствует и не равно null, пытается вычислить новое отображение с заданным ключом и его текущим отображенным значением.
- `boolean containsKey(Object key)`Возвращает true, если в этой карте присутствует отображение для указанного ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.containsKey("one")); // Output: true
System.out.println(map.containsKey("three")); // Output: false
```

- `boolean containsValue(Object value)`Возвращает true, если эта карта отображает один или несколько ключей на указанное значение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.containsValue(1)); // Output: true
System.out.println(map.containsValue(3)); // Output: false
```

- `Set<Map.Entry<K,V>> entrySet()`Возвращает представление набора отображений, содержащихся в этой карте.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
map.put("C", 3);

Set<Map.Entry<String, Integer>> entrySet = map.entrySet();

for (Map.Entry<String, Integer> entry : entrySet) {
    String key = entry.getKey();
    int value = entry.getValue();
    System.out.println(key + " -> " + value);
}

ВЫВОД:
A -> 1
B -> 2
C -> 3
```

- `void forEach(BiConsumer<? super K,? super V> action)`  
    Выполняет заданное действие для каждой записи в этой карте, пока все записи не будут обработаны или пока действие не вызовет исключение.  
    

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.forEach((key, value) -> System.out.println(key + " = " + value));
```

- `V get(Object key)`Возвращает значение, к которому отображен указанный ключ, или null, если в этой карте отсутствует отображение для ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.get("one")); // Output: 1
System.out.println(map.get("three")); // Output: null
```

- `V getOrDefault(Object key, V defaultValue)`Возвращает значение, к которому отображен указанный ключ, или defaultValue, если в этой карте отсутствует отображение для ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.getOrDefault("one", 0)); // Output: 1
System.out.println(map.getOrDefault("three", 0)); // Output: 0
```

- `boolean isEmpty()`Возвращает true, если в этой карте нет отображений ключ-значение.
- `Set<K> keySet()`Возвращает представление набора ключей, содержащихся в этой карте.`V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)`Если указанный ключ еще не связан со значением или связан с null, связывает его с заданным ненулевым значением.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("orange", 3);

Set<String> keys = map.keySet();
System.out.println("Keys: " + keys);

Keys: [orange, apple, banana]
```

- ``[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`` [`merge`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#merge-K-V-java.util.function.BiFunction-)`(`[`K`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) `key,` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) `value,` [`BiFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html)`<? super` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`,? super` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`,? extends` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`> remappingFunction)` позволяет объединить значение, связанное с указанным ключом, и новое значение, используя заданную функцию для выполнения операции слияния. В частности, если ключ уже присутствует в отображении, то новое значение будет добавлено к текущему значению с помощью заданной функции слияния. Если ключ не существует, то новое значение будет добавлено в отображение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);

// добавляем новое отображение с помощью merge()
map.merge("three", 3, (oldVal, newVal) -> oldVal + newVal);
System.out.println(map); // вывод: {one=1, two=2, three=3}

// обновляем существующее отображение с помощью merge()
map.merge("one", 5, (oldVal, newVal) -> oldVal + newVal);
System.out.println(map); // вывод: {one=6, two=2, three=3}
```

- `V put(K key, V value)`Связывает указанное значение с указанным ключом в этой карте.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.put("three", 3);
System.out.println(map); // Output: {one=1, two=2, three=3}
```

- `void putAll(Map<? extends K, ? extends V> m)` - копирует все отображения из указанного отображения в данное отображение.

```Java
HashMap<String, Integer> map1 = new HashMap<>();
map1.put("a", 1);
map1.put("b", 2);

HashMap<String, Integer> map2 = new HashMap<>();
map2.put("c", 3);
map2.put("d", 4);

map1.putAll(map2);
ВЫВОД {a=1, b=2, c=3, d=4}
```

- `V putIfAbsent(K key, V value)` - если указанный ключ еще не связан с каким-либо значением (или связан с `null`), то связывает его с указанным значением и возвращает `null`. Если ключ уже связан с каким-либо значением, то возвращает текущее значение.

```Java
import java.util.HashMap;
import java.util.Map;

public class Main {
  public static void main(String[] args) {
    Map<String, String> map = new HashMap<>();
    map.put("key1", "value1");

    String oldValue = map.putIfAbsent("key1", "new_value");
    System.out.println("Old value for key1: " + oldValue);
 // Output: Old value for key1: value1

    String newValue = map.putIfAbsent("key2", "value2");
    System.out.println("New value for key2: " + newValue);
 // Output: New value for key2: value2
  }
}
```

- `V remove(Object key)` - удаляет отображение для указанного ключа из этого отображения, если оно присутствует.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("orange", 2);
map.remove("apple"); // удаляем отображение для ключа "apple"
System.out.println(map); // выводит: {orange=2}
```

- `boolean remove(Object key, Object value)` - удаляет запись для указанного ключа только в том случае, если он в настоящее время связан с указанным значением.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("orange", 2);
map.remove("apple", 1); // не удаляем, так как значение для ключа "apple" не равно 1
map.remove("orange", 2); // удаляем, так как значение для ключа "orange" равно 2
System.out.println(map); // выводит: {apple=1}
```

- `V replace(K key, V value)` - заменяет запись для указанного ключа только в том случае, если он в настоящее время связан с каким-либо значением, и возвращает предыдущее значение, связанное с ключом.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

int oldValue = map.replace("apple", 20); // заменяем значение ключа "apple" на 20
System.out.println("Старое значение для ключа \"apple\": " + oldValue); // выведет "Старое значение для ключа "apple": 10"
System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 20"
```

- `boolean replace(K key, V oldValue, V newValue)` - заменяет запись для указанного ключа только в том случае, если он в настоящее время связан с указанным значением, и возвращает `true`. В противном случае он ничего не делает и возвращает `false`.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

boolean replaced = map.replace("apple", 10, 20); // заменяем значение ключа "apple" с 10 на 20
System.out.println("Значение ключа \"apple\" заменено? " + replaced); // выведет "Значение ключа "apple" заменено? true"
System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 20"
```

- `void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)` - заменяет значение каждого отображения на результат вызова данной функции для этой записи, пока не будут обработаны все записи или пока функция не выдаст исключение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

// увеличиваем значения каждого отображения на 5
map.replaceAll((key, value) -> value + 5);

System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 15"
System.out.println("Новое значение для ключа \"banana\": " + map.get("banana")); // выведет "Новое значение для ключа "banana": 10"
```

- `int size()` - возвращает количество отображений в данном отображении.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);

int size = map.size(); // size равен 3
```

- [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<`[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`> Collection<V> values()` - возвращает коллекцию, содержащую все значения в данном отображении.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);

Collection<Integer> values = map.values();
 // возвращает коллекцию [1, 2, 3]
```

### class LinkedHashMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>

LinkedHashMap является подклассом HashMap и имеет все преимущества и недостатки HashMap. Однако у LinkedHashMap есть дополнительные особенности:

Плюсы:

1. Порядок вставки: LinkedHashMap сохраняет порядок вставки элементов, что означает, что при обходе элементов они будут возвращаться в порядке, в котором они были вставлены.
2. Скорость доступа: LinkedHashMap поддерживает быстрый доступ к элементам, поскольку он использует хэш-таблицу для хранения данных, как и HashMap.
3. Итерация по ключам и значениям: LinkedHashMap позволяет легко итерироваться по его ключам и значениям, что делает его удобным для использования в циклах for-each.

Минусы:

1. Потребление памяти: LinkedHashMap потребляет больше памяти, чем обычный HashMap, поскольку он использует дополнительную связанные списки для сохранения порядка вставки элементов.
2. Сложность реализации: реализация LinkedHashMap сложнее, чем обычного HashMap, поскольку он должен поддерживать дополнительный список для хранения порядка вставки элементов.
3. Сложность вставки и удаления: при вставке и удалении элементов из LinkedHashMap может возникнуть дополнительное время на добавление или удаление элементов из списка порядка вставки.

### Методы

- `void clear()` - удаляет все отображения из данной карты.
- `boolean containsValue(Object value)` - возвращает `true`, если в этой карте отображается один или несколько ключей на указанное значение.
- `Set<Map.Entry<K, V>> entrySet()` - возвращает представление набора отображений, содержащихся в этой карте.
- `void forEach(BiConsumer<? super K, ? super V> action)`  
    - выполняет указанное действие для каждой записи в этой карте  
    
- `V get(Object key)` - возвращает значение, сопоставленное с указанным ключом в этой карте, или `null`, если данная карта не содержит отображение для ключа.
- `V getOrDefault(Object key, V defaultValue)` - возвращает  
    значение, сопоставленное с указанным ключом в этой карте, или  
    defaultValue, если данная карта не содержит отображение для ключа.  
    
- `Set<K> keySet()` - возвращает представление набора ключей, содержащихся в этой карте.
- `protected boolean removeEldestEntry(Map.Entry<K,V> eldest)` - возвращает `true`, если данная карта должна удалить свое самое старое отображение. Этот метод обычно вызывается методом `put` и описывается для реализации стратегии кэширования с фиксированным размером, при которой карта хранит только наиболее активно используемые отображения, а старые удаляются, когда количество отображений превышает определенный порог.

### class TreeMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>, [NavigableMap](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)<K,V>, [SortedMap](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)<K,V>

TreeMap в Java представляет собой отображение, которое хранит ключи в отсортированном порядке. Основным преимуществом TreeMap является то, что она хранит ключи в отсортированном порядке, что упрощает поиск и сравнение элементов. Однако у TreeMap есть и свои недостатки.

Плюсы TreeMap:

- Отображение хранит ключи в отсортированном порядке, что упрощает поиск и сравнение элементов.
- TreeMap позволяет использовать пользовательские объекты в качестве ключей, если они реализуют интерфейс Comparable или если задан компаратор.
- TreeMap предоставляет множество методов для работы с отсортированными данными, таких как методы для получения подмножеств отображения или для получения первого или последнего ключа в отсортированном порядке.

Минусы TreeMap:

- TreeMap может быть несколько медленнее, чем другие типы отображений, такие как HashMap, из-за необходимости сортировки ключей при каждом изменении отображения.
- Если пользовательские объекты используются в качестве ключей и не реализуют интерфейс Comparable или не задан компаратор, то при попытке использования TreeMap может возникнуть ошибка ClassCastException.
- TreeMap не является потокобезопасной, поэтому требует дополнительной синхронизации в многопоточной среде.

### Методы

- [`Map.Entry`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.Entry.html)`<`[`K`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)`,`[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)`> Map.Entry<K,V> ceilingEntry(K key)` - возвращает отображение ключ-значение, связанное с наименьшим ключом, который больше или равен заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> entry = treeMap.ceilingEntry(4);
        System.out.println(entry); // Вывод: 5=Orange
    }
}
```

- `K ceilingKey(K key)` - возвращает наименьший ключ, который больше или равен заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer ceilingKey = treeMap.ceilingKey(4);
        System.out.println(ceilingKey); // Вывод: 5
    }
}
```

- `void clear()` - удаляет все отображения из данного отображения.
- `Object clone()` - возвращает поверхностную копию этого экземпляра TreeMap.
- `Comparator<? super K> comparator()` - возвращает компаратор, используемый для упорядочивания ключей в этом отображении, или null, если это отображение использует естественное упорядочивание своих ключей.
- `boolean containsKey(Object key)` - возвращает true, если в этом отображении содержится отображение для указанного ключа.
- `boolean containsValue(Object value)` - возвращает true, если это отображение содержит один или несколько ключей, связанных с указанным значением.
- `NavigableSet<K> descendingKeySet()` - возвращает представление упорядоченного множества ключей в обратном порядке, содержащегося в этом отображении.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        NavigableSet<Integer> descendingKeySet = treeMap.descendingKeySet();
        System.out.println(descendingKeySet); // Вывод: [7, 5, 3, 1]
    }
}
```

- `NavigableMap<K,V> descendingMap()` - возвращает обратный порядок представления отображения для отображений, содержащихся в этом отображении.

```Java
import java.util.TreeMap;
import java.util.NavigableMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        NavigableMap<Integer, String> descendingMap = treeMap.descendingMap();
        for (Map.Entry<Integer, String> entry : descendingMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

7 - Mango
5 - Orange
3 - Banana
1 - Apple
```

- `Set<Map.Entry<K,V>> entrySet()` - возвращает представление отображения в виде набора отображений, содержащихся в данном отображении.

```Java
import java.util.TreeMap;
import java.util.Map;
import java.util.Set;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Set<Map.Entry<Integer, String>> entrySet = treeMap.entrySet();
        for (Map.Entry<Integer, String> entry : entrySet) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
1 - Apple
3 - Banana
5 - Orange
7 - Mango
```

- `Map.Entry<K,V> firstEntry()` - возвращает отображение ключ-значение, связанное с наименьшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> firstEntry = treeMap.firstEntry();
        System.out.println(firstEntry.getKey() + " - " + firstEntry.getValue()); // Вывод: 1 - Banana
    }
}

1 - Banana
```

- K `firstKey()` - возвращает первый (наименьший) ключ в этой карте.
- Map.Entry<K,V> `floorEntry(K key)` - возвращает отображение ключ-значение, связанное с наибольшим ключом, меньшим или равным заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> floorEntry = treeMap.floorEntry(4);
        System.out.println(floorEntry.getKey() + " - " + floorEntry.getValue()); // Вывод: 3 - Banana
    }
}

3 - Banana
```

- K `floorKey(K key)` - возвращает наибольший ключ, меньший или равный заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer floorKey = treeMap.floorKey(4);
        System.out.println(floorKey); // Вывод: 3
    }
}
```

- void `forEach(BiConsumer<? super K,? super V> action)` - выполняет заданное действие для каждой записи в этой карте, пока все записи не будут обработаны или действие не вызовет исключение.
- V `get(Object key)` - возвращает значение, сопоставленное с указанным ключом, или null, если в этой карте нет отображения для ключа.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        String value = treeMap.get(3);
        System.out.println(value); // Вывод: Banana
    }
}
```

- SortedMap<K,V> `headMap(K toKey)` - возвращает вид части этой карты, ключи которой строго меньше toKey.

```Java
import java.util.TreeMap;
import java.util.SortedMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        SortedMap<Integer, String> headMap = treeMap.headMap(5);
        for (Map.Entry<Integer, String> entry : headMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

1 - Apple
3 - Banana
```

- NavigableMap<K,V> `headMap(K toKey, boolean inclusive)` - возвращает вид части этой карты, ключи которой меньше (или равны, если inclusive равно true) toKey.

```Java
import java.util.TreeMap;
import java.util.NavigableMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        NavigableMap<Integer, String> headMapInclusive = treeMap.headMap(5, true);
        System.out.println("Inclusive:");
        for (Map.Entry<Integer, String> entry : headMapInclusive.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
Inclusive:
1 - Apple
3 - Banana
5 - Orange
        NavigableMap<Integer, String> headMapExclusive = treeMap.headMap(5, false);
        System.out.println("Exclusive:");
        for (Map.Entry<Integer, String> entry : headMapExclusive.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
Exclusive:
1 - Apple
3 - Banana
```

- Map.Entry<K,V> `higherEntry(K key)` - возвращает отображение ключ-значение, связанное с наименьшим ключом, строго большим заданного ключа, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> higherEntry = treeMap.higherEntry(4);
        System.out.println(higherEntry.getKey() + " - " + higherEntry.getValue()); // Вывод: 5 - Orange
    }
}

5 - Orange
```

- K `higherKey(K key)` - возвращает наименьший ключ, строго больший заданного ключа, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer higherKey = treeMap.higherKey(4);
        System.out.println(higherKey); // Вывод: 5
    }
}
```

- Set<\K> `keySet()` - возвращает вид множества ключей, содержащихся в этой карте.
- Map.Entry<K,V> `lastEntry()` - возвращает отображение ключ-значение, связанное с наибольшим ключом в этой карте, или null, если карта пуста.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> lastEntry = treeMap.lastEntry();
        System.out.println(lastEntry.getKey() + " - " + lastEntry.getValue()); // Вывод: 3 - Apple
    }
}

3 - Apple
```

- K `lastKey()` - возвращает последний (наибольший) ключ в этой карте.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Integer lastKey = treeMap.lastKey();
        System.out.println(lastKey); // Вывод: 3
    }
}
```

- `Map.Entry<K,V> lowerEntry(K key)`: возвращает отображение ключ-значение, связанное с наибольшим ключом, строго меньшим, чем заданный ключ, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> lowerEntry = treeMap.lowerEntry(4);
        System.out.println(lowerEntry.getKey() + " - " + lowerEntry.getValue()); // Вывод: 3 - Banana
    }
}

3 - Banana
```

- `K lowerKey(K key)`: возвращает наибольший ключ, строго меньший, чем заданный ключ, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        NavigableSet<Integer> navigableKeySet = treeMap.navigableKeySet();
        for (Integer key : navigableKeySet) {
            System.out.println(key);
        }
    }
}
1
2
3
```

- `NavigableSet<K> navigableKeySet()`: возвращает представление навигируемого набора ключей, содержащихся в данном отображении.
- `Map.Entry<K,V> pollFirstEntry()`: удаляет и возвращает отображение ключ-значение, связанное с наименьшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        NavigableSet<Integer> navigableKeySet = treeMap.navigableKeySet();
        for (Integer key : navigableKeySet) {
            System.out.println(key);
        }
    }
}

1
2
3
```

- `Map.Entry<K,V> pollLastEntry()`: удаляет и возвращает отображение ключ-значение, связанное с наибольшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> lastEntry = treeMap.pollLastEntry();
        if (lastEntry != null) {
            System.out.println(lastEntry.getKey() + " - " + lastEntry.getValue());
        } else {
            System.out.println("TreeMap is empty.");
        }
    }
}

3 - Apple
```

- `V put(K key, V value)`: связывает заданное значение с заданным ключом в этом отображении.
- `void putAll(Map<? extends K,? extends V> map)`: копирует все отображения из заданного отображения в данное отображение.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        // Создание и заполнение другого Map
        Map<Integer, String> otherMap = new TreeMap<>();
        otherMap.put(1, "Apple");
        otherMap.put(2, "Banana");
        otherMap.put(3, "Orange");

        // Добавление всех записей из otherMap в treeMap
        treeMap.putAll(otherMap);

        // Вывод treeMap
        for (Map.Entry<Integer, String> entry : treeMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
```

- `V remove(Object key)`: удаляет отображение ключ-значение для этого ключа из этого отображения, если он присутствует.
- `V replace(K key, V value)`: заменяет запись для указанного ключа только в том случае, если он сопоставлен с каким-то значением.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        Integer previousValue = treeMap.replace("Banana", 7);
        System.out.println("Previous value: " + previousValue); // Вывод: Previous value: 5

        Integer updatedValue = treeMap.get("Banana");
        System.out.println("Updated value: " + updatedValue); // Вывод: Updated value: 7
    }
}
```

- `boolean replace(K key, V oldValue, V newValue)`: заменяет запись для указанного ключа только в том случае, если он в данный момент сопоставлен с указанным значением.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        boolean replaced = treeMap.replace("Banana", 5, 7);
        System.out.println("Replaced: " + replaced); // Вывод: Replaced: true

        Integer updatedValue = treeMap.get("Banana");
        System.out.println("Updated value: " + updatedValue); // Вывод: Updated value: 7

        boolean notReplaced = treeMap.replace("Banana", 5, 9);
        System.out.println("Replaced: " + notReplaced); // Вывод: Replaced: false

        Integer unchangedValue = treeMap.get("Banana");
        System.out.println("Unchanged value: " + unchangedValue); // Вывод: Unchanged value: 7
    }
}
```

- `void replaceAll(BiFunction<? super K,? super V,? extends V> function)`: заменяет значение каждой записи результатом вызова данной функции на эту запись, пока все записи не будут обработаны или пока функция не выбросит исключение.

```Java
import java.util.TreeMap;
import java.util.function.BiFunction;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        BiFunction<String, Integer, Integer> function = (key, value) -> value * 2;

        treeMap.replaceAll(function);

        for (String key : treeMap.keySet()) {
            System.out.println(key + " - " + treeMap.get(key));
        }
    }
}

Apple - 6
Banana - 10
Orange - 4
```

- `int size()`: возвращает количество записей ключ-значение в этом отображении.
- `NavigableMap<K,V> subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive)`: возвращает представление части этого отображения, ключи которого находятся в диапазоне от fromKey до toKey.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        // Получение подмножества от ключа 3 (включительно) до ключа 7 (исключительно)
        NavigableMap<Integer, String> subMap = treeMap.subMap(3, true, 7, false);

        // Вывод записей подмножества
        for (Map.Entry<Integer, String> entry : subMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

3 - Banana
5 - Orange
Обратите внимание, что подмножество содержит записи
с ключами 3 и 5, включая их значения.
Запись с ключом 7 не включается в подмножество 
из-за параметра toInclusive, который установлен в false.
```

- `SortedMap<K,V> subMap(K fromKey, K toKey)`: возвращает представление части этого отображения, ключи которого находятся в диапазоне от fromKey (включительно) до toKey (исключительно).

```Java
import java.util.TreeMap;
import java.util.SortedMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);
        treeMap.put("Mango", 4);
        treeMap.put("Pineapple", 1);

        // Получение подмножества от ключа "Apple" до ключа "Mango"
        SortedMap<String, Integer> subMap = treeMap.subMap("Apple", "Mango");

        // Вывод записей подмножества
        for (String key : subMap.keySet()) {
            System.out.println(key + " - " + subMap.get(key));
        }
    }
}

Apple - 3
Banana - 5
Mango - 4
```

- `SortedMap<K,V> tailMap(K fromKey)`: возвращает подмножество `SortedMap`, содержащее все записи с ключами, начиная от `fromKey` и до конца `TreeMap`. Подмножество включает все записи с ключами, большими или равными `fromKey`.

```Java
import java.util.TreeMap;
import java.util.SortedMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        // Получение подмножества от ключа 5 (включительно) до конца TreeMap
        SortedMap<Integer, String> subMap = treeMap.tailMap(5);

        // Вывод записей подмножества
        for (Integer key : subMap.keySet()) {
            System.out.println(key + " - " + subMap.get(key));
        }
    }
}

5 - Orange
7 - Mango
9 - Pineapple
```

### class WeakHashMap

All Implemented Interfaces:[Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>

WeakHashMap является реализацией интерфейса Map в Java, где ключи являются слабыми ссылками. Вот некоторые плюсы и минусы использования WeakHashMap:

Плюсы:

1. Автоматическое удаление объектов: Основное преимущество WeakHashMap состоит в том, что он автоматически удаляет пары ключ-значение, когда ключи больше не используются в приложении. Если ключ становится недоступным для программы, то связанное с ним значение будет автоматически удалено, что помогает предотвратить утечку памяти и улучшить производительность.
2. Гибкость использования: WeakHashMap может быть полезным при реализации специальных кэшей, где требуется динамическое добавление и удаление элементов в зависимости от использования. Например, он может использоваться для кэширования ресурсов, таких как изображения, и автоматического освобождения памяти, когда ресурс больше не нужен.

Минусы:

1. Неустойчивость ключей: Слабые ссылки, используемые в WeakHashMap, могут быть удалены сборщиком мусора в любой момент, если нет сильных ссылок на них. Это может привести к тому, что некоторые пары ключ-значение будут удалены непредсказуемым образом, что может вызвать проблемы, если объекты в WeakHashMap используются важным образом в программе.
2. Производительность: Использование слабых ссылок в WeakHashMap может иметь некоторое влияние на производительность. Сравнительно с другими реализациями Map, WeakHashMap требует дополнительных проверок и операций сборки мусора, чтобы управлять слабыми ссылками. Это может вызвать небольшое снижение производительности в сравнении с обычными HashMap или LinkedHashMap.
3. Отсутствие гарантированного порядка элементов: WeakHashMap не гарантирует порядок элементов в своей коллекции. Это означает, что порядок обхода элементов может быть непредсказуемым, что может быть проблематично, если важен порядок элементов.

### class IdentityHashMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>

IdentityHashMap является реализацией интерфейса Map в Java, где сравнение ключей происходит на основе их идентичности (==), а не на основе метода `equals()`. Вот некоторые плюсы и минусы использования IdentityHashMap:

Плюсы:

1. Уникальность ключей: IdentityHashMap позволяет использовать ключи, которые имеют одинаковое содержимое, но разные идентичности. Это полезно в ситуациях, когда требуется сравнивать ключи на основе ссылок, а не их содержимого. Например, это может быть полезно при работе с экземплярами классов, где `equals()` переопределен для сравнения содержимого, но требуется уникальность по ссылке.
2. Производительность: Поскольку сравнение ключей осуществляется на основе их идентичности, а не метода `equals()`, IdentityHashMap может работать быстрее, особенно если в коллекции содержится большое количество ключей с одинаковым содержимым, но разной идентичностью.
3. Отсутствие необходимости в переопределении `equals()`: В отличие от других реализаций Map, где для сравнения ключей требуется переопределение метода `equals()`, в IdentityHashMap можно использовать любые объекты в качестве ключей без необходимости переопределения `equals()`.

Минусы:

1. Ограниченная гибкость: Использование идентичности для сравнения ключей может быть ограничивающим, особенно если требуется использовать разные объекты с одинаковым содержимым в качестве ключей. В таких случаях IdentityHashMap не подойдет, и лучше использовать другие реализации Map.
2. Ограниченная поддержка для `null` в качестве ключей: В IdentityHashMap нельзя использовать `null` в качестве ключа, так как для сравнения ключей используется оператор `==`. Если важно использовать `null` в качестве ключа, следует выбрать другую реализацию Map.
3. Неупорядоченность элементов: IdentityHashMap не гарантирует порядок элементов в своей коллекции. Это означает, что порядок обхода элементов может быть непредсказуемым, что может быть проблематичным, если важен порядок элементов.

### class EnumMap<K extends Enum\<K>,V>

EnumMap является специализированной реализацией интерфейса Map в Java, предназначенной для использования с перечислениями (enum). Она использует ключи, ограниченные заданным перечислением. Вот некоторые плюсы и минусы использования EnumMap:

Плюсы:

1. Эффективность и производительность: EnumMap реализована в виде массива, где индексы массива соответствуют значениям перечисления. Это делает операции чтения и записи в EnumMap очень быстрыми, поскольку доступ к элементам осуществляется непосредственно через индексы массива.
2. Оптимизация памяти: EnumMap использует массив фиксированного размера, основанный на количестве значений в перечислении. Это позволяет эффективно использовать память, особенно если перечисление имеет небольшое количество значений.
3. Гарантированный порядок элементов: EnumMap сохраняет порядок элементов, соответствующий порядку объявления значений в перечислении. Это полезно, если требуется обход элементов в определенном порядке.
4. Безопасность типов: EnumMap является типобезопасной реализацией Map, так как она ограничена конкретным перечислением в качестве ключей. Это помогает избежать ошибок типизации при работе с картой.

Минусы:

1. Ограничение использования только с перечислениями: EnumMap может использоваться только с перечислениями в качестве ключей. Если требуется использовать другие типы ключей, EnumMap не подойдет для таких случаев.
2. Неизменяемый размер: Размер EnumMap задается при создании на основе количества значений в перечислении, и его невозможно изменить после этого. Если требуется динамическое изменение размера карты, EnumMap может быть не подходящим выбором.
3. Отсутствие поддержки для `null` в качестве ключей: EnumMap не позволяет использовать `null` в качестве ключа, так как ключи ограничены перечислением. Если важно использовать `null` в качестве ключа, следует выбрать другую реализацию Map.

### class ConcurrentSkipListMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [ConcurrentMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html)<K,V>, [ConcurrentNavigableMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentNavigableMap.html)<K,V>, [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>, [NavigableMap](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)<K,V>, [SortedMap](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)<K,V>

`ConcurrentSkipListMap` является реализацией `NavigableMap` в Java, которая обеспечивает потокобезопасность при одновременном доступе из нескольких потоков. Вот некоторые преимущества и недостатки `ConcurrentSkipListMap`:

Плюсы:

1. Потокобезопасность: `ConcurrentSkipListMap` обеспечивает безопасность потоков при одновременном доступе из нескольких потоков. Это означает, что вы можете использовать `ConcurrentSkipListMap` без необходимости дополнительной синхронизации или блокировки.
2. Относительная производительность: `ConcurrentSkipListMap` обеспечивает эффективный доступ к данным и выполнение операций вставки, удаления и поиска даже в многопоточной среде. Она строится на основе пропускающего списка (skip list), что позволяет достичь высокой производительности при выполнении операций над картой.
3. Упорядоченность: `ConcurrentSkipListMap` является упорядоченной картой, что означает, что элементы будут храниться в отсортированном порядке. Она поддерживает операции, связанные с навигацией по карте, такие как получение первого или последнего элемента, поиск элемента, ближайшего к определенному значению и т. д.

Минусы:

1. Потребление памяти: `ConcurrentSkipListMap` использует структуру данных skip list для обеспечения производительности и потокобезопасности. Skip list требует дополнительной памяти для хранения указателей на разные уровни списка, что может привести к более высокому потреблению памяти по сравнению с другими структурами данных.
2. Сложность кода: Использование `ConcurrentSkipListMap` требует более внимательного подхода к написанию кода. Это может быть сложно для разработчиков, не знакомых с концепцией пропускающего списка или не привыкших к работе с потокобезопасными структурами данных.
3. Отсутствие поддержки изменяемых операций: `ConcurrentSkipListMap` не поддерживает изменяемые операции на итераторе, такие как удаление элементов, во избежание проблем с потокобезопасностью. Это может быть недостатком в случаях, когда требуется модифицировать карту в процессе итерации.

### class ConcurrentHashMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [ConcurrentMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html)<K,V>, [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>

Класс `ConcurrentHashMap` является потокобезопасной реализацией интерфейса `Map`. Он обеспечивает доступ к своим методам несколькими потоками одновременно, без необходимости блокировки всей карты. Вместо этого он использует механизмы синхронизации, которые позволяют нескольким потокам обновлять отображение одновременно, что повышает производительность.

Плюсы:

- Потокобезопасность: `ConcurrentHashMap` предоставляет механизмы синхронизации, которые позволяют нескольким потокам обновлять отображение одновременно без блокировки всей карты.
- Высокая производительность: благодаря своей реализации, `ConcurrentHashMap` может обрабатывать несколько запросов одновременно, что увеличивает производительность при работе с большим количеством потоков.
- Атомарные операции: `ConcurrentHashMap` поддерживает атомарные операции, такие как `putIfAbsent()`, `remove()`, `replace()` и другие, которые выполняются без блокировки всей карты.
- Широкие возможности настройки: `ConcurrentHashMap` предоставляет ряд параметров настройки, таких как начальная емкость, коэффициент загрузки и параллелизм, что позволяет настроить его под конкретные потребности приложения.

Минусы:

- Потребление памяти: `ConcurrentHashMap` потребляет больше памяти, чем обычная `HashMap`, из-за использования механизмов синхронизации.
- Сложность реализации: из-за сложности механизмов синхронизации, реализация `ConcurrentHashMap` может быть сложнее, чем у других реализаций `Map`.
- Невозможность обеспечения строгой семантики транзакций: `ConcurrentHashMap` не поддерживает строгую семантику транзакций, что может быть проблемой при реализации некоторых приложений.

### class Hashtable<K,V> extends java.util.Dictionary<K,V>

Устаревший снихронизированный класс типа Map<K,V> лучше использовать ConcurrentHashMap

# Class java.util.Collections

[https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html](https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html)

Это утилитный класс, предоставляющий набор методов для работы с коллекциями. В нем содержатся методы для сортировки, перемешивания, копирования, поиска элементов и другие методы, упрощающие работу с коллекциями.

### Fields

- `static` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) [`EMPTY_LIST`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_LIST)
- `static` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) [`EMPTY_MAP`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_MAP)
- `static` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html) [`EMPTY_SET`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_SET)

### Методы

- `static <T> boolean addAll(Collection<? super T> c, T... elements)`: Добавляет все указанные элементы в указанную коллекцию.

```Java
List<String> list = new ArrayList<>();
Collections.addAll(list, "foo", "bar", "baz");
System.out.println(list); // Output: [foo, bar, baz]
```

- `static <T>` [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)`<T> asLifoQueue(Deque<T> deque)`: Возвращает представление Deque в виде очереди Last-in-first-out (Lifo).

```Java
Deque<Integer> deque = new ArrayDeque<>();
deque.push(1);
deque.push(2);
deque.push(3);
Queue<Integer> lifoQueue = Collections.asLifoQueue(deque);
System.out.println(lifoQueue.poll()); // Вывод: 3
System.out.println(lifoQueue.poll()); // Вывод: 2
System.out.println(lifoQueue.poll()); // Вывод: 1
```

- `static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`: Ищет указанный объект в указанном списке с использованием алгоритма бинарного поиска.
- `static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)`: Ищет указанный объект в указанном списке с использованием алгоритма бинарного поиска и сравнивает элементы с помощью указанного компаратора.
- `static <E>` [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<E> checkedCollection(Collection<E> c, Class<E> type)`: Возвращает динамически типизированное представление указанной коллекции.
- `static <E>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<E> checkedList(List<E> list, Class<E> type)`: Возвращает динамически типизированное представление указанного списка.

```Java
List<Student> students = new ArrayList<>();
List<Student> checkedStudents = Collections.checkedList(students, Student.class);
Теперь при попытке добавления объекта,
который не является типом Student в список checkedStudents,
будет сгенерировано исключение ClassCastException.
```

- `static <K,V>` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,V> checkedMap(Map<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной карты.
- `static <K,V>` [`NavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)`<K,V> checkedNavigableMap(NavigableMap<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной упорядоченной карты.
- `static <E>` [`NavigableSet`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)`<E> checkedNavigableSet(NavigableSet<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного упорядоченного набора.
- `static <E>` [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)`<E> checkedQueue(Queue<E> queue, Class<E> type)`: Возвращает динамически типизированное представление указанной очереди.
- `static <E>` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)`<E> checkedSet(Set<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного набора.
- `static <K,V>` [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)`<K,V> checkedSortedMap(SortedMap<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной упорядоченной карты.
- `static <E>` [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)`<E> checkedSortedSet(SortedSet<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного упорядоченного набора.
- `static <T> void copy(List<? super T> dest, List<? extends T> src)`: Копирует все элементы из одного списка в другой.

```Java
List<Integer> srcList = Arrays.asList(1, 2, 3);
List<Number> destList = new ArrayList<>(Arrays.asList(0, 0, 0));

Collections.copy(destList, srcList);

System.out.println(destList); // Output: [1, 2, 3]
```

- `static boolean disjoint(Collection<?> c1, Collection<?> c2)`: Возвращает true, если две указанные коллекции не имеют общих элементов.

```Java
List<Integer> list1 = Arrays.asList(1, 2, 3);
List<Integer> list2 = Arrays.asList(4, 5, 6);

if (Collections.disjoint(list1, list2)) {
    System.out.println("Коллекции не имеют общих элементов"); // сработает это
} else {
    System.out.println("Коллекции имеют общие элементы"); 
}
```

- `static <T>` [`Enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)`<T> emptyEnumeration()`: Возвращает перечисление, которое не имеет элементов.
- `static <T>` [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`<T> emptyIterator()`: Возвращает итератор, который не имеет элементов.
- `static <T>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T> emptyList()`: Возвращает пустой список (неизменяемый).
- `static <T>` [`ListIterator`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html)`<T> emptyListIterator()`: Возвращает итератор списка без элементов.
- `static <K,V>` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,V> emptyMap()`: Возвращает пустую карту (неизменяемую).
- `static <K,V>` [`NavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)`<K,V> emptyNavigableMap()`: Возвращает пустую навигационную карту (неизменяемую).
- `static <E>` [`NavigableSet`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)`<E> emptyNavigableSet()`: Возвращает пустой навигационный набор (неизменяемый).
- `static <T>` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)`<T> emptySet()`: Возвращает пустой набор (неизменяемый).
- `static <K,V>` [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)`<K,V> emptySortedMap()`: Возвращает пустую отсортированную карту (неизменяемую).
- `static <E>` [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)`<E> emptySortedSet()`: Возвращает пустой отсортированный набор (неизменяемый).
- `static <T>` [`Enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)`<T> enumeration(Collection<T> c)`: Возвращает перечисление по указанной коллекции.
- `static <T> void fill(List<? super T> list, T obj)`: Заменяет все элементы указанного списка указанным элементом.

```Java
List<String> list = new ArrayList<>(Arrays.asList("foo", "bar", "baz"));
Collections.fill(list, "hello");
System.out.println(list); // выведет [hello, hello, hello]
```

- `static int frequency(Collection<?> c, Object o)`: Возвращает количество элементов в указанной коллекции, равных указанному объекту.
- `static int indexOfSubList(List<?> source, List<?> target)`: Возвращает начальную позицию первого вхождения указанного целевого списка в указанном исходном списке, или -1, если такое вхождение отсутствует.

```Java
List<Integer> source = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> target = Arrays.asList(3, 4, 5);

int index = Collections.indexOfSubList(source, target);

System.out.println(index); // Output: 2
```

- `static int lastIndexOfSubList(List<?> source, List<?> target)`: Возвращает начальную позицию последнего вхождения указанного целевого списка в указанном исходном списке, или -1, если такое вхождение отсутствует.
- `static <T>` [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`<T> list(Enumeration<T> e)`: Возвращает список ArrayList, содержащий элементы, возвращаемые указанным перечислением в порядке, в котором они возвращаются перечислением.
- `static <T extends` [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `&` [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)`<? super T>> T max(Collection<? extends T> coll)`: Возвращает максимальный элемент данной коллекции в соответствии с естественным порядком элементов.
- `static <T> T max(Collection<? extends T> coll, Comparator<? super T> comp)`: Возвращает максимальный элемент данной коллекции в соответствии с порядком, вызываемым указанным компаратором.
- `static <T extends` [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `&` [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)`<? super T>> T min(Collection<? extends T> coll)`: Возвращает минимальный элемент данной коллекции в соответствии с естественным порядком элементов.
- `static <T> T min(Collection<? extends T> coll, Comparator<? super T> comp)`: Возвращает минимальный элемент данной коллекции в соответствии с порядком, вызываемым указанным компаратором.
- `static <T>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T> nCopies(int n, T o)`: Возвращает неизменяемый список, состоящий из n копий указанного объекта.

```Java
List<String> list = Collections.nCopies(3, "Hello");
System.out.println(list); // Output: [Hello, Hello, Hello]
```

- `static <E> Set<E> newSetFromMap(Map<E,Boolean> map)` - возвращает новый `Set` с поддержкой предоставленного `Map`. В `Set` будут содержаться все ключи, которые имеют `Boolean` значение `true` в указанной `Map`. Это означает, что при добавлении ключа в этот `Set`, его значение в `Map` будет установлено в `true`, а при удалении ключа из `Set`, его значение в `Map` будет установлено в `false`.

```Java
Map<String, Boolean> map = new HashMap<>();
Set<String> set = Collections.newSetFromMap(map);

set.add("foo");
set.add("bar");

System.out.println(map); // выводит {bar=true, foo=true}

set.remove("foo");

System.out.println(map); // выводит {bar=true, foo=false}
```

- `static <T> boolean replaceAll(List<T> list, T oldVal, T newVal)` - заменяет все вхождения одного заданного значения в списке на другое.

```Java
List<String> list = new ArrayList<>(Arrays.asList("apple", "banana", "cherry"));
System.out.println("Before replaceAll: " + list); // [apple, banana, cherry]
Collections.replaceAll(list, "banana", "orange");
System.out.println("After replaceAll: " + list); // [apple, orange, cherry]
```

- `static void reverse(List<?> list)` - переворачивает порядок элементов в указанном списке.

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);
        
        System.out.println("Original list: " + list);
        
        Collections.reverse(list);
        
        System.out.println("Reversed list: " + list);
    }
}
Original list: [1, 2, 3, 4, 5]
Reversed list: [5, 4, 3, 2, 1]
```

- `static <T> Comparator<T> reverseOrder()` - возвращает компаратор, который накладывает обратный порядок естественного упорядочивания на коллекцию объектов, реализующих интерфейс Comparable.
- `static <T> Comparator<T> reverseOrder(Comparator<T> cmp)` - возвращает компаратор, который накладывает обратный порядок указанного компаратора.

```Java
List<Integer> numbers = Arrays.asList(5, 3, 8, 1, 9, 2);
Comparator<Integer> reverseComparator = Collections.reverseOrder();
Collections.sort(numbers, reverseComparator);
System.out.println(numbers); // [9, 8, 5, 3, 2, 1]
```

- `static void rotate(List<?> list, int distance)` - перемещает элементы в указанном списке на указанное расстояние.

```Java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
System.out.println("Before rotation: " + numbers);

Collections.rotate(numbers, 2);
System.out.println("After rotation: " + numbers);

Collections.rotate(numbers, -2);
System.out.println("After rotation: " + numbers);

Before rotation: [1, 2, 3, 4, 5]
After rotation: [4, 5, 1, 2, 3]
After rotation: [1, 2, 3, 4, 5]
```

- `static void shuffle(List<?> list)` - случайным образом переставляет указанный список, используя по умолчанию источник случайности.
- `static void shuffle(List<?> list, Random rnd)` - случайным образом переставляет указанный список, используя указанный источник случайности.

```Java
import java.util.*;

public class ShuffleExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            numbers.add(i);
        }

        System.out.println("Original List: " + numbers);

        Collections.shuffle(numbers);

        System.out.println("Shuffled List: " + numbers);
    }
}

Original List: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Shuffled List: [3, 6, 2, 4, 9, 8, 1, 10, 5, 7]
```

- `static <T> Set<T> singleton(T o)` - возвращает неизменяемый набор, содержащий только указанный объект.
- `static <T> List<T> singletonList(T o)` - возвращает неизменяемый список, содержащий только указанный объект.
- `static <K,V> Map<K,V> singletonMap(K key, V value)` - возвращает неизменяемое отображение, отображающее только указанный ключ на указанное значение. Попытка добавления новых элементов в созданный методом `singletonMap` `Map` приведет к `UnsupportedOperationException`, так как возвращаемый `Map` является неизменяемым.

```Java
Map<String, Integer> map = Collections.singletonMap("a", 1);
System.out.println(map); // выводит: {a=1}
```

- `static <T extends Comparable<? super T>> void sort(List<T> list)` - сортирует указанный список в порядке возрастания, в соответствии с естественным упорядочиванием его элементов.
- `static <T> void sort(List<T> list, Comparator<? super T> c)` - сортирует указанный список в соответствии с порядком, вызываемым указанным компаратором.

```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 40));
        
        Comparator<Person> byAgeDescending = (p1, p2) -> p2.getAge() - p1.getAge();
        Collections.sort(people, byAgeDescending);
        
        System.out.println(people); // [Charlie (40), Alice (30), Bob (25)]
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```

- `static void swap(List<?> list, int i, int j)` - меняет местами элементы на указанных позициях в указанном списке.

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");

        System.out.println(list); // [a, b, c]
        Collections.swap(list, 0, 2);
        System.out.println(list); // [c, b, a]
    }
}
```

- `static <T> Collection<T> synchronizedCollection(Collection<T> c)` - возвращает синхронизированную (потокобезопасную) коллекцию, поддерживаемую указанной коллекцией.
- `static <T> List<T> synchronizedList(List<T> list)` - возвращает синхронизированный (потокобезопасный) список, поддерживаемый указанным списком.
- `static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)` - возвращает синхронизированную (thread-safe) map, которая поддерживается указанной map.
- `static <K,V> NavigableMap<K,V> synchronizedNavigableMap(NavigableMap<K,V> m)` - возвращает синхронизированную (thread-safe) NavigableMap, которая поддерживается указанной NavigableMap.
- `static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)` - возвращает синхронизированную (thread-safe) NavigableSet, которая поддерживается указанным NavigableSet.
- `static <T> Set<T> synchronizedSet(Set<T> s)` - возвращает синхронизированный (thread-safe) set, который поддерживается указанным set.
- `static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)` - возвращает синхронизированный (thread-safe) sorted map, который поддерживается указанным sorted map.
- `static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)` - возвращает синхронизированный (thread-safe) sorted set, который поддерживается указанным sorted set.
- `static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c)` - возвращает неизменяемое представление указанной коллекции.
- `static <T> List<T> unmodifiableList(List<? extends T> list)` - возвращает неизменяемое представление указанного списка.

```Java
List<Person> persons = Collections.unmodifiableList(new ArrayList<Person>());
persons.add(new Person("Vadim",1000));
//Exception in thread "main" java.lang.UnsupportedOperationException
```

- `static <K,V> Map<K,V> unmodifiableMap(Map<? extends K,? extends V> m)` - возвращает неизменяемое представление указанной map.
- `static <K,V> NavigableMap<K,V> unmodifiableNavigableMap(NavigableMap<K,? extends V> m)` - возвращает неизменяемое представление указанной NavigableMap.
- `static <T> NavigableSet<T> unmodifiableNavigableSet(NavigableSet<T> s)` - возвращает неизменяемое представление указанного NavigableSet.
- `static <T> Set<T> unmodifiableSet(Set<? extends T> s)` - возвращает неизменяемое представление указанного set.
- `static <K,V> SortedMap<K,V> unmodifiableSortedMap(SortedMap<K,? extends V> m)` - возвращает неизменяемое представление указанной sorted map.
- `static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<T> s)` - возвращает неизменяемое представление указанного sorted set.