# Lambdas & Method References


<!-- .slide: data-auto-animate -->
## Functional Interfaces <!-- .element: data-id="title" -->
> A functional interface is an interface with exactly one abstract method


<!-- .slide: data-auto-animate -->
## Functional Interfaces <!-- .element: data-id="title" -->
> A functional interface is an interface with exactly one abstract method

```java
@FunctionalInterface
public interface Function<A, B> {
  B apply(A a);

  default <C> Function<A, C> andThen(Function<B, C> after) {
    return (A a) -> after.apply(this.apply(a));
  }
}
```

---

<!-- .slide: data-auto-animate -->
## Lambda Syntax <!-- .element: data-id="title" -->

- Compact behavior passing. 
- The compiler infers types from context 
    - similar to JS arrow functions.

```java
// Old way (anonymous class)
Collections.sort(list, new Comparator<Integer>() {
    public int compare(Integer a, Integer b) {
        return a.compareTo(b);
    }
});

// Modern way (lambda)
list.sort((a, b) -> a.compareTo(b));
```


<!-- .slide: data-auto-animate -->
## Lambda Syntax <!-- .element: data-id="title" -->
### Why does it work?
- Treating behavior as data. 
- A lambda is an implementation of an interface with a **Single Abstract Method (SAM)**.


<!-- .slide: data-auto-animate -->
## Lambda Syntax <!-- .element: data-id="title" -->
- **Traditional**: verbose anonymous classes.
- **Modern**: concise, logic-only syntax.
- **Mindset**: from *how* (implementation) to *what* (action)


<!-- .slide: data-auto-animate -->
## Lambda Syntax <!-- .element: data-id="title" -->

- `Predicate<T>`: A simple condition checker
- Takes an input of type `T` and returns a boolean

```java
// 1. Define the condition (The Predicate)
Predicate<User> isAdmin = user -> user.getRole().equals("ADMIN");

// 2. Usage example
if (isAdmin.test(user)) {
    System.out.println("Hello Admin!");
}
```


# Coding Example


## Common Functional Interfaces
| Name| Signature |
| -------- | ------- |
| `Predicate<T>`   | `T -> boolean` |
| `Function<T, R>` | `T -> R`      |
| `Consumer<T>`    | `T -> void`    |
| `Supplier<T>`    | `() -> T`      |


## Common Uses

- Sorting collections
- Filtering streams
- Event handling
- Passing strategies/callbacks

---

## Method References (`::`)

- Shorthand for lambdas
- Simplify call an existing method
- Improve readbility when lambda delegates to a method

```java
s -> s.toUpperCase() // lambda
String.toUpperCase   // method reference

x -> System.out.println(x) // lambda
System.out::println        // method reference
```

---

## Lab Stop 1

**Repo:** `java-training-day-2`

- Exercise 6: Lambda Functions
- Exercise 7: Method References
