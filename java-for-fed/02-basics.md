# Java Basics


<!-- .slide: data-auto-animate -->
## Variables <!-- .element: data-id="title" -->

```java
// these are the same
String a = "hello"; // traditional style
var a = "hello";    // modern style, use when type is obvious
```
<!-- .element: class="fragment" -->

```java
// mutable
var a = "hello";
a = "world";
```
<!-- .element: class="fragment" -->

```java
// does not compile
var a = "hello";
a = 5;
```
<!-- .element: class="fragment" -->


<!-- .slide: data-auto-animate -->
## Final Variables <!-- .element: data-id="title" -->
```java
// These are the same
final String a = "hello";
// or 
final var a = "hello";

// does not compile
a = "world";
```


<!-- .slide: data-auto-animate -->
## Functions <!-- .element: data-id="title" -->
<pre style="font-size: 150%; width: fit-content;">
  <code class="java" data-trim>
int add(int x, int y) {
  return x + y;
}
  </code>
</pre>


## Functions <!-- .element: data-id="title" -->
- Java is:
  - Pass-by-value for primitives
  - Pass-by-value for references
- Meaning:
  - You can mutate objects
  - You cannot replace them
- Design implication:
  - Prefer returning values over mutating inputs


<!-- .slide: data-auto-animate -->
## Minimal Java Program <!-- .element: data-id="title" -->

```java
// in file YourService.java
package workshop;

public class YourService {
  public static void main(String[] args) {
    System.out.println("Hello Wix!");
  }
}
```
- `main` function is the entry point
- Code lives in classes (later today)
- Public class name == file name
- Packages define structure

---

## Control Flow
### Conditionals

```java
if (age >= 18) {
    allowAccess();
} else {
    denyAccess();
}
```


## Control Flow
### Loops

```java
for (int i = 0; i < 10; ++i) {
  process(i);
}
```

```java
for (var item: items) {
  process(item);
}
```

```java
int i = 0;
while (i < 10) {
    process(i);
}
```


# Basic Collections


## Basic Collections
<img src="./java-for-fed/images/java-collections.png">


## Basic Collections: Traditional
```java
List<Integer> list = new LinkedList();
list.add(1); // (1)
list.add(2); // (1,2)
list.add(3); // (1,2,3)
```

- Mutable collections
- Boiler plate 🤬


<!-- .slide: data-auto-animate -->
## Basic Collections: Modern <!-- .element: data-id="title" -->
```java
var myList = List.of(1,2,3,3); // (1,2,3,3)
var mySet = Set.of(1,2,3,3); // (1,2,3)

var myMap = Map.of( 
  1, "one",
  2, "two",
  3, "three"
);
```

