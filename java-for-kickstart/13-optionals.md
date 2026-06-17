# Optionals


## Thinking in Optionals

- Returning `null` is ambiguous and causes runtime crashes (NPE). 
- It forces the caller to guess if a value exists.


### The Solution: `Optional`

- A wrapper that might contain an empty value. 
- "No Result" representation in the API signature.


## Optional API Overview
```java
// creation
Optional.of(VALUE);
Optional.ofNullable(VALUE);
Optional.empty();

// Extraction
Option[String] opt = Optional.of("hello");

opt.orElse("default");
opt.orElseThrow();
opt.get();
```

- **Safety Rule**: Never call `.get()` without checking `.isPresent()`


## Optionals: Defensive way
- `Optional<T>` a box that may contain a value of `T`
- Use `isPresent()` to verify the value before access
```java
Optional<User> userOpt = repository.findById("123");

// get email if user is found
if (userOpt.isPresent()) {
    User user = userOpt.get();
    System.out.println(user.getEmail());
}
```

<div align="left" class="fragment">
<ul><li>Verbose, similar to <code>null</code> checks</li></ul>
</div>

---

## Functional Chaining
- Optional can be treated as "stream of 1".
- Keep the value inside the "box" using map and filter.
- If "box" is empty, operations are skipped.

```java
// Modern null-safe chain
return findUser("123")
    .map(User::getEmail)
    .map(email -> email.endsWith(".com"))
    .forEach(System.out::println);
```

---

# Coding Example

---

## Lab Stop 3

**Exercise 9**: Optionals

Replace legacy `if-null` checks with functional Optional chains.
