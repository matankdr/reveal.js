# Optionals


## Thinking in Optionals

### The Problem: `null`

Returning `null` is ambiguous and causes runtime crashes (NPE). It forces the caller to guess if a value exists.


### The Solution: `Optional`

A container object which may or may not contain a non-null value. It explicitly represents "No Result" in the API signature.

---

## Optional API Overview

- **Creation**: `Optional.of(val)`, `Optional.ofNullable(val)`, `Optional.empty()`
- **Extraction**: `orElse("default")`, `orElseThrow()`
- **Safety Rule**: Never call `.get()` without checking `.isPresent()` first. Better yet, use the pipeline methods.

---

## Functional Chaining

You can chain `map` and `filter` directly on the Optional. If the box is empty, the operations are skipped.

This enables modern, null-safe logic without nested `if (x != null)` blocks.

```java
// Modern null-safe chain
return findUser("123")
    .map(User::address)
    .map(Address::city)
    .orElse("Unknown");
```

---

## Wix Best Practices

- **Returns Only** — use `Optional` as a method *return type* to signal "result might be missing"
- **No Parameters** — do not use `Optional` as a parameter type. Use `@Nullable` annotations instead (checked by Wix NullAway)
- **No Wrappers** — do not wrap Collections in Optionals. Return an empty list instead.

---

## Lab Stop 3

**Exercise 8**: Optionals

Replace legacy `if-null` checks with functional Optional chains.
