---
title: Interface NavigableSet
tags:
  - Collection
  - Set
related_topics: 
created: 2024-09-09 17:19
modified: 2024-09-09T17:19:45+03:00
questions: 
notes: 
links: 
---
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

