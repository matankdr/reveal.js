# Object Methods


<!-- .slide: data-auto-animate -->
## Object type: Important Methods <!-- .element: data-id="title" -->
#### `toString()`
String representation of the object state.


<!-- .slide: data-auto-animate -->
## Object type: Important Methods <!-- .element: data-id="title" -->

```java
class User {
  int id;
  String name;

  @Override
  public String toString() {
    // Returns a readable string instead of the memory address
    return "User{id='" + id + "', name='" + name + "'}";
  }
}
```


<!-- .slide: data-auto-animate -->
## Object type: Important Methods <!-- .element: data-id="title" -->

#### `equals()`
Logical identity check between objects.

#### `hashCode()`
Numerical ID for hash-based storage buckets.


<!-- .slide: data-auto-animate -->
## Object type: Important Methods <!-- .element: data-id="title" -->
#### `equals()`

```java
class User {
  int id;
  String name;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof User user)) return false;
    // Logical comparison based on the 'id' field
    return id == user.id;
  }
}
```


<!-- .slide: data-auto-animate -->
## Object type: Important Methods <!-- .element: data-id="title" -->
#### `hashCode()`

```java
class User {
  int id;
  String name;

  @Override
  public int hashCode() {
    // Required whenever equals is overridden to avoid collection bugs
    return Integer.hashCode(id);
  }
}
```

---

<!-- .slide: data-auto-animate -->
### Why equals and hashCode MUST match <!-- .element: data-id="title" -->

- **Rule**: If you override one, you MUST override both
- Leads to logical inconsistencies in collections


<!-- .slide: data-auto-animate -->
### Why equals and hashCode MUST match <!-- .element: data-id="title" -->
```java
class User {
    String id;
    User(String id) { this.id = id; }

    @Override // Logical equality based on ID
    public boolean equals(Object o) {
        if (!(o instanceof User user)) return false;
        return id.equals(user.id);
    }
    // BUG: hashCode() is NOT overridden!
}
```


<!-- .slide: data-auto-animate -->
### Why equals and hashCode MUST match <!-- .element: data-id="title" -->
```java
// Logic Failure in Collections:
var usersAndStatus = new HashMap<User, String>();
var u1  = new User("123");
map.put(u1, "Authorized");

var u2 = new User("123");
System.out.println(u1.equals(u2)); // true
System.out.println(usersAndStatus.get(u2));   // null (BUG!) 😱
```

---

## Hands-on Lab: Composition

Task: Create `BaseEntity` parent class for shared metadata
