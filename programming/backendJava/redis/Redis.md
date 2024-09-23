---
title: Redis
tags:
  - Redis
related_topics: 
created: 2024-09-23 14:50
modified: 2024-09-23T15:12:10+03:00
questions: 
notes: 
links: 
---

- Получить сколько данных лежит в базе

```Java
List<String> keys = new ArrayList<>();
try (Cursor<byte[]> cursor = redisTemplate.getConnectionFactory().getConnection().scan(ScanOptions.scanOptions().match("*").build())) {
    while (cursor.hasNext()) {
        keys.add(new String(cursor.next()));
    }
} catch (Exception e) {
    // Обработка ошибок
}
```