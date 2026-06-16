# Interfaces & State


<!-- .slide: data-auto-animate -->
## Interfaces: Traditional <!-- .element: data-id="title" -->

- Contract: what the class can do
- Do not provide implementation
- Achieve common behavior between multiple classes
- Class can implement multiple interfaces (!)


<!-- .slide: data-auto-animate -->
## Interfaces: Traditional <!-- .element: data-id="title" -->
```java
interface Greeter {
  String greet();
}

class User implements Greeter {
  String name;

  @Override
  String greet() {
    return "Hello, I'm " + name;
  }
}

var user = new User();
user.greet();
```


<!-- .slide: data-auto-animate -->
## Interfaces: Modern <!-- .element: data-id="title" -->

- `default` methods can have implementation
- `private` methods can share code between default methods

```java
interface Logger {
    default void logInfo(String msg) {
      log("INFO", msg);
    }

    default void logError(String msg) {
      log("ERROR", msg);
    }

    private void log(String level, String msg) {
      System.out.println("[" + level + "] " + msg);
    }
}
```


<!-- .slide: data-auto-animate -->
## Interfaces: Key feature <!-- .element: data-id="title" -->

- Classes in Java supports a single inheritance
- BUT...Multiple interfaces can be mixed in!

```java
interface Car {
  void drive();
}

interface Airplane {
  void fly();
}

class Porche implements Car {}
class Boeing implements Airplane {}
class Batmobile implements Car, Airplane {} 
```

---

## State Management: Instance vs Static

- **Instance**: Individual data per object instance
- **Static**: Shared state across the entire class

```java
public class User {
  static int count;  // Shared
  String name;       // Individual

  public void info() {
    System.out.println(name);
  }
}
```
