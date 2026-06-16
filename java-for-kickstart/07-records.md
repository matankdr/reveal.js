# Records


<!-- .slide: data-auto-animate -->
## Getters & Setters <!-- .element: data-id="title" -->

- Encapsulation requires manual accessors
- Standard POJO (Plain Old Java Object) pattern
- Traditional Java Boilerplate

```java
public String getName() {
    return name;
}
public void setName(String name) {
    this.name = name;
}
```


<!-- .slide: data-auto-animate -->
### Eliminating Boilerplate with `record` <!-- .element: data-id="title" -->

- One-line immutable data carriers
- Auto-generated Constructor and Getters
- Auto-generated object methods: 
  - `hashCode`, `equals`, `toString`

```java
// Definition
public record User(String name) {}

// Usage
var user = new User("Wix");
System.out.println(user.name());
```

---

## Hands-on Lab: Record Refactor
