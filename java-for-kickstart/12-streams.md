# Streams


## Introduction to Streams
- Declarative Logic | No Storage | Lazy Evaluation
  - Describe *what* you want, not *how*
- No Storage


|  to loop. Akin to `Array.prototype.map` in JS. | A Stream is a pipeline that moves data, not a data structure that holds it. | Operations only execute when a terminal operation (like `.toList()`) is called. |

---

## The Stream Pipeline

- **Source**: Collections, Arrays, or I/O Channels
- **Intermediate**: lazy operations — `filter`, `map`, `sorted`
- **Terminal**: triggers execution — `toList`, `collect`, `findFirst`

```java
collection.stream()        // Source
    .filter(...)           // Intermediate
    .map(...)              // Intermediate
    .toList();             // Terminal
```

---

## Transformation & Filtering

- **Filter**: drops elements that don't match a predicate
- **Map**: 1-to-1 transformation of elements
- **FlatMap**: flattens nested collections into a single stream

```java
var emails = users.stream()
    .filter(User::isActive)
    .map(User::email)
    .toList();

// Pipelines are easy to read and maintain.
```

---

## Aggregation: Collectors

`collect()` gathers stream elements into a container.

### Grouping

`groupingBy` organizes elements into a `Map` by property.

```java
var byRole = users.stream()
    .collect(Collectors.groupingBy(User::role));

// Result is a Map where:
//   Key   = Role
//   Value = List of Users with that role
```

---

## Lab Stop 2

**Exercise 7**: Streams API

Implement `filter`, `map`, and `grouping` logic.
Remember to use a terminal operation!
