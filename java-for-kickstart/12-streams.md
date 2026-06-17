# Streams


<!-- .slide: data-auto-animate -->
## What is a Stream? <!-- .element: data-id="title" -->

- A **pipeline** for processing a sequence of elements
- Introduced in Java 8
  - The functional answer to `for` loops


<!-- .slide: data-auto-animate -->
## What is a Stream? <!-- .element: data-id="title" -->

- A **pipeline** for processing a sequence of elements
- Introduced in Java 8
  - The functional answer to `for` loops

```java
// Imperative — describe HOW
var emails = new ArrayList<String>();
for (var user : users) {
    if (user.isActive()) {
        emails.add(user.email());
    }
}

// Declarative — describe WHAT
var emails = users.stream()
    .filter(user -> user.isActive())
    .map(user -> user.email())
    .toList();
```

---

<!-- .slide: data-auto-animate -->
## Three Properties <!-- .element: data-id="title" -->

### Declarative Logic

- Describe *what* you want, not *how* to loop. 
- The pipeline reads top-to-bottom like a sentence.


<!-- .slide: data-auto-animate -->
## Three Properties <!-- .element: data-id="title" -->

### No Storage

- A Stream is a **pipeline that moves data**, 
    - not a data structure that holds it. 
- The source collection is untouched.


<!-- .slide: data-auto-animate -->
## Three Properties <!-- .element: data-id="title" -->

### Lazy Evaluation

- Intermediate operations build the plan. 
- Nothing actually runs until a **terminal** operation is called 
    - e.g., `toList`, `count`, `findFirst`

```java
// Nothing executes here yet:
var pipeline = users.stream()
    .filter(User::isActive)
    .map(User::email);

// Execution happens HERE:
var emails = pipeline.toList();
```

---

## The Stream Pipeline

- **Source**: Collections, Arrays, or I/O Channels
- **Intermediate**: lazy operations: 
  - `filter`, `map`, `sorted`
- **Terminal**: triggers execution: 
  - `toList`, `collect`, `findFirst`

```java
collection.stream()        // Source
    .filter(...)           // Intermediate
    .map(...)              // Intermediate
    .toList();             // Terminal
```

---

## Transformation & Filtering

- **`filter`**: drops elements that don't match a predicate
- **`map`**: 1-to-1 transformation of elements
- **`flatMap`**: flattens nested collections into a single stream

```java
var emails = users.stream()
    .filter(User::isActive)
    .map(User::email)
    .toList();

// Pipelines are easy to read and maintain.
```

---

## Aggregation: Collectors

- `collect` gathers stream elements into a container.
- `groupingBy` organizes elements into a `Map` by property.

```java
var byRole = 
  users.stream()
    .collect(Collectors.groupingBy(User::role));

// Result is a Map where:
//   Key   = Role
//   Value = List of Users with that role
```

---

## Special Case: `Int Stream`
- The JDK provides us helper methods related to stream of integers
- Example: `min()`, `max()`, `sum()`

```java
var words = List.of("hello", "to", "wix");

words.stream()
  .mapToInt(String::length)
  .max();
```


# Coding Example

---

## Lab Stop 2

**Exercise 8**: Streams API

Implement `filter`, `map`, and `grouping` logic.
Remember to use a terminal operation!
