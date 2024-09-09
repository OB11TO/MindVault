---
title: Interface Queue
tags:
  - Collection
  - Queue
related_topics:
  - "[[SynchronousQueue]]"
  - "[[PriorityQueue]]"
  - "[[PriorityBlockingQueue]]"
  - "[[LinkedTransferQueue]]"
  - "[[LinkedBlockingQueue]]"
  - "[[LinkedBlockingDeque]]"
  - "[[Разница между ConcurrentLinkedDeque]]"
  - "[[ArrayBlockingQueue]]"
  - "[[ConcurrentLinkedDeque]]"
  - "[[ArrayDeque]]"
created: 2024-09-09 17:54
modified: 2024-09-09T18:21:19+03:00
questions: 
notes: 
links: 
---
## Interface Queue\<E>

Представляет собой<mark class="hltr-yellow"> коллекцию элементов, в которой элементы добавляются в конец</mark> (<mark class="hltr-green2">tail</mark>) и у<mark class="hltr-yellow">даляются из начала</mark> (<mark class="hltr-green2">head</mark>). Такая структура данных называется очередью (queue) или FIFO (first-in, first-out).

## Классы имплементирующие эти интерфейсы:

### class AbstractQueue\<E> extends java.util.AbstractCollection\<E>