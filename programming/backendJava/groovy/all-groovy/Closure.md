---
title: Closure
tags:
  - Groovy
related_topics: 
created: 2024-09-17 18:15
modified: 2024-09-17T18:47:00+03:00
questions: 
notes: 
links: 
---

---
[[Closure info]]

----


### Closure

<mark class="hltr-red">Замыкания (Closure)</mark> - это один из самых мощных инструментов в программировании. Он <mark class="hltr-yellow">существует и в Java в виде lambda выражений, в которых также можно использовать переменные вне области lambda выражений</mark>. Но<mark class="hltr-green2"> в Groovy пошли еще дальше</mark>

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

