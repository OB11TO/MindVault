---
title: SOLID
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 13:37
modified: 2024-08-31T13:39:04+03:00
difficulty: 
questions: 
notes: 
links: 
---
## Краткое описание


## Полное описание
# SOLID

### Принцип открытости/закрытости (OCP)

Этот принцип гласит, что классы должны быть открыты для расширения, но закрыты для изменения. Это означает, что мы должны быть в состоянии добавлять новое поведение в существующие классы без изменения их исходного кода.

**`Пример:`**

Предположим, у нас есть класс для вычисления зарплаты сотрудника:

```java
class Employee {
    private double salary;

    public Employee(double salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }
}

```

Теперь добавим класс для вычисления бонусов:

```java

interface Bonus {
    double calculate(Employee employee);
}

class FixedBonus implements Bonus {
    public double calculate(Employee employee) {
        return employee.getSalary() * 0.1; // 10% фиксированный бонус
    }
}

class PerformanceBonus implements Bonus {
    public double calculate(Employee employee) {
        return employee.getSalary() * 0.2; // 20% бонус за производительность
    }
}

```

Мы можем добавить новые классы бонусов, не изменяя существующие классы `Employee` или `Bonus`.

### Принцип подстановки Лисков (LSP)

Этот принцип говорит о том, что подклассы должны дополнять, а не изменять поведение базовых классов, то есть объекты подклассов должны заменять объекты базовых классов без изменения корректности программы.

**`Пример:`**

Предположим, у нас есть класс `Bird` и его подкласс `Penguin`:

```java

class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}

```

Этот пример нарушает LSP, потому что `Penguin` не может быть подставлен в код, ожидающий объект типа `Bird`, без изменения поведения программы.

Лучший подход будет использовать интерфейсы или абстрактные классы:

```java

interface Bird {
    void move();
}

class Sparrow implements Bird {
    @Override
    public void move() {
        System.out.println("Flying");
    }
}

class Penguin implements Bird {
    @Override
    public void move() {
        System.out.println("Swimming");
    }
}

```

Теперь подклассы `Sparrow` и `Penguin` дополняют, а не изменяют поведение, и могут быть использованы взаимозаменя

### Принцип разделения интерфейса (ISP)

Этот принцип утверждает, что лучше иметь несколько узкоспециализированных интерфейсов, чем один "толстый" интерфейс.

**`Пример`:**

Вместо одного интерфейса:

```java

interface Worker {
    void work();
    void eat();
    void sleep();
}

```

Лучше разделить его на несколько специализированных интерфейсов:

```java

interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

```

Теперь классы могут имплементировать только те интерфейсы, которые им действительно необходимы:

```java

class Employee implements Workable, Eatable {
    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        System.out.println("Eating");
    }
}

```

### Принцип инверсии зависимостей (DIP)

Этот принцип утверждает, что высокоуровневые модули не должны зависеть от низкоуровневых модулей. Оба типа модулей должны зависеть от абстракций.

**`Пример`:**

Плохой пример:

```java

class Keyboard {
    // ...
}

class Computer {
    private Keyboard keyboard;

    public Computer() {
        this.keyboard = new Keyboard();
    }
}

```

Здесь класс `Computer` зависит от конкретного класса `Keyboard`. Вместо этого мы можем ввести абстракцию:

```java

interface Keyboard {
    // ...
}

class MechanicalKeyboard implements Keyboard {
    // ...
}

class Computer {
    private Keyboard keyboard;

    public Computer(Keyboard keyboard) {
        this.keyboard = keyboard;
    }
}

```

Теперь класс `Computer` зависит от абстракции `Keyboard`, и мы можем подставлять любые конкретные реализации `Keyboard`, не изменяя класс `Computer`.