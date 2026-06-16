# Pattern Matching


## Smarter Type Checking

- Eliminates redundant manual casting
- Variable is automatically in scope

```java
// Traditional
if (obj instanceof String) {
    var s = (String) obj;
}

// Modern
if (obj instanceof String s) {
    // s is in scope
}
```

---

<!-- .slide: data-auto-animate -->
## The Evolution of Switch <!-- .element: data-id="title" -->

### Traditional

```java
String result;
switch (role) {
    case ADMIN:
        result = "Full Access";
        break;
    case USER:
        if (premium) {
            result = "High";
            break;
        }
    default:
        result = "Basic";
}
```


<!-- .slide: data-auto-animate -->
## The Evolution of Switch <!-- .element: data-id="title" -->

### Modern

```java
String result = switch (role) {
    case ADMIN              -> "Full Access";
    case USER when premium  -> "High";
    default                 -> "Basic";
};
```

- Traditional: Error-prone (fall-through), verbose statement style
- Modern: Arrow syntax, guarded patterns with `when`, compiler-checked exhaustiveness


<!-- .slide: data-auto-animate -->
### Switch Expressions: Same Logic, Less Code <!-- .element: data-id="title" -->
```java
// Traditional
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
```


<!-- .slide: data-auto-animate -->
### Switch Expressions: Same Logic, Less Code <!-- .element: data-id="title" -->

```java
// Modern
String season = switch (month) {
    case 12, 1, 2  -> "Winter";
    case 3, 4, 5   -> "Spring";
    case 6, 7, 8   -> "Summer";
    case 9, 10, 11 -> "Fall";
    default        -> "Unknown";
};
```

---

## Pattern Matching with Records

- Destructure records directly into local variables
- No more chained getter calls (`p.x()`, `p.y()`)

```java
record Point(int x, int y) {}

// Access via getters
if (obj instanceof Point p) {
    System.out.println(p.x() + ", " + p.y());
}

// Destructure into x and y directly
if (obj instanceof Point(int x, int y)) {
    System.out.println(x + ", " + y);
}
```


## Nested Record Patterns

Composition: pull apart deep structures

```java
record Point(int x, int y) {}
record Line(Point start, Point end) {}

if (line instanceof Line(Point(int x1, int y1),
                         Point(int x2, int y2))) {
    var length = Math.hypot(x2 - x1, y2 - y1);
}
```


## Patterns in `switch`

- `switch` arms can match by **type** — no `instanceof` ladder needed
- Variable is bound and in scope in the arm

```java
String describe(Object obj) {
    return switch (obj) {
        case Integer i -> "int: " + i;
        case String s  -> "string of length " + s.length();
        case null      -> "nothing";
        default        -> "something else";
    };
}
```


## Record Patterns in `switch`

Destructure the value as you match it.

```java
record Point(int x, int y) {}
record Line(Point start, Point end) {}

String describe(Object obj) {
    return switch (obj) {
        case Point(int x, int y)       -> "point at " + x + ", " + y;
        case Line(Point a, Point b)    -> "line from " + a + " to " + b;
        case null                      -> "nothing";
        default                        -> "unknown shape";
    };
}
```


## Guarded Patterns: `when`

Refine a pattern with a boolean condition.

```java
String classify(Object obj) {
    return switch (obj) {
        case Integer i when i < 0  -> "negative";
        case Integer i when i == 0 -> "zero";
        case Integer i             -> "positive";
        case String s when s.isBlank() -> "blank string";
        case String s              -> "string: " + s;
        case null, default         -> "not a number";
    };
}
```

- Each guard runs only after the pattern matches
- Order matters — first matching arm wins


## Sealed Types: Exhaustive Switch

Tie it all together — `sealed` + records + switch = exhaustive matching.

- `sealed` restricts which classes can implement the interface
- Compiler checks every case — no `default` needed
- Add a new permit later → compiler forces you to handle it

```java
sealed interface Shape permits Circle, Square, Rectangle {}
record Circle(double radius)         implements Shape {}
record Square(double side)           implements Shape {}
record Rectangle(double w, double h) implements Shape {}

double area = switch (shape) {
    case Circle(double r)              -> Math.PI * r * r;
    case Square(double s)              -> s * s;
    case Rectangle(double w, double h) -> w * h;
};
```

---

## Hands-on Lab: Role-Based Logic
