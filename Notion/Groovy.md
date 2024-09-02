---
modified: 2024-09-02T13:21:42+03:00
---
# Section 1: Введение

- ==Можно писать== `Groovy Script`

![[images/Untitled 147.png|Untitled 147.png]]

![[images/Untitled 1 8.png|Untitled 1 8.png]]

- `Groovy` ==классы наследуются== от `Object (Java)` и `GroovyObject`

![[images/Untitled 2 7.png|Untitled 2 7.png]]

- `Public` ==можно не указывать он по умолчанию==
- Чтобы сделать `PakagePriver` нужна ==аннотация==
- ==Точку с запятой можно не ставить,== круглые ==скобочки== ==тоже==

![[images/Untitled 3 7.png|Untitled 3 7.png]]

![[images/Untitled 4 7.png|Untitled 4 7.png]]

- `Groovy Script` extends `Sctips` при ==компиляции==

![[images/Untitled 5 7.png|Untitled 5 7.png]]

  

# Section 2: ClassLoader. ClassPath

![[images/Untitled 6 6.png|Untitled 6 6.png]]

![[images/Untitled 7 6.png|Untitled 7 6.png]]

- `Root ClassLoader` рутовый, который наслдуется от urlClassloader
- `Groovy ClassLoader`

# Section 3: Примитивные типы (их нет)

- _В_ `_Groovy_` _те же типы данных, что и в_ `_Java_`_, за_ ==_маленьким исключением_== _-_ ==_нет примитивных типов данных, а точнее, они все представлены в виде классов-оболочек._== _Так же появились новые представления литеральных форм для таких типов как_ `_BigInteger_` _и_ `_BigDecimal_`_, которые даже_ ==`_не нужно импортировать_`== _для использования._
    
    ![[images/Untitled 8 6.png|Untitled 8 6.png]]
    
    - ==появились альясы==
    
    ![[images/Untitled 9 6.png|Untitled 9 6.png]]
    
    ![[images/Untitled 10 4.png|Untitled 10 4.png]]
    
    ![[images/Untitled 11 4.png|Untitled 11 4.png]]
    
    - ==Привидение типов,== должна быть переопределена `asType(clazz) метод`
    
      
    

### ==String==

- Можно ==обращаться к вызову функций==

![[images/Untitled 12 4.png|Untitled 12 4.png]]

- Можно делать ==интерполяцию строк== через `$` - тогда будет создан класс `GString`

![[images/Untitled 13 4.png|Untitled 13 4.png]]

![[images/Untitled 14 3.png|Untitled 14 3.png]]

![[images/Untitled 15 3.png|Untitled 15 3.png]]

![[images/Untitled 16 3.png|Untitled 16 3.png]]

![[images/Untitled 17 3.png|Untitled 17 3.png]]

# Section 4: Closure

==_Замыкания (Closure_==_) - это один из самых мощных инструментов в программировании. Он существует и в Java в виде lambda выражений, в которых также можно использовать переменные вне области lambda выражений. Но в Groovy пошли еще дальше_

- По сути ==похож== на функциональный интерфейс

![[images/Untitled 18 2.png|Untitled 18 2.png]]

![[images/Untitled 19 2.png|Untitled 19 2.png]]

- Для простоты ==можно не вызывать== `call()` явно, он сам вызовется
- Есть к==лючевое зарезервированное слово== `it`, место `value`

![[images/Untitled 20 2.png|Untitled 20 2.png]]

![[images/Untitled 21 2.png|Untitled 21 2.png]]

- Замыкание
- ==Если== ==функция====, такая, как== ==check== ==принимает функцию или возвращает её,== ==то она называется функцией== ==высшего порядка==

![[images/Untitled 22 2.png|Untitled 22 2.png]]

![[images/Untitled 23 2.png|Untitled 23 2.png]]

- На самом деле ==под капотом создаются локальные классы==

![[images/Untitled 24 2.png|Untitled 24 2.png]]

  

**==Есть еще очень важные поля==**

- `closure.thisObject` - указывает на this объект
- `closure.owner` - там где определили closure
- `closure.delegate` - по умолчанию owner, можно поменять вручную

![[images/Untitled 25 2.png|Untitled 25 2.png]]

![[images/Untitled 26 2.png|Untitled 26 2.png]]

- `closure.delegate` - переопределяем

![[images/Untitled 27 2.png|Untitled 27 2.png]]

![[images/Untitled 28 2.png|Untitled 28 2.png]]

- `Паттерн делегирования` - ==Все запросы которые приходя==т `Closure`, мы ==делегируем== ==объекту==.
- ==Аналог==, лучше писать `with`

![[images/Untitled 29 2.png|Untitled 29 2.png]]

  

==ДЛЯ ОЗНАКОМЛЕНИЯ.==

