Builder Pattern
---

Builder Pattern은 Creational Type Design Pattern의 한 가지이다.

주로 Telescoping Constructor Anti-Pattern과 함께 언급된다.

Telescoping Constructor Anti-Pattern의 예를 살펴보자.

```java
public class Pizza {
    private long orderNo;
    private short size;
    private boolean cheese;
    private boolean ham;
    private boolean pepper;
    private boolean tomatoSource;

    public Pizza(long orderNo, short size, boolean cheese, boolean ham) {
        this(orderNo, size, cheese, ham, false, false)
    }
    public Pizza(long orderNo, short size, boolean cheese, boolean ham, boolean pepper) {
        this(orderNo, size, cheese, ham, pepper, false)
    }
    public Pizza(long orderNo, short size, boolean cheese, boolean ham, boolean pepper, boolean tomatoSource) {
        this.orderNo = orderNo;
        this.size = size;
        this.cheese = cheese;
        this.cheese = ham;
        this.pepper = pepper;
        this.tomatoSource = tomatoSource;
    }
}
```

점층적으로 Pizza Constructor의 Paramters가 늘어가고 있다. 그리고 미래에 새로운 재료가 추가 된다면 계속해서 Constructor의 수가 지저분하게 늘어날 것이 충분히 예상된다.

이러한 패턴은 thread-safe이며 코드가 동작하는데 문제는 없지만, 이는 팀으로서 다른 동료가 해당 Class를 사용하고자 할 때, 어떤 파라미터로 생성하는지 보기도 쓰기도 어렵다. 또한 원하는 동작을 하는 Constructor가 없을 수도 있어, 결국 자신이 new Constructor를 추가하거나 혹은 null parameter를 넘겨야만 할 수도 있다.

### Java Bean Pattern

```java
public class Pizza {
    private final long orderNo;
    private final short size;
    private boolean cheese = false;
    private boolean ham = false;
    private boolean pepper = false;
    private boolean tomatoSource = false;

    public Pizza(long orderNo, short size) {
        this.orderNo = orderNo;
        this.size = size;
    }
    public void setCheese(boolean cheese) {
        this.cheese = cheese;
    }
    public void setPepper(boolean pepper) {
        this.pepper = pepper;
    }
    public void setTomatoSource(boolean tomatoSource) {
        this.tomatoSource = tomatoSource;
    }
}
```

```java
public class PizzaOrder {
    public static void main(String[] args) {
        Pizza order = new Pizza(1, 1);
        order.setCheese(true);
        order.setTomatoSource(true);
    }
}
```

Pizza 클래스의 기본 생성자를 이용하여 비어있는 Pizza Instance를 만들고, 그 후 setter를 통해서 Pizza Instance의 Property를 채우는 방법이다.

하지만, 이는 Instance를 한 번 생성한 후 Property를 채우기 위해, 해당 Instance에 계속 변화를 요구하여 1번의 호출로 객체 생성이 끊나지 않으므로 객체의 consistency가 깨진다. 또한 이는 Setter 메소드가 필요하므로 immutable 객체를 생성할 수 없는 단점이다.

### Builder Pattern

```java
public class Pizza {
    private final long orderNo;
    private final short size;
    private final boolean cheese;
    private final boolean ham;
    private final boolean pepper;
    private final boolean tomatoSource;

    public Pizza(PizzaBuilder builder) {
        this.orderNo = builder.orderNo;
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.ham = builder.ham;
        this.pepper = builder.pepper;
        this.tomatoSource = builder.tomatoSource;
    }

    public void print() {
        System.out.printf(
            "Pizza: orderNo(%d), size(%d), cheese(%s), ham(%s), pepper(%s), tomato source(%s)",
            this.orderNo, this.size, this.cheese, this.ham, this.pepper, this.tomatoSource
        );
    }

    public static class PizzaBuilder {
        // Required Params
        private final long orderNo;
        private final short size;

        // Optional Params - should be assigned with default values
        private boolean cheese = false;
        private boolean ham = false;
        private boolean pepper = false;
        private boolean tomatoSource = false;

        public PizzaBuilder(long orderNo, short size) {
            this.orderNo = orderNo;
            this.size = size;
        }
        public PizzaBuilder addCheese(boolean cheese) {
            this.cheese = cheese;
            return this;
        }
        public PizzaBuilder addPepper(boolean pepper) {
            this.pepper = pepper;
            return this;
        }
        public PizzaBuilder addTomatoSource(boolean tomatoSource) {
            this.tomatoSource = tomatoSource;
            return this;
        }

        public Pizza build() {
            return new Pizza(this);
        }
    }
}
```

```java
public class PizzaOrder {
    public static void main(String[] args) {
        Pizza order = new Pizza.PizzaBuilder((long) 1, (short) 1)
                        .addCheese(true)
                        .addPepper(true)
                        .addTomatoSource(true)
                        .build();
        order.print();
    }
}
```
