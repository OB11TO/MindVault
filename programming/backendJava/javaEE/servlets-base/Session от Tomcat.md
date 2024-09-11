---
title: Session от Tomcat
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:58
modified: 2024-09-11T13:59:06+03:00
questions: 
notes: 
links: 
---
### Session от Tomcat

==Сессия с помощью куки определяет одна и та же эта сессия или нет.==

- Это всё `происходит в недрах Каталины.`
- Есть класс `Request` из ==Catalina==, который обертка над классом `Request` из ==Cayota==
- Достаем из ==Contexta== сессию, ==так как в томкате могут крутиться несколько приложений и у каждого будет свой контекст.==
- Из ==Manager== сессий, ==получаем сессию== по `jsessionId`
- Иначе ==создать== ==session==, со всеми данными и кладем в map с новый генерированным `jsessionId`

![[Untitled 52.png]]

![[Untitled 53.png]]
