---
title: Generics
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 13:39
modified: 2024-09-04T13:44:35+03:00
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

for(Object o: stringList) {
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

При компиляции, все параметры типов (в данном случае `T`) стираются и заменяются на тип `Object`. Это означает, что, <mark class="hltr-yellow">несмотря на то, что в исходном коде мы работаем с параметризованными типами, в скомпилированном коде мы работаем с обычными объектами </mark>`Object`. <mark class="hltr-red">Этот процесс называется стиранием типов.</mark>

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

<mark class="hltr-red">Wildcards</mark> — э<mark class="hltr-yellow">то специальный механизм языка Java Generics, который позволяет определить параметры типов нестрого и динамически.</mark> Они используются для обозначения типов, которые неизвестны в момент написания кода, но могут быть определены в момент выполнения.

<mark class="hltr-purple">Существует три разновидности Wildcards:</mark>

- `? extends T`: <mark class="hltr-yellow">ограничивает тип сверху</mark> (upper bounded wildcard). Этот wildcard принимает все типы, которые являются подтипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? extends Animal` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Animal` или его наследников.

```java
public static void printAnimals(List<? extends Animal> animals) {
    for (Animal a : animals) {
        System.out.println(a.getName());
    }
}
```

- `? super T`: <mark class="hltr-yellow">ограничивает тип снизу</mark> (lower bounded wildcard). Этот wildcard принимает все типы, которые являются супертипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? super Dog` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Dog` или его родительских классов. Наследники класса Dog передавать в список нельзя.

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

Потому что в качестве аргумента в <mark class="hltr-yellow">функцию может прилететь не</mark> List\<MyClass>, <mark class="hltr-yellow">a List наследник MyClass, и в него уже передать родителя нельзя.</mark>

Чтобы решить эту проблему <mark class="hltr-yellow">необходимо использовать super</mark>, так как тогда в функцию можно передать либо List\<MyClass> либо список родителя MyClass.

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

<mark class="hltr-yellow">Потому что в качестве аргумента в функцию может поступить класс Object, и перебор по MyClass невозможен.</mark>




![[images/Untitled 159.png|Untitled 159.png]]

![[images/Untitled 1 20.png|Untitled 1 20.png]]

![[images/Untitled 2 18.png|Untitled 2 18.png]]

![[images/Untitled 3 17.png|Untitled 3 17.png]]

### Наследование и расширители обобщений 

  
![[images/Untitled 4 17.png|Untitled 4 17.png]]

- Параметризация на уровне метода. (Такая же как у класса).
- Какой дженерик у `Hero<World>` значит можно передать людей `Class extends Herro`, но чтобы у этого класса был `<World>`
- Параметризация на уровне метода. \<T> решает эту проблему

### Wildcard

![[images/Untitled 5 17.png|Untitled 5 17.png]]

- Не так удобно оперировать, как с параметризацией на уровне метода
- Consumer -super / Produser - extends

  

  

![[images/Untitled 6 16.png|Untitled 6 16.png]]

![[images/Untitled 7 15.png|Untitled 7 15.png]]

Вопрос про 3 тип

![[images/Untitled 8 14.png|Untitled 8 14.png]]

![[images/Untitled 9 14.png|Untitled 9 14.png]]

![[images/Untitled 10 12.png|Untitled 10 12.png]]

![[images/Untitled 11 12.png|Untitled 11 12.png]]

![[images/Untitled 12 12.png|Untitled 12 12.png]]

  

![[images/Untitled 13 12.png|Untitled 13 12.png]]