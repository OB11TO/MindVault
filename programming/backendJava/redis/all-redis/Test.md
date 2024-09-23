---
modified: 2024-09-23T15:17:44+03:00
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