![[images/Untitled 30 2.png|Untitled 30 2.png]]

# Section 5:  Операторы

### Оператор if-else

-  _Теперь_ ==_не обязательно помещать_== _в if что-то, что является типом данных_ ==_boolean_==

![[images/Untitled 31 2.png|Untitled 31 2.png]]

- ==Под капотом есть функция== `asBoolean`, ==переопределенная==, которая и ==возвращает== `true/false`
- ==Её можно переопределить== в классе

![[images/Untitled 32 2.png|Untitled 32 2.png]]

- ==Есть вот такой вариант==

![[images/Untitled 33 2.png|Untitled 33 2.png]]

  

### Оператор switch

![[images/Untitled 34 2.png|Untitled 34 2.png]]

  

### Loops. Циклы

![[images/Untitled 35 2.png|Untitled 35 2.png]]

![[images/Untitled 36 2.png|Untitled 36 2.png]]

![[images/Untitled 37 2.png|Untitled 37 2.png]]

![[images/Untitled 38 2.png|Untitled 38 2.png]]

  

# Section 6:  Коллекции

###  Lists. Списки

![[images/Untitled 39 2.png|Untitled 39 2.png]]

![[images/Untitled 40 2.png|Untitled 40 2.png]]

![[images/Untitled 41 2.png|Untitled 41 2.png]]

###  Maps. Ассоциативные массивы

![[images/Untitled 42 2.png|Untitled 42 2.png]]

![[images/Untitled 43 2.png|Untitled 43 2.png]]

  

###  Ranges. Диапазоны

_В_ `_Groovy_` _добавили новую коллекцию, которая называется_ `_Range_`_. Она невероятно_ ==_упрощают работу с теми кусочками кода, которые требуют указания диапазона_==_._

![[images/Untitled 44 2.png|Untitled 44 2.png]]

### Object iteration

![[images/Untitled 45 2.png|Untitled 45 2.png]]

==_Любой объект представляется в виде коллекции из одного значения._== _Благодаря такому подходу, которые принят не только в этом языке программирования, можно привнести довольно интересный функционал. Все_ ==_эти полезные методы лежат в утилитном классе_== `_DefaultGroovyMethods_`

![[images/Untitled 46 2.png|Untitled 46 2.png]]

  

# Section 7:  OOP

- Getter/Setter не нужны

![[images/Untitled 47 2.png|Untitled 47 2.png]]

![[images/Untitled 48 2.png|Untitled 48 2.png]]

  

# Section 8:  AST transformations

`_AST (Abstract Syntax Tree)_` _- это_ ==_ключевой момент в понимании того, как устроены большинство компиляторов современных языков программирования_==_. На этом видео мы немного углубимся в них, чтобы понять, как это устроено все под капотом._

 _Можем_ ==_использовать аннотации, которые работают поверх_== `_AST_`_,_ ==_добавляя кучу разного кода перед тем_==_, как_ `_AST_` ==_преобразуется в байт код_==

![[images/Untitled 49 2.png|Untitled 49 2.png]]

![[images/Untitled 50 2.png|Untitled 50 2.png]]

![[images/Untitled 51 2.png|Untitled 51 2.png]]

# Section 9: Dynamic programming. MOP

`_Groovy_` _- это_ `_JVM_` _язык, следовательно, он должен быть невероятно похож на_ `_Java_`_, т.к. компилируется в байт код. Тогда каким образом можно было изменить настолько природу языка_ `_Groovy_`_:_ ==_привнести динамическую составляющую_==_. Ведь_ `_Java_` _- это чисто_ ==_статический язык,_== _и после компиляции невозможно добавить/изменить функционал в классах. А все на самом деле просто -_ ==_добавлена специальная прослойка_== ==_MOP (Meta Object Protocol)_==_, и_ ==_все вызовы из_== `_Groovy_` ==_классов проксируются через вспомогательные классы_== ==_MOP_==_._

![[images/Untitled 52 2.png|Untitled 52 2.png]]

![[images/Untitled 53 2.png|Untitled 53 2.png]]

![[images/Untitled 54 2.png|Untitled 54 2.png]]

  

# Section 10: DSL. Domain Specific Language

**==Как было==**

- `Чем плохо`, этот код не все смогут так написать. ==Технический язык, который знает только программист.==

![[images/Untitled 55 2.png|Untitled 55 2.png]]

Поэтому и появился `DSL`, ==который может написать каждый==

**==Как стало==**

![[images/Untitled 56 2.png|Untitled 56 2.png]]

==**Как это выглядит под капотом**==

![[images/Untitled 57 2.png|Untitled 57 2.png]]

![[images/Untitled 58 2.png|Untitled 58 2.png]]

![[images/Untitled 59 2.png|Untitled 59 2.png]]

![[images/Untitled 60 2.png|Untitled 60 2.png]]