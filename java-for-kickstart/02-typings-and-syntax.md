# Typings & Syntax


## Java Typings: Primitives and Beyond

- Strong typing
- **Primitive type**: stores simple values directly in stack memory
- **Object type**: reference types stored in heap memory

```java
int count = 10;
double price = 19.99;
boolean isWix = true;

String name = "Expert";
```


## Primitive vs. Object Types

- **Storage**: Primitives (e.g., `int`) store values directly in stack memory for high performance
- **Memory**: Objects are reference types stored in heap memory and accessed via pointers
- **Capabilities**: Object types can be `null` and provide built-in methods (e.g., `.toString()`)

```java
// Primitive: no methods, cannot be null
int age = 30;

// Object: has methods, can be null
Integer score = 100;
String scoreText = score.toString();
score = null;
```

---

## Local Type Inference: `var`

- Type inference introduced in Java 10
- Compiler determines type from assignment
- Only for local variables inside methods
- Reduces boilerplate while maintaining safety

```java
// Explicit Typing
int age = 30;
double price = 19.99;
String name = "Wix Engineer";

// Local Type Inference (var)
var user = "Frontend Expert"; // String
var count = 10;               // int
var isWix = true;             // boolean
```

---

## The Entry Point: The `main` Method

- Every Java application starts with a `main` method
- Must be located in a wrapper class
- `main` method signature is strict

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Wix!");
    }
}
```


### Java 21+: Wrapper class is not required

```java
void main() {
    System.out.println("Hello, from Java 21!");
}
```

---

## Logic and Flow Control

- Mandatory braces `{}` for logic blocks
- Required semicolons `;` after statements

```java
// if-else
if (score > 90) {
    // Logic here
}

if (score > 90) {
    // Logic here
} else {

}
```


## Loops

```java
// Traditional for loop
for (int i = 0; i < 5; i++) {
    // Loop logic
}

// For-each loop
for (var item : list) {
    System.out.println(item);
}

// while loop
while (condition) {
    // Continues while true
}
```

---

## Switch Case

- Cleaner approach instead of lots of `if/else`
- "Fall-through" between cases — `break` is required

```java
String season;
switch (month) {
  case 12:
  case 1:
  case 2:
    season = "Winter";
    break;
  case 3:
  case 4:
  case 5:
    season = "Spring";
    break;
  case 6:
  case 7:
  case 8:
    season = "Summer";
    break;
  case 9:
  case 10:
  case 11:
    season = "Fall";
    break;
  default:
    season = "Unknown";
}
```

---

## Hands-on Lab: Calculator API
[Exercise 1](https://github.com/wix-private/wix-academy/tree/master/server-onboarding/java-exercises/java-fundamentals#exercise-1-basic-java--variables-control-flow)

Implement `CalculatorExercises`

