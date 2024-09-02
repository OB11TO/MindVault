---
modified: 2024-09-02T13:33:47+03:00
---
# Section 1: Теория

  


# Section 1: Практика

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