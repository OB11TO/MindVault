---
title: Сookies
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:58
modified: 2024-09-11T13:58:46+03:00
questions: 
notes: 
links: 
---
### Сookies

![[Untitled 50.png]]

```Java
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Arrays;
import java.util.concurrent.atomic.AtomicInteger;

public class CookieServlet extends HttpServlet {
    private static final String UNIQUE_ID = "user_id";
    private final AtomicInteger counter = new AtomicInteger();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        var cookies = req.getCookies();
        if(cookies == null  || Arrays.stream(cookies)
                .filter(cookie -> UNIQUE_ID.equals(cookie.getName()))
                .findFirst()
                .isEmpty()){
            var cookie = new Cookie(UNIQUE_ID, "1");
            cookie.setPath("/cookies");
            cookie.setMaxAge(15*60);
            resp.addCookie(cookie);
            counter.incrementAndGet();
        }
        resp.setContentType("text/html");
        try(var writer = resp.getWriter()){
            writer.write(counter.get());
        }
    }
}
```

![[Untitled 51.png]]
