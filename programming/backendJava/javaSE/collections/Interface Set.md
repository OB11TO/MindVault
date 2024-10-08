---
title: Interface Set
tags:
  - Collection
  - Set
related_topics: []
created: 2024-09-09 17:16
modified: 2024-09-12T15:16:29+03:00
questions: 
notes: 
links: 
---
----
[[CopyOnWriteArraySet]]
[[ConcurrentSkipListSet]]
[[HashSet]]
[[LinkedHashSet]]
[[EnumSet]]
[[TreeSet]]

-----
## Interface Set\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/Set.html](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)

это интерфейс в языке программирования Java, который расширяет интерфейс `Collection` и <mark class="hltr-yellow">представляет собой набор уникальных элементов, не допускающих повторений.</mark>

<mark class="hltr-red">Интерфейс</mark> `SortedSet<E>` расширяет интерфейс `Set<E>` и<mark class="hltr-yellow"> добавляет возможность итерирования элементов в отсортированном порядке</mark>. Кроме того, он <mark class="hltr-green2">предоставляет методы для получения первого и последнего элементов множества, а также подмножеств между двумя элементами.</mark>

<mark class="hltr-red">Интерфейс</mark>  `NavigableSet<E>` также расширяет интерфейс `SortedSet<E>` и добавляет дополнительные методы для навигации в множестве. Например, он<mark class="hltr-red"> позволяет получать элементы, находящиеся ближайшими к определенному значению, и выполнять различные операции с элементами, основанные на их порядке.</mark>

 ==**ЕСЛИ ДОБАВЛЯТЬ ОБЪЕКТЫ С ОДИНАКОВЫМ ХЕШКОДОМ, в**== ==`**HASHSET**`== ==**ТО НИЧЕГО НЕ ПРОИЗОЙДЕТ ( ПОЛУЧИМ ПРОСТО СПИСОК)

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


![[images/Untitled 1 21.png|Untitled 1 21.png]]