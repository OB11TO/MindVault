---
title: Exception
tags:
  - JavaSE
related_topics: 
created: 2024-08-30 16:30
modified: 2024-08-30T16:32:14+03:00
difficulty: 
questions: 
notes: 
links: 
---
![[Pasted image 20240830163102.png]]
### ==try{} catch{}==
Используется для поимки исключений в блоке, который может вызвать исключения

```java
try {
    // код, который может вызвать исключение
} catch (NullPointerException e) {
    // обработка NullPointerException
} catch (IndexOutOfBoundsException e) {
    // обработка IndexOutOfBoundsException
} catch (IOException e) {
    // обработка IOException
} catch (Exception e) {
    // общий обработчик для всех исключений
}
``
```