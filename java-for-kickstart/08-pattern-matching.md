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

<!-- TODO: add record deconstruction patterns example -->

```java
// TODO
```


## Pattern Matching with Sealed Types

<!-- TODO: add sealed interface + exhaustive switch example -->

```java
// TODO
```

---

## Hands-on Lab: Role-Based Logic
