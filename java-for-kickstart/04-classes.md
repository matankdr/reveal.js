# Classes


## Classes in Java

- Blueprint for creating objects
- Bundles state and behavior together
- Main mechanism for abstraction and encapsulation

<div style="display: flex; gap: 1em;">
<div style="flex: 1;">

```java
class User {
    String name;
    int age;

    void sayHello() {
      System.out.println(
        "Hello, I'm " + name
      );
    }
}
```
</div>
<div style="flex: 1;">

```java
var user = new User();
user.name = "Wix Expert";
user.age = 30;
user.sayHello();
```
</div>
</div>

---

## Access Modifiers: Visibility

<table>
  <thead>
    <tr><th><code>public</code></th><th><code>private</code></th><th><code>protected</code></th></tr>
  </thead>
  <tbody style="font-size: 0.70em">
    <tr>
      <td>Accessible everywhere across all packages.</td>
      <td>Accessible only within the class boundaries.</td>
      <td>Accessible in same package and subclasses.</td>
    </tr>
  </tbody>
</table>

---

## Initializing State: Constructors

- Method called during object creation
- No return type
- **Constructor** name must match the **class** name

```java
public User(String name) {
    this.name = name;
}
```

---

## Constructor Chaining

- **Logic Reuse**: one constructor calls another in the same class to avoid redundant initialization code
- **`this`**: used to invoke a specific sibling constructor based on the arguments provided


## Constructor Chaining
```java
class User {
    String name;
    String role;

    // Primary constructor
    public User(String name, String role) {
        this.name = name;
        this.role = role;
    }

    // Chained constructor: provides a default value
    public User(String name) {
        this(name, "USER"); // Calls the constructor above
    }
}
```
