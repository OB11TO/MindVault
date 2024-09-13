---
title: Nested class
tags:
  - JavaSE
  - InnerClass
related_topics: 
created: 2024-08-30 14:02
modified: 2024-09-02T15:36:44+03:00
difficulty: medium
questions: 
notes: 
links: 
---
### Вложенные классы/Nested(static)

Статические вложенные классы (static nested classes) в Java представляют собой внутренние статические классы.

Cтатический вложенный класс ==похож на статический метод==. Слово static перед объявлением класса указывает, что этот ==класс не хранит в себе ссылок на объекты внешнего класса, внутри которого объявлен.==

```java
class Zoo
{
 private static int count = 7;
 private int mouseCount = 1;

 public static int getAnimalCount() //может обращаться к статическим переменным
 {
  return count;
 }

 public int getMouseCount() // может обращаться как к статитеским так  и не статическим
 {
  return mouseCount;
 }

 public static class Mouse // может обращаться только к статическим переменным класса Zoo
 {
  public Mouse()
  {
  }
   public int getTotalCount()
  {
   return **count** + **mouseCount**; //ошибка компиляции.
  }
 }}
```

В отличие от нестатических внутренних классов ==можно создать вложенный класс, даже когда не создан ни один экземпляр внешнего класса.==
