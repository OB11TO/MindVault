---
title: Generics
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 13:39
modified: 2024-08-31T13:47:25+03:00
difficulty: 
questions: 
notes: 
links:
  - https://javarush.com/quests/lectures/jru.questcollections.level05.lecture09
---
# Generics

## Raw type

Проблема связанная до появления Generics представлена в коде ниже, невозможно гарантировать, что в список добавят элементы, которые принадлежат одному типу

```java
ArrayList stringList = new ArrayList();
stringList.add("abc"); //добавляем строку в список
stringList.add("abc"); //добавляем строку в список
stringList.add( 1 ); //добавляем число в список

for(Object o: stringList)
{
 String s = (String) o; //тут будет ClassCastException,
// когда дойдем до элемента-числа
}

```

Для решения данной проблемы приходилось проверять на принадлежность к типу:

```java
for (Object o : stringList) {
            if(o instanceof String){
                String s = ((String) o).toUpperCase();
            }
            else if(o instanceof Integer){
                String s = String.valueOf(o).toUpperCase();
            }
        }
```

Как решают данную проблему Generics, типизируя список, мы разрешаем класть в него в данном случае только экземпляры класса String:

```java
ArrayList<String> stringList = new ArrayList<String>();
stringList.add("abc"); //добавляем строку в список
stringList.add("abc"); //добавляем строку в список

stringList.add( 1 );**//тут будет ошибка компиляции**

for(Object o: stringList)
{
 String s = (String) o;
}

Что происходит под капотом:

List strings = new ArrayList();

strings.add((String)"abc");
strings.add((String)"abc");

strings.add((String) 1); //ошибка компиляции

for(String s: strings)
{
 System.out.println(s);
}
```

## Стирание типов

Внутри класса-generic’а не хранится никакой информации о его типе-параметре:

```java
class Zoo<T>
{
 ArrayList<T> pets = new ArrayList<T>();

 public T createAnimal()
 {
  T animal = new T();
  pets.add(animal)
  return animal;
 }
}
```

```java
class Zoo
{
 ArrayList pets = new ArrayList();

 public Object createAnimal()
 {
  Object animal = new ???();
  pets.add(animal)
  return animal;
 }
}
```

При компиляции, все параметры типов (в данном случае `T`) стираются и заменяются на тип `Object`. Это означает, что, несмотря на то, что в исходном коде мы работаем с параметризованными типами, в скомпилированном коде мы работаем с обычными объектами `Object`. Этот процесс называется стиранием типов.

Таким образом, стирание типов позволяет Java сохранять обратную совместимость с более старыми версиями языка, где Generics не были поддерживаемы. Однако, это ограничивает возможности работы с параметризованными типами на уровне компиляции и требует использования различных механизмов для обхода ограничений стирания типов, таких как wildcard-аргументы, reflection

### Как обойти стирание типов через Reflection API

Если бы мы просто пользовались Class вместо Class<Т>, то кто-то мог по ошибке передать туда два разных типа – один в качестве параметра T, другой – в конструктор.

```java
public class Zoo<T> {
    List<T> animalList = new ArrayList<>();
    Class<T> clazz;
    Zoo(Class<T> clazz){
    this.clazz = clazz;
    }
    public T createNewAnimal(){
        T animal = null;
        try {
            animal = clazz.newInstance();
        } catch (InstantiationException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
        animalList.add(animal);
        return animal;
    }
}

public static void main(String[] args) {
	Zoo<Tiger> zoo = new Zoo<>(Tiger.class);
  Tiger tiger = zoo.createNewAnimal();
    }
```

## WildCards

Wildcards — это специальный механизм языка Java Generics, который позволяет определить параметры типов нестрого и динамически. Они используются для обозначения типов, которые неизвестны в момент написания кода, но могут быть определены в момент выполнения.

Существует три разновидности Wildcards:

- `? extends T`: ограничивает тип сверху (upper bounded wildcard). Этот wildcard принимает все типы, которые являются подтипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? extends Animal` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Animal` или его наследников.

```java
public static void printAnimals(List<? extends Animal> animals) {
    for (Animal a : animals) {
        System.out.println(a.getName());
    }
}
```

- `? super T`: ограничивает тип снизу (lower bounded wildcard). Этот wildcard принимает все типы, которые являются супертипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? super Dog` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Dog` или его родительских классов. Наследники класса Dog передавать в список нельзя.

```java
public static void addDog(List<? super Dog> animals) {
    animals.add(new Dog("Buddy"));
}
```

- `?`: неограниченный wildcard (unbounded wildcard). Этот wildcard может принимать любой тип.

```java
public static void addTotListAndPrint(List<?> list) {
list.add(new String("Hello)";

list.add(new Boolean(true);

    for (Object o : list) {
        System.out.println(o);
    }
}
```

### Проблема, которую решает extends

В данном примере нельзя передать List\<Archer> или List\<Magician>\в метод, который принимает List\<Warrior>\, хотя эти классы наследники Warrior.

Потому что List\<Archer> не наследник List\<Warrior>.

```java
public  abstract  class Warrior {
}

public class Magician extends Warrior{}

public class Archer extends Warrior{}
```

```java
public static void main(String[] args) {

List<Archer> archers = new ArrayList<>();
archers.add(new Archer());
List<Magician> magicians = new ArrayList<>();
magicians.add(new Magician());

boolean isFirstTeamCooler = teamFight(archers,magicians);
}  //ошибка компиляции

public static boolean teamFight(List<Warrior> firstTeam,
 List<Warrior> secondTeam) {
if (firstTeam.size() > secondTeam.size()) return true;
else return false;
}
}
```

Решается с помощью extends:

```java
public static boolean teamFight(List<? extends Warrior> firstTeam,
 List<? extends Warrior> secondTeam) {
        if (firstTeam.size() > secondTeam.size()) return true;
        else return false;
    }
```

### Проблема, которую решает super

```java
public void do(List<?extends Myclass list){
for(Myclass object : list){
sout(object.getState());  // тут все работает
list.add(new Myclass); // тут сломается
}
```

Потому что в качестве аргумента в функцию может прилететь не List\<MyClass>, a List наследник MyClass, и в него уже передать родителя нельзя.

Чтобы решить эту проблему необходимо использовать super, так как тогда в функцию можно передать либо List\<MyClass> либо список родителя MyClass.

```java
public void do(List<?super Myclass list){
for(Myclass object : list){
sout(object.getState());  // тут все работает
list.add(new Myclass); // тут сломается
}
```

Но сломается этот код:

```java
public void do(List<?super Myclass list){
for(Myclass object : list){ // тут сломается

list.add(new Myclass); // тут работает
}
```

Потому что в качестве аргумента в функцию может поступить класс Object, и перебор по MyClass невозможен.
